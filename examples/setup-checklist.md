# 装机前清单演练 | Pre-setup drill: everything to line up before SETUP.md

dsh-superagent 的安装是让你的编程 agent 读 `SETUP.md` 陪你一步步装，所以真正的功夫在开口之前——这张清单逐项备齐，装机时就不会卡在「这项得你本人来」上：

1. **运行时逐项验版本**：`node -v`（要 22.19+ 或 24+）、`pnpm -v`、`git --version`（2.32+）、`tmux -V`、`python3 --version`（3.8+）、`gh --version`（GitHub CLI 2.48+）。缺哪项先补哪项，别等装到一半才发现。
2. **DSH 本体先跑通**：确认 DeepSeek Harness 已装且版本在原仓 package.json 的 `engines.dsh` 范围内，并用一个模型供应商真实跑过一次对话——从没跑过 DSH 的，配供应商这步是操作者本人的活，agent 代替不了。
3. **给 agent 开专用 GitHub 账号**：新注册一个只给值班 agent 用的账号，**不要用你自己的**。它的一切提交、评论、PR 都会挂在这个名字下；提交作者身份也设成它。想好这个名字，以后别改。
4. **圈定监听范围**：先只盯一个仓库练手；飞书是可选项，要接就先备好机器人所在聊天的 id，装机时要填白名单。
5. **授权先按最严口径定稿**：默认 `observe` 只记录不开工；授权名单只放你自己；auto-merge 关、人来合。这三条写下来再开始装，装的过程中不许临时放宽。
6. **开口装机**：把原仓交给你的编程 agent，原话就是「Read SETUP.md in this repository and set up dsh-superagent with me.」它一次问一个短问题、展示完整计划后才动手——计划里出现清单外的新权限要求，先停下看第 5 条，别当场点头。
7. **observe 验一周再放行**：装完先以 observe 跑一周，看它「本会接」的活和「本会说」的话是否合你意；再按名单→规则的顺序逐条放开，每放开一条规则，观察它接的第一个活全程。
