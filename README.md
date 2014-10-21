# Procedimentos para instalação e execução do ambiente SISLEGIS no Fedora

## Instalação

### Utilizando a vm-fedora:

```bash
# logue-se na console da VM (login: aluno) (Password: @lun0123)
# execute (substituido 'pj' pelo nome de teu usuário em tua máquina):
ssh -fNR 2222:localhost:22 pj@base
# de um shell no HOST, execute:
ssh -p 2222 aluno@localhost
sudo yum -y install git
git clone http://github.com/pensandoodireito/sislegis-ambiente-fedora
cd sislegis-ambiente-fedora/
cp config.exemplo config
vi config
./instalar
sudo su - sislegis
app_baixar
app_remote_add_upstream
app_fetch_upstream
app_merge_upstream_master
mvn clean package -DskipTests
jboss_start &
cp target/sislegis.war $JBOSS_HOME/standalone/deployments
ssh -fNR 8080:localhost:8080 pj@base
# abra o browser na tua máquina HOST em http://localhost:8080/sislegis
```

### Utilizando uma máquina real:

```bash
git clone http://github.com/pensandoodireito/sislegis-ambiente-fedora
cd sislegis-ambiente-fedora/
cp config.exemplo config
vi config
./instalar
sudo su - sislegis
app_baixar
app_remote_add_upstream
app_fetch_upstream
app_merge_upstream_master
mvn clean package -DskipTests
jboss_start &
cp target/sislegis.war $JBOSS_HOME/standalone/deployments
firefox http://localhost:8080/sislegis &> /dev/null &
```

## Salvamento de binários baixados

* Para salvar os binários baixados pelo usuário `sislegis` (nos diretórios `.m2` e `.forge` e `instaladores`) com a finalidade de refazer seu ambiente de forma mais rápida (sem efetuar downloads), execute:
```bash
./salvar
```
* Lembre-se de configurar a variável `BAIXA_INSTALADORES` para `false` no arquivo `config`, antes de solicitar a reinstalação;
* O script `restaurar` será chamado automatimcamente pelo script `instalar` no momento necessário.

## Desinstalação

```bash
./remover
```

## Outros procedimentos (como usuário sislegis)

### Download de addons do JBoss Forge

```bash
forge_instalar_addons
```

### Download e instalação do JBoss Tools

* Download do zip "_update site_":
```bash
jbosstools_baixar
```
* Instalação: siga os procedimentos descritos em http://tools.jboss.org/downloads/jbosstools/luna/4.2.0.CR1.html#zips
