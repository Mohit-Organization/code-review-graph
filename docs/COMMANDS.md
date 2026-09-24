# All Available Commands

## Skills (Claude Code slash commands)

### `/code-review-graph:build-graph`
Build or update the knowledge graph.
- First time: performs a full build
- Subsequent: incremental update (only changed files)

### `/code-review-graph:review-delta`
Review only changes since last commit.
- Auto-detects changed files via git diff
- Computes blast radius (2-hop default)
- Generates structured review with guidance

### `/code-review-graph:review-pr`
Review a PR or branch diff.
- Uses main/master as base
- Full impact analysis across all PR commits
- Structured output with risk assessment

## MCP Tools (30 total)

Signatures below match the registrations in `code_review_graph/main.py`.
`repo_root=None` uses the server's repository resolution.

### `build_or_update_graph_tool`

Build or incrementally update the code knowledge graph.

```python
build_or_update_graph_tool(
    full_rebuild: bool = False,
    repo_root: Optional[str] = None,
    base: str = 'HEAD~1',
    postprocess: str = 'full',
    recurse_submodules: Optional[bool] = None,
)
```

### `run_postprocess_tool`

Run post-processing on existing graph (flows, communities, FTS index).

```python
run_postprocess_tool(
    flows: bool = True,
    communities: bool = True,
    fts: bool = True,
    repo_root: Optional[str] = None,
)
```

### `get_minimal_context_tool`

Get ultra-compact context for any task (~100 tokens). Always call this first.

```python
get_minimal_context_tool(
    task: str = '',
    changed_files: Optional[list[str]] = None,
    repo_root: Optional[str] = None,
    base: str = 'HEAD~1',
)
```

### `get_impact_radius_tool`

Analyze the blast radius of changed files in the codebase.

```python
get_impact_radius_tool(
    changed_files: Optional[list[str]] = None,
    max_depth: int = 2,
    repo_root: Optional[str] = None,
    base: str = 'HEAD~1',
    detail_level: str = 'standard',
)
```

### `query_graph_tool`

Run a predefined graph query to explore code relationships.

```python
query_graph_tool(
    pattern: str,
    target: str,
    repo_root: Optional[str] = None,
    detail_level: str = 'standard',
)
```

### `get_review_context_tool`

Generate a focused, token-efficient review context for code changes.

```python
get_review_context_tool(
    changed_files: Optional[list[str]] = None,
    max_depth: int = 2,
    include_source: bool = True,
    max_lines_per_file: int = 200,
    repo_root: Optional[str] = None,
    base: str = 'HEAD~1',
    detail_level: str = 'standard',
)
```

### `semantic_search_nodes_tool`

Search for code entities by name, keyword, or semantic similarity.

```python
semantic_search_nodes_tool(
    query: str,
    kind: Optional[str] = None,
    limit: int = 20,
    repo_root: Optional[str] = None,
    model: Optional[str] = None,
    provider: Optional[str] = None,
    detail_level: str = 'standard',
)
```

### `embed_graph_tool`

Compute vector embeddings for all graph nodes to enable semantic search.

```python
embed_graph_tool(
    repo_root: Optional[str] = None,
    model: Optional[str] = None,
    provider: Optional[str] = None,
)
```

### `list_graph_stats_tool`

Get aggregate statistics about the code knowledge graph.

```python
list_graph_stats_tool(
    repo_root: Optional[str] = None,
)
```

### `get_docs_section_tool`

Get a specific section from the LLM-optimized documentation reference.

```python
get_docs_section_tool(
    section_name: str,
    repo_root: Optional[str] = None,
)
```

### `find_large_functions_tool`

Find functions, classes, or files exceeding a line-count threshold.

```python
find_large_functions_tool(
    min_lines: int = 50,
    kind: Optional[str] = None,
    file_path_pattern: Optional[str] = None,
    limit: int = 50,
    repo_root: Optional[str] = None,
)
```

### `list_flows_tool`

List execution flows in the codebase, sorted by criticality.

```python
list_flows_tool(
    sort_by: str = 'criticality',
    limit: int = 50,
    kind: Optional[str] = None,
    detail_level: str = 'standard',
    repo_root: Optional[str] = None,
)
```

### `get_flow_tool`

Get detailed information about a single execution flow.

```python
get_flow_tool(
    flow_id: Optional[int] = None,
    flow_name: Optional[str] = None,
    include_source: bool = False,
    repo_root: Optional[str] = None,
)
```

### `get_affected_flows_tool`

Find execution flows affected by changed files.

```python
get_affected_flows_tool(
    changed_files: Optional[list[str]] = None,
    base: str = 'HEAD~1',
    repo_root: Optional[str] = None,
)
```

### `list_communities_tool`

List detected code communities in the codebase.

```python
list_communities_tool(
    sort_by: str = 'size',
    min_size: int = 0,
    detail_level: str = 'standard',
    repo_root: Optional[str] = None,
)
```

### `get_community_tool`

Get detailed information about a single code community.

```python
get_community_tool(
    community_name: Optional[str] = None,
    community_id: Optional[int] = None,
    include_members: bool = False,
    repo_root: Optional[str] = None,
)
```

### `get_architecture_overview_tool`

Generate an architecture overview based on community structure.

```python
get_architecture_overview_tool(
    repo_root: Optional[str] = None,
)
```

### `detect_changes_tool`

Detect changes and produce risk-scored, priority-ordered review guidance.

```python
detect_changes_tool(
    base: str = 'HEAD~1',
    changed_files: Optional[list[str]] = None,
    include_source: bool = False,
    max_depth: int = 2,
    repo_root: Optional[str] = None,
    detail_level: str = 'standard',
)
```

### `refactor_tool`

Graph-powered refactoring operations.

```python
refactor_tool(
    mode: str = 'rename',
    old_name: Optional[str] = None,
    new_name: Optional[str] = None,
    kind: Optional[str] = None,
    file_pattern: Optional[str] = None,
    repo_root: Optional[str] = None,
)
```

### `apply_refactor_tool`

Apply a previously previewed refactoring to source files.

```python
apply_refactor_tool(
    refactor_id: str,
    repo_root: Optional[str] = None,
    dry_run: bool = False,
)
```

### `generate_wiki_tool`

Generate a markdown wiki from the code community structure.

```python
generate_wiki_tool(
    repo_root: Optional[str] = None,
    force: bool = False,
)
```

### `get_wiki_page_tool`

Retrieve a specific wiki page by community name.

```python
get_wiki_page_tool(
    community_name: str,
    repo_root: Optional[str] = None,
)
```

### `get_hub_nodes_tool`

Find the most connected nodes in the codebase (architectural hotspots).

```python
get_hub_nodes_tool(
    top_n: int = 10,
    repo_root: Optional[str] = None,
)
```

### `get_bridge_nodes_tool`

Find architectural chokepoints via betweenness centrality.

```python
get_bridge_nodes_tool(
    top_n: int = 10,
    repo_root: Optional[str] = None,
)
```

### `get_knowledge_gaps_tool`

Identify structural weaknesses in the codebase graph.

```python
get_knowledge_gaps_tool(
    repo_root: Optional[str] = None,
)
```

### `get_surprising_connections_tool`

Find unexpected architectural coupling via composite surprise scoring.

```python
get_surprising_connections_tool(
    top_n: int = 15,
    repo_root: Optional[str] = None,
)
```

### `get_suggested_questions_tool`

Auto-generate review questions from graph analysis.

```python
get_suggested_questions_tool(
    repo_root: Optional[str] = None,
)
```

### `traverse_graph_tool`

BFS/DFS traversal from best-matching node with token budget.

```python
traverse_graph_tool(
    query: str,
    mode: str = 'bfs',
    depth: int = 3,
    token_budget: int = 2000,
    repo_root: Optional[str] = None,
)
```

### `list_repos_tool`

List all registered repositories in the multi-repo registry.

```python
list_repos_tool(
)
```

### `cross_repo_search_tool`

Search for code entities across all registered repositories.

```python
cross_repo_search_tool(
    query: str,
    kind: Optional[str] = None,
    limit: int = 20,
)
```

## MCP Prompts (5 workflow templates)

### `review_changes`
Pre-commit review workflow using detect_changes, affected_flows, and test gaps.
```
base: str = "HEAD~1"
```

### `architecture_map`
Architecture documentation using communities, flows, and Mermaid diagrams.

### `debug_issue`
Guided debugging using search, flow tracing, and recent changes.
```
description: str = ""
```

### `onboard_developer`
New developer orientation using stats, architecture, and critical flows.

### `pre_merge_check`
PR readiness check with risk scoring, test gaps, and dead code detection.
```
base: str = "HEAD~1"
```

## CLI Commands

```bash
# Setup
code-review-graph install           # Register MCP server with Claude Code (alias: init)
code-review-graph install --dry-run # Preview without writing files

# Build and update
code-review-graph build                        # Full build
code-review-graph update                       # Incremental update
code-review-graph update --base origin/main    # Custom base ref

# Deferred post-processing
code-review-graph postprocess                 # Compute flows, communities, and FTS

# Monitor and inspect
code-review-graph status                       # Graph statistics
code-review-graph watch                        # Auto-update on file changes
code-review-graph visualize                    # Generate interactive HTML graph

# Analysis
code-review-graph detect-changes               # Risk-scored change analysis
code-review-graph detect-changes --base HEAD~3 # Custom base ref
code-review-graph detect-changes --brief       # Compact output

# Wiki
code-review-graph wiki                         # Generate markdown wiki from communities

# Multi-repo
code-review-graph register <path> [--alias name]  # Register a repository
code-review-graph unregister <path_or_alias>       # Remove from registry
code-review-graph repos                            # List registered repositories

# Daemon (multi-repo watcher) — included with install, no extra dependencies
code-review-graph daemon start [--foreground]       # Start the watch daemon
code-review-graph daemon stop                       # Stop the daemon
code-review-graph daemon restart [--foreground]     # Restart the daemon
code-review-graph daemon status                     # Show daemon status and repos
code-review-graph daemon logs [--repo ALIAS] [-f]   # View daemon or per-repo logs
code-review-graph daemon add <path> [--alias NAME]  # Add a repo to daemon config
code-review-graph daemon remove <path_or_alias>     # Remove a repo from daemon config

# Evaluation (install code-review-graph[eval] first)
code-review-graph eval --all                   # Run all evaluation benchmarks
code-review-graph eval --report                # Generate report from results

# Server
code-review-graph serve                        # Start MCP server (stdio)
```

## Standalone Daemon CLI (`crg-daemon`)

The `crg-daemon` command is included with every `code-review-graph` installation — no
separate install required. It is also available as a standalone entry point. It mirrors the
`code-review-graph daemon` subcommands:

```bash
crg-daemon start [--foreground]       # Start the multi-repo watch daemon
crg-daemon stop                       # Stop the daemon and all watcher processes
crg-daemon restart [--foreground]     # Restart (stop + start)
crg-daemon status                     # Show daemon status, repos, and process liveness
crg-daemon logs [--repo ALIAS] [-f] [-n N]  # Tail daemon or per-repo log files
crg-daemon add <path> [--alias NAME]  # Add a repository to watch.toml
crg-daemon remove <path_or_alias>     # Remove a repository from watch.toml
```

### Configuration

The daemon reads its configuration from `~/.code-review-graph/watch.toml`:

```toml
session_name = "crg-watch"   # logical daemon name
log_dir = "~/.code-review-graph/logs"
poll_interval = 2            # seconds between config file polls

[[repos]]
path = "/home/user/project-a"
alias = "project-a"

[[repos]]
path = "/home/user/project-b"
alias = "project-b"
```

The daemon spawns one `code-review-graph watch` child process per repo,
managed via `subprocess.Popen`. It monitors the config file for changes and
automatically reconciles child processes (starting/stopping as repos are
added or removed). Health checks run every 30 seconds and automatically
restart dead watchers. No external dependencies (tmux, screen, etc.) are
required.
