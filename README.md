# Nature Reader Modified

Version: **2.1.1-local.1**

基于 [Yuan1z0825/nature-skills](https://github.com/Yuan1z0825/nature-skills) 的 Nature Reader，保留全文中英对照、图表定位与来源追溯，并增加面向生物医学研究的结构化精读笔记。

## 工作流

1. 区分全文阅读、局部翻译、摘要或来源问答，遵循用户指定范围。
2. 识别 PDF、扫描 PDF、HTML、DOI/arXiv 或粘贴文本，加载对应规则。
3. 全文任务先建立正文、图注、图表和公式来源索引。
4. 逐段翻译，维护术语表，将图表与双语图注放在相关正文附近。
5. 为生物医学全文阅读稿追加六部分科研笔记，区分直接证据与作者推测。
6. 检查来源、素材、缺失记录及公式格式；追问引用已核实的来源位置。

## 定制科研笔记

1. 核心问题与作者回答
2. 技术路线、结果与意义
3. 图表结果和前后逻辑
4. 文献直接证明的结论
5. 作者推测及待验证内容
6. 可能达到期刊水平的原因、优势、劣势与课题启发

每篇论文归档到 `YYYY_first-author_short-title/`，包含 `paper.md`、`source_map.json`、`translation_notes.md` 和 `assets/`。只有明确要求浏览器预览才生成 `reader.html`。

## 安装

将 `skills/nature-reader/` 放入你的 skill 安装目录，并将 `skills/_shared/core/terminology-ledger.md` 放在它的同级 `_shared/core/` 下。保留以下相对结构：

```text
<skills-directory>/
├── nature-reader/
│   ├── SKILL.md
│   ├── manifest.yaml
│   ├── scripts/
│   ├── references/
│   └── static/
└── _shared/core/terminology-ledger.md
```

Codex 通常使用 `~/.codex/skills/`。更新已有安装前先备份；已有共享术语表应先比较，不要盲目覆盖。安装后在新会话中调用 `nature-reader`。

本仓库包含本 skill 的直接共享文件依赖。PDF 提取/OCR 仍需要宿主环境可用的 PDF 工具或 pdf skill；浏览器预览属于可选能力。公式检查使用 Python 3.10+，仅依赖标准库。

## 验证

在本仓库根目录运行：

```sh
python3 skills/nature-reader/scripts/validate_reader_math.py --self-test
```

检查某篇阅读稿：

```sh
python3 skills/nature-reader/scripts/validate_reader_math.py /path/to/paper.md --source-map /path/to/source_map.json --strict
```

公式检查覆盖格式与来源链接，不证明科学内容正确或最终视觉渲染正常。当前版本已通过脚本自测及资源引用检查，尚未完成真实论文端到端验收。

## 来源与本地修改

上游来源及合并记录见 [LOCAL_CUSTOMIZATIONS.md](skills/nature-reader/LOCAL_CUSTOMIZATIONS.md)。原始贡献者署名保留；本地版本增加科研笔记、归档规范，并融合上游范围分流、公式处理、检查脚本及参考资料。发布时仅将本机专用路径改为可移植占位路径，并附带共享术语表。

遵循上游 Apache-2.0 许可证，见 [LICENSE](LICENSE)。
