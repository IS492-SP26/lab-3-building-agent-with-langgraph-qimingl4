[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/4huAMkyt)
Module 1: Structured Output & Intent Routing
Transitioning from simple chat to programmable logic.

🛠️ What was implemented:
Heuristic Router: A classify_intent function that acts as a traffic controller, deciding whether a user needs a calculation (math), a research plan (search), or a general chat (other).

Structured Output: Instead of raw text, the search mode uses a Pydantic-backed SearchQuery object. This forces the LLM to provide a specific search_query string and a justification, making it ready for API integration.

Native Tool Calling: In math mode, the LLM is bound to a multiply function. Rather than calculating 12 * 8 itself (where LLMs often fail), it emits a Tool Call with the correct arguments for a Python function to execute.

🛰️ Module 2: Orchestrator-Worker Pattern (LangGraph)
Scaling complexity through parallel execution and synthesis.

🚀 Key Improvements (v2.0):
1. Scalable Planning (The Orchestrator)
The Orchestrator was upgraded to plan 6 distinct sections (up from 4).

It uses a SectionsPlan schema to ensure every worker receives a clear "mission briefing" (Section Name + Description).

2. Parallel "Fan-out" Workers
We implemented a multi-threaded workflow where the following tasks happen simultaneously:

6x Section Writers: Each writing a specific markdown chapter.

Bullets Worker: Generating 5 concise key takeaways.

Glossary Worker: Defining technical terms (4-6 entries).

Title Worker: Crafting a compelling report headline.

References Worker [NEW]: A newly added node that suggests 3-5 relevant book or paper titles to add academic credibility.

3. Quality & Constraint Controls
Length Guard: Implemented a "Hard Truncation" in the section_writer. Any section exceeding 120 words is automatically trimmed to maintain an executive-summary style.

Deduplication: Added logic to clean up the glossary and bullet points, ensuring the LLM doesn't repeat the same information across different nodes.

4. The Synthesizer (The Join Node)
Acts as the final assembly line.

It collects the disparate pieces from the Graph State (Title, Sections, Bullets, Glossary, References) and weaves them into a single, professionally formatted Markdown Report.
