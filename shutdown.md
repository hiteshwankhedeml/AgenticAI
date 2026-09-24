# Shutdown

* No JSON RPC is exchanged for this
* **Stdio:**
  * Depends on client
  * Close stdin
  * SIGTERM if it does ⇒ Signal termination
  * SIGKILL ⇒ Last resort&#x20;
  * Server initiated is very rare
  * it closes output stream and exits
* Streamable HTTP:
  * Client ⇒ Just close the connection
  * Server ⇒ Server closes unexpectedly — client should reconnect gracefully
*

    <figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
