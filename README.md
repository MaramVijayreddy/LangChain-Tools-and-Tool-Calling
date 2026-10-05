# LangChain Tools and Tool Calling

A hands-on learning repository focused on understanding **Tools and Tool Calling in LangChain**.

This repository contains practical implementations starting from basic LangChain tools and progressing toward connecting tools with an LLM using **Gemini** and LangChain's `bind_tools()` functionality.

The notebooks demonstrate how Python functions can be converted into tools, how tools can be structured with schemas, how multiple tools can be grouped, and how an LLM can decide when to call a tool.

---

## 📚 Repository Structure

```text
LangChain-Tools-and-Tool-Calling/
│
├── 01_LangChain_Tools/
│   └── tools_in_langchain.ipynb
│
├── 02_Tool_Calling/
│   └── Tool_Calling_In_Langchain.ipynb
│
└── README.md
```

---

# 01. LangChain Tools

The first notebook focuses on understanding the fundamentals of **LangChain Tools**.

### Topics Covered

- Built-in tools
- DuckDuckGo Search Tool
- Shell Tool
- Custom tools
- `@tool` decorator
- Tool metadata
- Tool arguments
- Pydantic argument schemas
- `StructuredTool`
- `BaseTool`
- Toolkits
- Tool invocation using `.invoke()`

### Built-in Tools

The notebook demonstrates built-in tools such as:

#### DuckDuckGo Search

```python
from langchain_community.tools import DuckDuckGoSearchRun

search_tool = DuckDuckGoSearchRun()

results = search_tool.invoke(
    "top news in india today"
)

print(results)
```

The tool's name, description, and argument schema are also explored.

#### Shell Tool

The notebook demonstrates the use of LangChain's `ShellTool` for executing shell commands.

```python
from langchain_community.tools import ShellTool

shell_tool = ShellTool()

results = shell_tool.invoke("ls")

print(results)
```

> **Note:** Shell tools can execute system commands and should be used carefully.

---

## Custom Tools

The notebook demonstrates how a normal Python function can be converted into a LangChain Tool using the `@tool` decorator.

Example:

```python
from langchain_core.tools import tool

@tool
def multiply(a: int, b: int) -> int:
    """Multiply two numbers"""
    return a * b
```

The notebook explores:

- Tool name
- Tool description
- Tool arguments
- Argument schema
- Tool invocation

---

## StructuredTool

The repository also demonstrates creating tools using `StructuredTool`.

A Pydantic model is used to define the expected inputs.

```python
class MultiplyInput(BaseModel):
    a: int
    b: int
```

The function is then converted into a structured tool using:

```python
StructuredTool.from_function()
```

This demonstrates how tools can have structured and validated inputs.

---

## BaseTool

The notebook also demonstrates creating a custom tool by extending LangChain's `BaseTool` class.

The implementation defines:

- Tool name
- Tool description
- Argument schema
- `_run()` method

This provides a lower-level approach to creating custom LangChain tools.

---

## Toolkits

Multiple tools are grouped together into a toolkit.

The examples include simple mathematical tools such as:

- Addition
- Multiplication

The toolkit returns a collection of tools that can be used together.

---

# 02. Tool Calling

The second notebook builds on the tool concepts from the first notebook and introduces **LLM Tool Calling**.

The notebook uses:

- LangChain
- LangChain Google GenAI
- Gemini
- `ChatGoogleGenerativeAI`
- `bind_tools()`
- `tool_calls`
- `ToolMessage`
- Custom tools

---

## Connecting Tools with Gemini

A Gemini chat model is created using:

```python
from langchain_google_genai import ChatGoogleGenerativeAI

llm = ChatGoogleGenerativeAI(
    model="gemini-2.5-flash"
)
```

The custom tool is then connected to the model:

```python
llm_with_tool = llm.bind_tools([multiply_tool])
```

This allows the model to determine when the tool should be used.

---

## How Tool Calling Works

The basic flow demonstrated in the notebook is:

```text
User Question
      ↓
     LLM
      ↓
Does a tool need to be used?
      ↓
   Tool Call
      ↓
Tool Arguments
      ↓
Execute Tool
      ↓
Tool Result
      ↓
     LLM
      ↓
Final Answer
```

For example:

```text
User:
"What is the answer when we multiply 111 with 122?"

              ↓

Gemini identifies the required tool

              ↓

multiply(a=111, b=122)

              ↓

Tool returns:

13542

              ↓

Gemini generates:

"The product of 111 and 122 is 13542."
```

---

## `bind_tools()`

The notebook demonstrates connecting tools to an LLM using:

```python
llm_with_tool = llm.bind_tools([multiply_tool])
```

The model can then return a tool call containing:

- Tool name
- Arguments
- Tool call ID
- Tool call type

Example structure:

```python
result.tool_calls
```

which contains information similar to:

```text
[
    {
        "name": "multiply",
        "args": {
            "a": 111,
            "b": 122
        },
        "id": "...",
        "type": "tool_call"
    }
]
```

---

## Executing the Tool

The selected tool can then be invoked using the tool call information.

```python
answer = product.invoke(final[0])
```

The tool produces a `ToolMessage`, which is added back to the message history.

The LLM can then use the tool result to generate the final natural-language response.

---

# 🔄 Overall Learning Progression

This repository represents the progression:

```text
Python Functions
       ↓
LangChain Tools
       ↓
Custom Tools
       ↓
StructuredTool
       ↓
BaseTool
       ↓
Toolkits
       ↓
LLM + Tools
       ↓
bind_tools()
       ↓
Tool Calls
       ↓
Execute Tool
       ↓
Tool Result
       ↓
Final LLM Response
```

---

# 🛠️ Technologies Used

- Python
- LangChain
- LangChain Core
- LangChain Community
- Pydantic
- DuckDuckGo Search
- Google Gemini
- LangChain Google GenAI
- Google Colab

---

# 🎯 Learning Objectives

The main objective of this repository is to understand how **LLMs can interact with external functions and tools**.

Through these notebooks, the following concepts are explored:

1. How to create LangChain tools
2. How to define tool inputs
3. How Pydantic schemas are used with tools
4. Different approaches to creating custom tools
5. How multiple tools can be organized
6. How an LLM can be connected to tools
7. How `bind_tools()` works
8. How an LLM generates tool calls
9. How tool calls are executed
10. How tool results are returned to the LLM
11. How the LLM generates the final response

---

# 🚀 What's Next?

This repository forms the foundation for the next stage of learning:

```text
LangChain Tools
       ↓
Tool Calling
       ↓
AI Agents
       ↓
LangGraph
       ↓
Multi-Agent Systems
       ↓
Production AI Applications
```

The next planned project is an **AI Agent using Tool Calling**, where the LLM will be able to select and use multiple tools based on the user's request.

---

## 👨‍💻 Author

**Maram Vijay Reddy**

B.Tech – Computer Science Engineering  
Artificial Intelligence & Machine Learning

GitHub:  
https://github.com/MaramVijayreddy

Portfolio:  
https://maramvijayreddy.vercel.app/
