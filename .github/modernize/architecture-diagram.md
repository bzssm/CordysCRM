# Architecture Diagram

CordysCRM is a full-stack CRM platform with a Vue 3 web/mobile frontend and a Spring Boot backend, backed by MySQL and Redis, with integrations to enterprise messaging platforms and AI services.

## Application Architecture

```mermaid
flowchart TB
    subgraph Clients["Client Layer"]
        WEB["Web Browser\nVue 3 + Naive UI"]
        MOB["Mobile Browser\nVue 3 + Vant"]
    end

    subgraph Backend["Backend - Spring Boot 3.5 / Java 21"]
        GW["HTTP Layer\nSpring MVC / Apache Shiro Auth"]
        BL["Business Logic Layer\nCRM Modules: Customers, Leads,\nOpportunities, Contracts,\nProducts, Follow-ups, Dashboard"]
        FW["Framework Layer\nCommon Utils, UID Generator,\nPermission, Pagination"]
        SCHED["Scheduler\nQuartz"]
        NOTIFY["Notifications\nSSE Push"]
        EXPORT["Export\nFastExcel"]
    end

    subgraph Data["Data Layer"]
        DB[("MySQL\nHikariCP + MyBatis +\nFlyway Migrations")]
        CACHE[("Redis\nSession Store + Cache\nRedisson")]
    end

    subgraph Integrations["External Integrations"]
        DINGDING["DingTalk"]
        WECOM["WeCom"]
        LARK["Lark / Feishu"]
        AI["AI Agent\nMaxKB"]
        DE["DataEase\nBI / Analytics"]
        SSO["SSO"]
    end

    WEB -->|"REST API / SSE"| GW
    MOB -->|"REST API"| GW
    GW --> BL
    BL --> FW
    BL --> SCHED
    BL --> NOTIFY
    BL --> EXPORT
    BL --> DB
    BL --> CACHE
    SCHED --> DB
    BL -->|"Webhook / API"| DINGDING
    BL -->|"Webhook / API"| WECOM
    BL -->|"Webhook / API"| LARK
    BL -->|"HTTP API"| AI
    BL -->|"HTTP API"| DE
    BL -->|"OAuth2 / SAML"| SSO
```
