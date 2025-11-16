# Boatman-Protocol: 游戏联机数据交换协议

## 介绍
- 本协议是在 [EasyTier](https://easytier.cn/) 的基础上制作的游戏联机数据交换协议．
- 允许联机客户端相互交换游戏联机的一些相关数据
- 允许其他联机软件在实现该协议的基础上, 自由制作子协议, 但必须标注出是基于 **Boatman-Protocol** ．

## 名词定义
- 联机房间：一个 [EasyTier](https://easytier.cn/) 网络, 包含房主和房客．
- 房主：创建房间的客户端，和游戏服务器运行在同一物理机
- 房客：连接到其他房间的客户端

# 协议定义

## 房间码
房间码通过密码学安全的随机数生成器（CSPRNG），生成符合`B-NNNN-NNNN-SSSS-SSSS`形式的房间码，满足以下要求：

- X 和 S 为任意大写字母（A-Z）和数字（0-9）；
- 按照 0-9、A-Z顺序映射至 [0, 35]，每一位字符对应的数值为 d~1~ - d~16~ 满足**所有 d~i~ 乘以下标 i 的总和**为 **37** 的倍数，即
```
int s = 0;
for(int i = 1;i<=n;i++){
    s += d[i] * i;
}

if(s % 37 == 0){
    // 符合要求
}
else{
    // 不符合要求
}
```

## EasyTier联机房间
根据 **EasyTier** 的文档和 **Boatman房间码**，连接到EasyTier虚拟网络名称（**network-name**）应该为 `boatman-room-NNNN-NNNN`，连接到房间的网络密钥（network-secret）应为 `SSSS-SSSS`。
例如：房间码为 `B-A19E-B35T-F42K-C0D3` 时，网络名称为 `boatman-room-A19E-B35T` ，网络密钥为 `F42K-C0D3`。

## 