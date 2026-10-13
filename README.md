# Doc Traverse

Doc Traverse is a planned open-source Model Context Protocol (MCP) server for AI coding agents to search and navigate local Markdown documentation. The design pairs relevant source passages with file paths, line ranges, and Markdown heading outlines so an agent can continue reading in the original document.

> **Project status: Design stage.** The MCP server and CLI are not implemented yet.
> The workflows below describe the design. Installation and client configuration will be documented with a runnable prototype.

## Why Doc Traverse?

A coding task can depend on an architecture note, an API specification, or a decision buried in a large documentation folder. You may know the question without knowing the file path or the words used in the docs.

Doc Traverse is designed to help an agent find the relevant passage and understand its place in the documentation. Source references support further reading; heading outlines show nearby sections and other documents worth exploring.

The initial audience is developers and open-source maintainers working with local Markdown documentation across one or more projects.

## Planned workflow

For a question such as **"How are API rate limits handled?"**, the intended workflow is:

1. Ask the question within a selected project's configured documentation roots.
2. Find relevant sections using semantic and lexical search.
3. Return original source passages with file paths, line ranges, and heading outlines.
4. Follow a reference to inspect a specific document, section, or line range.

The proposed MCP tool is `docs_explore`. This is a design example, not a runnable invocation:

```json
{
  "query": "How are API rate limits handled?",
  "projectPath": "/path/to/project"
}
```

The same tool is intended to accept exact document paths, section addresses, and line ranges for follow-up reading. The final tool contract will be documented and tested with the prototype.

## Planned capabilities

These capabilities are design goals for the prototype and have not been implemented or benchmarked.

| Capability | Intended behavior |
| --- | --- |
| Local Markdown search | Combine semantic and lexical retrieval to find relevant documentation sections. |
| Document structure | Return heading outlines for relevant documents, including documents not quoted in the response. |
| Source references | Include original text, file paths, and line ranges so the agent can inspect the source. |
| Project-scoped indexes | Keep a separate local index for each project and read explicitly configured documentation roots. |
| Embedding mode choice | Let users choose local embeddings or OpenRouter explicitly during setup. |
| Index maintenance | Refresh changed files incrementally and check source revisions before returning line references. |

## Installation and client support

Doc Traverse does not have a runnable release or an installable package yet.

The first integration targets are **Codex** and **Claude Code**. Client support has not been verified. Installation commands and MCP configuration examples will be added after those integrations work in the prototype.

The planned implementation language is **Rust**. The first milestone is a runnable MCP server with a documented example from query to source passage and follow-up reading.

Development updates will be tracked in this repository and its [releases](https://github.com/doctraverse/doctraverse/releases).

## How will Doc Traverse handle documentation data?

The design uses local project indexes and an explicit embedding mode choice:

- **Local embeddings:** document chunks and query text are processed by an on-device embedding model.
- **OpenRouter embeddings:** document chunks, query text, and any metadata included in embedding inputs are sent to OpenRouter and the selected provider.

The coding agent has its own model and data-flow settings. Even with local embeddings, a client using a hosted model may send returned source passages to that model's provider.

These are planned data boundaries. Actual models, storage, network behavior, and provider settings will be documented with the first verified implementation.

## FAQ

### What is an MCP server?

A Model Context Protocol (MCP) server exposes tools or context to AI applications through a standard protocol. Doc Traverse is being designed to provide local documentation search and navigation through MCP. See the [official MCP introduction](https://modelcontextprotocol.io/docs/getting-started/intro).

### What documentation formats will Doc Traverse support?

The initial scope is local Markdown documentation. This includes project docs, architecture notes, specifications, and Markdown collections stored outside a source repository. Source-code indexing, hosted documentation scraping, and PDF ingestion are outside the first prototype's scope.

### Will Doc Traverse index files ignored by Git?

The design allows explicitly configured documentation roots even when Git ignores those files. Project configuration will determine which files are indexed. This is a planned capability and has not been tested in a running server.

## Contributing

Feedback on documentation workflows is welcome. [Open an issue](https://github.com/doctraverse/doctraverse/issues) with a question an agent needs to answer, a small public or synthetic Markdown example, and the source references you would expect back.

Implementation contributions will become easier to evaluate once the first runnable prototype and development instructions are available.

## License

Doc Traverse is licensed under the [MIT License](LICENSE).
