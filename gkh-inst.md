## When Docker allready installed do next:

### Some installation of Go lang
~~~sh
mkdir $HOME/work && cd $HOME/work
wget https://dl.google.com/go/go1.13.5.linux-amd64.tar.gz
tar -xzf go1.13.5.linux-amd64.tar.gz
mkdir tmp && mv go1.13.5.linux-amd64.tar.gz tmp/
mkdir $HOME/work/main-path
export GOROOT=$HOME/work/go
export GOPATH=$HOME/work/main-path
export PATH=$PATH:$GOROOT/bin
~~~

### Some inatallation of Kind
~~~sh
GO111MODULE="on" go get sigs.k8s.io/kind@v0.6.1
# kind pu into $(go env GOPATH)/bin
cat >> ~/.profile << EOF
export PATH=$PATH:$(go env GOPATH)/bin
EOF
#
source $HOME/.profile
~~~

### Install kubectl
From: https://kubernetes.io/docs/tasks/tools/install-kubectl/
~~~sh
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
~~~

### Install Helm 2
From: https://v2.helm.sh/docs/using_helm/#installing-helm
Releases: https://github.com/helm/helm/releases
~~~sh
wget https://get.helm.sh/helm-v2.16.1-linux-amd64.tar.gz
tar -xzf helm-v2.16.1-linux-amd64.tar.gz
mv tar -xzf helm-v2.16.1-linux-amd64.tar.gz tmp/
mv linux-amd64/helm /usr/local/bin/helm
~~~
_
https://golang.org/doc/install
https://golang.org/dl/
https://kind.sigs.k8s.io/docs/user/quick-start

####### Some template:
~~~sh
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
~~~