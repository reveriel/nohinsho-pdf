# nohinsho-pdf 迭代方案（NEXT-STEPS）

> 产品：納品書 PDF 生成ツール　https://www.reveriel.com/nohinsho-pdf/
> 运营者：GUO XING（个人事业主 / 大阪府大阪市）
> 更新：2026-10-12　当前版本：v1.1（法务三件套 + 首篇教程）

---

## 一、现状诊断

| 维度 | 状态 | 说明 |
|---|---|---|
| 工具功能 | ✅ 可上线 | 発行日/納品日分离、明細（品名・数量・**単位**・単価）、消費税 10%/8%、A4 打印 PDF |
| SEO 基建 | ✅ | canonical 绝对 URL、WebApplication + FAQPage JSON-LD、robots.txt、sitemap.xml |
| 分享裂变 | ✅ | X / LINE / 复制链接三个按钮（share-block） |
| 反馈入口 | ✅ | 页脚链接 + 右下固定 FAB（print 时隐藏） |
| beacon | ✅ | Cloudflare token `6fc01bf3ac5e457ba1bd16b91e5b653e` |
| 互链 | ✅ | 指向 適格請求書 / 見積書 / 領収書 / 注文書 / 判子 |
| **法务页** | ❌ → ✅ | 本次补齐 privacy.html / company.html |
| **内容层** | ❌ → 🟡 | 本次新增首篇教程 kakikata.html，仍需继续扩篇 |
| ASP 联盟位 | ❌ | 尚无广告位/联盟链接，待 ASP 过审后接入 |

**ASP 落审主因（据 japan-asp-report.md）**：无隐私政策 / 无运营者情报 / 无联系方式。
→ 本次已一次性补齐，满足 A8.net / もしも / afb 的审查前提（日语站 + 运营者信息披露）。

---

## 二、本次迭代（v1.1，已完成，未部署）

1. **privacy.html** — 日文隐私政策。10 个条款，明确写入「入力内容はサーバーへ送信されない」这一最大卖点，
   同时披露 Cloudflare Web Analytics 与 Google Fonts 两个外部服务。
2. **company.html** — 运营者情报。披露 運営者 `GUO XING`、所在地「大阪府大阪市」，
   番地/电话注明「請求があった場合には遅滞なく開示」，邮箱同样只在被要求时提供，公开联系口为既有 Google 表单。附 AboutPage JSON-LD。
3. **index.html** — 页脚新增「サイト情報」行：プライバシーポリシー / 運営者情報 / 書き方ガイド；
   SEO 区补一条指向教程页的内链；FAQ 标题加 `id="faqTitle"` 供锚点跳转。
4. **kakikata.html**（新教程页）— 「納品書の書き方・請求書との違いガイド」：
   発行日と納品日の違い / 単位欄の書き方（对照表）/ 納品書・請求書・受領書・領収書 四者对比表 /
   書類の流れ（見積→注文→納品→請求→領収）/ 作成手順 / FAQ 5 問。
   Article + FAQPage JSON-LD、canonical 自指、og:image 复用、首页 CTA ×4（顶部/中部/底部/页头）。
5. **guide.css** — 从 hanko-generator 复制并追加 `.legal` 系列样式，教程页与法务页共用。
6. **sitemap.xml** — 4 个 URL（首页 priority 1.0，教程 0.8，法务页 0.3）。

---

## 三、下一步迭代（按优先级）

### P0 — 部署与验收（下次上线时一起做）

- [ ] 部署 4 个 HTML + guide.css + sitemap.xml
- [ ] Google Search Console 提交 sitemap，请求收录 kakikata.html
- [ ] Rich Results Test 校验 Article / FAQPage 无报错
- [ ] 手机端实机确认：法务页 `.legal-table` 在窄屏不溢出、FAB 不遮挡页脚链接

### P1 — 内容层扩篇（SEO 长尾，每篇 1 个搜索意图）

| 篇目 | 目标关键词 | 状态 |
|---|---|---|
| 納品書の書き方・請求書との違い | 納品書 書き方 / 納品書 請求書 違い | ✅ 本次 |
| 納品書のテンプレート・記載例（業種別） | 納品書 テンプレート / 納品書 例 | 待做 |
| 納品書の再発行・訂正のやり方 | 納品書 再発行 / 納品書 訂正 | 待做 |
| 納品書はメール送付でよいか（PDF送付マナー） | 納品書 メール 送り方 | 待做 |
| 免税事業者・インボイス未登録の納品書 | 納品書 インボイス 免税 | 待做 |

要点：每篇复用 `guide.css`，canonical 自指，1 篇 = 1 个搜索意图，篇内 ≥3 次回首页 CTA，篇尾「あわせて使いたいツール」互链。

### P2 — 转化与联盟（ASP 过审后）

- [ ] ASP 过审后接入联盟位：会計ソフト（freee / マネーフォワード）、印鑑通販、ビジネスカード。
      **只挂在日语页**，工具主体区与法务页不挂。
- [ ] 广告位用 `.no-print`，避免打进用户的纳品书 PDF
- [ ] 教程页正文中段插 1 个原生文字链（比贴 banner 转化好且不伤体验）
- [ ] 在 privacy.html「4. Cookie 等および外部サービスの利用」补一句第三方广告投放说明（ASP 要求）

### P3 — 产品能力（后端留接口，为 app 迁移做准备）

- [ ] 数据层抽象：目前状态全在 DOM。抽一个 `state` 对象 + `render()`，未来接 localStorage / 后端
- [ ] 可选「下書きを保存」（localStorage，仅存本机，隐私政策已声明不上传，不冲突）
- [ ] 納品書 → 請求書 一键转化（与本矩阵 適格請求書 工具联动，强互链）

### P4 — 矩阵级复刻（重点）

法务三件套目前只有 japan-invoice-landing 与 nohinsho-pdf 具备。
**其余 13 个站按同一模板补齐 privacy.html + company.html**，是 ASP 批量过审的必要条件。

---

## 四、硬性验收清单（每次改动后跑）

- [ ] 所有页面 `<link rel="canonical">` 为绝对 URL 且自指
- [ ] 分享按钮 / 反馈 FAB 存在
- [ ] beacon token `6fc01bf3ac5e457ba1bd16b91e5b653e` 存在且每页仅一次
- [ ] print 时 `.feedback-fab` / 广告位 / 导航消失
- [ ] 站内互链可达（首页 ↔ 教程 ↔ 法务页）
- [ ] `node --check` 通过（JS 改动时）
- [ ] headless Chrome 打开无 console error / 无资源 404

---

## 五、红线提醒

- A8.net 明示「日本語で構成されているサイトのみ」→ **不得在任何页面挂中文/英文再挂联盟链接**
- 法务页承诺了「入力内容はサーバーへ送信されない」→ **未来加任何上传/保存后端功能前，必须先改 privacy.html**
- 运营者信息只披露到「大阪府大阪市」；番地・电话・邮箱一律「請求があった場合に開示」，不要在页面写死
