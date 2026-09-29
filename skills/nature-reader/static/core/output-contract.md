# Output contract

Prefer these outputs:

- `paper.md` for the full-paper Markdown artifact
- `source_map.json` for stable source anchors
- `translation_notes.md` for terminology, uncertainty, and layout notes
- `assets/` for extracted figures, tables, and equation crops when needed
- `reader.html` only when the user explicitly wants a browser preview

Do not hide missing information. If the source is incomplete, label the output as draft mode.

## Output folder organization

Create one dedicated output folder per paper. Put every generated artifact for that paper inside this folder, including `paper.md`, `source_map.json`, `translation_notes.md`, optional `reader.html`, and the nested `assets/` directory.

Name the paper folder with the publication year at the beginning:

- Preferred pattern: `YYYY_first-author_short-title`
- Acceptable pattern when first author is unclear: `YYYY_journal_short-title`
- Example: `2024_Wang_T_cell_exhaustion_lung_cancer`

Use the publication year from the paper metadata, DOI page, journal page, PDF first page, or citation record. If the year cannot be verified after reasonable inspection, ask the user to confirm the year before creating the final folder; for an unavoidable draft, use `0000_year-unverified_short-title` and record the uncertainty in `translation_notes.md`.

Keep folder names stable and filesystem-safe: avoid slashes, colons, quotes, and very long titles; use underscores for spaces.

## Structured biomedical research note

When the paper is biomedical, medical, translational, omics, clinical, or method-oriented, append a final section to `paper.md` titled `# 结构化科研笔记`.

Keep this section source-grounded and conservative. Separate what the paper directly demonstrates from what the authors infer, hypothesize, or frame as future implication. When the evidence is only associative, computational, exploratory, preclinical, or based on limited samples, say so plainly.

Use this exact section order:

1. `## 1. 本文试图回答的核心问题是什么？回答分别是什么？`
   - Use a table with columns: `类型`, `核心问题`, `作者给出的回答`, `主要证据来源`, `证据强度`, `需要保留的不确定性`.
   - Include rows as applicable for biological, medical/clinical, methodological, and translational questions.
2. `## 2. 本文使用了什么技术路线？分别获得了什么结果？这些结果意味着什么？`
   - Use a table with columns: `技术路线/实验模块`, `干实验或湿实验方法`, `样本/模型/数据来源`, `得到的主要结果`, `结果意味着什么`, `局限或注意事项`.
   - Distinguish wet-lab experiments, computational analyses, clinical cohorts, public datasets, animal/cell models, and validation experiments.
3. `## 3. 图片展示的结果和逻辑关系`
   - Use a table with columns: `图/表`, `这一图想回答的问题`, `使用的数据或方法`, `关键观察结果`, `作者据此得出的结论`, `与前后图的逻辑关系`.
   - Cover all main figures and important supplementary figures/tables when available.
4. `## 4. 文献直接证明的结论`
   - Use a table with columns: `直接结论`, `支撑证据`, `对应图表/段落`, `证据类型`, `可信度备注`.
   - Include only conclusions directly supported by presented data.
5. `## 5. 作者推测的内容`
   - Use a table with columns: `作者推测或解释`, `基于哪些结果推测`, `为什么还不是直接证明`, `后续需要什么实验或数据验证`.
   - Include mechanistic interpretations, clinical implications, causal language, and future applications that are not directly proven.
6. `## 6. 这篇文章为什么能发表在这个杂志？优势和劣势是什么？`
   - Include subsections:
     - `### 6.1 可能达到该期刊水平的原因`
     - `### 6.2 主要优势`
     - `### 6.3 主要劣势或可改进之处`
     - `### 6.4 对我自己课题的启发`
   - Evaluate importance, novelty, evidence completeness, mechanistic depth, cohort/data scale, translational value, figure narrative, limitations, and actionable lessons for the user's research.

If the user provides a different note template in the request, follow the user's template instead of this default structured note.

## Pre-response verification

For full-reader deliverables, verify before final response (explicit excerpts, summaries, and source-linked questions do not require a full reader or the six-part note):

- all generated files for one paper are inside its year-prefixed dedicated folder
- biomedical full readers include the final `# 结构化科研笔记` with all six sections unless the user disables it or supplies another template

- `paper.md` contains `**Original:**` and `**中文:**` block pairs
- every image/table link used in `paper.md` exists under `assets/`
- every figure/table in `assets/` has a corresponding Markdown block and source pointer
- display equations render inside `$$...$$` (or a fenced `math` block), and inline equations render inside `$...$`
- mathematical content is unchanged across the bilingual explanation: only prose is translated, each display equation is shown once, and Chinese text never uses `(I_0)`-style pseudo-math
- no bare LaTeX commands such as `\\frac`, `\\sum`, or `\\begin{...}` appear as ordinary prose
- every display equation has an `E...` anchor and a matching equation entry in `source_map.json`
- every low-confidence or image-only equation points to an existing file under `assets/equations/`
- `source_map.json` parses as JSON and includes source block IDs
- `translation_notes.md` records skipped, uncertain, or draft-mode content

Run the deterministic math check before delivery:

```bash
python3 "<nature-reader-skill-dir>/scripts/validate_reader_math.py" paper.md --source-map source_map.json
```

Add `--strict` for a published or reusable artifact. The command checks delimiters, bare LaTeX, equation IDs, source-map linkage, and equation fallback paths.

## Tooling guidance

- If the input is a PDF, load the `pdf` skill first for extraction and OCR guidance.
- If the user asks for a richer browser view, use `web-artifacts-builder` or `frontend-design` only as a preview layer on top of the Markdown workflow.
- If the user wants citation-level grounding to original text, keep the source map explicit and do not lose the page or block IDs.

`<nature-reader-skill-dir>` is a placeholder: resolve it to the absolute directory containing this skill’s SKILL.md before running the command. Run from the paper output directory, or supply absolute paper/source-map paths.
