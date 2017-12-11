# Gerando Classes de Entidade do Banco de Dados

**Entidades** em java são objetos de **domínio de persistência** - são utilizadas para representar **tabelas em um banco de dados** relacional. **Cada instância** de uma entidade corresponde a **uma linha** \(registros\) da tabela.

O estado de persistência de uma entidade é representado por meio de campos ou propriedades persistentes - estes utilizam **anotações de mapeamento** para "vincular" entidades e relacionamentos com os dados relacionais presentes no banco de dados original.

Iremos utilizar a anotação **@Entity** para indicar que uma classe é uma entidade.

Para gerar clases de entidade a partir  do banco de dados você deverá clicar  com botão direito do mouse sobre o projeto, expandir a opção **Novo** e selecionar **Outros** \(ao final da lista\).

![](/assets/1.png)

Do lado esquerdo da janela clique na Categoria: **Persistência** e no lado direito, em Tipos de Arquivos, selecione **Classes de Entidade do Banco de Dados. **Clique em **Próximo**.

![](/assets/2.png)

Agora selecione o campo vazio **Fonte de Dados **e escolha a última opção **Nova fonte de dados...**

![](/assets/importando.png)

Primeiro escolha um **nome** para a JNDI e selecione a opção **Nova Conexão de Banco de Dados...**  
A JNDI - Java Naming and Directory Interface é uma API permite que aplicações cliente descubram e obtenham dados ou objetos através de um nome

![](/assets/jndi.png)

Escolha o Driver **MySQL.**

![](/assets/mysql connector.png)

Preencha o nome do banco que estamos utilizando - neste caso, de acordo com nosso script \(**Parte II - Criando seu Banco de Dados**\), e indique também o nome do **usuário** e **senha** do Banco de Dados, estas informações foram definidas durante a instalação do MySQL, de acordo com a **Parte I** do guia.

Você pode clicar no botão **Testar** **Conexão** para verificar se os campos foram preenchidos corretamente.

Clique em **Próximo**.

![](/assets/conexao com banco.png)

Agora que a conexão foi estabelecida entre o NetBeans e o SQL Server, clique no botão **Adicionar Tudo &gt;&gt; ** ou selecione individualmente as tabelas desejadas. Clique em **Finalizar**.

![](/assets/Captura de tela de 2017-05-24 15:28:10.png)

Depois disso você perceberá que as classes serão criadas dentro do seu pacote.

![](/assets/classesentidadegerada.png)

**Atenção**, abra a classe Matricula.java e inclua 2 @NamedQuery , que servirão para fazer a verificação de existência de vínculo entre os registros de alunos que possuem vínculo com as outras tabelas \(Alunos e Disciplinas\).

```
@Entity
@Table(name = "matricula")
@XmlRootElement
@NamedQueries({
    @NamedQuery(name = "Matricula.findAll", query = "SELECT m FROM Matricula m")
    , @NamedQuery(name = "Matricula.findByAluno", query = "SELECT m FROM Matricula m JOIN m.idAluno i WHERE i.id = :idAluno")
    , @NamedQuery(name = "Matricula.findByDisciplina", query = "SELECT m FROM Matricula m JOIN m.idDisciplina d WHERE d.id = :idDisciplina")
    , @NamedQuery(name = "Matricula.findById", query = "SELECT m FROM Matricula m WHERE m.id = :id")})
```



