# Architecture Diagram

CordysCRM is a full-stack CRM platform with a Vue 3 web and mobile frontend, a Spring Boot backend, MySQL for persistent storage, and Redis for caching and session management.

## Application Architecture

```mermaid
flowchart TB
    subgraph Clients["Client Layer"]
        WEB["Web Browser\nVue 3 + Naive UI + TypeScript + Vite"]
        MOB["Mobile Browser\nVue 3 + TypeScript + Vite"]
    end

    subgraph Backend["Backend Layer (Spring Boot 3.5 / Java 21 / Jetty)"]
        subgraph App["app module - Entry Point"]
            BOOT["Application.java\nSpring Boot Bootstrap"]
        end

        subgraph Framework["framework module - Cross-cutting Concerns"]
            SEC["Security\nApache Shiro + JWT"]
            CFG["Configuration\nSpring Config + AOP"]
            ORM["ORM Layer\nMyBatis + PageHelper"]
            FILE["File Handling\nFastExcel + Commons IO"]
            REG["Registry"]
        end

        subgraph CRM["crm module - Business Logic"]
            CTRL["REST Controllers\nSpringDoc OpenAPI"]
            SVC["Services\nCustomer, Lead, Opportunity,\nContract, Product, Follow-up,\nDashboard, Search, Integration"]
            DOM["Domain Objects\nEntities + Mappers + DTOs"]
        end

        subgraph Jobs["Scheduling"]
            QRTZ["Quartz Scheduler\nBackground Jobs"]
        end
    end

    subgraph Storage["Data Storage"]
        MYSQL[("MySQL\nPersistent Data\n+ Flyway Migrations")]
        REDIS[("Redis\nSession Store\n+ Distributed Cache\nRedisson")]
    end

    subgraph External["External Services"]
        MAIL["Email Server\nJakarta Mail"]
        WECOM["WeCom / WeChat Work\nEnterprise Integration"]
    end

    WEB -- "REST/JSON over HTTP" --> CTRL
    MOB -- "REST/JSON over HTTP" --> CTRL
    BOOT --> Framework
    BOOT --> CRM
    BOOT --> QRTZ
    CTRL --> SVC
    SVC --> DOM
    DOM --> ORM
    ORM -- "JDBC" --> MYSQL
    SEC -- "Session" --> REDIS
    SVC -- "Cache" --> REDIS
    SVC --> FILE
    SVC --> MAIL
    SVC --> WECOM
```
