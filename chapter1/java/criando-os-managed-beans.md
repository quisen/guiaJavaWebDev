![](/assets/criando bean.png)



A classe referenciada com a annotation "@ManagedBean" atua como um intermediador entre a página web \(criada na próxima sessão do manual\) e os métodos java que interagem com as classes de entidade e o banco de dados.

Este é um exemplo básico para o Managed Bean dos alunos. No código seguinte, é declarada uma lista do tipo aluno que tem a finalidade de armazenar todos os alunos resultantes da query "SELECT \* FROM Aluno;"



```java
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
}
```

A annotation "@**PostConstruct" **é usada previamente em um método que precisa ser executado depois que as dependências da classe são carregadas, de forma a fazer algum tipo de inicialização no código. A annotation deve ser chamada antes da classe ser executada.

Desta forma, resumidamente, o método "init\(\)" será executado após a classe ser chamada \(inicializada\) por alguma página web.

Com o código atual, o método "atualizaListaAlunos\(\)" é chamado de forma a fazer uma query no banco que retorna todos os alunos, como mencionado acima. Vale ressaltar que a "NamedQuery" utilizada \("Classe.findAll"\) é criada automaticamente pelo Netbeans, e pode ser verificado no "Aluno.java".

