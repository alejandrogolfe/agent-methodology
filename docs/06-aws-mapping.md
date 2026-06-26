# ☁️ AWS Mapping — summary table

[← Governance](05-governance.md) · [Back to index](../README.md) · Next: [📓 Glossary →](07-glosario.md)

Quick view of which AWS service (or combination of services) covers each concept in the cycle. The detail and reasoning for each row is in the corresponding page — this table is just to avoid having to remember exact service names.

> 🚧 This table will change over time: AgentCore is a relatively new set of services and AWS expands it frequently. Check the date of the last update before assuming something is still current as written.

## Build

| Concept | AWS service / approach |
|---|---|
| Agent framework | Any (LangChain, CrewAI, direct SDK) — AgentCore is framework-agnostic |
| Agent runtime (state, control flow) | LangGraph / Strands Agents, deployed on AgentCore Runtime |
| Agent harness (skills, MCP, filesystem) | Deep Agents / Claude Agent SDK on AgentCore |
| Connect external tools without rewriting integrations | **AgentCore Gateway** (APIs, Lambda, OpenAPI → MCP tools) |
| No-code / collaborative build | No direct native equivalent — external tools or custom layer |

## Test

| Concept | AWS service / approach |
|---|---|
| Versioned datasets, regression coverage | **AgentCore Evaluations** — Dataset management |
| Metrics (ground truth + criteria) | **AgentCore Evaluations** — 13 built-in evaluators (`Builtin.*`) + Ground Truth |
| Experiments in CI/CD | **AgentCore Evaluations** — on-demand evaluation / dataset runner |
| Single model evaluation (not full agent) | Bedrock Model Evaluation job |
| Cloud-agnostic alternative | LangSmith (datasets, experiments, side-by-side comparison) |

## Deploy

| Concept | AWS service / approach |
|---|---|
| Production runtime (isolated sessions, durability) | **AgentCore Runtime** (microVMs per session) |
| Code execution sandbox | **AgentCore Code Interpreter** |
| Web browsing sandbox | **AgentCore Browser** |
| Virtual filesystem | Custom backend (S3 / DB) behind harness filesystem tools |
| Context Hub (versioned prompts/skills) | No exact native equivalent — AppConfig + dedicated Git repo, or LangSmith Context Hub if using LangChain |
| Delegated identity and permissions | **AgentCore Identity** (OAuth, AWS resources) |

## Monitor

| Concept | AWS service / approach |
|---|---|
| Tracing | **AgentCore Observability** + ADOT SDK → **CloudWatch** |
| Online evals on real traffic | **AgentCore Evaluations** — online evaluation |
| Dashboards and alerts | **CloudWatch** (namespace `Bedrock-AgentCore`) + `PutMetricAlarm` |
| Structured feedback linked to trace | No direct native service — metadata in spans/logs |
| Complementary agent-level observability (threads, sub-agents) | LangSmith Observability (coexists with CloudWatch) |

## Governance

| Concept | AWS service / approach |
|---|---|
| Cost tracking per agent/team | CloudWatch + Cost Explorer + tags + AWS Budgets |
| Tool access / delegated permissions | **AgentCore Identity** + IAM |
| Audit trails | Gateway/Runtime logs in CloudWatch (`request_id`, `trace_id`, `span_id`) |
| Human-in-the-loop | Session pauses in orchestration runtime + SQS/SNS for approval flows |
| Preventive behavioral guardrails | **AgentCore Policy** |
| Discoverability of agents/skills/tools | **AgentCore Agent Registry** (preview) |

## A closer look: AgentCore as a complete platform

It is worth noting that **AgentCore is not a single service** — it is a set of à-la-carte adoptable pieces: you can start with just Runtime, add Gateway when more tools are needed, add Identity when user-specific OAuth access is required, and add Evaluations when the team has enough traffic to need online evals. You do not need to adopt everything at once — composability is exactly the point.

## References

- AWS — [Amazon Bedrock AgentCore — Overview](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html)
- AWS — [Amazon Bedrock AgentCore FAQs](https://aws.amazon.com/bedrock/agentcore/faqs/)
- AWS — [Evaluating Deep Agents using LangSmith on AWS](https://aws.amazon.com/blogs/machine-learning/evaluating-deep-agents-using-langsmith-on-aws/)
