# sinica-econ-admin-procedures

**中央研究院經濟研究所**研究室的行政流程知識庫，整理自某研究室「專任助理交接—行政支援」簡報。
不綁定特定計畫主持人，**任何經濟所研究室都可以直接使用**；也**不綁定特定 AI 工具**，
Claude、Codex、GitHub Copilot、Cursor、Gemini CLI 都能讀同一份內容。

流程規則全部寫在 `procedures/` 底下，入口有兩個、內容同源：

| 入口檔 | 給誰用 |
| --- | --- |
| `SKILL.md` | Claude Code、Claude.ai、GitHub Copilot（Agent Skill 格式，含 frontmatter） |
| `AGENTS.md` | OpenAI Codex、Cursor、Gemini CLI 等讀 `AGENTS.md` 的工具 |

> **維護原則：改規則就改 `procedures/` 底下的檔案。** 兩個入口檔只做總覽與路由，不放流程細節，
> 各平台看到的內容才不會分歧。

## 資料夾結構

```
sinica-econ-admin-procedures/
├── SKILL.md          ← 入口（Claude / Copilot）：總覽、共通規則、路由表
├── AGENTS.md         ← 入口（Codex / Cursor / Gemini CLI），內容同源
├── procedures/       ← 流程細節，一個主題一個檔（請購、報帳、差旅、聘僱、公文…）
├── lab-profile/      ← 各研究室的私人資料，一年度一檔，不進版控
├── templates/        ← 表單與範本
└── setup/            ← 各家 AI 工具的安裝輔助檔
```

> 本 repo 含所內承辦人姓名與分機等內部資訊，**建議設為 Private**，只邀請需要的助理與老師加入。

---

## 第一次使用：建立自己的 `lab-profile/lab-profile-<年度>.md`

本 repo 刻意**不含任何研究室的私人資料**。老師姓名、經費金額、計畫代碼、訂閱帳號信箱、
記帳雲端位置等，全部集中在 `lab-profile/` 資料夾，**一個年度一個檔案**：

```
lab-profile/
├── lab-profile-example.md   ← 範本（進版控，只放佔位符）
├── lab-profile-2025.md      ← 舊年度，留著供回查
├── lab-profile-2026.md
└── lab-profile-2026-3.md    ← 目前有效的那一檔
```

clone 之後先做這一步：

**Windows（PowerShell）：**
```powershell
Copy-Item lab-profile\lab-profile-example.md lab-profile\lab-profile-2026.md
```

**Mac / Linux：**
```bash
cp lab-profile/lab-profile-example.md lab-profile/lab-profile-2026.md
```

然後打開新檔依提示填入自己研究室的資料。沒有這個檔案，
AI 仍然可以回答流程問題，但**不會知道**你們老師是誰、要用哪筆經費、計畫代碼是什麼。

**檔名規則**：`lab-profile-<年度>.md`；同一年度內要改版就加序號（`lab-profile-2026-2.md`）。
AI 一律讀**最新那一檔**，所以你只要新增檔案，不用去改任何引用。

**為什麼分年度**：經費金額、計畫代碼、訂閱週期年年變。開新的一檔、舊檔留著，
就能回查「去年那筆錢是用哪個代碼報的」。平常只維護最新那一檔。

> ⚠️ **不要把個資寫進其他 `.md` 檔。** 除 `lab-profile/` 底下的年度設定檔以外，
> 所有檔案都會上傳 GitHub。助理的身分證字號、戶籍地址、電話等，一律只寫在紙本或本機檔案上，
> 文件內的範例統一用 `〔身分證字號〕` 這類佔位符。

---

## 使用者的操作方式

### 步驟一：下載（每台電腦只做一次，各家工具共用同一份）

先 clone 到本機的 skills 目錄。**不論你用哪個 AI 工具，都只 clone 這一份**，
之後 `git pull` 一次，所有工具同步更新。

**Windows（PowerShell）：**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"
cd "$env:USERPROFILE\.claude\skills"
git clone https://github.com/<帳號>/sinica-econ-admin-procedures.git
cd sinica-econ-admin-procedures
Copy-Item lab-profile\lab-profile-example.md lab-profile\lab-profile-2026.md   # 然後填寫
```

**Mac / Linux：**
```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/<帳號>/sinica-econ-admin-procedures.git
cd sinica-econ-admin-procedures
cp lab-profile/lab-profile-example.md lab-profile/lab-profile-2026.md   # 然後填寫
```

> 資料夾叫 `.claude/skills` 只是因為 Claude 系工具規定要放在這裡才會自動載入。
> Codex、Cursor 等靠絕對路徑讀同一份，資料夾叫什麼都不影響。

日後若有更新，進入該目錄執行 `git pull` 即可（`lab-profile/` 中你自己的年度設定檔不受 pull 影響）。

### 步驟二：接上你用的 AI 工具

各工具只需設定一次。下面 `<skill 路徑>` 指的是上一步 clone 出來的資料夾絕對路徑，例如
`C:\Users\你的帳號\.claude\skills\sinica-econ-admin-procedures`。

#### Claude Code

不用額外設定。啟動 Claude Code 後直接提問，它會自動載入 `SKILL.md`。

#### VS Code（GitHub Copilot）

不用額外設定。clone 完成後重新開啟 VS Code，在 Copilot Chat 直接提問即可。

#### OpenAI Codex

三種接法，擇一即可：

**A. 在 skill 資料夾裡開 Codex（最簡單）**
```bash
cd <skill 路徑>
codex
```
Codex 會自動讀取該資料夾的 `AGENTS.md`，直接提問即可。

**B. 讓 Codex 在任何目錄都記得這份知識庫**
編輯（沒有就新建）`~/.codex/AGENTS.md`，加上一行：
```markdown
處理中研院經濟所研究室的行政、報帳、請購問題時，先讀 <skill 路徑>/AGENTS.md 再回答。
```

**C. 做成 `/lab-admin` 指令**
把 `setup/codex-prompt.md` 複製成 `~/.codex/prompts/lab-admin.md`，
打開它把第一段的路徑改成你的 `<skill 路徑>`。之後在 Codex 打 `/lab-admin 請購單怎麼填？` 即可。

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\prompts"
Copy-Item "<skill 路徑>\setup\codex-prompt.md" "$env:USERPROFILE\.codex\prompts\lab-admin.md"
```
```bash
# Mac / Linux
mkdir -p ~/.codex/prompts
cp "<skill 路徑>/setup/codex-prompt.md" ~/.codex/prompts/lab-admin.md
```

#### Cursor / Gemini CLI / 其他讀 `AGENTS.md` 的工具

在 skill 資料夾中開啟該工具（Cursor 就是 File → Open Folder 開這個資料夾），
`AGENTS.md` 會自動生效。

#### Claude.ai（網頁／App）

建議用 **Project** 功能，讓內容在整個專案內持續生效：

1. 登入 Claude.ai → 左側選單點「**New Project**」
2. 進入 Project 後，點「**Project knowledge** → Add content」
3. 上傳所有 `.md` 檔案（`SKILL.md`、`procedures/procurement.md`、`procedures/reimbursement.md` 等），
   **並記得一起上傳你填好的 `lab-profile/lab-profile-<年度>.md`**
4. 之後在這個 Project 內開新對話，直接提問即可

> 若只是臨時用一次，也可以在對話中直接上傳 `SKILL.md`、你的 `lab-profile-<年度>.md` 和相關子檔案，無需建 Project。

#### ChatGPT（網頁／App）

同上，用 **Project** 上傳 `.md` 檔案；沒有 Project 時，在對話中直接上傳
`AGENTS.md`、`SKILL.md`、你的 `lab-profile-<年度>.md` 和相關子檔案。

### 步驟三：直接問

設定完就可以用日常語言提問，例如：

- 「我要核銷國外差旅費，要準備哪些文件？」
- 「請購單怎麼填？」
- 「邀請國外學者來訪，餐費怎麼報？」
- 「ChatGPT Plus 的訂閱費怎麼報，匯率怎麼算？」

---

## 更新者的操作方式

### 首次設定（每台電腦做一次）

```bash
# 1. 安裝 git 後，設定身分
git config --global user.name "你的名字"
git config --global user.email "你的email"

# 2. 下載整個 skill 到本機
git clone https://github.com/<帳號>/sinica-econ-admin-procedures.git
cd sinica-econ-admin-procedures
```

> 若 repo 是 Private，push/pull 時 GitHub 會要求登入。密碼欄請貼 **Personal Access Token**
> （GitHub → Settings → Developer settings → Personal access tokens 產生，勾 `repo` 權限），
> 不能用帳號密碼。

### 下載最新版（pull）

別人（或你在另一台電腦）改過內容後，先拉最新版再開始編輯，避免衝突：

```bash
cd sinica-econ-admin-procedures
git pull
```

### 上傳修改（push）

改完檔案（例如更新了 `procedures/reimbursement.md` 的規則）之後：

```bash
git status                        # 看看改了哪些檔案（lab-profile-<年度>.md 不應出現）
git add -A                        # 把所有變更加入這次提交
git commit -m "更新報帳匯率規則"   # 用一句話描述改了什麼
git push                          # 上傳到 GitHub
```

### 建議的維護習慣

- **開始編輯前先 `git pull`，改完當天就 `git push`**，兩台電腦或兩個人交接時才不會互相蓋掉。
- commit 訊息寫清楚改了哪個流程（例：「新增 114 年健保資料展延窗口」），方便日後追查規則是哪一年改的。
- 所內規則年年改：每次照著 skill 跑流程時若發現與所內專區公告不符，**當場改檔案並 push**，這份文件才會一直是活的。
- **流程規則一律改子檔案，不要改 `SKILL.md` 或 `AGENTS.md`。** 這兩個是不同 AI 工具的入口，
  只放總覽與路由；規則寫在子檔案，Claude 和 Codex 看到的內容才會一致。
  只有在**新增或改名子檔案**時，才需要同步更新這兩個入口的路由表／檔案地圖。
- **改到通用流程就 push，改到自己研究室的資料就只改 `lab-profile/` 底下自己的年度設定檔**——這樣所有研究室都能共享流程更新，
  彼此的私人資料又不會混在一起。
- 若兩人同時改了同一段落，`git pull` 時會出現衝突（conflict）：打開檔案，保留正確版本、刪掉 `<<<<<<<`、`=======`、`>>>>>>>` 標記後，重新 `git add -A && git commit && git push`。
- push 前快速自檢：`git diff --cached` 看一下有沒有不小心貼進真實姓名、身分證字號或計畫代碼。

### 更新到各平台

push 完成後，請通知使用者依照「[使用者的操作方式](#使用者的操作方式)」各節說明同步最新版。
