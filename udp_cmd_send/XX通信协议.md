

# XX通信协议（初版）

[TOC]



## 协议说明

- 协议中字段大于1字节的数据，无特殊说明情况下均采用小端模式(低前高后)。

- 指令传输的通讯协议均为16进制数表示。

- RY24002项目协议含校验位，均为1字节累加和校验。

## 协议格式模板

| 序号 | 字节数 | 标识 | 含义      | 备注   |
| ---- | ------ | ---- | --------- | ------ |
| 1    | 2      | HEAD | 数据报头  | 0XFEEF |
| 2    | 2      | LEN  | 报文长度  | ushort |
| 3    | 2      | CMD  | 指令字    | ushort |
| 4    | 1      | SRC  | 源ID      | char   |
| 5    | 1      | DST  | 目的ID    | char   |
| 6    | N      | DATA | 数据/参数 |        |
| 7    | 1      | CRC  | 校验码    | 累加和 |
| SUM  | 9+N    |      |           |        |

## 网络信息

|      | 关系 | 网口速率 | 网络协议 | IP地址       | ID   | 端口号 |
| ---- | ---- | -------- | -------- | ------------ | ---- | ------ |
| PC   | 主机 | 千兆     | UDP      | 192.168.x.xx | 0x00 | 8080   |
| 板卡 | 从机 | 千兆     | UDP      | 192.168.x.10 | 0x01 | 5010   |

## 协议内容

### 1.射频类信号端口

| 指令名称     | 指令字                          | 方向     | 从机回复             |                                                              |      |
| ------------ | ------------------------------- | -------- | -------------------- | ------------------------------------------------------------ | ---- |
| XC01通道设置 | 00 01                           | PC下发   | YES                  |                                                              |      |
| XC02通道设置 | 00 02                           |          |                      |                                                              |      |
| XC03通道设置 | 00 03                           |          |                      |                                                              |      |
| XC04通道设置 | 00 04                           |          |                      |                                                              |      |
| XC05通道设置 | 00 05                           |          |                      |                                                              |      |
| XC06通道设置 | 00 06                           |          |                      |                                                              |      |
| XC07通道设置 | 00 07                           |          |                      |                                                              |      |
| XC08通道设置 | 00 08                           |          |                      |                                                              |      |
| XC09通道设置 | 00 09                           |          |                      |                                                              |      |
| XC10通道设置 | 00 0A                           |          |                      |                                                              |      |
| XC11通道设置 | 00 0B                           |          |                      |                                                              |      |
| XC12通道设置 | 00 0C                           |          |                      |                                                              |      |
|              |                                 |          |                      |                                                              |      |
|              |                                 |          |                      |                                                              |      |
| 字节序号     | 名称                            | 类型     | 说明                 |                                                              | 默认 |
| 0            | 信号样式                        | uchar    | 调制信号样式(十进制) |                                                              |      |
|              |                                 |          | 0：连续波 （LO）     | 00                                                           |      |
|              |                                 |          | 1：脉冲调幅（PAM）   | 01                                                           |      |
|              |                                 |          | 2：线性调频（LFM）   | 02                                                           |      |
|              |                                 |          | 3：噪声调频（NFM）   | 03                                                           |      |
|              |                                 |          | 4：FM                | 04                                                           |      |
|              |                                 |          | 5：AM                | 05                                                           |      |
|              |                                 |          | 6：FM_Vioce          | 06                                                           |      |
|              |                                 |          | 7：AM_Vioce          | 07                                                           |      |
|              |                                 |          |                      |                                                              |      |
| 1            | 通道开关                        | uchar    | 0：关 ；1：开        |                                                              |      |
|              | ***以下参数无用时置0***         |          |                      |                                                              |      |
| 2~9          | 信号样式参数（频率）            | uint64   | 单位：Hz             | 所选通道为XS01，05，09，02，06，10时，频率范围：DC-400MHz；所选通道为XS03，07，11，04，08，12时，频率范围：100M-4GHz； |      |
| 10           | 信号样式参数（衰减值）          | uchar    | 单位：db             |                                                              |      |
| 11~14        | 信号样式参数（脉宽）            | uint32   | 单位：us             |                                                              |      |
| 15~18        | 信号样式参数（重周）            | uint32   | 单位：us             |                                                              |      |
| 19~22        | 信号样式参数（LFM调频带宽）     | uint32   | 单位：KHz            |                                                              |      |
| 23~26        | 信号样式参数（LFM调频周期）     | uint32   | 单位：us             |                                                              |      |
| 27~28        | 信号样式参数（LFM调频点数）     | ushort16 | 单位：个             |                                                              |      |
| 29~32        | 信号样式参数（NFM噪声带宽）     | uint32   | 单位：KHz            |                                                              |      |
| 33~36        | 信号样式参数（NFM调频时间间隔） | uint32   | 单位：us             |                                                              |      |
| 37~40        | 信号样式参数（FM频偏）          | uint32   | 单位：KHz            |                                                              |      |
| 41           | 信号样式参数（AM幅度系数）      | uchar8   | 范围：0-100 （%）    |                                                              |      |
|              |                                 |          |                      |                                                              |      |



### 2.电平类信号端口

| 指令名称     | 指令字                               | 方向   | 从机回复        |                                              |      |
| ------------ | ------------------------------------ | ------ | --------------- | -------------------------------------------- | ---- |
| XC13通道设置 | 01 01                                | PC下发 | YES             | 固定输出和XC01，XC05，XC09的相关的脉冲和导前 |      |
| XC14通道设置 | 01 02                                |        |                 | XC02，XC06，XC10                             |      |
| XC15通道设置 | 01 03                                |        |                 | XC03，XC07，XC11                             |      |
| XC16通道设置 | 01 04                                |        |                 | XC04，XC08，XC12                             |      |
|              |                                      |        |                 |                                              |      |
| 字节序号     | 名称                                 | 类型   | 说明            |                                              | 默认 |
| 1            | 信号样式                             | uchar  | 信号样式        |                                              |      |
|              |                                      |        | 0：雷达脉冲信号 | 01                                           |      |
|              |                                      |        | 1：雷达导前信号 | 02                                           |      |
|              |                                      |        |                 |                                              |      |
| 2            | 开关                                 | uchar  | 0：关 ；1：开   |                                              |      |
|              | ***以下参数无用时置0***              |        |                 |                                              |      |
| 3~6          | 信号样式参数（雷达导前信号脉宽）     | uint32 | 单位：us        |                                              |      |
| 7~10         | 信号样式参数（雷达导前信号导后延时） | uint32 | 单位：us        |                                              |      |
|              |                                      |        |                 |                                              |      |



| 指令名称     | 指令字                      | 方向   | 从机回复          |      |      |
| ------------ | --------------------------- | ------ | ----------------- | ---- | ---- |
| XC17通道设置 | 01 05                       | PC下发 | YES               |      |      |
| XC18通道设置 | 01 06                       |        |                   |      |      |
| XC19通道设置 | 01 07                       |        |                   |      |      |
| XC20通道设置 | 01 08                       |        |                   |      |      |
|              |                             |        |                   |      |      |
| 字节序号     | 名称                        | 类型   | 说明              |      | 默认 |
| 1            | 信号样式                    | uchar  | 信号样式          |      |      |
|              |                             |        | 2：TTL高低电平    | 02   |      |
|              |                             |        | 3：时序脉冲       | 03   |      |
|              |                             |        |                   |      |      |
| 2            | 开关                        | uchar  | 0：关 ；1：开     |      |      |
|              | ***以下参数无用时置0***     |        |                   |      |      |
| 3            | 信号样式参数（TTL高低电平） | uchar8 | 00：全低 01：全高 |      |      |
| 4~7          | 信号样式参数（时序脉宽）    | uint32 | 单位：us          |      |      |
| 8~11         | 信号样式参数（时序周期）    | uint32 | 单位：us          |      |      |



### 3.电源类信号端口

| 指令名称     | 指令字        | 方向   | 从机回复        |             |      |
| ------------ | ------------- | ------ | --------------- | ----------- | ---- |
| XC21通道设置 | 02 01         | PC下发 | YES             |             |      |
| XC22通道设置 | 02 02         |        |                 |             |      |
| XC23通道设置 | 02 03         |        |                 |             |      |
| XC24通道设置 | 02 04         |        |                 |             |      |
|              |               |        |                 |             |      |
|              |               |        |                 |             |      |
|              |               |        |                 |             |      |
|              |               |        |                 |             |      |
| 字节序号     | 名称          | 类型   | 说明            |             | 默认 |
| 1            | 输出电压      | uchar  | 输出电压值      | 范围：0-36V |      |
| 2            | 输出电流（*） | uchar  | 输出电流值      | 范围：1-5A  | 1A   |
| 3            | 电源开关      | uchar  | 00：关 ；01：开 |             |      |
|              |               |        |                 |             |      |
|              |               |        |                 |             |      |



### 4.音频数据传输模式下FM和AM （保留功能）



1.先下发　FM或AM调制的相关参数。　				（下发参数）

2.发送首次传输所需的128包数据。（有回执）			（准备）

3.发送　音频数据传输＿控制　的开关指令。				（开关）

4.等待问询，每问询一次，回复　后续64包的音频数据。　	（连续传输）

5.后续关闭，发送　音频数据传输＿控制　的关指令即可。　	（关闭）



| 指令名称           | 指令字 | 方向   | 从机回复       |      |      |
| ------------------ | ------ | ------ | -------------- | ---- | ---- |
| 音频数据传输＿控制 | 0A 01  | PC下发 | YES            |      |      |
|                    |        |        |                |      |      |
|                    |        |        |                |      |      |
| 字节序号           | 名称   | 类型   | 说明           |      | 默认 |
| 1                  | 开关   | uchar  | 1：开　　0：关 |      |      |
|                    |        |        |                |      |      |
|                    |        |        |                |      |      |
|                    |        |        |                |      |      |
|                    |        |        |                |      |      |



| 指令名称               | 指令字         | 方向   | 从机回复                                            |      |      |
| ---------------------- | -------------- | ------ | --------------------------------------------------- | ---- | ---- |
| 音频数据传输＿数据格式 | 0A A0          | PC下发 | YES                                                 |      |      |
|                        |                |        |                                                     |      |      |
|                        |                |        |                                                     |      |      |
| 字节序号               | 名称           | 类型   | 说明                                                |      | 默认 |
| 1                      | 一次传输总包数 | uchar  | 首次开始传输时发包数：128　后续应答传输时发包数：64 |      |      |
| 2                      | 当前包数       | uchar  | 0-127 或 0-63                                       |      | 1A   |
| 3                      | 类型           | uchar  | 00：首次传输  01：上缓存区  02：下缓存区            |      |      |
| 4-1027                 | 音频数据       | int8   |                                                     |      |      |
|                        |                |        |                                                     |      |      |
|                        |                |        |                                                     |      |      |

### 5.面板指示灯控制

| 指令名称       | 指令字       | 方向   | 从机回复                              |      |      |
| -------------- | ------------ | ------ | ------------------------------------- | ---- | ---- |
| 面板指示灯控制 | 03 01        | PC下发 | YES                                   |      |      |
|                |              |        |                                       |      |      |
| 字节序号       | 名称         | 类型   | 说明                                  |      | 默认 |
| 1              | 指示灯控制字 | uint32 | 32bit，最低位代表xs01状态，高亮，低灭 |      |      |
|                |              |        |                                       |      |      |
|                |              |        |                                       |      |      |

## 问询-板卡 （保留功能）

当发送　音频数据传输＿控制　的开指令后，板卡会不断向上位机问讯数据。

| 指令名称                   | 指令字       | 方向   | 从机回复 |      |      |
| -------------------------- | ------------ | ------ | -------- | ---- | ---- |
| 问询1(回复上缓存区64K数据) | 0B 01        | PC下发 |          |      |      |
| 问询2(回复下缓存区64K数据) | 0B 02        |        |          |      |      |
|                            |              |        |          |      |      |
| 字节序号                   | 名称         | 类型   | 说明     |      | 默认 |
| 1~9                        | 预留（置零） | uchar  |          |      |      |
|                            |              |        |          |      |      |



## 回复-板卡

| 指令名称     | 指令字           | 方向     |                  |      |
| ------------ | ---------------- | -------- | ---------------- | ---- |
| 回复参数指令 | FF AA            | 板卡回复 |                  |      |
|              |                  |          |                  |      |
| 字节序号     | 名称             | 类型     | 说明             | 默认 |
| 1~2          | 参数下发是否成功 | uchar    | 1：成功 0：失败  |      |
| 3~4          | 参数是否有误     | uchar    | 1：无误 0：有误  |      |
| 5~11         | 预留（置零）     |          | 预留，无用时置零 |      |
|              |                  |          |                  |      |



（保留功能）

| 指令名称   | 指令字       | 方向     |      |      |
| ---------- | ------------ | -------- | ---- | ---- |
| 回复数据包 | FF BB        | 板卡回复 |      |      |
|            |              |          |      |      |
| 字节序号   | 名称         | 类型     | 说明 | 默认 |
| 1~2        | 总包数       | uchar    |      |      |
| 3~4        | 当前包数     | uchar    |      |      |
| 5~11       | 预留（置零） |          |      |      |
|            |              |          |      |      |
|            |              |          |      |      |

## 指令回复-PC （此项目暂无）

| 指令名称 | 指令字 | 方向   |      |      |      |
| -------- | ------ | ------ | ---- | ---- | ---- |
| 回复指令 |        | PC回复 |      |      |      |
|          |        |        |      |      |      |
| 字节序号 | 名称   | 类型   | 说明 | 默认 |      |
|          |        |        |      |      |      |
