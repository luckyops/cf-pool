# cf-pool

自动筛选 Cloudflare 优选 IP 的地址池：每天定时在真实网络环境下测速、验证，只保留当前真正可用且速度好的 IPv4，供 edgetunnel 等订阅工具直接拉取。

## 怎么用

在 edgetunnel 里把下面的地址填到对应变量即可，内容每天自动更新：

| 变量 | 地址 | 说明 |
|---|---|---|
| `ADDAPI` | https://raw.githubusercontent.com/luckyops/cf-pool/main/add.txt | TLS 池（`IP:443`），推荐使用 |
| `ADDNOTLSAPI` | https://raw.githubusercontent.com/luckyops/cf-pool/main/notls.txt | 非 TLS 池（`IP:80`） |

## 文件

| 文件 | 内容 |
|---|---|
| `add.txt` | TLS 池，约 20～40 个，按综合表现从好到差排序 |
| `notls.txt` | 非 TLS 池，按延迟排序 |
| `all.txt` | 上面两个合并 |
| `proxy*.txt` | 社区反代 IP（非 Cloudflare 自有 IP，自愿使用，风险自担） |
| `stats_report.txt` | 本轮统计报告 |
| `result_tls.csv` `result_notls.csv` | 本轮测速明细（延迟、丢包、速度、机房） |

## 说明

- 地址按我所在网络环境测得，不同网络（运营商、地区）的实际效果可能不同。
- 已排除香港、韩国、英国落地的节点。
- 使用 Cloudflare Workers 转发代理流量可能违反其服务条款，请自行评估风险。
