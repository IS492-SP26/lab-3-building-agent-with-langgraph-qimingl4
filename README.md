[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/4huAMkyt)  

##  Module 1: Structured Output & Intent Routing
*Transitioning from simple chat to programmable logic.*

###  Implementation Details:
* **Heuristic Router**: A `classify_intent` function acts as a "traffic controller," determining if a user requires a calculation (`math`), a research plan (`search`), or general assistance (`other`).
* **Structured Output**: In `search` mode, the LLM generates a Pydantic-backed `SearchQuery` object rather than raw text. This forces the model to provide a specific `search_query` string and a `justification`, making the output ready for immediate consumption by a Search API.
* **Native Tool Calling**: In `math` mode, the LLM is bound to a `multiply` function. Instead of attempting the calculation itself—where LLMs frequently hallucinate—it emits a **Tool Call** with precise arguments for a Python function to execute.

---

##  Module 2: Orchestrator-Worker Pattern (LangGraph)
*Scaling complexity through high-concurrency execution and synthesis.*

###  Key Improvements (v2.0):

#### 1. Scalable Planning (The Orchestrator)
* **Expansion**: The Orchestrator was upgraded to plan **6 distinct sections** (up from 4).
* It utilizes a `SectionsPlan` schema to provide every worker with a clear "mission briefing," including a specific section name and description.

#### 2. Parallel "Fan-out" Workers
The system triggers a multi-threaded workflow where the following nodes run simultaneously:
* **6x Section Writers**: Parallelized nodes that draft individual markdown chapters.
* **Bullets Worker**: Generates 5 concise key takeaways.
* **Glossary Worker**: Defines technical terms (4–6 entries).
* **Title Worker**: Crafts a compelling, informative report headline.
* **References Worker [NEW]**: A newly added node that suggests 3–5 relevant source titles to add academic credibility.

#### 3. Quality & Constraint Controls
* **Length Guard**: Implemented hard truncation logic in the `section_writer`. Any section exceeding **120 words** is automatically trimmed to ensure an executive-summary style.
* **Deduplication**: Added logic to clean up glossary entries and bullet points, preventing the LLM from repeating the same information across different nodes.

#### 4. The Synthesizer (The Join Node)
* Acts as the final assembly line.
* It collects fragments from the Graph State (Title, Sections, Bullets, Glossary, References) and weaves them into a single, professionally formatted **Markdown Report**.

### Next Step
**Would you like me to help you wrap this code into a Streamlit or Gradio UI so you can run these reports directly in your browser?**
