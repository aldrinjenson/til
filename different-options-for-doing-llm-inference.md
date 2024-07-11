---
description: >-
  Note formatted by Claude after from my voicenote transcription. About
  different options I've explored for hosting gen-ai models
---

# Different options for doing LLM inference

### Intro

While many developers default to using OpenAI's APIs for large language model (LLM) inference, there are numerous alternatives available that offer different advantages in terms of cost, performance, and flexibility. This guide explores various options for LLM inference, including both self-hosted and cloud-based solutions, as well as serverless and traditional hosting approaches.

### Cloud-Based Solutions

#### 1. Replicate

* **Pros**:
  * Offers thousands of models, including popular and fine-tuned versions
  * Supports custom model deployment (both public and private)
  * Easy to get started quickly
* **Cons**:
  * Initial pricing may be higher compared to other options
* If you want to build a prototype quickly for a startup, this is recommended

#### 2. Modal.com

* **Pros**:
  * Affordable pricing (e.g., 10 applications for $9 on GPUs)
  * Serverless architecture
* **Cons**:
  * Cold start issues, which can lead to longer initial processing times
* Based on experience with Indic Subtitler, this is cheap but the free tier will have sufficient lag if you have a large model which needs to be loaded to memory once the serverless system boots up

#### 3. Hugging Face Spaces

* **Pros**:
  * Always running, no cold start issues
  * Exposes API using Gradio
* **Cons**:
  * No pay-as-you-go pricing

#### 4. Fal.ai

* Offers serverless inference options
* Compare with Replicate and make a decision

#### 5. Groq

* **Pros**:
  * Significantly cheaper than OpenAI (e.g., 10x less cost for Whisper model)
  * Faster processing due to specialized LPU chips
* **Cons**:
  * Limited model availability (currently offering Whisper v3, Llama 2, Llama 3, Gemma, Mixtral)
  * Requires contacting for premium trial, with potential delays as of now

#### 6. AWS Options

* **Bedrock**: Umbrella service for various AI tools
* **SageMaker**: For model training and deployment

#### 7. NVIDIA NIM (New)

* **Pros**:
  * Containerizes LLM models (similar to Kubernetes approach)
  * Allows local development and easy cloud deployment
  * Simple inference, similar to OpenAI APIs
  * Supports local testing and cloud deployment
* **Cons**:
  * Currently not open for everyone (requires filling out a form to try)
* How it works:
  * Containerize your LLM model (e.g., GPT-3.5, GPT-4)
  * Develop locally
  * Push to cloud, which spins up a separate container for the model to run

### Self-Hosted Solutions

#### 1. eXLama V2

* **Pros**:
  * One of the fastest open-source LLM inference options
  * Supports quantized models
  * Based on LLaMA.cpp
* **Cons**:
  * Cannot offload to CPU; requires good VRAM

#### 2. VLLM

* **Pros**:
  * High-performance inference
* **Cons**:
  * Does not support quantized models

#### 3. LLaMA.cpp

* **Pros**:
  * Supports quantized models
  * Offers HTTP server functionality for API exposure

#### 4. BentoML

* Supports serverless inference
* Kubernetes based
* Open Source

#### 5. Seldon.io

* Kubernetes-based open-source solution for scaling models
* Alternative to BentoML

#### 6. Ollama&#x20;

* **Pros**:
  * One of the easiest ways to run models locally
  * Supports multiple GGUF format versions easily pullable from Ollama hub
  * Offers both CLI and API interfaces
  * Recently added support for concurrency and parallelization
* **How it works**:
  * Pull models from Ollama hub
  * Run locally with simple commands
  * Use CLI or API for inference

### Considerations When Choosing an LLM Inference Solution

1. **Cost**: Compare pricing structures, especially for long-term usage.
2. **Performance**: Consider factors like cold start times and inference speed.
3. **Scalability**: Evaluate how well the solution can handle increasing loads.
4. **Flexibility**: Check support for custom models and fine-tuning capabilities.
5. **Ease of Use**: Consider the learning curve and integration complexity.
6. **Hardware Requirements**: For self-hosted solutions, ensure you have the necessary hardware (e.g., GPUs with sufficient VRAM).

### Conclusion

While OpenAI's APIs are popular, exploring alternative LLM inference options can lead to cost savings, improved performance, or better alignment with specific project requirements. For those just starting, cloud-based solutions like Replicate or Modal.com offer an easy entry point. As your needs grow or become more specific, you might consider transitioning to more comprehensive services like AWS or self-hosted solutions for greater control and customization.

For local development and testing, tools like Ollama provide an excellent balance of ease of use and flexibility. NVIDIA NIM presents an interesting containerized approach that could bridge the gap between local development and cloud deployment.

Remember to regularly reassess your chosen solution as the field of AI and LLMs is rapidly evolving, with new options and improvements emerging frequently.
