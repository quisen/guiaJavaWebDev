# Criação de Páginas Web

Automaticamente um index.html é criado dentro das Páginas Web do seu projeto, você deve excluí-lo.

![](/assets/excluir index gerado.png)

Agora clique com o botão direito do mouse sobre o seu projeto e selecione Propriedades. Dentro das Categorias procure por Frameworks, em seguida clique para adicionar um framework e escolha a opção JavaServer Faces.

![](/assets/adicionando jsf framework.png)

Clique em **OK** e finalize.

**Atenção**: **não** **marque** a opção PrimeFaces nesta etapa, pois iremos adicioná-lo de outra maneira.

![](/assets/jsf click ok.png)

Agora, navegue até o arquivo **pom.xml**, localizado dentro de Arquivos do Projeto, nele você deverá adicionar as dependências do seu projeto.

A primeira que iremos adicionar é a dependência da biblioteca do **PrimeFaces** versão 6.1.

![](/assets/editar pom.xml add prime.png)

```XML
<dependency>
    <groupId>org.primefaces</groupId>
    <artifactId>primefaces</artifactId>
    <version>6.1</version>
</dependency>
```

Agora clique no ícone de Clean and Build para garantir que o Maven baixe todas as dependências corretamente.

![](/assets/rebuild.png)

