> ⚠️ This version was translated by AI.

<div align="center">
<h2> 📄 Agent Context Map </h2>

<b>How can you make sure the Agent has seen your project rules?</b>

<sub>This is a comparison table of Agent context loading behavior.<br>
It organizes the scope and priority of mainstream Agent instructions, including project instructions, user-level configuration, repository rules, skills, plugins, and README.md.<sub>
<br>

<p>
  <a href="./README.md">Simplified Chinese</a> ·
  <b>English</b> ·
  <a href="https://github.com/cclilshy/agent-context-map/issues">Submit Issue</a>
</p>

</div>

<hr>

## 🤔 What problem does it help me solve?

> Agent tools have entered daily development workflows, but the way they read project context is not always intuitive. When implementing Agent engineering management, common misjudgments include the following:

_Which files will the Agent read?_  
_Which directories are considered valid scopes?_  
_Should I install plugins under `$HOME` or `$PROJECT_PATH`?_  
_How do skills and plugins enter the context?_  
_Will `README.md` be read by default?_

</p>

The vendors of these tools have usually already considered developers in their designs, making tool behavior generally `intuitive`, but if you rely only on intuition, misjudgments can still easily occur. Therefore, I believe that when conducting Agent engineering governance, you should not simply rely only on `guessing` or `intuition` as the basis.

<a id="verified-version"></a>

## Verified Version

> This document prioritizes official documentation as evidence.

- [Codex CLI](#codex): `0.135.0`
- [Claude Code](#claude-code): `none`
- [Gemini CLI](#gemini-cli): `none`
- [Cursor](#cursor): `none`

**Currently, only the behavior description of the "latest verified version" is maintained**

## Codex

### Project Root

The `project_root_markers` configuration determines the project root. When not configured, `.git` is used by default; when no marker is found, only the current working directory is used; an empty array disables parent directory lookup.

<sub>References: [Documentation][codex-config-doc] / [Source][codex-agents-src] / [Source][codex-root-src]</sub>

### Project Instructions

| File Source                             | Description                                    |
| --------------------------------------- | ---------------------------------------------- |
| `$CODEX_HOME/AGENTS.override.md`        | User-level override instructions               |
| `$CODEX_HOME/AGENTS.md`                 | User-level default instructions                |
| `AGENTS.override.md`                    | Project-level override instructions            |
| `AGENTS.md`                             | Project-level default instructions             |
| `config:project_doc_fallback_filenames` | Candidate project instruction filenames in the configuration |

**When `AGENTS.md` and `AGENTS.override.md` both exist, the latter overrides the former**

<sub>References: [Documentation][codex-agents-doc] / [Source][codex-agents-src]</sub>

### SKILLS

| File Source                  | Description                 |
| ---------------------------- | --------------------------- |
| `.agents/skills/**/SKILL.md` | In-project skills           |
| `.codex/skills/**/SKILL.md`  | Project configuration-layer skills |
| `$HOME/.agents/skills`       | User-level skills           |
| `/etc/codex/skills`          | System-level skills         |
| `$CODEX_HOME/skills/.system` | Codex built-in system skills |
| Plugin declarations          | Skills provided by plugins  |

**Skills with the same name are not considered the same `SKILL`; `Codex` distinguishes them only by the absolute file path**

<sub>References: [Documentation][codex-skills-doc] / [Source][codex-skills-src]</sub>

### README.md

| File Source | Description       |
| ----------- | ----------------- |
| `README.md` | Repository README |

`README.md` is not read by default

> You can add `README.md` to `project_doc_fallback_filenames` so it is read as a candidate project instruction file.

<sub>References: [Documentation][codex-agents-doc] / [Source][codex-agents-src]</sub>

### Related Configuration

#### `project_root_markers`

> Marker directories, default value is `[".git"]`

```toml
# ~/.codex/config.toml
project_root_markers = [".git", ".hg", ".sl"]
```

#### `project_doc_fallback_filenames`

> Project instruction filenames

```toml
# ~/.codex/config.toml
project_doc_fallback_filenames = ["CLAUDE.md", "GEMINI.md", "README.md"]
```

#### `project_doc_max_bytes`

> Project-level instruction length limit

```toml
# ~/.codex/config.toml
project_doc_max_bytes = 32768
```

## Claude Code

> ignore

## Gemini CLI

> ignore

## Cursor

> ignore

## License

This document is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa-4]. You may copy, distribute, adapt, and use it commercially, but you need to retain attribution, the license link, and indicate whether modifications were made. When publicly publishing adaptations, translations, or derivative versions, you must use the same or a compatible license.

[cc-by-sa-4]: https://creativecommons.org/licenses/by-sa/4.0/
[codex-agents-doc]: https://developers.openai.com/codex/guides/agents-md
[codex-config-doc]: https://developers.openai.com/codex/config-advanced#project-root-detection
[codex-project-instructions-doc]: https://developers.openai.com/codex/config-advanced#project-instructions-discovery
[codex-config-reference-doc]: https://developers.openai.com/codex/config-reference
[codex-skills-doc]: https://developers.openai.com/codex/skills
[codex-agents-src]: https://github.com/openai/codex/blob/rust-v0.135.0/codex-rs/core/src/agents_md.rs
[codex-root-src]: https://github.com/openai/codex/blob/rust-v0.135.0/codex-rs/config/src/project_root_markers.rs
[codex-skills-src]: https://github.com/openai/codex/blob/rust-v0.135.0/codex-rs/core-skills/src/loader.rs
[codex-tag]: https://github.com/openai/codex/tree/rust-v0.135.0
