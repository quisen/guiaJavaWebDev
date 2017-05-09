# Introdução aos ManagedBeans

Agora crie uma nova classe java chamada** AlunoBean.**

![](/assets/criando bean.png)

A classe referenciada com a annotation "@ManagedBean" atua como um intermediador entre a página web \(criada na próxima sessão do manual\) e os métodos java que interagem com as classes de entidade e o banco de dados.

Este é um exemplo básico para o Managed Bean dos alunos. No código abaixo, é declarada uma lista do tipo aluno que tem a finalidade de armazenar todos os alunos resultantes da query "SELECT \* FROM Aluno;"

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

A annotation "@**PostConstruct" **é usada previamente em um método que precisa ser executado depois que as dependências da classe são carregadas, de forma a fazer algum tipo de inicialização no código. A annotation deve ser chamada antes da classe ser executada.

Desta forma, resumidamente, o método "init\(\)" será executado após a classe ser chamada \(inicializada\) por alguma página web.

Com o código atual, o método "atualizaListaAlunos\(\)" é chamado de forma a fazer uma query no banco que retorna todos os alunos, como mencionado acima. Vale ressaltar que a "NamedQuery" utilizada \("Classe.findAll"\) é criada automaticamente pelo Netbeans, e pode ser verificado no "Aluno.java".

Também vale ressaltar que, para todo objeto, atributo ou variável criado dentro do ManagedBean deve possuir seus respectivos "getters e setters", como da seguinte forma:

```java
public List<Aluno> getAlunos() {
    return alunos;
}

public void setAlunos(List<Aluno> alunos) {
    this.alunos = alunos;
}
```

Isso é necessário para que a página web possa "enxergar" as variáveis, e assim acessá-las.

