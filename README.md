# Product Requirement Document format for HSA/FSA/COBRA Administration System to use as Instructions for CursorAI or CoPilot

This project is a proof-of-concept system demonstrating automated benefits administration using CrewAI technology. At its core, it consists of a floating chat window interface where employees can interact with various AI agents to manage their HSA/FSA claims, COBRA benefits, dependent verification, and wellness program data. The system utilizes mock data stored in Supabase to simulate real-world scenarios while maintaining data privacy and security.

The architecture employs multiple specialized AI agents, including a Manager Agent that orchestrates interactions between other agents handling specific tasks like claim approvals, policy verification, and document processing. The system features both a user-facing chat interface built with Next.js 14 and an administrative portal for managing employee data, claims, and intervention tasks. Key technical components include CrewAI for agent management, Gemini LLM for natural language processing, and LangSmith for monitoring LLM usage, all while incorporating human-in-the-loop capabilities for complex decision-making processes that require human oversight.
