# GradeRelease - 高校期末成绩模拟发布系统

一个基于 GitHub Pages 的纯前端成绩发布与查询平台，无需后端服务器，管理员上传 Excel 成绩文件即可生成可分享的查询链接，学生通过姓名+学号安全查询个人成绩。

## 在线访问

- 首页：[https://caoyongshengcys.github.io/GradeRelease/](https://caoyongshengcys.github.io/GradeRelease/)
- 管理员发布：[https://caoyongshengcys.github.io/GradeRelease/admin.html](https://caoyongshengcys.github.io/GradeRelease/admin.html)
- 学生查询：`https://caoyongshengcys.github.io/GradeRelease/query.html?data=课程名.json`

## 项目架构

```
GradeRelease/
├── index.html            # 首页 - 系统入口与使用说明
├── admin.html            # 管理员页面 - 上传Excel、生成JSON数据、获取查询链接
├── query.html            # 学生查询页面 - 输入姓名+学号查询成绩
├── css/
│   └── style.css         # 全局样式表（响应式设计）
├── data/                 # 成绩数据目录（存放生成的JSON文件）
│   └── *.json            # 各课程成绩数据文件
└── README.md
```

## 技术方案

```
┌─────────────────────────────────────────────────────┐
│                    GitHub Pages                      │
│                  (静态网站托管)                        │
│                                                      │
│  ┌──────────┐  ┌──────────┐  ┌───────────────────┐  │
│  │ index    │  │ admin    │  │ query.html        │  │
│  │ .html    │  │ .html    │  │ (学生查询页面)      │  │
│  │ (首页)   │  │ (管理端)  │  │                   │  │
│  └──────────┘  └────┬─────┘  └────────┬──────────┘  │
│                     │                  │             │
│              上传Excel解析        fetch加载JSON       │
│              生成JSON文件              │             │
│                     │                  │             │
│                     ▼                  ▼             │
│               ┌─────────────────────────┐           │
│               │     data/*.json          │           │
│               │   (成绩数据存储)          │           │
│               └─────────────────────────┘           │
└─────────────────────────────────────────────────────┘
```

### 核心技术栈

| 技术 | 用途 |
|------|------|
| HTML / CSS / JavaScript | 纯前端实现，无框架依赖 |
| [SheetJS (xlsx)](https://sheetjs.com/) | 浏览器端解析 Excel (.xlsx) 文件 |
| GitHub Pages | 免费静态网站托管，提供互联网访问 |
| JSON | 成绩数据存储格式 |

## 使用流程

### 管理员发布成绩

1. 打开 `admin.html` 管理员页面
2. 上传从教务系统导出的 **平时成绩** 和 **期末成绩** Excel (.xlsx) 文件
3. 输入 **课程名称**，设置平时/期末成绩 **占比权重**
4. 点击「生成成绩数据文件」，下载生成的 JSON 文件
5. 将 JSON 文件上传到本仓库 `data/` 目录下
6. 系统自动生成查询链接，分享给学生

### 学生查询成绩

1. 通过管理员分享的链接打开查询页面
2. 输入 **姓名** 和 **学号**（两者必须同时匹配）
3. 查看成绩详情：
   - 平时成绩（含各课程目标分项明细）
   - 期末成绩（含各课程目标分项明细）
   - 总评成绩及等级

## 数据格式

生成的 JSON 数据结构如下：

```json
{
  "course": "课程名称",
  "semester": "2025-2026 第一学期",
  "weightUsual": 50,
  "weightFinal": 50,
  "publishTime": "2026-03-18T...",
  "count": 46,
  "students": [
    {
      "id": "学号",
      "name": "姓名",
      "class": "班级",
      "usual": {
        "subHeaders": ["课程目标1", "课程目标2", "..."],
        "subScores": [82.3, 88.4, "..."],
        "total": 91.2
      },
      "final": {
        "subHeaders": ["课程目标1", "课程目标2", "..."],
        "subScores": [25, 24, "..."],
        "total": 90
      },
      "totalScore": 90.6
    }
  ]
}
```

## 特性

- **零后端**：纯前端静态方案，GitHub Pages 免费托管
- **隐私安全**：姓名+学号双重验证，浏览器端匹配，数据不经过第三方服务器
- **多课程支持**：每门课程一个 JSON 文件，课程名称自由输入
- **权重可调**：平时/期末成绩占比自定义
- **响应式设计**：适配 PC 和移动端
- **分项成绩**：自动解析并展示各课程目标的子分数明细

## 部署指南

1. Fork 本仓库或克隆到本地
2. 在 GitHub 仓库 **Settings → Pages** 中开启 GitHub Pages
3. Source 选择 `Deploy from a branch`，Branch 选择 `main`，目录选 `/`
4. 等待部署完成即可通过 `https://用户名.github.io/仓库名/` 访问

## License

[MIT](LICENSE)
