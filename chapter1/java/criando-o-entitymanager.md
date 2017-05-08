# Criando o EntityManager e Configurando a Unidade de Persistência Gerada pelas Classes de Entidade do Banco de Dados

![](/assets/1 crie a classe.png)

![](/assets/2 procurar persistence.xml.png)

![](/assets/3 persistence gui.png)

![](/assets/4 persistence gui desejado.png)

![](/assets/5 emanager configurado.png)

```java
import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

public class EManager implements java.io.Serializable{

    private static final EntityManagerFactory emf = Persistence.createEntityManagerFactory("UniversidadePU");
    private static EntityManager em = emf.createEntityManager();

    public EManager() {
    }

    public static EntityManager getInstance() {
        if (em == null) {
            em = emf.createEntityManager();
        }
        return em;
    }

}
```



