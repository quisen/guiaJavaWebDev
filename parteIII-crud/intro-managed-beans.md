# Introdução aos ManagedBeans

ManagedBeans são classes referenciadas com a annotation "**@ManagedBean**", que atuam como **intermediadores** entre as **páginas web** \(abordadas na próxima sessão\) e os **métodos java** que interagem com as classes de entidade e com o banco de dados.

A seguir veremos um exemplo simples de criação e funcionamento de um ManagedBean para a tabela Aluno.

![](/assets/alunobean.png)

No código abaixo, é declarada uma lista do tipo aluno que tem a finalidade de armazenar todos os alunos resultantes da query "SELECT \* FROM Aluno;"

\(Esta query é representada por "Aluno.findAll" - pode ser observada na classe gerada Aluno.java\)

Abaixo temos um exemplo básico para o Managed Bean de alunos.

```java
import java.io.Serializable;
import java.util.List;
import javax.faces.bean.ManagedBean;
import javax.annotation.PostConstruct;
import javax.faces.bean.ViewScoped;

@ManagedBean
@ViewScoped
public class AlunoBean implements Serializable {

    private List<Aluno> alunos;

    @PostConstruct
    public void init() {
        atualizaListaAlunos();
    }

    public void atualizaListaAlunos() {
        alunos = EManager.getInstance().createNamedQuery("Aluno.findAll").getResultList();
    }

    public List<Aluno> getAlunos() {
        return alunos;
    }

    public void setAlunos(List<Aluno> alunos) {
        this.alunos = alunos;
    }

}
```

A annotation "@**PostConstruct" **é usada previamente em um método que precisa ser executado depois que as dependências da classe são carregadas, de forma a fazer algum tipo de inicialização no código. Essa annotation deve ser chamada antes da classe ser executada.

Desta forma, resumidamente, o método "init\(\)" será executado após a classe ser chamada \(inicializada\) por alguma página web.

Com o código atual, o método "atualizaListaAlunos\(\)" é chamado de modo a requisitar uma query no banco que retorna todos os registros de alunos.

Note que a "NamedQuery" utilizada \("Classe.findAll"\) é criada automaticamente pelo NetBeans, e pode ser encontrada dentro da classe "Aluno.java".

Também vale ressaltar que todo objeto, atributo ou variável criado dentro do ManagedBean deve possuir seus respectivos "getters e setters", conforme o trecho de código abaixo.

```java
public List<Aluno> getAlunos() {
    return alunos;
}

public void setAlunos(List<Aluno> alunos) {
    this.alunos = alunos;
}
```

Isso é **necessário** para que a **página** **web** possa "**enxergar**" as variáveis, e assim acessá-las.

