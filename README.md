# dsh-superagent 速查表 | dsh-superagent Cheatsheet | Guía rápida de dsh-superagent

> 一页看懂 DeepSeek Harness 团队开源的 dsh-superagent：给一个 GitHub 仓库配一个「值班 agent」——它监听仓库（可选接飞书），只有被授权的人开口要活、或团队预先写好的规则允许时才动手；每个活开一个独立工单、独立会话和独立目录去做，做完回到原对话里交结果（通常是一个 PR）；网页面板只显示「哪件事必须人出面」。一句话钩子：给仓库雇一个不用睡觉的值班员，还不许它没事来烦你。

- 上游项目：[deepseek-ai/dsh-superagent-prototype](https://github.com/deepseek-ai/dsh-superagent-prototype)（2026-11-02 实测 12,275 ★ / 1,273 forks，2026-10-31 建仓，MIT；根目录只有英文 README 与编号文档）
- 平台：DeepSeek Harness 的 bundle；监听 GitHub，可选接飞书（Feishu）
- 本页结构：中文主文 → English → Español → Português

---

Apoyo y más: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — plantillas y cuadernos que uso](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)

## 它是什么、不是什么

| 是 | 不是 |
|---|---|
| 主动型 agent：自己盯着一个仓库，有活按规则接、没活不出声，人只处理例外 | 不是坐在对面等指令的聊天 agent：它的设计目标恰恰是「尽量不占用人的注意力」 |
| 动手有门禁：只有被授权的人开口、或团队预先写好的规则放行，它才开工 | 不是装好就全自动乱跑：默认 observe 模式只记录不开工（见授权卡） |
| 每个活独立工单、独立会话、独立目录，做完回原对话交结果（通常是 PR） | 不是拿你的账号干活：必须给它一个专用 GitHub 账号，它写的一切都挂在那个名字下 |
| DeepSeek Harness 团队的研究原型，原仓头部明示「非 DeepSeek 正式产品」 | 不是生产级成品：原仓自述可能有 bug、工程风格不一致，用到自己环境前要先审、先改 |

## 工作原则一句话卡

> 尽量少占用人的注意力，尽量多交付价值（take as little human attention as possible, and give as much value as possible）。

落到行为上就三条：**只在被授权的人开口、或团队规则允许时才动手；只在「必须有人拍板」或「成果已经可用」时才说话；面板只显示需要人出面的事。** 一个能替整个团队干活的 agent，也能浪费整个团队的注意力——这套原则就是给它上的缰绳。细则在原仓 `docs/000-philosophy.md`。

## 装法卡：不是一条命令，是让 agent 陪你装

它的安装方式很特别：把原仓交给你的编程 agent，让它读 `SETUP.md` 一步步陪你装——一次只问一个短问题、先把完整计划展示给你看、然后才动手安装与配置。所以真正的门槛是下面这张前置清单，缺哪项都会变成计划里的一项，先备齐再开口：

| 前置项 | 要求（原仓 SETUP.md 口径） |
|---|---|
| 运行时 | Node.js 22.19+ 或 24+、pnpm、git 2.32+、tmux、Python 3.8+、GitHub CLI 2.48+ |
| Harness 本体 | DSH（DeepSeek Harness）在原仓 package.json 的 `engines.dsh` 版本范围内 |
| 模型供应商 | DSH 要先配好一个模型供应商；从没跑过 DSH 的，这一步是操作者本人要做的 |
| 专用 GitHub 账号 | 给 agent 单独一个账号，**绝不用操作者本人的账号**：它写的一切都显示在这个名字下，提交作者身份也用它 |
| 监听范围 | 要盯哪个仓库；飞书是可选项，接飞书还要备机器人的聊天 id 白名单 |
| 合并纪律 | auto-merge 默认不开：先由人来合，等它的 PR 长期靠谱了再谈 |

## 授权模式卡（全仓最不能看错的一页）

| 模式/设置 | 含义 | 什么时候用 |
|---|---|---|
| `observe`（默认） | 只记录它「本会接」的主动工作，不真正开工 | 刚装好先跑这个：看它会接什么、会说什么，验一周再放行 |
| 授权名单（allowed lists） | 只有名单里的人开口要活，它才接单 | 单人/小团队的默认门禁，GitHub 与飞书各配各的名单 |
| 团队规则（policy） | 团队预先写好的规则允许时，它可以不等开口自动接活 | 规则先行、逐条放开；每放开一条，先想清楚给错权的代价 |
| auto-merge | 默认关，agent 永不以自己账号开启 | 人合 PR 是它早期的信任建立期，别跳过 |

**给错权 = agent 拿着你的权限乱跑。** 授权这一页看错一步，比装错任何一步都贵：顺序只能是 observe → 名单 → 规则逐条放开，不许反过来。

## 编号文档阅读顺序卡

原仓 9 篇编号文档按序读，每篇只依赖前面的，跳读会断链：

| 编号 | 文档 | 讲什么 |
|---|---|---|
| 000 | Philosophy | 工作原则：为什么它必须少占注意力（先读这篇） |
| 001 | Domain model | 领域模型：工单、会话、来源这些概念怎么定义 |
| 002 | Authorization | 授权：谁能让它动手、规则怎么写 |
| 003 | Execution | 执行：一个活从接单到交付怎么跑 |
| 004 | Prompts | 提示词：给它的指令怎么组织 |
| 005 | Panel | 面板：人只看哪几件事 |
| 006 | Deployment | 部署：手动安装、配置、升级与排障（装机时主要翻这篇） |
| 007 | Architecture | 架构：内部怎么搭的 |
| 008 | Cookbook | 食谱：怎么给它加一个新的信息来源（Source） |

开发自检：原仓用 `pnpm check` 一条命令跑类型检查、测试与构建。

## 诚实标注区

- **研究原型，不是正式产品**（原仓头部自述）：「Research prototype for reference only, not an official DeepSeek product」；代码出自团队日常开发、大量工程由编程 agent 完成，工程风格与实践可能不一致，可能有 bug，用到自己环境前先审、先改。
- **与内部版和论文不完全一致**（原仓自述）：公开发布时做过适配，与团队内部版本、以及设计论文《Exploring Super-agent Design》里描述的实现不完全相同；论文里有些设计是未来方向，原型里并未实现。
- **模拟测试数字是自报口径**（证据 B）：西语媒体转述的团队模拟测试——约 300 条模拟消息里约 20 个工单、p95 完成时间 15.7 分钟（单会话基线 29.5 分钟）——是团队自报的模拟结果，没有测真实代码质量，别拿它当生产基准。
- **势头数据**：12,275 ★ / 1,273 forks（2026-11-02 原仓页实测，证据 A）；建仓 2 天冲上 GitHub 日榜增速第 1、单日约 +6,097 星（第三方榜单口径，证据 B）。体量是本批最小的一个，赌的是 DeepSeek 品牌与「值班型 agent」这个新品类位置。

---

## English (summary)

dsh-superagent (by the DeepSeek Harness team; 12,275 stars / 1,273 forks as of 2026-11-02, MIT, repo created 2026-10-31) is a proactive coding agent for a single GitHub repository, packaged as a bundle for DeepSeek Harness. It listens to GitHub (Feishu optional) and only acts when an authorized person asks for work or a rule the team wrote allows it. Each job gets its own work item, agent session and directory, and the result comes back in the original conversation — usually as a pull request. A web panel shows only what needs a person. Its working principle fits on one card: take as little human attention as possible, and give as much value as possible.

There is no one-line install: you hand the repo to your coding agent and have it read `SETUP.md` and set things up with you, one short question at a time. Prerequisites (per SETUP.md): Node.js 22.19+ or 24+, pnpm, git 2.32+, tmux, Python 3.8+, GitHub CLI 2.48+, DSH itself within the `engines.dsh` range, a configured model provider, and — critically — a **dedicated GitHub account for the agent, never your own**. Authorization is the page you must not misread: the default mode is `observe` (it records proactive work without doing it); allowed lists gate who can assign work; team-written policy rules are what eventually let it pick up work unprompted; auto-merge stays off and people merge until its PRs have been right for a while. The nine numbered docs (000 Philosophy → 001 Domain → 002 Authorization → 003 Execution → 004 Prompts → 005 Panel → 006 Deployment → 007 Architecture → 008 Cookbook) are meant to be read in order.

Honest notes: the upstream README itself labels this a research prototype, not an official DeepSeek product — adapted for publication, not identical to the internal version or the design paper, possibly buggy and inconsistently engineered. The simulation numbers circulating in Spanish-language coverage (about 20 work items out of ~300 simulated messages; p95 completion 15.7 min vs a 29.5 min single-session baseline) are the team's self-reported simulation results, with no real code quality measured — evidence level B.

---

## Español (guía rápida — traducción de estudio, pendiente de revisión por un hablante nativo)

dsh-superagent (del equipo DeepSeek Harness; 12.275 estrellas / 1.273 forks a 2 de noviembre de 2026, MIT, repositorio creado el 31 de octubre de 2026) es un agente de programación proactivo para un único repositorio de GitHub, empaquetado como bundle para DeepSeek Harness. Escucha GitHub (Feishu es opcional) y solo actúa cuando una persona autorizada pide trabajo o cuando una regla escrita por el equipo lo permite. Cada encargo recibe su propio ítem de trabajo, su propia sesión y su propio directorio, y el resultado vuelve a la conversación original, normalmente como un pull request. Un panel web muestra solo lo que necesita a una persona. Su principio de trabajo cabe en una tarjeta: ocupar la menor atención humana posible y entregar el mayor valor posible.

No hay instalación en un comando: entregas el repositorio a tu agente de programación y le pides que lea `SETUP.md` y lo instale contigo, con una pregunta corta cada vez. Requisitos previos (según SETUP.md): Node.js 22.19+ o 24+, pnpm, git 2.32+, tmux, Python 3.8+, GitHub CLI 2.48+, el propio DSH dentro del rango `engines.dsh`, un proveedor de modelos configurado y — lo crítico — **una cuenta de GitHub dedicada para el agente, nunca la tuya**. La autorización es la página que no puedes leer mal: el modo por defecto es `observe` (registra el trabajo proactivo sin hacerlo); las listas de permitidos controlan quién puede asignar trabajo; las reglas de política escritas por el equipo son lo que al final le permite tomar trabajo sin que nadie lo pida; y el auto-merge queda desactivado: fusiona la gente hasta que sus PR sean fiables durante un tiempo. Los nueve documentos numerados (000 Philosophy → 001 Domain → 002 Authorization → 003 Execution → 004 Prompts → 005 Panel → 006 Deployment → 007 Architecture → 008 Cookbook) están hechos para leerse en orden.

**Notas honestas**: el propio README lo etiqueta como prototipo de investigación, no un producto oficial de DeepSeek — adaptado para su publicación, no idéntico a la versión interna ni al artículo de diseño, posiblemente con bugs e ingeniería inconsistente. Las cifras de simulación que circulan en medios en español (unos 20 encargos de ~300 mensajes simulados; p95 de finalización de 15,7 min frente a una base de sesión única de 29,5 min) son resultados de simulación autodeclarados por el equipo, sin medir calidad real de código — evidencia nivel B.

---

## Português (guia rápido — tradução de estudo, aguardando revisão de um falante nativo)

dsh-superagent (da equipe DeepSeek Harness; 12.275 estrelas / 1.273 forks em 2 de novembro de 2026, MIT, repositório criado em 31 de outubro de 2026) é um agente de programação proativo para um único repositório do GitHub, empacotado como bundle para o DeepSeek Harness. Ele escuta o GitHub (Feishu é opcional) e só age quando uma pessoa autorizada pede trabalho ou quando uma regra escrita pela equipe permite. Cada tarefa recebe seu próprio item de trabalho, sua própria sessão e seu próprio diretório, e o resultado volta à conversa original, normalmente como um pull request. Um painel web mostra apenas o que precisa de uma pessoa. Seu princípio de trabalho cabe em um cartão: ocupar a menor atenção humana possível e entregar o maior valor possível.

Não há instalação em um comando: você entrega o repositório ao seu agente de programação e pede que ele leia o `SETUP.md` e instale tudo com você, uma pergunta curta por vez. Pré-requisitos (segundo o SETUP.md): Node.js 22.19+ ou 24+, pnpm, git 2.32+, tmux, Python 3.8+, GitHub CLI 2.48+, o próprio DSH dentro da faixa `engines.dsh`, um provedor de modelos configurado e — o ponto crítico — **uma conta do GitHub dedicada para o agente, nunca a sua**. A autorização é a página que você não pode ler errado: o modo padrão é `observe` (ele registra o trabalho proativo sem executá-lo); as listas de permitidos controlam quem pode atribuir trabalho; as regras de política escritas pela equipe são o que enfim permite que ele pegue trabalho sem ninguém pedir; e o auto-merge fica desligado: quem faz merge são pessoas, até que os PRs dele sejam confiáveis por um bom tempo. Os nove documentos numerados (000 Philosophy → 001 Domain → 002 Authorization → 003 Execution → 004 Prompts → 005 Panel → 006 Deployment → 007 Architecture → 008 Cookbook) foram feitos para serem lidos em ordem.

**Notas honestas**: o próprio README o rotula como protótipo de pesquisa, não um produto oficial da DeepSeek — adaptado para publicação, não idêntico à versão interna nem ao artigo de design, possivelmente com bugs e engenharia inconsistente. Os números de simulação que circulam na mídia em espanhol (cerca de 20 tarefas em ~300 mensagens simuladas; p95 de conclusão de 15,7 min contra uma base de sessão única de 29,5 min) são resultados de simulação autodeclarados pela equipe, sem medir a qualidade real do código — evidência nível B.

---

## 来源与声明

- 灵感来源 / Source / Fuente / Fonte: [deepseek-ai/dsh-superagent-prototype](https://github.com/deepseek-ai/dsh-superagent-prototype)（设计论文：Exploring Super-agent Design: Towards a More Autonomous Agent System）
- 本仓是个人学习整理：内容用我自己的话总结自上游公开文档，不搬运原文；前置清单、授权模式与文档顺序以 2026-11-02 的上游 README 与 SETUP.md 为准，上游迭代后请以官方文档为最终依据。
- 西语、葡语部分为学习译稿，待母语校对。Las secciones en español y portugués son traducciones de estudio pendientes de revisión nativa.
- License: MIT © 2026 Guangyi Zhao
