# Criando os ManagedBeans

Primeiramente iremos atualizar o nosso AlunoBean para, além de possibilitar a visualização de registros na tabela, também realização de operações de **inserção**, **edição** e **exclusão** no banco de dados.

**AlunoBean.java**

**Atributos** utilizados pela classe:

```java
private List<Aluno> alunos;
private Aluno aluno = new Aluno();
private Aluno alunoSelecionado = new Aluno();
```

* A lista "**alunos**" do tipo **Aluno** será utilizada para preencher a tabela principal com todos os registros, na visualização da página web de gerenciamento desta entidade.
* O objeto "**aluno**" será utilizado na inserção de novos registros.
* O objeto "**alunoSelecionado**" será utilizado para armazenar o registro da tabela que deseja-se fazer alguma alteração.

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
    EManager.getInstance().getTransaction().begin();
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
}
```

Desta forma recebemos o registro completo, incluindo seu **id.**

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
```

**Exercício** - tente repetir o mesmo processo utilizado no **AlunoBean** para o **DisciplinaBean **\(com exceção de que não será necessário gerar matrículas\). Caso encontre dificuldades, todas as classes serão disponibilizadas no final desta seção.

Por fim iremos criar o MatriculaBean para o gerenciamento da tabela Matricula que possui relacionamento com as outras 2 tabelas.

**MatriculaBean.java**

Classe aninhada **matriculaMin**:

```java
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
```

Utilizaremos essa classe para armazenar a **representação** de uma **matrícula**, de forma a **encapsular** os objetos **Aluno** e **Disciplina** de fato, ao **invés de apenas os seus IDs** - que é o caso da tabela matrícula **original**.

Atributos utilizados pela classe:

```
private List<Aluno> alunos;
private Aluno alunoSelecionado = new Aluno();
private Disciplina disciplinaSelecionada = new Disciplina();
private List<Disciplina> disciplinas;
private List<Matricula> matriculas;
private List<matriculaMin> matriculasMin = new ArrayList<>();
private Matricula matriculaSelecionada = new Matricula();
private matriculaMin matriculaMinSelecionada = new matriculaMin();
```

* A Lista **alunos** será utilizada para armazenar todos os registros de alunos;
* A Lista **disciplinas** será utilizada para armazenar todos os registros de disciplinas;
* A Lista **matrículas** será utilizada para armazenar todos os registros de matrículas;
* O objeto **alunoSelecionado** representará o aluno escolhido para realizar a inserção de uma nova matrícula;
* Assim como o objeto **disciplinaSelecionada** representará a a disciplina escolhida durante a inserção de uma nova matrícula;
* A Lista **matriculasMin** armazenará todas as matrículas no formato **descrito** **anteriormente**, com a representação da classe matriculaMin;
* O objeto **matriculaMinSelecionada** representará a matrícula escolhida para modificação ou exclusão dentro da tabela da página web.

Método que lista todas as matrículas, juntamente com os alunos e disciplinas relacionadas:

```
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
```

Aqui inicializamos nossa lista **matriculasMin** como um ArrayList - isto é necessário quando preenchemos nossa lista manualmente, não sendo necessário quando fazemos uma requisição direta do EntityManager que retorna uma lista de resultados.

Então percorremos nossa lista de matrículas, preenchendo um novo objeto matriculaMin - a cada iteração, buscamos o ID de cada aluno e disciplina, e fazemos uma query baseada nesse ID para obtermos o objeto respectivo a partir do registro. Desta forma, terminamos com uma lista mais completa \(que possui o identificador da matrícula, o aluno com todos os seus atributos, e também a disciplina com todos os seus atributos, ao invés de apenas o identificador de cada registro\).

Vamos analisar o método utilizado com mais atenção:

Primeiramente estamos com o intuito de inserir no novo objeto instanciado "**matriculaMin mat**"

```
mat.setAluno();
```

Para receber o objeto do banco, fazemos uma **requisição** ao banco utilizando a **NamedQuery Aluno.findById** - para isto, precisamos setar o parâmetro **id** da seguinte forma:

```
EManager.getInstance().createNamedQuery("Aluno.findById").setParameter("id", ???).getSingleResult()
```

Porém, para obter o** id do aluno desejado**, precisamos buscar a partir da lista de **matrículas** que estamos iterando:

```
matriculas.get(i).getIdAluno().getId()
```

E, por fim, precisamos converter o resultado obtido para **Aluno** \(o objeto resultante das queries é um objeto abstrato, porém como sabemos que está sendo retornado um aluno, fazemos a conversão \(Aluno\) do resultado\).

Resultado final:

```
(Aluno) EManager.getInstance().createNamedQuery("Aluno.findById").setParameter("id", matriculas.get(i).getIdAluno().getId()).getSingleResult()
```

Então podemos repetir o processo para as disciplinas.

Métodos utilizados para atualizar as listas de matrículas, alunos e disciplinas:

```
public void atualizaListaAlunos() {
    alunos = EManager.getInstance().createNamedQuery("Aluno.findAll").getResultList();
}

public void atualizaListaDisciplinas() {
    disciplinas = EManager.getInstance().createNamedQuery("Disciplina.findAll").getResultList();
}

public void atualizaListaMatriculas() {
    matriculas = EManager.getInstance().createNamedQuery("Matricula.findAll").getResultList();
}

public void atualizaTodos() {
        atualizaListaAlunos();
        atualizaListaDisciplinas();
        atualizaListaMatriculas();
        agrupaDadosPorId();
}
```

Aqui fazemos uma requisição básica ao banco de modo a receber **todos** os **registros** **atualizados** na forma de lista - e por fim chamando o método **agrupaDadosPorId\(\)** explicado anteriormente, que melhora a **manuseabilidade** dos **dados** **relacionados**.

Método que trata a inserção de novas matrículas:

```
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
```

Aqui, realizamos a **inserção** de uma nova matrícula - instanciamos uma nova matrícula e preenchemos os respectivos **IDs** do aluno e da disciplina, para então persistir os dados no banco. No final do método, **re-instanciamos** os objetos que "seguram" os dados dos objetos escolhidos relacionados para **prevenir** a existência de dados oriundos de **inserções** **antigas**.

Método que trata a modificação de matrículas:

```
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
```

Nesta parte, realizamos o mesmo procedimento do método anterior, porém, ao invés de fazer a inserção de uma nova matrícula utilizando o método **persist\(\)**, utilizamos o método **merge\(\)**, que tem a função de **atualizar** entidades** já existentes**.

Método que trata a remoção de matrículas:

```
public void deletaMatricula() {
        Matricula matricula = new Matricula();
        matricula.setId(matriculaMinSelecionada.getId());
        EManager.getInstance().getTransaction().begin();
        EManager.getInstance().remove(
                EManager.getInstance().find(Matricula.class, matricula.getId())
        );
        EManager.getInstance().getTransaction().commit();
        atualizaTodos();
}
```

Aqui excluímos uma matrícula passando como referência seu **id** - e para isso fazemos outra requisição ao banco utilizando o método **find** e passando como parâmetro a classe de entidade e o respectivo id.

Método que repassa a matrícula escolhida na tabela \(pela página web\) para nosso objeto já instanciado na classe:

```
public void enviaMatricula(matriculaMin a) {
    this.matriculaMinSelecionada = a;
}
```

Método que atualiza todos os registros das listas, atualizando também a visualização da tabela na nossa página web.

```
@PostConstruct
public void init() {
    atualizaTodos();
}
```

Getters e Setters:

```java
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
```

# Criando Converters

Para a utilização de um componente do primefaces que exibe uma **lista** de **entidades** **selecionáveis** \(para a seleção de alunos e disciplinas para a matrícula, por exemplo\), precisamos implementar um **Converter** para **cada** **classe** envolvida.

Em nossa utilização do java em conjunto com o JSF, um converter é um componente que permite o **encapsulamento** de um ou mais objetos em uma **representação** via **texto** \(String\) - não sendo esta a visualização que o **usuário** terá, mas sim uma representação **interna** que permite por exemplo, saber qual dos objetos de uma lista foi **selecionada**.

A seguir temos os converters que utilizaremos em nosso projeto.

Eles estão descritos em um formato **base**, que pode ser modificado para funcionar com qualquer outra classe/entidade de qualquer outro projeto.

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



