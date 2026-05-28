## 📄 Agent Context Map

这是一份 Agent 上下文加载行为的对照表。<br>
它整理了主流 Agent 指令作用域范围与优先级，包括项目指令、用户级配置、仓库规则、skills、插件和 README.md。

<p>
  <b>简体中文</b> ·
  <a href="#">English</a> ·
  <a href="https://github.com/cclilshy/agent-context-map/issues">提交 Issue</a>
</p>
</div>

<hr>

## 我为什么写它?

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

<!-- prettier-ignore -->
| 工具 | 版本 |
| --- | --- |
| [Codex CLI](#codex) | `0.134.0` |
| [Claude Code](#claude-code) | `none` |
| [Gemini CLI](#gemini-cli) | `none` |
| [Cursor](#cursor) | `none` |

**目前只维护 “最新已验证版本” 的行为说明**

## Codex

<!-- prettier-ignore -->
| 类型 | 说明 |
| --- | --- |
| 项目根 | `project_root_markers` 配置决定项目根；未配置时默认 `.git`，未找到 marker 时仅使用当前工作目录；空数组会禁用父目录查找<br><sub>来源：[文档][codex-config-doc] / [源码][codex-agents-src] / [源码][codex-root-src]</sub> |
| 项目指令 | 全局 `$CODEX_HOME/AGENTS.override.md` > `$CODEX_HOME/AGENTS.md`，只取第一个非空文件<br>项目内每级目录按 `AGENTS.override.md` > `AGENTS.md` > `project_doc_fallback_filenames` 选择一个候选文件；从项目根到当前目录顺序拼接；受 `project_doc_max_bytes` 限制<br>指令链在每次 run / TUI session 启动时构建<br><sub>来源：[文档][codex-agents-doc] / [源码][codex-agents-src]</sub> |
| SKILLS | skill 本体是 `SKILL.md`；初始上下文只注入可用 skill 的名称、描述和路径，完整 `SKILL.md` 仅在选择/调用该 skill 后读取<br>项目内：当前目录到项目根路径上的 `.agents/skills/**/SKILL.md`，以及项目配置层的 `.codex/skills/**/SKILL.md`<br>项目外：`$HOME/.agents/skills`、`/etc/codex/skills`、`$CODEX_HOME/skills/.system`、插件声明；`$CODEX_HOME/skills` 为兼容旧位置<br>同名 skill 不会合并，可能同时出现；去重按 `SKILL.md` **文件绝对路径**<br><sub>来源：[文档][codex-skills-doc] / [源码][codex-skills-src]</sub> |
| README | 默认不会作为项目指令读取；仅在用户/指令明确要求读取，或将 `README.md` 加入 `project_doc_fallback_filenames` 后作为候选项目指令读取<br><sub>来源：[文档][codex-agents-doc] / [源码][codex-agents-src]</sub> |

#### `project_doc_fallback_filenames`

```toml
# ~/.codex/config.toml
project_doc_fallback_filenames = ["CLAUDE.md", "GEMINI.md", "README.md"]
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
[codex-agents-src]: https://github.com/openai/codex/blob/rust-v0.134.0/codex-rs/core/src/agents_md.rs
[codex-root-src]: https://github.com/openai/codex/blob/rust-v0.134.0/codex-rs/config/src/project_root_markers.rs
[codex-skills-src]: https://github.com/openai/codex/blob/rust-v0.134.0/codex-rs/core-skills/src/loader.rs
[codex-tag]: https://github.com/openai/codex/tree/rust-v0.134.0
