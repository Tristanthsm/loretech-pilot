# Connect Codex after source bootstrap

This handoff describes the pinned Paperclip release. No executor is configured by this change. Do not assume the Codex desktop session is the identity or login used by a Paperclip worker.

## 1. Prepare an isolated local runtime

The pinned source requires Node `>=24.11.0`; `.nvmrc` selects Node 24. The package manager is `pnpm@9.15.4`. Follow [DEVELOPING.md](../DEVELOPING.md) for the current checkout. The native development entry is `pnpm install`, then `pnpm dev`; local development can use embedded PGlite. Dependency installation and runtime startup remain a later operation.

Before startup, choose a dedicated runtime directory, private database, listener and supported authenticated access mode. Keep the first listener local/private. Do not import a production database or host credential directory. Review the startup environment. Upstream telemetry is enabled by default unless CI, `PAPERCLIP_TELEMETRY_DISABLED=1`, `DO_NOT_TRACK=1`, or instance configuration disables it, as defined in [telemetry/config.ts](../../packages/shared/src/telemetry/config.ts). This does not disable unrelated OpenTelemetry or provider logging.

GitHub Actions are currently disabled on this fork. Inspect upstream workflows, credentials and release targets before enabling them. Run the upstream application gates before treating a runtime candidate as ready: `pnpm -r typecheck`, `pnpm test:run`, and `pnpm build`.

## 2. Set explicit executor permissions

Use the native `codex_local` adapter. Codex must be available to the Paperclip execution host. Consult both the [adapter guide](../../docs/adapters/codex-local.md) and the pinned [configuration source](../../packages/adapters/codex-local/src/index.ts). The source takes priority where they differ.

Critical default: `DEFAULT_CODEX_LOCAL_BYPASS_APPROVALS_AND_SANDBOX` is `true` in this release. The [argument builder](../../packages/adapters/codex-local/src/server/codex-args.ts) can emit `--dangerously-bypass-approvals-and-sandbox` when restrictions are omitted. Do not accept that default for the first executor.

Set these adapter fields explicitly through the native configuration surface:

| Field | First qualification choice |
| --- | --- |
| `engine` | `cli`; unset uses ACP, with no automatic fallback |
| `cwd` | A dedicated absolute workspace, not the host home directory |
| `dangerouslyBypassApprovalsAndSandbox` | `false` |
| `search` | `false` until search is needed and authorized |
| `extraArgs` | Explicit Codex sandbox settings appropriate to the task; exclude bypass flags and permissive profiles |
| `timeoutSec` | A bounded duration chosen for the test |

For a first read-only probe, use the adapter's `extraArgs` value `["--sandbox", "read-only"]`, then verify the effective invocation. Read-only filesystem settings do not by themselves establish network or tool authorization.

Optional spawn-level confinement uses `filesystemScope="workspace"` and `networkScope="deny"` or `"allowlist"`. This needs Bubblewrap and explicit `engine="cli"`; the pinned adapter rejects these controls with ACP. An allowlist must include the actual inference and Brain endpoints required by the run. Do not select network denial and then interpret a failed provider call as a Brain defect.

Review `env`, inherited environment, instructions, repository `AGENTS.md` and additional arguments before launch. Managed Codex authentication can seed a per-agent home from host authentication or a configured API key. Verify the actual worker identity and credential source without printing credentials. Authenticate the selected executor deliberately; do not copy the current desktop profile as a shortcut.

## 3. Bind the Brain and keep the agent inactive

Create the test company/Venture binding and executor only when ready for the runtime qualification. Keep autonomous wake sources, schedules and event triggers disabled. Verify the stored settings; this handoff makes no claim about their defaults. Set a small native budget and stop limits; verify their enforcement separately from provider billing.

Configure only the existing Brain MCP endpoint/profile and the operations needed by this executor. Resolve credentials on the host. Start with read-only tools. Check the actual `tools/list` output from the worker environment, then an authorized read and a forbidden read. No global administrator token belongs in the worker profile.

## 4. Run one bounded task manually

1. Read one authorized Mission and its revision from the Brain.
2. Retrieve a small, relevant set of sources with their references.
3. Record the task/run identity, result, errors, stop reason and evidence references.
4. End the run. Verify that it does not wake itself or retry the same external effect.
5. Test a forbidden Venture and a revoked grant. Both must be refused by the responsible host.

Only then admit an RSSHub call, followed by a separately authorized yt-dlp operation with bounded inputs, outputs and resource limits. A registered Cell is not proof of an installed, reachable or authorized service. Writes and Mission acceptance need separate grants. Preserve an explicit unknown result when a call cannot be reconciled.

The first commercial research task can then qualify pilot candidates from public sources. Preparing research does not authorize outreach. A useful result includes sourced findings, uncertainties and a next action, not an invented customer commitment.
