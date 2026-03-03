# Little-Tree-Clock-Resources

小树时钟静态 API 仓库 / Static API repository for Little Tree Clock

本仓库作为小树时钟软件的静态 API 后端，托管于 GitHub，通过原始文件 URL 对外提供服务。所有端点均返回 JSON 格式数据。

> This repository serves as the static API backend for Little Tree Clock. All endpoints return JSON.

---

## Base URL

```
https://raw.githubusercontent.com/shu-shu-1/Little-Tree-Clock-Resources/main/
```

---

## 1. 更新 API / Update API

### GET `update/latest.json`

返回最新版本的元数据（不含安装包本体）。

> Returns metadata for the latest release (no binary included).

**示例响应 / Example Response:**

```json
{
  "version": "1.0.0",
  "release_date": "2026-03-03",
  "download_url": "https://github.com/shu-shu-1/Little-Tree-Clock/releases/download/v1.0.0/LittleTreeClock-1.0.0.zip",
  "changelog_url": "https://github.com/shu-shu-1/Little-Tree-Clock/releases/tag/v1.0.0",
  "min_version": "0.1.0",
  "mandatory": false
}
```

**字段说明 / Field Description:**

| 字段 / Field    | 类型 / Type | 说明 / Description                                   |
| --------------- | ----------- | ---------------------------------------------------- |
| `version`       | string      | 最新版本号 / Latest version number                   |
| `release_date`  | string      | 发布日期 (YYYY-MM-DD) / Release date                 |
| `download_url`  | string      | 安装包下载地址 / Download URL for the installer      |
| `changelog_url` | string      | 更新日志页面 / Changelog page URL                    |
| `min_version`   | string      | 支持自动升级的最低版本 / Minimum upgradeable version |
| `mandatory`     | boolean     | 是否为强制更新 / Whether the update is mandatory     |

---

## 2. 插件商店 API / Plugin Store API

### GET `plugins/index.json`

返回所有插件的元数据列表（不含插件源码本体）。

> Returns the list of all available plugins with metadata only (no source code).

**示例响应 / Example Response:**

```json
{
  "plugins": [
    {
      "id": "example-plugin",
      "name": "Example Plugin",
      "description": "An example plugin for Little Tree Clock.",
      "version": "1.0.0",
      "author": "Little Tree",
      "download_url": "https://github.com/shu-shu-1/Little-Tree-Clock-Resources/raw/main/plugins/files/example-plugin-1.0.0.py",
      "homepage": "https://github.com/shu-shu-1/Little-Tree-Clock-Resources/blob/main/plugins/example-plugin.json",
      "tags": ["example"],
      "updated_at": "2026-03-03",
      "min_app_version": "1.0.0"
    }
  ]
}
```

### GET `plugins/{id}.json`

返回指定插件的详细元数据。

> Returns detailed metadata for a specific plugin.

**字段说明 / Field Description:**

| 字段 / Field       | 类型 / Type   | 说明 / Description                                         |
| ------------------ | ------------- | ---------------------------------------------------------- |
| `id`               | string        | 插件唯一标识符 / Unique plugin identifier                  |
| `name`             | string        | 插件名称 / Plugin display name                             |
| `description`      | string        | 插件简介 / Short description                               |
| `version`          | string        | 插件版本号 / Plugin version                                |
| `author`           | string        | 作者 / Author name                                         |
| `download_url`     | string        | `.py` 文件下载地址 / URL to download the `.py` plugin file |
| `homepage`         | string        | 插件主页或详情页 / Plugin homepage or detail page          |
| `tags`             | array[string] | 标签列表 / Tag list                                        |
| `updated_at`       | string        | 最后更新日期 (YYYY-MM-DD) / Last updated date              |
| `min_app_version`  | string        | 所需最低应用版本 / Minimum required app version            |

---

## 3. 公告 API / Announcement API

### GET `announcements/index.json`

返回所有公告的完整内容。

> Returns all announcements with full content.

**示例响应 / Example Response:**

```json
{
  "announcements": [
    {
      "id": "1",
      "title": "欢迎使用小树时钟！",
      "content": "小树时钟现已支持插件商店与自动更新功能，欢迎体验！\n\nWelcome to Little Tree Clock! The app now supports a plugin store and automatic updates.",
      "date": "2026-03-03",
      "level": "info"
    }
  ]
}
```

**字段说明 / Field Description:**

| 字段 / Field | 类型 / Type | 说明 / Description                                              |
| ------------ | ----------- | --------------------------------------------------------------- |
| `id`         | string      | 公告唯一 ID / Unique announcement ID                            |
| `title`      | string      | 公告标题 / Announcement title                                   |
| `content`    | string      | 公告正文（完整内容）/ Full announcement body                    |
| `date`       | string      | 发布日期 (YYYY-MM-DD) / Publication date                        |
| `level`      | string      | 级别：`info` / `warning` / `error` / Severity level            |

---

## 如何维护 / How to Maintain

### 发布新版本 / Releasing a new version

编辑 `update/latest.json`，更新 `version`、`release_date`、`download_url` 等字段，并将新版安装包上传至 Releases。

> Edit `update/latest.json`, update the `version`, `release_date`, and `download_url` fields, then upload the new installer to GitHub Releases.

### 上架新插件 / Publishing a new plugin

1. 在 `plugins/` 目录下创建 `{id}.json`，填写插件元数据。
2. 将对应 `.py` 文件上传至 `plugins/files/` 目录（或其他可访问的 URL）。
3. 将该插件条目追加到 `plugins/index.json` 的 `plugins` 数组中。

> 1. Create `plugins/{id}.json` with plugin metadata.
> 2. Upload the `.py` file to `plugins/files/` (or any accessible URL).
> 3. Append the plugin entry to the `plugins` array in `plugins/index.json`.

### 发布公告 / Publishing an announcement

在 `announcements/index.json` 的 `announcements` 数组中追加一条新记录，`id` 递增，`level` 可选 `info`、`warning` 或 `error`。

> Append a new entry to the `announcements` array in `announcements/index.json`. Increment `id`, and set `level` to `info`, `warning`, or `error`.

---

## 目录结构 / Directory Structure

```
.
├── README.md                        # 本文档 / This document
├── update/
│   └── latest.json                  # 最新版本元数据 / Latest version metadata
├── plugins/
│   ├── index.json                   # 插件列表 / Plugin listing
│   ├── example-plugin.json          # 示例插件元数据 / Example plugin metadata
│   └── files/                       # 插件 .py 文件存放目录 / Plugin .py files
└── announcements/
    └── index.json                   # 公告列表（含完整内容）/ Announcements (full content)
```
