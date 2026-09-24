# 🟢 Build with Agent Skills

* [<mark style="color:purple;background-color:purple;">**Agent skills**</mark>](https://agentskills.io/home) <mark style="color:purple;background-color:purple;">**are portable instruction sets that give AI coding assistants domain knowledge for a task.**</mark>
* <mark style="color:purple;background-color:purple;">**For MCP development, they encode the design decisions (deployment model, tool patterns, auth) so your agent can interrogate your use case and scaffold a server that fits**</mark>
* <mark style="color:purple;background-color:purple;">**Each skill ships a**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`SKILL.md`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**file plus a**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`references/`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**folder of supporting material (auth flows, tool-design patterns, widget templates, manifest schemas) that the agent reads on demand**</mark>

<mark style="color:purple;background-color:purple;">**Available Skills:**</mark>

*   [<mark style="color:purple;background-color:purple;">**`mcp-server-dev`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**plugin**</mark>](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/mcp-server-dev)<mark style="color:purple;background-color:purple;">**. It provides three composing skills**</mark>

    | Skill                                                            | Purpose                                                                                                                                                                                |
    | ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | <mark style="background-color:purple;">`build-mcp-server`</mark> | <mark style="color:purple;background-color:purple;">**Entry point. Interrogates the use case, picks a deployment model and tool-design pattern, routes to specialized skills.**</mark> |
    | `build-mcp-app`                                                  | <mark style="color:purple;background-color:purple;">Adds interactive UI widgets (forms, pickers, dashboards) rendered inline in chat.</mark>                                           |
    | `build-mcpb`                                                     | <mark style="color:purple;background-color:purple;">Packages a local stdio server with its runtime so users can install it without Node or Python.</mark>                              |
*
