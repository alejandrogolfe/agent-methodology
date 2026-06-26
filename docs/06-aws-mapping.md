# ☁️ AWS Mapping — tabla resumen

[← Governance](05-governance.md) · [Volver al índice](../README.md) · Siguiente: [📓 Glosario →](07-glosario.md)

Vista rápida de qué servicio (o combinación de servicios) de AWS cubre cada concepto del ciclo. El detalle y el razonamiento de cada fila está en la página correspondiente — esta tabla es solo para no tener que recordar nombres exactos de servicios.

> 🚧 Esta tabla cambiará con el tiempo: AgentCore es un conjunto de servicios relativamente nuevo y AWS lo amplía con frecuencia. Revisar la fecha de la última actualización antes de asumir que algo sigue vigente tal cual.

## Build

| Concepto | Servicio / enfoque AWS |
|---|---|
| Agent framework | Cualquiera (LangChain, CrewAI, SDK directo) — AgentCore es agnóstico de framework |
| Agent runtime (estado, control de flujo) | LangGraph / Strands Agents, desplegado sobre AgentCore Runtime |
| Agent harness (skills, MCP, filesystem) | Deep Agents / Claude Agent SDK sobre AgentCore |
| Conectar herramientas externas sin reescribir integraciones | **AgentCore Gateway** (APIs, Lambda, OpenAPI → herramientas MCP) |
| No-code / build colaborativo | Sin equivalente nativo directo — herramientas externas o capa propia |

## Test

| Concepto | Servicio / enfoque AWS |
|---|---|
| Datasets versionados, regression coverage | **AgentCore Evaluations** — Dataset management |
| Métricas (ground truth + criterios) | **AgentCore Evaluations** — 13 evaluadores integrados (`Builtin.*`) + Ground Truth |
| Experiments en CI/CD | **AgentCore Evaluations** — on-demand evaluation / dataset runner |
| Evaluación de un solo modelo (no agente completo) | Bedrock Model Evaluation job |
| Alternativa agnóstica de cloud | LangSmith (datasets, experiments, comparación) |

## Deploy

| Concepto | Servicio / enfoque AWS |
|---|---|
| Runtime de producción (sesiones aisladas, durabilidad) | **AgentCore Runtime** (microVMs por sesión) |
| Sandbox de ejecución de código | **AgentCore Code Interpreter** |
| Sandbox de navegación web | **AgentCore Browser** |
| Virtual filesystem | Backend propio (S3 / DB) detrás de las herramientas de filesystem del harness |
| Context Hub (prompts/skills versionados) | Sin equivalente nativo exacto — AppConfig + repo Git dedicado, o LangSmith Context Hub si se usa LangChain |
| Identidad y permisos delegados | **AgentCore Identity** (OAuth, AWS resources) |

## Monitor

| Concepto | Servicio / enfoque AWS |
|---|---|
| Tracing | **AgentCore Observability** + ADOT SDK → **CloudWatch** |
| Online evals sobre tráfico real | **AgentCore Evaluations** — online evaluation |
| Dashboards y alertas | **CloudWatch** (namespace `Bedrock-AgentCore`) + `PutMetricAlarm` |
| Feedback estructurado atado a traza | Sin servicio nativo directo — metadata en spans/logs |
| Observabilidad complementaria a nivel de agente (threads, sub-agentes) | LangSmith Observability (convive con CloudWatch) |

## Governance

| Concepto | Servicio / enfoque AWS |
|---|---|
| Cost tracking por agente/equipo | CloudWatch + Cost Explorer + tags + AWS Budgets |
| Tool access / permisos delegados | **AgentCore Identity** + IAM |
| Audit trails | Logs de Gateway/Runtime en CloudWatch (`request_id`, `trace_id`, `span_id`) |
| Human-in-the-loop | Pausas de sesión en runtime de orquestación + SQS/SNS para flujos de aprobación |
| Guardrails preventivos de comportamiento | **AgentCore Policy** |
| Discoverability de agentes/skills/tools | **AgentCore Agent Registry** (preview) |

## Vista de una pieza: AgentCore como plataforma completa

Vale la pena remarcar que **AgentCore no es un solo servicio**, es un conjunto de piezas adoptables a la carta: se puede empezar solo con Runtime, añadir Gateway cuando hagan falta más herramientas, sumar Identity cuando se necesite acceso OAuth específico de usuario, y añadir Evaluations cuando el equipo ya tenga suficiente tráfico como para necesitar online evals. No hace falta adoptarlo todo de golpe — la composabilidad es justo el punto.

## Referencias

- AWS — [Amazon Bedrock AgentCore — Overview](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html)
- AWS — [Amazon Bedrock AgentCore FAQs](https://aws.amazon.com/bedrock/agentcore/faqs/)
- AWS — [Evaluating Deep Agents using LangSmith on AWS](https://aws.amazon.com/blogs/machine-learning/evaluating-deep-agents-using-langsmith-on-aws/)
