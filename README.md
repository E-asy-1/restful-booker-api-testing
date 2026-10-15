# Restful Booker API Testing

## 项目简介

这是一个基于 Restful-Booker 酒店预订系统的 API 测试项目，用于实践和展示软件测试相关能力。

项目使用 **Postman** 设计和执行 API 测试，通过 JavaScript 编写 Post-response 测试脚本，对 HTTP 状态码、响应字段、数据类型及业务数据进行验证。同时使用 **Newman** 在命令行批量执行测试，并通过 **GitHub Actions** 实现 API 与 UI 自动化测试的持续集成。

项目还使用 Excel 管理测试用例，并通过 Git / GitHub 对测试资产进行版本管理。

## 测试范围

### API 测试

- **认证测试**：验证认证接口及 Token 获取。
- **查询测试**：验证 Booking 列表查询、指定 ID 查询及查询条件过滤。
- **创建测试**：验证正常创建、必填字段缺失等场景。
- **修改测试**：验证 PUT 全量更新及 PATCH 部分更新。
- **删除测试**：验证删除操作及删除后的资源状态。
- **数据校验**：检查响应字段、数据类型、业务数据及修改结果。
- **异常测试**：验证错误输入、认证相关操作及资源不存在等情况。

### UI 自动化测试

使用 Playwright 执行 Web UI 自动化测试，并通过 GitHub Actions 自动运行测试流程。

## 使用工具

| 工具 / 技术 | 用途 |
|---|---|
| Postman | 设计和执行 API 测试 |
| JavaScript | 编写接口响应断言及变量处理脚本 |
| Newman | 在命令行批量执行 Postman Collection |
| Playwright | 执行 Web UI 自动化测试 |
| Git / GitHub | 版本管理及项目托管 |
| GitHub Actions | 自动执行 API 和 UI 测试 |
| Excel | 编写和管理测试用例 |
| HTTP / REST API | 接口测试基础 |

## 项目结构

```text
restful-booker-api-testing/
├── .github/
│   └── workflows/
│       └── ci.yml
├── postman/
│   ├── restful-booker-api-testing.collection.json
│   ├── restful-booker.environment.json
│   └── newman/
│       └── api-test-report.html
├── playwright/
├── api测试用例.xlsx
└── README.md
```

以上结构为主要文件示意，具体文件以仓库实际内容为准。

## API 测试用例设计

测试用例结合软件测试基本方法进行设计，主要覆盖：

- **正常场景**：验证合法输入下接口能否完成预期操作。
- **异常场景**：验证非法参数、错误数据及资源不存在时的响应。
- **边界场景**：检查关键字段在边界值情况下的表现。
- **响应校验**：验证响应字段、数据类型及关键业务值。
- **数据一致性**：通过后续查询验证创建或修改后的数据。
- **优先级划分**：根据功能重要程度划分 P0、P1、P2 等优先级。

测试用例使用 Excel 进行管理，并记录预期结果、实际结果及测试状态。

## Postman 测试脚本示例

以下示例展示了如何验证创建 Booking 后的响应状态码及关键字段：

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
```

该代码为断言示例，具体测试数据应与当前 Collection 中的请求数据保持一致。

## 自动化执行与持续集成

### Newman 命令行执行

项目使用 Newman 执行 Postman Collection，并生成 HTML 测试报告。

示例命令：

```bash
newman run postman/restful-booker-api-testing.collection.json \
  -e postman/restful-booker.environment.json \
  -r cli,htmlextra
```

### GitHub Actions

项目通过 GitHub Actions 自动运行 API 和 UI 测试。在代码推送或 Pull Request 触发工作流后，分别执行 Newman API 测试与 Playwright UI 测试，并生成相应测试结果。

## 测试结果

本地最近一次 Newman 运行结果：

- 执行请求数：7
- 断言总数：30
- 失败请求：0
- 失败断言：0

GitHub Actions 中的 `API Tests (Newman)` 与 `UI Tests (Playwright)` 最近一次运行均成功通过。

HTML 测试报告位于：

`postman/newman/api-test-report.html`

## 项目实践与能力体现

通过本项目，实践了以下测试工作：

- 使用 Postman 设计和执行 API 测试。
- 使用 JavaScript 编写响应断言。
- 使用环境变量及动态变量传递测试数据。
- 使用 Newman 批量执行测试并生成 HTML 报告。
- 使用 Playwright 执行 UI 自动化测试。
- 使用 GitHub Actions 配置自动化测试流程。
- 使用 Excel 管理测试用例及测试结果。
- 根据实际响应分析接口行为，并结合接口规范判断是否属于缺陷。

## 后续改进方向

- 进一步完善异常输入和边界值测试。
- 提高测试数据管理与环境配置的可维护性。
- 完善缺陷记录、测试报告及测试文档。
- 根据项目需要补充更多自动化断言和持续集成检查。