# Edição e Exclusão de Registros

Para incluir a funcionalidade de edição ou exclusão de registros, faremos uma modificação no código da tabela dos alunos e também adicionaremos uma nova janela \(pop-up\) com as informações do aluno selecionado com campos modificáveis.

Primeiramente vamos relembrar como os dados dos alunos são acessados na tabela:

```
...
<p:dataTable id="tabela-alunos" var="aluno" value="#{alunoBean.alunos}">
                        <p:column headerText="Id">
                            <h:outputText value="#{aluno.id}" />
                        </p:column>
           ...
```

Como pode-se perceber, para cada linha da tabela temos um **objeto "aluno"**. O método que iremos utilizar, irá operar **individualmente** para cada registro da tabela.

Vamos criar um **novo objeto** em nosso código java, que irá representar o **aluno escolhido na tabela** para ser modificado ou excluído:

```java
private Aluno alunoSelecionado = new Aluno();
```

Não se esqueça de criar o getter e o setter para este objeto.

E então o método que irá **repassar** o objeto \(**aluno**\) **escolhido na tabela** para o **objeto previamente criado**, onde serão feitas as modificações, será mais fácil de entender quando todo o código estiver pronto:

```java
public void enviaAluno(Aluno a) {
    this.alunoSelecionado = a;
}
```

Com isto, podemos escrever o código que irá efetivamente **inserir no banco** o aluno com **dados modificados** - e também o código de **exclusão do registro**:

```java
public void modificaAluno() {
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().merge(this.alunoSelecionado);
        EManager.getInstance().getTransaction().commit();
        atualizaListaAlunos();
}

public void deletaAluno() {
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().remove(this.alunoSelecionado);
        EManager.getInstance().getTransaction().commit();
        atualizaListaAlunos();
}
```

Assim, podemos seguir em frente para incluir as funcionalidades em nossa tabela.

Vamos criar um novo form, contendo um novo pop-up que irá mostrar os dados do aluno selecionado:

```xhtml
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

Aqui temos dois botões que executam os métodos java que criamos anteriormente, um para modificação e outro para exclusão.

E então criamos uma nova coluna com um botão na nossa tabela original:

```xhtml
<p:column style="width:40px;text-align: center">
      <p:commandButton update=":modifica:modifica-aluno-panel" oncomplete="PF('modifica-aluno-widget').show()" icon="ui-icon-arrow-4-diag"
              title="View" action="#{alunoBean.enviaAluno(aluno)}" />
</p:column>
```

Este botão irá abrir o pop-up com as informações do aluno selecionado, e ao mesmo tempo acionar o método "enviaAluno" que repassa os dados do registro clicado para um objeto volátil \(sem informação fixa\) "alunoSelecionado" - que receberá o tratamento adequado.



Visualização do botão em nova coluna da tabela:![](/assets/novobotao expandir.png)

Visualização da janela pop-up ao clicar no botão da tabela:

![](/assets/popup modifica.png)

