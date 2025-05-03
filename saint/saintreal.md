graph TD
    subgraph "External Users"
        A[Users] -->|HTTPS| B[Azure Front Door<br>Global Load Balancing, WAF, SSL]
    end

    subgraph "Azure Region 1 (e.g., East US)"
        subgraph "Presentation Tier"
            B -->|Traffic Routing| C[Azure CDN<br>Static Content Delivery]
            B -->|Traffic Routing| D[Azure App Service<br>Frontend Web App<br>Auto-scaling]
            C -->|Cached Assets| D
        end

        subgraph "Application Tier"
            D -->|API Calls| E[Azure API Management<br>API Gateway, Rate Limiting]
            E -->|Secure Calls| F[Azure App Service<br>Backend API<br>Auto-scaling]
            F -->|Monitoring| G[Azure Application Insights<br>Performance Monitoring]
        end

        subgraph "Data Tier"
            F -->|Private Endpoint| H[Azure SQL Database<br>Geo-Replication, Backups]
            H -->|Secrets| I[Azure Key Vault<br>Secrets Management]
            H -->|Backup| J[Azure Backup<br>Scheduled Backups]
        end

        subgraph "Networking & Security"
            K[Azure Virtual Network<br>Subnets, NSGs]
            D --> K
            F --> K
            H --> K
            K -->|Traffic Filtering| L[Azure Firewall]
            K -->|DDoS Protection| M[Azure DDoS Protection]
            N[Azure Active Directory<br>Authentication, RBAC] --> D
            N --> F
        end
    end

    subgraph "Global Services"
        O[Azure Traffic Manager<br>Multi-Region Routing] --> B
        P[Azure Monitor<br>Logs, Metrics] --> D
        P --> F
        P --> H
        Q[Azure Alerts<br>Proactive Notifications] --> P
        R[Azure DevOps<br>CI/CD Pipeline] --> D
        R --> F
        S[ARM Templates<br>Infrastructure as Code] --> D
        S --> F
        S --> H
        T[Azure Site Recovery<br>Disaster Recovery] --> D
        T --> F
        T --> H
    end

    subgraph "Azure Region 2 (e.g., West Europe)"
        U[Replica: Azure App Service<br>Frontend & Backend]
        V[Replica: Azure SQL Database<br>Geo-Replicated]
        T --> U
        T --> V
        O --> U
        H -->|Geo-Replication| V
    end

    classDef tier fill:#e6f3ff,stroke:#333,stroke-width:2px;
    classDef security fill:#ffebcc,stroke:#333,stroke-width:2px;
    classDef global fill:#e6ffe6,stroke:#333,stroke-width:2px;
    class A,B,C,D,E,F,G,H,I,J tier;
    class K,L,M,N security;
    class O,P,Q,R,S,T,U,V global;
