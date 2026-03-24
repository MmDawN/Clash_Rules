# 自定义整合方案说明文件

本方案旨在为 `Custom_OpenClash_Rules` 的 fork 仓库设置一套可维持上游同步的自定义逻辑。

## 方案架构

### 1. 规则层 (Ruleset Layer)
- **文件**: `cfg/Custom_Clash.ini`
- **机制**: 在 `Custom_Clash.ini` 中通过 `ruleset` 引用外部 YAML 列表。
- **作用**: 承载具体的规则分流策略。

### 2. DNS/基础配置层 (DNS/Base Configuration)
- **文件**: `custom/mihomo_base.yaml`
- **机制**: 使用 `clash_rule_base` 在 Subconverter 转换时直接合并一个 YAML。
- **作用**: 控制 DNS 配置 (`fake-ip`、`nameserver`、`fake-ip-filter`)。

### 3. 给定 subconverter 的订阅链接 (Subscription URL)
你可以直接在 OpenClash/Clash 订阅转换中输入以下链接：
```
/sub?target=clash&url=<你的订阅链接>&config=https://cdn.jsdelivr.net/gh/MmDawN/Clash_Rules@zq_custom/cfg/Custom_Clash.ini
```

### 4. 自动同步层 (Automatic Sync)
- **文件**: `.github/workflows/sync-upstream.yml`
- **作用**: 每 6 小时自动从 `Aethersailor/Custom_OpenClash_Rules` 拉取更新并合并。

## 日常维护指南

所有自定义的核心资源都存放在当前 `custom/` 目录下：

### 1. 修改直连域名
前往 [zq_custom_direct.yaml](zq_custom_direct.yaml) 即可。支持 `clash-domain` 格式。

### 2. 修改 DNS 配置
前往 [mihomo_base.yaml](mihomo_base.yaml) 修改相关的 `fake-ip-filter` 或 DNS 服务器。

## 后续同步建议
1. 该方案仅在 `Custom_Clash.ini` 中插入了两行自定义配置，最大程度减少了 merge 冲突的可能性。
2. 所有新增的逻辑都封装在 `custom/` 文件夹内，不会干扰上游仓库的原本目录结构。

## GitHub Settings 配置要求
为了让自动同步 Action 正常工作，请在仓库设置中：
1. 前往 **Settings** > **Actions** > **General**。
2. 确保 `Workflow permissions` 勾选了 `Read and write permissions`。
