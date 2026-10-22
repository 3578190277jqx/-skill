# 离线基础导出工具

`scripts/materials.py` 接受模型依据真实资料编写的 JSON，负责检查字段、计划时长并导出基础文件，不会自行阅读仓库、写论文或证明结论真实。Python 3.10+；`audit` 仅标准库，DOCX 需 `python-docx`，PPTX 需 `python-pptx`。先检测本地或宿主提供的运行时，不默认安装依赖。包缺失时报告所缺项。

以下命令在技能目录执行；其他位置使用脚本绝对路径：

```powershell
python -X utf8 scripts/materials.py audit deck.json
python -X utf8 scripts/materials.py pptx deck.json output/答辩草稿.pptx
python -X utf8 scripts/materials.py docx thesis.json output/论文草稿.docx
```

不覆盖已存在文件；输出父目录可自动创建。校验错误退出码 2。工具只读输入及明确指定的本地图片，没有网络请求、外部上传、安装或删除逻辑。

## PPT 输入

```json
{
  "title": "实际项目名称",
  "duration_seconds": 480,
  "short_route": [1, 2],
  "slides": [
    {
      "title": "背景与目标",
      "bullets": ["有依据的背景", "明确的需求", "可核实的目标"],
      "notes": "本页完整口播稿。",
      "seconds": 240,
      "short_notes": "本页精简口播稿。",
      "short_seconds": 90,
      "evidence": ["材料路径及页码"],
      "image": "figures/architecture.png"
    },
    {
      "title": "局限与展望",
      "bullets": ["明确尚未验证部分"],
      "notes": "本页完整口播稿。",
      "seconds": 240,
      "short_notes": "本页精简口播稿。",
      "short_seconds": 90,
      "evidence": []
    }
  ]
}
```

这是最小格式示例，不是符合 12–15 页的完整答辩。图片可省略；相对路径以 JSON 所在目录为基准，PPT 基础工具只接受 PNG/JPEG。所有 slide 必须有非空 title、字符串数组 bullets、非空 notes 和正整数 seconds。`evidence` 为字符串数组，可为空；`short_route` 可省略，给出时为不重复的 1 起始页码，该路线每页须有正整数 `short_seconds`，合计不超过 180。要生成可用精简口播，模型还必须填入 `short_notes`，不能只设置时间。

正文 3–5 要点、每点 20 字以及 12–15 页只给警告，适配用户覆盖和图页/封面例外；模型应解释警告是否合理。总计划时长必须与 `duration_seconds` 精确相等。工具不按讲稿长度验证实际口播时长。

基础 PPT 使用 16:9、深蓝标题和白底。备注区保存口播稿、计划秒数、证据与可选精简稿；图片等比缩放。工具不导入学校母版、不渲染 Mermaid、不自动分配讲稿时间、不自动排布复杂图表。学校模板或精细视觉需求使用完整演示工具，不要拿本导出结果声称精确套版。

## 论文输入

```json
{
  "title": "实际论文题目",
  "chapters": [
    {"title": "第一章 绪论", "paragraphs": ["有依据的正文段落。"]}
  ],
  "references": ["已核实的完整文献条目，按学校规范排好编号"]
}
```

章节 title、非空 paragraphs 必填；references 为字符串数组，可为空。仅支持这些字段，出现图表、公式或其他未知字段会报错，避免静默丢失内容。输出 A4、2.54 厘米页边距、宋体 12 磅、1.5 倍行距，仅为草稿默认。不会为缺失引用补造条目；不具备模板、图表、公式、自动目录或域支持。复杂论文使用文档工具或基于模板编写的专门脚本。

## 验证边界

先 `audit`，再生成并用 Office 库重开；图像、备注、段落都应存在。实际交付还需渲染后目检；脚本退出成功不能证明排版无误、文献真实、学校格式合规或毕设成果成立。
