# Instalação automatizada dos recursos em ambiente Linux/WSL via shell script após instalação do SO

O script mais abaixo é um shell script e com ele é possível instalar todas as dependências necessárias para a realização da trilha, bem como outros aplicativos que serão úteis para o dia-a-dia da equipe.

Algumas referências utilizadas:

- OhmyZSH
  - Como instalar o ohmyZSH no ubuntu 22.04:
    - Passo a passo no [gist](https://gist.github.com/luizomf/1fe6c67f307fc1df19e58f224134dc8f)
    - [Video](https://www.youtube.com/watch?v=5i3TpDR8muU) explicativo do Otávio Miranda
    - [Instalar](https://medium.com/@satriajanaka09/setup-zsh-oh-my-zsh-powerlevel10k-on-ubuntu-20-04-c4a4052508fd) o Powerlevel10k
  - Microsoft Teams For Linux:
    - [Resolução](https://linuxconfig.org/how-to-enable-disable-wayland-on-ubuntu-20-04-desktop) caso não consiga compartilhar a tela no Teams (Ubuntu 22.04)

```bash
#!/bin/bash

# atualizar o sistema
sudo apt-get -y update && sudo apt-get -y upgrade && sudo apt-get -y autoclean && sudo apt-get -y autoremove

# instalar o curl
sudo apt install -y curl

# instalar o wget
sudo apt-get install -y wget

# instalar o dpkg
sudo apt install -y dpkg

# instalar o snap
sudo apt install -y snapd

# instalar yarn
sudo apt install -y yarn

# instalar o git
sudo apt-get install -y git

# instalar teams for linux
# https://technoracle.com/how-to-fix-zoom-screen-sharing-on-ubuntu-22-04-quickly/
sudo snap install teams-for-linux

#instalar o dbeaver
echo "deb https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -
sudo apt-get install dbeaver-ce

# instalar nodejs
sudo apt install -y nodejs

# instalar npm
sudo apt install -y npm

# instalar o robo3t (mongodb)
sudo snap install robo3t-snap

# Ajustar o repositório do Docker
# Add Docker's official GPG key:
# sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# instalar a última versão do Docker
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# instalar o kubectl+kubectx
sudo apt install kubectx

# instalar o minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

# instalar o helm
sudo snap install helm --classic

# instalar o terraform
# sudo apt update
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y
terraform --version

# instalar o cli do AWS
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"                                                                 
unzip awscliv2.zip
sudo ./aws/install

# instalar o vs code
sudo snap install --classic code

# instalar as extensões do vscode
code --install-extension davidanson.vscode-markdownlint
code --install-extension eamodio.gitlens
code --install-extension ms-azuretools.vscode-docker
code --install-extension ms-ceintl.vscode-language-pack-pt-br
code --install-extension ms-kubernetes-tools.vscode-kubernetes-tools
code --install-extension ms-python.debugpy
code --install-extension ms-python.python
code --install-extension ms-python.vscode-pylance
code --install-extension ms-vscode-remote.remote-containers
code --install-extension oderwat.indent-rainbow
code --install-extension pkief.material-icon-theme
code --install-extension redhat.vscode-yaml
code --install-extension tal7aouy.rainbow-bracket

# instalar melhorias no terminal com o ohmyzsh
sudo apt install -y zsh
sudo chsh -s $(which zsh)
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1
ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
mkdir -p ~/.fonts
git clone https://github.com/pdf/ubuntu-mono-powerline-ttf.git ~/.fonts/ubuntu-mono-powerline-ttf
fc-cache -vf
```

## Instalação:

No prompt, digitar:
```bash
mkdir <pasta_scripts>
cd <pasta_scripts>
touch <nome_do_arquivo>.sh
vi <nome_do_arquivo>.sh
```

Uma vez que o editor Vim abrir no terminal , colar o script acima e digitar o comando abaixo e, por fim, pressionar enter:
```vim
:wq <ENTER>
```

Uma vez que o arquivo estiver salvo é preciso habilitar para que ele seja executado como um programa:
```bash
chmod -x <nome_do_arquivo>.sh
```

Agora que ele está habilitado, devemos iniciar o processo de instalação:
```bash
./<nome_do_arquivo>.sh
```

Será solicitado a senha de usuário para que o processo dê continuidade. Caso algumas das dependências acima já estiverem instaladas na máquina, basta suprir o comando com “#” no início da linha ou simplesmente deletar as linhas de seu script.


