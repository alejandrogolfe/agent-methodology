# 📓 Glosario

[← AWS Mapping](06-aws-mapping.md) · [Volver al índice](../README.md)

Términos que aparecen en la charla/artículo original y que no eran obvios a la primera. Cada uno enlaza a la página donde se explica en contexto.

---

### Dogfooding trace
*(ver [Test → Inputs](02-test.md#inputs--de-dónde-salen-los-casos-de-prueba))*

Una traza capturada mientras el propio equipo, o usuarios internos, usan el agente de forma genuina — no como test sintético inventado. "Dogfooding" viene de la expresión inglesa "eat your own dog food": usar tu propio producto antes (o en paralelo) de dárselo a usuarios externos. La traza resultante refleja cómo se usa el agente *de verdad*, con toda la ambigüedad real del lenguaje humano, en lugar de los casos "limpios" que un ingeniero suele imaginar al escribir tests a mano. Suele ser la fuente de casos de test más valiosa antes de tener tráfico de producción real.

### Regression coverage
*(ver [Test → Datasets](02-test.md#datasets--cómo-se-preservan-los-casos))*

El subconjunto de un dataset (normalmente los "hard cases" ya resueltos en algún momento) que se vuelve a correr en cada cambio del agente, para verificar que una mejora en un sitio no rompe algo que ya funcionaba en otro. Es el equivalente, en testing de agentes, de los tests de regresión en software tradicional. La diferencia práctica es la disciplina de proceso: cada vez que se arregla un fallo real detectado en producción o en dogfooding, ese caso concreto entra en regression coverage para siempre — si no se hace esto sistemáticamente, los mismos errores reaparecen ciclo tras ciclo.

### Virtual filesystem
*(ver [Deploy → Virtual filesystem](03-deploy.md#virtual-filesystem--cuando-no-hace-falta-un-sandbox-completo))*

Un sistema de archivos que el agente puede usar (operaciones tipo `ls`, `read_file`, `write_file`, `glob`, `grep`) sin que eso implique ejecutar código arbitrario dentro de un sandbox aislado. Por debajo puede estar respaldado por almacenamiento normal (S3, una base de datos), pero de cara al agente se comporta como carpetas y archivos. Sirve como memoria de trabajo: el agente guarda resultados intermedios grandes, organiza notas, o mantiene historial — sin el coste ni el riesgo de un entorno de ejecución de código completo.

### Context Hub
*(ver [Deploy → Context Hub](03-deploy.md#context-hub--gestionar-prompts-y-contexto-como-un-sistema-aparte-del-código))*

Un sitio dedicado a guardar, versionar, revisar y actualizar las partes "no-código" de un agente — prompts, skills, políticas, ejemplos — separado del repositorio de código de la aplicación. Existe porque estas piezas cambian con más frecuencia que el código, y a menudo las edita gente que no es ingeniera (expertos de dominio, soporte, producto). Permite ajustar el comportamiento del agente sin un despliegue completo de la aplicación, y sirve como base para la discoverability de skills reutilizables entre agentes distintos.

### Harness (agent harness)
*(ver [Build → Las tres capas](01-build.md#agent-harnesses--el-entorno-de-trabajo-completo))*

La capa que da al agente el entorno de trabajo completo que necesita para tareas largas: prompts, skills, servidores MCP, hooks, middleware, y a veces un filesystem propio. Se distingue de un *framework* (que da abstracciones para componer llamadas a modelo y herramientas) y de un *runtime* (que da ejecución con estado, control de flujo y durabilidad). Ejemplos: Deep Agents, Claude Agent SDK.

### LLM-as-judge
*(ver [Test → Metrics](02-test.md#metrics--cómo-se-mide-el-éxito) y [Monitor → Online evals](04-monitor.md#online-evals--señales-sobre-tráfico-real))*

Usar un modelo de lenguaje como evaluador automático de la salida de otro agente o modelo, normalmente contra un criterio o rúbrica definida (¿fue útil?, ¿siguió la política?, ¿fue grounded?). Útil para tareas sin ground truth único. Conviene calibrar el juez LLM contra juicio humano periódicamente, porque los jueces automáticos no siempre aciertan y pueden derivar (drift) con el tiempo.

### Online evaluation vs. offline evaluation
*(ver [Test](02-test.md) y [Monitor → Online evals](04-monitor.md#online-evals--señales-sobre-tráfico-real))*

**Offline** = evaluación contra un dataset curado antes de desplegar un cambio — es el equivalente de tests unitarios/de integración para un agente, y se corre en desarrollo o en CI/CD. **Online** = evaluación continua sobre tráfico real de producción, sin necesidad de desplegar código nuevo para cada chequeo — es el equivalente de monitorización en vivo o canary testing. Ambas son necesarias y se complementan: offline para no desplegar a ciegas, online para detectar lo que el dataset offline no anticipó.

### Human-in-the-loop (HITL)
*(ver [Deploy → Runtime](03-deploy.md#runtime--la-base-de-la-ejecución) y [Governance → Tool Access](05-governance.md#human-in-the-loop))*

Patrón donde el agente se pausa a mitad de ejecución para esperar aprobación, aclaración o revisión humana, y luego reanuda exactamente donde lo dejó. Requiere que el runtime soporte ejecución durable (checkpoints) — sin eso, "pausar" significa en la práctica "perder el progreso y reintentar desde cero".

---

[← AWS Mapping](06-aws-mapping.md) · [Volver al índice](../README.md)
