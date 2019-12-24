Installation Go language to the Linux system 
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

Export Go language environments
~~~sh
# ^ ...
export GOPATH=$HOME/Go-main && export GOROOT=/usr/local/go
export PATH=$PATH:$GOPATH/bin:$GOROOT/bin
~~~

Save environment to user profile for vision Go after reboot
~~~sh
# ^ ...
cat >> ~/.profile << EOF
export GOPATH=$HOME/Go-main
export GOROOT=/usr/local/go
export PATH=$PATH:$GOPATH/bin:$GOROOT/bin
EOF
~~~

##### [Back to Home](https://github.com/motousr77/helm/)