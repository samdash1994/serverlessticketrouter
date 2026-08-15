Python 3.11 Azure Functions Apache Spark License: MIT OWASP Serverless

An enterprise-grade, dual-engine customer support ticket routing platform built with Python, Azure Functions (Serverless), and Apache Spark (PySpark).

Designed to handle real-time HTTP ingestion with sub-millisecond classification latency while scaling to distributed batch analytics across large historical ticket archives.

🌐 Live Azure Endpoints
Health Check (GET): https://fn-ticket-router-elazar-01.azurewebsites.net/api/health
Ticket Routing API (POST): https://fn-ticket-router-elazar-01.azurewebsites.net/api/route_ticket
📐 System Architecture & Data Flow
graph LR
    subgraph Ingress & Compute
        Client[HTTP Client / Service Desk] -->|POST /api/route_ticket| AzFunc[Azure Function Python v2]
        AzFunc --> Auth[Managed Identity / App Key]
    end

    subgraph Core Processing Engine
        AzFunc --> RegEx[Weighted RegEx Classifier<br/>2x Subject Weight]
        AzFunc --> SLA[Dynamic SLA & VIP Escalation]
        AzFunc --> Entity[Entity Extractor<br/>CVEs, HTTP 500/502]
    end

    subgraph Egress & Observability
        RegEx --> Queue[(Functional Queues<br/>SecOps / Infra / Billing)]
        AzFunc --> Log[Structured JSON UTC Logs]
        Log --> Insights[Azure Application Insights]
    end

    subgraph Batch Layer
        Archive[(Ticket Lake / Blob Storage)] --> Spark[PySpark Batch Engine<br/>spark_router.py]
        Spark --> Metrics[Historical KPI & SLA Analytics]
    end


