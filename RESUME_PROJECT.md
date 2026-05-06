# SmartFillMonitor — 智能灌装生产监控系统

## 简历项目经验（建议版本一：详实版）

**项目名称**: SmartFillMonitor 智能灌装产线实时监控平台

**项目时间**: 2025.03 — 2025.05（按实际调整）

**项目描述**:
基于 WPF + .NET 8 开发的工业自动化上位机监控系统，通过 Modbus RTU 协议与 PLC 实时通信，对灌装产线的设备状态、生产节拍、温度液位、阀门动作等关键参数进行毫秒级采集与可视化展示，实现生产过程的全链路数字化监控与异常告警。

**技术栈**: WPF / .NET 8 / CommunityToolkit.Mvvm / FreeSql + SQLite / Serilog / Modbus RTU (NModbus4) / LiveCharts / HandyControl / DI 依赖注入

**核心职责与成果**:

- **PLC 实时数据采集与指令下发**：封装串口通信服务（异步锁、超时重试、断线重连），通过 Modbus RTU 协议以 200ms 轮询周期读取 PLC 保持寄存器（温度、液位、节拍、产量、阀门状态），支持 ASCII 编码条码解析；实现上位机一键启停/复位指令的线圈写入。

- **MVVM 架构设计**：全量采用 CommunityToolkit.Mvvm 源生成器实现 MVVM 模式，结合 Microsoft.Extensions.DependencyInjection 构建依赖注入容器，统一管理 Service / ViewModel 生命周期，视图间通过 DI 切换实现 Dashboard → 报警查询 → 历史数据 → 日志 → 系统设置的导航。

- **生产数据持久化与追溯**：基于 FreeSql ORM + SQLite 设计 ProductionRecord / AlarmRecord 等数据实体，扫码触发自动记录批次产量、温度、节拍、NG 状态等信息至本地数据库，支持历史数据查询与 CSV 导出（CsvHelper）。

- **异常监测与分级告警**：实现原料液位过低、气压偏低、加热超温、PLC 通信故障、系统内部错误五类告警码 + 四级严重程度（提示/警告/错误/致命），触发时自动写库、UI 实时弹窗并记录确认人及处理时间。

- **实时可视化仪表盘**：LiveCharts 动态折线图展示温度趋势（滑动窗口），自定义状态指示灯控件（运行/待机/故障三色），HandyControl 统一 UI 风格。

**项目亮点**: 200ms 级实时轮询 · Modbus 串口异步锁防冲突 · 断线自动重连与状态机管理 · 条码变化触发自动录单 · 四级分级告警系统

---

## 简历项目经验（建议版本二：精简版—适合一页简历）

**SmartFillMonitor | 工业上位机监控系统**（2025.03 — 2025.05）

基于 WPF + .NET 8 + MVVM 架构开发的灌装产线实时监控平台，通过 Modbus RTU 协议与西门子/三菱 PLC 通信，实现设备数据 200ms 级实时采集、温度/液位/产量可视化、异常分级告警及生产数据自动记录。

- 封装 PLC 通信层（异步锁 + 超时重试 + 断线重连），支持保持寄存器读取与线圈指令写入
- 采用 CommunityToolkit.Mvvm + DI 容器构建插件式导航架构，5 个功能模块低耦合切换
- FreeSql + SQLite 实现本地数据持久化，条码变化自动触发批次记录入库
- 五类告警码 + 四级严重度分级，实时弹窗 + 数据库双写 + 人工确认闭环
- LiveCharts 动态温度趋势图 + 自定义状态指示灯控件

**技术栈**: WPF / .NET 8 / CommunityToolkit.Mvvm / FreeSql / SQLite / Serilog / Modbus RTU / LiveCharts / HandyControl

---

## 简历项目经验（建议版本三：英文版—外企/大厂）

**SmartFillMonitor | Industrial Filling Line Monitoring System**（Mar 2025 — May 2025）

Developed a real-time SCADA-style monitoring platform for automated filling production lines using WPF (.NET 8) and MVVM architecture. Enabled 200ms-interval data acquisition from PLC via Modbus RTU, visualized 9 device parameters (temperature, liquid level, cycle time, valve state, etc.), and implemented multi-level alarm management with persistent production record tracking.

- Built serial communication service with async semaphore locking, timeout retry, and auto-reconnect for Modbus RTU master-slave polling
- Designed plugin-based navigation with DI container (Microsoft.Extensions.DependencyInjection), isolating Dashboard / Alarms / History / Logs / Settings modules
- Implemented local SQLite persistence via FreeSql ORM — barcode change detection triggers automatic batch record insertion
- Delivered 5 alarm types × 4 severity levels with real-time toast, DB logging, and human acknowledgment workflow
- Integrated LiveCharts for real-time temperature trends and HandyControl for consistent industrial UI styling

**Tech Stack**: WPF / .NET 8 / CommunityToolkit.Mvvm / FreeSql (SQLite) / Serilog / Modbus RTU / LiveCharts / HandyControl

---

## 面试话术准备（STAR 原则）

**Situation**: 某自动化灌装产线原有监控方式依赖 PLC 面板指示灯 + 人工巡检，无法追溯历史数据，异常发现滞后。

**Task**: 开发一套上位机监控系统，实时采集 PLC 数据并可视化，实现异常自动告警与生产记录自动归档。

**Action**:
- 用 NModbus4 封装了完整的串口通信层，通过 `SemaphoreSlim` 异步锁保证多线程下串口读写互斥，配合 `CancellationToken` 实现安全断开。
- 采用 MVVM + DI 架构，将 PLC 数据通过事件总线分发到各 ViewModel，UI 通过数据绑定自动刷新。
- 设计了条码变化检测逻辑——当扫码枪读取到新条码时自动创建 ProductionRecord 写入 SQLite，实现零人工干预的生产追溯。

**Result**: 实现 200ms 实时刷新、5 类故障自动告警、生产批次全量可追溯，系统稳定运行无串口冲突或数据丢失。

---

> **使用建议**: 根据目标岗位选择版本——国内传统制造/工控行业用版本一（详实），互联网/软件公司用版本二（精简），外企用版本三（英文）。面试时重点讲 PLC 通信层的异步锁设计和条码自动录单逻辑，这两块技术含金量最高。
