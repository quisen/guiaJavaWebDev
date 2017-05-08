# Criando o EntityManager e Configurando a Unidade de Persistência Gerada pelas Classes de Entidade do Banco de Dados

Primeiramente você deve criar uma nova classe java e configurá-la para ser o seu entitiy manager.

![](/assets/1 crie a classe.png)

O arquivo **persistence**.**xml** foi criado automaticamente pelo NetBeans durante a geração das classes de entidade a partir do banco, esse arquivo guarda todas as informações sobre a camada de persistência que tem uma relação direta com o seu banco de dados.

![](/assets/2 procurar persistence.xml.png)

É recomendado que o **nome** **padrão** da Unidade de Persistência seja modificado para facilitar seu entendimento.

![](/assets/3 persistence gui.png)

Marque a opção **Nenhum** para o Modo de Cache Compartilhado.

![](/assets/4 persistence gui desejado.png)

Abaixo está um exemplo de classe java para um entity manager.![](/assets/5 emanager configurado.png)

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



