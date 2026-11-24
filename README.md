# parts.xiaoyanglab 部署说明

单文件站点：`index.html`（约 1.8MB，图片全部内嵌，零外部请求，断网也能用）。

## 方式 A：Cloudflare Pages（推荐，免费 + 自动 HTTPS）

1. 打开 Cloudflare → Workers & Pages → Create → Pages → Upload assets
2. 把本目录（含 index.html）拖进去，点 Deploy
3. 得到 `xxx.pages.dev` 后，进 Custom domains → 添加 `parts.xiaoyanglab.com`
4. 如果域名本来就在 Cloudflare，CNAME 会自动加好；否则去 DNS 加一条：
   `parts` → CNAME → `xxx.pages.dev`

## 方式 B：GitHub Pages

1. 新建仓库，把 `index.html` 传到根目录
2. Settings → Pages → Source 选 main 分支根目录
3. 仓库根目录加一个 `CNAME` 文件，内容一行：`parts.xiaoyanglab.com`
4. DNS 加：`parts` → CNAME → `<用户名>.github.io`

## 方式 C：自己的服务器（宝塔 / nginx）

把 `index.html` 放到站点根目录即可。nginx 建议：

```nginx
server {
    listen 443 ssl http2;
    server_name parts.xiaoyanglab.com;
    root /www/wwwroot/parts;
    index index.html;

    gzip on;
    gzip_types text/html application/javascript text/css image/svg+xml;
    gzip_min_length 1k;

    location = /index.html {
        add_header Cache-Control "public, max-age=300";
    }
}
```

> 1.8MB 的单页建议开 gzip，压完大约 400–500KB。


## 本仓库（GitHub Pages）专用

- 仓库根目录含 `index.html` 和 `CNAME`（内容 `parts.xiaoyanglab.xyz`）
- Pages 设置：Settings → Pages → Source = main / (root)
- DNS 记录（在 xiaoyanglab.xyz 的 DNS 里加）：

| 类型 | 主机记录 | 记录值 |
|---|---|---|
| CNAME | `parts` | `<你的用户名>.github.io` |

- 保存后等 5–30 分钟，GitHub 签发证书；签发完回到 Pages 设置勾上 **Enforce HTTPS**
- 验证：`https://parts.xiaoyanglab.xyz` 打开有 57 张卡；`dig parts.xiaoyanglab.xyz` 能解析到 `*.github.io`

> 免费账号的 Pages 仓库必须是 public；`.xyz` 域名做自定义域名没问题。

## 注意

- 49 张实拍来自 Wikimedia Commons（CC / 公有领域）；补齐的 8 张来自厂商和店铺产品页，**未获转载授权**。自己看没问题，公开可访问的站点建议把这 8 张换回示意图，或注明来源并准备随时下架。
- 页面里没有第三方脚本、没有统计、没有外链字体，隐私上很干净。
- 改内容只需改 `index.html` 里 `const CARDS=[...]` 那一段，文字都是明文。
