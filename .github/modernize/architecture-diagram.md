# Architecture Diagram

CordysCRM is a full-stack CRM application built with Spring Boot 3 (Java 21) on the backend and a Vue 3 TypeScript monorepo on the frontend, deployed via Docker.

## Application Architecture

```mermaid
flowchart TB
    subgraph Clients["Client Layer"]
        WEB["Web Browser\nVue 3 + Naive UI + ECharts\nTypeScript + Vite"]
        MOB["Mobile Browser\nVue 3 + Vant\nTypeScript + Vite"]
        WECOM["WeCom / WeChat Work\nJS SDK Integration"]
    end

    subgraph AppServer["Application Server - Spring Boot 3.5.7 / Java 21 / Jetty"]
        subgraph AppModule["app module - Entry Point"]
            BOOT["Spring Boot Application\nStatic File Serving\nApp Listener"]
        end

        subgraph FrameworkModule["framework module - Cross-Cutting"]
            FW["Common Utilities\nUID Generation\nResponse Handling\nException Handling\nPaging"]
        end

        subgraph CRMModule["crm module - Business Logic"]
            BIZ["CRM Business Services\nCustomer / Opportunity / Contract\nProduct / Clue / Follow-up\nDashboard / Search / System"]
            SEC["Security Layer\nApache Shiro\nJWT Authentication\nSession Management"]
            API["REST API Layer\nSpringDoc OpenAPI\nSwagger UI\nAOP / Validation"]
            SCHED["Scheduler\nQuartz Jobs\nTask Automation"]
            UTIL["Utilities\nFastExcel Export\nJakarta Mail\nCommons Libraries"]

            subgraph Integrations["External Integrations"]
                LARK["Lark / Feishu"]
                DTALK["DingTalk"]
                WCOM["WeCom"]
                DEASE["DataEase Analytics"]
                SSO["SSO / OAuth2"]
                AGENT["AI Agent"]
                SQLBOT["SQLBot AI"]
                SYNC["Data Sync"]
            end
        end
    end

    subgraph DataLayer["Data Layer"]
        MYSQL["MySQL\nPrimary Database\nHikariCP Connection Pool\nFlyway Migrations\nMyBatis ORM\nPageHelper"]
        REDIS["Redis\nSession Store\nCaching\nRedisson Client\nSpring Session"]
    end

    subgraph ExtServices["External Services"]
        LARKAPI["Lark / Feishu API"]
        DTALKAPI["DingTalk API"]
        WCOMAPI["WeCom API"]
        DEAPI["DataEase API"]
        SSOAPI["Identity Provider\nSSO / OAuth2"]
        AIAPI["AI Service\nLLM API"]
        MAILSRV["Mail Server\nSMTP"]
    end

    WEB -->|"HTTPS REST"| API
    MOB -->|"HTTPS REST"| API
    WECOM -->|"HTTPS REST"| API

    API --> SEC
    SEC --> BIZ
    BIZ --> FW
    BIZ --> SCHED
    BIZ --> UTIL
    BIZ --> Integrations

    BOOT --> API
    FW --> MYSQL
    BIZ --> MYSQL
    BIZ --> REDIS
    SEC --> REDIS

    LARK -->|"Lark API calls"| LARKAPI
    DTALK -->|"DingTalk API calls"| DTALKAPI
    WCOM -->|"WeCom API calls"| WCOMAPI
    DEASE -->|"DataEase API calls"| DEAPI
    SSO -->|"OAuth2 / SAML"| SSOAPI
    AGENT -->|"LLM API calls"| AIAPI
    SQLBOT -->|"LLM API calls"| AIAPI
    UTIL -->|"Send email"| MAILSRV
```
