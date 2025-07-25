# Nano-vLLM

A lightweight vLLM implementation built from scratch.

## Key Features

* 🚀 **Fast offline inference** - Comparable inference speeds to vLLM
* 📖 **Readable codebase** - Clean implementation in ~ 1,200 lines of Python code
* ⚡ **Optimization Suite** - Prefix caching, Tensor Parallelism, Torch compilation, CUDA graph, etc.

## Installation

```bash
pip install git+https://github.com/GeeeekExplorer/nano-vllm.git
```

## Manual Download

If you prefer to download the model weights manually, use the following command:
```bash
huggingface-cli download --resume-download Qwen/Qwen3-0.6B \
  --local-dir ~/huggingface/Qwen3-0.6B/ \
  --local-dir-use-symlinks False
```

## Quick Start

See `example.py` for usage. The API mirrors vLLM's interface with minor differences in the `LLM.generate` method:
```python
from nanovllm import LLM, SamplingParams
llm = LLM("/YOUR/MODEL/PATH", enforce_eager=True, tensor_parallel_size=1)
sampling_params = SamplingParams(temperature=0.6, max_tokens=256)
prompts = ["Hello, Nano-vLLM."]
outputs = llm.generate(prompts, sampling_params)
outputs[0]["text"]
```

## Benchmark

See `bench.py` for benchmark.

**Test Configuration:**
- Hardware: RTX 4070 Laptop (8GB)
- Model: Qwen3-0.6B
- Total Requests: 256 sequences
- Input Length: Randomly sampled between 100–1024 tokens
- Output Length: Randomly sampled between 100–1024 tokens

**Performance Results:**
| Inference Engine | Output Tokens | Time (s) | Throughput (tokens/s) |
|----------------|-------------|----------|-----------------------|
| vLLM           | 133,966     | 98.37    | 1361.84               |
| Nano-vLLM      | 133,966     | 93.41    | 1434.13               |

## Run Nano-vllm on AMD GPU
### CDNA3 datacenter GPUs
Access AMD CDNA3 datacenter GPU through [AMD developer cloud] (https://www.amd.com/en/developer/resources/cloud-access/amd-developer-cloud.html).It is stronly advised to use the pre-built vLLM GPU instance, which has already installed Triton and Rocm version flash attention library.if you have other MI300 GPU resource, you could try the docker image of rocm/vllm:latest to run nano-vllm

### RDNA3 Desktop GPUs 
#### using pre-built rocm navi vllm docker image
in these [ROCM vllm navi images](https://hub.docker.com/r/rocm/vllm-dev/tags?name=navi), flash attention library has been installed already. you can choose the latest verison for your test. 
#### using pre-built rocm pytorch docker image
1) use the pre-built rocm docker image to setup ROCM, like rocm/pytorch:latest
2) install the rocm-version flash triton library
```bash
   git clone --recursive https://github.com/Dao-AILab/flash-attention.git
   cd flash-attention 
   FLASH_ATTENTION_TRITON_AMD_ENABLE="TRUE" python setup.py install
```
4) install nano-vllm
5) run benchmark: FLASH_ATTENTION_TRITON_AMD_ENABLE="TRUE" python3 bench.py

   
## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=GeeeekExplorer/nano-vllm&type=Date)](https://www.star-history.com/#GeeeekExplorer/nano-vllm&Date)
