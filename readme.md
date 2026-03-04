# High-performance Deep Learning Inference Framework

基于 C++/CUDA 的高性能大模型推理框架，支持 Llama2/3、Qwen2.5、Qwen3 等模型，提供 CPU 与 CUDA 双后端及 Int8 量化能力。

## 特性

- **多模型支持**：Llama2、Llama3.x、Qwen2.5、Qwen3
- **双后端**：CPU 算子与 CUDA 加速，便于调试与性能对比
- **量化**：支持 Int8 量化推理
- **工程实践**：C++17/20、CMake 管理、单元测试与 Benchmark
- **企业级依赖**：glog、gtest、sentencepiece、Armadillo 等

## 快速开始

### 依赖

- CMake ≥ 3.16
- CUDA Toolkit
- [glog](https://github.com/google/glog)、[gtest](https://github.com/google/googletest)、[sentencepiece](https://github.com/google/sentencepiece)、[Armadillo](https://arma.sourceforge.net/) + OpenBLAS

### 编译

```bash
mkdir build && cd build
cmake ..
# 或使用 CPM 自动拉取依赖
cmake -DUSE_CPM=ON ..
make -j16
```

### 运行示例

```bash
# Llama2
./build/demo/llama_infer <model.bin> <tokenizer.model>

# Llama3.2（需 -DLLAMA3_SUPPORT=ON）
./build/demo/llama_infer Llama-3.2-1B.bin path/to/tokenizer.json

# Qwen2.5（需 -DQWEN2_SUPPORT=ON）
./build/demo/qwen_infer Qwen2.5-0.5B.bin path/to/tokenizer.json
```

## 模型导出

```bash
# Llama2
python tools/export.py llama2_7b.bin --meta-llama path/to/llama/7B

# Llama3.2（Hugging Face）
python tools/export.py Llama-3.2-1B.bin --hf=meta-llama/Llama-3.2-1B

# Qwen2.5
python tools/export_qwen2.py Qwen2.5-0.5B.bin --hf=Qwen/Qwen2.5-0.5B

# Qwen3：先按 tools/export_qwen3/ 内说明导出 pth，再导出 bin，编译时加 -DQWEN3_SUPPORT=ON
```

更多用法见各脚本命令行参数。

## 模型与分词器

- **Llama2**：[百度网盘](https://pan.baidu.com/s/1PF5KqvIvNFR8yDIY1HmTYA?pwd=ma8r) 或 [Hugging Face](https://huggingface.co/fushenshen/lession_model/tree/main)
- **TinyLlama**：模型 [karpathy/tinyllamas](https://huggingface.co/karpathy/tinyllamas)，分词器 [yahma/llama-7b-hf](https://huggingface.co/yahma/llama-7b-hf/blob/main/tokenizer.model)
- **Llama3.2 / Qwen2.5**：从 Hugging Face 下载后按上方导出命令转换

国内可设置镜像：`export HF_ENDPOINT=https://hf-mirror.com` 后使用 `huggingface-cli download`。

## 项目结构概览

```
├── demo/          # 推理示例
├── kuiper/        # 核心推理与算子
├── test/          # 单元测试
├── tools/         # 模型导出等脚本
├── hf_infer/      # Hugging Face 对比推理
└── cmake/         # 构建配置
```

## 路线图（计划中）

在现有高性能推理能力之上，计划逐步扩展以下能力：

- **多轮对话**：支持带历史的对话上下文与会话管理
- **RAG 系统**：检索增强生成，接入向量检索与文档问答
- **多 Agent**：多智能体协作与任务编排

欢迎提 Issue 或 PR 参与讨论与实现。

## 许可证

见仓库内 LICENSE 文件。
