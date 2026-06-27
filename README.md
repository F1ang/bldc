# VESC firmware

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Travis CI Status](https://travis-ci.com/vedderb/bldc.svg?branch=master)](https://travis-ci.com/vedderb/bldc)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/75e90ffbd46841a3a7be2a9f7a94c242)](https://www.codacy.com/app/vedderb/bldc?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=vedderb/bldc&amp;utm_campaign=Badge_Grade)
[![Contributors](https://img.shields.io/github/contributors/vedderb/bldc.svg)](https://github.com/vedderb/bldc/graphs/contributors)
[![Watchers](https://img.shields.io/github/watchers/vedderb/bldc.svg)](https://github.com/vedderb/bldc/watchers)
[![Stars](https://img.shields.io/github/stars/vedderb/bldc.svg)](https://github.com/vedderb/bldc/stargazers)
[![Forks](https://img.shields.io/github/forks/vedderb/bldc.svg)](https://github.com/vedderb/bldc/network/members)

An open source motor controller firmware.

This is the source code for the VESC DC/BLDC/FOC controller. Read more at
[https://vesc-project.com/](https://vesc-project.com/)

## Supported boards

All of them!

Check the supported boards by typing `make`

```
[Firmware]
     fw   - Build firmware for default target
                            supported boards are: 100_250 100_250_no_limits 100_500...
```

There are also many other options that can be changed in [conf_general.h](conf_general.h).

## Prerequisites

### On Ubuntu (Linux)/macOS
- Tools: `git`, `wget`, and `make`
- Additional Linux requirements: `libgl-dev` and `libxcb-xinerama0`
- Helpful Ubuntu commands:
```bash
sudo apt install git build-essential libgl-dev libxcb-xinerama0 wget git-gui
```
- Helpful macOS tools: 

```bash
brew install stlink
brew install openocd
```

### On Windows
- Chocolately: https://chocolatey.org/install
- Git: https://git-scm.com/download/win. Make sure to click any boxes to add Git to your Environment (aka PATH)

## Install Dev environment and build

### On Ubuntu (Linux)/MacOS
Open up a terminal
1.  `git clone http://github.com/vedderb/bldc.git`
2.  `cd bldc`
3.  Continue with [On all platforms](#on-all-platforms)

### On Windows

1.  Open up a Windows Powershell terminal (Resist the urge to run Powershell as administrator, that will break things)
2.  Type `choco install make`
3.  `git clone http://github.com/vedderb/bldc`
4.  `cd bldc`
5.  Continue with [On all platforms](#on-all-platforms)

### On all platforms

1.  `git checkout origin/master`
2.  `make arm_sdk_install`
3.  `make` <-- Pick out the name of your target device from the supported boards list. For instance, I have a Trampa **VESC 100/250**, so my target is `100_250`
4.   `make 100_250` <-- This will build the **VESC 100/250** firmware and place it into the `bldc/builds/100_250/` directory

## Other tools

**Linux Optional - Add udev rules to use the stlink v2 programmer without being root**
```bash
wget vedder.se/Temp/49-stlinkv2.rules
sudo mv 49-stlinkv2.rules /etc/udev/rules.d/
sudo udevadm trigger
```

## IDE
### Prerequisites
#### On macOS/Linux

- `python3`, and `pip`

#### On Windows
- Python 3: https://www.python.org/downloads/. Make sure to click the box to add Python3 to your Environment.

### All platforms

1.  `pip install aqtinstall`
2.  `make qt_install`
3.  Open Qt Creator IDE installed in `tools/Qt/Tools/QtCreator/bin/qtcreator`
4.  With Qt Creator, open the vesc firmware Qt Creator project, named vesc.pro. You will find it in `Project/Qt Creator/vesc.pro`
5.  The IDE is configured by default to build 100_250 firmware, this can be changed in the bottom of the left panel, there you will find all hardware variants supported by VESC

## Upload to VESC
### Method 1 - Flash it using an STLink SWD debugger

1.  Build and flash the [bootloader](https://github.com/vedderb/bldc-bootloader) first
2.  Then `_flash` to the target of your choice. So for instance, for the VESC 100/250: 
```bash
make 100_250_flash
```

### Method 2 - Upload Firmware via VESC tool through USB

1.  Clone and build the firmware in **.bin** format as in the above Build instructions

In VESC tool

2.  Connect to the VESC
3.  Navigate to the Firmware tab on the left side menu 
4.  Click on Custom file tab
5.  Click on the folder icon to select the built firmware in .bin format (e.g. `build/100_250/100_250.bin`)

##### [ Reminder : It is normal to see VESC disconnects during the firmware upload process ]  
#####  **[ Warning : DO NOT DISCONNECT POWER/USB to VESC during the upload process, or you will risk bricking your VESC ]**  
#####  **[ Warning : ONLY DISCONNECT your VESC 10s after the upload loading bar completed and "FW Upload DONE" ]**

6.  Press the upload firmware button (downward arrow) on the bottom right to start upload the selected firmware.
7.  Wait for **10s** after the loading bar completed (Warning: unplug sooner will risk bricking your VESC)
8.  The VESC will disconnect itself after new firmware is uploaded.

## In case you bricked your VESC
you will need to upload a new working firmware to the VESC.  
However, to upload a firmware to a bricked VESC, you have to use a SWD Debugger.


## Contribute

Head to the [forums](https://vesc-project.com/forum) to get involved and improve this project.
Join the [Discord](https://discord.gg/JgvV5NwYts) for real-time support and chat

## Tags

Every firmware release has a tag. They are created as follows:

```bash
git tag -a [version] [commit] -m "VESC Firmware Version [version]"
git push --tags
```

## License

The software is released under the GNU General Public License version 3.0



<!-- --------------------------------- fyp --------------------------------- -->
## 附录：目录索引

| 目录          | 路径              | 说明                     |
| ------------- | ----------------- | ------------------------ |
| ChibiOS RTOS  | ChibiOS_3.0.5/os/ | 实时操作系统内核和 HAL   |
| comm          | comm/             | VESC 通信协议栈          |
| driver        | driver/           | 硬件外设驱动             |
| motor         | motor/            | FOC 电机控制算法         |
| encoder       | encoder/          | 编码器驱动               |
| imu           | imu/              | IMU 驱动和姿态解算       |
| hwconf        | hwconf/           | 硬件 BSP（20+ 厂商）     |
| applications  | applications/     | 应用层功能               |
| blackmagic    | blackmagic/       | Black Magic Probe 调试器 |
| libcanard     | libcanard/        | UAVCAN 协议              |
| lispBM        | lispBM/           | 嵌入式 Lisp 解释器       |
| qmlui         | qmlui/            | Qt QML 上位机界面        |
| tests         | tests/            | 测试代码                 |
| util          | util/             | 通用工具函数             |
| make          | make/             | 构建系统配置             |
| documentation | documentation/    | 项目文档                 |
| Project       | Project/          | IDE 项目文件             |

---

## 文件总览

| 文件                              | 说明                                           |
| --------------------------------- | ---------------------------------------------- |
| foc_math.c /foc_math.h            | FOC 数学核心：观测器、SVM、PLL、PID、弱磁、HFI |
| mcpwm.c / mcpwm.h                 | BLDC 六步换相驱动（梯形波控制）                |
| mcpwm_foc.c / mcpwm_foc.h         | FOC 矢量控制驱动（主流控制方式）               |
| mc_interface.c / mc_interface.h   | 统一电机控制接口层（路由到 BLDC 或 FOC）       |
| virtual_motor.c / virtual_motor.h | 虚拟电机仿真模型（用于无硬件测试）             |
| mcconf_default.h                  | 电机控制默认配置宏                             |

### 核心数据结构

| 结构体            | 说明                                                                         |
| ----------------- | ---------------------------------------------------------------------------- |
| motor_state_t     | 存储当前时刻的电压/电流/调制等电气状态（αβ 和 dq 坐标系）                    |
| observer_state    | 反电动势观测器状态：x1, x2 为磁链估计，lambda_est 为磁链幅值估计             |
| hfi_state_t       | 高频注入状态机：用于零/低速转子位置估计                                      |
| mc_audio_state    | 音频调制状态（把电机当作扬声器）                                             |
| motor_all_state_t | 电机全部状态的超集结构体，聚合配置、控制模式、电机状态、PID 状态、HFI 状态等 |

### 核心函数

| 函数                        | 说明                                                                              |
| --------------------------- | --------------------------------------------------------------------------------- |
| foc_observer_update()       | **磁链观测器更新** — 基于电压/电流/磁链模型估计转子位置和速度。支持多种观测器类型 |
| foc_pll_run()               | **PLL 锁相环** — 从观测器估计的相位中锁出平滑的速度值                             |
| foc_svm()                   | **空间矢量调制 SVPWM** — 输入 αβ 电压，输出三相 TIM 占空比及 SVM 扇区号           |
| foc_run_pid_control_pos()   | **位置 PID 控制** — 级联 P-PI 结构，位置外环 PI 输出作为速度内环的目标            |
| foc_run_pid_control_speed() | **速度 PID 控制** — PI 控制器，输出作为电流环的 iq 目标值                         |
| foc_correct_encoder()       | **编码器校正** — 融合观测器角度与编码器角度                                       |
| foc_correct_hall()          | **霍尔传感器校正** — 将霍尔信号插值为连续电角度                                   |
| foc_run_fw()                | **弱磁控制 FW** — 注入负方向 id 电流以扩展转速范围                                |
| foc_hfi_adjust_angle()      | **高频注入角度调整** — 基于 HFI 角度误差调整估计角度                              |
| foc_precalc_values()        | **预计算值** — 计算 Lq/Ld、凸极系数、调制归一化因子                               |

## mcpwm.c / mcpwm.h — BLDC 六步换相驱动

### 核心函数

| 函数                                       | 说明                                               |
| ------------------------------------------ | -------------------------------------------------- |
| mcpwm_init()                               | 初始化 PWM 定时器、ADC、换相步骤状态机             |
| mcpwm_set_duty() / mcpwm_set_duty_noramp() | 设置占空比（带/不带斜率限制）                      |
| mcpwm_set_pid_speed()                      | 设置 PID 速度目标                                  |
| mcpwm_set_pid_pos()                        | 设置 PID 位置目标                                  |
| mcpwm_set_current()                        | 设置电流目标                                       |
| mcpwm_brake_now()                          | 立即制动                                           |
| mcpwm_get_rpm()                            | 获取估算转速                                       |
| mcpwm_set_detect()                         | 进入电机检测模式                                   |
| mcpwm_switch_comm_mode()                   | 切换换相模式（同步/异步/积分）                     |
| mcpwm_adc_int_handler()                    | **ADC 中断处理** — 核心运行时：采样电流、换相、PID |

## mcpwm_foc.c / mcpwm_foc.h — FOC 矢量控制驱动

这是 VESC 最主要的电机控制层，实现完整的 **磁场定向控制** 算法。

### 核心函数

| 函数                                      | 说明                                                 |
| ----------------------------------------- | ---------------------------------------------------- |
| mcpwm_foc_init()                          | 初始化 FOC：分配内存、配置定时器与 ADC、启动控制线程 |
| mcpwm_foc_set_duty() / set_duty_noramp()  | 设置占空比目标                                       |
| mcpwm_foc_set_pid_speed() / set_pid_pos() | 设置速度/位置 PID 目标                               |
| mcpwm_foc_set_current()                   | 设置电流目标                                         |
| mcpwm_foc_set_openloop_*()                | 开环控制系列 — 用于无传感器启动阶段                  |
| mcpwm_foc_get_rpm()                       | 获取转速                                             |
| mcpwm_foc_get_id() / get_iq()             | 获取 dq 轴电流值                                     |
| mcpwm_foc_get_phase_*()                   | 获取各种来源的转子位置                               |
| mcpwm_foc_encoder_detect()                | 自动检测编码器偏移                                   |
| mcpwm_foc_measure_resistance()            | 自动测量电机相电阻                                   |
| mcpwm_foc_measure_inductance()            | 自动测量电机电感                                     |
| mcpwm_foc_hall_detect()                   | 自动检测霍尔传感器换相表                             |
| mcpwm_foc_dc_cal()                        | ADC 直流偏置校准                                     |
| mcpwm_foc_beep() / play_tone()            | 音频输出                                             |
| control_current()                         | **电流环核心** — dq 轴 PI 控制、解耦、反 Park → SVM  |
| hfi_update()                              | 高频注入线程                                         |
| mcpwm_foc_adc_int_handler()               | **ADC 完成中断** — FOC 控制主循环入口                |

---

## mc_interface.c / mc_interface.h — 电机控制统一接口层

向上层应用提供统一的电机控制 API，自动根据配置选择 BLDC 或 FOC 实现。

### 核心函数

| 函数                                                                      | 说明                                   |
| ------------------------------------------------------------------------- | -------------------------------------- |
| mc_interface_init()                                                       | 初始化配置、故障处理线程、采样发送线程 |
| mc_interface_set_duty() / set_current() / set_pid_speed() / set_pid_pos() | 统一设置函数                           |
| mc_interface_get_rpm()                                                    | 获取转速                               |
| mc_interface_get_tot_current()                                            | 获取总电流                             |
| mc_interface_get_tachometer_value()                                       | 获取转速计脉冲                         |
| mc_interface_get_fault()                                                  | 获取当前故障码                         |
| mc_interface_fault_stop()                                                 | 故障停止处理                           |
| mc_interface_set_configuration()                                          | 设置配置并重新初始化底层驱动           |
| mc_interface_mc_timer_isr()                                               | 主定时器中断处理                       |
| mc_interface_get_battery_level()                                          | 计算电池剩余电量                       |
| mc_interface_get_speed() / get_distance()                                 | 计算车速和里程                         |
| mc_interface_stat_*()                                                     | 统计信息                               |

## virtual_motor.c /virtual_motor.h — 虚拟电机仿真

**实时电机仿真模型**，用于在无硬件时测试 FOC 控制算法。

### 核心函数

| 函数                                  | 说明                                                 |
| ------------------------------------- | ---------------------------------------------------- |
| irtual_motor_init()                   | 初始化，注册终端命令                                 |
| irtual_motor_set_configuration()      | 传入电机配置，预计算参数                             |
| irtual_motor_int_handler()            | **主入口** — 接收 v_alpha/v_beta，运行模型，回写 ADC |
|                                       |
| un_virtual_motor_electrical()         | **电气模型** — dq 轴电流方程积分                     |
|                                       |
| un_virtual_motor_mechanics()          | **机械模型** — 转矩与运动方程                        |
|                                       |
| un_virtual_motor_park_clark_inverse() | Park-1 + Clark-1 变换 + 回写 ADC 值                  |

---


## 文件依赖关系

`
应用层 / CAN / 终端
     |
     v
mc_interface        -- 统一接口层
     |
     +---> mcpwm         -- BLDC 六步换相
     |
     +---> mcpwm_foc     -- FOC 矢量控制
                 |
                 v
            foc_math      -- FOC 数学核心（观测器/SVM/PLL/PID）
                 |
                 v
            virtual_motor -- 电机仿真模型（测试用）
`

---

## FOC 控制数据流

`
ADC 采样中断
    | 读取相电流 ia, ib, ic
Clark 变换: ia, ib, ic -> iα, iβ
    |
Park 变换: iα, iβ -> id, iq  (使用估计角度 θ)
    |
电流环 PI: (id_ref - id) -> vd, (iq_ref - iq) -> vq
    |
反 Park 变换: vd, vq -> vα, vβ
    |
SVM SVPWM: vα, vβ -> TIM1/8 占空比寄存器
    |
观测器: vα, vβ, iα, iβ -> 估计角度 θ 和速度 ω
    |
PLL: 滤波观测器角度 -> 平滑速度
    |
（下一周期用更新后的 θ 重复）
`

---
app_set_configuration()          // 用户应用层（PPM/ADC/UART/无刷手柄）
    └─ mc_interface_init()       // 电机接口层
         └─ mcpwm_foc_init()     // FOC 驱动核心层
              ├─ TIM1/TIM8 (PWM发生器)
              ├─ TIM2 (ADC采样触发)
              ├─ ADC1/ADC2/ADC3 + DMA2
              └─ 创建 ChibiOS 线程：timer_thread / hfi_thread / pid_thread

---
    mcpwm_foc_adc_int_handler
1. 判断当前是 V0 还是 V7（通过 TIM1_DIR 位）
2. 如果是 V7：
   ├─ 读取 ADC 值 → 转换为电流 (GET_CURRENT1/2)
   ├─ 乘法因子校准 (FAC_CURRENT1/2)
   ├─ 相电流 → i_alpha/i_beta (Clarke变换)
   ├─ i_alpha/i_beta 低通滤波 (UTILS_LP_FAST)
   ├─ 更新上一个 PWM 周期的占空比到 TIM1 CCR
   ├─ control_current() ★ 电流环控制
   │   ├─ 读取当前相位（来自观测器/编码器/Hall/HFI）
   │   ├─ 反 Park 变换：vd/vq → v_alpha/v_beta
   │   ├─ SVPWM：foc_svm() → tAout/tBout/tCout
   │   ├─ 将 SVPWM 结果写入 TIM1 占空比（用于下一周期）
   │   ├─ 注入 HFI 高频信号（若使能）
   │   └─ 叠加音频调制信号（若使能）
   ├─ update_valpha_vbeta() ★ 电压重构 & 死区补偿
   │   ├─ 读取 ADC 相电压
   │   ├─ 计算实际 mod_alpha/mod_beta
   │   ├─ 死区补偿（基于电流方向）
   │   └─ 低通滤波电压/调制
   └─ 更新状态变量：duty_now, id/iq, speed_est 等
3. 如果是 V0（非 V7 控制采样模式）：
   └─ 仅补充采样（用于 HFI 等），不执行完整控制

---
1. 在 V7 时刻叠加高频旋转电压矢量 (mod_alpha, mod_beta)
2. 采集对应的高频电流响应
3. FFT 解调 (8/16点FFT) → 提取 bin1/bin2 分量
4. 角度估计：bin2/bin1 的反正切
5. hfi_adjust_angle() 校正：
   └─ 通过 PI 调节器跟踪转子凸极

---
| 线程                          | 栈大小 | 优先级       | 周期      | 功能                                             |
| ----------------------------- | ------ | ------------ | --------- | ------------------------------------------------ |
| `timer_thread` (mcpwm_foc)    | 512    | NORMALPRIO   | 1ms       | 调用 `timer_update()` → 位置/速度 PID + DTC 更新 |
| `timer_thread` (mc_interface) | 512    | NORMALPRIO   | 1ms       | 运行两个电机的 `run_timer_tasks()`               |
| `hfi_thread` (mcpwm_foc)      | 512    | -            | sleep等待 | 后台处理 HFI 角度估计的耗时计算                  |
| `pid_thread` (mcpwm_foc)      | 256    | -            | sleep等待 | 后台 PID 参数计算（未来扩展）                    |
| `sample_send_thread`          | 512    | NORMALPRIO-1 | event触发 | 调试数据采样发送到上位机                         |
| `fault_stop_thread`           | 512    | HIGHPRIO-3   | event触发 | 故障时紧急停止                                   |
| `stat_thread`                 | 512    | NORMALPRIO   | 1000ms    | 统计信息更新（功耗、温度等）                     |

---
上位机 → USB/UART → packet_process() → commands_process_packet()
    ↑                                         ↓
    |                                    switch (packet_id):
    │                                       ├─ COMM_FW_VERSION → 返回固件版本
    │                                       ├─ COMM_GET_VALUES → mc_interface 采集数据 → 回复
    │                                       ├─ COMM_SET_DUTY  → mcpwm_foc_set_duty()
    │                                       ├─ COMM_SET_CURRENT → mcpwm_foc_set_current()
    │                                       ├─ COMM_SAMPLE_PRINT → 启动波形采样
    │                                       ├─ COMM_PLOT_INIT → 绘图初始化
    │                                       └─ ... 等超过 100 种命令
    │
    └── ← reply_func() → packet → USB/UART

---
PWM周期 (e.g. 25kHz = 40μs)
│
├── [V7 时刻] TIM2 CC2 中断
│   └── ADC 三路注入采样 (DMA) → DMA 完成中断
│       └── mcpwm_foc_adc_int_handler()  ★ ★ ★
│           ├── 读取 ADC → 电流值 (偏移补偿 + 标定)
│           ├── Clarke 变换: ia,ib → i_alpha,i_beta
│           ├── Park 变换: i_alpha,i_beta → id,iq (使用 phase_sin/cos)
│           ├── 电流 PI 控制: id_err, iq_err → vd, vq
│           ├── 解耦补偿 (交叉耦合 + 反电势)
│           ├── 电压饱和 + anti-windup
│           ├── 反 Park 变换: vd,vq → v_alpha,v_beta
│           ├── SVPWM: v_alpha,v_beta → tA,tB,tC (PWM占空比)
│           ├── 写入 TIM1 CCR 用于下一周期
│           ├── 更新状态: id_filter, iq_filter, duty_now
│           ├── 更新转速估计 (PLL 或差分法)
│           ├── [可选] HFI 注入
│           └── [可选] 音频调制叠加
│
├── [V0 时刻] TIM2 CC2 中断（若使能双采样）
│   └── 补充电流采样（仅用于 HFI）
│
└── [1ms 周期] timer_thread (ChibiOS)
    └── timer_update():
        ├── 位置 PID 控制 (若使能)
        │   └── foc_run_pid_control_pos()
        │       └── 位置误差 → 速度目标值
        └── 速度 PID 控制 (若使能)
            └── foc_run_pid_control_speed()
                └── 速度误差 → 电流目标值 iq_set
                    └── 电流环在 ADC ISR 中跟踪该目标

---
                  ┌──────────────┐
  ADC 原始值 ───→│  偏移校正     │
                  └──────┬───────┘
                         ↓
                  ┌──────────────┐
                  │  Clarke 变换  │  ia,ib → iα,iβ
                  └──────┬───────┘
                         ↓
                  ┌──────────────┐
  phase sin/cos ─→│  Park 变换    │  iα,iβ → id,iq
                  └──────┬───────┘
                         ↓
               ┌─────────────────┐
  id_set=0 ───→│  d轴 PI 控制器  │ → vd
  iq_set  ───→│  q轴 PI 控制器  │ → vq
               └──────┬──────────┘
                      ↓
               ┌──────────────┐
               │  解耦补偿     │ → vd_c, vq_c
               └──────┬───────┘
                      ↓
               ┌──────────────┐
               │  饱和+抗饱和  │
               └──────┬───────┘
                      ↓
               ┌──────────────┐
  phase sin/cos→│ 反Park变换   │ vd,vq → vα,vβ
               └──────┬───────┘
                      ↓
               ┌──────────────┐
               │  SVPWM        │ vα,vβ → tA,tB,tC
               └──────┬───────┘
                      ↓
               ┌──────────────┐
               │  TIM1 CCR     │ → PWM 输出 → 逆变器 → 电机
               └──────────────┘

   角度获取分支：
   ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
   │ 编码器   │  │ Hall     │  │ 观测器   │  │ HFI      │
   │ (SPI/ABZ)│  │ (GPIO)   │  │ (BEMF)   │  │ (高频)   │
   └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘
        └──────────────┴─────────────┴──────────────┘
                                  ↓
                          m_motor_state.phase
                          (用于 Park/反Park)

---