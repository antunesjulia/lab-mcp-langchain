# MCP Lab Proof

## 1) MCP server connected

I connected to the local filesystem MCP server using the Model Context Protocol filesystem server:

- Command: `npx -y @modelcontextprotocol/server-filesystem`
- Allowed directory: a project-local documents folder under the workspace
- Integration: `MultiServerMCPClient` from `langchain_mcp_adapters`

This setup was verified in the notebook by printing the allowed directory and confirming the client was configured successfully.

## 2) Tools loaded

The MCP server exposed the following tools after loading into LangChain:

- `read_file`
- `read_text_file`
- `read_media_file`
- `read_multiple_files`
- `write_file`
- `edit_file`
- `create_directory`
- `list_directory`
- `list_directory_with_sizes`
- `directory_tree`
- `move_file`
- `search_files`
- `get_file_info`
- `list_allowed_directories`

These were loaded through `await mcp_client.get_tools()` and printed directly from the notebook output.

## 3) Query tested

I tested the filesystem MCP by asking it to expose and list the available tools for the connected workspace. The notebook query was effectively:

- "List the MCP tools available from the connected filesystem server."

This is grounded in the notebook code:

```python
tools = await mcp_client.get_tools()

print("Available MCP tools:")
for tool in tools:
    print(f"- {tool.name}: {tool.description}")
```

## 4) Tool result that supported the final answer

The supporting result came from the tool output showing the server successfully loaded filesystem tools, including `list_directory` and `read_text_file`.

Example result from the notebook:

```text
Available MCP tools:
- read_file: Read the complete contents of a file as text. DEPRECATED: Use read_text_file instead.
- read_text_file: Read the complete contents of a file from the file system as text...
- list_directory: Get a detailed listing of all files and directories in a specified path...
- search_files: Recursively search for files and directories matching a pattern...
```

This result confirms the MCP client connected correctly and that the filesystem server was usable from LangChain.

## 5) Limitation or failure case

One clear limitation was environment dependence: the notebook initially failed because `npx` and Node.js were not available in the Python environment. The troubleshooting output showed:

```text
node: None
npm: None
npx: None
```

After installing Node.js via conda, the tools became available and the MCP server could run. This demonstrates that the workflow depends on the local runtime environment and that missing Node tooling is a real failure mode for filesystem-based MCP servers.
