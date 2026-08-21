# What is LangChain

* Framework for building agents
* Agent = Model + Harness
* Harness ⇒ Everything around model like system prompt, middleware, tools
* Created using create\_agent (In langchain classic ⇒ `AgentExecutor` and `initialize_agent`  was used)
* Advantage of using framework:
  * **Model standardization:** consistent interface so switching providers doesn't mean rewriting your app
  * **Complex orchestration:** To manage the loop ⇒ dealing with complex workflow, tool calls
* Deep Agents is a harness built on top of LangChain and LangChain is built on top of LangGraph
* For production-readiness — checkpointer, tracing, and guardrails are all separate opt-in pieces you still have to wire up yourself
* If a project genuinely is one prompt in, one answer out, plain SDK calls are often simpler to write and debug
* **LangChain Family:**
*

    <table><thead><tr><th width="132.6666259765625">Product</th><th width="181.99993896484375">What it is</th><th width="390.6666259765625">Best for</th></tr></thead><tbody><tr><td>LangChain</td><td>A configurable harness you assemble yourself via <code>create_agent</code></td><td>Custom agent architectures where you want fine control over every piece</td></tr><tr><td>Deep Agents</td><td>"Batteries-included" — built on top of LangChain via <code>create_deep_agent</code></td><td>Complex autonomous tasks; ships with automatic context compression, a virtual filesystem, and subagent-spawning already wired up</td></tr><tr><td>LangGraph</td><td>The low-level graph engine underneath both of the above</td><td>Fully custom workflows where you need to control execution flow directly</td></tr></tbody></table>
*
