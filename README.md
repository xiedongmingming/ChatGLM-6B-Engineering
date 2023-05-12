# CHATGLM-6B-ENGINEERING

Re-edit from [ChatGLM-6B](https://github.com/THUDM/ChatGLM-6B)

https://www.bilibili.com/video/BV1gX4y1B7PV

## 介绍

CHATGLM-6B是一个开源的、支持中英双语的对话语言模型，基于[GENERAL LANGUAGE MODEL(GLM)](https://github.com/THUDM/GLM)架构，具有62亿参数。结合模型量化技术，用户可以在消费级的显卡上进行本地部署

本项目基于CHATGLM-6B进行了后期调教，支持网上搜索及生成图片

生成图片则需要本地部署[STABLE DIFFUSION](https://github.com/AUTOMATIC1111/stable-diffusion-webui)并加载API：

```powershell
python webui.py --xformers --nowebui
```

运行程序需要先运行API.PY，再运行：

```powershell
streamlit run streamlit_new.py
```

加载完成后在浏览器中查看：http://localhost:8501/

## 功能

### 基本对话

可支持上下文对话

![Basic](examples/basic.png "基本对话")

### 生成图片

需要[STABLE DIFFUSION](https://github.com/AUTOMATIC1111/stable-diffusion-webui)支持

![Stable Diffusion](examples/sd.png "Stable Diffusion")

### 网络搜索

![Web](examples/web.png "网络搜索")

### [CLIP](https://github.com/openai/CLIP)(PREVIEW)

需要CLIP-INTERROGATOR支持

![CLIP](examples/clip.png)

## 运行时错误

AssertionError: Torch not compiled with CUDA enabled

RuntimeError: CUDA error: no kernel image is available for execution on the device

请运行

```powershell
nvidia-smi
```

及

```powershell
nvcc -V
```

查看结果 如都正常无ERROR，请运行

```python
import torch
print(torch.cuda.is_available())
```

**如返回为TRUE**

请将在API.PY中第57行

```python
model = AutoModel.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True).quantize(4).half().cuda()
```

更改为

```python
model = AutoModel.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True).half().cuda()
```

**如返回为FALSE**

请确认自己是否已安装GPU版本的TORCH

可参考网络教程

若设备无NVIDIA显卡，可参考[README](https://github.com/THUDM/ChatGLM-6B/blob/main/README.md)修改模型为CPU量化模型

## 引用

Forked from https://github.com/THUDM/ChatGLM-6B
