# USB 3.2 10G Dual-Port Quick Tester (USB 3.2 10G 双头极速接口测试器)

## 1. Project Purpose (项目目的)

本项目旨在开发一款面向工厂产线、QC 质检、售后维修及实验室的高性价比 **USB 3.2 Gen 2 (10Gbps) 双头极速接口测试器**：
- **解决产线测试痛点**：传统简易通断测试板仅测物理通断，无法验证 10G/5G 真实差分信号与协议握手；示波器等专业设备又过于昂贵繁琐。本项目基于硬件级链路训练状态机（LTSSM），实现**“即插即识、1秒快速判定”**，无需电脑驱动与软件干预。
- **全接口双头覆盖**：集成 USB 3.0 Type-A 公头与 Type-C 满针公头，支持主流 PC 接口与 Type-C 正反插盲测。
- **硬件级状态与安全指示**：板载 4 颗 LED 直观指示链路状态（10G / U3 5G / U2 480M）及 OVP（过压/故障报警），异常电压纳秒级拦截。
- **极致降本与量产设计**：核心超高速 PHY **独选 GL3590**，搭配国产管家单片机（CH32V003），外壳采用**工业级 3D 打印**实现免模具费极速迭代，将整机硬件 BOM 成本压降至 **17~21 元**，实现高毛利、高可靠性规模量产。

---

## 2. Document Structure (文档结构)

项目所有资料均按规范工程体系命名，并按硬件研发全流程进行数字编号归档：

```text
USB_3.2_Quick_Tester/
│
├── 01_Product_Definition/          # 产品定义与需求
│   ├── 01_PRD.md                   # 产品需求文档（功能指标、指示灯真值表、电气规格）
│   ├── 02_Competitive_Analysis.md  # 竞品与市场分析（对比市售 93 元产品及传统治具）
│   ├── 03_Project_Roadmap.md       # 项目研发路线图（阶段里程碑 M0~M4、WBS与排期）
│   └── 04_Cost_Reduction_Plan.md   # 核心降本策略与关键指标（BOM 拆解与优化路径）
│
├── 02_System_Design/               # 方案设计与系统架构
│   ├── 01_System_Architecture.md   # 硬件系统拓扑与信号流向设计
│   ├── 02_GL3590_Chip_Design.md    # 核心握手芯片方案（GL3590 与 CH32V003 管家协同）
│   ├── 03_Dual_Port_Switch_Design.md# Type-A / Type-C 双头切换与 MUX 方案设计
│   ├── 04_Power_and_OVP_Design.md  # VBUS 供电、理想二极管防倒灌与 OVP 保护设计
│   └── 05_BOM_Cost_Analysis.xlsx   # 阶段性 BOM 成本核算明细表
│
├── 03_Hardware_Design/             # 硬件工程源文件
│   ├── 01_Project/                 # EDA 工程源文件（KiCad / Altium Designer）
│   │   ├── Tester_MainBoard.PrjPcb / .kicad_pro
│   │   ├── Schematic/              # 原理图源文件（双头输入、核心PHY、MCU及LED）
│   │   └── PCB/                    # 4 层高频阻抗控制 PCB Layout 源文件
│   │
│   ├── 02_Library/                 # 专用器件封装库
│   │   ├── Symbols/                # 原理图符号库（主控、Type-C、开关芯片等）
│   │   ├── Footprints/             # PCB 封装库（含超短焊盘公头、QFN 封装等）
│   │   └── 3D_Models/              # 3D 结构 STEP/STL 模型（适配 3D 打印外壳）
│   │
│   ├── 03_Outputs/                 # 研发交付物归档
│   │   ├── PDF/                    # 归档原理图 PDF（带版本号，如 V1.0_20261008.pdf）
│   │   ├── Gerber/                 # 制板光绘文件（含阻抗说明、工艺参数）
│   │   └── BOM/                    # 采购用器件清单（含立创/华强北订货料号）
│   │
│   └── 04_Reviews_and_Checklists/  # 硬件自检与工程评审
│       ├── Hardware_Checklist.md   # 打样前核心自检表（10G 差分阻抗、等长、AC 电容、DFM）
│       └── Issue_Log.md            # 硬件调试、版本迭代与缺陷整改记录
│
├── 04_Firmware_and_Configuration/  # 固件与配置程序
│   ├── 01_MCU_Firmware/            # CH32V003 管家代码（ADC 电压采样、LED 状态机、看门狗）
│   └── 02_PHY_Configuration/       # 主控芯片寄存器/EEPROM 配置脚本与量产烧录工具
│
├── 05_Manufacturing_Files/         # 生产与工装资料
│   ├── 01_Gerber/                  # 最终量产 PCB 光绘文件
│   ├── 02_Stencil/                 # SMT 激光钢网阶梯开孔文件
│   ├── 03_Pick_and_Place/          # 贴片机元件坐标文件（Centroid / CPL）
│   └── 04_Assembly_Drawing/        # 装配图与 3D 打印外壳工程图纸
│
├── 06_Testing_and_Validation/      # 验证与测试规范
│   ├── 01_Test_Specification.md    # 产线快速测试操作规范与判定基准
│   ├── 02_Compatibility_Matrix.md  # 主机兼容性测试矩阵（Intel/AMD/Mac/车机等）
│   └── 03_Validation_Report.xlsx   # 高低温、重复插拔耐久度与可靠性报告
│
└── 07_Mass_Production/             # 量产支持与维护
    ├── 01_User_Manual.md           # 产品使用说明书与指示灯状态对照表
    ├── 02_SOP_Production.md        # 工厂组装与贴片标准作业程序（SOP）
    └── 03_Revision_History.md      # 工程变更通知（ECN）与版本演进记录
```
