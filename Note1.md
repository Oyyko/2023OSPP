## 总线
Platform总线、VirtIO总线、特定总线。

Platform总线上挂载着各种驱动和设备。其中我们需要关注的是virtio_mmio驱动和virtio_mmio设备。它们挂载在platform总线上执行mmio功能的虚拟化。

virtio_mmio驱动负责把platform总线和挂载在VirtIO总线上的virtio设备相连。之后再由挂载再VirtIO总线上的各类virtio驱动来连接到挂载在特定总线上的特定设备。

## virt-blk设备的注册过程
1. 解析设备树btb
2. 填充Platform总线
3. 触发virtio-mmio驱动与设备关联
4. 把兼容virt-blk的virtio设备注册到virtio总线
5. 触发virtio-blk驱动与设备关联
6. 把这个virt-blk设备按照设备类型，注册到对应的特定总线上。

## virtio驱动和virtio设备的交互
1. 基于vring环形队列。在Guest和Host都可见可写
2. 中断响应的通道（适用于读取大块数据）

## Arceos主流程
参考: [Here](https://www.bilibili.com/video/BV1Tv4y157uE/?spm_id_from=333.788&vd_source=bb5d641d14c8afb44fc189151d27f392)

1. Axhal负责系统boot启动，完成最初的环境准备。
2. Axruntime完成主要运行环境的初始化准备。
3. 进入main

## Arceos增加新类型的virtio驱动设备的方式
需要：
1. 在Axruntime:rust_main中新增virtio驱动init
2. 在axdriver:init_drivers中新增驱动probe
