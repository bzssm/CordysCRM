# Architecture Diagram

CordysCRM is a full-stack CRM application with a Vue 3 frontend and a Spring Boot backend, backed by MySQL and Redis.

## Application Architecture

```mermaid
flowchart TB
    subgraph Clients["Client Layer"]
        WEB["Web Browser\nVue 3 + Naive UI + TypeScript\nVite / pnpm"]
        MOB["Mobile Browser\nVue 3 + TypeScript\nVite / pnpm"]
    end

    subgraph Frontend["Frontend Build (Maven + Node 22)"]
        LIB["lib-shared\nShared Components and Utilities"]
    end

    WEB --> LIB
    MOB --> LIB

    subgraph Backend["Backend - Spring Boot 3.5 / Java 21 (Jetty)"]
        API["REST API Layer\nSpringDoc / OpenAPI / Swagger UI"]
        SEC["Security Layer\nApache Shiro + JWT"]
        BIZ["Business Logic Layer\nCRM Modules: Customer, Clue, Contract,\nOpportunity, Product, Follow, Dashboard,\nSystem, Integration, Search"]
        DAL["Data Access Layer\nMyBatis + PageHelper + Flyway"]
        SVC["Cross-Cutting Services\nQuartz Scheduler, FastExcel,\nJakarta Mail, AOP, Validation"]
    end

    LIB -- "HTTP REST" --> API
    API --> SEC
    SEC --> BIZ
    BIZ --> DAL
    BIZ --> SVC

    subgraph Storage["Data Storage"]
        DB[("MySQL\nPrimary Database\nHikariCP Connection Pool")]
        CACHE[("Redis\nSession Store + Cache\nRedisson / Spring Session")]
    end

    DAL --> DB
    BIZ --> CACHE

    subgraph External["External Services"]
        WECOM["WeCom\nEnterprise WeChat Integration"]
        MAIL["Email Server\nJakarta Mail"]
    end

    SVC --> MAIL
    BIZ --> WECOM
```
