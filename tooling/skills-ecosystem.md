---
name: skills-ecosystem
description: >
  Catalog of Claude Code research/science skills and plugins ecosystem. Covers: Anthropic official skills
  (16 skills, document/design/meta), K-Dense scientific skills (148+ skills, 250+ databases for bioinformatics,
  cheminformatics, clinical research, ML, literature), MCP servers for research (Zotero, arXiv, Scholar),
  community skill collections (awesome-claude-skills 33k stars, Superpowers 16k stars), and agentic research
  frameworks (AgenticSciML, LiRA, AI-Researcher). Use when installing new skills, evaluating tool options,
  or setting up new research projects.
survey_date: 2026-03-02
lang: en
---

# Claude Code Research/Science Skills & Plugins Ecosystem

## 1. Official Anthropic Skills

**Repo**: [anthropics/skills](https://github.com/anthropics/skills) (~29k stars)

### Available Skills (16 official)
| Skill | Purpose |
|-------|---------|
| `docx` | Word document creation/editing |
| `pdf` | PDF processing |
| `pptx` | PowerPoint generation |
| `xlsx` | Excel spreadsheet handling |
| `skill-creator` | Meta-skill for creating new skills |
| `mcp-builder` | Build MCP servers |
| `webapp-testing` | Test web applications |
| `frontend-design` | UI/UX design |

**Note**: No dedicated research/science skill in official repo.

---

## 2. K-Dense Scientific Skills (RECOMMENDED)

**Repo**: [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) (~8.6k stars)

148+ skills, 250+ databases. BixBench: 29.2% accuracy (vs GPT-5: 22.9%).

### Key Categories
| Category | Examples |
|----------|---------|
| Bioinformatics | BioPython, Scanpy, Cellxgene Census |
| Cheminformatics | RDKit, DeepChem, DiffDock |
| Clinical Research | ClinicalTrials.gov, ClinVar, COSMIC |
| Literature | PubMed, OpenAlex, bioRxiv |
| ML | PyTorch Lightning, scikit-learn, SHAP |

### Installation
```bash
git clone https://github.com/K-Dense-AI/claude-scientific-skills.git
cp -r claude-scientific-skills/scientific-skills/* .claude/skills/
```

---

## 3. MCP Servers for Research

| Server | Repo | Purpose |
|--------|------|---------|
| **Zotero MCP** | [54yyyu/zotero-mcp](https://github.com/54yyyu/zotero-mcp) | Library access |
| **arXiv MCP** | [blazickjp/arxiv-mcp-server](https://github.com/blazickjp/arxiv-mcp-server) | Preprint search |
| **Scholar MCP** | [han-517/scholar-mcp](https://github.com/han-517/scholar-mcp) | Unified paper search |

---

## 4. Community Collections

| Collection | Stars | Highlights |
|-----------|-------|-----------|
| [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | ~33k | 78+ SaaS integrations |
| [Superpowers](https://github.com/obra/superpowers) | ~16k | Full dev workflow |
| everything-claude-code | ~50k | 13 Agents, 40+ Skills |

---

## 5. Agentic Research Frameworks

| Framework | Source | Focus |
|-----------|--------|-------|
| AgenticSciML | Brown University, arXiv:2511.07262 | Scientific ML discovery |
| LiRA | arXiv:2510.05138 | Reliable scientific writing |
| AI-Researcher | arXiv:2505.18705 | Autonomous scientific innovation |

---

## Sources

- [anthropics/skills](https://github.com/anthropics/skills)
- [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills)
- [agentskills.io](https://agentskills.io/)
- [Skills Marketplace](https://skillsmp.com/zh) - 58,000+ skills
