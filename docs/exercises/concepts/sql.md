# Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Practice prompt:** What happens when you query a city that is not present?

## Official course and guides

- [4_langgraph/2_lab2.ipynb](../course/week-04-langgraph/2-lab2/walkthrough.md) · [Practice](../course/week-04-langgraph/2-lab2/practice.ipynb)
- [5_autogen/1_lab1_autogen_agentchat.ipynb](../course/week-05-autogen/1-lab1-autogen-agentchat/walkthrough.md) · [Practice](../course/week-05-autogen/1-lab1-autogen-agentchat/practice.ipynb)
- [6_mcp](../course/week-06-mcp/trading-project/walkthrough.md) · [Practice](../course/week-06-mcp/trading-project/practice.ipynb)

## Community variants

- [1_foundations/community_contributions/OptimaChatV3AccessDBOption.ipynb](../community/week-01-foundations/optimachatv3accessdboption-61657b/notebooks/optimachatv3accessdboption-61657b/walkthrough.md) · [Practice](../community/week-01-foundations/optimachatv3accessdboption-61657b/notebooks/optimachatv3accessdboption-61657b/practice.ipynb)
- [4_langgraph/community_contributions/1_basics/02_langgraph_fundamentals.ipynb](../community/week-04-langgraph/1-basics-42f615/notebooks/02-langgraph-fundamentals-09455a/walkthrough.md) · [Practice](../community/week-04-langgraph/1-basics-42f615/notebooks/02-langgraph-fundamentals-09455a/practice.ipynb)
- [4_langgraph/community_contributions/bharat_puri/sidekick.ipynb](../community/week-04-langgraph/bharat-puri-fc7915/notebooks/sidekick-a6935e/walkthrough.md) · [Practice](../community/week-04-langgraph/bharat-puri-fc7915/notebooks/sidekick-a6935e/practice.ipynb)
- [4_langgraph/community_contributions/kisali/2_lab2_tools_memory.ipynb](../community/week-04-langgraph/kisali-32831c/notebooks/2-lab2-tools-memory-10076b/walkthrough.md) · [Practice](../community/week-04-langgraph/kisali-32831c/notebooks/2-lab2-tools-memory-10076b/practice.ipynb)
- [4_langgraph/community_contributions/langgraph_checkpoint_howto/2_lab2 copy_2.ipynb](../community/week-04-langgraph/langgraph-checkpoint-howto-10e1d2/notebooks/2-lab2-copy-2-e7aaef/walkthrough.md) · [Practice](../community/week-04-langgraph/langgraph-checkpoint-howto-10e1d2/notebooks/2-lab2-copy-2-e7aaef/practice.ipynb)
- [5_autogen/community_contributions/kisali/1_lab1_autogen_agentchat.ipynb](../community/week-05-autogen/kisali-1d0e7c/notebooks/1-lab1-autogen-agentchat-4bd7cc/walkthrough.md) · [Practice](../community/week-05-autogen/kisali-1d0e7c/notebooks/1-lab1-autogen-agentchat-4bd7cc/practice.ipynb)
- [5_autogen/community_contributions/ShubhamSavani/1_lab1_autogen_agentchat_gemini.ipynb](../community/week-05-autogen/shubhamsavani-59c284/notebooks/1-lab1-autogen-agentchat-gemini-0372b9/walkthrough.md) · [Practice](../community/week-05-autogen/shubhamsavani-59c284/notebooks/1-lab1-autogen-agentchat-gemini-0372b9/practice.ipynb)
- [1_foundations/community_contributions/week_1_sql_linkedin](../community/week-01-foundations/week-1-sql-linkedin-709a1b/project/walkthrough.md) · [Practice](../community/week-01-foundations/week-1-sql-linkedin-709a1b/project/practice.ipynb)
- [2_openai/community_contributions/Automated_Inventory_Management](../community/week-02-openai-agents/automated-inventory-management-2ff1ee/project/walkthrough.md) · [Practice](../community/week-02-openai-agents/automated-inventory-management-2ff1ee/project/practice.ipynb)
- [2_openai/community_contributions/deep_research_using_google_search](../community/week-02-openai-agents/deep-research-using-google-search-6f7142/project/walkthrough.md) · [Practice](../community/week-02-openai-agents/deep-research-using-google-search-6f7142/project/practice.ipynb)
- [2_openai/community_contributions/kachaje-andela-genai-bootcamp](../community/week-02-openai-agents/kachaje-andela-genai-bootcamp-5b9cce/project/walkthrough.md) · [Practice](../community/week-02-openai-agents/kachaje-andela-genai-bootcamp-5b9cce/project/practice.ipynb)
- [2_openai/community_contributions/workflow_patterns](../community/week-02-openai-agents/workflow-patterns-470b07/project/walkthrough.md) · [Practice](../community/week-02-openai-agents/workflow-patterns-470b07/project/practice.ipynb)
- [3_crew/community_contributions/1_swap_trading_prism](../community/week-03-crewai/1-swap-trading-prism-e8865f/project/walkthrough.md) · [Practice](../community/week-03-crewai/1-swap-trading-prism-e8865f/project/practice.ipynb)
- [4_langgraph/community_contributions/exercise_week4_jom](../community/week-04-langgraph/exercise-week4-jom-6d64a1/project/walkthrough.md) · [Practice](../community/week-04-langgraph/exercise-week4-jom-6d64a1/project/practice.ipynb)
- [4_langgraph/community_contributions/SideKick(Ugraded)](../community/week-04-langgraph/sidekick-ugraded-158da2/project/walkthrough.md) · [Practice](../community/week-04-langgraph/sidekick-ugraded-158da2/project/practice.ipynb)
- [6_mcp/community_contributions/abdoul](../community/week-06-mcp/abdoul-daf78e/project/walkthrough.md) · [Practice](../community/week-06-mcp/abdoul-daf78e/project/practice.ipynb)
- [6_mcp/community_contributions/AutoTrader_hopeogbons](../community/week-06-mcp/autotrader-hopeogbons-fe2d12/project/walkthrough.md) · [Practice](../community/week-06-mcp/autotrader-hopeogbons-fe2d12/project/practice.ipynb)
- [6_mcp/community_contributions/dkisselev-zz](../community/week-06-mcp/dkisselev-zz-3e16f4/project/walkthrough.md) · [Practice](../community/week-06-mcp/dkisselev-zz-3e16f4/project/practice.ipynb)
- [6_mcp/community_contributions/fix_polygon_not_authorized_error](../community/week-06-mcp/fix-polygon-not-authorized-error-77eaae/project/walkthrough.md) · [Practice](../community/week-06-mcp/fix-polygon-not-authorized-error-77eaae/project/practice.ipynb)
- [6_mcp/community_contributions/iamumarjaved](../community/week-06-mcp/iamumarjaved-210bae/project/walkthrough.md) · [Practice](../community/week-06-mcp/iamumarjaved-210bae/project/practice.ipynb)
- [6_mcp/community_contributions/muhammad_qasim_sheikh](../community/week-06-mcp/muhammad-qasim-sheikh-ae5251/project/walkthrough.md) · [Practice](../community/week-06-mcp/muhammad-qasim-sheikh-ae5251/project/practice.ipynb)
- [6_mcp/community_contributions/ranskills_dynamic_db_queries](../community/week-06-mcp/ranskills-dynamic-db-queries-89c006/project/walkthrough.md) · [Practice](../community/week-06-mcp/ranskills-dynamic-db-queries-89c006/project/practice.ipynb)
- [6_mcp/community_contributions/salah](../community/week-06-mcp/salah-60aa4d/project/walkthrough.md) · [Practice](../community/week-06-mcp/salah-60aa4d/project/practice.ipynb)
- [6_mcp/community_contributions/solisoma](../community/week-06-mcp/solisoma-0c963e/project/walkthrough.md) · [Practice](../community/week-06-mcp/solisoma-0c963e/project/practice.ipynb)
- [6_mcp/community_contributions/tochi](../community/week-06-mcp/tochi-396a7d/project/walkthrough.md) · [Practice](../community/week-06-mcp/tochi-396a7d/project/practice.ipynb)
- [6_mcp/community_contributions/trading_with_short](../community/week-06-mcp/trading-with-short-b5d857/project/walkthrough.md) · [Practice](../community/week-06-mcp/trading-with-short-b5d857/project/practice.ipynb)
