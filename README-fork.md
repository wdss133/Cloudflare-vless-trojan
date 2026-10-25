# Cloudflare-vless-trojan (fork)

上游：https://github.com/yonggekkk/Cloudflare-vless-trojan

## 这个 fork 做了什么

1. `.github/workflows/sync-upstream.yml` 每 6 小时把上游 `main` 合并进来。
2. 合并后重新生成两份可部署副本：
   - `deploy-vless/_worker.js`（对应 Pages 项目 root_dir = `deploy-vless`）
   - `deploy-trojan/_worker.js`（对应 `deploy-trojan`）
3. 生成时把 `const proxyIPs = ["pyip.ygkkk.dpdns.org"];` 强制改为 `[""]`（proxyIP 留空，不使用作者的公益中转）。
4. 如果上游把这一行改掉了，工作流会 **报错停下**，而不是带着作者默认值部署。

## 部署参数

| 变量 | 值 |
|---|---|
| uuid / pswd | 在 Pages 项目的「变量和机密」里设置 |
| proxyip | 不设置（留空） |
| cdnip | `yg1.ygkkk.dpdns.org`（作者维护的优选域名，自动跟随其 DNS） |
| ip8-13 | 104.16/17/18/19/20/21.0.0（实测 200） |

> 注意：Pages 通道强制 HTTPS，80 系端口会 301，ip1-7 生成的节点不可用。
