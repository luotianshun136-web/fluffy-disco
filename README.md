# 卡彭团队 · 女性吸睛私人定制

公开品牌网站、免注册私密咨询，以及单管理员图文视频后台。

## 日常使用

- 网站首页展示团队介绍、服务流程、承接边界与已发布动态。
- 管理入口为网站地址后的 `/admin`。管理员账号为 `capone`；初始密码仅在本地 `.private/管理员登录.txt` 中提供，首次登录必须修改。
- 咨询收件箱支持查看、回复、归档和删除；客户端无需注册，在同一浏览器内继续会话。
- 团队动态支持草稿、发布、编辑、撤下与删除。每条帖子包含一张图片或一段视频，画幅限16:9或9:16。
- 图片支持 JPG、PNG、WebP，最大10 MB；视频支持 H.264 MP4，最大50 MB。
- 语音沟通与私人交付通过官方微信或 WhatsApp 承接，本站不提供付款与客户附件上传。

## 数据与权限

Sites 托管应用；D1 保存会话、消息、帖子和登录状态；R2 保存上传媒体。匿名会话使用 HttpOnly Cookie 隔离。管理接口逐次核验唯一管理员身份；未发布媒体不可匿名读取。

初始化接口仅接受部署密钥，并在管理员创建后关闭。公开注册与非必要账号接口均禁用。登录与留言具有限流和请求来源检查。

`.private/`、`.dev.vars`、`.env`、本地数据库、测试结果和构建产物不进入源代码仓库。请勿公开管理员凭据或服务密钥。

## 本地开发

需要 Node.js 22.13+ 与 npm。

```sh
npm ci
node scripts/prepare-secrets.mjs
npm run build
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0000_cute_morbius.sql
npm run dev
```

数据库迁移仅对尚未初始化的本地数据库执行一次。生产环境由 Sites 应用生成的迁移，并通过 Sites 配置 `.env.example` 中的运行变量。

`scripts/qa.mjs` 仅对本地预览运行，使用 Playwright 验证权限、消息、媒体与多端布局；测试结束会清理测试帖子、媒体与咨询。测试用的管理员新密码保存在本地 `.private/qa-password.json`，不影响生产环境初始凭据。

前台提供可选的 WebMCP 咨询窗口打开接口，浏览器不支持时正常忽略。测试浏览器不提供原生 WebMCP，因此未验证原生代理调用。
