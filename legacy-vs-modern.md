# Legacy vs Modern

* Nov 2025 \_ legacy specs
* July 2026 - Modern specs
* **Legacy: Remember me:**
  * You shake hands once. After that, the server remembers who you are for the whole conversation.
  * Old `Mcp-Session-Id`. Whichever server first "picked up the call" is the only one that remembers your context — which makes it hard to just add more servers behind the scenes to handle busier days.
  * Don't exist: the old handshake, `ping`, `logging/setLevel`, the old session header.
  * Roots, Sampling, Logging, the old HTTP+SSE transport — these keep working until at least July 2027
* **Modern: Tell me every time:**
  * No handshake. Every single message carries everything about itself — who's asking, what version, what's needed.
  * There's no session id at all — the full details travel with every single request, so any server can handle it, not just the one that "answered the call" first.
  * Slightly more information travels with every request.&#x20;
  * But for most real systems, that small extra cost is worth it — being able to add more servers freely, without anyone needing to "remember" you, turns out to matter a lot more once things get busy
* MCP is stateless in modern version
* Now the request can be served by any server
* If nothing is "remembered" anymore, how does a server ever ask you a question in the middle of doing something? ⇒ a server can only ask you something while it's already working on a request you sent it
* Every request now includes an `Mcp-Method` and `Mcp-Name` label, right in the headers.&#x20;
* Anything managing traffic between you and a server — a load balancer, for instance — can glance at just those two labels and route the request instantly, without needing to open up and inspect the whole thing.
* A server can now tell a client exactly how long to hold onto an answer before asking again — and whether that answer is safe to share.
* The new fields are `ttlMs` (how long an answer stays fresh, in milliseconds) and `cacheScope` (whether that cached answer is safe to share across many people, or private to just you)
