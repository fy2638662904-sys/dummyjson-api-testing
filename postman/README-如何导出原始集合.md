# 如何从 Postman 导出原始集合并替换本目录文件

本目录里的集合是按测试报告重建的版本。如果你本地 Postman 里还保留着当时实际执行的那份，导出它就能让仓库里的证据变成「原始版本」。

## 导出集合

1. 打开 Postman，左侧找到集合 `API接口测试项目`（或你当时使用的名字）。
2. 点集合右侧的 `...` → **Export**。
3. 选择 **Collection v2.1** → 导出为 JSON 文件。
4. 把导出的文件复制到本目录，覆盖 `DummyJSON接口测试.postman_collection.json`（或改名保留两份）。

## 导出环境

1. 右上角环境下拉框 → 环境右侧 `...` → **Export**。
2. 导出为 JSON 文件，覆盖 `DummyJSON接口测试.postman_environment.json`。

## 导出前检查

- 环境变量里如果有真实 Token 或账号密码，导出后请先把值改成占位符（例如 `{{token}}`、空字符串），再提交到 GitHub。
- 导出文件可以在 Postman 里用 **Import** 再打开一次，确认内容是完整可执行的。
