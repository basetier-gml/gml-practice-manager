# GML 练习记录 · 项目存档管理器

[![单文件应用](https://img.shields.io/badge/单文件-HTML-blue)](#) [![中英双语](https://img.shields.io/badge/中英-双语-green)](#) [![GitHub Pages](https://img.shields.io/badge/hosted-GitHub%20Pages-222)](#)

> **一个网页，多个互不干扰的项目存档。** 给 GML 打卡练习网页套上一层项目管理器：数据落盘到你自己的文件夹，网页的每一次自动存档都由管理器接管，清除浏览器缓存也不会丢。

**English:** A single-file project manager for the [GML practice log](https://basetier-gml.github.io/gml-practice-log/). It turns one web page into many isolated project saves, takes over the page's auto-save, and stores everything in a folder you own — nothing lives in browser storage.

---

## 为什么需要它

原打卡网页把数据存在浏览器的 `localStorage` 里：清理缓存、换浏览器、换设备，记录就没了，也没法把不同练习拆成独立项目。

这个管理器解决了三件事：

- **数据归你**：所有存档写成文件夹里的 `.json` 文件，浏览器只记一个文件夹位置。
- **多项目隔离**：一个网页任意切换多个项目，互不干扰。
- **存档可更新**：打卡网页作为独立模块，可以随时抓取新版本，不必重做管理器。

---

## 功能特性

| 特性 | 说明 |
|---|---|
| 📂 **文件夹即真相源** | 打开时扫描文件夹里的 `.json` 存档，列表完全由真实文件决定，并显示文件名、大小、修改时间 |
| 🔁 **自动恢复上次路径** | 首次选一次文件夹，之后打开自动恢复并扫描，不再弹选择菜单 |
| 🗑 **回收站 + 垃圾回收文件** | 删除先把文件移进回收站并登记清单，可随时恢复；清空回收站才真正删除 |
| 💾 **接管自动存档** | 网页的每次自动保存都写回当前打开的项目文件，不再塞进浏览器 |
| 🔄 **打卡网页独立更新** | 内置版本兜底，可一键从来源抓取最新版并缓存到文件夹，随时切回内置版 |
| 🌐 **中英双语联动** | 顶部切换 `中文 / EN`，管理页与打卡页整页同步切换，路径始终不含中文 |
| 🔒 **隐私优先** | 浏览器 `localStorage` 只有上次文件夹名，IndexedDB 只有目录句柄，**零项目数据** |

---

## 界面预览

| 中文管理页 | 英文管理页 |
|---|---|
| ![中文管理页](gml-manager-zh.png) | ![英文管理页](gml-manager-en.png) |

![打卡页面](gml-checkin-en.png)

---

## 快速开始

### 在线使用（推荐）

打开部署地址：**https://basetier-gml.github.io/gml-practice-manager/**

1. **首次**：点「选择文件夹」，挑一个专门存放存档的本地文件夹（如 `GML-存档`）。
2. **新建项目**：点「＋ 新建项目」，输入名称，列表里就会出现一个空白项目。
3. **开始练习**：点项目卡片的「打开项目 →」，打卡网页加载后即为该项目，自动存档会写回对应文件。
4. **下次打开**：自动恢复上次的文件夹并扫描，无需再选。

### 本地使用

下载本仓库的 `index.html`（即成品网页），用 **Chrome / Edge 桌面浏览器**直接打开即可。功能与在线版一致；由于 File System Access API 的限制，请勿用 Safari 或手机浏览器。

---

## 存储说明

管理器在你选择的文件夹里创建以下固定结构（目录名永远为英文，与界面语言无关）：

```
你选择的文件夹/
├── GML-Saves/                  # 项目存档
│   ├── save-index.json         # 存档索引（含项目列表与界面语言偏好）
│   └── <项目名>.<ID>.json      # 每个项目一个存档文件
├── GML-Trash/                  # 回收站
│   ├── trash-index.json        # 垃圾回收文件：记录所有移入回收站的项目
│   └── <ID>.<删除时间>.json    # 被删除项目的文件
└── GML-App/                    # 打卡网页缓存（抓取最新版后才有）
    ├── app.html                # 缓存的网页
    └── app-meta.json           # 来源地址、版本、抓取时间、大小
```

**浏览器里只存两样东西**：`localStorage` 中上次选择的文件夹名（便于提示），以及 IndexedDB 中该文件夹的访问句柄。**没有任何项目数据存进浏览器。** 清除站点数据只会忘记文件夹位置，重新选回同一个文件夹，项目和回收站条目会完整恢复。

---

## 打卡网页版本管理

管理页与打卡网页是解耦的：管理页是稳定的主程序，打卡网页是可独立更新的模块。

- **内置版本**：出厂自带一份，开箱即用。
- **⇩ 抓取最新版本**：从来源地址（默认 `https://basetier-gml.github.io/gml-practice-log/`）重新下载网页，缓存到文件夹的 `GML-App/`，之后所有项目都用这份新版。抓取时会校验内容确实是打卡网页，防止抓错。
- **更换来源**：填一个新的公开网页地址即可切换。
- **恢复内置版**：删掉缓存，回到出厂版本。

网页更新后，只要往来源仓库推新版，再点一次「抓取最新版本」即可，无需改动管理器。

---

## 部署到 GitHub Pages

本仓库已开启 Pages，把成品网页命名为 `index.html` 放在仓库根目录即可访问：

```
https://basetier-gml.github.io/gml-practice-manager/
```

步骤：仓库 **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: main / (root) → Save**，等待 1～2 分钟。以后更新网页，只要推送新的 `index.html` 就会自动重新发布。

---

## 浏览器要求

需要支持 **File System Access API** 的桌面浏览器：**Chrome / Edge**（推荐）。Safari、Firefox、手机浏览器不支持选择文件夹，无法使用完整功能。Pages 是 HTTPS，满足该 API 的安全上下文要求。

---

## 技术说明

- **单文件成品**：全部逻辑、样式与内置网页打包进一个 HTML，无需服务器、无需依赖，双击即用。
- **存档接管**：在网页脚本启动前注入一层虚拟 `localStorage`，把网页的读写重定向到当前项目文件，通过 `postMessage` 与管理器通信；网页自带的 `gml.cmd('export'/'import'/'lang')` 接口用于导入导出与语言联动。
- **零浏览器存储**：除文件夹句柄与文件夹名外不写入任何浏览器存储，符合"数据只存文件夹"的设计。
- **多语言**：管理页内置中英文字典，打卡页通过其自带的 i18n 字典渲染，二者由管理器语言统一主控。

---

## 相关链接

- 打卡网页原项目：<https://basetier-gml.github.io/gml-practice-log/>
- 本管理器在线版：<https://basetier-gml.github.io/gml-practice-manager/>
