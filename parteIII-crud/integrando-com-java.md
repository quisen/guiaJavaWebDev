# Integrando com Java

Nesta seção iremos complementar as páginas com a inserção de alguns componentes do PrimeFaces. 

*  **alunos.xhtml**
* **disciplinas.xhtml**
* **matriculas.xhtml**

Primeiramente gostaríamos de mostrar a estrutura **genérica** para as nossas páginas:

![](/assets/alunoMinimal.png)

É importante ressaltar que alguns trechos de código estão **omissos** na imagem a cima.

A estrutura principal dessa página está dentro da tag** ui:define="content"**, e ela consiste basicamente em três forms distintos que executam funções para a visualização, inserção, edição e exclusão.

Os forms são: **tabela**, **modifica **e **cadastro.**

Ou seja, para cada páginas iremos customizar seus reespectivos forms de acordo com os atributos de cada tabela.

