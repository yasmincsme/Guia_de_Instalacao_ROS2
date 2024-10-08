<h4 align="center">Guia de Instação do ROS 2>

> Olá, pessoal! Este guia tem como objetivo principal simplificar a instalação do ROS 2 em desktops e placas. Notei que há uma quantidade significativa de conteúdo online, mas a maioria está em inglês. Por isso, decidi traduzir o material e compilar as melhores referências que encontrei. Espero que gostem e, em caso de dúvidas, podem olhar as referências ou entrar em contato comigo via email pelo endereço cordeiro.yasmin@ieee.org!

## Pré-requisitos para Instalação

- Ao instalar o ROS 2, é recomendado instalar a versão de Suporte de Longo Prazo (LTS), pois ela oferece uma base estável para o seu desenvolvimento.
- Antes da instalação, é crucial verificar a compatibilidade da versão do ROS 2 com a versão do seu sistema operacional. Essas informações podem ser encontradas na [documentação oficial](https://www.ros.org/reps/rep-2000.html#humble-hawksbill-may-2022-may-2027) da versão do ROS 2 que você pretende instalar. Garantir a compatibilidade ajudará a evitar problemas que possam surgir de versões incompatíveis.
- Além disso, é importante verificar a compatibilidade da sua versão do Gazebo com a versão do ROS 2 que você pretende instalar. Você pode verificar isso [aqui](https://gazebosim.org).

## Instalação do ROS 2

### Configurar Localidade

Certifique-se de que você tenha uma localidade que suporte UTF-8. Se você estiver em um ambiente mínimo (como um container Docker), a localidade pode ser algo básico como POSIX. No entanto, deve funcionar bem se você estiver usando uma localidade diferente que suporte UTF-8.

```bash
locale  # verificar se há suporte a UTF-8

sudo apt update && sudo apt install locales # comandos para instalação
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verificar configurações
```

### Configurar Fontes

Você precisará adicionar o repositório apt do ROS 2 ao seu sistema.

Primeiro, certifique-se de que o repositório Ubuntu Universe está habilitado.

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

Agora, adicione a chave GPG do ROS 2 com o apt.

```bash
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Em seguida, adicione o repositório à sua lista de fontes.

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

### Instale os Pacotes do ROS 2

Atualize seus caches de repositório apt após configurar os repositórios.

```bash
sudo apt update
```

Os pacotes do ROS 2 são construídos em sistemas Ubuntu frequentemente atualizados. É sempre recomendável garantir que seu sistema esteja atualizado antes de instalar novos pacotes.

```bash
sudo apt upgrade
```

**Aviso**

Devido a atualizações iniciais no Ubuntu 22.04, é importante que os pacotes relacionados ao systemd e ao udev sejam atualizados antes de instalar o ROS 2. A instalação das dependências do ROS 2 em um sistema recém-instalado, sem realizar a atualização, pode acionar a remoção de pacotes críticos do sistema.

Por favor, consulte ros2/ros2#1272 e Launchpad #1974196 para mais informações.

### Instalação no Desktop

- Inclui ROS, RViz, demonstrações e tutoriais.
- Fornece um ambiente de desktop completo para o desenvolvimento em ROS.

```bash
sudo apt install ros-humble-desktop
```

### Instalação Mínima

- Inclui bibliotecas de comunicação, pacotes de mensagens e ferramentas de linha de comando.
- Não inclui ferramentas GUI, adequado para configurações minimalistas.

```bash
sudo apt install ros-humble-ros-base
```

### Instalação de Ferramentas de Desenvolvimento

Instalaremos o “ros-humble-desktop” no Sistema de Controle e Simulação (nosso Desktop/Laptop) e o “ros-humble-ros-base” no Computador do Robô (Raspberry Pi).

Por fim, instale as ferramentas de Desenvolvimento: Compiladores e outras ferramentas para construir pacotes ROS.

```bash
sudo apt install ros-dev-tools
```
## Exemplo de Uso

Após concluir a instalação, recomendo a execução dos comandos a seguir para conferir se o procedimento foi bem-sucedido.

Em um terminal, execute o arquivo de configuração e depois rode um *Talker* em C++:

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

Em outro terminal, execute o arquivo de configuração e depois rode um *Listener* em Python:

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```

Você verá o *Talker* dizendo que está publicando mensagens e o *Listener* dizendo "Eu ouvi essas mensagens". Isso verifica que as APIs de C++ e Python estão funcionando corretamente e que a instalação do ROS 2 Humble foi concluída com sucesso.  

## Bônus

O `.bashrc` é um arquivo de configuração usado pelo Bash (um dos shells mais comuns no Linux e no macOS). Ele é executado sempre que uma nova sessão de terminal interativa é aberta. O arquivo `.bashrc` contém comandos e variáveis de ambiente que configuram o ambiente de trabalho do usuário.

Para automatizar o processo de configuração do ambiente e evitar executar o comando de *source* manualmente toda vez, podemos adicionar o comando ao arquivo `.bashrc`. Dessa forma, o comando será executado automaticamente sempre que abrirmos um novo terminal ou sessão SSH.

Veja como podemos editar o `.bashrc`:

```bash
nano ~/.bashrc
```

E adicionar o comando `source /opt/ros/humble/setup.bash` ao final do arquivo.

Isso garantirá que o ambiente do ROS 2 seja configurado automaticamente.

## Referências

https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html

https://medium.com/@nullbyte.in/raspberry-pi-4-ubuntu-20-04-lts-ros2-a-step-by-step-guide-to-installing-the-perfect-setup-57c523f9d790

https://medium.com/spinor/getting-started-with-ros2-install-and-setup-ros2-humble-on-ubuntu-22-04-lts-ad718d4a3ac2
