# Gerando Classes de Entidade do Banco de Dados

Para gerar clases de entidade a partir  do banco de dados você deverá clicar  com botão direito do mouse sobre o projeto, expandir a opção **Novo** e selecionar **Outros** \(ao final da lista\).

![](/assets/1.png)

Do lado esquerdo da janela clique na Categoria: **Persistência** e no lado direito, em Tipos de Arquivos, selecione **Classes de Entidade do Banco de Dados. **Clique em **Próximo**.

![](/assets/2.png)

Agora selecione o campo vazio **Fonte de Dados **e escolha a última opção **Nova fonte de dados...**

![](/assets/importando.png)

Primeiro escolha um **nome** para a JNDI e selecione a opção **Nova Conexão de Banco de Dados...**

![](/assets/jndi.png)

Escolha o Driver **MySQL.**

![](/assets/mysql connector.png)

Preencha o nome do Banco de Dados e indique também o nome do **usuário** e **senha** do Banco de Dados \(previamente definidos\).

Você pode clicar no botão **Testar** **Conexão** para verificar se os campos foram preenchidos corretamente.

Clique em **Próximo**.

![](/assets/conexao com banco.png)

Agora que a conexão foi estabelecida entre o NetBeans e o SQL Server, clique no botão **Adicionar Tudo &gt;&gt; ** ou selecione individualmente as tabelas desejadas. Clique em **Finalizar**. 

![](/assets/entidades.png)

Depois disso você perceberá que as classes estarão dentro do seu pacote.

![](/assets/entidades criadas.png)

