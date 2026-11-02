# LoreTech Pilot

This is the Paperclip fork for the LoreTech control plane. GitHub retains the upstream parent and history. The `loretech` base branch pins Paperclip `v2026.1023.0` at `1b4c803816e82b0265c8422e1a804327a7ed8c54`. The upstream `master` branch is preserved. See [source provenance](doc/loretech/upstream.json).

The three structural responsibilities are:

| Repository | Authority |
| --- | --- |
| Cell SDK | Portable capability contracts, validation, packaging and consumption |
| Brain | Ventures, Mission contracts, knowledge, evidence and acceptance decisions |
| LoreTech Pilot | Paperclip companies, agents, operational tasks, attempts and execution controls |

Cells remain independent capability implementations. Existing private integration adapters remain separate. Their source and credentials are not copied here.

## Current delivery

This bootstrap changes documentation only. It does not start a server, create an agent, connect Codex, connect the Brain, or invoke a Cell. The upstream application remains unchanged. GitHub Actions are disabled on this fork during bootstrap; no remote CI result is claimed.

- [Ownership and integration boundaries](doc/loretech/ARCHITECTURE.md)
- [Codex executor handoff](doc/loretech/CODEX-HANDOFF.md)
- [Implementation stages and acceptance](doc/plans/2026-11-02-loretech-pilot-bootstrap.md)
- [Bootstrap task](https://github.com/Tristanthsm/loretech-pilot/issues/1)

Use private, host-owned configuration for deployment, credentials and access bindings. This fork is public. Keep product data and private configuration outside Git.

## Upstream maintenance

Keep `upstream` pointed at `https://github.com/paperclipai/paperclip.git`. Fetch a selected release, review its changes, qualify the executor and integration contracts, then propose an explicit update to the LoreTech branch and provenance file. Do not automatically follow `master` or publish upstream releases from this fork. Preserve the upstream [MIT license](LICENSE).
