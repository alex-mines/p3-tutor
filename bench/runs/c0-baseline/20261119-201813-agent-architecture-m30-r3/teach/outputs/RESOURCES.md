# Agent architecture Resources

## Knowledge

- [Article: "Effective context engineering for AI agents" (Anthropic Engineering)](https://anthropic.com/engineering/effective-context-engineering-for-ai-agents)
  Context as a limited resource, context rot, just-in-time retrieval, token-efficient tools. Use for: what to keep in context and how to keep it small.
- [Docs: Context windows (Claude Platform)](https://platform.claude.com/docs/en/build-with-claude/context-windows)
  How the window works and current per-model sizes. Use for: checking limits before quoting a number.
- [Docs: Memory tool (Claude Platform)](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)
  Memory as files in a directory, with operations run by your own code. Use for: designing notes between sessions.
- [Article: "Managing context" (Anthropic)](https://www.anthropic.com/news/context-management)
  Context editing and the memory tool, with the reported results. Use for: trimming old tool results.
- [Article: "Contextual Retrieval" (Anthropic)](https://anthropic.com/research/contextual-retrieval)
  Chunking, keyword plus embedding search and reranking, with failure-rate figures. Use for: finding the right page in a long manual.

## Wisdom (Communities)

No community found yet. Until one is vetted, the design review and the sponsor are the real-world test.

## Gaps

- A high-moderation community for agent builders; to search for in a later session.
- Pages were read from search snippets only; open the full pages before quoting figures.
