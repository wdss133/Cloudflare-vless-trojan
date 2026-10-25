# Cloudflare-vless-trojan (fork)

上游：https://github.com/yonggekkk/Cloudflare-vless-trojan

## 为什么用「明」版而不是混淆版

Cloudflare Pages 的 **Git 构建**会用 esbuild 打包 `_worker.js`，混淆版过不了它的静态检查：

```
✘ [ERROR] Cannot assign to "Q" because it is a constant
```

直传（上传资产）不打包，所以能跑；Git 构建必打包，所以只能用未混淆的 `_worker明.js`。
这是本项目改用 Git 集成后唯一必须接受的取舍。

## 自动化

`.github/workflows/sync-upstream.yml` 每 6 小时：

1. `git merge upstream/main` —— 把上游最新代码合并进来
2. 重新生成 `deploy-vless/_worker.js` 与 `deploy-trojan/_worker.js`
3. 生成时把 `const proxyIPs = ["pyip.ygkkk.dpdns.org"];` 强制改成 `[""]`（proxyIP 留空）
4. 有变化就 push —— Pages 监听到 push 自动构建部署

上游如果改了这一行的结构，工作流会 **报错停下**，不会带着作者的默认 proxyIP 部署。

## Pages 项目配置

| 项目 | root_dir | 构建命令 | 输出目录 |
|---|---|---|---|
| cf-vless | `deploy-vless` | 留空 | 留空 |
| cf-trojan | `deploy-trojan` | 留空 | 留空 |

## 环境变量

| 变量 | 值 |
|---|---|
| `uuid` / `pswd` | 见 Cloudflare 项目设置 |
| `proxyip` | **不设置**（= 留空，不走作者的公益中转） |
| `cdnip` | `yg1.ygkkk.dpdns.org`（作者维护的优选域名，DNS 自动跟随） |
| `ip1`-`ip7` | 104.16.0.0 … 104.24.0.0（80 系端口，Pages 会 301，仅占位） |
| `ip8`-`ip13` | 104.16.0.0 / 104.17.0.0 / 104.18.0.0 / 104.19.0.0 / 104.20.0.0 / 104.21.0.0（443 系，实测 200） |
