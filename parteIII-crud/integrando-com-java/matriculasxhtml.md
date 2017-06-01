# matriculas.xhtml

## &lt;h:form id="tabela"&gt;

```xhtml
<h:form id="tabela">
    <p:commandButton value="Nova Matrícula" process="@this"
    update=":cadastro:nova-matricula-dialog"
    oncomplete="PF('nova-matricula-widget').show()" />
    <br/>
    <br/>
    <p:dataTable id="tabela-matriculas" var="matriculaMin"
    value="#{matriculaBean.matriculasMin}" sortBy="#{matriculaMin.aluno.nome}"
    expandableRowGroups="true" tableStyle="text-align:center" >

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
            <p:commandButton update=":modifica:modifica-matricula-panel"
            oncomplete="PF('modifica-matricula-widget').show()"
            icon="ui-icon-arrow-4-diag" title="View"
            action="#{matriculaBean.enviaMatricula(matriculaMin)}" />
        </p:column>
    </p:dataTable>
</h:form>
```

## &lt;h:form id="cadastro"&gt;

```xhtml
<h:form id="cadastro">
    <p:dialog header="Nova Matrícula" widgetVar="nova-matricula-widget"
    id="nova-matricula-dialog" resizable="false"
    modal="false" closeOnEscape="true">
        <p:outputPanel style="text-align:center">
            <h:panelGrid  columns="2" columnClasses="label,value">
                <p:outputLabel value="Aluno: "  />
                <p:selectOneMenu value="#{matriculaBean.alunoSelecionado}"
                converter="alunoConverter" panelStyle="width:180px"
                effect="fade" var="a" style="width:160px"
                filter="true" filterMatchMode="startsWith">
                    <f:selectItem itemLabel="Selecione" itemValue="" />
                    <f:selectItems value="#{matriculaBean.alunos}"
                    var="aluno" itemLabel="#{aluno.matricula} - #{aluno.nome}"
                    itemValue="#{aluno}" />

                    <p:column style="width:10%">
                        <h:outputText value="#{a.matricula}" />
                    </p:column>

                    <p:column>
                        <h:outputText value="#{a.nome}" />
                    </p:column>
                </p:selectOneMenu>

                <p:outputLabel value="Disciplina "  />
                <p:selectOneMenu value="#{matriculaBean.disciplinaSelecionada}"
                converter="disciplinaConverter" panelStyle="width:180px"
                effect="fade" var="d" style="width:160px"
                filter="true" filterMatchMode="startsWith">
                    <f:selectItem itemLabel="Selecione" itemValue="" />
                    <f:selectItems value="#{matriculaBean.disciplinas}"
                    var="disciplina" itemLabel="#{disciplina.nome}"
                    itemValue="#{disciplina}" />

                    <p:column style="width:10%">
                        <h:outputText value="#{d.nome}" />
                    </p:column>
                </p:selectOneMenu>

                <br/>
                <p:commandButton id="btnCadastro" value="Cadastrar"
                action="#{matriculaBean.novaMatricula}"
                update=":tabela:tabela-matriculas"
                oncomplete="PF('nova-matricula-widget').hide()" >
                    <p:confirm header="Confirmação" message="Tem certeza?"
                    icon="ui-icon-alert"  />
                </p:commandButton>
                <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                    <p:commandButton value="Sim" type="button"
                    styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                    <p:commandButton value="Não" type="button"
                    styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
                </p:confirmDialog>

            </h:panelGrid>
        </p:outputPanel>
    </p:dialog>
    <p:defaultCommand target="btnCadastro" />
</h:form>
```

## &lt;h:form id="modifica"&gt;

```xhtml
<h:form id="modifica">
    <p:dialog header="Modificar Matrícula Selecionada"
    widgetVar="modifica-matricula-widget" modal="false"
    showEffect="fade" hideEffect="fade" resizable="false"
    id="modifica-matricula-dialog">
        <p:outputPanel id="modifica-matricula-panel"
        style="text-align:center">

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
                    <p:selectOneMenu
                    value="#{matriculaBean.matriculaMinSelecionada.disciplina}"
                    converter="disciplinaConverter" panelStyle="width:180px"
                    effect="fade" var="d" style="width:160px"
                    filter="true" filterMatchMode="startsWith">
                        <f:selectItem itemLabel="Selecione" itemValue="" />
                        <f:selectItems value="#{matriculaBean.disciplinas}"
                        var="disciplina" itemLabel="#{disciplina.nome}"
                        itemValue="#{disciplina}" />

                        <p:column style="width:10%">
                            <h:outputText value="#{d.nome}" />
                        </p:column>
                    </p:selectOneMenu>
            </h:panelGrid>
            <br/>
            <p:commandButton value="Modificar" id="btnEdit"
            actionListener="#{matriculaBean.modificaMatricula}"
            update=":tabela:tabela-matriculas"
            oncomplete="PF('modifica-matricula-widget').hide()" >
                <p:confirm header="Confirmação" message="Tem certeza?"
                icon="ui-icon-alert"  />
            </p:commandButton>
            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                <p:commandButton value="Sim" type="button"
                styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                <p:commandButton value="Não" type="button"
                styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
            </p:confirmDialog>
            <p:commandButton value="Excluir" actionListener="#{matriculaBean.deletaMatricula}"
            update=":tabela:tabela-matriculas"
            oncomplete="PF('modifica-matricula-widget').hide()">
                <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
            </p:commandButton>
            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                <p:commandButton value="Sim" type="button"
                styleClass="ui-confirmdialog-yes" icon="ui-icon-check" />
                <p:commandButton value="Não" type="button"
                styleClass="ui-confirmdialog-no" icon="ui-icon-close" />
            </p:confirmDialog>
        </p:outputPanel>
    </p:dialog>
    <p:defaultCommand target="btnEdit" />
</h:form>
```

---

Link para download do arquivo matriculas.xhtml: https://gist.github.com/quisen/ededf8cdf2dd5a7b46b1ac8810f57671

