<div align="center">
<h2> 📄 Agent Context Map </h2>

<b>如何确保 Agent 看到了你的项目规则?</b>

<sub>这是一份 Agent 上下文加载行为的对照表。<br>
它整理了主流 Agent 指令作用域范围与优先级，包括项目指令、用户级配置、仓库规则、skills、插件和 README.md。<sub>
<br>

<p>
  <b>简体中文</b> ·
  <a href="./README.en.md">English</a> ·
  <a href="https://github.com/cclilshy/agent-context-map/issues">提交 Issue</a>
</p>

</div>

<hr>

## 🤔 它帮助我解决问么问题?

> Agent 工具已经进入日常开发流程，但它们读取项目上下文的方式并不总是直观。在落地 Agent 工程管理时，常见误判包括如下：

_Agent 会读取哪些文件?_  
_哪些目录被认为是有效作用域?_  
_我应该把插件安装在 `$HOME` 还是 `$PROJECT_PATH`?_  
_skills 和插件如何进入上下文?_  
_`README.md` 是否会被默认读取?_

</p>

这些工具的供应商通常已经在设计上照顾开发者，让工具行为基本上 `符合直觉`，但如果只凭直觉使用，仍然容易产生误判。所以我认为在进行Agent工程治理时，不应该单纯只靠 `猜测` 或`符合直觉` 来作为依据。

<a id="检验版本"></a>

## 检验版本

> 本文优先采用官方文档作为证据。

- [Codex CLI](#codex)：`0.135.0`
- [Claude Code](#claude-code)：`none`
- [Gemini CLI](#gemini-cli)：`none`
- [Cursor](#cursor)：`none`

**目前只维护 “最新已验证版本” 的行为说明**

## Codex

### 项目根

`project_root_markers` 配置决定项目根。未配置时默认使用 `.git`；未找到 marker 时，仅使用当前工作目录；空数组会禁用父目录查找。

<sub>参考：[文档][codex-config-doc] / [源码][codex-agents-src] / [源码][codex-root-src]</sub>

### 项目指令

| 文件来源                                | 说明                       |
| --------------------------------------- | -------------------------- |
| `$CODEX_HOME/AGENTS.override.md`        | 用户级覆盖指令             |
| `$CODEX_HOME/AGENTS.md`                 | 用户级默认指令             |
| `AGENTS.override.md`                    | 项目级覆盖指令             |
| `AGENTS.md`                             | 项目级默认指令             |
| `config:project_doc_fallback_filenames` | 配置中的候选项目指令文件名 |

**`AGENTS.md` 与 `AGENTS.override.md` 同时存在时，后者会覆盖前者**

<sub>参考：[文档][codex-agents-doc] / [源码][codex-agents-src]</sub>

### SKILLS

| 文件来源                     | 说明                  |
| ---------------------------- | --------------------- |
| `.agents/skills/**/SKILL.md` | 项目内 skills         |
| `.codex/skills/**/SKILL.md`  | 项目配置层 skills     |
| `$HOME/.agents/skills`       | 用户级 skills         |
| `/etc/codex/skills`          | 系统级 skills         |
| `$CODEX_HOME/skills/.system` | Codex 内置系统 skills |
| 插件声明                     | 插件提供的 skills     |

**同名 `SKILL` 不会被认为是同一个 `SKILL`，`Codex` 只以文件绝对路径区分**

<sub>参考：[文档][codex-skills-doc] / [源码][codex-skills-src]</sub>

### README.md

| 文件来源    | 说明        |
| ----------- | ----------- |
| `README.md` | 仓库 README |

`README.md` 不会被默认读取

> 可将 `README.md` 加入 `project_doc_fallback_filenames` 后作为候选项目指令读取。

<sub>参考：[文档][codex-agents-doc] / [源码][codex-agents-src]</sub>

### 相关配置

#### `project_root_markers`

> marker 的目录，默认值为 `[".git"]`

```toml
# ~/.codex/config.toml
project_root_markers = [".git", ".hg", ".sl"]
```

#### `project_doc_fallback_filenames`

> 项目指令文件名

```toml
# ~/.codex/config.toml
project_doc_fallback_filenames = ["CLAUDE.md", "GEMINI.md", "README.md"]
```

#### `project_doc_max_bytes`

> 项目级指令长度限制

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

本文档采用 [Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa-4] 授权。你可以复制、分发、改编和商用，但需要保留署名、许可链接，并说明是否修改。公开发布改编、翻译或衍生版本时，必须使用相同或兼容许可证。

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
