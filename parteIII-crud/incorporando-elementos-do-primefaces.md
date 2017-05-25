# Incorporando elementos do PrimeFaces

Abaixo segue o link com exemplos simples de componentes para tabelas do PrimeFaces:

[https://www.primefaces.org/showcase/ui/data/datatable/basic.xhtml](https://www.primefaces.org/showcase/ui/data/datatable/basic.xhtml)

Agora iremos inserir no **aluno.xhtml** alguns componentes do PrimeFaces já customizados com as variáveis do nosso projeto:

```xhtml
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
        <h:form id="tabela">            
            <p:dataTable id="tabela-alunos" var="aluno" value="#{alunoBean.alunos}">
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

Neste exemplo a **"dataTable"** possui o atributo "**var**", que é o tipo de objeto que será iterado no preenchimento - e o "**value**" é uma lista que contém objetos do mesmo tipo que declaramos na tag anterior.

Em cada coluna, usa-se diretamente a referência de variáveis em relação ao objeto que estamos utilizando, como por exemplo, "\#{aluno.nome}".

**ATENÇÃO, **nosso Managed Bean possui o nome "AlunoBean", porém na hora de referenciá-lo no código xhtml, devemos sempre utilizar a **primeira letra minúscula,** caso contrário, ocorrerão **erros** de execução.

**Repita** o processo para o arquivo **disciplina.xhtml**:

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





