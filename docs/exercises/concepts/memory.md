# Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Practice prompt:** Does a different thread_id share the same list?

## Official course and guides

- [4_langgraph/2_lab2.ipynb](../course/week-04-langgraph/2-lab2/walkthrough.md) · [Practice](../course/week-04-langgraph/2-lab2/practice.ipynb)
- [4_langgraph/3_lab3.ipynb](../course/week-04-langgraph/3-lab3/walkthrough.md) · [Practice](../course/week-04-langgraph/3-lab3/practice.ipynb)
- [4_langgraph/4_lab4.ipynb](../course/week-04-langgraph/4-lab4/walkthrough.md) · [Practice](../course/week-04-langgraph/4-lab4/practice.ipynb)
- [6_mcp/3_lab3.ipynb](../course/week-06-mcp/3-lab3/walkthrough.md) · [Practice](../course/week-06-mcp/3-lab3/practice.ipynb)
- [3_crew/stock_picker](../course/week-03-crewai/stock-picker/walkthrough.md) · [Practice](../course/week-03-crewai/stock-picker/practice.ipynb)
- [4_langgraph](../course/week-04-langgraph/sidekick-project/walkthrough.md) · [Practice](../course/week-04-langgraph/sidekick-project/practice.ipynb)

## Community variants

- [2_openai/community_contributions/Book Writer with Agents/notebook.ipynb](../community/week-02-openai-agents/book-writer-with-agents-fe4447/notebooks/notebook-37f011/walkthrough.md) · [Practice](../community/week-02-openai-agents/book-writer-with-agents-fe4447/notebooks/notebook-37f011/practice.ipynb)
- [4_langgraph/community_contributions/1_basics/02_langgraph_fundamentals.ipynb](../community/week-04-langgraph/1-basics-42f615/notebooks/02-langgraph-fundamentals-09455a/walkthrough.md) · [Practice](../community/week-04-langgraph/1-basics-42f615/notebooks/02-langgraph-fundamentals-09455a/practice.ipynb)
- [4_langgraph/community_contributions/4_lab4_consent_handling.ipynb](../community/week-04-langgraph/4-lab4-consent-handling-03a837/notebooks/4-lab4-consent-handling-03a837/walkthrough.md) · [Practice](../community/week-04-langgraph/4-lab4-consent-handling-03a837/notebooks/4-lab4-consent-handling-03a837/practice.ipynb)
- [4_langgraph/community_contributions/abdoul/sidekick.ipynb](../community/week-04-langgraph/abdoul-5cd129/notebooks/sidekick-bbe575/walkthrough.md) · [Practice](../community/week-04-langgraph/abdoul-5cd129/notebooks/sidekick-bbe575/practice.ipynb)
- [4_langgraph/community_contributions/kisali/2_lab2_tools_memory.ipynb](../community/week-04-langgraph/kisali-32831c/notebooks/2-lab2-tools-memory-10076b/walkthrough.md) · [Practice](../community/week-04-langgraph/kisali-32831c/notebooks/2-lab2-tools-memory-10076b/practice.ipynb)
- [4_langgraph/community_contributions/kwabena/sidekick_planner_clarify.ipynb](../community/week-04-langgraph/kwabena-9adf19/notebooks/sidekick-planner-clarify-b64e88/walkthrough.md) · [Practice](../community/week-04-langgraph/kwabena-9adf19/notebooks/sidekick-planner-clarify-b64e88/practice.ipynb)
- [4_langgraph/community_contributions/langgraph_checkpoint_howto/2_lab2 copy_2.ipynb](../community/week-04-langgraph/langgraph-checkpoint-howto-10e1d2/notebooks/2-lab2-copy-2-e7aaef/walkthrough.md) · [Practice](../community/week-04-langgraph/langgraph-checkpoint-howto-10e1d2/notebooks/2-lab2-copy-2-e7aaef/practice.ipynb)
- [4_langgraph/community_contributions/solisoma/assesment.ipynb](../community/week-04-langgraph/solisoma-d6b2f7/notebooks/assesment-5f3ba6/walkthrough.md) · [Practice](../community/week-04-langgraph/solisoma-d6b2f7/notebooks/assesment-5f3ba6/practice.ipynb)
- [5_autogen/community_contributions/1_microsoft_agent_framework/09_microsoft_agent_framework.ipynb](../community/week-05-autogen/1-microsoft-agent-framework-a1e5af/notebooks/09-microsoft-agent-framework-30fe02/walkthrough.md) · [Practice](../community/week-05-autogen/1-microsoft-agent-framework-a1e5af/notebooks/09-microsoft-agent-framework-30fe02/practice.ipynb)
- [5_autogen/community_contributions/2_sidekick/10_sidekick.ipynb](../community/week-05-autogen/2-sidekick-6d1eb5/notebooks/10-sidekick-a4a457/walkthrough.md) · [Practice](../community/week-05-autogen/2-sidekick-6d1eb5/notebooks/10-sidekick-a4a457/practice.ipynb)
- [1_foundations/community_contributions/weather-tool](../community/week-01-foundations/weather-tool-057af7/project/walkthrough.md) · [Practice](../community/week-01-foundations/weather-tool-057af7/project/practice.ipynb)
- [2_openai/community_contributions/deep_research_with_opentelemetry](../community/week-02-openai-agents/deep-research-with-opentelemetry-0aa860/project/walkthrough.md) · [Practice](../community/week-02-openai-agents/deep-research-with-opentelemetry-0aa860/project/practice.ipynb)
- [2_openai/community_contributions/dkisselev-zz](../community/week-02-openai-agents/dkisselev-zz-7e6075/project/walkthrough.md) · [Practice](../community/week-02-openai-agents/dkisselev-zz-7e6075/project/practice.ipynb)
- [3_crew/community_contributions/conversational-debate](../community/week-03-crewai/conversational-debate-081921/project/walkthrough.md) · [Practice](../community/week-03-crewai/conversational-debate-081921/project/practice.ipynb)
- [3_crew/community_contributions/crypto_market](../community/week-03-crewai/crypto-market-9c5b43/project/walkthrough.md) · [Practice](../community/week-03-crewai/crypto-market-9c5b43/project/practice.ipynb)
- [3_crew/community_contributions/kisali](../community/week-03-crewai/kisali-e30edb/project/walkthrough.md) · [Practice](../community/week-03-crewai/kisali-e30edb/project/practice.ipynb)
- [3_crew/community_contributions/stock-picker-advance](../community/week-03-crewai/stock-picker-advance-f83dff/project/walkthrough.md) · [Practice](../community/week-03-crewai/stock-picker-advance-f83dff/project/practice.ipynb)
- [3_crew/community_contributions/stock_picker_emads22](../community/week-03-crewai/stock-picker-emads22-5b8b7b/project/walkthrough.md) · [Practice](../community/week-03-crewai/stock-picker-emads22-5b8b7b/project/practice.ipynb)
- [4_langgraph/community_contributions/basic_page_language_detector](../community/week-04-langgraph/basic-page-language-detector-8b6ad1/project/walkthrough.md) · [Practice](../community/week-04-langgraph/basic-page-language-detector-8b6ad1/project/practice.ipynb)
- [4_langgraph/community_contributions/dkisselev-zz](../community/week-04-langgraph/dkisselev-zz-8ea9bc/project/walkthrough.md) · [Practice](../community/week-04-langgraph/dkisselev-zz-8ea9bc/project/practice.ipynb)
- [4_langgraph/community_contributions/emmy_sidekick](../community/week-04-langgraph/emmy-sidekick-850b86/project/walkthrough.md) · [Practice](../community/week-04-langgraph/emmy-sidekick-850b86/project/practice.ipynb)
- [4_langgraph/community_contributions/exercise_week4_jom](../community/week-04-langgraph/exercise-week4-jom-6d64a1/project/walkthrough.md) · [Practice](../community/week-04-langgraph/exercise-week4-jom-6d64a1/project/practice.ipynb)
- [4_langgraph/community_contributions/iamumarjaved](../community/week-04-langgraph/iamumarjaved-ef973b/project/walkthrough.md) · [Practice](../community/week-04-langgraph/iamumarjaved-ef973b/project/practice.ipynb)
- [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp](../community/week-04-langgraph/kachaje-andela-genai-bootcamp-52f840/project/walkthrough.md) · [Practice](../community/week-04-langgraph/kachaje-andela-genai-bootcamp-52f840/project/practice.ipynb)
- [4_langgraph/community_contributions/langgraph_checkpoint_howto](../community/week-04-langgraph/langgraph-checkpoint-howto-10e1d2/project/walkthrough.md) · [Practice](../community/week-04-langgraph/langgraph-checkpoint-howto-10e1d2/project/practice.ipynb)
- [4_langgraph/community_contributions/muhammad_qasim_sheikh](../community/week-04-langgraph/muhammad-qasim-sheikh-733f7d/project/walkthrough.md) · [Practice](../community/week-04-langgraph/muhammad-qasim-sheikh-733f7d/project/practice.ipynb)
- [4_langgraph/community_contributions/philip](../community/week-04-langgraph/philip-1b529d/project/walkthrough.md) · [Practice](../community/week-04-langgraph/philip-1b529d/project/practice.ipynb)
- [4_langgraph/community_contributions/ranskills-toolbox](../community/week-04-langgraph/ranskills-toolbox-7f4500/project/walkthrough.md) · [Practice](../community/week-04-langgraph/ranskills-toolbox-7f4500/project/practice.ipynb)
- [4_langgraph/community_contributions/rnik12](../community/week-04-langgraph/rnik12-2a1b28/project/walkthrough.md) · [Practice](../community/week-04-langgraph/rnik12-2a1b28/project/practice.ipynb)
- [4_langgraph/community_contributions/sach91-docker-ollama-telegram](../community/week-04-langgraph/sach91-docker-ollama-telegram-382147/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sach91-docker-ollama-telegram-382147/project/practice.ipynb)
- [4_langgraph/community_contributions/salah](../community/week-04-langgraph/salah-3f9699/project/walkthrough.md) · [Practice](../community/week-04-langgraph/salah-3f9699/project/practice.ipynb)
- [4_langgraph/community_contributions/SideKick(Ugraded)](../community/week-04-langgraph/sidekick-ugraded-158da2/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-ugraded-158da2/project/practice.ipynb)
- [4_langgraph/community_contributions/sidekick_hf_docker_chromium_deploy](../community/week-04-langgraph/sidekick-hf-docker-chromium-deploy-7cc833/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-hf-docker-chromium-deploy-7cc833/project/practice.ipynb)
- [4_langgraph/community_contributions/Sidekick_hopeogbons](../community/week-04-langgraph/sidekick-hopeogbons-a329e2/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-hopeogbons-a329e2/project/practice.ipynb)
- [4_langgraph/community_contributions/sidekick_juan](../community/week-04-langgraph/sidekick-juan-5a235e/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-juan-5a235e/project/practice.ipynb)
- [4_langgraph/community_contributions/sidekick_proj](../community/week-04-langgraph/sidekick-proj-d7ebd8/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-proj-d7ebd8/project/practice.ipynb)
- [4_langgraph/community_contributions/sidekick_research_and_action](../community/week-04-langgraph/sidekick-research-and-action-80bfaf/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-research-and-action-80bfaf/project/practice.ipynb)
- [4_langgraph/community_contributions/sidekick_with_preamble](../community/week-04-langgraph/sidekick-with-preamble-ec1a56/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-with-preamble-ec1a56/project/practice.ipynb)
- [4_langgraph/community_contributions/week4_exercise_agentic.py](../community/week-04-langgraph/week4-exercise-agentic-740a85/project/walkthrough.md) · [Practice](../community/week-04-langgraph/week4-exercise-agentic-740a85/project/practice.ipynb)
- [6_mcp/community_contributions/dkisselev-zz](../community/week-06-mcp/dkisselev-zz-3e16f4/project/walkthrough.md) · [Practice](../community/week-06-mcp/dkisselev-zz-3e16f4/project/practice.ipynb)
