# 毕业设计答辩 Skill

把项目材料和学校规范整理成图表、答辩 PPT、口播稿、论文文档与评审问答。源码位于 [`skills/graduation-defense`](skills/graduation-defense)，可用于支持 `SKILL.md` 的本地 Agent。

本项目依据用户提供的《毕业答辩-ChatGPT物料工作流》改写：保留图表、PPT、论文和问答规则，把针对 ChatGPT GUI 的限制与 Codex 的实际能力区分开。没有上传学校模板、论文或私人项目资料。

## 安装与调用

将整个 `skills/graduation-defense` 文件夹放到 Codex 用户技能目录；也可在 Windows 用 Junction 指向仓库，保持源码只有一份。以下以本次仓库目录为例，路径按实际克隆位置调整：

```powershell
$skillSource = 'E:\毕设答辩skill\skills\graduation-defense'
$skillHome = if ($env:CODEX_HOME) { Join-Path $env:CODEX_HOME 'skills' } else { Join-Path $env:USERPROFILE '.codex\skills' }
$skillLink = Join-Path $skillHome 'graduation-defense'
if (Test-Path -LiteralPath $skillLink) { throw "已存在，请先核对：$skillLink" }
New-Item -ItemType Directory -Path $skillHome -Force | Out-Null
New-Item -ItemType Junction -Path $skillLink -Target $skillSource
```

安装后新开对话；若技能列表尚未刷新，重启客户端。默认支持自动选择，显式调用最直接：

```text
使用 $graduation-defense，读取我的学校模板、建表 SQL 和项目说明，
先列证据与缺口，再制作 15 页、8 分钟的答辩 PPT，并给 3 分钟精简稿。
```

也可以单独调用：

- “使用 $graduation-defense，将这份 SQL 转成 ER 图，附完整 Mermaid 源码。”
- “使用 $graduation-defense，按学校 Word 模板整理第三章，保留原稿。”
- “使用 $graduation-defense，审阅现有答辩 PPT，列 10 个最可能追问的问题。”

## 包含的能力

| 交付物 | 主要约束 |
|---|---|
| 流程、ER、时序与架构图 | 完整 Mermaid 源码，默认每图最多 9 个业务节点，可拆总览和细节图 |
| PPT 与备注 | 默认 12–15 页，正文 3–5 要点；全篇限时优先，真实演讲者备注 |
| 精简讲稿 | 3 分钟路线、独立压缩稿与每页用时 |
| 论文 | 读取学校规格，章节图表编号、文中引用、真实参考文献 |
| 模拟答辩 | 默认 10 个问题、证据位置、追问及尚答不上来的缺口 |
| 基础文件导出 | 离线生成可编辑 DOCX/PPTX 草稿，检查计划时长，拒绝覆盖 |

技能需要 Agent 阅读资料和撰写内容；Python 工具只导出已准备好的 JSON，不是自动生成毕设成果的程序。复杂学校模板由宿主的文档/演示工具处理，基础脚本不能精确套版。没有真实测试记录就不生成通过率；没有 FK 声明就不把推断关系冒充数据库约束。

## 本地运行与验证

Python 3.10+。先检查已有包；若缺失，再在选定环境安装：

```powershell
python -c "import docx, pptx; print('Office dependencies ready')"
# 仅在缺少依赖时执行
python -m pip install -r requirements.txt

python -X utf8 skills/graduation-defense/scripts/materials.py audit examples/mini-deck.json
python -X utf8 skills/graduation-defense/scripts/materials.py pptx examples/mini-deck.json build/答辩草稿.pptx
python -X utf8 skills/graduation-defense/scripts/materials.py docx examples/mini-thesis.json build/论文草稿.docx
python -X utf8 -m unittest discover -s tests -v
```

示例仅用于测试文件导出，包含两页和虚构内容，不是 12–15 页答辩成品。输出默认不进 Git；再次导出请换文件名。完整输入格式与限制见 [导出说明](skills/graduation-defense/references/export.md)，验证记录见 [docs/validation.md](docs/validation.md)。

仓库还包含一次 [15 页完整工作流试用](examples/fictional-defense/README.md)：可编辑 PPTX、输入 JSON、ER/draw.io 图源、练习稿、证据表、测试方案和 10 题评审问答。它使用虚构三表结构，主稿计划 480 秒、精简路线 160 秒；PPT 的图像仍待渲染嵌入，所有真实项目结论均标为待核实，不能作为学生成果直接提交。

结构/回读测试不等于视觉验收，不证明论文事实、文献或学校格式合规。真实交付需要按技能流程补齐证据，并渲染检查。
