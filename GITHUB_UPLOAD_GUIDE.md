# SmartFillMonitor GitHub 上传建议

## 一、项目概况

- **项目类型**: WPF (.NET 8.0) 桌面应用
- **当前状态**: 非 Git 仓库，无 .gitignore
- **根目录结构**:

```
SmartFillMonitor/
├── .vs/                          # VS  IDE 缓存
├── Logs/                         # 运行时日志（空目录）
├── Readme.assets/                # README 截图资源
├── Readme.md                     # 项目说明
├── SmartFillMonitor.db           # SQLite 运行时数据库
├── SmartFillMonitor.slnx         # 解决方案文件 (VS 新格式)
└── SmartFillMonitor/             # 主项目
    ├── bin/                      # 编译输出
    ├── obj/                      # 中间产物
    ├── App.xaml(.cs)
    ├── AssemblyInfo.cs
    ├── MainWindow.xaml(.cs)
    ├── MainWindowViewModel.cs
    ├── Models/
    ├── Services/
    ├── ViewModels/
    ├── Views/
    ├── UserControls/
    ├── Converters/
    ├── Assests/styles/
    └── Properties/
```

---

## 二、必须加入 .gitignore 的项

### 1. Visual Studio / IDE 文件

```gitignore
# Visual Studio
.vs/
*.suo
*.user
*.userosscache
*.sln.docstates
```

### 2. 编译输出 & 中间产物

```gitignore
# Build results
bin/
obj/
*.dll
*.exe
*.pdb
```

### 3. 运行时数据（关键！）

```gitignore
# Runtime data
*.db
*.db-shm
*.db-wal
Logs/
device-settings.json
```

### 4. NuGet / 包管理

```gitignore
# NuGet
*.nupkg
**/[Pp]ackages/*
*.nuget.props
*.nuget.targets
```

### 5. 操作系统临时文件

```gitignore
# OS files
Thumbs.db
.DS_Store
*.swp
*.tmp
```

---

## 三、需要删除的文件/目录（不要上传）

| 文件/目录 | 原因 | 处理方式 |
|-----------|------|----------|
| `.vs/` | VS IDE 自动生成，每台机器不同 | 加入 .gitignore，本地保留 |
| `SmartFillMonitor/bin/` | 编译输出，含第三方 DLL、NuGet runtime 等（约 100+ 文件） | 加入 .gitignore，本地保留 |
| `SmartFillMonitor/obj/` | 中间编译产物（project.assets.json 等） | 加入 .gitignore，本地保留 |
| `SmartFillMonitor.db` | SQLite 运行时数据库，含本地运行数据 | 加入 .gitignore，本地保留 |
| `Logs/` | 运行时日志目录（当前为空） | 加入 .gitignore，本地保留 |
| `device-settings.json` (在 bin/Debug 内) | 运行时生成的设备配置文件 | bin/ 已整体忽略，无需单独处理 |

> **注意**: 以上文件/目录不需要手动删除，加上 .gitignore 后 git 会自动忽略它们。建议保留在本地不删，否则程序运行时可能报错。

---

## 四、建议上传到 GitHub 的内容

| 文件/目录 | 说明 |
|-----------|------|
| `SmartFillMonitor.slnx` | 解决方案文件，协作者需要它打开项目 |
| `SmartFillMonitor/*.csproj` | 项目文件，定义依赖和编译选项 |
| `SmartFillMonitor/*.xaml` | WPF 界面文件 |
| `SmartFillMonitor/*.cs` | C# 源代码 |
| `SmartFillMonitor/Models/` | 数据模型 |
| `SmartFillMonitor/Services/` | 业务逻辑服务 |
| `SmartFillMonitor/ViewModels/` | MVVM ViewModel |
| `SmartFillMonitor/Views/` | 视图 |
| `SmartFillMonitor/UserControls/` | 自定义控件 |
| `SmartFillMonitor/Converters/` | 值转换器 |
| `SmartFillMonitor/Assests/` | 静态资源（主题样式） |
| `SmartFillMonitor/Properties/` (除 PublishProfiles 外可选) | 程序集信息 |
| `Readme.md` | 项目文档 |
| `Readme.assets/` | README 引用的截图 |

---

## 五、额外建议

### 5.1 PublishProfiles 是否上传？

`Properties/PublishProfiles/FolderProfile.pubxml` 包含发布配置。**建议忽略**，因为每个开发者的发布路径不同：

```gitignore
# Publish profiles
**/PublishProfiles/*.pubxml
```

### 5.2 解决方案文件格式

你使用的是 `.slnx`（Visual Studio 新格式）。确保协作者使用 **VS 2022 17.10+** 才能打开。如果团队中有用旧版 VS 的人，建议保留传统 `.sln` 文件。

### 5.3 敏感信息检查

在上传前，检查以下文件是否包含敏感信息（IP 地址、串口号、密码等）：
- `Services/PlcService.cs` — Modbus PLC 连接配置
- `Services/ConfigServices.cs` — 系统配置
- `Services/DbProvider.cs` — 数据库连接串

---

## 六、完整 .gitignore 模板

```gitignore
# Visual Studio
.vs/
*.suo
*.user
*.userosscache
*.sln.docstates

# Build outputs
[Dd]ebug/
[Rr]elease/
bin/
obj/

# NuGet
*.nupkg
**/[Pp]ackages/*

# Runtime data
*.db
*.db-shm
*.db-wal
Logs/

# Publish profiles
**/PublishProfiles/

# OS files
Thumbs.db
.DS_Store
```

---

## 七、操作步骤

```bash
# 1. 进入项目目录
cd SmartFillMonitor

# 2. 创建 .gitignore（将上面的内容粘贴进去）

# 3. 初始化 Git 仓库
git init

# 4. 添加所有文件（.gitignore 会自动过滤）
git add .

# 5. 检查暂存区确认无误
git status

# 6. 首次提交
git commit -m "Initial commit: SmartFillMonitor WPF application"

# 7. 关联 GitHub 远程仓库并推送
git remote add origin <你的GitHub仓库URL>
git branch -M main
git push -u origin main
```

---

## 八、总结

- **要上传**: 源代码(.cs/.xaml)、项目文件(.csproj/.slnx)、文档(Readme.md)、静态资源(Assests/)
- **不要上传**: 编译产物(bin/obj/)、IDE 缓存(.vs/)、运行时数据(.db/Logs/)、发布配置(PublishProfiles/)
- **风险点**: 检查 PlcService.cs / ConfigServices.cs 里是否有硬编码的 IP、串口号或密码
