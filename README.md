# 感谢:
 https://github.com/awuaaaaa/vless-py
 
 https://github.com/wzdnzd/aggregator
 
 https://github.com/0xJins/x.sub
 
 https://github.com/w1770946466/Auto_proxy

 https://github.com/VPNforWindowsSub/base64

 https://github.com/mojolabs-id/GeoLite2-Database

 https://github.com/midpoint/ClashForge

 https://github.com/mlzlzj/df

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
