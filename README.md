# ngc_utilities
Nice starting points for using Nvidia NGC images on Ubuntu. Perfect PyTorch, more reproducible.

⭐️ List of all containers: https://catalog.ngc.nvidia.com/containers (requires Developer account)

## Install Docs

Linux install: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#install-guide

Overview: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/overview.html

Requires: Regular Docker AND specific `nvidia-docker2`

Regular docker install (don't do if you already have docker)
```
curl https://get.docker.com | sh \
  && sudo systemctl --now enable docker
```

Fetch special docker:

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

Install special Docker:

```bash
sudo apt-get update; \
  sudo apt-get install -y nvidia-docker2; \
  sudo systemctl restart docker
```

Test that it works (warning: 5GB+ download).
```
# test with ubuntu & cuda 11.0.3
sudo docker run --rm --gpus all nvidia/cuda:11.0.3-base-ubuntu20.04 nvidia-smi
# or test with pytorch and cuda 11.7. See the doc links at top of this readme.
sudo docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 nvcr.io/nvidia/pytorch:22.08-py3 nvidia-smi
```


## Install Nvidia drivers
```bash
sudo apt install nvidia-headless-515-server -y

nvidia-smi

sudo shudown -r now
```

For other versions, use `sudo apt search nvidia-driver`. 

I recommend the `server` version (or `server-headless` for headless machines) because it's more stable, slower updates. Used in industry. 

### Troubleshooting

```bash
sudo apt-get install -y nvidia-container-toolkit
```

Might need to purge snap version of docker. Just running this fixed it for me I think. 
```bash
sudo snap remove --purge docker
```

## Run

For testing (run `nvidia-smi`)
```bash
sudo docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 nvcr.io/nvidia/pytorch:22.08-py3 nvidia-smi
```
For interactive use (run `bash`):
Running with local dirs mounted with `-v`. Use custom images with `-t`, get interactive access with `-i/--interactive`. 
```bash
# note this is using container named -t ngc_alpa
sudo docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v /home/kastan/ai/alpa:/workspace/alpa --interactive -t ngc_alpa bash
```

## 4090 Pytorch Compatability 
Lovelace (`compute_89`) requires Cuda 11.8 and later. Pytorch is not yet distributed with 11.8 (latest is 11.7), so we use NGC containers to get around it. 
```bash
 sudo docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -it -v /home/kastan/alexa/:/workspace nvcr.io/nvidia/pytorch:22.12-py3 bash
```

TODO: Write docs on taking snapshot of running image. 
