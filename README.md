# DummyJSON API 接口测试项目

用 Postman 对 DummyJSON 练习 API 做的一轮接口测试，覆盖登录认证、用户信息、错误 Token 与不存在资源四类场景，重点练习「正常 / 异常 / 边界 / 参数缺失 / 鉴权失败」的用例设计思路，以及用 Postman Tests 对状态码、JSON 响应、关键字段和内容做断言。

- 测试对象：DummyJSON 练习 API（`https://dummyjson.com`）
- 主要接口：`POST /auth/login`、`GET /auth/me`、`GET /users`、`GET /users/99999`
- 认证方式：Bearer Token
- 用例数量：14 个请求（API-001 ~ API-014）
- 执行结果：51 个断言全部通过，`0 Failed / 0 Errors`，平均响应时间约 367 ms（见 `docs/` 中的测试报告）

## 测试范围

| 分组 | 用例 | 覆盖点 |
| --- | --- | --- |
| 登录认证接口 | API-001 ~ API-010 | 正常登录、错误密码、错误用户名、用户名/密码为空、特殊字符、超长输入、缺少 username、缺少 password |
| 用户接口 | API-011、API-012 | 携带 Token 获取当前用户、获取用户列表 |
| 异常场景 | API-013、API-014 | 错误 Token（401）、不存在资源（404） |

## 断言策略

| 断言类型 | 作用 |
| --- | --- |
| 状态码断言 | 验证 200 / 400 / 401 / 404 是否符合预期 |
| JSON 断言 | 验证响应体是 JSON 格式 |
| 字段断言 | 验证 `id`、`username`、`users`、`message`、`accessToken` 等关键字段 |
| 内容断言 | 对明确错误信息做精确匹配，例如 `Invalid credentials` |
| Token 断言 | 登录成功后检查 `accessToken` 并写入环境变量 `token` |

## 如何运行

1. 打开 Postman，导入：

   - `postman/DummyJSON接口测试.postman_collection.json`
   - `postman/DummyJSON接口测试.postman_environment.json`

2. 在右上角选择环境「DummyJSON 接口测试环境」。
3. 右键集合 → **Run collection**，`Iterations = 1`，执行顺序必须是默认顺序：

   **API-001 先执行**，它会把 `accessToken` 写入环境变量 `token`，API-011 依赖这个变量。

4. 查看 Collection Runner 的 Tests 统计与每个请求的 Test Results。

也可以用命令行（需安装 Node.js）：

```
npx newman run "postman/DummyJSON接口测试.postman_collection.json" -e "postman/DummyJSON接口测试.postman_environment.json"
```

## 目录结构

```
api接口测试/
├─ README.md
├─ docs/
│  └─ API接口测试项目_测试报告.docx      正式测试报告
└─ postman/
   ├─ DummyJSON接口测试.postman_collection.json
   ├─ DummyJSON接口测试.postman_environment.json
   └─ README-如何导出原始集合.md
```

## 说明与局限

- 本仓库中的集合是**按正式测试报告重建**的版本，用于让测试可复现；报告里的 51 个断言来自原始执行，重建版本的断言条数可能略有差异。
- 被测对象是公开练习 API，业务深度有限；本项目的价值在于接口测试思路与断言设计，而不是业务复杂度。
- 仓库中的账号密码是 DummyJSON 官方文档公开的演示账号，不是真实凭据。
