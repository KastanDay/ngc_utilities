# ngc_utilities
Nice starting points for using Nvidia NGC images on Ubuntu. Perfect PyTorch, more reproducible.

⭐️ List of all containers: https://catalog.ngc.nvidia.com/orgs/nvidia/containers/

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
sudo apt-get update; sudo apt-get install -y nvidia-docker2 && sudo systemctl restart docker
```

Test it:
```
sudo docker run --rm --gpus all nvidia/cuda:11.0.3-base-ubuntu20.04 nvidia-smi
```


## install Nvidia drivers
```
sudo apt install nvidia-driver-515-server -y

nvidia-smi
```

## Run


```bash
echo "BEST DOCKER PYTORCH COMMAND";
sudo docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 nvcr.io/nvidia/pytorch:22.08-py3 nvidia-smi
```
