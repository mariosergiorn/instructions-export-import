# Vivendo e aprendendo: Import/Export MySQL Workbench 8.0 

* Mário Sergio Ribeiro Nunes ([mariosergio3108@gmail.com](mailto:mariosergio3108@gmail.com))

Este documento foi criado para auxiliar iniciantes em banco de dados que utilizam MySQL Workbench como ferramenta visual, integrado para o sistema de banco de dados MySQL. 

## Exportando o banco do servidor remoto para o local

Para realizar a exportação do banco para a sua máquina local, você deverá acessar na barra de navegação a aba 'Administration' e clicar na opção 'Data Export'.



![Export](/img/Export.png)



Ao clicar nesta opção, será aberta uma nova aba 'Administration - Data Export', nela você irá selecionar o banco desejado, e as tabelas que serão exportadas.

Caso você queira exportar todo o banco de dados em um único arquivo, basta selecionar a opção ‘Export to Self-Contained File’ nas opções de exportação (Export Options) e selecionar o caminho para despejo do arquivo.
Se o interesse da exportação seja apenas algumas tabelas, o ideal é que seja realizada pela opção ‘Export to Dump Project Folder’

### A opção ‘Dump Structure and Data’ é importante para salvar a estrutura da tabela e as linhas de dados nela



![Export2](/img/Export2.png)



Com tudo pronto, podemos iniciar a Exportação em 'Start Export'.

Se no fim da exportação, apresentar o seguinte erro: “Unknown table 'column-statistics' in information_schema (1109)”, você deverá alterar uma configuração dentro do arquivo my.ini.

O seguinte comando deverá ser inserido dentro deste arquivo.
Encontre os colchetes [mysqldump] e insira o comando, ‘column-statistics=0’.

Caso esteja utilizando o Xampp v3.2.4, você poderá alterar o arquivo my.ini, localizado no caminho C:\xampp\mysql\bin\my.ini, porém, para que o Workbench possa identificar este arquivo, o caminho do mesmo deverá ser definido em Edit > Preference > Administration > Path to mysqldump tool.


## Importando os dados e a estrutura para dentro do seu banco

Para a realização da importação do banco de dados, você deverá acessar na barra de navegação a aba 'Administration' e clicar na opção 'Data Import/Restore'.

Ao acessar a 'Data Import' são exibidas opções de importação, caso você queira importar os dados de apenas um único arquivo (que foi exportado anteriormente), você deverá selecionar a opção 'Import from Self-Contained File' e definir o caminho onde o arquivo .sql foi criado.

Após definido o caminho deste arquivo, você deverá criar um novo Schema, clicando na opção 'New...'. Lembre-se de definir a opção 'Dump Structure and Data' antes de iniciar a importação.

Feito isto, pode iniciar a importação clicando no botão 'Start Import'.



![Import](/img/Import.png)



Se no fim da sua importação o Workbench retornar o seguinte erro:

“ERROR 2006 (HY000) at line 27652: MySQL server has gone away”

Não fique assustado! Seu banco não foi excluído.

Isso se dar por conta da variável ‘max_allowed_packet’ definido no arquivo my.ini, que por default, encontra-se definido com 1024M (se não me engano). 

Portanto, essa variável deverá ser definida com o tamanho maior que o banco que será importado.

[mysqldump]

max_allowed_packet=5120M // Definindo o tamanho máximo de 5Gb

### Para saber o tamanho do banco, basta acessar o local aonde ele foi exportado.

# Mais do mesmo: Erros!



![Erro](/img/Erro.png)



Ao iniciar a importação e o Workbench apresentar a mensagem acima, sabia que, provavelmente nem todas as tabelas do banco serão importadas, devido a versão do Workbench não ser ‘compatível’ com o servidor remoto.

O método utilizado para resolução desta problemática, é feito da seguinte forma:

1. Download da versão anterior do mysqldump. (https://dev.mysql.com/downloads/mysql/5.7.html#downloads)
2. Extração dos arquivos contidos na pasta ZIP.
3. Localizar arquivo mysqldump.exe nos arquivos extraídos. (Possivelmente será encontrado dentro da pasta bin).
4. Copiar o mesmo para dentro da pasta do Workbench. (C:\Program Files\MySQL\MySQL Workbench 8.0 CE).

Para que o arquivo de destino não seja substituído, realizamos uma pequena alteração no nome do arquivo, conforme imagens abaixo.

Se substituir, não necessita fazer mais nenhum passo que irá funcionar. 
Mas se precisar se ligar a servidores com a versão 8, não seria prático fazer esta substituição, pois, teríamos que substituir sempre que nos ligássemos a servidores diferentes. No meu caso, optei por colocar com um nome diferente.



![mysqldump](/img/mysqldump.png)



Após inserido o arquivo dentro do diretório, torna-se necessário definir o caminho do mesmo dentro do Workbench. 

**Edit > Preference > Administration > Path to mysqldump Tool**

Feito isto, o arquivo será importado com êxito!

> A instrução acima, também soluciona a mesma problemática do parâmetro “column-statistics”. Pois, o arquivo mysqldump.exe versão 8 utiliza o parâmetro “column- statistics” ativo por default, que é uma funcionalidade não presente nas versões anteriores do MySQL.

