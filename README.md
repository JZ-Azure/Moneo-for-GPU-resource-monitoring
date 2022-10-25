# Moneo-for-GPU-resource-monitoring

## First setup CC cluster with SLURM. 
[Use-AzureHPC-to-deploy-CycleCloud-Cluster](https://github.com/JZ-Azure/Use-AzureHPC-to-deploy-CycleCloud-Cluster)

## Install conda and docker on the CC cluster login node
Install conda. [Installation instructions](https://docs.conda.io/en/latest/miniconda.html#linux-installers)
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
bash Miniconda3-py39_4.12.0-Linux-x86_64.sh
```
Install docker. [Installation instructions](https://docs.docker.com/engine/install/ubuntu/)
```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
```bash
sudo docker run hello-world
```
## Install and deploy Moneo
```bash
# get the code
git clone https://github.com/Azure/Moneo.git
cd Moneo

# install dependencies
python3 -m pip install ansible
```
```bash
cp ./tests/data/host.ini ./
vim host.ini
```
```bash
$ cat host.ini
[master]
10.21.8.6

[worker]
10.21.8.9
10.21.8.10

[all:vars]
ansible_user=hpcadmin
```

Deploy Moneo on all nodes
```bash
python3 moneo.py -d -c host.ini full
```

View Grafana dash board from address `10.21.8.6:3000`. The default username and password are both `azure`. 
![vnet_image1](/images/Moneo_1.png)
![vnet_image2](/images/Moneo_2.png)
![vnet_image3](/images/Moneo_3.png)
