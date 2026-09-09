# HIT-Quick-App-TODO

基于快应用（Quick App）技术栈实现的 TODO 事项管理应用，来自本科阶段的课程实践和移动端开发实验。项目围绕“记录事项—设置时间—跟踪状态—查看统计”的完整流程展开，并保留了当时的页面设计、组件拆分和本地持久化实现。

## Overview

应用将事项划分为 `TODO`、`DOING` 和 `DONE` 三种状态。用户可以创建事项、设置开始时间和截止时间、编辑事项、标记完成或删除已完成事项；主页负责列表管理，统计页通过 Canvas 展示事项数量和截止时间分布。

项目使用快应用页面组件和系统能力接口，不依赖后端数据库。事项数据保存在设备本地存储中，语音输入、震动反馈和桌面快捷方式属于设备/平台能力。

## Features

- TODO / DOING / DONE 三状态事项管理。
- 新建和编辑事项，支持任务名称、开始时间和截止时间。
- 支持无截止日期事项。
- 事项完成和删除操作。
- 使用本地 storage 保存事项列表，重新进入页面时恢复数据。
- 使用 Canvas 绘制事项数量曲线和截止时间分布图。
- 使用 `service.asr` 提供语音输入入口。
- 使用系统震动和 Toast 提供操作反馈。
- 支持通过系统快捷方式将应用添加到桌面。

## Tech Stack

- Quick App / HAP
- UX single-file components
- JavaScript
- Less
- `@system.storage`
- `@system.router`
- `@system.prompt`
- `@system.shortcut`
- `@system.vibrator`
- `@service.asr`
- HAP Toolkit 1.9.5

## Architecture

```text
App
├── MainPage
│   ├── TODO list
│   ├── DOING list
│   ├── DONE list
│   └── Canvas statistics
└── Input
    ├── Task name input
    ├── Date/time picker
    ├── Optional deadline
    └── Voice input
```

事项对象使用 `name`、`start` 和 `end` 字段，并按照状态数组存储：

```json
{ "toDoList": [], "doingList": [], "doneList": [] }
```

## Project Structure

```text
src/
├── MainPage/index.ux          # 主页面、状态列表和统计图
├── MainPage/main-page-item.ux # 单个事项组件
├── Input/index.ux             # 新建/编辑事项页面
├── Common/                    # 图标、字体和样式资源
├── app.ux                     # 应用级能力注册
├── global.js                  # 全局数据辅助函数
├── util.js                    # 菜单、快捷方式和 Toast
└── manifest.json              # 应用元数据、路由和系统能力
docs/
├── course-practice-requirements.pdf
├── todo-project-report.docx
└── demo.mp4
```

## Manifest and Platform Requirements

根据历史 `src/manifest.json`：

- 应用包名：`com.application.demo`
- 应用版本：`1.0.0`
- 最低平台版本：`1060`
- 目标设备：手机
- 入口页面：`MainPage`
- 使用系统存储、路由、提示、快捷方式、语音识别和震动能力。

## Prerequisites

这是旧版快应用工程，原始 `package.json` 使用 Node.js `>=8.10` 和 HAP Toolkit。建议在隔离的旧 Node 环境中运行，避免与现代项目共用依赖。

## Installation

```bash
npm install
```

如果需要使用指定的历史工具链，可以根据 `package.json` 安装 HAP Toolkit 和 ESLint 相关开发依赖。当前项目的 `package-lock.json` 保留了当时的依赖解析结果。

## Available Commands

| Command | Purpose |
| --- | --- |
| `npm run start` | 启动开发服务器并监听文件变化 |
| `npm run server` | 启动开发服务器 |
| `npm run watch` | 监听并构建项目 |
| `npm run build` | 构建 HAP 应用 |
| `npm run release` | 生成发布包 |
| `npm run lint` | 检查并修复 UX/JavaScript 代码风格 |

## Usage Flow

1. 启动快应用开发工具或开发服务器。
2. 在 `MainPage` 中查看 TODO、DOING 和 DONE 列表。
3. 点击添加按钮进入 `Input` 页面。
4. 输入事项名称，选择开始时间和结束时间，也可以选择无截止日期。
5. 保存后，事项会按照开始时间进入 TODO 或 DOING。
6. 点击事项可以再次编辑，点击完成图标可以移动到 DONE。
7. 在统计页面查看事项数量和截止时间分布。

## Historical Results

`docs/demo.mp4` 保存了课程实践时期的应用演示；`docs/todo-project-report.docx` 保存了当时的项目说明/报告。两者用于记录历史实现，不代表项目已经在现代快应用工具链上重新构建。

## Historical Course Materials

`docs/course-practice-requirements.pdf` 是过往课程实践要求，仅用于课程内容回顾、实验整理和历史归档，不用于当前课程作业、考试或抄袭。

## Known Limitations

- 依赖旧版 HAP Toolkit、较老的 Node.js 生态和 Chrome 65 目标环境。
- `service.asr`、桌面快捷方式和震动能力依赖具体快应用运行环境。
- 事项数据只保存在设备本地，不提供云同步或跨设备备份。
- 统计图的曲线计算在所有状态数量相同的边界情况下可能需要额外处理。
- Manifest 使用较宽松的历史平台配置，若重新发布应按照当前快应用平台规范重新检查。

## Asset Note

项目包含 Montserrat 字体和图标资源。公开再分发前应核对字体和图片资源的许可证；如无法确认，可替换为已确认许可的资源。
