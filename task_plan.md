# graduation-defense 实现计划

目标：把用户给出的毕业答辩物料工作流实现为可安装、可复用的 Codex Skill；所有交付进入指定 GitHub 仓库并推送。

范围：Mermaid/可编辑图、12–15 页 PPT 与限时讲稿、学校格式论文、证据约束的评审问答；原文中的 ChatGPT GUI 限制不作为 Codex 能力结论。

- [x] 阅读来源、检测本地依赖与远端权限。
- [x] 编写 Skill 入口、分场景参考与使用示例。
- [x] 实现经过测试的 DOCX/PPTX 导出与讲稿审查工具。
- [x] 验证结构、文件与行为；记录验证边界。
- [ ] 提交并推送 main，核对远端提交。

实现选择：仓库内 `skills/graduation-defense` 为唯一源码；Python 使用已存在的 python-docx/python-pptx，不安装重复依赖；默认自动发现。学校模板缺失时生成可审阅草稿，不声称符合学校格式。PPT 的全篇限时优先于原文逐页 60–90 秒默认值。
