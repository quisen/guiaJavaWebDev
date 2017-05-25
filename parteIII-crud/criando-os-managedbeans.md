# Criando os ManagedBeans

Primeiramente iremos atualizar o nosso AlunoBean para, além de possibilitar a visualização de registros na tabela, também realização de operações de **inserção**, **edição** e **exclusão** no banco de dados.

**AlunoBean.java**

**Atributos** utilizados pela classe:

```
private List<Aluno> alunos;
private Aluno aluno = new Aluno();
private Aluno alunoSelecionado = new Aluno();
```

A lista "**alunos**" do tipo **Aluno** será utilizada para preencher a tabela principal com todos os registros, na visualização da página web de gerenciamento desta entidade.

O objeto "**aluno**" será utilizado na inserção de novos registros.

O objeto "**alunoSelecionado**" será utilizado para armazenar o registro da tabela que deseja-se fazer alguma alteração.



Método que **popula** a **lista** de alunos com registros **atualizados**:

```
public void atualizaListaAlunos() {
    alunos = EManager.getInstance().createNamedQuery("Aluno.findAll").getResultList();
}
```

Nesta linha de código, fazemos uma requisição ao **EntityManager** para retornar todos os **alunos** **cadastrados**.

\(**Obs**: NamedQuery Aluno.findAll = SELECT \* FROM Aluno\).



Método que trata a **inserção** de novos alunos na tabela:

```
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

```
public void modificaAluno() {
    EManager.getInstance().getTransaction().begin();
    EManager.getInstance().merge(this.alunoSelecionado);
    EManager.getInstance().getTransaction().commit();
    atualizaListaAlunos();
}
```

Utilizamos a mesma metodologia empregada na parte de inserção de alunos, porém aqui ocorre uma mudança durante a transação - chamamos o método "**merge**", passando para ele o objeto "**alunoSelecionado"**, que segura os valores do registro clicado na tabela durante a exibição na página web. Este método indica para o EntityManager que estaremos fazendo mudanças em registros já existentes.



Método que trata a **remoção** de registros de alunos na tabela:

```
public void deletaAluno() {
    EManager.getInstance().getTransaction().begin();
    EManager.getInstance().remove(this.alunoSelecionado);    
    EManager.getInstance().getTransaction().commit();
    atualizaListaAlunos();
}
```

Utilizamos aqui o mesmo processo, diferenciando-se dos outros com o método "**remove**", que indica para o EntityManager que estamos apagando um registro da tabela.

**Vale ressaltar que**, todas estas operações ocorrem com base no **id** do registro que deseja-se manipular - sendo na inserção adicionado um id gerado automaticamente pelo banco SQL \(pois utilizamos **auto increment**\), e na modificação e exclusão utilizamos o id do registro já existente.



Método que inicializa a tabela preenchida com os registros, de forma automática ao carregar a página:

```
@PostConstruct
public void init() {
    atualizaListaAlunos();
}
```





```java

    



    public void enviaAluno(Aluno a) {
        this.alunoSelecionado = a;
    }

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

}
```

Repita o processo para o DisciplinaBean.

**DisciplinaBean.java**

```java
package br.edu.utfpr.universidade;

import java.io.Serializable;
import java.util.List;
import java.util.Random;
import javax.faces.bean.ManagedBean;
import javax.annotation.PostConstruct;
import javax.faces.bean.ViewScoped;

@ManagedBean
@ViewScoped
public class DisciplinaBean implements Serializable {

    private List<Disciplina> disciplinas;
    private Disciplina disciplina = new Disciplina();
    private Disciplina disciplinaSelecionada = new Disciplina();

    @PostConstruct
    public void init() {
        atualizaListaDisciplinas();
    }

    public void atualizaListaDisciplinas() {
        disciplinas = EManager.getInstance().createNamedQuery("Disciplina.findAll").getResultList();
    }

    public void modificaDisciplina() {
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().merge(this.disciplinaSelecionada);
        EManager.getInstance().getTransaction().commit();
        atualizaListaDisciplinas();
    }

    public void deletaDisciplina() {
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().remove(this.disciplinaSelecionada);
        EManager.getInstance().getTransaction().commit();
        atualizaListaDisciplinas();
    }

    public void enviaDisciplina(Disciplina a) {
        this.disciplinaSelecionada = a;
    }

    public void novoCadastro() {
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().persist(this.disciplina);
        EManager.getInstance().getTransaction().commit();
        this.disciplina = new Disciplina();
        atualizaListaDisciplinas();
    }

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



}
```

Por fim iremos criar o MatriculaBean para o gerenciamento da tabela Matricula que possui relacionamento com as outras 2 tabelas.

**MatriculaBean.java**

```java
package br.edu.utfpr.universidade;

import java.io.Serializable;
import java.util.ArrayList;
import java.util.List;
import javax.annotation.PostConstruct;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.ViewScoped;

@ManagedBean
@ViewScoped
public class MatriculaBean implements Serializable {

    private List<Aluno> alunos;
    private Aluno alunoSelecionado = new Aluno();
    private Disciplina disciplinaSelecionada = new Disciplina();
    private List<Disciplina> disciplinas;
    private List<Matricula> matriculas;
    private List<matriculaMin> matriculasMin = new ArrayList<>();
    private Matricula matriculaSelecionada = new Matricula();
    private matriculaMin matriculaMinSelecionada = new matriculaMin();

    @PostConstruct
    public void init() {
        atualizaTodos();
    }

    public void novaMatricula() {
        Matricula matricula = new Matricula();
        matricula.setIdAluno(alunoSelecionado);
        matricula.setIdDisciplina(disciplinaSelecionada);
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().persist(matricula);
        EManager.getInstance().getTransaction().commit();
        this.alunoSelecionado = new Aluno();
        this.disciplinaSelecionada = new Disciplina();
        atualizaTodos();
    }

    public void agrupaDadosPorId() {
        this.matriculasMin = new ArrayList<>();

        for (int i = 0; i < matriculas.size(); i++) {
            matriculaMin mat = new matriculaMin();
            mat.setId(matriculas.get(i).getId());
            mat.setAluno((Aluno) EManager.getInstance().createNamedQuery("Aluno.findById").setParameter("id", matriculas.get(i).getIdAluno().getId()).getSingleResult());
            mat.setDisciplina((Disciplina) EManager.getInstance().createNamedQuery("Disciplina.findById").setParameter("id", matriculas.get(i).getIdDisciplina().getId()).getSingleResult());

            matriculasMin.add(mat);
        }
    }

    public void atualizaTodos() {
        atualizaListaAlunos();
        atualizaListaDisciplinas();
        atualizaListaMatriculas();
        agrupaDadosPorId();
    }

    public void enviaMatricula(matriculaMin a) {
        this.matriculaMinSelecionada = a;
    }

    public void deletaMatricula() {
        Matricula matricula = new Matricula();
        matricula.setId(matriculaMinSelecionada.getId());
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().remove(
                EManager.getInstance().find(
                        Matricula.class, matricula.getId()
                )
        );
        EManager.getInstance().getTransaction().commit();
        atualizaTodos();
    }

    public void modificaMatricula() {
        Matricula matricula = new Matricula();
        matricula.setId(matriculaMinSelecionada.getId());
        matricula.setIdAluno(matriculaMinSelecionada.getAluno());
        matricula.setIdDisciplina(matriculaMinSelecionada.getDisciplina());
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().merge(matricula);
        EManager.getInstance().getTransaction().commit();
        atualizaTodos();
    }

    public void atualizaListaAlunos() {
        alunos = EManager.getInstance().createNamedQuery("Aluno.findAll").getResultList();
    }

    public void atualizaListaDisciplinas() {
        disciplinas = EManager.getInstance().createNamedQuery("Disciplina.findAll").getResultList();
    }

    public void atualizaListaMatriculas() {
        matriculas = EManager.getInstance().createNamedQuery("Matricula.findAll").getResultList();
    }

    public List<Aluno> getAlunos() {
        return alunos;
    }

    public void setAlunos(List<Aluno> alunos) {
        this.alunos = alunos;
    }

    public List<Disciplina> getDisciplinas() {
        return disciplinas;
    }

    public void setDisciplinas(List<Disciplina> disciplinas) {
        this.disciplinas = disciplinas;
    }

    public List<Matricula> getMatriculas() {
        return matriculas;
    }

    public void setMatriculas(List<Matricula> matriculas) {
        this.matriculas = matriculas;
    }

    public Matricula getMatriculaSelecionada() {
        return matriculaSelecionada;
    }

    public void setMatriculaSelecionada(Matricula matriculaSelecionada) {
        this.matriculaSelecionada = matriculaSelecionada;
    }

    public List<matriculaMin> getMatriculasMin() {
        return matriculasMin;
    }

    public void setMatriculasMin(List<matriculaMin> matriculasMin) {
        this.matriculasMin = matriculasMin;
    }

    public Aluno getAlunoSelecionado() {
        return alunoSelecionado;
    }

    public void setAlunoSelecionado(Aluno alunoSelecionado) {
        this.alunoSelecionado = alunoSelecionado;
    }

    public Disciplina getDisciplinaSelecionada() {
        return disciplinaSelecionada;
    }

    public void setDisciplinaSelecionada(Disciplina disciplinaSelecionada) {
        this.disciplinaSelecionada = disciplinaSelecionada;
    }

    public matriculaMin getMatriculaMinSelecionada() {
        return matriculaMinSelecionada;
    }

    public void setMatriculaMinSelecionada(matriculaMin matriculaMinSelecionada) {
        this.matriculaMinSelecionada = matriculaMinSelecionada;
    }

    public class matriculaMin {

        int id;
        Aluno aluno;
        Disciplina disciplina;

        public matriculaMin() {
        }

        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }

        public Aluno getAluno() {
            return aluno;
        }

        public void setAluno(Aluno aluno) {
            this.aluno = aluno;
        }

        public Disciplina getDisciplina() {
            return disciplina;
        }

        public void setDisciplina(Disciplina disciplina) {
            this.disciplina = disciplina;
        }

    }

}
```

# Criando Converters

**AlunoConverter.java**

```java
package br.edu.utfpr.universidade;

import javax.faces.component.UIComponent;
import javax.faces.context.FacesContext;
import javax.faces.convert.Converter;
import javax.faces.convert.FacesConverter;

@FacesConverter(value = "alunoConverter")
public class AlunoConverter implements Converter {

    @Override
    public Object getAsObject(FacesContext facesContext, UIComponent uiComponent, String value) {
        if (value != null && !value.isEmpty()) {
            return (Aluno) uiComponent.getAttributes().get(value);
        }
        return null;
    }

    @Override
    public String getAsString(FacesContext facesContext, UIComponent uiComponent, Object value) {
        if (value instanceof Aluno) {
            Aluno entity = (Aluno) value;
            if (entity != null && entity instanceof Aluno && entity.getId() != null) {
                uiComponent.getAttributes().put(entity.getId().toString(), entity);
                return entity.getId().toString();
            }
        }
        return "";
    }
}
```

**DisciplinaConverter.java**

```java
package br.edu.utfpr.universidade;

import javax.faces.component.UIComponent;
import javax.faces.context.FacesContext;
import javax.faces.convert.Converter;
import javax.faces.convert.FacesConverter;

@FacesConverter(value = "disciplinaConverter")
public class DisciplinaConverter implements Converter {

    @Override
    public Object getAsObject(FacesContext facesContext, UIComponent uiComponent, String value) {
        if (value != null && !value.isEmpty()) {
            return (Disciplina) uiComponent.getAttributes().get(value);
        }
        return null;
    }

    @Override
    public String getAsString(FacesContext facesContext, UIComponent uiComponent, Object value) {
        if (value instanceof Disciplina) {
            Disciplina entity = (Disciplina) value;
            if (entity != null && entity instanceof Disciplina && entity.getId() != null) {
                uiComponent.getAttributes().put(entity.getId().toString(), entity);
                return entity.getId().toString();
            }
        }
        return "";
    }
}
```



