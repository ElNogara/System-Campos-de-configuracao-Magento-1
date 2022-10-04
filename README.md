<h1>Campo de configuração de módulo para Magento 1</h1>
Nesse repositório estarei ensinando como criar um campo no admin da sua loja Magento 1 e como acessar seus valores salvos.<br>

<h2>Primeiro vamos entender a estrutura de um campo no Magento 1?</h2>
Todo campo criado no admin do Magento 1 deve seguir uma estrutura fixa para que sejá possível localiza-lo, primeiro a <strong>TAB</strong>, que é onde será inserida a sua <strong>SECTION</strong>, dentro da section vamos possuir <strong>GROUPS</strong> com todos os seus <strong>FIELDS</strong> dentro.

Módulo que será desenvolvido terá:
</br>
<strong>CODEPOOL = local
</br>
NAMESPACE = Elnogara
</br>
MODULENAME = Sistemaconfig</strong>
</br>
Caso não saiba nada de como iniciar um módulo, declarar seu codepool, namespace, modulename... Deixei um tutorial bem fácil <a href="https://github.com/ElNogara/Primeiro-Modulo-Magento-1">Aqui</a> </br>

Todo módulo Magento 1 que tem campos de configuração no admin da loja, deve obrigatoriamente conter um arquivo chamado <strong>system.xml</strong> dentro da pasta <strong>etc</strong> da sua raiz.
Esse arquivo é responsável por receber todos os campos que devem ser criados no administrador, segue abaixo um exemplo bem explicado de como utilizar seus nós XML:
```
<?xml version="1.0" encoding="UTF-8"?>
<config> <!--Deve ser declarado.-->
    <tabs> <!--Será definido uma nova tab, não é obrigatório conter apenas se deseja criar uma nova tab no seus campos de administrador.-->
        <nogaratab> <!--Deve dar um nome exclusivo para a sua tab, qualquer outro módulo que for utilizar a sua tab vai usar esse nome como referência para colocar as sections dentro.-->
            <label>Campos Nogara TAB</label> <!--Esse label é o texto/título da sua tab.-->
            <sort_order>1</sort_order> <!--Determina a ordenação da sua tab entre as outras.-->
        </nogaratab>
    </tabs>
    <sections> <!--Aqui estamos criando a nossa section.-->
        <nogarasection> <!--Como de costume também será necessário inserir um nome/rótulo para a sua section, porquê será necessário atribuir os campos a essa sections utilizando esse identificador.-->
            <label>Titulo da Section</label> <!--Esse label é o texto/título da sua section.-->
            <tab>nogaratab</tab> <!--Aqui é muito importante, pois é a parte aonde você vincula a sua section a uma tab... Estou vinculando a minha com a minha tab criada logo acima, MAS você poderia estar criando um campo que seria visível em alguma tab já existente do Magento, onde seria apenas necessário você pegar o nome dessa tab e inserir aqui, sua section já será exibida nela.-->
            <!--Os campos abaixo determinam em quais níveis de permissão seus campos serão exibidos para configurar. (Caso não saiba o que isso significa, pode deixar tudo como 1 que vai funcionar perfeitamente.)-->
            <show_in_default>1</show_in_default> <!--Valores de 1 ou 0 para SIM e NAO. Define se esses novos campos serão exibidos na visão Default de configuração.-->
            <show_in_website>1</show_in_website> <!--Define se esses novos campos serão exibidos nas visões de website de configuração.-->
            <show_in_store>1</show_in_store> <!--Define se esses campos serão exibidos para o nível de Store.-->
            <groups> <!--Dentro da section é necessário definir grupos para estar alocando seus fields, que são seus campos.-->
                <nogaragroup> <!--Também devem estar todos com um nome exclusivo.-->
                    <label>Título do Grupo</label> <!--Esse label é o texto/título do seu grupo-->
                    <show_in_default>1</show_in_default>
                    <show_in_website>1</show_in_website> 
                    <show_in_store>1</show_in_store> 
                    <fields> <!--E aqui começa a criar os campos-->
                        <nogaracampo1> <!--Cada campo terá seu nome para ser identificado pelo Magento-->
                            <label>Titulo do campo 1</label> <!--Esse label é o texto/título do seu field.-->
                            <frontend_type>text</frontend_type> <!--Essa configuração é muito importante para o seu campo, é aqui onde se determina se ele será um campo do tipo text, textarea, select, multiselect, password, time, image, allowspecific, import, export, label, obscure, entre outros. Nesse momento vamos criar um do tipo texto-->
                            <comment>Campo para exibir titulo na home.</comment> <!--São comentarios para especificar o que esse field faz.-->
                            <!--Caso você precise deixar o campo com um comentário mais elaborado, com um HTML diferente, basta inserir ele dentro de um CDATA, fica da seguinte forma... ![CDATA[TEXTO DO CAMPO COM HTML]]-->
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website> 
                            <show_in_store>1</show_in_store> 
                        </nogaracampo1>
                        <nogaracampo2> <!--Criando meu segundo campo-->
                            <label>Titulo do campo 2</label> <!--Esse label é o texto/título do seu field.-->
                            <frontend_type>select</frontend_type> <!--VOu criar um SELECT para explicar como funciona ele-->
                            <frontend_model>1<frontend_model> <!--Todo campo tipo SELECT ou MULTISELECT deve conter um frontend_model, que basicamente é uma classe que vai retornar para ele um array de opções, que serão as opções sugeridas para selecionar.-->
                        </nogaracampo2>
                    </fields>
                </nogaragroup>
            </groups>
        </nogarasection>
    </sections>
</config>
```

Se criado corretamente, ao acessar suas configurações no admin, através do caminho, Sistema -> Configurações, você já deve visualizar seus novos fields.

<img style="width: 500px;" src="https://user-images.githubusercontent.com/50090354/193133061-0f186503-04d0-48e9-9289-8d6fe2605c13.png" alt="Minha Figura">

<h2>Como acessar esses campos por código?</h2>
Caso você precise pegar esses valores inseridos pelos usuários através do código, para fazer alguma tratativa nos seus módulos ou condições, é bem simples... Basta ter em mente o nome da sua <strong>SECTION</strong>, <strong>GROUPS</strong> e <strong>FIELDS</strong> do seu campo, pois como disse anteriormente, esses nomes ÚNICOS são basicamente a 'rota' que o Magento usa para identificar esse campo, então utilize o código:

```
Mage::getStoreConfig('<SECTION>/<GROUP>/<FIELD>'); //Dessa forma é possível acessar qualquer valor de campos no Magento 1, basta passar seus respectivos nomes de seções, grupos e campos, por isso devem ser nomes únicos na hora de declarar.

Mage::getStoreConfig('nogarasection/nogaragroup/nogaracampo1') //Exemplo de como pegar os valores do primeiro campo que criamos.
```

Esses campos criados no admin servem apenas para usuários que utilizem uma conta administrador no sistema, caso exista a necessidade de que esses campos sejam vísiveis para permissões diferentes de usuários dentro do painel, é necessário criar uma rota de ACL para eles, saiba como fazer isso através desse <a href="https://github.com/ElNogara/ACL-System-Campos-Magento-1">repositório</a>.

<h2>Erro 404</h2>
Existem duas formas de resolver isso:

1° - Por motivos de segurança no Magento 1, sempre que é criado uma nova section ele exige que sejá feito o login novamente para ter permissão de acessar ela, então basta deslogar e logar novamente que vai conseguir visualizar seu campo.
2° - Caso mesmo logando novamente não consiga será necessário criar o ACL da sua seção, aprenda isso nesse <a href="https://github.com/ElNogara/ACL-System-Campos-Magento-1">repositório</a>.

Qualquer dúvida estou a disposição.
