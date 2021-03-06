# Inserção de Registros

Agora veremos um exemplo de como executar ações assíncronas com componentes web via primefaces/jsf.

Para realizar uma** Ação via botão** adicionamos um **commandButton** na nossa página web:

```xhtml
<p:commandButton value="Cadastrar novo Aluno" process="@this" update=":cadastro:novo-aluno-dialog"
                             oncomplete="PF('novo-aluno-widget').show()" />
```

O atributo **"update"** indica que irá ocorrer alguma mudança visual na página, sendo que a parte ":cadastro:" é uma referência à outro form \(veja em seguida\).

O atributo **"oncomplete"** define uma ação que irá ser realizada quando o botão for clicado.

O atributo **"pop-up"** serve para abrir uma caixa de diálogo com os campos para cadastro:

```xhtml
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

Nesta parte, criamos um novo "**form**", essa tag indica basicamente a entrada e saída de dados em relação ao java, preste atenção em seu id \(id="cadastro"\), pois precisaremos disso para referenciar componentes entre si em forms diferentes, como visto no componente do botão acima.

Partes relevantes do código:

* O **panelGrid** é um componente que estabelece uma área pré-determinada com uma organização específica, neste caso - duas colunas, sendo a primeira um label \(etiqueta, texto indicativo\), e a segunda o valor do campo.
* Na entrada de dados \(**inputText**\), o atributo "value" indica qual variável no código java será linkado com o campo no xhtml respectivo. Neste caso, referenciamos nossa classe alunoBean, e dentro da classe teremos um objeto chamado "aluno", então acessamos suas variáveis utilizando o ponto \(criaremos este objeto e os métodos de criação logo em seguida\).
* Criamos um botão que executa uma ação \(**action**=""\), neste caso, executa o método novoCadastro do nosso código java.
* O botão possui o atributo "**update**" que realizará uma mudança visual na tabela, pois ao adicionar o novo usuário queremos que ele seja listado imediatamente junto com os demais.
* A tag "**defaultCommand**" indica o id do componente que será executado ao pressionar a tecla Enter.

E então adicionamos o método novoCadastro\(\) no nosso managed bean, bem como o objeto "aluno", que será utilizado para fazer o link dos campos de texto para cadastro.



Visualização do pop-up:

![](/assets/popup.PNG)

Preenchimento do pop-up:

![](/assets/preenche.PNG)

Novo aluno listado e cadastrado após confirmação do preenchimento no pop-up:

![](/assets/listado.PNG)

