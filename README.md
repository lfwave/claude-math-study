# kiho-math-study-skill

Claude Code skill that explains mathematical equations with **`\underbrace`-annotated LaTeX** — every meaningful term gets a label underneath, delivered both as a rendered equation and as copy-paste-ready LaTeX.

수식 설명을 요청하면 각 항에 `\underbrace` 주석을 단 수식을 **렌더링용(`$$...$$`)과 복사용(```latex 코드 블록)** 두 형태로 동시에 제공하는 Claude Code 스킬입니다.

## Installation / 설치

### Option A — Claude Code plugin marketplace (recommended)

Type these two commands into the Claude Code prompt, one after the other:

```
'/plugin marketplace add lfwave/kiho-math-study-skill'
```

```
'/plugin install explaining-equations@kiho-math-study-skill'
```

`lfwave/kiho-math-study-skill` is the GitHub repo; `explaining-equations` is the
plugin and `kiho-math-study-skill` is the marketplace name.

### Option B — Manual install (Claude Code desktop app / 데스크탑 앱)

If you use the **Claude Code desktop app** (Mac / Windows) and prefer not to type
commands, install by hand:

**1. Download the skill / 스킬 다운로드**

On this GitHub page, click the green **`Code`** button → **`Download ZIP`**
(or run `git clone https://github.com/lfwave/kiho-math-study-skill.git`).
Unzip it. You only need the `skills/explaining-equations` folder inside.

데스크탑 앱을 쓰고 명령어 입력이 번거롭다면, 이 GitHub 페이지의 초록색 **`Code`**
버튼 → **`Download ZIP`** 으로 내려받아 압축을 풉니다. 그 안의
`skills/explaining-equations` 폴더만 있으면 됩니다.

**2. Paste it into the skills folder / 스킬 폴더에 붙여넣기**

Copy the whole `explaining-equations` folder into your personal Claude skills
directory (create the `skills` folder if it doesn't exist yet):

복사한 `explaining-equations` 폴더를 아래 개인 스킬 폴더에 통째로 붙여넣습니다
(`skills` 폴더가 없으면 직접 만드세요):

| OS | Paste into / 붙여넣을 위치 |
|----|--------------------------|
| Windows | `C:\Users\<사용자명>\.claude\skills\` |
| macOS | `/Users/<username>/.claude/skills/` |
| Linux | `~/.claude/skills/` |

> Tip: in Windows Explorer you can paste `%USERPROFILE%\.claude\skills` into the
> address bar to jump straight there. (`.claude` is a hidden folder — enable
> "Hidden items" in the View menu if you can't see it.)
>
> 팁: 윈도우 탐색기 주소창에 `%USERPROFILE%\.claude\skills` 를 입력하면 바로
> 이동합니다. (`.claude` 는 숨김 폴더라 안 보이면 보기 메뉴에서 "숨긴 항목"을 켜세요.)

The final path should look like
`...\.claude\skills\explaining-equations\SKILL.md`.

최종 경로는 `...\.claude\skills\explaining-equations\SKILL.md` 형태가 되어야 합니다.

**3. Restart / 재시작**

Fully quit and reopen the Claude Code desktop app (or start a new session). The
skill is picked up automatically — no command needed.

Claude Code 데스크탑 앱을 완전히 종료했다가 다시 열면(또는 새 세션 시작) 스킬이
자동으로 인식됩니다.

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
