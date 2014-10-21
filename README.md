# Procedimentos para montagem e execução do SISLEGIS no Fedora

## Utilizando a vm-fedora:

```bash
# logar na console da VM (login: aluno) (Password: @lun0123)
# executar, substituido 'pj' pelo nome de teu usuário em tua máquina:
ssh -fNR 2222:localhost:22 pj@base
# de um shell no HOST, executar:
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
mvn clean package -DskipTests
jboss_start &
cp target/sislegis.war $JBOSS_HOME/standalone/deployments
ssh -fNR 8080:localhost:8080 pj@base
# abra o browser, no HOST, em http://localhost:8080/sislegis
```

## Utilizando uma máquina real:

```bash
git clone http://github.com/pensandoodireito/sislegis-ambiente-fedora
cd sislegis-ambiente-fedora/
cp config.exemplo config
vi config
./instalar
sudo su - sislegis
```
