# Integrando com Java

Agora iremos complementar a página **aluno.xhtml** com a inserção de alguns componentes do PrimeFaces.

Mas, note que os elementos que iremos mostrar abaixo **já estão customizados** com as **variáveis** do nosso projeto, ou seja, na **página** do **PrimeFaces** o código estará disponível de maneira **diferente**, mais **genérica**.

Primeiramente gostaríamos de mostrar a estrutura almejada para a página **aluno**:

![](/assets/alunoCOLAPSED.png)

É importante ressaltar que alguns elementos estão **omissos** na imagem a cima. Não se preocupe em tentar copiá-lo agora, pois todos os **códigos** estarão **disponiveis** no **fim** dessa página.

A estrutura principal dessa página está dentro da tag** ui:define="content"**, e ela consiste basicamente em três **forms  **- tabela, modifica e cadastro - que executam funções  para a visualização, inserção edição e exclusão.

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
                    <p:commandButton value="Cadastrar novo Aluno" process="@this" update=":cadastro:novo-aluno-dialog"
                                     oncomplete="PF('novo-aluno-widget').show()" />
                    <br/>
                    <br/>
                    <p:dataTable id="tabela-alunos" var="aluno" value="#{alunoBean.alunos}" tableStyle="text-align:center">
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
                            <p:commandButton update=":modifica:modifica-aluno-panel" oncomplete="PF('modifica-aluno-widget').show()" icon="ui-icon-arrow-4-diag"
                                             title="View" action="#{alunoBean.enviaAluno(aluno)}" />
                        </p:column>
                    </p:dataTable>
                </h:form>

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

            </ui:define>
        </ui:composition>
    </h:body>

</html>
```

Neste exemplo a **"dataTable"** possui o atributo "**var**", que é o tipo de objeto que será iterado no preenchimento - e o "**value**" é uma lista que contém objetos do mesmo tipo que declaramos na tag anterior.

Em cada coluna, usa-se diretamente a referência de variáveis em relação ao objeto que estamos utilizando, como por exemplo, "\#{aluno.nome}".

**ATENÇÃO, **nosso Managed Bean possui o nome "AlunoBean", porém na hora de referenciá-lo no código xhtml, devemos sempre utilizar a **primeira letra minúscula,** caso contrário, ocorrerão **erros** de execução.

**Repita** o processo para o arquivo **disciplina.xhtml**:

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
                    <p:commandButton value="Cadastrar nova Disciplina" process="@this" update=":cadastro:nova-disciplina-dialog"
                                     oncomplete="PF('nova-disciplina-widget').show()" />
                    <br/>
                    <br/>
                    <p:dataTable id="tabela-disciplinas" var="disciplina" value="#{disciplinaBean.disciplinas}" tableStyle="text-align:center">
                        <p:column headerText="Id">
                            <h:outputText value="#{disciplina.id}" />
                        </p:column>

                        <p:column headerText="Nome">
                            <h:outputText value="#{disciplina.nome}" />
                        </p:column>

                        <p:column headerText="Professor">
                            <h:outputText value="#{disciplina.professor}" />
                        </p:column>

                        <p:column headerText="Período">
                            <h:outputText value="#{disciplina.periodo}" />
                        </p:column>

                        <p:column style="width:40px;text-align: center">
                            <p:commandButton update=":modifica:modifica-disciplina-panel" oncomplete="PF('modifica-disciplina-widget').show()" icon="ui-icon-arrow-4-diag"
                                             title="View" action="#{disciplinaBean.enviaDisciplina(disciplina)}" />
                        </p:column>
                    </p:dataTable>
                </h:form>

                <h:form id="modifica">
                    <p:dialog header="Modificar Disciplina Selecionada" widgetVar="modifica-disciplina-widget" modal="false" showEffect="fade" hideEffect="fade" resizable="false" id="modifica-disciplina-dialog">
                        <p:outputPanel id="modifica-disciplina-panel" style="text-align:center">

                            <h:panelGrid  columns="2" columnClasses="label,value">
                                <p:outputLabel value="Nome" />
                                <p:inputText size="20" value="#{disciplinaBean.disciplinaSelecionada.nome}" />

                                <p:outputLabel value="Professor" />
                                <p:inputText value="#{disciplinaBean.disciplinaSelecionada.professor}" size="20" />

                                <p:outputLabel value="Período" />
                                <p:inputText value="#{disciplinaBean.disciplinaSelecionada.periodo}" size="20" />
                            </h:panelGrid>
                            <br/>
                            <p:commandButton value="Modificar" id="btnEdit" actionListener="#{disciplinaBean.modificaDisciplina}" update=":tabela:tabela-disciplinas" oncomplete="PF('modifica-disciplina-widget').hide()" >
                                <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
                            </p:commandButton>
                            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                                <p:commandButton value="Sim" type="button" styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                                <p:commandButton value="Não" type="button" styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
                            </p:confirmDialog>
                            <p:commandButton value="Excluir" actionListener="#{disciplinaBean.deletaDisciplina}" update=":tabela:tabela-disciplinas" oncomplete="PF('modifica-disciplina-widget').hide()">
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
                    <p:dialog header="Novo Disciplina" widgetVar="nova-disciplina-widget" id="nova-disciplina-dialog"
                              resizable="false" modal="false" closeOnEscape="true">
                        <p:outputPanel style="text-align:center">
                            <h:panelGrid  columns="2" columnClasses="label,value">

                                <p:outputLabel value="Nome" />
                                <p:inputText size="20" value="#{disciplinaBean.disciplina.nome}" />

                                <p:outputLabel value="Professor" />
                                <p:inputText value="#{disciplinaBean.disciplina.professor}" size="20" />

                                <p:outputLabel value="Período" />
                                <p:inputText value="#{disciplinaBean.disciplina.periodo}" size="20" />

                                <br/>
                                <p:commandButton id="btnCadastro" value="Cadastrar" action="#{disciplinaBean.novoCadastro}" update=":tabela:tabela-disciplinas" oncomplete="PF('nova-disciplina-widget').hide()" >
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



