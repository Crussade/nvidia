# Dokumentation Nvidia-MTMC Deployment

[1. Install Docker](#install-docker)

[2. Install Nvidia Container Toolkit](#install-nvidia-container-toolkit)






## Install Docker

1. Uninstall old versions:

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

2. Set up the repository:

   ```
   # Add Docker's official GPG key:
   sudo apt-get update
   sudo apt-get install ca-certificates curl gnupg
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   sudo chmod a+r /etc/apt/keyrings/docker.gpg

   # Add the repository to Apt sources:
   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update
   ```
3. Install Docker Engine:

   ```
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

## Install Nvidia Container Toolkit

1. Setup the repository and the GPG key:

   ```
   sudo mkdir -p /etc/apt/keyrings
   curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /etc/apt/keyrings/nvidia-container-toolkit-keyring.gpg


   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/nvidia-container-toolkit-keyring.gpg] https://nvidia.github.io/libnvidia-container/stable/deb/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list > /dev/null
   ```
2. Install the nvidia-docker2 package:

   ```
   sudo apt-get update
   sudo apt-get install -y nvidia-container-toolkit
   ```
3. Configure the Docker daemon to recognize the Nvidia Container Runtime:

   ```
   sudo nvidia-ctk runtime configure --runtime=docker
   ```
4. Restart the Docker daemon:

   ```
   sudo systemctl restart docker
   ```
5. Verify Installation Importent Docker is non-sudo user:

   ```
   docker run --rm --gpus all nvidia/cuda:12.2.0-base-ubuntu20.04 nvidia-smi
   ```

## Login int Nvidia Team with API Key

Create an NGC account. Go to [NGC webstie](https://ngc.nvidia.com/).

Generate the API key. Be careful dont use personal key.

Log into nvcr.io on your system:

```
docker login nvcr.io
Username: $oauthtoken
Password: <your ngc API key>
```

Disable IPv6

```
sudo sysctl -w net.ipv6.conf.lo.disable_ipv6=1
```
