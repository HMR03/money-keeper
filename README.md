# 记账本 PWA

极简记账 · 零广告 · 数据存本地 · 隐私无忧

一个纯前端 PWA（渐进式网页应用），可像原生 App 一样安装到手机主屏幕，支持离线使用。

## 功能

- **记一笔** — 支出/收入，数字键盘，分类选择，备注 & 日期
- **流水页** — 按月查看，收支汇总，点击条目可编辑
- **报表** — 月度/年度趋势图（Chart.js），支出分类排行
- **分类管理** — 自定义支出/收入分类，添加删除
- **数据导出/导入** — JSON 格式，一键备份恢复
- **OPFS 静默备份** — 每记一笔自动在浏览器私密文件区多存一份，IndexedDB 数据丢失时自动恢复
- **PWA** — 可安装到桌面，离线可用

## 技术栈

| 层 | 方案 |
|---|---|
| 框架 | 纯 HTML/CSS/JS，零依赖（Chart.js CDN 仅报表页） |
| 数据存储 | IndexedDB（交易记录）+ localStorage（分类） |
| 静默备份 | OPFS（Origin Private File System） |
| 离线 | Service Worker，cache-first 策略 |
| 图表 | Chart.js 4.x |
| 部署 | GitHub Pages |

## 安装到手机

1. Safari/Chrome 打开网址
2. iOS：点击底部「分享」→「添加到主屏幕」
3. Android：点击菜单 →「添加到主屏幕」或「安装应用」

## 数据存储

### 交易记录 → IndexedDB

```
数据库: MoneyKeeper
表: transactions
索引: yearMonth（按月查询）
```

### 分类 → localStorage

```
mk_cats_expense  → 支出分类
mk_cats_income   → 收入分类
```

### 备份 → OPFS

每次记账成功后自动将全量数据写入 `moneykeeper_backup.json`，存放于浏览器 OPFS 私密文件区。系统清理空间时不会删除 OPFS 文件。

App 启动时如果检测到 IndexedDB 为空且 OPFS 有备份，自动恢复数据。

## 开发

```bash
# 克隆仓库
git clone https://github.com/HMR03/money-keeper.git
cd money-keeper



