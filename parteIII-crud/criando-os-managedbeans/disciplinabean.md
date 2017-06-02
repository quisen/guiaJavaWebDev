# Disciplina**Bean.java**

**Atributos** utilizados pela classe:

```java
private List<Disciplina> disciplinas;
private Disciplina disciplina = new Disciplina();
private Disciplina disciplinaSelecionada = new Disciplina();
private String msgConfirmacao = "Tem certeza?";
private int larguraPopupConfirma = 200;
```

* A lista "**disciplinas**" do tipo **Disciplina** será utilizada para preencher a tabela principal com todos os registros, na visualização da página web de gerenciamento desta entidade.
* O objeto "**disciplina**" será utilizado na inserção de novos registros.
* O objeto "**disciplinaSelecionada**" será utilizado para armazenar o registro da tabela que deseja-se fazer alguma alteração.

Método que **popula** a **lista** de disciplinas com registros **atualizados**:

```java
public void atualizaListaDisciplinas() {
    disciplinas = EManager.getInstance().createNamedQuery("Disciplina.findAll").getResultList();
}
```

Nesta linha de código, fazemos uma requisição ao **EntityManager** para retornar todos as **disciplinas** **cadastradas**.

\(**Obs**: NamedQuery Disciplina.findAll = SELECT \* FROM Disciplina\).

Método que trata a **inserção** de novas disciplinas na tabela:

```java
public void novoCadastro() {
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().persist(this.disciplina);
        EManager.getInstance().getTransaction().commit();
        this.disciplina = new Disciplina();
        atualizaListaDisciplinas();
}
```

Neste método, utilizamos a instância do objeto disciplina criado no início da classe \(this.disciplina\) como objeto base para **receber** as informações da **nova** disciplina.

Então iniciamos uma **transação** com o banco de dados através da **unidade de persistência \(gerenciada pelo EntityManager\)** e "**persistimos**" a entidade disciplina - Isto indica que esta entidade será inserida como um novo registro. Por fim, é feito o "**commit**", que **grava** as mudanças realizadas durante a transação.

Após a inserção, re-instanciamos nosso objeto disciplina para prevenir que existam dados **remanescentes** de transações anteriores, e então atualizamos a lista - para obter o novo registro junto com os demais.

Método que trata a **modificação** de registros de disciplinas na tabela:

```java
public void modificaDisciplina() {
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().merge(this.disciplinaSelecionada);
        EManager.getInstance().getTransaction().commit();
        atualizaListaDisciplinas();
}
```

Utilizamos a mesma metodologia empregada na parte de inserção de disciplinas, porém aqui ocorre uma mudança durante a transação - chamamos o método "**merge**", passando para ele o objeto "**disciplinaSelecionada"**, que segura os valores do registro clicado na tabela durante a exibição na página web. Este método indica para o EntityManager que estaremos fazendo mudanças em registros já existentes.

Método que trata a **remoção** de registros de disciplinas na tabela:

```java
public void deletaDisciplina() {
        List<Matricula> m = EManager.getInstance().createNamedQuery("Matricula.findByDisciplina").setParameter("idDisciplina", this.disciplinaSelecionada.getId()).getResultList();
        EManager.getInstance().getTransaction().begin();
        for (int i = 0; i < m.size(); i++) {
            EManager.getInstance().remove(m.get(i));
        }
        EManager.getInstance().remove(this.disciplinaSelecionada);
        EManager.getInstance().getTransaction().commit();
        atualizaListaDisciplinas();
}
```

Utilizamos aqui o mesmo processo, diferenciando-se dos outros com o método "**remove**", que indica para o EntityManager que estamos apagando um registro da tabela.

**Vale ressaltar que**, todas estas operações ocorrem com base no **id** do registro que deseja-se manipular - sendo na inserção adicionado um id gerado automaticamente pelo banco SQL \(pois utilizamos **auto increment**\), e na modificação e exclusão utilizamos o id do registro já existente.

Método que irá repassar o objeto **Disciplina** escolhido na tabela \(pela página web\) para nosso objeto instanciado anteriormente.

```java
public void enviaDisciplina(Disciplina a) {
    this.disciplinaSelecionada = a;
    List<Matricula> m = EManager.getInstance().createNamedQuery("Matricula.findByDisciplina").setParameter("idDisciplina", this.disciplinaSelecionada.getId()).getResultList();
    if (m.size() > 0) {
        this.msgConfirmacao = "Tem certeza? A remoção da disciplina acarretará na exclusão de todas as respectivas matrículas.";
        this.larguraPopupConfirma = 400;
    } else {
        this.msgConfirmacao = "Tem certeza?";
        this.larguraPopupConfirma = 200;
        }
}
```

Desta forma recebemos o registro completo, incluindo seu **id.**

Método que inicializa a tabela preenchida com os registros, de forma automática ao carregar a página:

```java
@PostConstruct
public void init() {
    atualizaListaDisciplinas();
}
```

Aqui apenas utilizamos o método **atualizaListaDisciplinas\(\)** descrito anteriormente, com a anotação **@PostConstruct** \(sua funcionalidade é descrita na seção anterior\).

E então adicionamos todos os **getters **e **setters** \(dica: utilize o atalho Alt+Insert para inserção automática\).

```java
public List<Disciplina> getDisciplinas() {
    return disciplinas;
}

public void setDisciplinas(List<Disciplina> disciplinas) {
    this.disciplinas = disciplinas;
}

public Disciplina getDisciplina() {
    return disciplina;
}

public void setDisciplina(Disciplina disciplina) {
    this.disciplina = disciplina;
}

public Disciplina getDisciplinaSelecionada() {
    return disciplinaSelecionada;
}

public void setDisciplinaSelecionada(Disciplina disciplinaSelecionada) {
    this.disciplinaSelecionada = disciplinaSelecionada;
}

public String getMsgConfirmacao() {
    return msgConfirmacao;
}

public void setMsgConfirmacao(String msgConfirmacao) {
    this.msgConfirmacao = msgConfirmacao;
}

public int getLarguraPopupConfirma() {
    return larguraPopupConfirma;
}

public void setLarguraPopupConfirma(int larguraPopupConfirma) {
    this.larguraPopupConfirma = larguraPopupConfirma;
}
```

Link para download da classe DisciplinaBean.java: [https://gist.github.com/quisen/f8933141f19ffdb2a80a0c3551b5a863](https://gist.github.com/quisen/f8933141f19ffdb2a80a0c3551b5a863)

