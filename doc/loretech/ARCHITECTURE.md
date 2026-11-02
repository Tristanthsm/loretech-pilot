# Control plane boundaries

Paperclip owns operational coordination. The Brain owns durable business meaning. The Cell SDK defines how capabilities are distributed and consumed. These are boundaries to implement and test, not integrations established by this fork.

| Object or operation | Authority | Integration rule |
| --- | --- | --- |
| Venture | Brain | Bind one Paperclip company to one Venture for the first test. Do not infer cross-Venture access from company membership. |
| Mission contract and revision | Brain | A Venture can have many Missions. A task references a Mission and its exact revision; it does not become a second editable contract. |
| Task, assignment, run, retry | Paperclip | Keep operational state here. Associate attempts with the Mission reference and execution receipts. |
| Evidence and acceptance | Brain host | Submit evidence references. A completed task does not accept a Mission. Review authority remains distinct from execution authority. |
| Lab | Brain | Groups capabilities by sector and contribution. A Lab is not automatically an agent, scheduler or permission grant. |
| Playbook | Brain | Versioned reusable method. A run references the chosen revision. It is not a second workflow engine. |
| Cell release | Cell repository and SDK | Share a release between Ventures; keep each installation, private binding and authorization separate. |
| Memory index | Brain with Cognee | Search is a derived view. Indexed text is neither accepted knowledge nor a grant of access. |
| External account | Provider and connection owner | OAuth/token management is separate from permission to invoke an operation on a resource. |

## Agent access

Use the existing Brain API/MCP boundary through a small integration adapter or Paperclip plugin. Prefer the upstream [plugin extension model](../plugins/PLUGIN_SPEC.md) to changes in the Paperclip core. Do not create another MCP server merely to rename the existing Brain tools.

The host authenticates the agent and imposes its principal, Venture and authorized operation scope. Model-supplied identity is not trusted. The Brain host enforces its OpenFGA policy for every protected operation, including memory search and evidence retrieval. A Paperclip grant must not bypass that policy. The effective permission is the intersection of provider scope, runtime policy and Brain/resource authorization.

Delegate with a distinct principal and a bounded scope. A subagent does not inherit an owner credential. Revocation must stop subsequent calls. Check the existing host's handling of in-flight asynchronous operations before admitting the bridge. Do not put secrets in task descriptions, prompts, receipts or this repository.

Expose only the tools admitted to that agent. A listed tool is not proof that a call is permitted or that the provider is reachable. Confirm discovery, allowed call, forbidden call, inter-Venture denial and revocation separately.

## Execution and memory

Paperclip owns dispatch and retries for its tasks. The Brain must not poll and schedule those same tasks. Activepieces can own a delegated fixed workflow; it must not independently replay Paperclip's external effects. Nango can manage a selected provider connection; it does not replace resource authorization. Neither service is installed or connected by this bootstrap.

Start Brain use with bounded reads and proposals: retrieve relevant sources with references, read a Mission revision, invoke an authorized Cell, then propose evidence and findings. Acceptance requires its own authorized reviewer. Cognee and the canonical Brain data remain behind the Brain host; external executors do not get direct database access.

Keep retrieval bounded by query, scope, result count and freshness. Preserve references instead of copying whole corpora into every prompt. Measure task quality, latency, tokens and full cost against a direct execution baseline before claiming an optimization.

For external effects, assign one idempotency/reconciliation owner. A timeout after sending a request is an unknown result. Reconcile it before a retry. Use expected revisions for concurrent writes. Keep task/run identifiers and evidence references in receipts without mirroring private content.

## First integration gate

One company/Venture binding, one manually invoked executor and one bounded task are sufficient. Prove a Brain read, an authorized Cell result, an explicit stop and a receipt. Then prove denial in a second Venture and revocation. Failure must remain visible in Paperclip; missing evidence must not become a successful Mission. Add more Missions and agents only after this boundary works.
