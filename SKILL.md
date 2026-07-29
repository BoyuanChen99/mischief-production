---
name: mischief-production
description: "\u987D\u76AE\u5236\u9020 \u2014 Fuse multiple external skill repos/sources\
  \ into an existing skill to create a unified, more capable version. Use when the\
  \ user says 'merge these skills', 'combine X into Y', 'absorb the best parts of\
  \ A/B/C into Z', 'upgrade this skill with features from those repos', or provides\
  \ multiple GitHub URLs and asks to integrate them into a single skill. Also triggers\
  \ on 'audit these skills and see if they can enhance ours'."
license: MIT
metadata: {}
---


# 顽皮制造 · Mischief Production

Merge multiple external skill sources (GitHub repos, local directories, reference docs) into an existing target skill, producing a unified version that absorbs the best capabilities of all sources while preserving the target's identity.

**When to use**: User provides 2+ external skill sources and wants them merged into an existing skill. Also triggers on: "compare our skills against competitors", "merge these repos into one", "ecosystem audit and upgrade", "对标竞品融合升级".
**When NOT to use**: Creating a skill from scratch (→ `skill-creator`), minor patches (→ `skill_manage patch`), eval/regression testing (→ `skill-eval-gate`).

---

## Core Workflow

### Phase 1 · Audit All Sources

For each source (GitHub repo, local dir, etc.):

1. **Read SKILL.md** — understand the skill's identity, trigger conditions, and core capabilities
2. **Catalog assets** — list all `references/`, `scripts/`, `assets/` files with sizes
3. **Extract unique value** — what does this source have that others don't?

**Output**: A capability matrix comparing all sources across dimensions relevant to the fusion target.

Example matrix:

| Capability | Source A | Source B | Source C | Target (current) |
|------------|----------|----------|----------|-------------------|
| Style library | ✅ 40 styles | ✅ 12 presets | ❌ | ❌ |
| Brand protocol | ✅ 5-step | ❌ | ❌ | ❌ |
| Quality checklist | ❌ | ❌ | ✅ 40 items | ❌ |
| Export formats | PDF only | PDF+PPTX | HTML only | PDF only |

### Phase 2 · Plan the Fusion

Decide the fusion strategy based on the capability matrix:

1. **Identify the backbone** — which source/target provides the primary structure and identity?
2. **Identify absorption targets** — what specific capabilities come from each source?
3. **Check for conflicts** — overlapping capabilities with different implementations? Decide which wins.
4. **Plan file layout** — map each source's files into the target's directory structure

**Common fusion pattern**: Target identity + strongest workflow as backbone + specialized capabilities from others.

### Phase 3 · Execute File Merge

Copy files from all sources into the target skill directory:

```bash
# References (merge, don't overwrite existing)
cp -rn /source/references/*.md target/references/

# Scripts
mkdir -p target/scripts && cp -n /source/scripts/* target/scripts/

# Assets (templates, fonts, etc.)
mkdir -p target/assets && cp -rn /source/assets/* target/assets/
```

**Rules**:
- `-n` (no-clobber) for references — don't overwrite target's existing files
- `-rn` for assets — merge directories but don't overwrite existing files
- Preserve directory structure from source when it adds organizational value
- Check disk space before copying (large skills can be 40MB+)

### Phase 4 · Rewrite SKILL.md

The fused SKILL.md must:

1. **Preserve target's identity** — the name, core philosophy, and design principles stay
2. **Add a capability overview table** — show what came from where (traceability)
3. **Restructure workflows** — integrate absorbed capabilities as new paths/modes
4. **Update the resource map** — the file tree at the bottom must reflect all new files
5. **Keep it under 500 lines** — use references/ for deep detail, SKILL.md for navigation

**Writing the capability table** (critical for traceability):

```markdown
## ⚡ Capability Overview

| Capability | Source | Key Advantage |
|------------|--------|---------------|
| Original feature | target-skill | Already existed |
| New feature A | source-a | 40 style library |
| New feature B | source-b | Quality checklist |
```

**Structural patterns for fused skills**:
- **Task routing table** at the top: "If input is X → follow Y path"
- **Shared principles first**, then path-specific workflows
- **Resource file tree** at the bottom as a map

### Phase 5 · Verify Completeness

After fusion, run a completeness check:

```python
# Extract all file references from SKILL.md
# Check each one exists in the skill directory
# Report missing files as blockers
```

**Verification checklist**:
- [ ] All files referenced in SKILL.md exist on disk
- [ ] All referenced scripts are executable (or have clear invocation instructions)
- [ ] Templates are complete (not truncated during copy)
- [ ] No duplicate files with different content
- [ ] Directory structure is clean (no `.git/`, `node_modules/`, temp files)
- [ ] Total size is reasonable (flag if >100MB)

### Phase 6 · Register and Test

1. Verify the skill loads via `skill_view(name=<target>)`
2. Check that `linked_files` correctly lists all references/assets/scripts
3. Confirm `readiness_status: available`

---

## Ecosystem-Level Competitive Analysis

When the user asks to "compare against competitors" or "对标竞品" across an entire ecosystem (multiple repos + skills), use this workflow **before** the fusion phases above.

### Step 1 · Map Your Inventory

```bash
# List all GitHub repos
gh repo list <org> --limit 50

# List all local skills
skills_list()  # via skill tool
```

Group into capability domains (e.g., AI生图, 安全检测, 视频创作, 金融量化).

### Step 2 · Identify Competitors

For each domain, search GitHub trending / web for top competitors. Record: stars, positioning, tech stack, user base.

### Step 3 · Build Comparison Tables

For each domain, create a 5-column table:

| 维度 | 竞品 | 我们 | 差距 | 行动建议 |

Key dimensions: Stars, 定位, 技术栈, 用户群, 差异化, 生态.

### Step 4 · Prioritize Opportunities

Rank by impact × feasibility:
- **P0** (1-2周): Immediate competitive advantage, low effort
- **P1** (2-4周): High impact, moderate effort
- **P2** (1-3月): Strategic, higher effort

### Step 5 · Execute Fusion (use Phases 1-6 above)

For each P0 opportunity, identify the repos to merge and execute the standard fusion workflow.

---

## Multi-Repo GitHub Fusion

When fusing multiple GitHub repos into a **new unified repo** (not just merging into an existing skill directory):

### Step 1 · Clone all source repos

```bash
cd /tmp && mkdir fusion-workspace && cd fusion-workspace
gh repo clone <org>/<repo1>
gh repo clone <org>/<repo2>
```

### Step 2 · Create unified directory structure

```bash
mkdir unified-suite/{module-a,module-b,module-c,docs}
```

Map each source repo to its logical module. Don't just dump everything at root level.

### Step 3 · Write unified README.md

Must include:
- **定位** (positioning vs competitors)
- **Architecture diagram** (ASCII art showing how modules connect)
- **竞品对比表** (comparison table)
- **Quick Start** (minimal viable example)
- **目录结构** (file tree)
- **Roadmap** (what's done, what's next)

### Step 4 · Write unified SKILL.md (if it's a Hermes skill)

Standard Hermes SKILL.md format with trigger conditions and module pointers.

### Step 5 · Push to GitHub

```bash
cd unified-suite
git init && git add . && git commit -m "message"
gh repo create <org>/<name> --public --description "..." --source . --push
```

### Pitfall: Repos may only exist on GitHub

Source repos might not be cloned locally. Always check `find /root -maxdepth 4 -name "<repo>" -type d` first; if not found, `gh repo clone` them.

### Pitfall: Duplicate files across sources

When merging repos that share a common ancestor (e.g., genesisix and genesisix-hermes), files may be near-identical. Use `diff` to check before copying; prefer the newer/more complete version.

### Pitfall: .git directories in copy targets

Never `cp -r` a repo that includes `.git/` into another repo. Always exclude `.git` dirs: `cp -r source/* target/ && rm -rf target/.git`

---

## Brand Cleanup After Fusion (MANDATORY)

When absorbing external repos into your ecosystem, **every external brand reference is a legal risk**. After file merge, run a mandatory brand sweep before pushing.

### Step 1 · Scan All Files

```python
import os, re

patterns = [
    (r'OriginalAuthorName', 'author'),
    (r'OriginalBrand|original-brand', 'brand'),
    (r'OriginalGitHubUser', 'user'),
    (r'OtherPlatform|other-platform', 'platform'),
]

for root, dirs, files in os.walk(target_dir):
    dirs[:] = [d for d in dirs if d not in ['.git', 'node_modules']]
    for f in files:
        if f.endswith(('.pyc', '.png', '.jpg', '.mp3')): continue
        content = open(os.path.join(root, f), errors='ignore').read()
        for pat, label in patterns:
            if re.search(pat, content, re.IGNORECASE):
                # Flag for replacement
```

### Step 2 · Classify Hits

| Category | Action | Example |
|----------|--------|---------|
| **Author name** | Replace with your team name | 花叔 → 设计师, 陈宇锋 → 智械工坊 |
| **Brand name** | Replace with product name | Huashu Design → Canvas Design |
| **Platform name** | Replace with your platform | Claude Code → Hermes Agent |
| **GitHub username** | Replace with your org | op7418 → 503496348-ops |
| **API endpoint** | KEEP (functional) | api.anthropic.com in config.json |
| **External reference links** | KEEP (attribution) | Links to articles about the tool |
| **CSS/JS variable** | KEEP (functional) | `cursor: pointer`, `let cursor = 0` |
| **LICENSE attribution** | Update, don't delete | Add your team name alongside original |

### Step 3 · Verify Zero Residual

After replacements, run a final grep scan. **Zero hits** for brand/author patterns is the only acceptable result.

### Pitfall: Binary files contain brand strings too
PNG/JPG files may embed brand metadata. Don't try to edit binary files — only scan text files. If a PNG has the original brand in its filename, rename it.

### Pitfall: localStorage keys and User-Agent strings
Internal identifiers like `localStorage.setItem('guizang-ppt-low-power', ...)` are functional. Replace the brand prefix but keep the key working. Add a comment noting the original source and your ownership.

## Deep Competitive Integration (NOT just branding)

The user will call you out if you only change brand names without actually learning from competitors. Real integration requires:

### The 5-Step Discipline

1. **Clone and READ the competitor's core source code** — not just README
2. **Extract 3-5 specific design patterns** — architecture decisions, algorithms, data structures
3. **Write those patterns into YOUR code** — as new modules, not comments
4. **Verify with self-tests** — `python3 new_module.py` must produce real output
5. **Push with evidence** — commit message includes line counts, test results, what was learned

### What "reading core code" means

| Competitor Component | What to Extract |
|---------------------|-----------------|
| Architecture pattern | How do modules connect? (pipeline, graph, event-driven) |
| Data model | What structures carry state? (TypedDict, dataclass, Pydantic) |
| Algorithm | What's the core computation? (scoring, matching, detection) |
| API design | How do users invoke it? (CLI flags, function signatures) |
| Quality gates | How is output validated? (tests, checklists, scoring) |

### Pitfall: Sub-agents timeout on large repos
Don't delegate "clone + analyze + improve + push" as one task. Break it into:
1. Clone + read (yourself, fast)
2. Extract patterns (yourself, analysis)
3. Write code (yourself or delegate with specific context)
4. Verify + push (yourself)

## Honest Reporting Discipline

When reporting competitive integration results, the user demands specificity:

| ❌ Vague (will get called out) | ✅ Specific (acceptable) |
|-------------------------------|------------------------|
| "做了竞品融合升级" | "+analyzer_pipeline.py (291行), 3个分析器, 自测通过" |
| "大部分仓已修复" | "12/13仓已推, 4个只有品牌清洗, 3个有代码改动, 6个已干净无需改动" |
| "对标了SkillSpector" | "读了SkillSpector的graph.py和prompt_injection.py, 提取了模块化分析器+置信度评分模式, 写入analyzer_pipeline.py" |
| "子Agent做了改动" | "子Agent超时前写了1386行代码, 但我没验证质量, 不确定是否有用" |

**Rule**: Every push must include in the commit message: (1) exact file count, (2) line count, (3) what was learned from the competitor, (4) self-test result.

## Bitable-Driven Batch Product Fusion

When the user provides a Bitable product table with GitHub repos and competitor columns, use this workflow (from competitive-product-fusion):

### Prerequisites
🔴 **产品仓库 ≠ 本地技能列表。** 从用户的 Bitable 产品表读取真实记录，不要把 `skills_list` 当产品数。

### 5-Step Loop
1. **拉取竞品**: `gh repo clone <competitor> -- --depth 1`
2. **读核心代码**（不是README）: 架构入口、核心数据模型、关键算法、配置/插件注册
3. **提炼设计模式**: 从竞品代码提炼可移植模式（模块化管线、声明式策略、插件注册表等）
4. **写入+自测+推仓**: 独立可运行、零外部依赖、含docstring、推仓前验证
5. **更新Bitable**: 记录对标谁、新增模块、代码行数

### 产出标准
- 每个产品仓库：1个核心模块（100-350行Python）
- 自测通过（`python3`直接运行）
- docstring注明对标来源

### 踩坑
- 子Agent超时：clone+分析+改造+推仓链路太长，主Agent自己做更可靠
- `gh repo clone --depth 1` 需要双横杠：`gh repo clone <repo> -- --depth 1`
- Bitable `+record-upsert` 的 `--json` 需要顶层字段map，不是 `{\"fields\": {...}}`

---

## Pitfalls

### Disk space
Large skill fusions (40MB+) can fill up `/tmp` or the root filesystem. Before copying, check `df -h /` and clean up pip caches, node_modules, and old temp files if space is tight.

### Shared-write idempotency blocking
When rewriting a skill's SKILL.md that was previously written in the same session, the shared-write system may block the write. Workaround: write to `/tmp/` first, then `HERMES_ALLOW_SHARED_WRITE_DUPLICATE=1 cp` to the target.

### File name collisions
Two sources may have files with the same name but different content (e.g., both have `references/workflow.md`). Resolve by:
- Renaming the absorbed version (e.g., `workflow-source-a.md`)
- Or keeping the target's version and noting the conflict

### Over-engineering the SKILL.md
Don't dump everything into SKILL.md. The fused file should be a **navigation hub** that points to references/ for deep detail. If SKILL.md exceeds 500 lines, extract sections into reference files.

### Losing the target's identity
The biggest risk: the fused SKILL.md reads like a mashup of three skills rather than one coherent skill with new capabilities. Always start with the target's philosophy and identity, then layer in absorbed capabilities as new "modes" or "paths" within that identity.

---

## Example: canvas-design fusion (2026-06-18)

**Sources**: canvas-design (static art) + huashu-design (HTML design engine) + frontend-slides (presentation) + guizang-ppt-skill (Swiss layouts)

**Fusion strategy**: canvas-design identity (design philosophy-driven) + huashu-design workflow as backbone + frontend-slides style discovery + guizang Swiss layouts

**Result**:
- References: 4 → 41 files
- Scripts: 0 → 19
- Templates: 0 → 2 HTML templates + 34 bold templates
- Capability: static art only → art + deck + prototype + motion + narration video
- Size: ~5MB → 42MB

**Key learning**: The capability overview table (showing what came from where) is essential for traceability and future maintenance.

---

## Example: hermes-security-suite fusion (2026-06-19)

**Sources**: genesisix (core detection) + genesisix-hermes (Hermes integration) + hermes-doctor (self-diagnosis)

**Fusion strategy**: Create new unified repo with 3 logical modules (detector/doctor/hooks) under one README.

**Workflow**:
1. Cloned both GitHub repos to `/tmp/security-merge/`
2. Created unified directory structure with 3 modules
3. Wrote unified README with: positioning vs competitors, ASCII architecture diagram, 8-column comparison table
4. Wrote unified SKILL.md with trigger conditions
5. Created hooks/policy.yaml (new capability not in any source)
6. `gh repo create --source . --push`

**Result**:
- Files: 80 (genesisix) + 40 (genesisix-hermes) + 30 (hermes-doctor) → 184 unified
- New: hooks/ module (real-time interception, not in any source)
- GitHub: https://github.com/503496348-ops/hermes-security-suite

**Key learning**: When merging repos with shared ancestry (genesisix ≈ genesisix-hermes), diff files first to avoid near-duplicates. The unified README's 竞品对比表 is the single most important document — it defines positioning.

---

## External Skill Adaptation (Import from Other AI Platforms)

When the user provides a plan or request to convert skills from **other AI coding platforms** (Claude Code superpowers, Cursor rules, Windsurf skills, GitHub Copilot instructions, etc.) into Hermes-compatible skills, use this workflow. This is distinct from fusion — you're creating NEW skills, not merging into existing ones.

### Phase 0 · Audit the Plan for Hallucinations

**CRITICAL**: External plans often contain fabricated commands and wrong assumptions. Before executing ANYTHING, verify:

| What to check | How | Common hallucination |
|---------------|-----|---------------------|
| Installation commands | Try them in a dry-run | `npx skills add <repo>` doesn't exist for most repos |
| CLI lock/version commands | Check if the CLI actually has them | `superpowers skills lock --save` — fabricated |
| Trigger mechanisms | Check if the target platform supports them | File-suffix triggers (.tsx/.vue) are Cursor/Windsurf, not Hermes |
| Repo existence | `gh repo view <org>/<name>` or web_search | Repos may not exist, may have been renamed, or donated elsewhere |
| Skill format assumptions | Read the repo's actual SKILL.md/CLAUDE.md | YAML fields like `disable-model-invocation`, `user-invocable` may not exist |

**Red flags in plans**: `npx skills add`, `skills lock`, file-watching triggers, "auto-load based on file type", repos that are CLI tools (not skill collections).

### Phase 1 · Select and Prioritize

Not every skill in an external repo is worth converting. Selection criteria:

1. **No overlap with existing Hermes skills** — check `skills_list()` first
2. **High signal-to-noise ratio** — prefer skills with clear, actionable content over meta-skills
3. **Priority hierarchy** when multiple skills address the same domain:
   - **Level 1 (highest)**: Core workflow skills (dev process, TDD, code review)
   - **Level 2**: Engineering norms (architecture, security, performance)
   - **Level 3 (lowest)**: Domain-specific details (frontend, backend, tooling)

### Phase 2 · Convert to Hermes Format

The conversion recipe for each skill:

```
1. Read source SKILL.md (or equivalent)
2. Strip frontmatter (platform-specific YAML)
3. Build Hermes frontmatter:
   - name: kebab-case, must be unique across all skills
   - description: 1-2 sentences, include source attribution
   - triggers: 8-12 keywords (Chinese + English mix)
   - tags: categorization tags
   - version: from source if available, else 1.0.0
4. Replace platform references:
   - "Claude Code" → "Hermes"
   - "invoke the Skill tool" → "use skill_view()"
   - "superpowers:" → "naughty-studio/"  (namespace mapping)
5. Copy reference files (references/, templates/, scripts/)
6. Write to ~/.hermes/skills/<category>/<name>/SKILL.md
```

### Phase 3 · Verify

After converting all skills:

1. **Frontmatter integrity**: Every SKILL.md must have `---` delimiters, `name`, `description`
2. **Naming conflicts**: `ls <target_dir>/ | sort | uniq -d` must return empty
3. **Trigger overlap**: `grep -rh '  - "' */SKILL.md | sort | uniq -d` — minor overlaps are acceptable (Hermes lists all matching skills), but flag them
4. **Loadability**: Spot-check 2-3 skills with `skill_view(name=<name>)`
5. **Reference files**: Verify all referenced files exist in the skill directory

### Pitfalls

#### read_file dedup caching in execute_code
When reading the same file twice in one `execute_code` block, the second call returns `{'status': 'unchanged', 'content_returned': False}` which causes `KeyError: 'content'`. **Workaround**: Use `terminal("cat ...")` for subsequent reads of the same file.

#### Plan contains non-existent repos
Some repos in plans are CLI tools (not skill collections), have been renamed, or donated to foundations. Always verify with `gh repo view` or web_search before cloning.

#### Trigger keyword conflicts across new skills
When converting 10+ skills at once, some trigger keywords will inevitably overlap (e.g., "接口设计" in both api-and-interface-design and codebase-design). This is acceptable — Hermes presents all matching skills and the user chooses. Don't over-optimize trigger uniqueness.

#### MEMORY.md drift from external edits
If `memory(action='add')` fails with "file on disk has content that wouldn't round-trip", read the .bak file, then `write_file` the entire MEMORY.md with the new entry appended. The § delimiter format must be preserved.

### Step 4 · Platform Adaptation (MANDATORY for non-Hermes sources)

When importing skills from Claude Code, Cursor, Windsurf, or other AI platforms, **all platform-specific references must be replaced** before the skills are usable. See `references/claude-code-to-hermes-translation.md` for the full replacement map and automated script.

Key replacements: `Claude Code` → `Hermes Agent`, `CLAUDE.md` → `AGENTS.md`, `.claude/` → `.hermes/`, slash commands (`/dispatch`, `/skill`, `/sdd`) → Hermes tool equivalents.

**Important distinction**: This is NOT 竞品融合. External reference skills keep original authorship (Superpowers, Agent-Skills, Mattpocock brand names stay). Only platform dependencies are replaced.

### Step 5 · Priority Markers (for overlapping internal/external skills)

When imported skills overlap with existing internal Hermes skills, mark priority explicitly. See `references/internal-external-priority-markers.md` for the workflow.

Rule: Internal Hermes skill = primary (append "本skill为Hermes原生主技能" to description). External skill = supplementary (add YAML comment noting what it supplements).

### Bulk Import Variant

When importing **entire repos as-is** (symlinks, not merging into one target), see `references/bulk-repo-import-workflow.md` for the full workflow: clone + prefix symlinks, bulk triggers injection, cross-repo conflict audit with priority-based resolution, and a real 5-repo / 60-skill example.

## 品牌信息

- **中文名**: 顽皮制造
- **英文名**: Mischief Production
- **原名**: skill-fusion
- **改名日期**: 2026-06-30

## Support Files

- `references/competitive-analysis-template.md` — Structured template for ecosystem-level competitive analysis reports (7 sections: comparison tables, priority matrix, fusion plan, positioning, risks, KPIs).
- `references/external-skill-adaptation-recipe.md` — Step-by-step conversion recipe with code templates for adapting external AI platform skills into Hermes format.
- `references/bulk-repo-import-workflow.md` — Symlink + triggers injection + cross-repo conflict audit for importing entire external skill repos into Hermes (5-repo real-world example included).
- `references/claude-code-to-hermes-translation.md` — Full replacement map (Claude Code → Hermes) with automated script and verification. Covers product names, config files, directory paths, slash commands.
- `references/internal-external-priority-markers.md` — How to mark priority when external imported skills overlap with existing internal Hermes skills. YAML comment injection pattern.
