# Procedimentos para instalação e execução do [sislegis-app] no [Fedora]

## Fork de projetos

Para construir o [sislegis-app] num ambiente Fedora o primeiro passo é você [fazer o fork](https://help.github.com/articles/fork-a-repo/) dos seguintes projetos:
* [sislegis-dotfiles]
* [sislegis-ambiente]
* [sislegis-app]

A máquina para a execução desse ambiente pode ser real (física) ou virtual (VM). Alternativamente, você também pode montar um contêiner [Docker] com todo o ambiente. Apresentamos, aqui, todos os passos para isso. Enfim, você escolhe a forma como deseja montar o teu ambiente para o sislegis-app dependendo de tuas necessidades.

## Inicialização e configuração da máquina

### Na [vm-fedora], utilizando o [VirtualBox]

Após baixar, extrair, registrar e iniciar a vm-fedora em teu ambiente, execute:
```bash
# logue-se na console da VM (login: aluno) (Password: @lun0123)
# execute (substituido 'pj' pelo nome de teu usuário em tua máquina):
ssh -fNR 2222:localhost:22 pj@base
# de um shell no HOST, execute:
ssh -p 2222 aluno@localhost
sudo yum -y install git
```

### Numa máquina real executando o Fedora

Basta estar logado com um usuário com poderes administrativos (privilégios de `root` concedidos pelo uso do `sudo`).

### Num contêiner fedora, utilizando o [Docker]

Execute um contêiner fedora da seguinte forma:
```bash
sudo docker run -it -p 8080:8080 fedora /bin/bash
```
Instale o git nesse contêiner:
```bash
yum -y install git
```

## Instalação do ambiente e login como usuário sislegis

```bash
git clone http://github.com/pensandoodireito/sislegis-ambiente-fedora
cd sislegis-ambiente-fedora/
cp config.exemplo config
vi config
./instalar
sudo su - sislegis
```

## Download dos fontes mais atuais e geração do pacote binário

```bash
app_baixar
app_remote_add_upstream
app_fetch_upstream
app_merge_upstream_master
mvn clean package -DskipTests
```

## Deploy, execução e acesso a aplicação sislegis-app

Inicie o JBoss e faça o deploy da aplicação:
```bash
jboss_start &
cp target/sislegis.war $JBOSS_HOME/standalone/deployments
```

### Na [vm-fedora], utilizando o [VirtualBox]

Crie um túnel para acesso a porta 8080 a partir de teu HOST:
```bash
ssh -fNR 8080:localhost:8080 pj@base
```
Abra o browser na tua máquina HOST em http://localhost:8080/sislegis

### Numa máquina real executando o Fedora

Abra o firefox diretamente na porta 8080:
```bash
firefox http://localhost:8080/sislegis &> /dev/null &
```

### Num contêiner fedora, utilizando o [Docker]

Abra o browser na tua máquina HOST em http://localhost:8080/sislegis. O túnel é criado automaticamente na inicialização do Docker.

## Salvamento de binários baixados

Para salvar os binários baixados pelo usuário `sislegis` (nos diretórios `.m2` e `.forge` e `instaladores`) com a finalidade de refazer seu ambiente de forma mais rápida (sem repetir downloads já realizados), execute:
```bash
./salvar
```

Caso deseje, você pode configurar a variável `BAIXA_INSTALADORES` para `false` no arquivo `config`, após salvar os instaladores com o comando acima. Isso ainda reduzirá o tempo de instalação das ferramentas pois, dessa forma, o script que faz esse trabalho não tentará refazer download algum.

O script `restaurar` será chamado automaticamente pelo script `instalar` no momento necessário.

## Desinstalação do ambiente

```bash
./remover
```

## Outros procedimentos (como usuário sislegis)

### Configuração para atualização do fork do sislegis-app a partir do repositório principal

```bash
app_remote_add_upstream
``` 

### Atualização do fork do sislegis-app

```bash
app_fetch_upstream
app_merge_upstream_master
``` 

### Download de addons do JBoss Forge

```bash
forge_instalar_addons
```

### Download e instalação do JBoss Tools

Download do zip "_update site_":
```bash
jbosstools_baixar
```
Instalação: siga os procedimentos descritos em http://tools.jboss.org/downloads/jbosstools/luna/4.2.0.CR1.html#zips

[Docker]:http://www.docker.com
[VirtualBox]:http://virtualbox.org
[sislegis-ambiente]:http://github.com/pensandoodireito/sislegis-ambiente
[sislegis-dotfiles]:http://github.com/pensandoodireito/sislegis-dotfiles
[vm-fedora]:http://gdriv.es/vm-fedora
[Fedora]:http://fedoraproject.org
[sislegis-app]:http://github.com/pensandoodireito/sislegis-app
