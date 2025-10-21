# LLama3.2 推理

- 以 meta-llama/Llama-3.2-1B 为例，huggingface 上下载模型：
```shell
export HF_ENDPOINT=https://hf-mirror.com
pip3 install huggingface-cli
huggingface-cli download --resume-download meta-llama/Llama-3.2-1B --local-dir meta-llama/Llama-3.2-1B --local-dir-use-symlinks False
```
- 导出模型：
```shell
python3 tools/export.py Llama-3.2-1B.bin --hf=meta-llama/Llama-3.2-1B
```
- 编译：
```shell
mkdir build 
cd build
# 开启 USE_CPM 选项，自动下载第三方依赖，前提是需要网络畅通
cmake -DUSE_CPM=ON -DLLAMA3_SUPPORT=ON .. 
make -j16
```
- 运行：
```shell
./build/demo/llama_infer Llama-3.2-1B.bin meta-llama/Llama-3.2-1B/tokenizer.json
# 和 huggingface 推理的结果进行对比
python3 hf_infer/llama3_infer.py
```

# Qwen2.5 推理

- 以 Qwen2.5-0.5B 为例，huggingface 上下载模型：
```shell
export HF_ENDPOINT=https://hf-mirror.com
pip3 install huggingface-cli
huggingface-cli download --resume-download Qwen/Qwen2.5-0.5B --local-dir Qwen/Qwen2.5-0.5B --local-dir-use-symlinks False
```
- 导出模型：
```shell
python3 tools/export_qwen2.py Qwen2.5-0.5B.bin --hf=Qwen/Qwen2.5-0.5B
```
- 编译：
```shell
mkdir build 
cd build
# 开启 USE_CPM 选项，自动下载第三方依赖，前提是需要网络畅通
cmake -DUSE_CPM=ON -DQWEN2_SUPPORT=ON .. 
make -j16
```
- 运行：
```shell
./build/demo/qwen_infer Qwen2.5-0.5B.bin Qwen/Qwen2.5-0.5B/tokenizer.json
# 和 huggingface 推理的结果进行对比
python3 hf_infer/qwen2_infer.py
```

## Qwen3推理
和上面同理，我们先从huggingface仓库中将模型下载到本地。
1. tools/export_qwen3/load.py中导出为pth，模型的输入`model_name`和输出地址`output_file`依次需要填写；
2. 导出pth格式的模型后，再用同文件夹下的write_bin.py导出qwen.bin；
3. 用CMake选项`QWEN3_SUPPORT`重新编译项目，其他步骤就都是一样的了。
