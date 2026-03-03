# Architecture Diagram

CordysCRM is a full-stack CRM platform built with a Vue 3 frontend and a Spring Boot 3 multi-module backend, backed by MySQL and Redis, with integrations to external collaboration and BI tools.

## Application Architecture

```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        WEB["Web App\nVue 3 · TypeScript · Vite\nElement Plus · Pinia · ECharts"]
        MOBILE["Mobile App\nVue 3 · TypeScript · Vite"]
    end

    subgraph Backend["Backend - Spring Boot 3 / Java 21"]
        subgraph AppModule["app module\n(Entry Point)"]
            BOOT["Spring Boot Application\nSpringDoc OpenAPI\nQuartz Scheduler"]
        end

        subgraph FrameworkModule["framework module\n(Shared Infrastructure)"]
            SEC["Security\nApache Shiro · JWT"]
            AOP["AOP / Logging\nAspectJ"]
            COMMON["Common Utilities\nUID Generator · Response Handling"]
        end

        subgraph CrmModule["crm module\n(Business Logic)"]
            CTRL["REST Controllers\n/customer · /opportunity · /clue\n/contract · /product · /follow · /dashboard"]
            SVC["Services\nCustomer · Opportunity · Clue\nContract · Product · Dashboard"]
            MAPPER["Data Access\nMyBatis · PageHelper"]
            EXPORT["Export\nFastExcel"]
        end

        subgraph IntegrationModule["Integration Layer"]
            DINGTALK["DingTalk"]
            LARK["Lark / Feishu"]
            WECOM["WeCom"]
            DATAEASE["DataEase BI"]
            MAXKB["MaxKB AI Agent"]
            SSO["SSO"]
            TENDER["Tender System"]
        end
    end

    subgraph Storage["Data Storage"]
        MYSQL[("MySQL\nPrimary Database\nFlyway Migrations")]
        REDIS[("Redis\nCache · Session\nRedisson")]
        MINIO[("MinIO\nFile Storage")]
    end

    WEB -->|"REST API / HTTPS"| CTRL
    MOBILE -->|"REST API / HTTPS"| CTRL
    BOOT --> FrameworkModule
    BOOT --> CrmModule
    CTRL --> SVC
    SVC --> MAPPER
    SVC --> EXPORT
    MAPPER -->|"SQL"| MYSQL
    SEC -->|"Session / Token"| REDIS
    SVC -->|"Cache"| REDIS
    SVC -->|"Files"| MINIO
    SVC --> DINGTALK
    SVC --> LARK
    SVC --> WECOM
    SVC --> DATAEASE
    SVC --> MAXKB
    SVC --> SSO
    SVC --> TENDER
```
