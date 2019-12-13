### Installation Docker CE (debian)
~~~sh
# ^ do not Copy-Paste All of Scope!!! Because it can be broken.
# ^ update the APT package index:
sudo apt update

# ^ install packages to allow apt to use a repository over HTTPS:
sudo apt install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# ^ add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# ^ verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88
sudo apt-key fingerprint 0EBFCD88

# ^ use the following command to set up the stable repository:
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
# ^ update the APT package index:
sudo apt update

# ^ (Optional) Get list versions of docker-ce
sudo apt-cache policy docker-ce

# ^ (Optional) Install 18.06.3 version of docker-ce  
sudo apt-get install docker-ce=18.06.3~ce~3-0~ubuntu containerd.io

# ^ check installation:
sudo docker version

# ^ if you want use docker without sudo do this:
sudo usermod -aG docker $USER
# ^ then exit from $USER session and login again.
~~~
... from: https://docs.docker.com/install/linux/docker-ce/ubuntu/
\
### Installation Go to some working direcoty (NOT recomended for production)
~~~sh
# ^ maked Work directories
mkdir $HOME/work && cd $HOME/work
wget https://dl.google.com/go/go1.13.5.linux-amd64.tar.gz
tar -xzf go1.13.5.linux-amd64.tar.gz

# ^ some moves ...
mkdir tmp && mv go1.13.5.linux-amd64.tar.gz tmp/
mkdir $HOME/work/main-path

# ^ create and save environment variables
export GOROOT=$HOME/work/go
export GOPATH=$HOME/work/main-path
export PATH=$PATH:$GOROOT/bin
~~~
... from: https://golang.org/doc/install ... releases: https://golang.org/dl/
\
### Inatallation Kind (kubernetes cluster in docker)
~~~sh
GO111MODULE="on" go get sigs.k8s.io/kind@v0.6.1
# ^ Go put Kind into $(go env GOPATH)/bin
cat >> ~/.profile << EOF
export PATH=$PATH:$(go env GOPATH)/bin
EOF
# ^ refresh env
source $HOME/.profile
~~~
... from: https://kind.sigs.k8s.io/docs/user/quick-start
\
### Installation kubectl
~~~sh
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
~~~
... from: https://kubernetes.io/docs/tasks/tools/install-kubectl/
\
### Inatallation Helm 2
~~~sh
wget https://get.helm.sh/helm-v2.16.1-linux-amd64.tar.gz
tar -xzf helm-v2.16.1-linux-amd64.tar.gz
mv helm-v2.16.1-linux-amd64.tar.gz tmp/
sudo mv linux-amd64/helm /usr/local/bin/helm
~~~
... from: https://v2.helm.sh/docs/using_helm/#installing-helm
... releases: https://github.com/helm/helm/releases
\

#### Next Steps: [Create Kind-cluster & Install Applications](https://github.com/motousr77/helm/blob/master/gkh-inst.md)
