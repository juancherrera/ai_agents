# CrewAI

Sources: [debate README](../3_crew/debate/README.md), [crew definition](../3_crew/debate/src/debate/crew.py), [agents YAML](../3_crew/debate/src/debate/config/agents.yaml), [tasks YAML](../3_crew/debate/src/debate/config/tasks.yaml), [entry point](../3_crew/debate/src/debate/main.py).

## Run the course crew

```powershell
uv tool install crewai
cd 3_crew\debate
crewai install
crewai run
```

Set the provider configuration required by that crew's README in its project environment. These commands can make model calls and write the crew's output files.

## Mental model

| Object | Responsibility |
| --- | --- |
| Agent | Role, goal, backstory, model, and tools |
| Task | Work description, expected output, and assigned agent |
| Crew | Agents, tasks, execution process |
| `Process.sequential` | Execute the configured tasks in sequence |
| YAML config | Store roles and task instructions separately from Python |

## Small script adaptation

Requires CrewAI installed in the environment that runs the script, plus configured provider access. The root project manifest does not declare CrewAI; use a CrewAI project environment.

```python
from crewai import Agent, Task, Crew, Process

teacher = Agent(
    role="Python teacher",
    goal="Explain Python clearly to a beginner",
    backstory="You teach with short, concrete examples.",
    llm="openai/gpt-4o-mini",
)
lesson = Task(
    description="Explain a dictionary with one small example.",
    expected_output="A short explanation and a Python example.",
    agent=teacher,
)
crew = Crew(agents=[teacher], tasks=[lesson], process=Process.sequential)
print(crew.kickoff())
```

The course's decorated class uses `@CrewBase`, `@agent`, `@task`, and `@crew` to assemble agents and tasks from YAML. Its entry point supplies `inputs` to `kickoff`; placeholders in the YAML are filled from those inputs.

Other core projects in `3_crew/` include stock picking, financial research, coding, and engineering teams. Start with the debate project when learning configuration before adding custom tools or hierarchical coordination.
