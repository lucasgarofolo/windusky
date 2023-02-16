# Windusky
## Projeto Windusky proposto por Lucas Garofolo
### A ideia é aplicar os aprendizados em engenharia e ciência de dados do início ao fim do projeto nos dados de sua tese como base o site Ventusky.

Primeiramente, iniciaremos com uma abordagem local, posteriormente passaremos para aplicação em redes e quem sabe, online ou em cloud.

O volume de dados é imenso, não necessito obviamente de velocidade mas é bom levar em conta como se fosse uma aplicação com usuários. Os dados são estruturados, são binários, organizados em horário a cada 3 horas entre os anos de 1961 a 2005. A estrutura interna dos dados são em Latitude e Longitude, todos os arquivos sob o mesmo domínio. Dentro de cada arquivo horário possui 38 variáveis diferentes.

Pela quantidade de dados e a possibilidade de clusterização, optei por iniciar com o Hadoop. O sistema de arquivos distribuídos HDFS e o processamento via MapReduce consegue lidar bem com o volume de dados, além de ter ferramentas possíveis de Data Lake e Data Warehouse.

O Hadoop está sob a tutela da Fundação Apache e possui 3 módulos: Hadoop Common, Hadoop MapReduce e Hadoop Distributed File System (HDFS).

A arquitetura do HDFS é estruturada em master-slave.

O Hadoop trabalha com 5 processos: NameNode, DataNode, SecondaryNameNode, JobTracker e TaskTracker.

## Instalação e configuração do Hadoop

## DO 0 por que essa de cima não deu bom
#### Instalação do OpenJDK no Ubuntu
sudo apt update
sudo apt install openjdk-8-jdk -y

#### Tornar o java 8 o principal no sistema 
sudo update-alternatives --config java
sudo update-alternatives --config javac

#### Configurando um usuário não-root para o ambiente Hadoop
sudo adduser hadoop

#### habilitar uma ssh para não necessitar de senha para o hadoop user
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys

#### Download Hadoop
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.2.4/hadoop-3.2.4.tar.gz
tar xzf hadoop-3.2.1.tar.gz

#### Configurar o variaveis de ambiente do Hadoop
sudo nano .bashrc

#Hadoop Related Options
export HADOOP_HOME=/home/hadoop/hadoop-3.2.4
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib/native"

source ~/.bashrc

#### Editar o arquivo hadoop-env.sh
sudo nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
##### pra saber o caminho do Java
which javac
readlink -f /usr/bin/javac

#### Editar o arquivo core-site.xml
nano $HADOOP_HOME/etc/hadoop/core-site.xml
<configuration>
<property>
  <name>hadoop.tmp.dir</name>
  <value>/home/hadoop/tmpdata</value>
</property>
<property>
  <name>fs.default.name</name>
  <value>hdfs://127.0.0.1:9000</value>
</property>
</configuration>

mkdir /home/hadoop/tmpdata

#### Editar hdfs-site.xml
nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml

<configuration>
<property>
  <name>dfs.data.dir</name>
  <value>/home/hadoop/dfsdata/namenode</value>
</property>
<property>
  <name>dfs.data.dir</name>
  <value>/home/hadoop/dfsdata/datanode</value>
</property>
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>
</configuration>

mkdir /home/hadoop/dfsdata/
mkdir /home/hadoop/dfsdata/namenode
mkdir /home/hadoop/dfsdata/datanode

#### Editar arquivo mapred-site.xml
nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
<configuration> 
<property> 
  <name>mapreduce.framework.name</name> 
  <value>yarn</value> 
</property> 
</configuration>

#### Editar arquivo yarn-site.xml
nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>127.0.0.1</value>
</property>
<property>
  <name>yarn.acl.enable</name>
  <value>0</value>
</property>
<property>
  <name>yarn.nodemanager.env-whitelist</name>   
  <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PERPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>

#### Formatar o namenode hdfs
hdfs namenode -format

#### Iniciar o cluster hadoop
Vá até o diretório hadoop-3.2.4/sbin
./start-dfs.sh
./start-yarn.sh
jps !para conferir se os processos iniciaram
#### Neste momento eu tive que copiar a chave pública 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

#### Dai deu erro no datanodes e eu parei o serviço e reformatei o namenodes
sbin/stop-dfs.sh
sudo bin/hdfs namenode -format

#### era o caminho do datanode no arquivo hdfs-site.xml que estava errado


