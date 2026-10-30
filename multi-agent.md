# Multi Agent

* [https://ai-automation-with-mayank.netlify.app/#agents](https://ai-automation-with-mayank.netlify.app/#agents)
* The simplest possible setup is one AI agent with a pile of tools bolted on - it can check email, look at a calendar, search the web, update a spreadsheet, all from one prompt.&#x20;
* That works fine for small tasks. It starts breaking the moment a request touches more than one thing at once.

**Patterns:**

* **Chain in Agents:**
  * Agents run one after another, each one's output feeding directly into the next.&#x20;
  * No agent starts until the one before it finishes.&#x20;
  * Best for jobs that genuinely have to happen in a fixed order
* **Parallel Agents:**
  * Several agents work on independent parts of the same request at the same time, and a final step merges their results together.&#x20;
  * Works when the pieces genuinely don't depend on each other.
* **Controller Agents:**
  * One agent's only job is deciding who should handle a request, then handing it off.&#x20;
  * It doesn't do the work itself, it reads the request and routes it to exactly one specialist
* **Reactive Agents:**
  * A worker agent's output gets checked by a second agent before it goes any further.&#x20;
  * If it's not good enough, it goes back for another pass, not out the door.&#x20;
  * This one extra loop catches mistakes a single unchecked agent would ship straight through.
* **Hierarchical:**
  * One orchestrator agent sits above several specialist agents, and treats each one as a tool it can call, not unlike calling a function.&#x20;
  * It decides which specialists to use, in what order, and combines their work into one final result.
* **Planner Executor:**
  * One agent's whole job is breaking a big, vague goal into a clear list of smaller steps.&#x20;
  * A separate agent then works through that list, one item at a time.&#x20;
  * The planner never executes; the executor never plans.
