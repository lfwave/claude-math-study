# kiho-math-study-skill

Claude Code skill that explains mathematical equations with **`\underbrace`-annotated LaTeX** — every meaningful term gets a label underneath, delivered both as a rendered equation and as copy-paste-ready LaTeX.

수식 설명을 요청하면 각 항에 `\underbrace` 주석을 단 수식을 **렌더링용(`$$...$$`)과 복사용(```latex 코드 블록)** 두 형태로 동시에 제공하는 Claude Code 스킬입니다.

## Installation / 설치

### Option A — Claude Code plugin marketplace (recommended)

Inside Claude Code, run:

```
/plugin marketplace add lfwave/kiho-math-study-skill
/plugin install explaining-equations@kiho-math-study-skill
```

`lfwave/kiho-math-study-skill` is the GitHub repo; `explaining-equations` is the
plugin and `kiho-math-study-skill` is the marketplace name.

### Option B — Manual copy

Copy the skill folder into your personal Claude Code skills directory:

```
# Windows
xcopy /E /I skills\explaining-equations %USERPROFILE%\.claude\skills\explaining-equations

# macOS / Linux
cp -r skills/explaining-equations ~/.claude/skills/explaining-equations
```

Restart Claude Code (or start a new session) and the skill is picked up automatically.

## Trigger keywords / 활성화 키워드

The skill activates when your message asks Claude to explain a formula, equation, or expression. The following phrases (and close variants) are the exact triggers registered in the skill description:

| Language | Trigger phrases |
|----------|----------------|
| English | `explain this equation`, `explain this formula`, `what does each term mean`, `break down this formula` |
| 한국어 | `수식 설명해줘`, `이 식이 무슨 뜻이야` |
| 日本語 | `数式を説明して`, `この式の意味は` |
| 中文 | `解释这个公式` |
| Français | `explique cette équation` |
| Deutsch | `erkläre diese Formel` |
| Español | `explica esta fórmula` |

**Language-independent trigger**: pasting LaTeX or any math expression and asking for a term-by-term breakdown also activates the skill, in any language.

> Labels, term explanations, and section headers follow the language of your question — ask in English, get English labels; ask in Korean, get Korean labels.

## What you get / 출력 형식

Every explanation follows a fixed 4-part format:

1. **Original equation** — the plain equation in `$$...$$`
2. **Annotated equation (rendered)** — each meaningful term wrapped in `\underbrace{...}_{\text{label}}`, shown as display math
3. **Annotated equation (copyable)** — the identical LaTeX in a fenced code block, **including the `$$` delimiters**, so you can copy the whole block and paste it straight into Markdown, Notion, Jupyter, or a paper
4. **Term-by-term explanation** — prose walkthrough in the order of the labels

### Example / 예시

Ask: `수식 설명해줘: f(x) = (1/√(2πσ²)) exp(-(x-μ)²/(2σ²))`

You get (abridged):

$$
f(x) = \underbrace{\frac{1}{\sqrt{2\pi\sigma^2}}}_{\text{정규화 상수}} \, \underbrace{\exp\!\left(-\underbrace{\frac{(x-\mu)^2}{2\sigma^2}}_{\text{표준화된 거리}}\right)}_{\text{종 모양 감쇠}}
$$

```latex
$$
f(x) = \underbrace{\frac{1}{\sqrt{2\pi\sigma^2}}}_{\text{정규화 상수}} \, \underbrace{\exp\!\left(-\underbrace{\frac{(x-\mu)^2}{2\sigma^2}}_{\text{표준화된 거리}}\right)}_{\text{종 모양 감쇠}}
$$
```

…followed by a prose explanation of each labeled term.

## Built-in safety rules / 렌더링 안전 규칙

The skill enforces rules that prevent common LaTeX rendering breakage, learned from real failures:

- Word labels are always wrapped in `\text{...}` (prevents CJK glyph breakage in math mode)
- A "dangerous character" table bans characters that break renderers inside math: Unicode middle dot `·` (renders as a literal `\cdotp`), Unicode dashes/ellipsis, unescaped `%` (silently comments out the rest of the line), `&`, `#`, `_`, curly quotes, `<` `>`, and `|` inside Markdown table cells
- Escaping applies **only inside math** — prose keeps natural text like `5%` untouched
- 2–5 underbraces per equation, grouped by meaning (not per symbol); nesting limited to 2 levels
- The rendered and copyable LaTeX are kept character-for-character identical

## Repository layout

```
kiho-math-study-skill/
├── README.md
├── LICENSE
├── .claude-plugin/
│   ├── marketplace.json   # marketplace definition (for /plugin marketplace add)
│   └── plugin.json        # plugin definition (lists the bundled skill)
└── skills/
    └── explaining-equations/
        └── SKILL.md        # the skill itself
```
