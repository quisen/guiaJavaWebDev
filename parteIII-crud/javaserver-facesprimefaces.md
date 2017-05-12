# Criação de Páginas Web

Automaticamente um index.html é criado dentro das Páginas Web do seu projeto, você deve excluí-lo.

![](/assets/excluir index gerado.png)

Agora clique com o botão direito do mouse sobre o seu projeto no canto esquerdo da tela e vá em Propriedades, dentro das Categorias procure por Frameworks, depois disso clique para adicionar um framework e escolha pela opção JavaServer Faces.

![](/assets/adicionando jsf framework.png)

Clique em OK e finalize.

**Atenção**: **não** **marque** a opção PrimeFaces nesta etapa, pois iremos adicioná-lo de outra maneira.

![](/assets/jsf click ok.png)

Em seguida, navegue até o arquivo **pom.xml **dentro dele você deverá adicionar as dependências do seu projeto. A primeira que iremos adicionar é a biblioteca do **PrimeFaces** versão 6.1.

![](/assets/editar pom.xml add prime.png)

```java
<dependency>
    <groupId>org.primefaces</groupId>
    <artifactId>primefaces</artifactId>
    <version>6.1</version>
</dependency>
```

Agora clique no ícone de Clean and Build para garantir que o Maven baixe todas as dependências corretamente.

![](/assets/rebuild.png)

Agora um novo arquivo chamado **index**.**xhtml** foi criado pelo Netbeans.

![](/assets/index novo.png)

