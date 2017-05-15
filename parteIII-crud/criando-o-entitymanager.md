# Criando o EntityManager e Configurando a Unidade de Persistência

Agora que já temos as classes de entidades geradas a partir do banco, podemos criar o nosso EntityManager.

O **EntityManager** é uma ferramenta utilizada para **interagir com o contexto de persistência** - sendo este um conjunto de instâncias de entidades. Com esta associação, podemos realizar várias ações com o nosso banco de dados, como **criação, exclusão e alteração** de registros de entidades, bem como **buscas (queries)** baseadas em atributos e campos identificadores.

Todas as entidades que podem ser **gerenciadas** por um EntityManager são descritas utilizando a **unidade de persistência** - esta define o conjunto de **classes** que têm algum tipo de **interação com o banco**.

Primeiramente você deve criar uma nova classe java dentro do seu pacote e configurá-la para ser o seu entity manager.

![](/assets/1 crie a classe.png)

O arquivo **persistence**.**xml** foi criado automaticamente pelo Netbeans durante a geração das classes de entidade a partir do banco, esse arquivo guarda todas as informações sobre a camada de persistência que tem uma relação direta com o seu banco de dados.

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
