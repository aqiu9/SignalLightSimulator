# 信号机点灯模拟系统

基于 VB.NET WinForms 编写的铁路信号机点灯、进站模拟程序。用于演示信号机在不同进路（直向、侧向、双黄预警等）下的显示逻辑，以及故障状态下的显示切换。

## 项目结构

```
.
├── 信号机点灯模拟系统.sln       # 解决方案文件
├── 信号机点灯模拟系统.vbproj    # 项目文件
├── App.config
├── Module1.vb                   # 程序入口（Sub Main），负责创建并显示各窗体
├── Form1.vb / .Designer.vb / .resx   # 主控制窗体：操作按钮、进路选择
├── Form2.vb / .Designer.vb / .resx   # 辅助窗体
├── Form3.vb / .Designer.vb / .resx   # 信号机示意图绘制窗体（LineShape 灯位定义）
└── My Project/                  # VB.NET 项目自动生成的设置、资源文件
```

## 环境要求

- Windows + Visual Studio 2019 或更高版本
- .NET Framework 4.7.2

## 运行方式

1. 使用 Visual Studio 打开 `信号机点灯模拟系统.sln`。
2. 还原/确认依赖引用（`Microsoft.VisualBasic.PowerPacks` 等，均为 .NET Framework 自带或随 Visual Studio 安装）后，直接按 `F5` 启动调试，或选择 `生成 -> 生成解决方案` 后运行生成的 exe。
3. 启动后会依次弹出 Form1（主控制窗体）、Form2、Form3（信号机示意图），在 Form1 上进行进路操作即可观察灯位变化。

## 需求说明

以下术语用于描述信号机之间的位置关系与操作：

| 术语 | 含义 |
| --- | --- |
| line | 指信号机右侧一条线 |
| me | 指我们研究的信号机 |
| a | 指左侧共线距离较近一个开关（信号机） |
| b | 指左侧共线最远端、线路终点一个开关（信号机） |
| c | 指左侧不共线一个开关（信号机） |
| 取消 | 界面下方“取消”按钮 |
| 人解 | 界面下方“人解”按钮 |

### 无操作（其他操作结束后）—— 单红

灯位：`LineShape_red1, LineShape_red2, LineShape_red3, LineShape_red4_1, LineShape_red4_2, LineShape_red4_3, LineShape_red4_4, LineShape_red6_1, LineShape_red6_2, LineShape_red6_3, LineShape_red6_4, LineShape_red6_5, LineShape_red6_6, LineShape_red8, LineShape_red9`

点亮顺序：`XJZ220—RD1—DJ5-6—LXJ41-43—HBⅠ1-Ⅰ3—LXJ63-61—RD3—XJF220`

### me → a —— 单黄

灯位：`LineShape_red1, LineShape_red2, LineShape_red3, LineShape_yellow4, LineShape_yellow5_1, LineShape_yellow5_2, LineShape_yellow6, LineShape_yellow7, LineShape_yellow8_1, LineShape_yellow8_2, LineShape_yellow8_3, LineShape_yellow9_1, LineShape_yellow9_2, LineShape_yellow9_3, LineShape_yellow9_4, LineShape_yellow9_5, LineShape_red8, LineShape_red9`

点亮顺序：`XJZ220—RD1—DJ5-6—LXJ41-42—ZXJ81-82—TXJ21-23—LUXJ21-23—UBⅠ1-Ⅰ3—LXJ62-61—RD3—XJF220`

### me → b —— 单绿

灯位：`LineShape_red1, LineShape_red2, LineShape_red3, LineShape_yellow4, LineShape_yellow5_1, LineShape_green6_1, LineShape_green6_2, LineShape_green6_3, LineShape_yellow7_1, LineShape_yellow7_2, LineShape_yellow7_3, LineShape_yellow7_4, LineShape_yellow9_5, LineShape_red8, LineShape_red9`

点亮顺序：`XJZ220—RD1—DJ5-6—LXJ41-42—ZXJ81-82—TXJ21-22—LBⅠ1-Ⅰ3—LXJ62-61—RD3—XJF220`

### me → c —— 双黄

先亮一个单黄：

`LineShape_red1, LineShape_red2, LineShape_red3, LineShape_yellow4, LineShape_yellow5_1, LineShape_yellow5_2, LineShape_yellow6, LineShape_yellow7, LineShape_yellow8_1, LineShape_yellow8_2, LineShape_yellow8_3, LineShape_yellow9_1, LineShape_yellow9_2, LineShape_yellow9_3, LineShape_yellow9_4, LineShape_yellow9_5, LineShape_red8, LineShape_red9`

再接着：

`LineShape_dy1, LineShape_dy2, LineShape_dy3, LineShape_dy4, LineShape_dy5, LineShape_dy6_1, LineShape_dy6_2, LineShape_dy6_3, LineShape_dy7_1, LineShape_dy7_2, LineShape_dy7_3`

点亮顺序：
```
XJZ220—RD2—2DJ5-6—LXJ71-72—ZXJ71-73—TXJ11-13—2UBⅠ1-Ⅰ3—LXJ62-61—RD3—XJF220
XJZ220—RD2—DJ5-6—LXJ41-42—ZXJ81-83—2DJ21-22—LUXJ21-23—UBⅠ1-Ⅰ3—LXJ62-61—RD3—XJF220
```

上述操作的恢复操作：`取消 → me`，恢复为单红。

### 对 line 操作设置故障 → me —— 红白（PPT 中为红蓝）

Role.Null 的单红：

`LineShape_red1, LineShape_red2, LineShape_red3, LineShape_red4_1, LineShape_red4_2, LineShape_red4_3, LineShape_red4_4, LineShape_red6_1, LineShape_red6_2, LineShape_red6_3, LineShape_red6_4, LineShape_red6_5, LineShape_red6_6, LineShape_red8, LineShape_red9`

叠加蓝：

`LineShape_blue4_1, LineShape_blue4_2, LineShape_blue5_1, LineShape_blue5_2, LineShape_blue5_3, LineShape_blue6_0, LineShape_blue6_1, LineShape_blue6_2, LineShape_blue6_3, LineShape_blue6_4, LineShape_blue7_1, LineShape_blue7_2, LineShape_red8, LineShape_red9`

点亮顺序：
```
XJZ220—RD1—DJ5-6—LXJ41-43—HBⅠ1-Ⅰ3—LXJ63-61—RD3—XJF220
XJZ220—RD2—2DJ5-6—LXJ71-72—YXJ71-72—YBBⅠ1-Ⅰ3—YXJ62-61—LXJ63-61—RD3—XJF220
```

恢复操作：`人解 → me`，恢复为单红。

## 截图

![screenshot-1](https://user-images.githubusercontent.com/51012896/168103473-14a8c989-ad50-4ee3-a7b1-1cefdc53eeb7.jpg)

![screenshot-2](https://user-images.githubusercontent.com/51012896/168103828-ffd4d3b9-a96d-43bb-bdf1-290d4daea285.png)
![screenshot-3](https://user-images.githubusercontent.com/51012896/168103834-a544cba2-d54f-4d10-a474-2e26fd279a74.png)
![screenshot-4](https://user-images.githubusercontent.com/51012896/168103837-fa8a88c1-fb4d-42de-b0bc-968086b73afd.png)
![screenshot-5](https://user-images.githubusercontent.com/51012896/168103844-ce446a76-37bc-436e-9f70-2e1803909a56.png)
![screenshot-6](https://user-images.githubusercontent.com/51012896/168103848-fef0239d-6b41-453e-bca3-b0d4b5a11bfc.png)

![screenshot-7](https://user-images.githubusercontent.com/51012896/168103960-0dd4ab5a-3527-49b2-b8ce-23889ba28412.png)
![screenshot-8](https://user-images.githubusercontent.com/51012896/168103972-54c56fbe-13e8-4732-95f9-88e2c5d24662.png)

## 演示（画质已加密）

https://user-images.githubusercontent.com/51012896/168104739-f933ad4a-e2a4-43d7-927e-db762d63c9ab.mp4

![screenshot-9](https://user-images.githubusercontent.com/51012896/168105381-6b6b5749-d726-49fa-bcdf-7b1b3b5e98bd.png)
![screenshot-10](https://user-images.githubusercontent.com/51012896/168105392-a154c920-6f50-441c-8262-e7675b4cc106.png)
![screenshot-11](https://user-images.githubusercontent.com/51012896/168105397-3f211cd1-0b4e-42df-91ca-582cda509ab2.png)
![screenshot-12](https://user-images.githubusercontent.com/51012896/168105405-48817d9a-fafc-429b-8acf-943606a032df.png)
