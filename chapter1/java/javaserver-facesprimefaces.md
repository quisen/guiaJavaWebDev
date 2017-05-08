# Introdução à Criação de Páginas Web

Automaticamente um index.html é criado dentro das Páginas Web do seu projeto, você deve excluí-lo.

![](/assets/excluir index gerado.png)

Agora clique com o botão direito do mouse sobre o seu projeto no canto esquerdo da tela e vá em Propriedades, dentro das Categorias procure por Frameworks, depois disso clique para adicionar um framework e escolha pela opção JavaServer Faces.

![](/assets/adicionando jsf framework.png)

Clique em OK e finalize. 

**Atenção**: **não** **marque** a opção PrimeFaces nesta etapa, pois iremos adicioná-lo de outra maneira.

![](/assets/jsf click ok.png)

Em seguida, navegue até o arquivo **pom.xml **dentro dele você deverá adicionar as dependências do seu projeto. A primeira que iremos adicionar é a biblioteca do **PrimeFaces** versão 6.1. 

![](/assets/editar pom.xml add prime.png)

Agora clique no ícone de Clean and Build para garantir que o Maven baixe todas as dependências corretamente.

![](/assets/rebuild.png)

Agora um novo arquivo chamado **index**.**xhtml** foi criado pelo NetBeans.

![](/assets/index novo.png)

Abaixo segue o link com exemplos simpoles de componentes para tabelas do PrimeFaces:

[https://www.primefaces.org/showcase/ui/data/datatable/basic.xhtml](https://www.primefaces.org/showcase/ui/data/datatable/basic.xhtml)



index.xhtml

```
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
      xmlns:h="http://xmlns.jcp.org/jsf/html"
      xmlns:p="http://primefaces.org/ui"
      xmlns:f="http://xmlns.jcp.org/jsf/core">

    <h:head>
        <title>Universidade Exemplo</title>
    </h:head>

    <h:body>
        <h:form>            
            <p:dataTable var="aluno" value="#{alunoBean.alunos}">
                <p:column headerText="Id">
                    <h:outputText value="#{aluno.id}" />
                </p:column>

                <p:column headerText="Nome">
                    <h:outputText value="#{aluno.nome}" />
                </p:column>

                <p:column headerText="Matricula">
                    <h:outputText value="#{aluno.matricula}" />
                </p:column>

                <p:column headerText="CPF">
                    <h:outputText value="#{aluno.cpf}" />
                </p:column>

                <p:column headerText="Telefone">
                    <h:outputText value="#{aluno.telefone}" />
                </p:column>

                <p:column headerText="Endereço">
                    <h:outputText value="#{aluno.endereco}" />
                </p:column>

                <p:column headerText="Período">
                    <h:outputText value="#{aluno.periodo}" />
                </p:column>
            </p:dataTable>
        </h:form>
    </h:body>

</html>
```

Neste exemplo que utilizamos, a "dataTable" possui o atributo "var", que é o tipo de objeto que será iterado no preenchimento - e o "value" é uma lista que contém objetos do mesmo tipo que declaramos na tag anterior. Em cada coluna, usa-se diretamente a referência de variáveis em relação ao objeto que estamos utilizando, como por exemplo, "\#{aluno.nome}".

**Vale ressaltar que, **nosso managed bean possui o nome "AlunoBean", porém na hora de referenciar no código xhtml, deve-se usar a primeira letra minúscula, caso contrário, ocorrerão erros de execução.



Resultado final:

![](/assets/tabela.png)

Agora veremos um exemplo de como executar ações assíncronas com componentes web.

**Ação via botão**

Adicionamos o botão na nossa página web:

```
<p:commandButton value="Cadastrar novo Aluno" action="#{alunoBean.novoCadastro}"/>
```

E então adicionamos o método novoCadastro\(\) no nosso backing bean:



```
    public void novoCadastro() {
            TODO        
    }
```



