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

## 0. 根索引 / Root Index

### GET `index.json`

返回 API 的总体索引，包含各端点路径。

> Returns the root index of the API, listing all endpoint paths.

**示例响应 / Example Response:**

```json
{
  "name": "Little Tree Clock Resources API",
  "version": "1",
  "base_url": "https://raw.githubusercontent.com/shu-shu-1/Little-Tree-Clock-Resources/main/",
  "endpoints": {
    "index": "index.json",
    "update": {
      "latest": "update/latest.json",
      "stable": "update/stable.json",
      "beta": "update/beta.json",
      "dev": "update/dev.json"
    },
    "plugins": "plugins/index.json",
    "announcements": "announcements/index.json"
  }
}
```

**字段说明 / Field Description:**

| 字段 / Field  | 类型 / Type         | 说明 / Description                                  |
| ------------- | ------------------- | --------------------------------------------------- |
| `name`        | string              | API 名称 / API name                                 |
| `version`     | string              | API 版本 / API version                              |
| `base_url`    | string              | 所有端点的基础 URL / Base URL for all endpoints     |
| `endpoints`   | object              | 端点路径映射 / Map of endpoint names to paths       |

---

## 1. 更新 API / Update API

支持多频道更新：`stable`（稳定版）、`beta`（测试版）、`dev`（开发版）。

> Supports multiple update channels: `stable`, `beta`, `dev`.

### GET `update/latest.json`

默认端点，指向 `stable` 频道。

> Default endpoint, points to `stable` channel.

### GET `update/{channel}.json`

返回指定频道的最新版本元数据。`channel` 可选值：`stable`、`beta`、`dev`。

> Returns metadata for the latest release of specified channel. Channel options: `stable`, `beta`, `dev`.

**频道说明 / Channel Description:**

| 频道 / Channel | 说明 / Description                           |
| -------------- | -------------------------------------------- |
| `stable`       | 稳定版，经过充分测试 / Stable, fully tested  |
| `beta`         | 测试版，新功能预览 / Beta, preview features  |
| `dev`          | 开发版，仅供开发者 / Dev, for developers only |

**示例响应 / Example Response:**

```json
{
  "channel": "stable",
  "version": "1.0.0",
  "release_date": "2026-03-03",
  "download_url": "https://github.com/shu-shu-1/Little-Tree-Clock/releases/download/v1.0.0/LittleTreeClock-1.0.0.zip",
  "changelog": "## 新增功能\n\n- 新增插件商店\n- 新增自动更新功能\n\n## 改进\n\n- 优化界面显示效果",
  "min_version": "0.1.0",
  "mandatory": false
}
```

**字段说明 / Field Description:**

| 字段 / Field    | 类型 / Type | 说明 / Description                                   |
| --------------- | ----------- | ---------------------------------------------------- |
| `channel`       | string      | 更新频道: stable / beta / dev                        |
| `version`       | string      | 最新版本号 / Latest version number                   |
| `release_date`  | string      | 发布日期 (YYYY-MM-DD) / Release date                 |
| `download_url`  | string      | 安装包下载地址 / Download URL for the installer      |
| `changelog`     | string      | 更新日志 (Markdown 格式) / Changelog in Markdown     |
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
      "file": "example-plugin.json"
    }
  ]
}
```

**字段说明 / Field Description:**

| 字段 / Field | 类型 / Type | 说明 / Description                                                     |
| ------------ | ----------- | ---------------------------------------------------------------------- |
| `id`         | string      | 插件唯一标识符 / Unique plugin identifier                            |
| `file`       | string      | 插件详情 JSON 的文件名，位于 `plugins/` 目录下 / Plugin detail JSON filename under `plugins/` |

### GET `plugins/{id}.json`

返回指定插件的详细元数据。

**响应格式：**

```json
{
  "id": "my_plugin",
  "name": "我的插件",
  "name_i18n": {
    "zh-CN": "我的插件",
    "en-US": "My Plugin"
  },
  "description": "一句话描述插件功能",
  "description_i18n": {
    "zh-CN": "一句话描述插件功能",
    "en-US": "One-line plugin description"
  },
  "version": "1.0.0",
  "author": "作者名",
  "icon": "https://example.com/icon.png",
  "download_url": "https://example.com/my_plugin-1.0.0.ltcplugin",
  "homepage": "https://github.com/yourname/my_plugin",
  "tags": ["notification", "alarm"],
  "plugin_type": "feature",
  "permissions": ["network", "install_pkg"],
  "supported_os": ["windows", "macos", "linux"],
  "min_host_version": "0.1.0",
  "updated_at": "2024-01-01"
}
```

---

## 字段说明

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | string | ✅ | 全局唯一标识符，`snake_case`，与 `plugin.json` 中 `id` 一致 |
| `name` | string \| object | ✅* | 插件显示名称；支持字符串或多语言对象（见 [i18n](#多语言支持i18n)） |
| `name_i18n` | object | | 名称多语言映射（可选），与 `name` 合并解析 |
| `description` | string \| object | | 插件简介；支持字符串或多语言对象 |
| `description_i18n` | object | | 描述多语言映射（可选），与 `description` 合并解析 |
| `version` | string | | 语义化版本号（如 `"1.0.0"`） |
| `author` | string | | 作者名或邮箱 |
| `icon` | string | | 插件图标 URL 或 Base64（`data:image/...;base64,...`） |
| `download_url` | string | | 插件安装包下载地址（`.py` 或 `.ltcplugin`） |
| `homepage` | string | | 插件主页或详情页 URL |
| `tags` | array[string] | | 分类标签 |
| `plugin_type` | string | | `"feature"`（默认）或 `"library"` |
| `permissions` | array[string] | | 所需系统权限声明（参考 `plugin.json` 的 `permissions`） |
| `supported_os` | array[string] | | 支持的操作系统列表，可选值：`"windows"` `"macos"` `"linux"` |
| `min_host_version` | string | | 要求的最低宿主版本（与 `plugin.json` 中 `min_host_version` 一致） |
| `updated_at` | string | | 最后更新日期（`YYYY-MM-DD`） |

> `*` 必填规则：`name` 与 `name_i18n` 至少提供一个即可。

### 字段解析优先级

客户端按以下优先级解析多语言文本：

1. `name` / `description` 若为 `{"zh-CN": "...", "en-US": "..."}` 形式的对象，直接按当前语言选择
2. `name_i18n` / `description_i18n` 的值会**合并到**上述对象中（`name_i18n` 优先）
3. 若 `name` 为纯字符串、`name_i18n` 非空，则合并为 `{"zh-CN": "原name", ...name_i18n}` 供语言选择
4. 若均为纯字符串，直接使用

### 向后兼容

- `min_app_version`：旧版商店文件中使用 `min_app_version` 表示最低版本要求，客户端仍会识别该字段作为 `min_host_version` 的回退。**新文件应使用 `min_host_version`**。


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
      "uuid": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
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
| `uuid`         | string      | 公告唯一 UUID / Unique announcement UUID                 |
| `id`           | string      | 公告序号 ID（已弃用，建议使用 uuid）/ Announcement ID (deprecated, use uuid) |
| `title`        | string      | 公告标题 / Announcement title                                   |
| `content`    | string      | 公告正文（完整内容）/ Full announcement body                    |
| `date`       | string      | 发布日期 (YYYY-MM-DD) / Publication date                        |
| `level`      | string      | 级别：`info` / `warning` / `error` / Severity level            |

---

## 如何维护 / How to Maintain

### 发布新版本 / Releasing a new version

1. 根据版本类型，编辑对应的频道文件：
   - 稳定版：`update/stable.json`（同时更新 `update/latest.json`）
   - 测试版：`update/beta.json`
   - 开发版：`update/dev.json`
2. 更新 `version`、`release_date`、`download_url`、`changelog` 等字段。
3. 将新版安装包上传至 GitHub Releases。

> 1. Edit the corresponding channel file based on version type:
>    - Stable: `update/stable.json` (also update `update/latest.json`)
>    - Beta: `update/beta.json`
>    - Dev: `update/dev.json`
> 2. Update `version`, `release_date`, `download_url`, `changelog` fields.
> 3. Upload the new installer to GitHub Releases.

### 上架新插件 / Publishing a new plugin

1. 在 `plugins/` 目录下创建 `{id}.json`，填写插件元数据，包括 `supported_os` 字段。
2. 将对应 `.py` 文件上传至 `plugins/files/` 目录（或其他可访问的 URL）。
3. 将该插件条目追加到 `plugins/index.json` 的 `plugins` 数组中。

`supported_os` 可选值为 `windows`、`macos`、`linux` 的任意组合，例如 `["windows"]` 表示仅支持 Windows，`["windows", "macos", "linux"]` 表示全平台支持。

> 1. Create `plugins/{id}.json` with plugin metadata, including the `supported_os` field.
> 2. Upload the `.py` file to `plugins/files/` (or any accessible URL).
> 3. Append the plugin entry to the `plugins` array in `plugins/index.json`.
>
> `supported_os` accepts any combination of `windows`, `macos`, and `linux`.

### 发布公告 / Publishing an announcement

在 `announcements/index.json` 的 `announcements` 数组中追加一条新记录，`id` 递增，`level` 可选 `info`、`warning` 或 `error`。

> Append a new entry to the `announcements` array in `announcements/index.json`. Increment `id`, and set `level` to `info`, `warning`, or `error`.

---

## 目录结构 / Directory Structure

```
.
├── README.md                        # 本文档 / This document
├── index.json                       # API 根索引 / API root index
├── update/
│   ├── latest.json                  # 默认更新（指向 stable）/ Default update (points to stable)
│   ├── stable.json                  # 稳定版元数据 / Stable version metadata
│   ├── beta.json                    # 测试版元数据 / Beta version metadata
│   └── dev.json                     # 开发版元数据 / Dev version metadata
├── plugins/
│   ├── index.json                   # 插件列表 / Plugin listing
│   ├── example-plugin.json          # 示例插件元数据 / Example plugin metadata
│   └── files/                       # 插件 .py 文件存放目录 / Plugin .py files
└── announcements/
    └── index.json                   # 公告列表（含完整内容）/ Announcements (full content)
```
