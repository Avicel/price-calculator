# 价格计算器 · Preiskalkulator · Price Calculator

三语价格计算器，适用于 eBay / Etsy 平台，针对 UG (haftungsbeschränkt) 公司结构设计。

---

## 功能

- 实时计算每件商品的完整价格结构
- 支持 eBay 和 Etsy 两个平台（分别计算，不做对比）
- 三语同步显示：中文 / Deutsch / English
- 保存多个商品，汇总成表格，一键导出 CSV（三语列标题，Excel 可直接打开）
- 全局设置存在浏览器本地，刷新不丢失

---

## 计算逻辑

```
售价 VKP（含税）
 └─ 减：MwSt. 19%（转交 Finanzamt，非公司收入）
 = 净营业额 Netto-Erlös

 └─ 减：平台费（净额，UG 可抵扣进项税）
      eBay：VKP × 佣金率 + 固定费/单
      Etsy：VKP × (交易费% + 支付费%) + 支付固定费 + 刊登费
 └─ 减：发货邮费（Versandmarke，实际邮资）
 └─ 减：履约服务费（包装 + 储存 + 打包 + 发货，固定 €0.80/件）
 └─ 减：进货价（EK-Preis）
 └─ 减：进货运费（EK-Versand，可选）
 └─ 减：进口关税（Einfuhrzoll，可选）
 = 税前利润 Betriebsgewinn

 └─ 减：企业税（Körperschaftsteuer + Gewerbesteuer，约 30%）
 = 净利润 Nettogewinn

另算：最低售价（SKP / Break-even）= 利润为零时的最低 VKP
```

> **UG 税务说明**：平台费（eBay / Etsy）包含 19% MwSt.，对 UG 来说可作进项税（Vorsteuer）抵扣，因此计算时直接使用净费率，不额外计入 MwSt. 成本。

---

## 全局设置说明

| 设置项 | 默认值 | 说明 |
|---|---|---|
| eBay 佣金率 | 12% | ⚠️ 2026年7月1日起改为 **14%**，届时需更新 |
| eBay 固定费/单 | €0.45 | 2026年2月起（原 €0.35） |
| Etsy 交易费 | 6.5% | 按 VKP 计算 |
| Etsy 支付处理费 | 4% | 按 VKP 计算 |
| Etsy 支付固定费 | €0.30 | 每单固定 |
| Etsy 刊登费 | €0.18 | 每件每次上架 |
| 企业税率 | 30% | Körperschaftsteuer ~15% + Gewerbesteuer ~15%，可按实际有效税率调整 |
| 默认履约服务费 | €0.80 | 包含包装、储存、打包、发货人工，每件固定 |

---

## 待办 / 已知需更新项

- [ ] **2026年7月1日**：eBay 佣金率从 12% 改为 14%（在设置里手动更新）
- [ ] 如添加 Amazon 平台，参考 eBay 逻辑新增费率配置

---

## 本地开发

纯静态单文件，无需构建步骤：

```bash
# 直接用浏览器打开
open index.html

# 或起一个本地服务器
python3 -m http.server 3456
# 访问 http://localhost:3456
```

## 部署

每次修改 `index.html` 后：

```bash
git add index.html
git commit -m "描述修改内容"
git push
```

Netlify 连接此 repo 后会自动重新部署（通常 1 分钟内生效）。
