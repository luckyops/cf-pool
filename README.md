# cf-pool · Cloudflare 优选 IP 地址池

由 `10.0.1.181` 上的 CloudflareSpeedTest 每天自动扫描本网络环境 3 次（UTC 7/15/23 点）后推送。

**区域策略**：排除落地机房为 香港/韩国/英国 的 IP；优先收录 日本/新加坡/美国/澳大利亚/加拿大，其次其他地区；同层内按扫描排名（速度/延迟）排序。

**扫描范围**：Cloudflare 官方 IP 段 + AS13335/AS395747 全部 BGP 宣告前缀 + 104.28.0.0/14 补充块，归并后约 180 万 IP（CFST 默认每 /24 随机采样 1 个 IP）。

| 文件 | 用途 | edgetunnel 变量 |
|---|---|---|
| `add.txt`    | TLS 池（IP:443，延迟+速度双排序） | `ADDAPI` |
| `notls.txt`  | 非TLS 池（IP:80，延迟排序） | `ADDNOTLSAPI` |
| `all.txt`    | 两池合并 | 任意 |
| `proxy.txt`  | 社区反代 IP 池（非 CF 自有 IP，由 IPDB 聚合，自愿取用） | `ADD`（自担风险） |
| `result_*.csv` | 扫描明细（延迟/丢包/速度/落地机房） | — |

Raw 地址：
- https://raw.githubusercontent.com/luckyops/cf-pool/main/add.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/notls.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/all.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/proxy.txt
