# GitHub 与 Sites 发布

## 两个仓库的职责

- 内容和源码主仓库：https://github.com/Eliosa/codexDailyFinance ，分支 main。
- Sites 部署仓库：平台为现有项目 appgprj_6aeecfc289bc81918e65080ff2d412d2 管理的仓库。
- 网站：https://market-brief-zw-20261107.carvedbuttercup1.chatgpt.site

GitHub 为公开仓库：提交的报告、源码和索引公开可见。Sites 仍保持仅本人访问。不得提交 .env、API 密钥或仓库凭据。

## 每次更新

1. 定时任务通过 GitHub 插件读取 main 最新提交；生成内容到 content/daily/YYYY/MM/YYYY-MM-DD.md 或 content/weekly/YYYY/YYYY-Www.md。
2. 按 HANDOFF.md 更新 content/manifest.json；同路径只保留一个当前索引，历史由 Git 提交保留。
3. 在独立 checkout 运行 node build.mjs，失败则停止，不提交、不部署。校验通过后以最新 main 为父提交提交内容，分支更新检查预期 SHA，避免覆盖并发修改。
4. 发布阶段使用 Sites 插件读取原项目访问权限，获取该项目的短期 source repository write credential。用 Sites 技能的 site-workflow helper 打开部署仓库。
5. 将已验证的 GitHub 提交中的 index.html、build.mjs、content/ 及相关文档复制到部署 checkout。保留现有 .openai/hosting.json 中项目 ID 和平台配置；不复制 .git 或秘密文件，不强推、不替换历史。
6. 通过 site-workflow 执行 node build.mjs、提交、推送与打包。再用 Sites 保存并部署该确切 commit/archive；仍为 owner-private 时使用 save_version_and_deploy_private。等待部署成功后才报告上线。
7. 通过 Sites 支持的服务访问读取 /content/manifest.json 和对应 Markdown，核对日期与原始 UTF-8 字节的 SHA-256。

## 当前状态与限制

GitHub push 不会自动发布 Sites。本仓库没有伪造自动部署 GitHub Action，也不保存 Sites 短期凭据。后续 Codex 任务需要同时具备 GitHub 写入和 Sites 发布工具权限；当前交互会话已验证 GitHub 写入，独立无人值守任务仍需验证。普通 GitHub Actions 环境不能假定拥有 Codex 插件或本机文件系统。

本次同步没有改变在线页面，因此不需要重新部署。网站现有日报为明确标记 partial 的真实来源核验记录，不是完整市场日报。
