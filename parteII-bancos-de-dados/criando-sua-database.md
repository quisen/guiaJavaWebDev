# Criando seu Banco de Dados

Depois de conectar-se ao seu servidor SQL, utilize o campo de **edição de querys **para realizar os primeiros comandos de criação.

Como exemplo,** **iremos utilizar um banco nomeado como **universidade**, contendo tabelas e registros fictícios.

Utilizaremos o comando CREATE DATABASE para criar e definir o nome do banco.

```SQL
CREATE DATABASE universidade;
```

É necessário executar o comando **USE** para definirmos em qual banco iremos operar, **caso contrário** o programa **não** **saberá** em qual database você deseja executar seus comandos.

```SQL
USE universidade;
```

Depois disso criaremos a primeira tabela chamada** aluno**:

```SQL
CREATE TABLE aluno (
    id INT NOT NULL AUTO_INCREMENT,
    nome VARCHAR(255) DEFAULT NULL,
    matricula INT DEFAULT NULL,
    cpf VARCHAR(13) DEFAULT NULL,
    telefone VARCHAR(100) DEFAULT NULL,
    endereco VARCHAR(255) DEFAULT NULL,
    periodo INT DEFAULT NULL,
    PRIMARY KEY (id)
);
```

Note que o atributo **id**, definido como chave primária \(primary key\), também possui o sufixo **AUTO\_INCREMENT **que delega ao banco a criação de registros de maneira **sequenciada** e **única**.

Note que ao inserir registros na tabela,** não é necessário **definir o **id **como parâmetro dentro do comando **INSERT**.

Abaixo estão alguns comandos de inserção de registros na tabela aluno:

```SQL
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Lois",1421360,"1632010497799","(22) 98492-2558","8603 Per Ave",7);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Medge",299215,"1613011317199","(14) 98501-1201","Ap #157-3813 Justo Street",6);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Quincy",1578504,"1600011477899","(65) 98901-5201","269-9229 Vitae Street",4);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Joel",1837305,"1632120394499","(57) 98292-9924","Ap #110-1827 Vel, Ave",5);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Beatrice",956740,"1614051310799","(74) 98485-2976","349 Vulputate Ave",3);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Hedda",175672,"1681041935699","(13) 98531-2988","428-1136 Sagittis Ave",10);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Brynn",329045,"1688122144799","(91) 98673-1553","P.O. Box 893, 4739 Tempus Rd.",7);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Marny",1750590,"1650031912599","(42) 98708-9072","P.O. Box 650, 6606 Commodo Rd.",3);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Ronan",1052162,"1663062501899","(85) 98252-5089","941-3521 Vitae Avenue",6);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Tyrone",1708255,"1604071662099","(73) 98497-2770","718-5241 At Road",2);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Galvin",310567,"1695060570899","(82) 98170-4905","351-2310 Non St.",6);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Hu",597773,"1642072055599","(24) 98132-8119","Ap #379-373 Mollis St.",9);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Beck",1268638,"1634023093499","(75) 98242-4603","Ap #162-4786 Magna. Av.",10);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Signe",513875,"1633012250799","(64) 98672-0313","655 Feugiat St.",6);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Candace",1336855,"1677011908299","(75) 98430-6504","676-5472 Quis Av.",7);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Melinda",1661667,"1696060657699","(62) 98398-5846","822 Lorem Street",3);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Francesca",671559,"1672092085199","(71) 98287-5162","231-4714 Mauris Ave",4);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Liberty",1854362,"1627080895599","(33) 98482-2013","Ap #443-9664 Elementum Road",4);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Troy",1700232,"1624010821399","(88) 98542-6386","Ap #173-797 Ac Rd.",1);
INSERT INTO aluno (nome,matricula,cpf,telefone,endereco,periodo) VALUES ("Bertha",488541,"1608110458399","(53) 98878-6683","4043 Nam Rd.",5);
```

Agora criaremos a tabela **disciplina**:

```SQL
CREATE TABLE disciplina (
    id INT NOT NULL AUTO_INCREMENT,
    nome VARCHAR(255) DEFAULT NULL,
    periodo INT DEFAULT NULL,
    professor VARCHAR(255),
    PRIMARY KEY (id)
);
```

Depois populamos a tabela com alguns registros:

```SQL
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Gestão",1,"João");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Arquitetura de Computadores",2,"Cláudio");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Matemática",3,"Maria");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Economia",4,"Beatriz");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Programação",5,"Marcelo");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Lógica Matemática",6,"Marcos");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Redes de Computadores",7,"Rogério");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Comunicação Visual",8,"Matheus");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Dispositivos Móveis",9,"Felipe");
INSERT INTO disciplina (nome,periodo,professor) VALUES ("Desenvolvimento Web",10,"Mariana");
```

Agora criaremos a tabela **aluno\_matriculado** que irá ter uma **relação** direta com as **tabelas** **criadas** **anteriormente** por meio de chaves estrangeiras \(foreign key\):

```SQL
CREATE TABLE aluno_matriculado (
    id INT NOT NULL AUTO_INCREMENT,
    id_aluno INT NOT NULL,
    id_disciplina INT NOT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (id_aluno) REFERENCES aluno (id),
    FOREIGN KEY (id_disciplina) REFERENCES disciplina (id)
);
```

Por fim, iremos popular a tabela aluno\_matriculado. Note que os registros criados relacionam-se com os registros de **aluno** e **disciplina** por meio de suas **chaves** **primárias**:

```SQL
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (1,2);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (1,4);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (2,2);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (2,3);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (3,2);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (4,4);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (4,5);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (5,1);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (5,2);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (5,3);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (6,2);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (6,3);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (7,6);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (8,7);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (8,8);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (8,9);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (8,10);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (9,5);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (9,3);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (10,8);
INSERT INTO aluno_matriculado (id_aluno,id_disciplina) VALUES (10,9);
```

---

Download do script completo:

[https://gist.github.com/quisen/bab74027c8140efd34c70c8e9feaf350](https://gist.github.com/quisen/bab74027c8140efd34c70c8e9feaf350 "Download do script SQL completo.")

