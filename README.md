## System Architecture

The Braindler architecture is designed in a modular way to ensure scalability and independent evolution of components. Below is a description of the main system components and their interactions:

- **1. User Interface (Telegram Bot):**  
  Users interact with the system through a Telegram bot. The bot implements the logic of receiving messages (e.g., using Telegram Bot API via webhook) and sending responses back to users. It acts as a **thin client**, forwarding requests to the core system. Additional interfaces (web chat, Slack bot, etc.) can be added later following the same principles.

- **2. Backend Server (Application):**  
  This is the central orchestrator, implemented as a web service (e.g., with FastAPI or Flask). Core functions:
  - Receives messages from the bot (via HTTP webhook), creates a request object containing the query text, user/chat info, and dialogue context.
  - **Processes the request:** Determines whether to answer the question or perform an action (e.g., with a simple classifier or rule-based logic).
  - **Calls the RAG subsystem:** Forms a search query based on the user's message and retrieves relevant knowledge fragments.
  - **Prepares a prompt for the LLM:** Combines user input and retrieved data into a single prompt, such as: *"Using the following information: [insert texts], answer the user's question: [question]."*
  - Sends the prompt to the LLM module and receives the generated answer.
  - If an action is required (e.g., create a calendar event), the server triggers the appropriate integration module and returns the action result to the user.
  - Sends the final response (text, buttons, links if applicable) back to the bot for delivery.
  - Logs all interactions and reports performance metrics.

- **3. RAG Service (Knowledge Retrieval):**  
  This subsystem handles Retrieval-Augmented Generation and includes:
  - **Knowledge storage:** A vector database (e.g., FAISS, Milvus, Pinecone) where each document fragment is stored as an embedding.
  - **Embedding model:** Converts text queries and documents into a vector space, typically with lightweight models (e.g., SentenceTransformers).
  - **Search logic:** Calculates embeddings for user queries, finds the nearest neighbors, and returns the top-N relevant fragments.
  - **Knowledge updates:** New documents are indexed and added to the vector database to keep knowledge up-to-date.

- **4. LLM Module:**  
  A large language model (LLM), such as Llama 3.1, deployed locally. The module:
  - Runs as a service (via RPC or HTTP) and accepts prepared prompts.
  - Requires GPU memory for hosting the model to ensure fast inference.
  - Can operate with streaming outputs to improve user experience.
  - Allows configuration of generation parameters like maximum length and temperature for determinism.

- **5. Integration Modules:**  
  Components to connect with external systems (e.g., Google Calendar API, CRM systems). They:
  - Are implemented as adapters or separate services.
  - Include an action manager to handle "intent recognition" (deciding between generating an answer or executing an action).
  - Require secured token handling and user authorization (OAuth, API keys).

- **6. Databases and Storages:**  
  Besides the vector store:
  - **Relational database** for user information, integration tokens, access rights.
  - **Dialogue history** storage to improve context and analytics.
  - **Logs and telemetry** data for reporting and optimization.

**Data Flow:**  
User → Bot → Backend → RAG (knowledge retrieval) → LLM (response generation) → Backend (action handling if needed) → Bot → User.  
All components log activities and send telemetry metrics. The architecture ensures **scalability**, where each service (bot, backend, RAG, LLM) can scale horizontally via containers and load balancing.

---

## Monitoring and Alerting Plan

To maintain the reliability of Braindler, a comprehensive monitoring system will be implemented, using Prometheus + Grafana (for metrics) and Alertmanager (for alerts). Main areas:

- **Performance and Latency Monitoring:**
  - Bot response time.
  - Backend processing time.
  - RAG retrieval time and LLM inference time.
  - **Metrics:** `request_duration_seconds`, `rag_search_time_ms`, `llm_inference_time_ms`.
  - **Alerts:** Trigger if response time exceeds 3 seconds consistently.

- **Resource Usage Monitoring:**
  - GPU utilization and memory usage.
  - CPU and RAM usage on backend and RAG nodes.
  - Disk space and I/O performance.
  - **Metrics:** `gpu_utilization_percent`, `cpu_usage_percent`, `disk_free_bytes`.
  - **Alerts:** High CPU (>85%), low RAM (<10%), low disk space (<15%).

- **Service Stability Monitoring:**
  - Health checks for all services (backend, RAG, LLM).
  - Error rates (HTTP 5xx responses, application exceptions).
  - Container restarts monitoring.
  - **Metrics:** `service_up`, `http_requests_total{status="5xx"}`.
  - **Alerts:** Service down or high 5xx rate.

- **Quality of RAG and LLM Responses:**
  - Percentage of empty or unsuccessful answers.
  - RAG results count (number of documents returned).
  - User feedback (likes/dislikes).
  - **Metrics:** `unsuccessful_answers_total`, `rag_results_count`, `action_success_rate`.
  - **Alerts:** High percentage of empty RAG results or failed actions.

- **Business Metrics (external to monitoring system):**
  - Active user count.
  - Daily request volume.
  - Free-to-paid user conversion.

**Tools:**  
Prometheus exporters for each component, Nvidia exporter for GPUs, Grafana dashboards for visualization, and Sentry for detailed application error tracking.

---

## RAG Module Quality KPIs

Key metrics to measure RAG module performance:

- **Precision@K (Relevance):**  
  How often the top-K returned fragments are relevant.

- **Recall:**  
  How often relevant documents are successfully retrieved when they exist.

- **RAG Utilization Rate:**  
  How often RAG data is used in LLM responses.

- **Answer Accuracy/Helpfulness:**  
  Percentage of fully correct or helpful answers based on human or benchmark evaluation.

- **Average Number of Sources per Answer:**  
  How many document fragments are retrieved per user query (optimal around 3).

- **RAG Latency:**  
  Average retrieval time.

- **Knowledge Base Growth vs Performance:**  
  Monitor retrieval quality as the database size increases.

**Target Goals (for Version 1.0):**
- Precision@3 > 0.8
- Recall > 0.9
- RAG usage > 70% of queries
- User satisfaction (rating) > 4/5

---

## Additional Recommendations

- **Use modern LLM frameworks:**  
  Consider LangChain or similar libraries for faster development and better pipeline management.

- **Implement CI/CD:**  
  Automated testing and deployment pipelines for faster and safer updates.

- **Optimize for Russian language:**  
  Fine-tune Llama 3.1 on Russian datasets for better native performance.

- **Keep architecture modular:**  
  New features should be implemented as independent modules or plugins.

- **Security by Design:**  
  Validate all inputs, secure admin access, and provide deployment options for private environments.

- **User Experience Improvements:**  
  Add custom Telegram buttons, dialogue clarifications, and minimize misunderstanding.

- **GPU and Inference Optimization:**  
  Apply quantization (e.g., int8, int4) and batching to optimize LLM performance and resource usage.

- **Expand Integrations:**  
  Plugins for IDEs (e.g., VSCode), browser extensions, and internal knowledge systems (e.g., Confluence).

- **Data Confidentiality:**  
  Allow users to control data usage policies, offering strict on-record only modes if required.

- **Knowledge Base and Model Updates:**  
  Regular re-indexing of new documents and model fine-tuning with collected user interaction data.

- **Client Analytics:**  
  Provide analytics dashboards for enterprise clients to track usage, popular topics, and ROI of the assistant.
