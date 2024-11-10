## 查看mac所有网络接口及其名称
`sudo networksetup -listallnetworkservices`

## 在 Wi-Fi 和 以太网接口上禁用 IPv6
```
sudo networksetup -setv6off Wi-Fi
sudo networksetup -setv6off Ethernet
```


## 启用ipv6
```
sudo networksetup -setv6automatic Ethernet
sudo networksetup -setv6automatic Wi-Fi
```