# Menu Principal

Agora que já temos a estrutura básica do nosso projeto, podemos iniciar a customização do menu principal e a criação das páginas que irão preencher e atualizar o content.

Primerio criaremos **três **novas páginas, **alunos.xhtml**, **disciplinas.xhtml** e **matriculas.xhtml**.

Essas páginas irão preencher o conteúdo do facelet Content de acordo com a opção selecionada no menu.

Portanto, elas precisam ser Clientes do Modelo de Facelets que foi definido anteriormente.

![](/assets/contentscustom.png)

Depois de criar essas três páginas, iremos utilizara outro elemento do **PrimeFaces**, o **menu**.

Lembre que para toda página que contiver elementos do PrimeFaces é preciso declarar sua utilização no cabeçalho.

Observe que dentro da tag **menu** temos um **submenu** e é nele que estarão os itens do menu para a navegação entre facelets. 

Quem controla a transição entre facelets é o do atributo **outcome**, é nele que deve ser declarado qual facelet deverá ser exibido quando sua opção for selecionada**.** 

```xhtml
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
      xmlns:h="http://xmlns.jcp.org/jsf/html"
      xmlns:p="http://primefaces.org/ui"
      xmlns:f="http://xmlns.jcp.org/jsf/core">

    <body>
        <h:form>            
            <br/>
            <p:menu>
                <p:submenu label="Menu" >
                    <p:menuitem id="bt1" value=" Alunos" outcome="alunos.xhtml" icon="ui-icon-script" ajax="false"/>
                    <p:menuitem id="bt2" value=" Disciplinas" outcome="disciplinas.xhtml"  icon="ui-icon-script" ajax="false"/>
                    <p:menuitem id="bt3" value=" Matrículas" outcome="matriculas.xhtml"  icon="ui-icon-script" ajax="false"/>
                </p:submenu>
            </p:menu>
        </h:form>

    </body>
</html>
```

Ao executar seu projeto, ele deverá estar como mostra a figura abaixo:

![](/assets/menuateagora.png)

