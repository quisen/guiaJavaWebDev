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



