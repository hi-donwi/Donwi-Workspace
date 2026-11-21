# ADR-0028: Publisher branding stays outside the contract

- **Date:** 2026-11-21
- **Status:** Accepted
- **Decider:** Workspace owner

## Context

The framework and its sibling repositories are published under one account and now carry
one brand: the framework is published as DONWI Workspace (repository `Donwi-Workspace`),
next to DONWI Desk, DONWI Brain, DONWI Secure, and DONWI Skills. Before the rename these
repositories were called `Agent-*`, and the framework referred to several of them by
repository name in `ws`, CI, `.agents/skills.manifest`, and documentation.

The framework is also a contract that other organisations adopt (`ws init`, `AGENTS.md`,
`.agents/standards/`, templates, skills). A brand that leaks into that contract would make
an adopter's workspace look like the publisher's product, and every future rename would
become a breaking change for them.

## Decision

The publisher's brand names the products, never the contract. It may appear in a
repository slug, a README title, a badge, a repository description, and where the framework
names a sibling product (help text such as "the desk (DONWI Desk)", the skill source URL,
the reusable security workflow). It does not appear in `AGENTS.md` rules, standards,
templates, skill content, or anything `ws init` writes into an adopter's workspace.

A rename keeps every identifier that someone else may have typed or stored: CLI names
(`ws`, `brain`, `agent-secure`, ...), published package names, project keys in
`registry.tsv`, and contract values such as suite component IDs. Repository slugs may
change, because GitHub redirects git and web URLs; references that GitHub does not
redirect (Actions `uses:`, project Pages URLs) are updated in the same change window. A
repository is never re-created under a name it gave up, since that ends the redirect.

## Consequences

### Positive
- Adopters get an organisation-neutral contract; the brand can change again without
  touching their workspaces.
- CLI and package names stay stable, so hooks written into product repositories and
  install lines keep working.

### Negative
- Product names and identifiers drift apart: DONWI Secure ships the `agent-secure` binary,
  DONWI Desk keeps the `agent-control` project key.
- Historical ADRs and run records keep the old repository names; readers must know the
  mapping.

### Neutral
- Go module paths follow the slug, because a module path is the repository URL. Tags cut
  before the rename keep resolving by the old path through the redirect.

## Alternatives considered

| Option | Why rejected |
|---|---|
| Brand the contract too (DONWI in standards, templates, `ws init` output) | Adopters would inherit the publisher's brand and every future rename would break them. |
| Rename the CLIs and packages with the repositories | `ws` is written into hooks in product repositories, and published package names are typed by users. |
| Keep the `Agent-*` slugs and brand only the README titles | Leaves generic repository names, and "Agent Skills" collides with the open skill format it implements. |

## Follow-up

- [x] Framework references to renamed repositories updated (`ws`, CI, skills manifest and
      lock, README, `.ignore`)
- [ ] Historical ADRs left unchanged on purpose; they record the names that were true then
