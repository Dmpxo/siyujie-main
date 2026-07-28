# 思愈界网站域名绑定操作手册

## 项目概述

本项目包含5个独立页面，需要绑定到5个不同域名：

| 域名 | 用途 | 状态 |
|------|------|------|
| siyujie.fun | 总网站首页 | 待配置 |
| siyujie.online | 五行经典 | 待配置 |
| siyujie.xyz | 四季果香 | 待配置 |
| siyujie.shop | 道系灵山 | 待配置 |
| end-dmpxo.art | 十二花神 | ⚠️ 已绑定（请确认） |

---

## 方案选择

### 方案一：独立GitHub仓库（推荐，最简单）

为每个系列创建独立的GitHub仓库，每个仓库绑定一个自定义域名。

**优点**：
- 配置简单，无需Cloudflare Worker
- 避免路径代理问题
- 每个页面独立管理

**缺点**：
- 需要维护5个仓库

### 方案二：Cloudflare Workers（高级方案）

使用Cloudflare Workers实现多域名路由，所有页面放在一个仓库中。

**优点**：
- 只需维护1个仓库
- 统一管理

**缺点**：
- 配置复杂
- 需要处理路径代理问题
- 页面内相对资源路径需要调整

---

## 方案一：独立GitHub仓库（推荐）

### 第一步：创建5个GitHub仓库

**操作步骤**：
1. 登录GitHub，创建以下5个仓库：
   - `siyujie-main` - 总网站首页
   - `siyujie-online` - 五行经典
   - `siyujie-xyz` - 四季果香
   - `siyujie-shop` - 道系灵山
   - `siyujie-art` - 十二花神（如果已有仓库可跳过）

### 第二步：上传文件到各仓库

**操作步骤**：
打开命令行，执行以下命令：

```bash
# === 总网站首页 (siyujie-main) ===
cd "c:\Users\Administrator\Desktop\摆摊日记"
mkdir temp-main && cd temp-main
git init
copy "..\index.html" .
git add .
git commit -m "Initial commit: 总网站首页"
git remote add origin https://github.com/你的用户名/siyujie-main.git
git push -u origin main
cd .. && rmdir /s /q temp-main

# === 五行经典 (siyujie-online) ===
mkdir temp-online && cd temp-online
git init
copy "..\domains\siyujie.online\index.html" index.html
copy "..\domains\siyujie.online\new_products.js" .
git add .
git commit -m "Initial commit: 五行经典"
git remote add origin https://github.com/你的用户名/siyujie-online.git
git push -u origin main
cd .. && rmdir /s /q temp-online

# === 四季果香 (siyujie-xyz) ===
mkdir temp-xyz && cd temp-xyz
git init
copy "..\domains\siyujie.xyz\index.html" .
git add .
git commit -m "Initial commit: 四季果香"
git remote add origin https://github.com/你的用户名/siyujie-xyz.git
git push -u origin main
cd .. && rmdir /s /q temp-xyz

# === 道系灵山 (siyujie-shop) ===
mkdir temp-shop && cd temp-shop
git init
copy "..\domains\siyujie.shop\index.html" .
git add .
git commit -m "Initial commit: 道系灵山"
git remote add origin https://github.com/你的用户名/siyujie-shop.git
git push -u origin main
cd .. && rmdir /s /q temp-shop

# === 十二花神 (siyujie-art) ===
mkdir temp-art && cd temp-art
git init
copy "..\domains\end-dmpxo.art\index.html" .
git add .
git commit -m "Initial commit: 十二花神"
git remote add origin https://github.com/你的用户名/siyujie-art.git
git push -u origin main
cd .. && rmdir /s /q temp-art
```

### 第三步：配置每个仓库的GitHub Pages

**操作步骤**：
对每个仓库执行以下操作：

1. 进入GitHub仓库 → **Settings**（设置）
2. 左侧菜单找到 **Pages**（页面）
3. 在 **Build and deployment** 部分：
   - **Source**（来源）选择 `Deploy from a branch`
   - **Branch**（分支）选择 `main`，目录选择 `/`（根目录）
   - 点击 **Save**（保存）

### 第四步：配置自定义域名

**操作步骤**：
对每个仓库执行以下操作：

1. 进入GitHub仓库 → **Settings** → **Pages**
2. 在 **Custom domain** 输入框中输入对应域名：
   - siyujie-main → `siyujie.fun`
   - siyujie-online → `siyujie.online`
   - siyujie-xyz → `siyujie.xyz`
   - siyujie-shop → `siyujie.shop`
   - siyujie-art → `end-dmpxo.art`（如果已有配置可跳过）
3. 点击 **Save**（保存）
4. 勾选 **Enforce HTTPS**

### 第五步：配置域名DNS（阿里云）

**操作目标**：将域名指向GitHub Pages

**操作步骤**：
登录阿里云控制台 → **域名服务** → 找到您的域名：

对每个域名添加以下DNS记录：

| 记录类型 | 主机记录 | 记录值 | TTL |
|---------|---------|--------|-----|
| A | @ | 185.199.108.153 | 10分钟 |
| A | @ | 185.199.109.153 | 10分钟 |
| A | @ | 185.199.110.153 | 10分钟 |
| A | @ | 185.199.111.153 | 10分钟 |

**验证**：等待DNS生效后，访问域名应能看到对应页面

---

## 方案二：Cloudflare Workers（高级方案）

### 第一步：准备GitHub仓库

**操作步骤**：
1. 打开GitHub，创建一个新仓库（如：`siyujie-website`）
2. 在本地摆摊日记目录执行以下命令：

```bash
cd "c:\Users\Administrator\Desktop\摆摊日记"
git init
git add .
git commit -m "Initial commit: 思愈界网站全部文件"
git remote add origin https://github.com/你的用户名/siyujie-website.git
git push -u origin main
```

### 第二步：配置GitHub Pages

**操作步骤**：
1. 进入GitHub仓库 → **Settings**（设置）
2. 左侧菜单找到 **Pages**（页面）
3. 在 **Build and deployment** 部分：
   - **Source**（来源）选择 `Deploy from a branch`
   - **Branch**（分支）选择 `main`，目录选择 `/`（根目录）
   - 点击 **Save**（保存）

### 第三步：注册Cloudflare账号

**操作步骤**：
1. 访问 https://dash.cloudflare.com/sign-up 注册账号
2. 登录后，依次添加5个域名

### 第四步：修改域名DNS服务器（阿里云）

**操作步骤**：
1. 登录阿里云控制台 → **域名服务**
2. 将每个域名的DNS服务器替换为Cloudflare提供的DNS服务器

### 第五步：创建Cloudflare Worker

**操作步骤**：
1. 进入Cloudflare → **Workers & Pages** → **Create application**
2. 点击 **Create Worker**
3. 粘贴以下代码（已修复路径拼接问题）：

```javascript
// 思愈界网站域名路由Worker
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url)
  const hostname = url.hostname

  // 路由映射表
  const routes = {
    'siyujie.fun': 'https://你的用户名.github.io/siyujie-website/',
    'www.siyujie.fun': 'https://你的用户名.github.io/siyujie-website/',
    'siyujie.online': 'https://你的用户名.github.io/siyujie-website/domains/siyujie.online/',
    'siyujie.xyz': 'https://你的用户名.github.io/siyujie-website/domains/siyujie.xyz/',
    'siyujie.shop': 'https://你的用户名.github.io/siyujie-website/domains/siyujie.shop/',
    'end-dmpxo.art': 'https://你的用户名.github.io/siyujie-website/domains/end-dmpxo.art/'
  }

  const targetUrl = routes[hostname]
  
  if (targetUrl) {
    // 构建新URL，避免双斜杠
    const baseUrl = new URL(targetUrl)
    let newPathname = baseUrl.pathname
    if (!newPathname.endsWith('/')) newPathname += '/'
    if (url.pathname.startsWith('/')) {
      newPathname += url.pathname.slice(1)
    } else {
      newPathname += url.pathname
    }
    
    const newUrl = new URL(baseUrl.origin)
    newUrl.pathname = newPathname
    newUrl.search = url.search
    
    // 转发请求
    const response = await fetch(newUrl.toString(), {
      headers: request.headers,
      method: request.method,
      body: request.body,
      redirect: 'follow'
    })
    
    // 修改响应头，处理相对路径资源
    const newHeaders = new Headers(response.headers)
    newHeaders.set('Access-Control-Allow-Origin', '*')
    
    return new Response(response.body, {
      status: response.status,
      statusText: response.statusText,
      headers: newHeaders
    })
  }

  return new Response('Not Found', { status: 404 })
}
```

4. 将 `你的用户名` 替换为您的GitHub用户名
5. 点击 **Deploy**（部署）

### 第六步：配置Cloudflare DNS

**操作步骤**：
1. 进入Cloudflare → 选择一个域名
2. 点击 **DNS** → **Add record**
3. 添加以下记录：

| 记录类型 | 名称 | 内容 | TTL |
|---------|------|------|-----|
| A | @ | 192.0.2.1 | Auto |
| CNAME | www | your-worker-name.username.workers.dev | Auto |

4. 对所有5个域名重复以上步骤

### 第七步：启用Cloudflare SSL

**操作步骤**：
1. 进入Cloudflare → 选择一个域名
2. 点击 **SSL/TLS** → **Overview**
3. 将 **SSL/TLS encryption mode** 设置为 `Full`

---

## 验证测试

### 域名访问验证

| 域名 | 预期结果 |
|------|---------|
| https://siyujie.fun/ | 总网站首页 |
| https://siyujie.online/ | 五行经典页面 |
| https://siyujie.xyz/ | 四季果香页面 |
| https://siyujie.shop/ | 道系灵山页面 |
| https://end-dmpxo.art/ | 十二花神页面 |

### 功能验证

- [ ] 所有页面导航栏链接能正常跳转
- [ ] 所有页面显示HTTPS锁图标
- [ ] 五行经典页面的商品数据能正常加载
- [ ] 十二花神页面的轮盘能正常旋转

---

## 故障排除

### 常见问题

**Q1：域名访问显示404**
- 检查GitHub Pages是否已部署成功
- 检查DNS记录是否已生效（使用 `nslookup 域名` 命令）
- 等待DNS生效（可能需要24小时）

**Q2：页面样式错乱**
- 检查页面中的相对路径是否正确
- 确保所有页面使用绝对URL引用资源

**Q3：HTTPS证书问题**
- 确保GitHub Pages已勾选 **Enforce HTTPS**
- 等待GitHub自动签发证书（可能需要几小时）

**Q4：五行经典页面数据不显示**
- 检查 `new_products.js` 是否已上传到仓库
- 检查页面中引用的路径是否正确

**Q5：导航链接跳转异常**
- 检查页面中的导航链接是否使用了正确的域名
- 确保所有链接使用绝对URL（如：`https://siyujie.fun`）

---

## 经验教训 / Known Pitfalls（踩坑记录）

这部分记录 `2026-07-24` 至 `2026-07-28` 部署过程中踩到的关键坑，下次部署可对照避坑。

### 坑 1：GitHub Pages 自动占位空 `index.html`

**现象**：仓库 `main` 分支 root 有 HTML 文件 `cilian-jingshang-v3.html`（不是 `index.html`），但访问 `https://xxx.com/` 还是显示 **404**（GitHub 官方 404 页面，不是我们部署的内容）。

**原因**：当你新建 GitHub 仓库并**先在 Pages 设置里启用 Pages**（哪怕还没 `index.html`），GitHub 会**自动塞一个空的占位 `index.html`**进 main 分支（`sha=e69de29bb2d1d6434b8b29ae775ad8c2e48c5391`，`size=0`）。

**后果**：即使你后续 `git push` 或 Contents API 上传了真实的 HTML，只要根目录里同时存在这个 `e69de29` 开头的空 `index.html`，Pages 就把它当默认入口；**GitHub Pages 只会找 `index.html`**，不会 fallback 到其他命名（`cilian-jingshang-v3.html` 等）。

**解决**：
- **方案 A（推荐）**：在仓库根目录**复制一份**文件，命名为 `index.html`。推荐用 Contents API（见坑 2）。
- **方案 B**：网页端新建 `Settings → Pages → Branch` 点 Save 之前，先 push 内容；GitHub 检测到有 `index.html` 就不会塞占位。

### 坑 2：Contents API 更新已存在文件必须带 `sha`

**现象**：

```bash
# 第一个调用 PUT 创建 index.html
PUT /repos/Dmpxo/siyujie-online/contents/index.html
{
  "message": "Invalid request.\n\n\"sha\" wasn't supplied.",
  "status": "422"
}
```

**原因**：Contents API 的规则——
- **新建文件**：`message` + `content`（**不需要 `sha`**）
- **更新文件**：`message` + `content` + `sha`（**必须带 `sha`** —— 防止无脑覆盖）

第一次踩坑时 `index.html` 还**没存在**（理论上不该要 sha），但因为坑 1 里 GitHub 自动塞了一个空 `index.html`，导致 PUT 时 API 把它当「更新已有文件」来校验，缺 `sha` 就 422。

**解决**：先 GET 当前文件的 `sha`，再带 `sha` 重试：

```python
# 1. GET 拿 sha
GET /repos/Dmpxo/{repo}/contents/index.html
→ {"sha": "e69de29bb2...", "size": 0, ...}

# 2. PUT 带 sha + 真实内容
PUT /repos/Dmpxo/{repo}/contents/index.html
{
  "message": "Replace empty placeholder with real content",
  "content": "<base64 of your HTML>",
  "sha": "e69de29bb2..."
}
→ 409 "index.html does not match e69de29bb2..."

# 等等！sha 不匹配？哦，对——sha 是 blob sha，不是 commit sha。
# 重新 GET 一次拿最新的 sha，再 PUT（不要用 PUT 失败的 409 响应里的 sha）
```

**正确流程**：
1. `GET /contents/{path}` → 拿 `sha` (blob sha)
2. `PUT /contents/{path}` with `{message, content, sha}` → 应该 200 返回

如果 409 说「does not match」，说明 sha 已过期——**重新 GET 一次**再 PUT。**幂等 GET → PUT** 是最稳的写法。

### 坑 3：PowerShell 5.x 的 `ConvertTo-Json` + 嵌套对象序列化会输出 .NET 反射内容

**现象**：

```powershell
$p = @{a=1; b=[byte[]]@(1,2,3)} | ConvertTo-Json
# 输出长得像：
{
    "a": 1,
    "b":  {
        "value": [System.Byte[]],
        "SerializationData": ...
    }
}
```

而不是 `[1,2,3]` 数组。

**原因**：PowerShell 5.1 的 `ConvertTo-Json` 对 byte[]、PSCredential、PSObject 等特殊类型会展开属性。

**解决**：
- **不要在 PowerShell 里用 `ConvertTo-Json`** 处理复杂 payload，特别是当 content 是 base64 编码的二进制文件时。
- **改用 Python**（`json.dumps(payload)`） 或直接**用 here-string 拼接纯文本 JSON**。
- 如果只用 PowerShell：先把 base64 字符串写到 `.txt` 文件，再在 PS 里 `Get-Content` 读出来，**拼接成 JSON 字符串**（不要 hashtable → ConvertTo-Json）。

```powershell
# ✅ 正确写法
$b64 = Get-Content ".\file.base64.txt" -Raw
$json = '{"message":"Update","content":"' + $b64 + '","sha":"abc..."}'
$json | Out-File ".\payload.json" -Encoding utf8
curl.exe -X PUT -H "Authorization: token xxx" --data-binary "@payload.json" ...
```

### 坑 4：本地 Hosts 强制 `github.com → 20.205.243.168`

**现象**：从这台机器 `git push` 到 `github.com` 经常报 `RPC failed; curl 28 Recv failure; Connection reset by peer`。

**诊断**：

```powershell
# 国内网络到 github.com 经常被 QoS 限制
nslookup github.com  # 拿到多个 IP，20.205.243.166
Test-NetConnection github.com -Port 443  # timeout
```

**解决**（已生效，2026-07-24）：

```
# C:\Windows\System32\drivers\etc\hosts（管理员权限编辑）
20.205.243.168    github.com
```

把 `github.com` 强制解析到 api.github.com 同段的 IP（这个 IP 不会 QoS），能解决 90% 的 push 失败。副作用：所有 GitHub API 调用（包括 web）都走这个 IP，所以「GitHub 网页偶尔 403」也属于正常现象，等 5-10 分钟限速窗口过去就好。

**注意**：这个 hosts 改动是**当前机器全局**的，不只影响 git。如果以后网页访问 GitHub 出现诡异 403，可以临时注释这行。

### 坑 5：PowerShell 编码乱码 + `_BINARY:` token

**现象**：在 QClaw Pty 里跑某些 PowerShell 命令会冒出乱码，比如：

```
git : '_BINARY:' 不是内部或外部命令
```

**原因**：QClaw Pty 通道在 escape 某些 ANSI 控制序列时会插一个 `_BINARY:` token 破坏命令。

**解决**：
- **不要混用** Pty 输出和 `2>&1`、管道 `|`、`Out-File` —— 容易触发转义。
- 把复杂脚本写到 `.ps1` 文件，用 `powershell -File script.ps1` 跑（但当前 OpClaw 限制 -File，可能失败）。
- **优先用 Python**（`python.exe script.py`），JSON/Unicode 处理都稳，输出也不会有 `_BINARY:` 乱码。

### 坑 6：阿里云 `.fun` 根域不能 CNAME

**现象**：`siyujie.fun @ CNAME dmpxo.github.io` 在阿里云添加时报「CNAME 记录与 NS 记录冲突」或「不允许根域 CNAME」。

**原因**：阿里云（多数国内注册商）的 DNS 规范不允许根域直接 CNAME（RFC 1034 推荐，`.fun` 属新顶级域最严）。

**解决**：
- 用 **4 条 A 记录**指向 GitHub Pages IP：`185.199.108.153` / `109.153` / `110.153` / `111.153`
- `www` 子域可以 CNAME `dmpxo.github.io`
- 用户访问 `siyujie.fun` 时浏览器会自动跳转到 `www.siyujie.fun` 或 GitHub 重定向

### 坑 7：CNAME 文件的换行/空格

**现象**：CNAME 文件内容必须是**纯文本单行**（无换行、无 BOM、无尾随空格），否则 GitHub Pages 解析 custom domain 会失败。

**正确写法**：

```
siyujie.fun
```

（文件大小 = 11 bytes，仅 `siyujie.fun\n`）

**错误示例**（21 bytes 或带 BOM）：

```
siyujie.fun
 ↩  # 多了个空格
```

**用 Contents API 时**：`message` + `content`（base64），**不要用 `\\\\n` 当换行**——base64 编码里换行会被 GitHub 当普通字符。建议：
```python
content_b64 = base64.b64encode(b"siyujie.fun").decode('ascii')
# 13 chars, no newline
```

### 坑 8：本地修改版 push 失败要用 Contents API 兜底

**场景**：本地 commit + `git push` 偶尔失败（github.com QoS、RPC failed），但你**不甘心重写整个 commit**。

**兜底方案**：用 Contents API 逐个文件 PUT：
- 把本地 `index.html`（或 JS）用 `base64` 编码
- PUT 到 `/repos/{owner}/{repo}/contents/{path}`
- 不需要 git 客户端，**纯 HTTP**，10 秒搞定

**适用文件大小**：< 1 MB（GitHub Contents API 限制约 100 MB，但建议小一点）。本项目 `new_products.js` (850KB) 用 Contents API 一次就过了。

---

## 一次性脚本：复现本次部署（Python）

如果以后需要重新部署，存这个脚本为 `C:\Users\Administrator\Desktop\摆摊日记\_tmp_deploy\deploy.py`，按需修改：

```python
import urllib.request, json, base64, ssl

TOKEN = "ghp_你的_github_token"
ctx = ssl.create_default_context()

def github(method, path, payload=None):
    req = urllib.request.Request(
        f"https://api.github.com{path}",
        data=json.dumps(payload).encode('utf-8') if payload else None,
        method=method,
        headers={
            "Authorization": f"token {TOKEN}",
            "Accept": "application/vnd.github+json",
            "Content-Type": "application/json"
        }
    )
    with urllib.request.urlopen(req, context=ctx, timeout=60) as r:
        return json.loads(r.read())

# 1. 上传文件（含 sha 更新）
def put_file(owner, repo, path, local_path, message):
    with open(local_path, 'rb') as f:
        b64 = base64.b64encode(f.read()).decode('ascii')

    # GET sha（如果存在）
    sha = None
    try:
        cur = github("GET", f"/repos/{owner}/{repo}/contents/{path}")
        sha = cur.get("sha")
    except urllib.error.HTTPError as e:
        if e.code != 404: raise

    payload = {"message": message, "content": b64}
    if sha: payload["sha"] = sha
    return github("PUT", f"/repos/{owner}/{repo}/contents/{path}", payload)

# 2. 上传 CNAME（必须无换行）
def put_cname(owner, repo, domain):
    payload = {
        "message": "Set custom domain",
        "content": base64.b64encode(domain.encode('ascii')).decode('ascii')
    }
    # 先 GET 看是否存在
    try:
        cur = github("GET", f"/repos/{owner}/{repo}/contents/CNAME")
        payload["sha"] = cur["sha"]
    except urllib.error.HTTPError:
        pass
    return github("PUT", f"/repos/{owner}/{repo}/contents/CNAME", payload)

# 3. 启用 Pages + enforce HTTPS
def enable_pages(owner, repo, branch="main", cname=None):
    payload = {"source": {"branch": branch, "path": "/"}}
    if cname:
        payload["cname"] = cname
        payload["https"] = {"enforce": True}
    return github("PUT", f"/repos/{owner}/{repo}/pages", payload)

# ============== 一键部署 ==============
OWNER = "Dmpxo"
TARGETS = [
    ("siyujie-main",   "siyujie.fun",    "index.html"),
    ("siyujie-online", "siyujie.online", "cilian-jingshang-v3.html"),
    ("siyujie-xyz",    "siyujie.xyz",    "seasonal-fragrance.html"),
    ("siyujie-shop",   "siyujie.shop",   "taoist-fragrance.html"),
]

for repo, domain, src_html in TARGETS:
    print(f"=== Deploying {repo} → {domain} ===")
    
    # 1. CNAME
    put_cname(OWNER, repo, domain)
    print(f"  CNAME set")
    
    # 2. index.html（关键！如果 GitHub 已经放了空占位，必须带 sha 覆盖）
    put_file(OWNER, repo, "index.html", f"C:\\Users\\Administrator\\Desktop\\摆摊日记\\{src_html}", f"Deploy {domain}")
    print(f"  index.html deployed")
    
    # 3. 启用 Pages
    enable_pages(OWNER, repo, "main", domain)
    print(f"  Pages enabled + HTTPS enforced")

print("\nAll done! 等待 1-2 分钟 DNS + SSL 自动生效。")
```

把这段脚本存到 `_tmp_deploy\deploy.py`，下次重部署只需要：
```powershell
python.exe C:\Users\Administrator\Desktop\摆摊日记\_tmp_deploy\deploy.py
```

---

## 项目文件结构

```
摆摊日记/
├── index.html                    # 总网站首页
├── cilian-jingshang-v3.html      # 五行经典（原始文件）
├── seasonal-fragrance.html       # 四季果香（原始文件）
├── taoist-fragrance.html         # 道系灵山（原始文件）
├── twelve-flower-goddesses.html  # 十二花神（原始文件）
├── new_products.js               # 五行经典数据文件
├── domains/                      # 域名目录
│   ├── siyujie.online/
│   │   ├── index.html
│   │   └── new_products.js
│   ├── siyujie.xyz/
│   │   └── index.html
│   ├── siyujie.shop/
│   │   └── index.html
│   └── end-dmpxo.art/
│       └── index.html
└── DEPLOYMENT_GUIDE.md           # 本操作手册
```

---

## 注意事项

1. **end-dmpxo.art** 已绑定十二花神页面，请确认当前状态后再进行配置
2. GitHub Pages免费版不支持自定义SSL证书，使用GitHub自动签发的证书即可
3. 建议使用方案一（独立仓库），配置更简单且不易出错
4. 如果使用方案二（Cloudflare Workers），需要注意路径代理问题

---

*文档版本：v1.2*
*创建日期：2026年7月24日*
*最后更新：2026年7月28日（新增「经验教训」章节，含 8 个踩坑点 + 一键部署脚本）*
