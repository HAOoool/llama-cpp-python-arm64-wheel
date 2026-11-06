# llama-cpp-python arm64 wheel builder

用 GitHub Actions 的 arm64 原生 runner 编译 `llama-cpp-python==0.3.36` 的 **linux/aarch64 (manylinux) wheel**。

## 为什么存在

PyPI 上 `llama-cpp-python` 全部 209 个发布版本中，**没有任何一个提供 aarch64 的预编译 wheel**，只有源码包。要在 arm64 Linux 上用，只能自己编译。

本项目把编译过程放到 GitHub 的 `ubuntu-24.04-arm` runner 上跑，几分钟出结果，不用占本地机器资源。

## 关键编译参数

| 参数 | 原因 |
|---|---|
| `-DGGML_NATIVE=OFF` | 关闭 `-march=native`，否则编出来的二进制绑定编译机 CPU 特性 |
| `-DGGML_CPU_ARM_ARCH=armv8-a` | 锁定 armv8-a 基线。目标机是 Cortex-A53（骁龙625），不支持 armv8.2 指令，用错会 `Illegal instruction` |

## 用法

1. Actions 页面手动触发 `build-llama-cpp-python-aarch64`，或直接 push 到 `main`
2. 跑完在 Artifacts 里下载 `llama-cpp-python-wheel-aarch64-py313`
3. 传到目标机器安装：

```bash
pip install llama_cpp_python-0.3.36-*.whl
```

## 说明

- 这是**为了自用**构建的 wheel，非官方发布物
- 仅针对 **Python 3.13 + linux/aarch64** 编译
- 未经广泛测试，不保证在其他 arm64 设备上可用
- 构建产物在 Actions artifacts 里保留 7 天
