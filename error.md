## Apache Flume
Decidi utilizar o Apache Flume para ingestão dos dados binários, estruturados. Como meus dados não são em tempo real e são arquivos em lote, optei por essa tecnologia através de pesquisas onde vi que essa ferramenta possui uma melhor performance para esse tipo de dado. Pretendo testar algumas outras tecnologias após obtiver sucesso com esta.

### Instalação
Fiz o download da versão 1.11.0 e descompactei na pasta de downloads mesmo. Estou usando o usuário hadoop para realizar essa operação. Descompactei com

tar xzf apache-flume-1.11.0-bin.tar.gz

#### inseri as variáveis de ambiente no .bashrs
export FLUME_HOME=/home/dataflair/apache-flume-1.9.0-bin
export PATH=$PATH:$FLUME_HOME/bin

source .bashrc

#### já está instalado, negócio é bruto mesmo
Para ver a versão do flume instalado é só executar:

flume-ng version 

#### Configurando o Apache Flume
Criar um arquivo de configuração dentro da pasta do Flume
touch flume.conf

## Disclaimer: percebi que o Flume é mais utilizado para arquivos de log. A maioria dos materiais na internet é sobre log e, através de pesquisas percebi que o Apache NiFi parece uma boa opção para arquivos binários.