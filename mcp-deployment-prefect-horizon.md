# MCP Deployment - Prefect/Horizon

* Push only whats needed, dont add anything else in repo
* Make sure repo has main.py, uv.lock
* Push it to git
* Sign it to Horizon ⇒ Connect git ⇒ Build and deploy ⇒ Select repo
* Configure:
  * Server name
  * Entry point
* It also has a chat interface, where we can add our server and test
* To connect this server from outside horizon using claude or something, we will need to add authenticate using horizon and should be part of the org in horizon under which project was created
* Can be used for MCP deployment
