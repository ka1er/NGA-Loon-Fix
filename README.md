# NGA 图片域名修复

修复 NGA 论坛助手因 `imgN.nga.178.com` 系列域名失效导致图片无法加载的问题，统一重定向到 `imgN.nga.cn`。

## Loon 用户

导入 `NGA-Fix.lnplugin`（透明 `header` 改写，客户端无感）。MITM hostname 用通配 `img*.nga.178.com`，精确覆盖图床、不解密主站，实测可加载。

## Quantumult X 用户

QX 无 Loon 那种透明 `header` 改写类型，改用 `302`（图片照常加载，仅客户端多一次跳转）。

### 方案：重写引用 + 手动 MITM（推荐，零覆盖风险）

本仓库 `nga-fix.conf` 只含重写规则，用「重写→引用」导入，**不碰你的 MITM 列表**，避免模块导入覆盖主配置的风险。

**步骤：**

1. 一键引用重写（iPhone 上点击链接，QX 弹确认即可）：

   ```
   quantumult-x:///add-resource?remote-resource={"rewrite_remote":[{"url":"https://raw.githubusercontent.com/ka1er/NGA-Loon-Fix/main/nga-fix.conf","tag":"NGA图片域名修复"}]}
   ```

   或手动：QX「设置 → 重写 → 引用」右上角添加，填入 raw 链接并开启。

2. **手动添加 MITM 解密域名**（必做，否则重写不生效）：
   QX「设置 → MitM → 主机名」添加一行：

   ```
   img*.nga.178.com
   ```

   确认 MitM 开关已开、证书已安装并信任。

3. **重开一次 QX 代理**（MITM 变更必须重启才加载，否则解密不生效）。

### 说明

- 重写正则 `img(\d*)\.nga\.178\.com` 覆盖任意数字后缀（`img.` / `img4.` / `img10.` 等），路径原样带到 `img$1.nga.cn`。
- MITM 用 QX 官方通配写法 `img*.nga.178.com`（官方 sample.conf 注释：`wildcard * and ? are supported`），`img*` 前缀精确覆盖图床（`img.`/`img4.`/`img10.` 等），不解密主站 `bbs.`/`www.` 等其它子域。
- 已验证 Loon 版图片加载正常，说明站方路径结构一致，目标 `imgN.nga.cn` 正确。
