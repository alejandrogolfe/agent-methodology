# 📓 Glossary

[← AWS Mapping](06-aws-mapping.md) · [Back to index](../README.md)

Terms that appear in the original talk/article and were not obvious the first time. Each one links to the page where it is explained in context.

---

### Dogfooding trace
*(see [Test → Inputs](02-test.md#inputs--where-test-cases-come-from))*

A trace captured while the team itself, or internal users, use the agent genuinely — not as an invented synthetic test. "Dogfooding" comes from the expression "eat your own dog food": using your own product before (or in parallel with) giving it to external users. The resulting trace reflects how the agent is *actually* used, with all the real ambiguity of human language, rather than the "clean" cases an engineer typically imagines when writing tests by hand. It is usually the most valuable source of test cases before having real production traffic.

### Regression coverage
*(see [Test → Datasets](02-test.md#datasets--how-cases-are-preserved))*

The subset of a dataset (usually the "hard cases" that were resolved at some point) that gets re-run on every agent change, to verify that an improvement in one place does not break something that was already working elsewhere. It is the equivalent, in agent testing, of regression tests in traditional software. The practical difference is process discipline: every time a real failure detected in production or dogfooding is fixed, that specific case enters regression coverage permanently — if this is not done systematically, the same errors resurface cycle after cycle.

### Virtual filesystem
*(see [Deploy → Virtual filesystem](03-deploy.md#virtual-filesystem--when-a-full-sandbox-isnt-needed))*

A filesystem the agent can use (operations like `ls`, `read_file`, `write_file`, `glob`, `grep`) without that implying executing arbitrary code inside an isolated sandbox. Under the hood it can be backed by normal storage (S3, a database), but from the agent's perspective it behaves like folders and files. It serves as working memory: the agent stores large intermediate results, organizes notes, or maintains history — without the cost or risk of a full code execution environment.

### Context Hub
*(see [Deploy → Context Hub](03-deploy.md#context-hub--managing-prompts-and-context-separately-from-code))*

A dedicated place to store, version, review and update the "non-code" parts of an agent — prompts, skills, policies, examples — separate from the application code repository. It exists because these pieces change more frequently than code, and are often edited by people who are not engineers (domain experts, support, product). It allows adjusting agent behavior without a full application deployment, and serves as the foundation for the discoverability of reusable skills across different agents.

### Harness (agent harness)
*(see [Build → The three layers](01-build.md#agent-harnesses--the-complete-work-environment))*

The layer that gives the agent the complete work environment it needs for long tasks: prompts, skills, MCP servers, hooks, middleware, and sometimes its own filesystem. It is distinguished from a *framework* (which provides abstractions to compose model and tool calls) and from a *runtime* (which provides stateful execution, control flow and durability). Examples: Deep Agents, Claude Agent SDK.

### LLM-as-judge
*(see [Test → Metrics](02-test.md#metrics--how-success-is-measured) and [Monitor → Online evals](04-monitor.md#online-evals--signals-on-real-traffic))*

Using a language model as an automatic evaluator of the output of another agent or model, typically against a defined criterion or rubric (was it helpful? did it follow the policy? was it grounded?). Useful for tasks without a unique ground truth. It is worth calibrating the LLM judge against human judgment periodically, because automatic judges don't always get it right and can drift over time.

### Online evaluation vs. offline evaluation
*(see [Test](02-test.md) and [Monitor → Online evals](04-monitor.md#online-evals--signals-on-real-traffic))*

**Offline** = evaluation against a curated dataset before deploying a change — the equivalent of unit/integration tests for an agent, run in development or in CI/CD. **Online** = continuous evaluation on real production traffic, without needing to deploy new code for each check — the equivalent of live monitoring or canary testing. Both are necessary and complementary: offline to avoid deploying blindly, online to detect what the offline dataset didn't anticipate.

### Human-in-the-loop (HITL)
*(see [Deploy → Runtime](03-deploy.md#runtime--the-foundation-of-execution) and [Governance → Tool Access](05-governance.md#human-in-the-loop))*

A pattern where the agent pauses mid-execution to wait for human approval, clarification or review, and then resumes exactly where it left off. Requires the runtime to support durable execution (checkpoints) — without that, "pausing" means in practice "losing progress and retrying from scratch".

---

[← AWS Mapping](06-aws-mapping.md) · [Back to index](../README.md)
