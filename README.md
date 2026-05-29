# Project--Neural-Vault-Software-Requirements-Specification-Document
This project is a conceptual vision of software created for an academic project.

```mermaid
[graph TD
    %% 1. Definição dos Subgráficos e Componentes Internos
    subgraph Ingestao ["Fontes de Dados Externas (RNF-18)"]
        Slack["Canais do Slack"]
        Teams["Microsoft Teams"]
        GDrive["Google Drive"]
    end

    subgraph Backend ["Camada de Aplicacao: FastAPI Engine"]
        Gateway["FastAPI API Gateway"]
        Pipeline["Pipeline NLP Sequencial"]
        Retriever["Mecanismo de Busca Semantica RAG"]
    end

    subgraph Inteligencia ["Camada de IA e Persistencia de Contexto"]
        STTEngine["Speech-to-Text Engine"]
        LLM["Modelos de IA / LLM Vertical"]
        VecDB[("Banco Vetorial: Qdrant / pgvector")]
        RelDB[("Banco Relacional: PostgreSQL")]
    end

    %% 2. Componentes Isolados de Interface
    WebClient["Interface Web Responsiva"]

    %% 3. Relacionamentos e Fluxo de Dados
    Slack -->|Conector API| WebClient
    Teams -->|Conector API| WebClient
    GDrive -->|Conector API| WebClient

    WebClient -->|"Requisicoes HTTPS / TLS 1.3"| Gateway
    Gateway -->|"Fluxo de Ingestao de Audio e Texto"| Pipeline
    Gateway -->|"Consultas e Busca Semantica"| Retriever

    Pipeline -->|"Processamento de Reunioes"| STTEngine
    STTEngine -->|"Transcricao Estruturada"| LLM
    Retriever -->|"Busca de Contexto Semantico"| VecDB
    LLM -->|"Geracao de Resumos Estruturados"| RelDB
    LLM -->|"Geracao de Respostas Condicionadas"| WebClient

    %% 4. Estilizacao Explicita
    style WebClient fill:#E1F5FE,stroke:#0288D1,stroke-width:2px
    style Slack fill:#E1F5FE,stroke:#0288D1,stroke-width:1px
    style Teams fill:#E1F5FE,stroke:#0288D1,stroke-width:1px
    style GDrive fill:#E1F5FE,stroke:#0288D1,stroke-width:1px

    style Gateway fill:#E8F5E9,stroke:#388E3C,stroke-width:2px
    style Pipeline fill:#E8F5E9,stroke:#388E3C,stroke-width:2px
    style Retriever fill:#E8F5E9,stroke:#388E3C,stroke-width:2px

    style STTEngine fill:#EDE7F6,stroke:#5E35B1,stroke-width:2px
    style LLM fill:#EDE7F6,stroke:#5E35B1,stroke-width:2px]

    style VecDB fill:#FFF3E0,stroke:#F57C00,stroke-width:2px
    style RelDB fill:#FFF3E0,stroke:#F57C00,stroke-width:2px]
