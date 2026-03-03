# Architecture Diagram

CordysCRM is a full-stack CRM application with a Vue 3 frontend and a Spring Boot backend, backed by MySQL and Redis.

## Application Architecture

```mermaid
flowchart TD
    subgraph Frontend["Frontend (Vue 3 + TypeScript + Vite)"]
        Web["Web App\nNaive UI · ECharts · Vue Router"]
        Mobile["Mobile App\nVue 3 · Vite"]
    end

    subgraph Backend["Backend (Spring Boot 3.5.7 · Java 21 · Jetty)"]
        API["REST API Layer\nSpringdoc OpenAPI"]
        Security["Security\nApache Shiro · JWT"]
        BizModules["CRM Business Modules\nCustomer · Opportunity · Contract\nClue · Follow-up · Product\nDashboard · Home · System · Search"]
        Integration["Integration Module\nWeCom · Email (Jakarta Mail)"]
        Framework["Framework Layer\nAOP · Exception Handling · UID · Paging"]
        DataAccess["Data Access Layer\nMyBatis · Flyway · PageHelper"]
        Scheduler["Scheduler\nQuartz"]
        Excel["Excel Export\nFastExcel"]
    end

    subgraph DataStores["Data Stores"]
        MySQL[("MySQL\n(Primary Database)")]
        Redis[("Redis\n(Session Cache · Distributed Lock)")]
    end

    subgraph External["External Services"]
        WeCom["WeCom\n(WeChat Work API)"]
        EmailSvr["SMTP Email Server"]
    end

    Web -->|"HTTP REST"| API
    Mobile -->|"HTTP REST"| API
    API --> Security
    Security --> BizModules
    BizModules --> Framework
    BizModules --> DataAccess
    BizModules --> Integration
    BizModules --> Scheduler
    BizModules --> Excel
    DataAccess --> MySQL
    Security -->|"Session / Cache"| Redis
    Scheduler --> DataAccess
    Integration --> WeCom
    Integration --> EmailSvr
```
