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
