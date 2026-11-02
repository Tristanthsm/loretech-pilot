# LoreTech Pilot bootstrap

Task: [fork bootstrap](https://github.com/Tristanthsm/loretech-pilot/issues/1). Owner: Codex, with Tristan as the operator for the later executor connection.

## Outcome and constraints

Create a real Paperclip fork as the third structural repository. Preserve the independent Brain and Cell SDK. Use native executor adapters and plugins before changing core contracts. This delivery is public source and documentation only. No runtime, provider connection, executor registration or public deployment is included.

## Stages

| Stage | Acceptance | State at bootstrap |
| --- | --- | --- |
| Source baseline | Verified fork parent, exact upstream release, preserved MIT license and upstream remote | Prepared on `loretech` |
| Operator handoff | Version-specific Codex defaults, authority boundaries and bounded test protocol documented | Prepared for review |
| Local runtime | Supported Node/pnpm, upstream checks, private access and startup verified | Pending |
| Codex executor | Deliberate authentication, explicit permissions, disabled automatic triggers, bounded manual run | Pending; operator connects later |
| Brain bridge | Real discovery/read, scoped proposal, denial and revocation through the host | Pending |
| Cells | RSSHub and yt-dlp qualified separately with receipts and error cases | Pending |
| BeFood work | Useful sourced pilot research with measured cost and a clear decision | Pending |

## Bootstrap evidence

Check fork metadata, tag/commit equality, base ref, additive diff, unchanged license, JSON provenance and local links. Run the upstream Node policy check. Record candidate, checks and remaining gates in the Issue/PR. Full application tests/build are not evidence required for a documentation-only draft, but remain required before claiming runtime readiness. GitHub Actions are disabled during bootstrap.

## Later changes

Do not implement a second scheduler, credential vault or editable Mission store. First qualify the existing Brain integration surface. If a gap remains, add the smallest plugin/adapter change with counterexample tests: cross-Venture access, revoked access, stale revision, malformed result, duplicate event and timeout after send. Do not enable Activepieces or Nango until a specific workflow or provider requires them.
