# traffic-placement · 流量投放

流量落地页投放系统：从域名路由、落地页渲染，到订单支付，再到企业微信承接转化的完整链路。各环节拆分为独立可部署的子系统，统一在本组织下维护。

## 系统组成

| 仓库 | 定位 | 技术栈 |
| --- | --- | --- |
| `traffic_frontend` | 前台入口：OpenResty Lua 路由、落地页模板渲染与共享资源注入 | OpenResty / Lua / Nginx |
| `traffic_backend` | 业务后端：落地页订单、支付通道、投诉处理、报表与后台管理 | Nuxt 4 / TypeScript / Drizzle ORM |
| `work-wechat-callback-service` | 企业微信服务商代开发：回调验签解密、授权换码、外部联系人与订单联动 | Nuxt 4 / Nitro / MySQL / Drizzle ORM |
| `redirect-control` | 重定向与反向代理管理服务：内存规则引擎 + 可视化管理后台 | Go / Gin / GORM / SQLite + React / shadcn/ui |

## 请求链路

```mermaid
flowchart LR
    A[用户请求] --> B[redirect-control<br/>域名路由 / 重定向 / 反代回源]
    B --> C[traffic_frontend<br/>OpenResty 落地页渲染]
    C --> D[traffic_backend<br/>订单创建与支付]
    D --> E[work-wechat-callback-service<br/>企微回调与外部联系人联动]
    E -- 订单数据回传 --> D
```

典型业务流程：用户经域名路由进入落地页 → 浏览后创建订单并支付 → 支付成功引导添加企业微信 → 企微回调服务识别外部联系人、关联订单并回传后台，完成从流量到私域的承接闭环。

## 仓库约定

- 所有仓库默认私有，支付密钥、回调密钥等敏感配置一律不进仓库，走环境变量。
- 各仓库根目录的 `AGENTS.md` 为 AI 协作规范；业务文档统一放 `docs/**` 并按类型归档。
- 部署放行要求（traffic_frontend）：`/api/**`、`/report/**`、`/reports/**`、`/r/**` 必须透传至后端，不可被静态化拦截。
