# Transport Layer

* **Stdio:**
  * Standard input and Output
  * Server is started as a subprocess and is a server
  * Client and server can easily read each other using std in and std out
  * Connection will be very fast
  * Secure as both are running on the same system
* **Streamable http:**
  * Based on http protocol
  * Allows host to reach to server running anywhere on the internet
  * Sends a POST/GET call to server using JSON RPC
  * API authentication can be there
  * In the end there will be http://abc.mcp
