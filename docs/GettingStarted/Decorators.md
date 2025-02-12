# Decorators
> With only a few lines of code, builders can easily deploy custom AI agents to the Naptha hub or their own local node.

### Context
Naptha supports a web of multi-agent systems that grow and evolve. This walkthrough explains how to decorate functions in order to **quickly publish existing agents, *allowing them to interact with others.***

### Prerequisites

* Python `>= 3.10, <= 3.13`
* Poetry package manager
* Naptha username & password

*See [installation guide](/GettingStarted/Installation) for help.*

### Step-by-step Walkthrough

#### 1. Setup
Configure your `.env` file:
```
HUB_USER=<your_naptha_username>
HUB_PASS=<your_naptha_password>
HUB_URL=ws://node.naptha.ai:3001/rpc

NODE_URL=http://node.naptha.ai:7001
```
*These variables are used to connect to the Naptha network.*

#### 2. Add Naptha SDK
Update your `pyproject.toml` file:
```toml
[tool.poetry.dependencies]
naptha-sdk = {git = "https://github.com/NapthaAI/naptha-sdk"}
```
If not using poetry, you can just add `naptha-sdk` to your `requirements.txt` file, then run:
```bash
pip install -r requirements.txt
```

#### 3. Install dependencies:
Execute via CLI:
```bash
poetry install
```

#### 4. Import Naptha SDK
Put this in your main Python file: `<agent>.py`
```python
from naptha_sdk.client.naptha import agent as naptha_agent
```
*We recommend importing as `naptha_agent` to avoid naming conflicts.*

#### 5. Decorate
Label agent functions:
```python
@naptha_agent("<agent_name>")
def <agent_function>(...):
    # agent logic goes here
    return <agent_output>
```
*Replace `<agent_name>` with a unique identifier for your agent. Keep `<agent_function>` and `<agent_output>` the same, however you named them.*

#### 6. Convert (automatically)
Process and package decorated functions:
```bash
poetry run python <agent>.py
```
*This creates a folder named `agent_pkgs`, which contains your "Napthafied" agent functions. The SDK translates your code into a format compatible with other agents on Naptha.*

#### 7. Test
```bash
cd agent_pkgs/<agent_name>
```
```bash
poetry install
```
```bash
poetry run python <agent_name>/run.py
```
*If you're using an agent framework other than CrewAI, you will likely need to change the inputs dict, where `tool_name` is the name of the agent method that you would like to call, `tool_input_type` is the type (e.g. pydantic schema name) of the input for that call, and `tool_input_value` is a dict of the schema parameters and values.*

#### 8. Publish
Enter this command:
```bash
naptha publish
```
*This command publishes all agents in the `agent_pkgs` folder to the Naptha node specified in your `.env` file.*

Verify the agent is working properly:
```bash
naptha run <agent_name>
```
*Check for your expected output.*

### How does this work?

Let's break it down:

`@naptha_agent("<agent_name>")`  with a unique name for the agent. Below that line, agent functionality can be defined normally using various frameworks.

By running Python code that includes our decorator, agent functions are automatically processed and converted into Naptha-compatible packages.

Later, when you enter `naptha publish` via the CLI, those agent packages will be added to the Naptha node specified in your `.env` file.

## Usage Examples

### [CrewAI](https://github.com/NapthaAI/crewAI-examples)

#### Stock Analysis
View the full code sample [here](https://github.com/NapthaAI/crewAI-examples/blob/main/stock_analysis/src/stock_analysis/crew.py).
```python
@naptha_agent("financial_agent")
    def financial_agent(self) -> Agent:
        return Agent(
            ...
        )
```

### Other Frameworks (Coming Soon)

## Feedback

[Create issues](https://github.com/NapthaAI/naptha-sdk/issues) in the Naptha SDK GitHub repository to let us know any problems, ideas, or questions. We plan to continue adding examples, and your help would be much appreciated!