#  “Agentes Autônomos – Análise de CSV”.
# Epopeia - Agente de Análise de Notas Fiscais

## Visão Geral do Projeto

Este projeto apresenta um Agente de IA inteligente desenvolvido para auxiliar na consulta e análise de notas fiscais. Utilizando a plataforma n8n para orquestração de fluxos de trabalho, o agente é capaz de ingerir automaticamente documentos (CSV e PDF) de uma pasta do Google Drive, processá-los, criar embeddings e armazená-los em um banco de dados vetorial (Supabase). Posteriormente, o agente de IA pode responder a perguntas complexas sobre o conteúdo dessas notas fiscais, fornecendo detalhes como valores, datas, fornecedores e itens.

## Funcionalidades

*   **Ingestão Automatizada:** Monitora uma pasta específica do Google Drive para novos arquivos CSV e PDF, realizando a ingestão automática.
*   **Processamento de Documentos:** Extrai texto de PDFs e dados de CSVs, preparando-os para a criação de embeddings.
*   **Banco de Dados Vetorial:** Armazena os embeddings dos documentos no Supabase, permitindo buscas semânticas eficientes.
*   **Agente de IA Conversacional:** Permite que os usuários façam perguntas em linguagem natural sobre as notas fiscais, e o agente recupera e sintetiza as informações relevantes.
*   **Memória de Conversa:** O agente mantém o contexto da conversa, proporcionando uma experiência de usuário mais fluida.

## Tecnologias Utilizadas

*   **n8n:** Plataforma de automação de fluxo de trabalho.
*   **LangChain:** Framework para desenvolvimento de aplicações com LLMs.
*   **OpenAI:** Modelos de linguagem (GPT-4o, GPT-4o-mini) e embeddings.
*   **Supabase:** Banco de dados vetorial (PostgreSQL com extensão pgvector).
*   **Google Drive:** Serviço de armazenamento em nuvem para a fonte de dados.

## Estrutura da Solução

A solução é composta por um fluxo de trabalho no n8n, dividido em duas partes principais:

1.  **Fluxo de Ingestão de Dados:**
    *   **Gatilho:** `Google Drive Trigger` (monitora criação/atualização de arquivos).
    *   **Processamento:** `Google Drive` (download), `Switch` (roteamento por tipo de arquivo - PDF/CSV), `Extract from File` (extração de texto/dados).
    *   **Preparação:** `Default Data Loader`, `Recursive Character Text Splitter` (divisão de texto em chunks).
    *   **Embeddings e Armazenamento:** `Embeddings OpenAI` (criação de vetores), `Supabase Vector Store` (armazenamento no Supabase).

2.  **Fluxo do Agente de IA Conversacional:**
    *   **Gatilho:** `When chat message received` (inicia a interação com o usuário).
    *   **Agente Principal:** `AI Agent` (orquestra a conversa).
    *   **Modelos de Linguagem:** `OpenAI Chat Model` (GPT-4o para o agente principal, GPT-4o-mini para a ferramenta de busca).
    *   **Memória:** `Postgres Chat Memory` (mantém o histórico da conversa).
    *   **Ferramenta de Busca:** `Answer questions with a vector store` (ferramenta personalizada que interage com o `Supabase Vector Store` para recuperar informações relevantes).

## Como Configurar e Executar (Visão Geral)

1.  **Configurar Credenciais:** No n8n, configure as credenciais para Google Drive, OpenAI e Supabase.
2.  **Configurar Supabase:** Crie um projeto no Supabase, configure a extensão `pgvector` e crie a tabela `documents` (ou a tabela que você configurou no fluxo) com as colunas necessárias (id, conteudo, embedding, metadata).
3.  **Importar Fluxo n8n:** Importe o arquivo JSON do fluxo de trabalho para o seu ambiente n8n.
4.  **Definir Pasta do Google Drive:** No nó `Google Drive Trigger`, configure a pasta específica que será monitorada para os arquivos de notas fiscais.
5.  **Ativar o Fluxo:** Ative o fluxo de trabalho no n8n.
6.  **Interagir:** Acesse a URL do webhook do nó `When chat message received` para interagir com o agente.

## Exemplos de Uso

Após a ingestão dos seus arquivos de notas fiscais, você pode perguntar ao agente:

*   "Qual a nota fiscal de maior valor?"
*   "Me dê os detalhes da compra de 29 de janeiro de 2024."
*   "Existe alguma nota fiscal relacionada a materiais de escritório?"
*   "Qual a nota fiscal de menor valor?"

## Contribuição

Este projeto foi desenvolvido pelo Grupo Epopeia.

**Membro:** Miriam Oliveira de Aguiar Sobral

---
