# 待解决的问题

1. Validation Layer  问题修复
2. 动态更改 fov
3. Rust 使用外部资源
4. Log 信息规定
5. 摄像机旋转优化
6. 低端显卡 vulkan 运行失效

VUID-vkCmdWaitEvents-srcStageMask-parameter(ERROR / SPEC): msgNum: 1019064795 - Validation Error: [ VUID-vkCmdWaitEvents-srcStageMask-parameter ] Object 0: handle = 0x28b1a3d5880, type = VK_OBJECT_TYPE_COMMAND_BUFFER; | MessageID = 0x3cbdb1db | Submitting cmdbuffer with call to VkCmdWaitEvents using srcStageMask 0x1 which must be the bitwise OR of the stageMask parameters used in calls to vkCmdSetEvent and VK_PIPELINE_STAGE_HOST_BIT if used with vkSetEvent but instead is 0x400. The Vulkan spec states: srcStageMask must be a valid combination of VkPipelineStageFlagBits values (https://vulkan.lunarg.com/doc/view/1.3.204.1/windows/1.3-extensions/vkspec.html#VUID-vkCmdWaitEvents-srcStageMask-parameter)
    Objects: 1
