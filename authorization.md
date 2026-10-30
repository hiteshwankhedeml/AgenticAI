# Authorization

http ⇒ OAuth 2.0

STDIO ⇒ use environment-based credentials or credentials provided by third-party libraries embedded directly in the MCP server



**Steps:**

1. **Initial Handshake:**

* Server responds with a `401 Unauthorized` and tells the client where to find authorization information, captured in a [Protected Resource Metadata (PRM) document](https://datatracker.ietf.org/doc/html/rfc9728). This will be URI

2. **Protected Resource Metadata Discovery:**

* With the URI pointer to the PRM document, the client will fetch the metadata to learn about the authorization serve

3. <sup>**Authorization Server Discovery:**</sup>

* the client will then construct a standard metadata URI and issue a request to the [OpenID Connect (OIDC) Discovery](https://openid.net/specs/openid-connect-discovery-1_0.html) or [OAuth 2.0 Auth Server Metadata](https://datatracker.ietf.org/doc/html/rfc8414) endpoints

4. **Client Registration:**

* The client now needs to make sure that it’s registered with the authorization server
* The client can be **pre-registered** with a given authorization server, in which case it can have embedded client registration information that it uses to complete the authorization flow.
* The client can use Dynamic Client Registration (DCR) to dynamically register itself with the authorization server.

5. **User Authorization:**

* The client will now need to open a browser to the `/authorize` endpoint, where the user can log in and grant the required permissions
* The access token is what the client will use to authenticate requests to the MCP server.

6. **Making Authenticated Requests:**

* The client can make requests to your MCP server using the access token embedded in the `Authorization` header





