# Restful Booker API Testing

## 项目简介

这是一个基于 Restful-Booker 酒店预订系统的 API 测试项目，主要用于练习和展示软件测试岗位所需的 API 测试能力。

项目使用 Postman 设计和执行 API 测试，通过 JavaScript 编写 Post-response 测试脚本，对接口的 HTTP 状态码、响应字段、数据类型、业务数据及数据一致性进行验证。

同时结合 Excel 编写测试用例，并使用 Git / GitHub 对测试资产进行版本管理。
---

## 测试范围

本项目目前主要覆盖 Restful-Booker API 的以下功能：

- **认证接口**：测试登录认证及 Token 获取
- **Booking 查询**：测试获取 Booking 列表及根据 ID 查询 Booking
- **Booking 创建**：测试正常创建、异常数据及响应数据校验
- **Booking 修改**：测试认证后的 Booking 数据修改
- **Booking 删除**：测试认证后的 Booking 删除及删除结果验证
- **数据一致性**：通过 API 请求结果验证创建或修改后的数据是否符合预期
- **异常场景**：测试非法数据、未认证请求及资源不存在等情况
---

## 使用工具

| 工具 / 技术 | 用途 |
|---|---|
| Postman | API 请求发送、接口测试与结果验证 |
| JavaScript | 编写 Postman Post-response 测试脚本 |
| Excel | 编写和管理 API 测试用例 |
| Git | 测试脚本和测试用例的版本管理 |
| GitHub | 项目代码及测试资产托管 |
| REST API | 被测接口类型 |
| HTTP | 接口请求与响应通信协议 |
---

## 项目结构

```text
restful-booker-api-testing/
├── postman/
│   └── restful-booker-api-testing.collection.json
├── api测试用例.xlsx
└── README.md
```
---

## API 测试用例设计

测试用例结合软件测试基本方法进行设计，主要覆盖：

- **正常场景**：验证接口在合法输入下能够正常完成业务操作
- **异常场景**：验证非法参数、错误数据及未认证请求的处理情况
- **边界场景**：验证关键字段在边界值情况下的系统表现
- **数据校验**：验证响应字段是否存在、数据类型是否正确以及数据是否符合业务规则
- **数据一致性**：通过后续 GET 请求验证创建或修改后的数据是否与预期一致
- **优先级划分**：根据功能重要程度划分 P0 / P1 / P2 等优先级

测试用例统一使用 Excel 管理，并记录测试结果及问题情况。
---

## Postman 测试脚本

项目使用 Postman 的 Post-response Scripts 编写 JavaScript 断言，对接口响应进行自动化校验。

例如创建 Booking 后，验证 HTTP 状态码、`bookingid`、姓名以及价格等关键数据：

```javascript
pm.test("Successful POST request", function () {
    pm.expect(pm.response.code).to.be.oneOf([200, 201]);
});

pm.test("Created booking contains bookingid", function () {
    const data = pm.response.json();

    pm.expect(data).to.have.property("bookingid");
});

pm.test("Created booking has correct firstname", function () {
    const data = pm.response.json();

    pm.expect(data.booking.firstname).to.equal("Tom");
});

pm.test("Created booking has correct totalprice", function () {
    const data = pm.response.json();

    pm.expect(data.booking.totalprice).to.equal(100);
});
```
---

## 测试结果

本项目通过 Postman 实际执行 API 测试，并根据测试结果持续调整和完善测试脚本。

当前已完成并验证的主要接口包括：

| 接口 | 测试内容 |
|---|---|
| `POST /auth` | 认证及 Token 获取 |
| `GET /booking` | Booking 列表查询及响应数据校验 |
| `GET /booking/{id}` | 指定 Booking 查询及字段、类型、业务规则校验 |
| `POST /booking` | Booking 创建及响应数据校验 |
| `PUT /booking/{id}` | Booking 修改及修改结果验证 |
| `DELETE /booking/{id}` | Booking 删除及删除结果验证 |

测试过程中针对异常数据进行了实际验证，例如 `totalprice` 传入字符串时接口返回 `200 OK`，但响应中的 `totalprice` 为 `null`。该结果已记录在测试用例中，并标记为“待确认”，避免在缺少明确接口规范的情况下直接将接口行为判定为缺陷。
---

## 能力体现

通过本项目，主要实践和掌握了以下 API 测试能力：

- 理解 REST API 的基本测试流程及 HTTP 请求 / 响应机制
- 使用 Postman 独立设计和执行 API 测试
- 使用 JavaScript 编写 Postman Post-response 断言
- 根据响应 JSON 结构进行字段、类型和值的校验
- 使用等价类、边界值及场景分析设计测试用例
- 覆盖正常、异常、边界、认证及数据一致性场景
- 使用 Excel 管理测试用例并记录实际测试结果
- 使用 Git / GitHub 管理测试项目及测试资产
- 能够根据实际测试结果分析接口行为，并结合需求判断是否属于缺陷
---

## 后续计划

后续将继续完善项目的自动化测试能力：

- 使用 Newman 批量执行 Postman Collection
- 增加更完整的异常及边界测试场景
- 增加环境变量和动态测试数据
- 尝试接入 GitHub Actions，实现 API 测试自动执行
- 根据测试过程中发现的问题完善缺陷记录和测试报告


