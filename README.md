# cf-pool · Cloudflare 优选 IP 地址池

由 `10.0.1.181` 上的 CloudflareSpeedTest 每天自动扫描本网络环境 3 次（UTC 7/15/23 点）后推送。

**区域策略**：排除落地机房为 香港/韩国/英国 的 IP；优先收录 日本/新加坡/美国/澳大利亚/加拿大，其次其他地区；同层内按扫描排名（速度/延迟）排序。

| 文件 | 用途 | edgetunnel 变量 |
|---|---|---|
| `add.txt`    | TLS 池（IP:443，延迟+速度双排序） | `ADDAPI` |
| `notls.txt`  | 非TLS 池（IP:80，延迟排序） | `ADDNOTLSAPI` |
| `all.txt`    | 两池合并 | 任意 |
| `result_*.csv` | 扫描明细（延迟/丢包/速度/落地机房） | — |

Raw 地址：
- https://raw.githubusercontent.com/luckyops/cf-pool/main/add.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/notls.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/all.txt
