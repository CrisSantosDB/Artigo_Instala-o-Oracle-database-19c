![Oracle](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/oracle.png)
<div style="text-align: center;">
<p style="text-align: center;">

# instalação do Banco de Dados Oracle 19c com interface Gráfica. Oracle Linux 7.9
</p>


<p style="text-align: center;">

## Preparação do ambiente
</p>

</div>

<div style="text-align: center;">

**Antes de qualquer coisa precisamos executar um update e upgrade no S.O**
</div>

<div style="text-align: center;">

```      yum -y update       ```

```      yum -y upgrade      ```
</div>

<p style="text-align: center;">
Agora com tudo atualizado precisamos pegar o IP e HOSTNAME do S.O
</p>

<div style="text-align: center;">

``         ip a           ``

``         hostname       ``
</div>
<div style="text-align: center;">

![IP Hostname](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/ip_hostname.jpg)

</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<div style="text-align: center;">
<p style="text-align: center;">

### Com essas informações, vamos atualizar o arquivo */etc/hosts* no S.O para adicionar uma nova entrade de mapeamento de IP ###
</p>

</div>
<div style="text-align: center;">

*visualizar o arquivo*

``      cat /etc/hosts      ``   
\
*Copiar o IP e HOSTNAME para dentro do arquivo*

``   echo "192.168.3.15  oradb03  oradb03.localdomain" >>/etc/hosts   ``

![Configuração do arquivo hosts](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/echo_hosts.jpg)


</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<div style="text-align: center;">
<p style="text-align: center;">

## Próximo passo: vamos desabilitar o *SELinux* para que o sistema não aplique nenhuma política de segurança, o que poderia interferir na instalação do banco de dados. ##
</p>
</div>
<div style="text-align: center;">

``    vi /etc/selinux/config     ``
</div>

<div style="text-align: center;">

*colocar o SELINUX como disabled*

![Configuração do SELinux antes](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/selinux_antes.jpg)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


![Configuração do SELinux depois](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/selinux_depois.jpg)



</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


<div style="text-align: center;">
 <p style="text-align: center;">

   ## Próximo passo: vamos parar e desabilitar o *firewalld* para garantir que o firewall não interfira na instalação do banco de dados. 

*Observação: não se esqueça de reabilitar e configurar o firewall após a conclusão da instalação do banco de dados. Certifique-se de abrir as portas necessárias para o funcionamento do banco de dados e ajustar as regras de segurança conforme necessário para proteger o sistema.*
</p>
</div>

<div style="text-align: center;">

``     systemctl stop firewalld      ``

``     systemctl disable firewalld   ``

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

![Desabilitar firewall](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/desabilitar_firewall.jpg)

</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<div style="text-align: center;">
 <p style="text-align: center;">

## Próximo passo: vamos instalar o pacote *oracle-database-preinstall-19c* para preparar o ambiente para a instalação do Oracle Database 19c. ##

*Este pacote configura automaticamente o sistema com os ajustes necessários, incluindo a criação de usuários e grupos específicos, a instalação de dependências essenciais e a configuração de parâmetros do sistema recomendados pela Oracle. Isso garante que o ambiente esteja pronto e otimizado para a instalação e operação do banco de dados.*

*Vamos verificar a compatibilidade do nosso S.O com o Banco de dados. No meu caso é compatível com o banco que vou instalar*
</p>
</div>

<div style="text-align: center;">

  ``     yum search preinstall     ``

  ![Verificar pré-instalação](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/verificar_preinstall.jpg)

</div>


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;



<div style="text-align: center;">

*Agora vamos instalar o pacote*

``     yum -y install oracle-database-preinstall-19c     ``

</div>

## Após a instalação do pacote, vamos mudar a senha do usuário Oracle, criar diretórios e ajustar permissões ##

<div style="text-align: center;">

``     passwd oracle    ``

![Mudar senha Oracle](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/mudar_senha_oracle.jpg)

</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


## Próximo passo: vamos configurar o diretório para a instalação do Oracle Database. ##

1. **Criar o Diretório Necessário e arrumando permissões:**
   ```bash
   mkdir -p /u01/app/oracle/product/19c/db_1
   chmod -R 775 /u01/
   chown -R oracle:oinstall /u01/

   ````


<div style="text-align: center;">

![Criação do diretório](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/criacao_diretorio.jpg)

*Essas etapas preparam o diretório com as permissões e propriedade corretas para a instalação do Oracle Database, garantindo que o usuário Oracle tenha controle adequado sobre o diretório de instalação.*

</div>

**Precisa ficar assim:**
````bash
Permissões      Dono    Grupo
drwxrwxr-x.   3 oracle oinstall   17 Aug 21 14:42 u01

````

## Nesse passo você precisa ter o arquivo do banco na seu S.O ##

*link para download, baixe o arquivo no formato zip*

<https://www.oracle.com/br/database/technologies/oracle19c-linux-downloads.html>

**Vou mostrar passo a passo como copiar o arquivo do banco no S.O pelo mobaxterm. Eu copiei o arquivo no diretório que a gente acabou de criar */u01/app/oracle/product/19c/db_1***

<div style="text-align: center;">

![Passo 1](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/passo1.jpg)

![Passo 2](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/passo2.jpg)

![Passo 3](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/passo3.jpg)

![Passo 4](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/passo4.jpg)

![Passo 5](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/passo5.jpg)

</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


## Próximo passo vamos entrar com o usuário Oracle ##

<div style="text-align: center;">

``      su - oracle      `` 


![Usuário Oracle](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/usuario_oracle.jpg)


</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

## Vamos entrar no diretório HOME onde a gente passou o arquivo ZIP do banco de dados ##

````bash
                                   cd /u01/app/oracle/product/19c/db_1/

````
<div style="text-align: center;">

![Entrar no diretório home](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/entrar_home.jpg)

*Vamos descompactar o arquivo ZIP*
</div>
<div style="text-align: center;">

````             unzip -q DATABASE_LINUX.X64_193000_db_home-001.zip                      ````

</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<div style="text-align: center;">

![Descompactar](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/descompactar.jpg)

</div>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

## Após a descompactação precisamos declarar as variáveis de ambiente ##

````bash
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=/u01/app/oracle/product/19c/db_1
export PATH=$ORACLE_HOME/bin:$PATH

````

![Variáveis de Ambiente](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/variaveis_de_ambiente.jpg)

## Agora podemos iniciar a instalação do SGBD(binários) do Oracle Database ##

<div style="text-align: center;">

``         ./runInstaller           ``

![Instalação SGBD](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/instacao_sgbd.jpg)

</div>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


# Passo a passo da instalação via interface gráfica #

<div style="text-align: center;">

*Vamos marcar essa opção, pois vamos apenas instalar o SGBD*

![Instalação 1](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install1.jpg)

* Nosso ambiente está configurado para Single Instance, ou seja, apenas uma instância*

![Instalação 2](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install2.jpg)

*Como aqui é laboratório de estudos podemos instalar a versão Enterprise Edition e aproveitar os resursos dessa versão. Em ambiente de produção precisamos pagar uma lincença.*

![Instalação 3](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install3.jpg)

*Como as variáveis de ambiente foram declaradas a instalação já recolhece o ORACLE_BASE
e o ORACLE_HOME*

![Instalação 4](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install4.jpg)


*Aqui a instalação já cria o caminho para o repositório oraInventory e já coloca o grupo oinstall, ou seja, nessa janela apenas clique em next*

![Instalação 5](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install5.jpg)

*Aqui são os grupos é opcional mexer em cada grupo, eu deixei padrão*

![Instalação 6](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install6.jpg)

*Aqui é opcional marcar essa "caixinha" com ela marcada você precisa colocar a senha do root, pois a instalação vai rodar sozinha os scripts de root, caso não marque vai precisar rodar com o root no S.O quando a instalçao pedi, para assim continuar a instalação, no meu apenas vai pedir para confirmar.*

![Instalação 7](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install7.jpg)

<div style="text-align: center;">
<p style="text-align: center;">

*Verificando os pré-requisitos, se houver algum problema, ele será exibido. Se você seguiu todos os passos que mostrei, o máximo que aparecerá é uma mensagem informando que a sua swap está abaixo do esperado. Nesse caso, você pode ignorar e continuar.*

</p>
</div>

![Instalação 8](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install8.jpg)


*Tudo certo com os prè-requisitos*

![Instalação 9](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install9.jpg)


*Iniciando a instalação*
![Instalação 10](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install10.jpg)


*Como eu coloquei na instalação para executar os scripts de root na instalação vai aparecer pedindo para confirmar*

![Instalação 11](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install11.jpg)

*SGBD instalado com sucesso*


![Instalação 12](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/install12.jpg)


</div>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


# Após a instalação do SGDB, vamos instalar o listener #


<div style="text-align: center;">

``      netca     ``

![Listener Image](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner.jpg)

*O NETCA (Network Configuration Assistant) é uma ferramenta da Oracle usada para configurar e gerenciar a comunicação de rede entre o banco de dados Oracle e os clientes. Ele facilita a criação, modificação e verificação dos arquivos de configuração de rede, como o listener.ora, tnsnames.ora, e sqlnet.ora.*



\
![Listener 1](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner1.jpg)

![Listener 2](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner2.jpg)

*Aqui podemos colocar qualquer nome, como é laboratório recomendo deixar assim. Na empresa pode seguir o padrão dela*

![Listener 3](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner3.jpg)

![Listener 4](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner4.jpg)

*Aqui é a porta do listener, em laboratório usamos sempre a padrão. Em ambientes produtivos segui o padrão da empresa*
![Listener 5](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner5.jpg)

![Listener 6](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner6.jpg)


![Listener 7](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner7.jpg)


![Listener 8](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/listerner8.jpg)

</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

## Agora vamos finalmente fazer a instalação do Oracle Database 19c ##

<div style="text-align: center;">

``           dbca        ``

![DBCA](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/dbca.jpg)

\

*O DBCA (Database Configuration Assistant) é uma ferramenta gráfica e de linha de comando fornecida pelo Oracle para facilitar a criação, configuração e gerenciamento de bancos de dados Oracle.*
\
# Passo a passo da instalação #
\
Criar um banco de dados


</div>

<div style="text-align: center;">

![Banco 1](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco1.jpg)

\
Vamos em configurações avançadas, pois queremos configurar tudo passo a passo.

![Banco](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco.jpg)

\
Nosso labaratório é uma Single Instance (Um instancia e um Banco de Dados) e modelo de banco de dados é General Purpose or Transaction Processing (Propósito geral ou Processamento de transações)

![Banco 3](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco3.jpg)

\
Colocamos o SID e nome do nosso Banco de dados. Estamos criando um Banco com arquitetura Multitenant, com apenas um pdb. Você pode criar sem essa arquitetura, bastar desmarcar ***'Create as Container database'***

![Banco 4](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco4.jpg)

\
Vamos usar o método File System. No meu caso, deixei o local dos arquivos do banco de dados como padrão, mas você pode escolher outro local. Em ambientes produtivos, é importante definir locais estratégicos, como, por exemplo, em um disco dedicado exclusivamente a esses arquivos.

Deixe o OMF habilitado, pois pois o Oracle cria e gerencia automaticamente os arquivos.

![Banco 5](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco5.jpg)

\
Vamos habilitar nossa FRA. A FRA (Flash Recovery Area) é um espaço reservado no disco para armazenar backups, arquivos de recuperação e logs de arquivamento do Oracle, facilitando a recuperação e a gestão do banco de dados.

Deixei padrão o tamanho da FRA, como é ambiente de teste, mas em ambiente de produção, precisa ser bem analisado o tamanho dela, pois uma FRA sem espaço fará com que todo o banco de produção pare. Podemos posteriormente aumentá-la conforme nossa necessidade.

Vocês podem ver que deixei o check box do Enable archiving desmarcado. Em ambiente de teste, podemos fazer isso para não gerar archivelog e consumir espaço, porém, em ambiente de produção, é um erro grave criar um banco de dados sem habilitar o archivelog, que permite a recuperação completa dos dados em caso de falha. Podemos habilitá-lo depois, mas o banco precisará ficar indisponível, e não é isso que queremos em ambiente de produção

![Banco 6](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco6.jpg)

\
Como criamos o listener, a instalação já recolheceu o listener e mostra que ele está 'UP"

![Banco 7](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco7.jpg)

\
Nesta etapa da instalação, você pode ver as opções para habilitar o Oracle Database Vault e o Oracle Label Security. Essas ferramentas são recursos avançados de segurança que a Oracle oferece, e ambas exigem licenças adicionais.

![Banco 8](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco8.jpg)

\
Aqui é onde definimos o tamanho e a gestão da nossa SGA (System Global Area) e PGA (Program Global Area). Neste exemplo, marquei a opção "Use Automatic Shared Memory Management" (ASMM) e deixei o tamanho padrão sugerido pelo Oracle. Esta configuração pode ser ajustada posteriormente, se necessário.

Não vou entrar em detalhes sobre a gestão da SGA e PGA, pois é um tópico que exige uma explicação mais aprofundada e não é o foco deste artigo. As setas na imagem destacam as abas disponíveis. No ambiente de laboratório, não há necessidade de mexer nessas configurações, pois já estão predefinidas para atender às nossas necessidades. No entanto, em um ambiente de produção, essas abas devem ser configuradas de acordo com os requisitos específicos de cada cenário.

![Banco 9.0](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco9.jpg)

\
Ao continuar com as configurações da SGA e PGA, poderá aparecer um aviso indicando que o tamanho especificado para o "shared pool" não atende aos requisitos mínimos recomendados. Este aviso, mostrado na imagem, surge com o código de erro [DBT-11205] e alerta que isso pode causar falhas na criação da base de dados.

Neste ponto, tens a opção de ajustar o tamanho do shared pool para cumprir os requisitos mínimos ou prosseguir com a configuração atual. No meu caso, optei por seguir em frente clicando em "Yes" para continuar. Se estiveres num ambiente de produção, é recomendável revisar as configurações para garantir que todas as alocações de memória atendam aos padrões mínimos, evitando possíveis problemas mais tarde.

![Banco 9.1](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco9.1.jpg)

\
Nessa janela é o Enterprise Manager, eu não vou usar, então não marquei nada.
![Banco 10](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco10.jpg)

\
Nesta janela, destaquei os avisos de segurança do Oracle. Em ambiente de laboratório, utilizamos frequentemente a mesma senha para os utilizadores SYS, SYSTEM e PDBADMIN (se optarmos pela arquitetura Multitenant). No entanto, em ambientes de produção, é essencial atribuir uma senha distinta para cada um desses utilizadores, seguindo o padrão de segurança estabelecido por cada empresa.

![Banco 11](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco11.jpg)

\
Nesta etapa, temos a opção de salvar um template e gerar scripts desta instalação para uso posterior. Estas opções são úteis para automatizar a criação de bases de dados em futuras implementações. No entanto, neste exemplo, selecionei apenas a opção "Create database" para proceder com a criação imediata de uma nova base de dados. 

![Banco 12](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco12.jpg)

\
Nosso sumário de instalação, podemos ver tudo que configuramos.

![Banco 13](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco13.jpg)

\
Agora, durante o processo de instalação, podemos acompanhar o progresso clicando em "Details" para ver informações detalhadas. Alternativamente, é possível monitorizar o log de instalação no caminho sugerido pelo Oracle, o que permite uma visão mais profunda de cada etapa.




![Banco 14](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco14.jpg)

\
Como podemos ver, o Oracle 19c foi instalado com sucesso. Agora, a base de dados está pronta para ser utilizada, e podemos proceder com as configurações adicionais conforme necessário.

![banco 15](https://raw.githubusercontent.com/CrisSantosDB/imagens-de-projeto/main/banco15.jpg)
