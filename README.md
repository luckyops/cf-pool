# cf-pool · Cloudflare 自适应优选 IP 地址池

由 `10.0.1.181` 每天 3 轮自动扫描本网络环境后推送。目标：**在可控成本下，最大化找到
「当前网络出口真正可作为 Cloudflare CDN frontend 且速度优秀」的 IPv4**。

## 分层扫描架构

| 层 | 范围 | 深度 | 频率 |
|---|---|---|---|
| Core | CF 官方 IPv4 段 | 1 IP/ /24 | 3 次/天 |
| Hot | 近期表现优秀 /24 + 全量基准回灌金矿（上限 24 段） | 16 IP/ /24 | 3 次/天 |
| Warm | 近期有表现的 /24 | 4 IP/ /24 | 3 次/天 |
| Deep | 最热 Top3 /24 | 全量 256 IP（-allip） | 3 次/天 |
| Extra | 已验证可用的非官方 /24（如 8.35.211.0/24） | 8 IP/ /24 | 3 次/天 |
| Cold | 连续 15 轮无表现的 /24 | 1 IP/ /24（:80 检查） | 1 次/天 |
| Discovery | AS13335/AS395747 官方外 BGP 宣告空间 | 1 IP/ /24 | 1 次/天 |
| Country | 落地国家注册表（美西/加/澳等延迟吃亏的优先区段） | 4 IP × 12 段/国家 | 3 次/天 |

## 国家配额

资格赛为 US/CA/AU 保留下载名额（`US:20,CA:10,AU:6`），入池保底席位 `US:8,CA:5`（有候选才生效），
注册表由每轮落地探测自动维护。实测当前网络出口 CF 落地分布：LAX×1126 段 / FRA×907 / AMS×318 /
SIN×97 / SJC×13 / NRT×8，**CA 落地段为 0**（境内出境路由不经过 CF 加拿大机房，属路由物理现实；
配额机制已就绪，路由变化会自动接入）。**加拿大的现实替代**：`proxy_ca.txt` 提供经功能验证、
物理位于加拿大的社区反代地址（流量从加拿大出境），每日随源刷新自动更新。

## TLS 下载资格赛

下载测速名额是稀缺资源，采用三段式：**纯延迟扫描 → CDN 功能探测（延迟前 120）→
资格赛下载**。只有同时通过「功能验证（SNI 进 CF CDN 管道）+ 区域过滤」的候选才有
下载名额（优先区 50 + 次选 20 /轮）——测速预算 100% 花在可能入池的 IP 上。

## CDN 功能验证

候选 IP 必须以真实 SNI（speed.cloudflare.com）完成 TLS + HTTP 且返回 `colo=` 才被认定
进入 CF CDN 管道——仅 TCP 可达的 Magic Transit / BYOIP 地址一律丢弃。
Discovery 通过功能验证的 /24 需**连续 2 天成功**才自动晋升进 Extra 池。

## 区域策略

排除落地 香港/韩国/英国；优先 日本/新加坡/美国/澳大利亚/加拿大；其余次选；同层内按扫描排名排序。

## 文件

| 文件 | 用途 | edgetunnel 变量 |
|---|---|---|
| `add.txt` | TLS 池（IP:443，每池 50 个，速度+延迟排序） | `ADDAPI` |
| `notls.txt` | 非TLS 池（IP:80，每池 50 个，延迟排序） | `ADDNOTLSAPI` |
| `all.txt` | 两池合并（100 个） | 任意 |
| `proxy.txt` | 社区反代 IP 池（非 CF 自有 IP，IPDB 聚合，自愿取用） | `ADD`（自担风险） |
| `proxy_ca.txt` 等 | 按地理国家细分的反代子池（ca/us/au/jp/sg；ca 池经 SNI 功能验证，流量从加拿大出境） | `ADD`（自担风险） |
| `stats_report.txt` | /24 滚动统计 Top30（热点分布） | — |
| `result_*.csv` | 本轮扫描明细（延迟/丢包/速度/落地机房） | — |

Raw 地址：
- https://raw.githubusercontent.com/luckyops/cf-pool/main/add.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/notls.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/all.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/proxy.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/proxy_ca.txt
