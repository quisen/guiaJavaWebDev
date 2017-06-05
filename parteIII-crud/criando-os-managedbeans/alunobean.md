# AlunoBean.java

**Atributos** utilizados pela classe:

```java
private List<Aluno> alunos;
private Aluno aluno = new Aluno();
private Aluno alunoSelecionado = new Aluno();
private String msgConfirmacao = "Tem certeza?";
private int larguraPopupConfirma = 200;
```

* A lista "**alunos**" do tipo **Aluno** será utilizada para preencher a tabela principal com todos os registros, na visualização da página web de gerenciamento desta entidade.
* O objeto "**aluno**" será utilizado na inserção de novos registros.
* O objeto "**alunoSelecionado**" será utilizado para armazenar o registro da tabela que deseja-se fazer alguma alteração.
* A String "**msgConfirmacao**" seta a mensagem padrão para a exclusão de alunos \(será diferente quando o aluno estiver matriculado em alguma disciplina\).

Método que **popula** a **lista** de alunos com registros **atualizados**:

```java
public void atualizaListaAlunos() {
    alunos = EManager.getInstance().createNamedQuery("Aluno.findAll").getResultList();
}
```

Nesta linha de código, fazemos uma requisição ao **EntityManager** para retornar todos os **alunos** **cadastrados**.

\(**Obs**: NamedQuery Aluno.findAll = SELECT \* FROM Aluno\).

Método que trata a **inserção** de novos alunos na tabela:

```java
public void novoCadastro() {
    this.aluno.setMatricula(1000000 + new Random().nextInt(9999999 - 1000000 + 1));
    EManager.getInstance().getTransaction().begin();
    EManager.getInstance().persist(this.aluno);
    EManager.getInstance().getTransaction().commit();
    this.aluno = new Aluno();
    atualizaListaAlunos();
}
```

Neste método, utilizamos a instância do objeto aluno criado no início da classe \(this.aluno\) como objeto base para **receber** as informações do **novo** aluno \(com exceção da matrícula, todos os outros atributos serão preenchidos automaticamente através da página web JSF criada posteriormente\).

Geramos a matrícula de forma **aleatória**, com valores entre 1000000 e 9999999 - estes valores foram escolhidos arbitrariamente para este projeto, porém poderiam ser setados qualquer tipos de valores inteiros.

Então iniciamos uma **transação** com o banco de dados através da **unidade de persistência \(gerenciada pelo EntityManager\)** e "**persistimos**" a entidade aluno - Isto indica que esta entidade será inserida como um novo registro. Por fim, é feito o "**commit**", que **grava** as mudanças realizadas durante a transação.

Após a inserção, re-instanciamos nosso objeto aluno para prevenir que existam dados **remanescentes** de transações anteriores, e então atualizamos a lista - para obter o novo registro junto com os demais.

Método que trata a **modificação** de registros de alunos na tabela:

```java
public void modificaAluno() {
    EManager.getInstance().getTransaction().begin();
    EManager.getInstance().merge(this.alunoSelecionado);
    EManager.getInstance().getTransaction().commit();
    atualizaListaAlunos();
}
```

Utilizamos a mesma metodologia empregada na parte de inserção de alunos, porém aqui ocorre uma mudança durante a transação - chamamos o método "**merge**", passando para ele o objeto "**alunoSelecionado"**, que segura os valores do registro clicado na tabela durante a exibição na página web. Este método indica para o EntityManager que estaremos fazendo mudanças em registros já existentes.

Método que trata a **remoção** de registros de alunos na tabela:

```java
    public void deletaAluno() {
        List<Matricula> m = EManager.getInstance().createNamedQuery("Matricula.findByAluno").setParameter("idAluno", this.alunoSelecionado.getId()).getResultList();
        EManager.getInstance().getTransaction().begin();
        for (int i = 0; i < m.size(); i++) {
            EManager.getInstance().remove(m.get(i));
        }
        EManager.getInstance().remove(this.alunoSelecionado);
        EManager.getInstance().getTransaction().commit();
        atualizaListaAlunos();
    }
```

Utilizamos aqui o mesmo processo, diferenciando-se dos outros com o método "**remove**", que indica para o EntityManager que estamos apagando um registro da tabela.

**Vale ressaltar que**, todas estas operações ocorrem com base no **id** do registro que deseja-se manipular - sendo na inserção adicionado um id gerado automaticamente pelo banco SQL \(pois utilizamos **auto increment**\), e na modificação e exclusão utilizamos o id do registro já existente.

Método que irá repassar o objeto **Aluno** escolhido na tabela \(pela página web\) para nosso objeto instanciado anteriormente.

```java
public void enviaAluno(Aluno a) {
    this.alunoSelecionado = a;
    List<Matricula> m = EManager.getInstance().createNamedQuery("Matricula.findByAluno").setParameter("idAluno", this.alunoSelecionado.getId()).getResultList();
    if (m.size() > 0) {
        this.msgConfirmacao = "Tem certeza? A remoção do(a) aluno(a) acarretará na exclusão de todas as respectivas matrículas.";
        this.larguraPopupConfirma = 400;
    } else {
    this.msgConfirmacao = "Tem certeza?";
    this.larguraPopupConfirma = 200;
    }
}
```

Desta forma recebemos o registro completo, incluindo seu **id.**

Também verificamos se o aluno está matriculado em alguma disciplina, para atualizar a mensagem de confirmação de exclusão \(caso clicado\) para alertar o usuário da consequência de tal ação - adicionalmente modificamos a largura do pop-up, para se adequar ao tamanho do novo texto.

Método que inicializa a tabela preenchida com os registros, de forma automática ao carregar a página:

```java
@PostConstruct
public void init() {
    atualizaListaAlunos();
}
```

Aqui apenas utilizamos o método **atualizaListaAlunos\(\)** descrito anteriormente, com a anotação **@PostConstruct** \(sua funcionalidade é descrita na seção anterior\).

E então adicionamos todos os **getters **e **setters** \(dica: utilize o atalho Alt+Insert para inserção automática\).

```java
    public List<Aluno> getAlunos() {
        return alunos;
    }

    public void setAlunos(List<Aluno> alunos) {
        this.alunos = alunos;
    }

    public Aluno getAluno() {
        return aluno;
    }

    public void setAluno(Aluno aluno) {
        this.aluno = aluno;
    }

    public Aluno getAlunoSelecionado() {
        return alunoSelecionado;
    }

    public void setAlunoSelecionado(Aluno alunoSelecionado) {
        this.alunoSelecionado = alunoSelecionado;
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

Link para download da classe AlunoBean.java: [https://gist.github.com/quisen/42eadb84505bc6aa8259c70ea66fee74](https://gist.github.com/quisen/42eadb84505bc6aa8259c70ea66fee74)

