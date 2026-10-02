# 小Lin说式视频剪辑 · Skill Pack / Xiaolin-style Video Skills

[English](#english) | [中文](#chinese)

Agent skills that turn a raw talking-head clip (口播) into a **小Lin说-style** knowledge short: dense B-roll, circle avatar, pop-in cards, beat-synced cuts, licensed BGM/SFX — built on the open-source [feitangyuan/onetake](https://github.com/feitangyuan/onetake) motion engine.

> **License warning / 许可警示:** onetake is **PolyForm Noncommercial 1.0.0**. Personal / non-monetised posts are fine. Paid client work, ads, and monetised accounts need a commercial licence for onetake **or** another tool (AE, DaVinci, etc.) while reusing only the *method* from these skills. See [LICENSE](LICENSE).

---

<a name="chinese"></a>
## 中文

### 这个包做什么
把口播成片做成「小Lin说」式知识短视频工作流，供 **Grok Bot / Cursor Agent Skills** 导入使用。技能是**文档与流程**，不是 onetake 本体；剪辑引擎请从上游安装。

### 包含的技能
| 文件夹 | 用途 |
|--------|------|
| `skills/getting-started-xiaolin-video/` | 新主人首次对话：语言、平台、是否商用、素材与交付偏好 |
| `skills/onetake-motion-promo/` | onetake 安装、构图协议、动效库、渲染与校验（短片基座） |
| `skills/xiaolin-style-talking-head-edit/` | 口播 → 小Lin说层：转写、人脸圆头像、真图 B-roll、卡点、配乐配效、预览与交付 |

### 如何给 Grok Bot / Cursor 用
1. Clone 本仓库，或把 `skills/*` 拷到 Agent 的 skills / workflows 目录（例如 `~/.cursor/skills/` 或 agent `workflows/`）。
2. 按 [INSTALL.md](INSTALL.md) 准备 onetake kit、依赖与 CJK 字体。
3. 新会话先跑 `getting-started-xiaolin-video`，再对第一条口播启用 `xiaolin-style-talking-head-edit`（它会依赖 `onetake-motion-promo`）。
4. **交付**：成片放到主人指定的本地文件夹或网盘位置，不要写死某一网盘厂商或私人路径。

### 不包含什么
- 不含私人账号、项目成片、凭证、具体网盘路径或内部工程名。
- 不含 onetake 源码；请链到 [feitangyuan/onetake](https://github.com/feitangyuan/onetake)。

---

<a name="english"></a>
## English

### What this pack does
A reusable **Agent Skill** pack for editing talking-head clips into 小Lin说-style knowledge shorts. Skills are **docs and workflow**; install the [onetake](https://github.com/feitangyuan/onetake) engine separately.

### Skills included
| Folder | Role |
|--------|------|
| `skills/getting-started-xiaolin-video/` | First conversation: language, platforms, commercial gate, asset & delivery prefs |
| `skills/onetake-motion-promo/` | Install, composition protocol, motion library, render & verify |
| `skills/xiaolin-style-talking-head-edit/` | Talking-head layer: ASR, circle avatar, real B-roll, cards, music/SFX, preview & delivery |

### Use with Grok Bot / Cursor skills
1. Clone this repo, or copy each `skills/<name>/SKILL.md` folder into the agent's skills or workflows directory.
2. Follow [INSTALL.md](INSTALL.md) for the onetake kit, Python/Chromium deps, and a CJK font.
3. Run `getting-started-xiaolin-video` once for a new owner, then `xiaolin-style-talking-head-edit` on the first clip (it depends on `onetake-motion-promo`).
4. **Delivery:** put exports in the owner's preferred local folder or netdisk location — never hard-code a vendor or private path.

### What is not included
- No private accounts, project masters, credentials, or owner-specific cloud paths.
- No onetake source; link upstream only.

### Upstream
- Engine: [feitangyuan/onetake](https://github.com/feitangyuan/onetake) — PolyForm Noncommercial 1.0.0
- This pack's skill text: see [LICENSE](LICENSE)
