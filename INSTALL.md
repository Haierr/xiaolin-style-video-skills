# Install / 安装

## English

### 1. Upstream onetake
Clone or sync [feitangyuan/onetake](https://github.com/feitangyuan/onetake) into a local **kit** directory on the agent machine, for example:

```text
<kit-root>/
  repo/          # full feitangyuan/onetake checkout
  fonts/         # e.g. NotoSansSC.ttf (OFL) — pass to look.py --cjk
  dump_events.py # optional stand-in if missing upstream
  score.py       # optional stand-in if missing upstream
  example/       # optional short reference comp
```

If you already maintain a box kit (common pattern: `…/tools/onetake-kit/` with `repo/`, `fonts/`, helpers), point the skills at that path instead of copying again.

Fetch stale files from `raw.githubusercontent.com/feitangyuan/onetake/main/...` or via the GitHub API / MCP tree tools. Binaries (fonts) are not always in the repo — download separately under a permissive licence (e.g. Google Noto OFL).

### 2. Dependencies
```bash
pip install playwright numpy scipy pillow matplotlib fonttools brotli opencv-python
playwright install --with-deps chromium
# also: ffmpeg on PATH
# optional VO: faster-whisper, kokoro-onnx (Chinese VO needs another TTS)
# optional music sync: librosa
```

### 3. Wire skills into the agent
Copy or symlink:

```text
skills/getting-started-xiaolin-video/     → <agent-skills>/getting-started-xiaolin-video/
skills/onetake-motion-promo/              → <agent-skills>/onetake-motion-promo/
skills/xiaolin-style-talking-head-edit/   → <agent-skills>/xiaolin-style-talking-head-edit/
```

Exact destination depends on the host (Cursor Agent Skills folder, Grok Bot `workflows/`, etc.). Each skill is a folder containing `SKILL.md` with YAML front matter (`name`, `description`).

### 4. Project layout when editing
Under the agent workspace, prefer a fresh project per film:

```text
/workspace/<project>/
  film/          # comp.html, BEATS.md, look.js, render outputs
  assets/        # B-roll, CREDITS.md
  work/          # timemap.json, face tracks, etc.
```

Copy `repo/{lib,looks,scripts,templates}` and the CJK font into the project as described in `onetake-motion-promo`.

### 5. Licence reminder
Confirm **non-commercial** use before rendering with onetake. Commercial jobs: reuse method only; render in another tool, or obtain a commercial onetake licence.

---

## 中文

### 1. 上游 onetake
将 [feitangyuan/onetake](https://github.com/feitangyuan/onetake) 同步到本机 **kit** 目录（结构见上文英文节）。若机器上已有 `…/tools/onetake-kit/` 一类缓存，直接复用即可。

### 2. 依赖
安装 Playwright + Chromium、`ffmpeg`，以及可选的 faster-whisper / librosa 等（命令见上）。

### 3. 导入技能
把本仓库 `skills/` 下三个文件夹拷到或软链到 Agent 的 skills / workflows 目录。

### 4. 成片交付
交付到**主人指定的本地文件夹或网盘位置**，不要写死某一网盘厂商、私人链接或账号路径。

### 5. 许可
onetake 仅限非商用；商用请换工具或取得商业授权。
