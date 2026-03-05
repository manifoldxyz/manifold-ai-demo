# Manifold AI Demo

Resources and skills from the [Manifold AI event](https://luma.com/q3o2kz2n). See the working demo at [fundrasing-seven.vercel.app](https://fundrasing-seven.vercel.app/).

## What's Inside

This repo contains Claude Code slash commands and agent skills covered during the presentations:

### Slash Commands (`commands/`)

| Command | Description |
|---|---|
| `/contex-gather` | Conversational interview that captures your project vision, goals, tech stack, and approach into a `CLAUDE.md` file |
| `/project-spec` | Takes a feature from `CLAUDE.md` and produces a detailed, engineering-ready spec in `specs/` |

### Agent Skills (`.agents/skills/`)

| Skill | Description |
|---|---|
| `manifold-nft-minting` | Build NFT minting experiences with Manifold's `@manifoldxyz/client-sdk` — supports Edition and Blind Mint products across Ethereum, Base, Optimism, Shape, Sepolia, and ApeChain |
| `frontend-design` | Create distinctive, production-grade frontend interfaces with high design quality |
| `implement-design` | Translate Figma designs into production-ready code with 1:1 visual fidelity via the Figma MCP server |

## Getting Started

1. Clone this repo
2. Open it in [Claude Code](https://claude.ai/code)
3. Run `/contex-gather` to set up your project context
4. Run `/project-spec` to spec out individual features
5. Build using the skills — they activate automatically based on what you're working on

## Installing Skills in Other Projects

You can add individual skills to any project:

```bash
npx skills add https://github.com/manifoldxyz/manifold-ai --skill manifold-nft-minting
npx skills add https://github.com/manifoldxyz/manifold-ai --skill frontend-design
npx skills add https://github.com/manifoldxyz/manifold-ai --skill implement-design
```
