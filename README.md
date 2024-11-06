# Tutorial-for-a-Math-Agent
Demonstrates hwo to use langgraph and wolframalpha to create two types of Math Agents

The first graph demonstrates a simple Math Agent with one node and conditional edge.


The second graph has two nodes. The first node creates a Math problem and is not bound to the math tool. It tries to solve the problem nevertheless.The second node is bound to the math tool and also solves the problem. 

Exercise: Change the graph such that the first node only creates the problem without trying to solve it. The second node then solves the problem with the math tool. Pay attention how state["messages"] is filled and passed. 

# Interactive Graph-based Chatbot Framework

This repository demonstrates a dynamic chatbot framework using a graph-based approach to manage state and tool interactions, where each step of the chatbot's response process is controlled by nodes in a state graph. This framework can be useful for building chatbots that need conditional logic, tool integration (like mathematical computations or external API calls), and modular, flexible interaction paths.

## Overview

The chatbot is implemented with a state graph that processes user queries and determines whether additional tool invocations or chatbot responses are required. It leverages Python's `TypedDict` to structure the state and conditionally routes queries through different nodes.

## Key Components

### `State` Class

The `State` class uses Python's `TypedDict` to store a list of messages that drive the conversation flow.

```python
class State(TypedDict):
    messages: Annotated[list, add_messages]
```

### Nodes and Workflow

1. **Primary Node (`node1`)**: The entry point where the chatbot processes the input query.
   - It uses a model (`model_with_tool.invoke`) to interpret the messages and determine any required actions or responses.
   - This node passes the processed messages back as an updated `state`.

2. **Tool Node (`tool_node`)**: Represents an external tool node, defined as `wolfram_node` here, which might handle specialized computations.

3. **Conditional Logic**: A `should_continue` function decides if the chatbot should call more tools or terminate based on whether there are any pending tool invocations in the last message.

4. **Graph Initialization**: The `init_graph` function sets up the graph with conditional edges and entry points:
   - Adds nodes and links them with conditional edges based on the `should_continue` function.
   - Loops back to `node1` whenever a tool is invoked to allow further processing.

5. **Execution**: The `run_graph` function initiates the graph processing with a given query, handling it through the conditional nodes until a terminal state is reached.

### Sample Usage

The graph is initialized and run with a sample query:

```python
graph = init_graph()
state = State()
query = "What is 2^3.45 plus 1. Then take the third root. Use this as the upper limit of the integral of log(x) with lower limit 1"
state["messages"] = query

state = run_graph(state, graph)
```

### Visualization

The generated state graph can be visualized with `mermaid`, enabling clear insight into the chatbot's decision-making flow and process paths.

## Installation and Requirements

This setup requires:
- `TypedDict` and `Annotated` from Python's typing library.
- Additional modules for handling state graphs and memory persistence.

To run the code, install the necessary packages and dependencies using `pip` or by including them in a `requirements.txt` file.

## Summary

This framework provides a modular, state-based approach for chatbot development, with a focus on decision-making, tool integration, and flexible state management through a graph-based flow. It can be extended for various applications requiring conditional logic and complex tool interactions.
