# Private local qualification runtime

This fork uses the native Paperclip server, local execution environment and managed AI connections. It does not need another gateway or a sandbox provider for the first local Codex sign-in.

## Install and start

Use Node 24.11 or newer and the pinned pnpm 9.15.4. Keep the package manager on the child process PATH too. Install with the committed lockfile:

```sh
corepack pnpm install --frozen-lockfile
corepack pnpm --filter @paperclipai/plugin-sdk build
corepack pnpm --filter @paperclipai/ui build
```

The plugin SDK must produce `packages/plugins/sdk/dist/index.js` before server startup. A built UI avoids the development-module waterfall over a private proxy.

Use a dedicated `PAPERCLIP_HOME`, `PAPERCLIP_INSTANCE_ID`, `PAPERCLIP_CONFIG`, PostgreSQL data directory/port, storage, backups, encryption key and logs. Keep each private path under an ignored runtime directory with restrictive permissions. Never reuse another Paperclip database or import the operator's Codex profile.

The config must select `authenticated`, `private`, and loopback binding. Put its external private HTTPS origin in `auth.publicBaseUrl` and `server.allowedHostnames`. Set `PAPERCLIP_TELEMETRY_DISABLED=1`, `PAPERCLIP_UI_DEV_MIDDLEWARE=false`, `HEARTBEAT_SCHEDULER_ENABLED=false`, and private random `BETTER_AUTH_SECRET` / `PAPERCLIP_AGENT_JWT_SECRET`. Keep provider keys out of the service environment. Point service `CODEX_HOME` to a dedicated empty directory so legacy fallback cannot select the desktop profile.

Run the native server with the installed `server/node_modules/tsx/dist/cli.mjs` and `server/src/index.ts`. A user service can own restart and shutdown. This is process supervision, not an additional task scheduler. The source runtime needs an explicit Node heap sufficient for imports; the qualification host uses `NODE_OPTIONS=--max-old-space-size=1536` within a 2 GiB service cap. Measure actual consumption on another host.

## Operator connection path

1. Open the private operator activation link. Create the local Paperclip operator account, then use **Claim Board ownership** to adopt the prepared company and agent. A fresh authenticated installation uses first-admin claim; an instance prepared in local trusted mode uses the native one-time board claim. This account is separate from the OpenAI account. The claim link is private, expires after 24 hours, and changes on server restart; never put it in Git or a public task.
2. Open the prepared company, then the prepared Codex agent's **Configuration** tab.
3. In **AI connection**, open **Connect an account** and choose the OpenAI subscription sign-in.
4. Complete the displayed browser/device authorization using the intended OpenAI account. Save the agent configuration when the connection returns.

Use the managed AI connection flow. It creates a separate temporary login home, clears inherited provider keys, and stores verified credentials in Paperclip's managed secret storage. It also verifies the account against the provider. This is a real external authentication action, to be completed by the operator.

Do not confuse it with the adapter-specific **Sign in to OpenAI** device-login route: that older route requires a compatible sandbox. Native **AI connection** local login supports an active local environment. The runtime health endpoint advertises `localAiLoginSupported`.

When connecting from an existing agent, this fork scopes the new credential to that agent even if the operator can manage all connections. A manager permission must not silently create a company-wide credential grant. Responsible-user default selection and per-agent grant scope are separate: another agent does not gain access merely because the account is the user's default.

## Prepared executor and Brain boundary

Keep the executor paused, all heartbeat/wake sources disabled, concurrency one, a bounded timeout and a small native budget. Use explicit CLI mode, no approval/sandbox bypass, a read-only Codex sandbox and verified filesystem confinement. Do not grant agent, Skill, task-assignment or tool-administration authority.

Register the existing Brain MCP launcher as an operator-approved local stdio template. Private Brain credentials stay in their owning host files. Install it only for the chosen agent. A deny-default profile includes exact catalog entries for `brain_read` and `brain_search`; it excludes Brain writes and Cell invocation. Inspect effective access after activation. Template catalog declarations are not runtime discovery: qualify a real call through Paperclip too.

The first bridge uses an existing host-bound Brain reader profile. It is not yet a distinct Brain principal for each Paperclip agent. Before multi-agent write access, qualify per-agent delegated identity, revocation and Venture isolation through the Brain host. Do not represent a successful board test call as a model-driven agent run.

## Check and rollback

Verify health, unauthenticated API refusal, browser login rendering, active local environment, persisted paused agent, exact effective tool profile, allowed Brain read and refused write. Restart the private service and verify the same company, agent, profile and state survive. No model call is needed to prepare authentication.

For rollback, remove only the new private proxy listener and stop/disable the dedicated user service. Preserve the private database, backups, config and secret key together. Do not modify other runtime instances or existing proxy routes. Full upstream typecheck, test suite and Rust runner build remain separate qualification gates; targeted checks and a working connection screen do not establish autonomous company readiness.
