Google ADK is a modilar, production-ready framework for building LLM-powered agents. It helps the developers to build powerful, flexible and interoperable multi-agent application.
## Why to use Agent Development Kit?
ADK provides the flexibility of Python with built-in structures for state management, callbacks, streaming, and structured input/output. Let’s take a look at its main features:
- Multi-agent by design: ADK can compose agents in parallel, sequential, or hierarchical workflows.
- Model-agnostic: It works with Gemini, GPT-4o, Claude, Mistral, and others via `LiteLlm`.
- Modular and scalable: The user can define specialized agents and delegate intelligently using built-in orchestration.
- Streaming-ready: It supports real-time interaction, including bidirectional audio/video.
- Built-in tooling: It supports local CLI and web UI for debugging and evaluation.
- Supports deployment: ADK easily containerizes and deploys agents across environments.
---
## Installing ADK
```python
pip install google-adk
```
---
## Folder Structure
To create the agent we need to flow the project structure that is mentioned by the google-adk
``` 
parent_folder/
	multi_tool_agent/
		__init__.py
		agent.py
		.env
```
- **`parent_folder/`**: This is the root directory for the entire project. It's a top-level container for all the project files and sub-directories.
- **`multi_tool_agent/`**: This is the core directory for the agent's code. It's a Python package, indicated by the `__init__.py` file.
- **`__init__.py`**: This file marks the directory as a Python package. It can also contain initialization code for the package, like setting up logging or defining package-level variables.
- **`agent.py`**: This file likely contains the main logic for the multi-tool agent. This is where you would define the agent's behavior, how it uses different tools, and its decision-making process.
- **`.env`**: This file is used to store **environment variables**.
> When we are building the agents using the ADK we need to make the name of the agent and folder name needs to be same
---
## Core Concepts
- **Agents**: These are the central decision-making entities that take action. The ADK offers several types:
    - **LLM Agents**: Use a large language model (LLM) as their core engine for reasoning, planning, and dynamic tool selection.
    - **Workflow Agents**: Control the execution flow of other agents in a predefined, deterministic way (e.g., sequentially or in parallel).
    - **Custom Agents**: Allow for the implementation of unique logic by extending the base `BaseAgent` class.
- **Tools**: These are functions or capabilities that agents can use to perform specific actions, such as searching the web, executing code, or retrieving data from a database.
- **Runners**: These components manage the execution flow of agents and handle messages, events, and state management.
![[ADKAgents.png]]
---
##  Creating an Agent

### 1. Set Up the Environment

Before you begin, you need to set up your development environment. This typically involves:

- Creating a Google Cloud Project and enabling billing.
- Enabling necessary APIs like AI Platform API in your Google Cloud Project.
- Setting up a virtual environment (e.g., using `uv` or `venv`) to manage dependencies.
- Installing the Google ADK library with `pip install google-adk`.
### 2. Define Your Agent
You can create an agent using the ADK's Command Line Interface (CLI) or by writing Python code. The core components you define are:
- **Model**: Specify the language model the agent will use (e.g., `gemini-2.0-flash`).
- **Name**: Give your agent a unique name.
- **Instruction**: Write a clear, detailed prompt that defines the agent's role, persona, and guidelines for how to use its tools. This is where the "magic" happens, as it tells the agent how to behave. Use specific formatting like headers and bullet points for readability.
- **Tools**: Provide a list of tools that the agent can access. The ADK's `LlmAgent` will dynamically decide which tool to use based on the user's query and your instructions.
### 3. Implement Tools (if needed)
For custom tools, you create a Python function and use its docstring to describe its purpose. The ADK uses this information to understand when and how to call the tool. Best practices for tools include:
- Using clear, descriptive function names (e.g., `fetch_weather_forecast`).
- Defining a clear function signature with type hints.
- Handling errors gracefully and returning a structured response.
### 4. Manage Agent State and Sessions
ADK uses sessions to manage the context of a conversation. It supports both in-memory sessions for local development and cloud-based managed sessions for deployment. You can also define memory services to give your agent a long-term memory that persists across different sessions.
### 5. Run and Test Your Agent
You can run your agent locally from the command line. `ADK web` or `ADK run` The ADK also includes built-in evaluation tools that allow you to systematically test your agent's performance against predefined test cases. 
## Tools
ADK allows to use several types of tools:
1. Function tools: The tools which are created by us nothing the python functions.
	- Function: Methods: python function
	- Agent-as-tools: using another agent as a tool
	- Long Running Function Tools: The function which take significant amount of time to complete
2. Built-in tools: The tools which are provided by the framework
3. Third party tools: The tools form different libraries.
## Conversational Context
### Why Context Matters

- In multi-turn conversations, you want the system to remember what’s been said/done earlier, so replies make sense, avoid repetition, maintain coherence. 
- You also often want personalized behavior (user preferences, past interactions) and continuity across different sessions. ADK provides abstractions to enable this.
---
## Core Components: Session, State, Memory
ADK splits context into three related but distinct pieces:

| Component                   | Purpose / What it stores                                                                                                                                                                                                                                                     | Lifetime & Scope                                                                                                                                                                                                                                       | Key Uses                                                                                                                                                                                                           |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Session**                 | Represents _one conversation thread_ between a user and the agent. Holds the sequence of events (user messages, agent replies, tool usages) for that conversation.                                                                                                           | From when a user starts interacting until that conversation is ended (or abandoned/expired). Scoped to that session only.                                                                                                                              | To retrieve the history of what was said/done so far in this chat. To maintain “turns” (what to reference in a followup). To manage multi-step flows (e.g. wizard dialogs, quizzes).                               |
| **State** (`session.state`) | Data associated with that session that you want to persist _during_ it: temporary information relevant to this conversation. For example, which question number you are on in a quiz; whether certain user preferences were expressed in this session; intermediate choices. | Lives for the duration of the session. Once the session ends (or with certain resets), that state is lost (unless persisted elsewhere). If you use persistent session storage, state can survive restarts of the service, but only for _that_ session. | To keep track of conversation flow, partial results, collecting user inputs over multiple turns, controlling branching logic. Also helps reduce re-prompting (you already know what user said earlier in session). |
| **Memory**                  | A long-term store of information, possibly aggregated across many sessions. This is for context you want to recall in _future_ sessions: user preferences, past behaviors, previous conversations etc.                                                                       | Spans sessions. Designed to survive session restarts, agent restarts, etc., when configured with persistent/backed storage. Not every bit of session history becomes memory; usually only relevant, distilled parts.                                   | To personalize user experience over time. To recall things like “last time you asked me …”, “you prefer flights from X”, “your score/history in quiz”, etc. To avoid starting every session from scratch.          |

---
### How ADK Implements These: services & tools
#### SessionService
- Manages sessions: creating, retrieving, appending events, modifying state, deleting sessions.
- Different implementations depending on need:
    - **In-MemorySessionService** — easy, no persistence, good for dev/prototyping. 
    - **DatabaseSessionService** — persistent storage in relational database. For production. 
    - **VertexAiSessionService** — using Google Cloud / Vertex AI infrastructure. Scales, managed, persistent.
#### MemoryService
- Manages the long-term memory store: ingest session(s), search memory, retrieve relevant items.
- Key operations:
    1. **add_session_to_memory** — at certain times (end of session or after relevant events) you push content from a session into memory. The system may extract summaries or “memories” from session events. 
    2. **search_memory** — given a query, find relevant past memories. Could be keyword match or semantic (vector embedding) search. 
- Memory backends:
    - **InMemoryMemoryService** — simple, no persistence, good for testing. 
    - **VertexAiMemoryBankService** — a managed Google Cloud service (Memory Bank) that handles persistence, indexing, semantic search etc.
---
## How They Work Together (Typical Workflow)
Here’s how these pieces interact in a real ADK agent:
1. **Session starts** — user sends first message → SessionService creates a Session with app_name, user_id, session_id. `session.state` can start with some initial values.
2. **User & Agent exchange messages**, Tools may be invoked, and each turn you:
    - Add events to the session’s events history.
    - Update `session.state` to store intermediate info (e.g. current step in a flow, flags etc.).
3. **During the session**, you may also query Memory: e.g. “Do I already know this user’s preference?” using `search_memory`. Memory results may enrich the prompt or guide behavior.
4. **At appropriate point(s)** (often at session end, or via callback, or periodically), you call `add_session_to_memory(session)` to ingest relevant parts (maybe summarised or filtered) into the long-term memory store. 
5. **Future sessions** can recall from memory so agent has context from prior interactions. This helps with personalization. 
---
## Key Details / Nuances & Best Practices

- **State Key Prefixes**: ADK supports certain prefixes on state keys like `user:`, `app:`, `temp:` etc., to distinguish scope. For instance:
    - `user:<something>` may be intended to persist across all sessions for that user.
    - `app:<something>` for things global to the application.
    - `temp:` for data only relevant in the current invocation or very short-term. 
- **Persistence of State**: Even within a session, whether the state persists when your service restarts depends on which SessionService you use. InMemory loses everything on restart; Database or VertexAi ones persist.
- **Filtering / Summarizing Memory**: You don’t want to store _everything_ (unless cost/storage/efficiency are non-issues); memory is more useful when you store _relevant_, _important_ facts (user preferences, goals, repeated interactions, etc.). ADK/MemoryBank may help by extracting key memories rather than dumping full session transcripts.
- **Controlling Memory Retrieval**: When calling `search_memory`, setting appropriate limits (how many results), selecting relevant query strings, etc., to avoid overwhelming prompts or irrelevant context. Also consider privacy implications of intentionally retrieving past information.
- **Session Expiry / Cleanup**: In production, you’ll want policies for when sessions are considered “ended” or should be deleted (e.g. after a timeout, or after a flow finishes), to avoid unbounded growth.
- **Latency & Scalability**: Using persistent storages and memory search (especially semantic search) can add latency. Choose backends accordingly, cache when possible, optimize what you store & how you index.
---
### Some Examples
To make this more concrete, here are simplified examples (pseudocode) showing how one might use these in ADK (based on documentation):

```python
from google.adk.sessions import InMemorySessionService
from google.adk.memory import VertexAiMemoryBankService

session_svc = InMemorySessionService()
memory_svc = VertexAiMemoryBankService(project_id, location, agent_engine_id)

# Start session
session = await session_svc.create_session(app_name="my_bot", user_id="user123", session_id="sess1")
# user says something
user_msg = "Hi, my favorite color is blue."
# agent code processes, updates session state
session.state["user:favorite_color"] = "blue"

# Later, after session is done:
await memory_svc.add_session_to_memory(session)

# In a new session
session2 = await session_svc.create_session(app_name="my_bot", user_id="user123", session_id="sess2")
# when user says something, agent checks memory:
mem = await memory_svc.search_memory(query="favorite color")
# memory might return: favorite_color = blue
# then agent says: "Welcome back! I remember your favorite color is blue. How can I help today?"
```

---
## When to Use What
- Use **Session + State** whenever you need to track conversation flow, immediate context. For things that only matter _now_.
- Use **Memory** for things you want to persist across conversations or long gaps: user profile, preferences, past tasks, history.
- If you expect the same user to come back and reference past conversations, memory is essential.