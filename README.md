# NGA 图片域名修复

修复 NGA 论坛助手因 `imgN.nga.178.com` 系列域名失效导致图片无法加载的问题，统一重定向到 `imgN.nga.cn`。

## Loon 用户

导入 `NGA-Fix.lnplugin`（透明 `header` 改写，客户端无感）。

## Quantumult X 用户

使用模块文件 `nga-fix.conf`（QX 无透明改写类型，改用 `302`，图片照常加载）。

**一键导入**（iPhone 上点击下面的链接，QX 弹出添加模块确认即可）：

```
quantumult-x:///add-module?url=https://raw.githubusercontent.com/ka1er/NGA-Loon-Fix/main/nga-fix.conf&name=NGA图片域名修复
```

手动方式：把 `nga-fix.conf` 内容贴进 QX 配置，或「模块」里引用该文件。

### 注意事项

- 模块 `[mitM]` 已用 `%APPEND%` 追加解密域名，**不会覆盖你主配置里已有的 MITM 列表**，可单独开关/删除。
- MITM hostname 已**显式枚举** `img.nga.178.com` 及 `img0`–`img10`（含原 Loon 版漏列的 `img1`），不依赖通配符，导入即用。
- 重写规则用 `img(\d*)\.nga\.178\.com` 正则，自动覆盖任意数字后缀（`img.` / `img4.` / `img10.` 等），路径原样带到 `img$1.nga.cn`。
- 导入后重开一次 QX 代理（MITM 变更的通病，否则解密不生效）。
