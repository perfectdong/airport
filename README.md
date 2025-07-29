# jichangnodes - 自动化代理节点收集和管理项目

## 项目概述

`jichangnodes` 是一个自动化代理节点收集和管理项目，主要用于从各种机场（代理服务商）获取试用订阅、收集代理节点、生成配置文件，并提供多种格式的订阅服务。项目包含多个 Python 脚本，每个脚本都有特定的功能和用途。

## 感谢

感谢以下开源项目的贡献：
- https://github.com/awuaaaaa/vless-py
- https://github.com/wzdnzd/aggregator
- https://github.com/0xJins/x.sub
- https://github.com/w1770946466/Auto_proxy
- https://github.com/VPNforWindowsSub/base64
- https://github.com/mojolabs-id/GeoLite2-Database
- https://github.com/midpoint/ClashForge
- https://github.com/mlzlzj/df

## 核心 Python 脚本详细说明

### 1. 试用节点获取模块

#### `get_trial.py` - 主要试用获取脚本
**功能**：自动化机场试用订阅获取的核心脚本

**主要特性**：
- **多线程并发处理**：使用 `ThreadPoolExecutor` 实现高效的并发试用获取
- **智能注册机制**：支持自动用户注册、登录和邮箱验证
- **临时邮箱集成**：使用 `TempEmail` 类处理邮箱验证码获取
- **订阅管理**：获取、验证和更新订阅链接
- **配置文件生成**：生成 Base64 和 Clash 格式的配置文件
- **缓存机制**：通过 `trial.cache` 避免重复操作
- **节点过滤**：支持排除特定节点和关键词过滤

**工作流程**：
1. 读取 `trial.cfg` 配置文件
2. 清理旧的试用文件
3. 并发执行试用获取任务
4. 汇总所有获取到的节点信息
5. 生成最终的配置文件

#### `get_trial_update_url.py` - 短链接生成脚本
**功能**：为项目生成的订阅文件创建便于分享的短链接

**主要特性**：
- **多平台支持**：支持 `dd.al` 短链接服务和 GitHub Raw 链接
- **智能别名生成**：根据项目和文件名生成标准化别名
- **CDN 加速**：支持基于 `GITHUB_SHA` 的 CDN URL 生成
- **自动化管理**：支持链接的插入、更新和搜索
- **环境变量配置**：通过环境变量灵活配置

### 2. 配置生成模块

#### `ClashForge.py` - Clash 配置生成器
**功能**：专门用于生成 Clash 配置文件的脚本

**主要特性**：
- **多协议支持**：解析 Hysteria2, SS, Trojan, Vless, Vmess 等代理协议
- **节点去重**：基于服务器地址和端口进行智能去重
- **模板系统**：根据预定义模板生成标准化 Clash YAML 配置
- **规则管理**：支持自定义代理规则和分组
- **性能优化**：高效的节点解析和配置生成算法

#### `subconverter.py` - 订阅转换器
**功能**：高级订阅转换和配置生成工具

**主要特性**：
- **多格式支持**：支持 Base64 和 Clash 配置生成
- **代理提供者管理**：支持代理提供者的分割和合并
- **规则优化**：自动移除冗余规则和分组
- **短链接集成**：与短链接服务集成
- **缓存机制**：使用装饰器实现配置缓存
- **灵活配置**：支持排除特定节点和自定义配置

### 3. 数据收集模块

#### `collectSub.py` - 订阅链接收集器
**功能**：从指定 URL 收集和验证订阅链接

**主要特性**：
- **URL 验证**：验证订阅链接的有效性
- **关键词过滤**：过滤包含特定关键词的链接
- **配置驱动**：从 `config.yaml` 读取配置
- **结果保存**：将有效链接保存到 `data/subscribes.txt`

#### `hy2.py` - GitHub 代理搜索器
**功能**：从 GitHub 仓库搜索、收集和测试代理 URL

**主要特性**：
- **GitHub API 集成**：使用 GitHub API 搜索关键词
- **多协议支持**：支持 SS, SSR, Vmess, Trojan, Vless, Hysteria 等
- **异步测试**：异步测试 URL 连通性
- **YAML 解析**：解析 YAML 内容和 Base64 数据
- **自动更新**：更新 GitHub 仓库中的文件

#### `jichang_list.py` - 机场链接抓取器
**功能**：从 Telegram 频道或 HTML 页面抓取机场订阅链接

**主要特性**：
- **多源抓取**：支持 Telegram 频道和网页抓取
- **HTML 解析**：使用 BeautifulSoup 解析 HTML 内容
- **并发处理**：多线程并发抓取和处理
- **连通性测试**：测试 URL 的可用性
- **数据持久化**：加载和保存已有的 URL 列表

### 4. 订阅处理模块

#### `subscribe/collect.py` - 高级订阅收集器
**功能**：企业级订阅收集和管理系统

**主要特性**：
- **模块化设计**：集成多个子模块（crawl, executable, push, utils, workflow）
- **机场管理**：使用 `AirPort` 类管理机场信息
- **GitHub 集成**：支持 GitHub API 操作和文件管理
- **环境变量配置**：通过环境变量进行灵活配置
- **命令行界面**：提供丰富的命令行参数
- **日志系统**：完整的日志记录和错误处理

**命令行参数**：
- `-a, --all`：生成包含所有节点的完整配置
- `-c, --chuck`：丢弃需要人工验证的候选网站
- `-e, --easygoing`：使用 Gmail 别名处理邮箱白名单
- `-f, --flow`：按剩余流量过滤订阅
- `-n, --num`：设置处理线程数
- `-r, --refresh`：仅刷新现有订阅
- `-t, --targets`：选择生成的配置类型

### 5. 核心工具模块

#### `apis.py` - API 接口库
**功能**：机场面板和临时邮箱服务的 API 封装

**主要特性**：
- **多面板支持**：支持 V2Board, SSPanel, Hkspeedup 等面板
- **临时邮箱集成**：集成 8+ 临时邮箱服务
- **会话管理**：完整的 HTTP 会话管理
- **自动重定向**：智能的重定向处理
- **错误处理**：统一的错误处理机制
- **数据解析**：自动解析响应数据

**支持的面板类型**：
- V2Board：现代化的代理面板
- SSPanel：经典的代理面板
- Hkspeedup：特定的加速服务面板

**支持的临时邮箱服务**：
- MailGW, Snapmail, MailCX, GuerrillaMail
- Emailnator, Moakt, Rootsh, Linshiyou

#### `utils.py` - 工具函数库
**功能**：项目通用工具函数集合

**主要特性**：
- **文件操作**：读写、删除、列表等文件操作
- **配置管理**：配置文件的读取和写入
- **缓存机制**：函数结果缓存装饰器
- **数据结构**：IP CIDR 段树、域名后缀树、AC 自动机
- **并行处理**：并行映射函数
- **时间处理**：时间戳和字符串转换
- **流量计算**：流量大小的格式化和解析

**核心数据结构**：
- `IP_CIDR_SegmentTree`：IP 地址段管理
- `DOMAIN_SUFFIX_Tree`：域名后缀匹配
- `AC` / `AC_Online`：字符串匹配算法

# cloxy.io.yaml 生成流程说明

## 1. 节点数据抓取与处理
- 脚本会从机场订阅链接、API 或本地文件抓取原始节点数据。
- 支持多种协议，自动解析为 Clash 支持的格式。
- 对节点进行去重、清洗（如去除无效、重复、格式错误的节点）。

## 2. 节点分组自动生成
- 根据模板和实际节点，自动生成 proxy-groups。
- 每个分组（如“低延迟”、“AI”、“B站”）自动引用对应的节点集合。
- 支持分组嵌套、分组别名（如 &id001, *id001）。

## 3. 规则集自动合并
- 脚本会自动拉取本地/远程规则集（如广告屏蔽、分流、直连等）。
- 支持自定义规则，自动合并去重。
- 规则与分组自动关联（如广告走“💩 广告”分组）。

## 4. YAML 文件生成与校验
- 所有配置项（基础配置、分组、节点、规则）拼接为完整 YAML。
- 自动格式化，保证 YAML 合法性。
- 生成文件保存到指定路径（如 `trials/https_/cloxy.io.yaml`）。

## 5. 自动化与定时更新
- 支持通过定时任务、GitHub Actions 自动更新节点和规则。
- 可配置机场订阅、规则模板、输出格式等参数。

## 6. 主要相关脚本
- `subscribe/collect.py`：主聚合与生成逻辑。
- `subscribe/replace.py`：节点格式转换与清洗。
- `subconverter.py`：模板渲染与最终输出。

# .github/workflows/ 目录下各 workflow 文件详细功能说明

本项目的 `.github/workflows/` 目录包含多个 GitHub Actions workflow 文件，每个文件实现不同的自动化功能，具体说明如下：

- **auto_update.yml**  
  功能：定时或手动触发，自动拉取机场订阅、聚合节点、清洗并生成 Clash 配置文件，最后自动提交并推送到仓库。  
  作用：保证节点和配置文件的持续更新，无需人工干预，确保订阅内容始终为最新。

- **subconverter.yml**  
  功能：定时或手动触发，调用 subconverter 脚本，将聚合后的节点数据转换为多种订阅格式（如 Clash、Surge、QuantumultX 等），并输出到指定目录，自动推送更新。  
  作用：为不同客户端自动生成兼容的订阅文件，方便多端使用，提升订阅适配性。

- **check.yml**  
  功能：定时或每次推送时触发，自动检测所有节点的可用性，筛除失效节点，仅保留可用节点，更新配置文件并推送。  
  作用：提升节点订阅的可用性和质量，确保用户获取到的都是可用节点。

- **clean.yml**  
  功能：定期自动清理无用文件、临时文件或日志，保持仓库整洁。可扩展为自动归档、备份等辅助功能。  
  作用：防止仓库膨胀，便于维护和管理，减少无关文件对主流程的影响。

- **backup.yml**  
  功能：定时备份关键配置文件和节点数据，防止意外丢失。可将备份上传到指定分支或外部存储。  
  作用：保障数据安全，便于灾难恢复和历史追溯。

- **log_archive.yml**  
  功能：定期归档 workflow 运行日志，便于后续排查和分析历史任务执行情况。  
  作用：方便问题定位和追踪，提升运维效率。

> 说明：实际 workflow 文件名和功能以 `.github/workflows/` 目录下为准，部分功能可根据项目需求增删。

# GitHub Actions Workflow 流程与功能说明

本项目包含多个 GitHub Actions workflow，自动化实现节点聚合、配置生成、定时更新等功能。主要流程和作用如下：

## 1. 自动聚合与配置生成（如 auto_update.yml）

- **触发方式**：定时（如每6小时）、手动触发（workflow_dispatch）。
- **主要步骤**：
  1. **Checkout 代码**：拉取仓库最新代码。
  2. **安装依赖**：自动安装 Python 依赖（如 `pip install -r requirements.txt`）。
  3. **运行聚合脚本**：执行 `subscribe/collect.py` 等脚本，自动抓取、聚合、清洗节点，生成 Clash 配置文件。
  4. **生成/更新配置**：输出如 `trials/https_/cloxy.io.yaml` 等 YAML 文件。
  5. **自动提交并推送**：如有变更，自动 commit 并推送到仓库。

## 2. 订阅转换与多格式输出（如 subconverter.yml）

- **触发方式**：定时或手动。
- **主要步骤**：
  1. **拉取代码与依赖**。
  2. **运行 subconverter 脚本**：将聚合后的节点转换为多种订阅格式（如 Clash、Surge、QuantumultX 等）。
  3. **输出多格式订阅文件**。
  4. **自动推送更新**。

- **作用**：为不同客户端自动生成兼容的订阅文件，方便多端使用。

## 3. 健康检查与可用性检测（如 check.yml）

- **触发方式**：定时或每次推送。
- **主要步骤**：
  1. **运行节点可用性检测脚本**。
  2. **筛选掉失效节点，保留可用节点**。
  3. **更新配置文件并推送**。

- **作用**：保证聚合输出的节点均为可用，提高订阅质量。

## 4. 自动清理与日志归档（如 clean.yml、log_archive.yml）

- **触发方式**：定时。
- **主要步骤**：
  1. **自动清理无用文件、临时文件或日志**。
  2. **归档 workflow 运行日志，便于后续排查和分析**。

- **作用**：保持仓库整洁，便于维护和问题追踪。

## 5. 定期备份（如 backup.yml）

- **触发方式**：定时。
- **主要步骤**：
  1. **备份关键配置文件和节点数据**。
  2. **上传备份到指定分支或外部存储**。

- **作用**：防止数据丢失，便于恢复历史配置。

---

## 典型 workflow 文件示例

```yaml
name: Auto Update

on:
  schedule:
    - cron: '0 */6 * * *'
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run aggregator
        run: python subscribe/collect.py

      - name: Commit and push changes
        run: |
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git config --global user.name "github-actions[bot]"
          git add .
          git commit -m "Auto update: $(date '+%Y-%m-%d %H:%M:%S')" || echo "No changes to commit"
          git push
```

---

## 总结

- 所有 workflow 文件位于 `.github/workflows/` 目录。
- 主要实现自动抓取、聚合、转换、检测、推送等自动化流程。
- 通过 GitHub Actions，无需人工干预即可保持节点和配置的最新与可用。

---

# 使用建议

如需自定义 workflow，可在 `.github/workflows/` 目录下新建或修改 YAML 文件，调整定时、脚本路径、推送策略等参数。

## 项目架构和工作流程

### 整体架构
```
jichangnodes/
├── 核心获取模块 (get_trial.py, apis.py)
├── 配置生成模块 (ClashForge.py, subconverter.py)
├── 数据收集模块 (collectSub.py, hy2.py, jichang_list.py)
├── 订阅处理模块 (subscribe/)
├── 媒体处理模块 (main.py, vt.py)
├── 代理聚合模块 (TG_proxy_main.py)
├── 同步管理模块 (ji_github_sync.py, urls.py)
└── 工具支持模块 (utils.py)
```

### 典型工作流程

1. **数据收集阶段**
   - `jichang_list.py` 抓取机场链接
   - `hy2.py` 从 GitHub 搜索代理
   - `collectSub.py` 收集订阅链接

2. **试用获取阶段**
   - `get_trial.py` 自动注册和获取试用
   - `apis.py` 提供面板和邮箱 API 支持
   - `utils.py` 提供工具函数支持

3. **配置生成阶段**
   - `ClashForge.py` 生成 Clash 配置
   - `subconverter.py` 进行高级转换
   - `subscribe/convert_to_base64.py` 生成 Base64 格式

4. **链接管理阶段**
   - `get_trial_update_url.py` 生成短链接
   - `ji_github_sync.py` 同步到 GitHub

5. **质量保证阶段**
   - `urls.py` 验证链接可用性
   - 各模块内置的连通性测试

## 配置文件说明

### 主要配置文件
- `trial.cfg`：试用机场配置列表
- `subconverters.cfg`：订阅转换器配置
- `config.yaml`：主配置文件
- `trial.cache`：试用缓存文件

### 环境变量
项目大量使用环境变量进行配置，主要包括：
- `GITHUB_REPOSITORY`：GitHub 仓库信息
- `GITHUB_TOKEN`：GitHub API 令牌
- `DDAL_EMAIL` / `DDAL_PASSWORD`：短链接服务凭据
- `ALL_CLASH_DATA_API`：Clash 数据 API
- `GIST_PAT`：GitHub Personal Access Token

## 使用建议

### 新手使用
1. 首先运行 `get_trial.py` 获取基础试用节点
2. 使用 `ClashForge.py` 生成 Clash 配置
3. 通过 `get_trial_update_url.py` 生成分享链接

### 高级使用
1. 配置完整的环境变量
2. 使用 `subscribe/collect.py` 进行企业级收集
3. 结合 GitHub Actions 实现自动化

### 性能优化
- 合理设置并发线程数
- 使用缓存机制避免重复操作
- 定期清理临时文件

## 注意事项

1. **合规使用**：请确保在合法合规的前提下使用本项目
2. **频率控制**：避免过于频繁的请求，以免被目标服务器封禁
3. **数据安全**：妥善保管 API 密钥和敏感信息
4. **资源管理**：注意内存和磁盘空间的使用
5. **错误处理**：关注日志输出，及时处理错误

## 总结

`jichangnodes` 项目是一个功能完整、架构清晰的代理节点管理系统。通过模块化的设计，项目实现了从数据收集、试用获取、配置生成到链接管理的完整流程。每个 Python 脚本都有明确的职责和功能，相互配合形成了一个高效的自动化系统。

项目的核心优势在于：
- **自动化程度高**：最大程度减少人工干预
- **扩展性强**：模块化设计便于功能扩展
- **稳定性好**：完善的错误处理和重试机制
- **效率高**：多线程并发处理提升性能
- **兼容性强**：支持多种协议和格式

无论是个人使用还是企业部署，该项目都能提供可靠的代理节点管理解决方案。

## GitHub Actions 环境配置详细指南

### 概述

本项目使用 GitHub Actions 实现完全自动化的代理节点收集、处理和分发流程。为确保工作流正常运行，需要正确配置相应的 Secrets 和 Variables。

### 必需的 Secrets 配置

#### 核心认证 Secrets

##### 1. GIST_PAT (GitHub Personal Access Token)
- **功能作用**：GitHub 个人访问令牌，用于访问 GitHub API 和 Gist 服务
- **使用场景**：节点数据存储、配置文件更新、仓库操作
- **权限要求**：`repo`（完整仓库访问）、`gist`（Gist 访问）、`workflow`（工作流权限）
- **获取方式**：
  1. 进入 GitHub Settings > Developer settings > Personal access tokens > Tokens (classic)
  2. 点击 "Generate new token (classic)"
  3. 选择所需权限范围
  4. 生成并复制令牌（仅显示一次）
- **配置示例**：`ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
- **使用工作流**：Collect.yml, get_trial.yml

##### 2. GT_TOKEN (GitHub Token)
- **功能作用**：GitHub 访问令牌，主要用于 Telegram 订阅相关操作
- **使用场景**：仓库文件读写、配置更新、数据同步
- **权限要求**：与 GIST_PAT 相同，可以使用同一个令牌
- **配置示例**：`ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
- **使用工作流**：Telegram Subscriptions.yml

#### 数据源配置 Secrets

##### 3. CLASH_API
- **功能作用**：Clash 配置数据源 API 地址
- **使用场景**：获取外部 Clash 配置模板和规则集
- **配置要求**：完整的 HTTP/HTTPS URL，支持直接访问
- **配置示例**：`https://api.example.com/clash/config`
- **使用工作流**：Collect.yml
- **注意事项**：确保 API 地址可访问且返回有效的 Clash 配置数据

##### 4. SOURCE_URLS
- **功能作用**：SS 节点源 URL 列表
- **使用场景**：批量获取 Shadowsocks 节点信息
- **配置格式**：每行一个 URL，支持多个数据源
- **配置示例**：
  ```
  https://example1.com/ss-nodes
  https://example2.com/proxy-list
  https://example3.com/free-nodes
  ```
- **使用工作流**：ss.yml
- **功能特点**：支持并发处理多个数据源，自动去重和验证

#### 通信服务 Secrets

##### 5. BOT (Telegram Bot Token)
- **功能作用**：Telegram 机器人令牌，用于频道监控和消息处理
- **使用场景**：监控 Telegram 频道、获取订阅链接、发送通知
- **获取方式**：
  1. 在 Telegram 中搜索 @BotFather
  2. 发送 `/newbot` 命令
  3. 按提示设置机器人名称和用户名
  4. 获得 Bot Token
- **配置示例**：`1234567890:ABCdefGHIjklMNOpqrsTUVwxyz`
- **使用工作流**：Conversion.yml

### 工作流详细说明和配置

#### 1. Auto_proxy.yml - 自动代理任务
- **触发方式**：手动触发 + 每8小时自动运行
- **核心功能**：
  - 执行 `TG_proxy_main.py` 脚本
  - 处理大规模代理节点收集（700+ 机场支持）
  - 生成多格式配置文件（Clash 和 V2Ray）
  - Base64 编解码处理
- **依赖文件**：`TG_proxy_main.py`, `requirements2.txt`
- **输出结果**：聚合的代理配置文件
- **特点**：海量机场支持，高效并发处理

#### 2. Collect.yml - 订阅数据收集
- **触发方式**：手动触发 + 每3小时自动运行
- **核心功能**：
  - 运行 `subscribe/collect.py` 企业级收集器
  - 聚合多源订阅数据
  - 生成标准化配置文件
  - 支持命令行参数配置
- **所需 Secrets**：`CLASH_API`, `GIST_PAT`
- **处理流程**：
  1. 从配置的 API 获取数据
  2. 解析和验证节点信息
  3. 生成 Clash 配置文件
  4. 上传到 Gist 服务
- **高级特性**：模块化设计、GitHub 集成、日志系统

#### 3. Conversion.yml - Base64 转换
- **触发方式**：手动触发 + 每8小时自动运行
- **核心功能**：
  - Base64 格式转换
  - 节点连通性测试
  - 多格式输出支持
  - 代理指纹生成和去重
- **所需配置**：`BOT`, `URL_LIST_REPO_API`
- **环境变量**：
  - `TIMEOUT_SECONDS=10`：连接超时时间
  - `MAX_RETRIES=3`：最大重试次数
  - `CONCURRENT_LIMIT=50`：并发连接限制
- **支持协议**：VMess, Trojan, Shadowsocks, Hysteria2

#### 4. Telegram Subscriptions.yml - Telegram 频道监控
- **触发方式**：手动触发 + 每8小时自动运行
- **核心功能**：
  - 监控指定 Telegram 频道
  - 提取订阅链接
  - 验证链接有效性
  - 保存到指定仓库
- **所需 Secrets**：`GT_TOKEN`, `SUBSCRIPTION_TARGET_REPO`, `SUBSCRIPTION_SAVE_PATH`, `CONFIG_REPO_NAME`, `CONFIG_FILE_PATH`, `SEARCH_KEYWORDS_ENV`
- **智能特性**：关键词匹配、自动过滤、数据持久化

#### 5. get_trial.yml - 试用节点获取
- **触发方式**：手动触发 + 每3小时自动运行
- **核心功能**：
  - 自动注册机场试用账户
  - 获取试用订阅链接
  - 生成配置文件和短链接
  - 临时邮箱集成
- **所需 Secrets**：`DDAL_EMAIL`, `DDAL_PASSWORD`
- **权限要求**：write-all（用于文件提交）
- **高级特性**：多线程并发、智能注册、缓存机制

#### 6. Node Tester.yml - 节点测试
- **触发方式**：手动触发 + 每小时自动运行
- **核心功能**：
  - 测试节点连通性
  - 评估节点质量
  - 过滤无效节点
  - 性能基准测试
- **依赖文件**：`package.json`, `test_nodes.js`
- **运行环境**：Node.js
- **测试指标**：延迟、稳定性、可用性

#### 7. jichang_list.yml - 机场列表更新
- **触发方式**：手动触发 + 每天凌晨3点运行
- **核心功能**：
  - 更新机场列表
  - 验证机场可用性
  - 更新 `trial.cfg` 配置
  - 多源抓取（Telegram 频道、网页）
- **输出文件**：`trial.cfg`
- **特性**：HTML 解析、并发处理、连通性测试

#### 8. ss.yml - SS 节点收集
- **触发方式**：手动触发 + 每3小时自动运行 + 推送触发
- **核心功能**：
  - 从多个源收集 SS 节点
  - 节点去重和验证
  - 自动提交更新
  - 并发处理优化
- **所需 Secrets**：`ACTIONS_DEPLOY_KEY`, `SOURCE_URLS`
- **权限要求**：contents: write
- **处理特点**：两阶段处理、智能去重、进度监控

#### 9. clashforge.yml - Clash 配置生成
- **触发方式**：仅手动触发
- **核心功能**：
  - 运行 `ClashForge.py` 脚本
  - 生成标准 Clash 配置
  - 节点解析和去重
  - 模板系统支持
- **依赖文件**：`requirements.txt`, `ClashForge.py`
- **支持协议**：Hysteria2, SS, Trojan, Vless, Vmess

### 配置优先级和建议

#### 最小配置（新手推荐）
适合初次使用，可运行基础功能：
```
✅ GITHUB_TOKEN (自动提供)
✅ GIST_PAT = 你的GitHub个人访问令牌
✅ GT_TOKEN = 同上（可复用）
```
**可运行工作流**：clashforge.yml, jichang_list.yml

#### 基础功能配置
支持大部分核心功能：
```
✅ 最小配置 +
✅ CLASH_API = Clash配置API地址
✅ URL_LIST_REPO_API = GitHub API地址
```
**可运行工作流**：Collect.yml, Conversion.yml

#### 完整功能配置
支持所有高级功能：
```
✅ 基础配置 +
✅ BOT = Telegram机器人令牌
✅ ACTIONS_DEPLOY_KEY = SSH私钥
✅ SOURCE_URLS = SS节点源列表
✅ DDAL_EMAIL & DDAL_PASSWORD = 短链接服务凭据
✅ Telegram订阅相关的6个Secrets
```
**可运行工作流**：全部工作流

### 安全最佳实践

#### 1. 敏感信息保护
- **原则**：所有敏感信息必须设置为 Secrets，禁止硬编码
- **范围**：API 密钥、密码、令牌、私钥
- **验证**：定期检查代码中是否有泄露的敏感信息
- **工具**：使用 git-secrets 等工具扫描敏感信息

#### 2. 权限最小化
- **GitHub PAT**：只授予必需的最小权限
- **SSH 密钥**：使用专用密钥，避免复用个人密钥
- **工作流权限**：根据实际需要设置权限范围
- **定期审查**：定期检查和调整权限设置

#### 3. 定期维护
- **令牌轮换**：定期更新和轮换 API 密钥（建议3-6个月）
- **权限审查**：每月检查权限设置
- **日志监控**：监控工作流运行情况和异常
- **安全扫描**：定期进行安全漏洞扫描

#### 4. 备份和恢复
- **密钥备份**：安全保存重要密钥的备份
- **配置文档**：维护详细的配置文档
- **恢复流程**：制定密钥丢失的恢复流程
- **测试恢复**：定期测试备份恢复流程

### 故障排除指南

#### 常见错误类型

##### 1. Secret not found
- **错误信息**：`Error: Secret XXXX not found`
- **原因**：Secret 名称拼写错误或未设置
- **解决方案**：
  1. 检查 Secret 名称拼写
  2. 确认已在仓库设置中正确添加
  3. 验证 Secret 值不为空
- **验证方法**：在工作流中添加调试输出（注意不要泄露敏感信息）

##### 2. Permission denied
- **错误信息**：`Error: Permission denied` 或 `403 Forbidden`
- **原因**：GitHub PAT 权限不足
- **解决方案**：
  1. 检查 PAT 权限范围
  2. 重新生成具有足够权限的令牌
  3. 确认令牌未过期
- **验证方法**：
  ```bash
  curl -H "Authorization: token YOUR_PAT" https://api.github.com/user
  ```

##### 3. SSH key error
- **错误信息**：`Permission denied (publickey)`
- **原因**：SSH 密钥配置错误或权限不足
- **解决方案**：
  1. 重新生成密钥对
  2. 确保正确配置 Deploy key
  3. 检查私钥格式完整性
- **验证方法**：
  ```bash
  ssh -T git@github.com
  ```

##### 4. API rate limit
- **错误信息**：`API rate limit exceeded`
- **原因**：API 调用频率过高
- **解决方案**：
  1. 调整工作流运行频率
  2. 添加延迟机制
  3. 使用认证的 API 调用（更高限额）
- **预防措施**：合理设置定时任务间隔

#### 调试技巧

##### 1. 日志分析
- 查看 Actions 页面的详细运行日志
- 关注错误信息和堆栈跟踪
- 检查环境变量和 Secret 的使用情况
- 使用 `set -x` 启用详细调试输出

##### 2. 本地测试
```bash
# 测试 SSH 连接
ssh -T git@github.com

# 测试 GitHub API
curl -H "Authorization: token YOUR_PAT" https://api.github.com/user

# 测试 Clash API
curl -X GET "YOUR_CLASH_API_URL"

# 测试 Telegram Bot
curl "https://api.telegram.org/botYOUR_BOT_TOKEN/getMe"
```

##### 3. 分步验证
- 从最简单的工作流开始测试（如 clashforge.yml）
- 逐步添加复杂功能
- 每次只修改一个配置项
- 使用手动触发测试新配置

### 性能优化建议

#### 1. 并发控制
- **合理设置并发数**：根据服务器性能调整
- **避免资源竞争**：错开高负载工作流的运行时间
- **使用队列机制**：处理大量任务时使用队列
- **监控资源使用**：定期检查 CPU 和内存使用情况

#### 2. 缓存策略
- **GitHub Actions 缓存**：缓存依赖包和构建结果
- **数据缓存**：缓存频繁访问的数据
- **避免重复计算**：缓存计算结果
- **缓存失效策略**：设置合理的缓存过期时间

#### 3. 资源管理
- **监控运行时间**：优化长时间运行的任务
- **内存优化**：避免内存泄漏和过度使用
- **磁盘清理**：及时清理临时文件
- **网络优化**：减少不必要的网络请求

### 监控和维护

#### 1. 运行监控
- **状态监控**：定期检查工作流运行状态
- **失败通知**：设置失败通知机制
- **性能监控**：监控运行时间和资源使用
- **日志分析**：定期分析运行日志

#### 2. 数据质量
- **完整性检查**：验证输出数据的完整性
- **可用性测试**：检查节点的可用性
- **更新频率**：监控配置文件的更新频率
- **数据一致性**：确保不同格式数据的一致性

#### 3. 系统维护
- **依赖更新**：定期更新依赖包
- **安全补丁**：及时应用安全补丁
- **清理任务**：定期清理过期数据
- **备份策略**：实施定期备份策略

### 快速配置检查清单

#### Secrets 配置检查表
```
□ GIST_PAT = ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
□ GT_TOKEN = ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
□ CLASH_API = https://your-clash-api.com/config
□ BOT = 1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
□ DDAL_EMAIL = your-email@example.com
□ DDAL_PASSWORD = your-secure-password
□ ACTIONS_DEPLOY_KEY = -----BEGIN OPENSSH PRIVATE KEY-----...
□ SOURCE_URLS = https://example1.com/ss-nodes\nhttps://example2.com/proxy-list
□ SUBSCRIPTION_TARGET_REPO = moneyfly1/jichangnodes
□ SUBSCRIPTION_SAVE_PATH = data/subscriptions.txt
□ CONFIG_REPO_NAME = moneyfly1/jichangnodes
□ CONFIG_FILE_PATH = config/telegram_config.json
□ SEARCH_KEYWORDS_ENV = 节点,代理,vpn,翻墙
```

#### Variables 配置检查表
```
□ URL_LIST_REPO_API = https://api.github.com/repos/moneyfly1/jichangnodes/contents/urls.txt
```

#### Deploy Keys 配置检查表
```
□ SSH 公钥已添加到仓库 Deploy keys
□ 已勾选 "Allow write access"
□ 私钥已正确设置为 ACTIONS_DEPLOY_KEY Secret
```

#### 工作流测试检查表
```
□ clashforge.yml 手动触发测试通过
□ Collect.yml 运行正常
□ get_trial.yml 试用获取功能正常
□ ss.yml SSH 部署功能正常
□ 所有定时任务按预期运行
```

## 项目总结

`jichangnodes` 是一个功能强大的代理节点聚合和管理平台，通过自动化的 GitHub Actions 工作流，实现了从节点收集、验证、转换到配置生成的完整流程。项目支持多种代理协议，集成了丰富的数据源，并提供了灵活的配置选项，是代理服务管理的理想解决方案。

### 核心优势
- **全自动化**：通过 GitHub Actions 实现完全自动化的节点收集和处理
- **多源聚合**：支持 700+ 机场和多种数据源
- **高可靠性**：内置节点验证、去重和质量检测机制
- **灵活配置**：支持多种输出格式和自定义配置
- **安全可靠**：采用最佳安全实践，保护敏感信息
- **易于维护**：模块化设计，便于扩展和维护

### 适用场景
- 个人代理节点管理
- 企业级代理服务部署
- 代理节点质量监控
- 多源数据聚合分析
- 自动化运维管理

通过合理配置和使用本项目，您可以轻松构建一个稳定、高效的代理节点管理系统。
