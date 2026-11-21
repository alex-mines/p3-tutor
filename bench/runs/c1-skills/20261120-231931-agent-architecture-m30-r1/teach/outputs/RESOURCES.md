# Agent Architecture Resources

## Knowledge

- [Anthropic: Effective context engineering for AI agents](https://anthropic.com/engineering/effective-context-engineering-for-ai-agents)
  Engineering article on curating an agent's context: attention budget, note-taking, just-in-time retrieval. Use for: deciding what the assistant keeps in context.
- [Claude docs: Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)
  Official definition of the context window and how it is counted. Use for: limits, tokens and cost questions.
- [Claude docs: Memory tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)
  File-based memory outside the context window, with the application running the operations. Use for: saving notes between sessions.
- [Anthropic: Context management](https://claude.com/blog/context-management)
  Clearing old tool results and how that complements memory. Use for: keeping long runs affordable.
- [Anthropic: Contextual retrieval](https://anthropic.com/research/contextual-retrieval)
  Chunking, embeddings, keyword search and reported failure-rate gains. Use for: finding the right page in a long manual.

## Wisdom (Communities)

- Local: the team's design review and the course instructor.
  Use for: testing architecture decisions against real questions. No online community has been vetted yet.

## Gaps

- A vetted, moderated online community for agent builders.
