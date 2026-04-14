# Code Review Graph (MCP)

The `code-review-graph` MCP server provides structural code intelligence inside Claude. Use it instead of Grep/Glob/Read for codebase exploration — it's faster and gives architectural context that file scanning cannot.

---

## Setup

In `.mcp.json` at the project root:
```json
{
  "mcpServers": {
    "code-review-graph": {
      "command": "uvx",
      "args": ["code-review-graph", "serve"],
      "type": "stdio"
    }
  }
}
```

Also add to `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, `.windsurfrules`, `GEMINI.md` — the same MCP instructions block in all of them.

## Auto-Update Hook

The graph updates automatically on file changes via the post-commit hook (installed via `/graphify hook install`). In Claude Code, a `PostToolUse` hook also triggers updates after Edit/Write/Bash:

```json
// .claude/settings.json
{
  "hooks": {
    "PostToolUse": [{ "matcher": "Edit|Write|Bash", "hooks": [{ "type": "command", "command": "code-review-graph update --quiet" }] }]
  }
}
```

---

## Tool Reference

| Tool | When to use |
|------|------------|
| `semantic_search_nodes` | Find functions/classes by name or keyword — replaces Grep for code symbols |
| `query_graph` | Trace callers, callees, imports, tests, dependencies |
| `get_impact_radius` | Understand blast radius before making a change |
| `detect_changes` | Review staged/recent changes with risk scoring |
| `get_review_context` | Get source snippets for a change — token-efficient alternative to reading files |
| `get_affected_flows` | Find which execution paths a change touches |
| `get_architecture_overview` | High-level codebase structure — start here when new to a project |
| `list_communities` | See all detected module clusters |
| `refactor_tool` | Plan renames, find dead code, identify safe extraction points |

## Query Patterns

```
# Find all callers of a function
query_graph pattern="callers_of" node="fetchGitHubUsage"

# Find what a module imports
query_graph pattern="imports_of" node="worker/services/github.ts"

# Find tests for a module
query_graph pattern="tests_for" node="lib/encryption.ts"

# Find all callees (what a function calls)
query_graph pattern="callees_of" node="pollCycle"
```

## Workflow

1. **Start a session** → `get_architecture_overview` + `list_communities`
2. **Explore a feature** → `semantic_search_nodes` to find relevant nodes
3. **Before changing code** → `get_impact_radius` to understand what breaks
4. **After making a change** → `detect_changes` + `get_affected_flows`
5. **Code review** → `get_review_context` for token-efficient diff reading
6. **Check test coverage** → `query_graph pattern="tests_for"`

## Fallback to File Tools

Use Grep/Glob/Read **only** when:
- Looking for a very specific string not captured as a graph node
- The graph hasn't been built yet on a new project
- The file is config/data (JSON, env, schema SQL) not represented as code nodes
