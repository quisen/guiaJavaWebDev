# Menu Principal

Agora que já temos a estrutura básica do nosso projeto, podemos iniciar a customização do menu principal e a criação das páginas que irão preencher e atualizar o content.

Primerio criaremos **três **novas páginas, **alunos.xhtml**, **disciplinas.xhtml** e **matriculas.xhtml**\).

Essas páginas irão preencher o conteúdo do facelet Content de acordo com a opção selecionada no menu.

Portanto, elas precisam ser Clientes do Modelo de Facelets que foi definido anteriormente.

![](/assets/contentscustom.png)

Depois de criar essas três páginas, modifique seu **leftFacelet.xhtml** para implementar as opções no Menu Principal.

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

Seu projeto deverá estar como mostra na figura abaixo:

![](/assets/menuateagora.png)

