[![made Language {generic badge}](https://img.shields.io/badge/Made%20with-Mardown-8A2BE2)](https://github.com/alanmugiwara)
[![create date](https://badges.pufler.dev/created/alanmugiwara/moodle-deploy?color=8A2BE2)](https://github.com/alanmugiwara)
[![last update date](https://badges.pufler.dev/Updated/alanmugiwara/moodle-deploy?color=8A2BE2)](https://github.com/alanmugiwara)
[![Commits Badge](https://img.shields.io/github/commit-activity/m/alanmugiwara/moodle-deploy?color=8A2BE2)](https://github.com/alanmugiwara)

[![contributors](https://img.shields.io/github/contributors/alanmugiwara/moodle-deploy?color=8A2BE2)](https://github.com/alanmugiwara)
[![issues counter](https://img.shields.io/github/issues/alanmugiwara/moodle-deploy?color=8A2BE2)](https://github.com/alanmugiwara)
[![repo size](https://img.shields.io/github/repo-size/alanmugiwara/moodle-deploy?color=8A2BE2)](https://github.com/alanmugiwara)
[![directory files](https://img.shields.io/github/directory-file-count/alanmugiwara/moodle-deploy?color=8A2BE2)](https://github.com/alanmugiwara)

## Deploy documentado da plataforma de aprendizado OpenSource Moodle
<p align="center"><a href="https://moodle.org" target="_blank" title="Moodle Website">
  <img src="https://raw.githubusercontent.com/moodle/moodle/main/.github/moodlelogo.svg" alt="The Moodle Logo">
</a></p>

### **Visão Geral**
-  **Aplicação:** Moodle
- **Versão do Moodle:** 4.5.3
- **Ambiente:** Desenvolvimento
- **Propósito do Ambiente:** Disponibilizar cursos para uma instituição
- **Sistema Operacional:** Windows 11 24H2 Build 26100.2894 (x64)
- **URL de Acesso:** localhost (127.0.0.1)
- **Responsáveis:** Álan Cruz
- **Data da Última Atualização:** 24.03.2025
###  **Arquitetura do Ambiente**

-  **Visão Geral:** Máquina local rodando *Apache* como servidor web, *PHP* para carregar as páginas web e *MariaDB/MySQL* como banco de dados (com pelo menos 1 usuário comum e o root).

- **Componentes:**
    - **Servidor Web:** Apache 2.4.63 
    - Localização C:\Apache24
    - **PHP:** 8.2.28
    - Localização: C:\php
    - Extensões Ativadas: key *(via php.ini):* curl, ftp, fileinfo, gd, intl, mbstring, exif, mysqli, openssl, pdo_mysql, pdo_oci, soap, sodium, zip, o "max_input_vars" deve ser descomentando e receber valor "= 5000".
     - **Banco de Dados:** MariaDB 11.5
	- **Instalação:** manual - PHP, Apache, MariaDB
    - **Código Moodle:**
        - Localização: C:\Users\x132248\moodle-4.5.3
    - **Diretório de Dados Moodle:** 
	    - Localização: C:\Users\x132248\moodle-4.5.3\moodledata
#### **3. Pré-requisitos**

- **Software:**
    - Binários do Apache 2.4 para Windows
    - Binários do PHP para Windows 
    - Servidor MariaDB instalado e rodando
    - Download do pacote Moodle 4.5.3 (via repositório remoto)
    - Editor de Texto/Código
    
- **Acessos:**
    - Permissões de Administrador no Windows (para instalar serviço Apache, editar arquivos em C:\, modificar Variáveis de Ambiente).
    - Acesso ao cliente do banco de dados (HeidiSQL, DBeaver, phpMyAdmin, linha de comando mysql) para criar banco e usuário.
    - Credenciais do usuário Moodle no banco de dados.
#### **4. Passos detalhados do Deploy**

- **a. Download e Configuração do Moodle:**
1. Baixar o código do moodle 4.5.3 através do repositório oficial *([Moodle release 4.5.3](https://github.com/moodle/moodle/releases/tag/v4.5.3))*
2. Extrair o conteúdo para um diretório raiz.
    - Exemplo: Extrair para *C:\Users\bebetoburgues\moodle-4.5.3*.
3. Editar o arquivo *moodle-4.5.3/config.php* e por o seguinte no trecho *DATABASE SETUP*. Substituir os trechos "xxx" pelos dados correspondentes ao banco de dados utilizado.
   
``` bash
$CFG->dbtype    = 'mariadb';
$CFG->dblibrary = 'native';
$CFG->dbhost    = '127.0.0.1';  
$CFG->dbname    = 'xxx';    
$CFG->dbuser    = 'xxx'; 
$CFG->dbpass    = 'xxx';  
$CFG->prefix    = 'mdl_'; 
$CFG->dboptions = array(
    'dbpersist' => false,
    'dbsocket'  => false,
    'dbport'    => '3306', // Use o valor de DB_PORT
    'dbhandlesoptions' => false,
    'dbcollation' => 'utf8mb4_unicode_ci', // ou a collation que você está usando
```

4. Ainda no arquivo *moodle-4.5.3/config.php* no trecho *DATA FILES LOCATION*, na linha *$CFG->dataroot  =* o valor deve ficar como abaixo ou deve-se definir qualquer outro diretório. O moodle-data armazena arquivos enviados pelos usuários, cache e sessões, backups de cursos arquivos temporários, dados privados.

``` bash
$CFG->dataroot  = 'C:\Users\bebetoburgues\moodle-data';
```

- **b. Download e Configuração do PHP:**
3. Baixar o PHP 8.2.28 para Windows em *([PHP For Windows: Binaries and sources Releases](https://windows.php.net/download/))*
4. Extrair em C:\php.
5. Adicionar C:\php às Variáveis de Ambiente do Sistema (PATH).
        - Como: Painel de Controle -> Sistema -> Configurações avançadas do sistema -> Variáveis de Ambiente -> Editar variável Path em "Variáveis do sistema".
6. Navegar até C:\php.
7. Renomear o arquivo *php.ini-development* para *php.ini*.
        
8. Editar o *C:\php\php.ini:*
        - Localizar e descomentar removendo *(;)* das linhas: *extension=curl, ftp, fileinfo, gd, intl, mbstring, exif, mysqli, openssl, pdo_mysql, pdo_oci, soap, sodium, zip*
        - Localizar e definir o caminho para as extensões como: *extension_dir = "C:/php/ext"*.
        - Localizar e alterar *max_input_vars = 1000* para *max_input_vars = 5000.*
        - Verificar/Ajustar outros parâmetros úteis para o Moodle: memory_limit, post_max_size, upload_max_filesize.
- **c. Download e Configuração do Apache:**
3. Baixar o Apache 2.4 para Windows. em *[Apache 2.4.63-250207 Releases](https://www.apachelounge.com/download/)*
4. Extrair em C:\Apache24.
5. Editar o arquivo *C:\Apache24\conf\httpd.conf* e abaixo da linha *Define SRVROOT "c:/Apache24"", colar o seguinte

``` bash
LoadModule php_module "C:/php/php8apache2_4.dll"
```
4. Ainda no mesmo arquivo, abaixo de *"symbolic links and aliases may be used to point to other location"*, substituir o trecho abaixo:
``` bash
DocumentRoot "${SRVROOT}/htdocs"
<Directory "${SRVROOT}/htdocs">
```
5. Substituir o trecho acima pelo seguinte mais abaixo: *C:/Users/bebetoburgues/moodle-4.5.3* é o *Diretório externo do projeto*. por padrão o apache reconhece apenas o diretório *"htdocs"* como local de execução.
``` bash
DocumentRoot "C:/Users/bebetoburgues/moodle-4.5.3"
<Directory "C:/Users/bebetoburgues/moodle-4.5.3">
    Options Indexes FollowSymLinks ExecCGI
    AllowOverride None
    Require all granted
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
<FilesMatch \.php$>
    php_value extension_dir "C:/php/ext"
</FilesMatch>
</Directory>
```

6. Criar um arquivo *php.conf* dentro do diretório *"C:\Apache24\conf\extra"* e colar a seguinte configuração.

``` bash
<FilesMatch \.php$>
   SetHandler application/x-httpd-php
</FilesMatch>
PHPIniDir "C:/php"
```

5. Abaixo do trecho *symbolic links and aliases may be used to point to other locations.* colar a configuração abaixo para definir o diretório do moodle

``` bash
DocumentRoot "C:/Users/bebetoburgues/moodle-4.5.3"

<Directory "C:/Users/bebetoburgues/moodle-4.5.3">
    Options Indexes FollowSymLinks ExecCGI
    AllowOverride None
    Require all granted
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
<FilesMatch \.php$>
    php_value extension_dir "C:/php/ext"
</FilesMatch>
</Directory>
```

6. No final deste mesmo arquivo *php.conf* colar o trecho abaixo para que o apache reconheça o php e permita que o php carregue sua as extensões, algumas exigidas pelo guia de instalação do moodle.

``` bash
Include conf/extra/php.conf
```

7. Por fim executar o seguinte comando para registrar o Apache como um serviço do Windows 
``` bash
C:\Apache24\bin\httpd -k install
```
8. É necessário abrir o menu executar digitando *win+x*, digitar *services.msc*, e no gestor de serviços procurar pelo serviço do apache, iniciá-lo e defini-lo com o tipo de inicialização automático.
#### **Instalação do ambiente Moodle**

- **a. Instalação do moodle via localhost:**

1. Após toda a configuração do Apache e do PHP basta acessar o endereço http://localhost/install.php e seguir o guia de instalação do ambiente moodle para finalizar a implementação.

Contato
-------

Para dúvidas, sugestões ou problemas, entre em contato com Álan Cruz:

<a href="https://instagram.com/alancruz_tec" target="_blank"><img loading="lazy" src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" alt="Instagram"></a>
<a href="mailto:contato@alancruz.tec.br"><img loading="lazy" src="https://img.shields.io/badge/E--Mail-D14836?style=for-the-badge&logo=gmail&logoColor=white" alt="E-mail"></a>
<a href="https://linkedin.com/in/alansilvadacruz" target="_blank"><img loading="lazy" src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="Linkedin"></a>
<a href="https://alancruz.tec.br" target="_blank"><img loading="lazy" src="https://img.shields.io/badge/-My%20Website-%230077B5?style=for-the-badge&logo=wordpress&logoColor=white" alt="Website"></a>
