# HS2 HPM51 Zephyr 3.7 内核补丁

基线为远端 `leadjoy/hs2-hpm6e00` 的 `e3136c4c7498685fd9409ee9f93f386c15228a93`，保留上游USB产品补丁及无用MCUboot trailer补丁清理。长期维护分支为 `leadjoy/hs2-hpm5100`；没有升级到Zephyr4.4。

在此远端基线上移入以下两处r30已有补丁：

1. `subsys/usb/device_next/usbd_config.c`：HS2_SINGLECORE下先停用class再拆端点，避免切换协议时被取消的OUT工作重新占用端点。
2. `subsys/fs/nvs/nvs.c`：启用DATA_CRC时不越过调用方payload比较CRC尾字节，单独核验已存CRC，正确处理相同记录与损坏CRC。

不改原6E工作区/分支。不添加看门狗/故障自动恢复。版本由应用的west-hpm51.yml固定，不能替换成任意“最新Zephyr”。

2026-10-23：官方SDK1.13组合构建与16组离线回归通过，其中实际NVS引擎覆盖300个写入中断点和1800次保存/GC。r30/r31相同插桩的静置采样没有发现明显退化，正常r31已通过应用USB升级、完整设备回读、启动确认及原参数回读；最后不含临时计时接口。此次没有重新做物理断电测试或所有协议/动态动作的端到端验收。

发布目标仅为leadjoy/hs2-hpm5100；保留leadjoy/hs2-hpm6e00及main原状，不创建PR。配套构建、测量边界及AB维护限制见固件仓库HPM51_BUILD.md和HPM51_RELEASE_NOTES.md。
