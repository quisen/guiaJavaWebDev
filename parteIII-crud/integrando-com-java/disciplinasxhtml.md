# disciplinas.xhtml

Agora iremos complementar a página **disciplinas.xhtml**, perceba que o processo é análogo ao realizado na página anterior alunos.xhtml.

Os forms são: **tabela**, **modifica **e **cadastro. **Iremos detalhar um form de cada vez.

## &lt;h:form id="tabela"&gt;

```xhtml
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
            <p:commandButton update=":modifica:modifica-disciplina-panel"
            oncomplete="PF('modifica-disciplina-widget').show()"
            icon="ui-icon-arrow-4-diag" title="View"
            action="#{disciplinaBean.enviaDisciplina(disciplina)}" />
        </p:column>
    </p:dataTable>
</h:form>
```

O form **tabela** pode ser explicado em três blocos:

* O **primeiro** é o **commandButton** para **cadastrar** uma nova disciplina. 
* O **segundo** é o **dataTable** para a **visualização** dos registros. 
* O **terceiro** é outro **commandButton** para **editar** uma disciplina já existente, e ele está inserido na **última** **coluna** da direita.

Para o **primeiro** bloco \(cadastrar\) o atributo **"update"** indica que irá ocorrer alguma mudança visual na página, sendo que a parte ":cadastro:" é uma referência à outro form \(veja em seguida\). O atributo **"oncomplete"** define uma ação que irá ser realizada quando o botão for clicado.

Dentro do **segundo** bloco \(dataTable - tabela-disciplinas\) observe que a **"dataTable"** possui o atributo "**var**", que é o tipo de objeto que será iterado no preenchimento - e o "**value**" é uma lista que contém objetos do mesmo tipo que declaramos na tag anterior.

Em cada coluna, usa-se diretamente a referência de variáveis em relação ao objeto que estamos utilizando, como por exemplo, "\#{disciplina.nome}".

No **terceiro** bloco \(EDITAR\) o atributo **"update" **dentro do commandButton indica que irá ocorrer alguma mudança visual na página, sendo que a parte ":modifica:" é uma referência à outro form \(veja em seguida\).

Esse mesmo botão servirá também para abrir o pop-up com as informações da disciplina selecionada, e ao mesmo tempo acionar o método "enviaDisciplina" que repassa os dados do registro clicado para um objeto volátil \(sem informação fixa\) "disciplinaSelecionada" - que receberá o tratamento adequado.

Abaixo é possível visualizar o botão inserido em uma nova coluna da tabela:

![](/assets/editaDisciplina.png)

**ATENÇÃO!**

Nosso Managed Bean possui o nome "**DisciplinaBean**", porém na hora de referenciá-lo no código xhtml, devemos sempre utilizar a **primeira letra MINÚSCULA,** caso contrário, ocorrerão **ERROS** de execução.

## &lt;h:form id="cadastro"&gt;

Agora iremos detalhar o segundo form, **cadastro**.

```xhtml
<h:form id="cadastro">
    <p:dialog header="Novo Disciplina" widgetVar="nova-disciplina-widget"
    id="nova-disciplina-dialog" resizable="false" modal="false" closeOnEscape="true">
        <p:outputPanel style="text-align:center">
            <h:panelGrid  columns="2" columnClasses="label,value">

                <p:outputLabel value="Nome" />
                <p:inputText size="20" value="#{disciplinaBean.disciplina.nome}" />

                <p:outputLabel value="Professor" />
                <p:inputText value="#{disciplinaBean.disciplina.professor}" size="20" />

                <p:outputLabel value="Período" />
                <p:inputText value="#{disciplinaBean.disciplina.periodo}" size="20" />

                <br/>

                <p:commandButton id="btnCadastro" value="Cadastrar"
                action="#{disciplinaBean.novoCadastro}"
                update=":tabela:tabela-disciplinas"
                oncomplete="PF('nova-disciplina-widget').hide()" >
                    <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
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

Partes relevantes do código:

* O **panelGrid** é um componente que estabelece uma área pré-determinada com uma organização específica, neste caso - duas colunas, sendo a primeira um label \(etiqueta, texto indicativo\), e a segunda o valor do campo.
* Na entrada de dados \(**inputText**\), o atributo "value" indica qual variável no código java será linkado com o campo no xhtml respectivo. Neste caso, referenciamos nossa classe disciplinaBean, e dentro da classe teremos um objeto chamado "disciplina", então acessamos suas variáveis utilizando o ponto \(criaremos este objeto e os métodos de criação logo em seguida\).
* Criamos um botão que executa uma ação \(**action**=""\), neste caso, executa o método novoCadastro do nosso código java.
* O botão possui o atributo "**update**" que realizará uma mudança visual na tabela, pois ao adicionar o novo usuário queremos que ele seja listado imediatamente junto com os demais.
* A tag "**defaultCommand**" indica o id do componente que será executado ao pressionar a tecla Enter.

E então adicionamos o método novoCadastro\(\) no nosso managed bean, bem como o objeto "disciplina", que será utilizado para fazer o link dos campos de texto para cadastro.

Visualização do pop-up:

![](/assets/dialogDisciplina.png)

Preenchimento do pop-up:

![](/assets/dialogDisciplinaPreenchido.png)

Nova disciplina listada e cadastrada após confirmação do preenchimento no pop-up:

![](/assets/resultadoDisciplinaInserida.png)

## &lt;h:form id="modifica"&gt;

Esse form irá conter um pop-up que irá mostrar os dados do aluno selecionado para edição ou exclusão.

```xhtml
<h:form id="modifica">
    <p:dialog header="Modificar Disciplina Selecionada"
    widgetVar="modifica-disciplina-widget" modal="false"
    showEffect="fade" hideEffect="fade" resizable="false" id="modifica-disciplina-dialog">
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
            <p:commandButton value="Modificar" id="btnEdit"
            actionListener="#{disciplinaBean.modificaDisciplina}"
            update=":tabela:tabela-disciplinas" oncomplete="PF('modifica-disciplina-widget').hide()" >
                <p:confirm header="Confirmação" message="Tem certeza?" icon="ui-icon-alert"  />
            </p:commandButton>
            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade">
                <p:commandButton value="Sim" type="button" styleClass="ui-confirmdialog-yes"
                icon="ui-icon-check" />
                <p:commandButton value="Não" type="button" styleClass="ui-confirmdialog-no"
                icon="ui-icon-close" />
            </p:confirmDialog>
            <p:commandButton value="Excluir" actionListener="#{disciplinaBean.deletaDisciplina}" update=":tabela:tabela-disciplinas" oncomplete="PF('modifica-disciplina-widget').hide()">
                <p:confirm header="Confirmação" message="#{disciplinaBean.msgConfirmacao}" icon="ui-icon-alert"  />
            </p:commandButton>
            <p:confirmDialog global="true" showEffect="fade" hideEffect="fade" closeOnEscape="true" responsive="true" width="#{disciplinaBean.larguraPopupConfirma}">
                <p:commandButton value="Sim" type="button" styleClass="ui-confirmdialog-yes"
                icon="ui-icon-check" />
                <p:commandButton value="Não" type="button" styleClass="ui-confirmdialog-no"
                icon="ui-icon-close" />
            </p:confirmDialog>
        </p:outputPanel>
    </p:dialog>
    <p:defaultCommand target="btnEdit" />
</h:form>
```

Note que no trecho de código acima temos dois botões que executam os métodos java que criamos anteriormente, um para **modificação** e outro para **exclusão**.

Visualização da janela pop-up ao clicar no botão de edição localizado na última coluna à direita:

![](/assets/modificaDisciEdit.png)

Após o usuário escolher modificar ou excluir será necessário confirmar a ação clicando em Sim ou Não.

![](/assets/modifiConfirmDiscipl.png)

Caso seja escolhida a opção de exclusão, a mensagem de confirmação e largura do popup serão diferentes, como apresentados anteriormente no código java. Também note a referência às variáveis do nosso bean na parte de exclusão no código xhtml.

Por fim, o registro será atualizado ou removido automaticamente da tabela após a confirmação.

---

Link para download do arquivo disciplinas.xhtml: [https://gist.github.com/quisen/791930ba6c15a23a2dfbb55a0d392d63](https://gist.github.com/quisen/791930ba6c15a23a2dfbb55a0d392d63)

