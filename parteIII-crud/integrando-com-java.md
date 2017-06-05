# Integrando com Java

Nesta seção iremos complementar as páginas com a inserção de alguns componentes do PrimeFaces. Também abordaremos como relacionar esses elementos com o código Java criado anteriormente.

* **alunos.xhtml**
* **disciplinas.xhtml**
* **matriculas.xhtml**

Primeiro gostaríamos de mostrar a estrutura **genérica** para as nossas páginas, para facilitar seu entendimento:

![](/assets/alunoMinimal.png)

A estrutura principal dessa página está dentro da tag** ui:define name="content"**, e ela consiste basicamente em três forms distintos que executam funções para a visualização, inserção, edição e exclusão.

Os forms são: **tabela**, **modifica **e **cadastro.**

É importante ressaltar que alguns trechos de código estão **omissos** na imagem a cima.

Ou seja, para cada páginas iremos customizar seus respectivos forms de acordo com os atributos de cada tabela.

