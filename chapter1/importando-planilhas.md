# Importando Planilhas para o Banco

É possível importar dados de planilhas no formato **.csv** diretamente ao banco de dados com o Workbench.

Para que a importação ocorra da forma adequada é **importante** que as **colunas da planilha **estajam **ordenadas** da mesma forma que as **colunas \(atributos\)** do banco de dados.

Caso a primeira linha da planilha esteja identficando o nome de cada coluna, essa linha também será incluída como um registro, portanto é **recomendado** que ela seja removida antes da importação. O que importa é a **ordem** das colunas.

É importante ressaltar também que caso já **existam** **registros** na tabela, as **chaves** **primárias** dos novos registros deverão ser **diferentes** daqueles já presentes no banco.

Como exemplo, utilizaremos uma planilha com dados de alunos para serem inseridos na tabela aluno.

![](/assets/planilhaAlunos.png)

Agora que a planilha está com a ordenação adequada das colunas \(id, nome, matricula, cpf, telefone, endereco, periodo\), iremos importá-la ao banco com o Workbech.

Primeiramente é preciso realizar um **SELECT** dos dados da tabela aluno.

Em seguida, selecione a opção "**Import Records From a External File**"  \(ícone azul com uma flecha para cima\) localizada na parte superior ao resultado da consulta e busque o **csv**.![](/assets/importExternalFile.png)

Depois  é necessário selecionar **Apply **no canto inferior direito para que o Workbench converta em instruções SQL e efetive a inserção.

Realize um **SELECT** novamente e os novos registros poderão ser visualizados.

![](/assets/selectAlunosImportFeito.png)

