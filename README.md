# iPXE 构建项目

## 项目简介

本项目提供了一个 GitHub Actions 工作流，用于从官方 iPXE 源码构建 iPXE 固件并嵌入自定义的 `autoexec.ipxe` 自动执行脚本。

## 功能特性

- 支持从指定的 iPXE 仓库和分支/标签/提交构建
- 支持从指定的仓库获取自定义的 `autoexec.ipxe` 脚本
- 构建多种格式的 iPXE 固件：`undionly.kpxe`、`ipxe.pxe`、`ipxe.iso` 等
- 自动上传构建产物作为 GitHub Actions  artifacts

## 使用方法

### 通过 GitHub Actions 手动触发

1. 进入本仓库的 GitHub Actions 页面
2. 选择 "构建 iPXE 并嵌入自动执行脚本" 工作流
3. 点击 "Run workflow" 按钮
4. 填写以下参数：
   - `ipxe_repo`: 要使用的 iPXE 仓库（默认：`ipxe/ipxe`）
   - `ipxe_ref`: iPXE 仓库的分支、标签或提交（默认：`master`）
   - `autoexec_repo`: 包含 `autoexec.ipxe` 的仓库（默认：`ipxe/ipxe`）
   - `autoexec_ref`: autoexec 仓库的分支、标签或提交（默认：`master`）
   - `autoexec_path`: `autoexec.ipxe` 在仓库中的路径（默认：`autoexec.ipxe`）
5. 点击 "Run workflow" 开始构建

### 构建产物

构建完成后，工作流会上传以下构建产物：

- `undionly.kpxe`: 通用网络启动固件
- `ipxe.pxe`: PXE 格式固件
- `ipxe.iso`: ISO 格式固件
- 以及其他可能的固件格式
- 嵌入的 `autoexec.ipxe` 脚本

## 工作流配置

工作流配置文件位于 `.github/workflows/build-ipxe.yml`，您可以根据需要修改配置。

### 支持的架构

目前工作流默认只构建 x86_64 架构的固件，如果需要构建其他架构，可以修改工作流文件中的 `matrix` 配置。

## 示例 `autoexec.ipxe`

以下是一个简单的 `autoexec.ipxe` 示例：

```ipxe
#!ipxe

# 显示欢迎信息
echo 正在启动 iPXE...

# 网络配置
ifopen net0

# 启动菜单
menu iPXE 启动菜单
item linux 启动 Linux
item windows 启动 Windows
item shell 进入 iPXE shell
choose --default linux --timeout 10000 target && goto ${target}

:linux
echo 正在启动 Linux...
# 这里添加启动 Linux 的命令
goto end

:windows
echo 正在启动 Windows...
# 这里添加启动 Windows 的命令
goto end

:shell
shell
goto end

:end
echo 启动完成！
```

## 注意事项

- 确保 `autoexec_repo` 和 `autoexec_path` 参数正确，否则构建会失败
- 构建过程可能需要几分钟时间，请耐心等待
- 如果构建失败，请检查工作流日志以了解详细错误信息

## 相关链接

- [官方 iPXE 项目](https://github.com/ipxe/ipxe)
- [iPXE 文档](https://ipxe.org/docs)
