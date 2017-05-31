# Integrando com Java

Agora iremos complementar a página **aluno.xhtml** com a inserção de alguns componentes do PrimeFaces.

Note que os elementos que iremos mostrar abaixo **já estão customizados** com as **variáveis** do nosso projeto, ou seja, na **página** do **PrimeFaces** o código estará disponível de maneira **diferente**, mais **genérica**.

Primeiramente gostaríamos de mostrar a estrutura almejada para a página **aluno**:

![](/assets/alunoMinimal.png)

É importante ressaltar que alguns trechos de código estão **omissos** na imagem a cima. Não se preocupe em tentar copiá-lo agora, pois todos os **códigos** estarão **disponiveis** no **fim** dessa página.

A estrutura principal dessa página está dentro da tag** ui:define="content"**, e ela consiste basicamente em três forms distintos que executam funções para a visualização, inserção, edição e exclusão.

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

Para o **segundo** bloco \(dataTable - tabela-alunos\) observe que a **"dataTable"** possui o atributo "**var**", que é o tipo de objeto que será iterado no preenchimento - e o "**value**" é uma lista que contém objetos do mesmo tipo que declaramos na tag anterior.

Em cada coluna, usa-se diretamente a referência de variáveis em relação ao objeto que estamos utilizando, como por exemplo, "\#{aluno.nome}".

#### **ATENÇÃO!**

Nosso Managed Bean possui o nome "**AlunoBean**", porém na hora de referenciá-lo no código xhtml, devemos sempre utilizar a **primeira letra MINÚSCULA,** caso contrário, ocorrerão **ERROS** de execução.

O segundo form é o **modifica**.

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

**Repita** o processo para o arquivo **disciplina.xhtml**

Então fazemos um processo semelhante em **matricula.xhtml**:

```xhtml
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
      xmlns:h="http://xmlns.jcp.org/jsf/html"
      xmlns:p="http://primefaces.org/ui"
      xmlns:f="http://xmlns.jcp.org/jsf/core">

    <h:body>
        <ui:composition template="./template.xhtml">
            <ui:define name="content">
                <h:form id="tabela">
                    <p:commandButton value="Nova Matrícula" process="@this" update=":cadastro:nova-matricula-dialog"
                                     oncomplete="PF('nova-matricula-widget').show()" />
                    <br/>
                    <br/>
                    <p:dataTable id="tabela-matriculas" var="matriculaMin" value="#{matriculaBean.matriculasMin}"
                                 sortBy="#{matriculaMin.aluno.nome}" expandableRowGroups="true" tableStyle="text-align:center" >

                        <p:headerRow>
                            <p:column colspan="1">
                                <h:outputText value="Aluno(a): #{matriculaMin.aluno.nome}" />
                            </p:column>
                            <p:column colspan="1">
                                <h:outputText value="RA: #{matriculaMin.aluno.matricula}" />
                            </p:column>
                            <p:column colspan="2">
                                <h:outputText value="Período: #{matriculaMin.aluno.periodo}" />
                            </p:column>
                        </p:headerRow>

                        <p:column headerText="Disciplina">
                            <h:outputText value="#{matriculaMin.disciplina.nome}" />
                        </p:column>

                        <p:column headerText="Professor">
                            <h:outputText value="#{matriculaMin.disciplina.professor}" />
                        </p:column>

                        <p:column headerText="Período da Disciplina">
                            <h:outputText value="#{matriculaMin.disciplina.periodo}" />
                        </p:column>

                        <p:column style="width:40px;text-align: center">
                            <p:commandButton update=":modifica:modifica-matricula-panel" oncomplete="PF('modifica-matricula-widget').show()" icon="ui-icon-arrow-4-diag"
                                             title="View" action="#{matriculaBean.enviaMatricula(matriculaMin)}" />
                        </p:column>
                    </p:dataTable>
                </h:form>

                <h:form id="modifica">
                    <p:dialog header="Modificar Matrícula Selecionada" widgetVar="modifica-matricula-widget" modal="false" showEffect="fade" hideEffect="fade" resizable="false" id="modifica-matricula-dialog">
                        <p:outputPanel id="modifica-matricula-panel" style="text-align:center">

                            <h:panelGrid  columns="2" columnClasses="label,value">
                                <p:outputLabel value="Aluno:" />
                                <p:outputLabel value="#{matriculaBean.matriculaMinSelecionada.aluno.nome}" />
                                <br/>
                                <p:outputLabel value=""/>
                                <p:outputLabel value="RA:" />
                                <p:outputLabel value="#{matriculaBean.matriculaMinSelecionada.aluno.matricula}" />
                                <br/>
                                <p:outputLabel value=""/>
                                <p:outputLabel value="Disciplina"  />
                                <p:selectOneMenu value="#{matriculaBean.matriculaMinSelecionada.disciplina}" converter="disciplinaConverter" panelStyle="width:180px"
                                                 effect="fade" var="d" style="width:160px" filter="true" filterMatchMode="startsWith">
                                    <f:selectItem itemLabel="Selecione" itemValue="" />
                                    <f:selectItems value="#{matriculaBean.disciplinas}" var="disciplina" itemLabel="#{disciplina.nome}" itemValue="#{disciplina}" />

                                    <p:column style="width:10%">
                                        <h:outputText value="#{d.nome}" />
                                    </p:column>
                                </p:selectOneMenu>

                            </h:panelGrid>
                            <br/>
                            <p:commandButton value="Modificar" id="btnEdit" actionListener="#{matriculaBean.modificaMatricula}" update=":tabela:tabela-matriculas" oncomplete="PF('modifica-matricula-widget').hide()" >
                                <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
                            </p:commandButton>
                            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                                <p:commandButton value="Sim" type="button" styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                                <p:commandButton value="Não" type="button" styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
                            </p:confirmDialog>
                            <p:commandButton value="Excluir" actionListener="#{matriculaBean.deletaMatricula}" update=":tabela:tabela-matriculas" oncomplete="PF('modifica-matricula-widget').hide()">
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

                <h:form id="cadastro">
                    <p:dialog header="Nova Matrícula" widgetVar="nova-matricula-widget" id="nova-matricula-dialog"
                              resizable="false" modal="false" closeOnEscape="true">
                        <p:outputPanel style="text-align:center">
                            <h:panelGrid  columns="2" columnClasses="label,value">

                                <p:outputLabel value="Aluno: "  />
                                <p:selectOneMenu value="#{matriculaBean.alunoSelecionado}" converter="alunoConverter" panelStyle="width:180px"
                                                 effect="fade" var="a" style="width:160px" filter="true" filterMatchMode="startsWith">
                                    <f:selectItem itemLabel="Selecione" itemValue="" />
                                    <f:selectItems value="#{matriculaBean.alunos}" var="aluno" itemLabel="#{aluno.matricula} - #{aluno.nome}" itemValue="#{aluno}" />

                                    <p:column style="width:10%">
                                        <h:outputText value="#{a.matricula}" />
                                    </p:column>

                                    <p:column>
                                        <h:outputText value="#{a.nome}" />
                                    </p:column>
                                </p:selectOneMenu>

                                <p:outputLabel value="Disciplina "  />
                                <p:selectOneMenu value="#{matriculaBean.disciplinaSelecionada}" converter="disciplinaConverter" panelStyle="width:180px"
                                                 effect="fade" var="d" style="width:160px" filter="true" filterMatchMode="startsWith">
                                    <f:selectItem itemLabel="Selecione" itemValue="" />
                                    <f:selectItems value="#{matriculaBean.disciplinas}" var="disciplina" itemLabel="#{disciplina.nome}" itemValue="#{disciplina}" />

                                    <p:column style="width:10%">
                                        <h:outputText value="#{d.nome}" />
                                    </p:column>
                                </p:selectOneMenu>

                                <br/>
                                <p:commandButton id="btnCadastro" value="Cadastrar" action="#{matriculaBean.novaMatricula}" update=":tabela:tabela-matriculas" oncomplete="PF('nova-matricula-widget').hide()" >
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

            </ui:define>
        </ui:composition>
    </h:body>

</html>
```

alunos.xhtml Link: [https://gist.github.com/quisen/9e575c58ff28cdded07f789ca16b9146](https://gist.github.com/quisen/9e575c58ff28cdded07f789ca16b9146)

disciplinas.xhtml Link: [https://gist.github.com/quisen/791930ba6c15a23a2dfbb55a0d392d63](https://gist.github.com/quisen/791930ba6c15a23a2dfbb55a0d392d63)

matriculas.xhtml Link: https://gist.github.com/quisen/ededf8cdf2dd5a7b46b1ac8810f57671

