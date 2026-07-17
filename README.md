# ContractFlow Admin

ContractFlow 是合同全生命周期后台与 Go Gin API，覆盖合同起草、审核、用印、电子签署、归档和到期续签。每次写操作都会记录状态事件与审计日志，重复提交使用 `Idempotency-Key` 安全重试。

## 关联仓库

- [ContractFlow Miniapp](https://github.com/xrlnewman/contractflow-miniapp)
- [个人博客项目页](https://field-notes-2fi.pages.dev/projects/contractflow-platform/)

## 技术栈与运行

Go 1.25 + Gin、MySQL 8.4、Redis 8、Vue 3 + Vite。执行 `docker compose -f deploy/docker-compose.yml up --build` 可启动完整 API、MySQL、Redis；未配置依赖时 API 自动回退内存演示数据。

```bash
go vet ./... && go test ./...
cd web && npm install && npm test && npm run build
```

## API 摘要

所有接口返回 `{ code, message, data, traceId }`，写接口支持 `Idempotency-Key`。合同状态依次为：合同起草 → 法务审核 → 审批用印 → 双方签署 → 自动归档 → 到期提醒与续签。

| Method | URL | 用途 |
| --- | --- | --- |
| GET | `/api/v1/dashboard` | 签约、审核、签署与合同金额指标 |
| GET | `/api/v1/warehouses` | 业务部门与合同空间 |
| GET | `/api/v1/products` | 合同分页与关键词搜索 |
| GET | `/api/v1/stocks/alerts` | 到期与续签提醒 |
| POST | `/api/v1/purchase-orders/:id/receive` | 合同审核完成 |
| POST | `/api/v1/sales-orders/:id/ship` | 签署归档确认 |

默认演示管理员：`13900000000` / `demo123456`，仅用于本地预览。

MIT © xrlnewman
