# 🟢 Streamable Http

* &#x20;<mark style="color:purple;background-color:purple;">**Similar to stdio, but in this we need to specify streamable, host and port**</mark>

```python
if __name__ == "__main__":
    # Run the MCP server using HTTP transport
    mcp.run(transport="streamable-http",host="0.0.0.0",port=8050)
```

