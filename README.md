### Installation Docker CE to the Linux system (debian/ubuntu)
~~~sh
# ^ do not Copy-Paste All of Scope!!! Because it can be broken.
# ^ update the APT package index
sudo apt update

# ^ install packages to allow apt to use a repository over HTTPS
sudo apt install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# ^ add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# ^ verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88
sudo apt-key fingerprint 0EBFCD88

# ^ use the following command to set up the stable repository
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# ^ update the APT package index
sudo apt update

# ^ ERROR scope
# ^ if you catch update error (update failed) then you need some solution (will be edit)
sudo apt-add-repository -r 'deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable'
# ^ this is the temporary solution until i find new
sudo apt update && sudo apt install docker.io -y
# ^ RORRE epocs

# ^ get list versions of docker-ce (Optional)
sudo apt-cache policy docker-ce

# ^ install docker-ce (Optional) 18.06.3 version
sudo apt-get install -y docker-ce=18.06.3~ce~3-0~ubuntu containerd.io

# ^ check installation
sudo docker version

# ^ if you want use docker without sudo do this
sudo usermod -aG docker $USER
# ^ then exit from $USER session and login again
# ^ may be need to reboot (frequently in ubuntu)
~~~
_ from: https://docs.docker.com/install/linux/docker-ce/ubuntu

### Installation Go language to the Linux system 
~~~sh
# ^ prerequisite to installation Go language
export GOVERSION=1.13.5
export OS=$(uname -s | tr '[:upper:]' '[:lower:]')
export GOVAR=go$GOVERSION.$OS-amd64.tar.gz

# ^ install go
wget https://dl.google.com/go/${GOVAR}
sudo tar -C /usr/local -xzf $GOVAR
# ^ ...
{
  if [ ! -d $HOME/Go-main ]; then
    mkdir $HOME/Go-main
  fi
}
~~~

Create Go language environment variables
~~~sh
# ^ ...
export GOPATH=$HOME/Go-main && export GOROOT=/usr/local/go
export PATH=$PATH:$GOPATH/bin:$GOROOT/bin
~~~

Save environment variables to user profile for vision Go after reboot
~~~sh
# ^ ...
cat >> ~/.profile << EOF
export GOPATH=$HOME/Go-main
export GOROOT=/usr/local/go
export PATH=$PATH:$HOME/Go-main/bin:/usr/local/go/bin
EOF
~~~
_ from: https://golang.org/doc/install _ releases: https://golang.org/dl


### Installation Kind (Kubernetes in Docker)
~~~sh
# ^ install Kind
GO111MODULE="on" go get sigs.k8s.io/kind@v0.6.1
~~~

### Installation kubectl
~~~sh
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
~~~
_ from: https://kubernetes.io/docs/tasks/tools/install-kubectl/

### Inatallation Helm 2 (2.16.1 in case below)
~~~sh
wget https://get.helm.sh/helm-v2.16.1-linux-amd64.tar.gz
tar -xzf helm-v2.16.1-linux-amd64.tar.gz
mv helm-v2.16.1-linux-amd64.tar.gz tmp/
sudo mv linux-amd64/helm /usr/local/bin/helm
~~~
_ from: https://v2.helm.sh/docs/using_helm/#installing-helm
_ releases: https://github.com/helm/helm/releases

#### Next Step: [Create Kind-cluster](https://github.com/motousr77/helm/blob/master/gkh-inst.md)
