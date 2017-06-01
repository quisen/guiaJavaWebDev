# alunos.xhtml

Agora iremos complementar a página **alunos.xhtml** com a inserção de alguns componentes do PrimeFaces.

Note que os elementos que iremos mostrar abaixo **já estão customizados** com as **variáveis** do nosso projeto, ou seja, na **página** do **PrimeFaces** o código estará disponível de maneira **diferente**, mais **genérica**.

O **código** completo estará **disponível** ao final da página.

A estrutura principal dessa página está dentro da tag** ui:define="content" **que possui basicamente três forms distintos que executam funções para a visualização, inserção, edição e exclusão.

Os forms são: **tabela**, **modifica **e **cadastro.**

Iremos abordar um form por vez, o primeiro a ser comentado será o **tabela**.

```xhtml
<h:form id="tabela">
    <p:commandButton value="Cadastrar novo Aluno" process="@this"
    update=":cadastro:novo-aluno-dialog"
    oncomplete="PF('novo-aluno-widget').show()"/>
    <br/>
    <br/>
    <p:dataTable id="tabela-alunos"
        var="aluno"
        value="#{alunoBean.alunos}"
        tableStyle="text-align:center">

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

        <p:column style="width:40px;text-align: center">
            <p:commandButton update=":modifica:modifica-aluno-panel" 
                oncomplete="PF('modifica-aluno-widget').show()" 
                icon="ui-icon-arrow-4-diag"
                title="View" action="#{alunoBean.enviaAluno(aluno)}" />
        </p:column>

    </p:dataTable>
</h:form>
```

O form **tabela** pode ser decomposto em três principais blocos:

* O **primeiro** é o **commandButton** para **cadastrar** um novo aluno. 
* O **segundo** é o **dataTable** para a **visualização** dos registros. 
* O **terceiro** é outro **commandButton** para **editar** um aluno já existente, que está inserido na **última** **coluna** da direita.

Para o **primeiro** bloco \(cadastrar\) o atributo **"update"** indica que irá ocorrer alguma mudança visual na página, sendo que a parte ":cadastro:" é uma referência à outro form \(veja em seguida\).

O atributo **"oncomplete"** define uma ação que irá ser realizada quando o botão for clicado.

Dentro do **segundo** bloco \(dataTable - tabela-alunos\) observe que a **"dataTable"** possui o atributo "**var**", que é o tipo de objeto que será iterado no preenchimento - e o "**value**" é uma lista que contém objetos do mesmo tipo que declaramos na tag anterior.

Em cada coluna, usa-se diretamente a referência de variáveis em relação ao objeto que estamos utilizando, como por exemplo, "\#{aluno.nome}".

No **terceiro** bloco \(EDITAR\) o atributo **"update"** indica que irá ocorrer alguma mudança visual na página, sendo que a parte ":modifica:" é uma referência à outro form \(veja em seguida\).

#### **ATENÇÃO!**

Nosso Managed Bean possui o nome "**AlunoBean**", porém na hora de referenciá-lo no código xhtml, devemos sempre utilizar a **primeira letra MINÚSCULA,** caso contrário, ocorrerão **ERROS** de execução.

Agora iremos detalhar o segundo form, **modifica**.

```
<h:form id="modifica">
    <p:dialog header="Modificar Aluno Selecionado" widgetVar="modifica-aluno-widget" modal="false" showEffect="fade" hideEffect="fade" resizable="false" id="modifica-aluno-dialog">
        <p:outputPanel id="modifica-aluno-panel" style="text-align:center">
            <h:panelGrid  columns="2" columnClasses="label,value">
                <p:outputLabel value="Nome" />
                <p:inputText size="20" value="#{alunoBean.alunoSelecionado.nome}" />

                <p:outputLabel value="CPF" />
                <p:inputText value="#{alunoBean.alunoSelecionado.cpf}" size="20" />

                <p:outputLabel value="Telefone" />
                <p:inputText value="#{alunoBean.alunoSelecionado.telefone}" size="20" />

                <p:outputLabel value="Endereço" />
                <p:inputText value="#{alunoBean.alunoSelecionado.endereco}" size="20" />

                <p:outputLabel value="Período" />
                <p:inputText value="#{alunoBean.alunoSelecionado.periodo}" size="20" />
            </h:panelGrid>
    <br/>
            <p:commandButton value="Modificar" id="btnEdit" actionListener="#{alunoBean.modificaAluno}" update=":tabela:tabela-alunos" oncomplete="PF('modifica-aluno-widget').hide()" >
                <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
            </p:commandButton>
            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                <p:commandButton value="Sim" type="button" styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                <p:commandButton value="Não" type="button" styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
            </p:confirmDialog>
            <p:commandButton value="Excluir" actionListener="#{alunoBean.deletaAluno}" update=":tabela:tabela-alunos" oncomplete="PF('modifica-aluno-widget').hide()">
                <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
            </p:commandButton>
            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                <p:commandButton value="Sim" type="button" styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                <p:commandButton value="Não" type="button" styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
            </p:confirmDialog>
        </p:outputPanel>
    </p:dialog>
    <p:defaultCommand target="btnEdit" />
</h:form>
```

Agora iremos detalhar o terceiro form, **cadastro**.

```
                <h:form id="cadastro">
                    <p:dialog header="Novo Aluno" widgetVar="novo-aluno-widget" id="novo-aluno-dialog"
                              resizable="false" modal="false" closeOnEscape="true">
                        <p:outputPanel style="text-align:center">
                            <h:panelGrid  columns="2" columnClasses="label,value">

                                <p:outputLabel value="Nome" />
                                <p:inputText size="20" value="#{alunoBean.aluno.nome}" />

                                <p:outputLabel value="CPF" />
                                <p:inputText value="#{alunoBean.aluno.cpf}" size="20" />

                                <p:outputLabel value="Telefone" />
                                <p:inputText value="#{alunoBean.aluno.telefone}" size="20" />

                                <p:outputLabel value="Endereço" />
                                <p:inputText value="#{alunoBean.aluno.endereco}" size="20" />

                                <p:outputLabel value="Período" />
                                <p:inputText value="#{alunoBean.aluno.periodo}" size="20" />

                                <br/>
                                <p:commandButton id="btnCadastro" value="Cadastrar" action="#{alunoBean.novoCadastro}" update=":tabela:tabela-alunos" oncomplete="PF('novo-aluno-widget').hide()" >
                                    <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
                                </p:commandButton>
                                <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                                    <p:commandButton value="Sim" type="button" styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                                    <p:commandButton value="Não" type="button" styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
                                </p:confirmDialog>

                            </h:panelGrid>
                        </p:outputPanel>
                    </p:dialog>

                    <p:defaultCommand target="btnCadastro" />

                </h:form>
```


