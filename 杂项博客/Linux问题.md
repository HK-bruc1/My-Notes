# Linux问题

## 网络图标不见了

ubantu图形化界面的网络图标不见了：

**NetworkManager没有接管网络**

- 终端输入：nmcli networking
  - 结果显示:disabled，

- 则表示 NetworkManager 未接管网络，网络图标消失也是由此导致的。
  - 解决办法：nmcli networking on