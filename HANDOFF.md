# 内容发布约定

项目 appgprj_6aeecfc289bc81918e65080ff2d412d2；分支 main。

任务每次通过 Sites create_source_repository_write_credential 获取短期授权，使用 Sites site-workflow helper 打开独立 checkout。不得依赖本机目录或复用过期凭据。先检查远端最新提交。

内容位于 content/daily/YYYY/MM/YYYY-MM-DD.md、content/weekly/YYYY/YYYY-Www.md。manifest 为 schema_version:1、updated_at:ISO8601、files 数组；每条包含 path/date/type/updated_at/sha256。sha256 为原始 UTF-8 字节的小写十六进制摘要。date：日报是北京时间日期，周报是 ISO 周编号。按路径替换同一条索引，不追加重复路径；保留其他条目。历史修正使用 Git 新提交，禁止 force push 或重写历史。

Front Matter 使用 UTF-8、LF、文件首部 --- 和独立结束 ---。支持简单单行 YAML 标量，禁止复杂对象、数组、锚点。必填 schema_version:1、type:daily|weekly、date、timezone:Asia/Shanghai、generated_at:带时区的 ISO8601、title、status:complete|partial。partial 页面显示不完整。

每次先在独立工作副本中准备内容和 manifest，运行 node build.mjs；校验通过才提交。使用 site-workflow 包装构建与推送，随后显式调用 Sites 保存版本和部署，轮询至成功。push 本身不保证上线。失败时旧部署继续服务。Git 保留版本，不会按月份清理。

读取 /content/manifest.json 和原始 /content/...md，比较日期和 SHA-256；展示数据 /reports.json。访问权限由 Sites 保持，自动任务的读取可用 Sites 提供的服务访问方式，不把凭据写入文件或任务提示。

尚须在全新无人值守任务中验证凭据工具和部署工具可用；本次交互凭据获取不证明定时任务具备同样权限。
