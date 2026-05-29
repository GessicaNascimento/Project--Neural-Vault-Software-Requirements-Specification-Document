# Project--Neural-Vault-Software-Requirements-Specification-Document
This project is a conceptual vision of software created for an academic project.

```mermaid
[graph TD
    %% Definição de Estilos Visuais
    classDef client fill:#E1F5FE,stroke:#0288D1,stroke-width:2px;
    classDef api fill:#E8F5E9,stroke:#388E3C,stroke-width:2px;
    classDef ai fill:#EDE7F6,stroke:#5E35B1,stroke-width:2px;
    classDef db fill:#FFF3E0,stroke:#F57C00,stroke-width:2px;

    %% Ingestão e Fontes Externas
    subgraph Ingestao [Fontes de Dados Externas (RNF-18)]
        Slack[Canais do Slack]
        Teams[Microsoft Teams]
        GDrive[Google Drive]
    end

    %% Camada de Clientes
    WebClient[Interface Web Responsiva]:::client
    Ingestao -->|Conectores de API| WebClient

    %% Camada de Aplicação (Back-end)
    subgraph Backend [Camada de Aplicação: FastAPI Engine]
        Gateway[FastAPI API Gateway]:::api
        Pipeline[Pipeline NLP Sequencial]:::api
        Retriever[Mecanismo de Busca Semântica RAG]:::api
    end

    WebClient -->|Requisições HTTPS / TLS 1.3| Gateway
    Gateway -->|Fluxo de Ingestão de Áudio/Texto| Pipeline
    Gateway -->|Consultas e Busca Semântica| Retriever

    %% Camada de Inteligência Artificial e Dados
    subgraph Inteligencia [Camada de IA e Persistência de Contexto]
        STTEngine[Speech-to-Text Engine]:::ai
        LLM[Modelos de IA / LLM Vertical]:::ai
        VecDB[(Banco Vetorial: Qdrant / pgvector)]:::db
        RelDB[(Banco Relacional: PostgreSQL Soft Delete)]:::db
    end

    Pipeline -->|Processamento de Reuniões| STTEngine
    STTEngine -->|Transcrição Estruturada| LLM
    Retriever -->|Busca de Contexto Semântico| VecDB
    LLM -->|Geração de Resumos Estruturados| RelDB
    LLM -->|Geração de Respostas Condicionadas| WebClient

    %% Vinculando os estilos aos componentes fora do subgraph local
    class Slack,Teams,GDrive client;]
