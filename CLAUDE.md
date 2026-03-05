# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

Resources and skills from the [Manifold AI event](https://luma.com/q3o2kz2n). Contains Claude Code slash commands and agent skills covered during the presentations — no application source code. Working demo: [fundrasing-seven.vercel.app](https://fundrasing-seven.vercel.app/). There is no build system, package.json, or tests.

## Structure

```
commands/           # Slash commands for Claude Code
  contex-gather.md  # /contex-gather — conversational interview to populate CLAUDE.md with project context
  project-spec.md   # /project-spec — takes a feature from CLAUDE.md and creates a detailed spec in specs/

.agents/skills/     # Agent skills (loaded automatically by Claude Code)
  manifold-nft-minting/  # Guides building NFT minting with @manifoldxyz/client-sdk
  frontend-design/       # Creates distinctive, production-grade frontend UIs
  implement-design/      # Translates Figma designs to code via Figma MCP server
```

## Workflow

The intended workflow for new projects using this repo:

1. **Gather context** — Run `/contex-gather` to interview the user and generate project context in this CLAUDE.md file (it will be merged/replaced with project-specific content)
2. **Spec features** — Run `/project-spec` to turn individual features/milestones into detailed specs saved to `specs/{feature-name}.md`
3. **Build** — Use the specs + skills to implement. Skills like `manifold-nft-minting`, `frontend-design`, and `implement-design` activate automatically based on task context

## Key Conventions

- The `/contex-gather` command is intentionally misspelled (missing the "t" in "context") — use it as-is
- Specs are written to `specs/` directory with kebab-case filenames
- Skills in `.agents/skills/` have reference docs loaded lazily — only when needed for the current step
- The `manifold-nft-minting` skill requires users to have a Manifold campaign with an instance ID before SDK integration begins
- When using RainbowKit, always pin `wagmi@^2.9.0` — never install without the version pin
- Prefer viem over ethers for new projects unless the user has an existing ethers codebase
