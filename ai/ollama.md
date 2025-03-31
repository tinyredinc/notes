# Ollama Deployment
## Hardware Specs
- CPU: Xeon E5-2686 v4 @ 2.30GHz (18C/36T 2.3-3.0GHz)
- MB: C612 Series Chipset
- RAM: DDR3 RECC 32GB 1866 MT/s * 8
- GPU: Vega 20 [Radeon VII/MI50] * 2
- SSD: KINGSTON SA2000M81000G

## Preparation

- For AMD GPU
```
https://www.amd.com/en/support/download/linux-drivers.html
Radeon™ Software for Linux® version 24.30.4 for Ubuntu 22.04.5 HWE with ROCm 6.3.4

sudo apt update

wget https://repo.radeon.com/amdgpu-install/6.3.4/ubuntu/jammy/amdgpu-install_6.3.60304-1_all.deb

sudo apt install ./amdgpu-install_6.3.60304-1_all.deb

sudo amdgpu-install -y --usecase=graphics,rocm
...this will take a while...

sudo usermod -a -G render,video $LOGNAME

sudo reboot
```
```
Post-install verification checks

dkms status
...amdgpu/6.10.5-2125197.22.04, 5.15.0-135-generic, x86_64: installed

rocminfo
...ROCk module version 6.10.5 is loaded...

clinfo
...Platform Name: AMD Accelerated Parallel Processing...

rocm-smi
...ROCm System Management Interface...
```


- For NVIDIA GPU
```
...Not tried yet...
```

## Ollama Install

- (Auto) Ollama Install Script
```
curl -fsSL https://ollama.com/install.sh | sh
```

- (Manual)Ollama Install
```
1. Download Ollama
curl -L https://ollama.com/download/ollama-linux-amd64.tgz -o ollama-linux-amd64.tgz
sudo tar -C /usr -xzf ollama-linux-amd64.tgz

*Check Files
sudo ls -la /usr/bin/ollama
```

```
2. Download Ollama ROCm Lib
curl -L https://ollama.com/download/ollama-linux-amd64-rocm.tgz -o ollama-linux-amd64-rocm.tgz
sudo tar -C /usr -xzf ollama-linux-amd64-rocm.tgz

*Check Files
sudo ls -la /usr/lib/ollama/rocm/
```
```
3. Adding Ollama as a startup service
sudo useradd -r -s /bin/false -U -m -d /usr/share/ollama ollama
sudo usermod -a -G ollama $(whoami)

sudo vim /etc/systemd/system/ollama.service
***START***
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=$PATH"

[Install]
WantedBy=multi-user.target
***END***

sudo systemctl daemon-reload
sudo systemctl enable ollama
```
```
4. Post-install verification checks
sudo systemctl start ollama
sudo systemctl status ollama
ollama -v
```

## Usage

- Models: https://ollama.com/library
```
gemma3
The current, most capable model that runs on a single GPU.
https://ollama.com/library/gemma3/tags
[17GB] gemma3:27b-it-q4_K_M
[30GB] gemma3:27b-it-q8_0
```
```
qwq
QwQ is the reasoning model of the Qwen series.
https://ollama.com/library/qwq/tags
[20GB] qwq:32b-q4_K_M
[35GB] qwq:32b-q8_0
```
```
deepseek-r1
DeepSeek's first-generation of reasoning models with comparable performance to OpenAI-o1
https://ollama.com/library/deepseek-r1/tags
[4.7G] deepseek-r1:7b-qwen-distill-q4_K_M
[9.0G] deepseek-r1:14b-qwen-distill-q4_K_M
[20GB] deepseek-r1:32b-qwen-distill-q4_K_M 
[43GB] deepseek-r1:70b-llama-distill-q4_K_M 
[404GB] deepseek-r1:671b-q4_K_M 404GB
[713GB] deepseek-r1:671b-q8_0 
[1.3TB] deepseek-r1:671b-fp16 
```
- CLI Reference
```
- Create a model
ollama create mymodel -f ./Modelfile

-ModelFile
https://github.com/ollama/ollama/blob/main/docs/modelfile.md

- Pull a model
ollama pull gemma3:27b-it-q4_K_M
ollama pull qwq:32b-q4_K_M
ollama pull deepseek-r1:32b-qwen-distill-q4_K_M

- Run a model
ollama run qwq:32b-q4_K_M
ollama run qwq:32b-q4_K_M "Summarize this file: $(cat README.md)"
ollama run gemma3:27b-it-q4_K_M "What's in this image? /Users/jmorgan/Desktop/smile.png"

- Remove a model
ollama rm llama3.2

- Copy a model
ollama cp llama3.2 my-model

- Show model information
ollama show qwq:32b-q4_K_M

- List models
ollama list

- PS Models
ollama ps

- Stop a model
ollama stop qwq:32b-q4_K_M
```