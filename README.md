# cf-pool · Cloudflare 优选 IP 地址池

由 `10.0.1.181` 上的 CloudflareSpeedTest 每天自动扫描本网络环境 3 次（UTC 7/15/23 点）后推送。

| 文件 | 用途 | edgetunnel 变量 |
|---|---|---|
| `add.txt`    | TLS 池（IP:443，延迟+速度双排序） | `ADDAPI` |
| `notls.txt`  | 非TLS 池（IP:80，延迟排序） | `ADDNOTLSAPI` |
| `all.txt`    | 两池合并 | 任意 |
| `result_*.csv` | 扫描明细（延迟/丢包/速度/落地机房） | — |

Raw 地址（把 `main` 换成分支名即可）：
- https://raw.githubusercontent.com/luckyops/cf-pool/main/add.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/notls.txt
- https://raw.githubusercontent.com/luckyops/cf-pool/main/all.txt
