# 记账本 PWA

极简记账 · 零广告 · 数据存本地 · 隐私无忧

一个纯前端 PWA，可像原生 App 安装到手机主屏幕，离线可用。

## 功能

### 📋 流水

- 按月浏览所有记录，按日期分组显示
- 顶部收支汇总卡片：本月支出、收入、结余
- 点击任意条目进入编辑，可修改或删除
- 有图片的记录旁显示 📷 标记，点击查看大图

### ➕ 记一笔

- 支出 / 收入一键切换
- 自建数字键盘，输金额快
- 12 个默认支出分类 + 6 个收入分类，可自定义
- 备注 & 日期可选填
- 可拍照/选图（压缩到 ~60KB，存 OPFS，完全不占数据库体积）
- 编辑模式下底部双按钮：**删除** + **保存**

### 📊 报表

- 月度 / 年度收支趋势折线图
- 支出分类排行（金额 + 占比 + 进度条）

### ⚙️ 设置

- 自定义支出/收入分类（增删）
- 数据导出/导入（JSON 一键备份恢复）
- 清除全部数据（双重确认）
- 底部显示版本号 & 数据存储位置说明

### 🔒 静默备份

每记一笔自动在 OPFS 多存一份 `moneykeeper_backup.json`，App 启动时如果 IndexedDB 为空自动恢复。系统清理空间不删 OPFS 文件。

## 技术栈

| 层     | 方案 |
|--------|------|
| 框架   | 纯 HTML/CSS/JS，零框架依赖 |
| 数据   | IndexedDB（交易记录）+ localStorage（分类） |
| 图片   | OPFS 存文件，Canvas 前端压缩（800px / JPEG 0.7） |
| 备份   | OPFS 静默全量备份 + 自动恢复 |
| 图表   | Chart.js 4.x CDN（仅报表页加载） |
| 离线   | Service Worker cache-first，预缓存核心资源 |
| 部署   | GitHub Pages / 任意静态服务器 |

## 数据存储

```
IndexedDB                          localStorage
┌─────────────────────┐            ├─ mk_cats_expense → 支出分类
│ MoneyKeeper         │            └─ mk_cats_income  → 收入分类
│  └─ transactions    │
│     ├─ id           │            OPFS
│     ├─ type         │            ├─ moneykeeper_backup.json（全量备份）
│     ├─ amount       │            ├─ img_{id}.jpg（账目图片，可选）
│     ├─ category     │            └─ ...
│     ├─ note         │
│     ├─ date         │
│     ├─ yearMonth ← 索引
│     └─ createdAt    │
└─────────────────────┘
```

## 开发

```bash
git clone https://github.com/HMR03/money-keeper.git
cd money-keeper

# 本地预览
npx serve .
# 或
python -m http.server 8000
```

浏览器打开 `http://localhost:8000`。

## PWA 注意事项

- 每次修改代码部署后，**必须升级 `sw.js` 里的 `CACHE_NAME` 版本号**，否则手机端 Service Worker 会继续使用旧缓存，看不到更新。
- iOS 添加到主屏幕：Safari → 分享 → 添加到主屏幕
- Android 安装：Chrome → 菜单 → 添加到主屏幕 / 安装应用

## License

MIT
