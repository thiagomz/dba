# Linux Pre install

```
yum -y install oracle-database-preinstall-19c
yum -y install java-1.8.0-openjdk tigervnc-server tmux htop nmon compat-openssl10 -y

groupadd -g 54421 oinstall
groupadd -g 54422 dba
usermod -g oinstall -G dba,oper,wheel,vboxsf oracle 

# Ajustando Horario
systemctl enable chronyd
systemctl restart chronyd
chronyc -a 'burst 4/4'
chronyc -a makestep

# Ajustando SELinux
sed -i -e "s|SELINUX=enforcing|SELINUX=permissive|g" /etc/selinux/config
cat /etc/selinux/config
setenforce permissive

# Desativando o firewall (Somente em Laboratorio)
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld

# Configuração do tmux (scroll de linhas)
# Criar o arquivo no home do usuario oracle
$ cat /home/oracle/.tmux.conf
set -g mouse on
```
