# Architecture Diagram

CordysCRM is a full-stack CRM application with a Vue 3 frontend and a Spring Boot backend, backed by MySQL and Redis.

## Application Architecture

```mermaid
flowchart TD
    subgraph Frontend["Frontend (Vue 3 + TypeScript + Vite)"]
        WEB["Web App\n(Vue 3, Pinia, Vue Router,\nElement Plus, ECharts)"]
        MOBILE["Mobile App\n(Vue 3, Pinia, Vue Router)"]
        SHARED["Shared Library\n(lib-shared)"]
        WEB & MOBILE --> SHARED
    end

    subgraph Backend["Backend (Spring Boot 3.5.7 / Java 21)"]
        APP["app module\n(Spring Boot Entry Point)"]
        CRM["crm module\n(Business Logic)\nCustomer / Opportunity /\nContract / Dashboard /\nSystem / Home"]
        FRAMEWORK["framework module\n(Cross-cutting Concerns)\nAOP / UID / Excel / Security /\nCommon Utilities"]
        APP --> CRM
        APP --> FRAMEWORK
        CRM --> FRAMEWORK
    end

    subgraph Security["Security and Auth"]
        SHIRO["Apache Shiro\n(AuthN / AuthZ)"]
        JWT["JWT Tokens"]
        RSA["RSA Encryption"]
    end

    subgraph Data["Data Layer"]
        MYSQL[("MySQL\n(HikariCP Connection Pool)\nFlyway Migrations")]
        REDIS[("Redis\n(Redisson / Spring Session)\nSession Store and Cache")]
    end

    subgraph Integrations["External Integrations"]
        MAIL["Email Service\n(Spring Mail)"]
        QUARTZ["Quartz Scheduler\n(Background Jobs)"]
        SWAGGER["OpenAPI / Swagger UI\n(API Docs)"]
    end

    WEB -- "REST/JSON" --> APP
    MOBILE -- "REST/JSON" --> APP
    APP --> SHIRO
    SHIRO --> JWT
    SHIRO --> RSA
    CRM -- "MyBatis ORM\nPageHelper" --> MYSQL
    CRM -- "Redisson Client" --> REDIS
    APP --> MAIL
    APP --> QUARTZ
    QUARTZ --> MYSQL
    APP --> SWAGGER
```
