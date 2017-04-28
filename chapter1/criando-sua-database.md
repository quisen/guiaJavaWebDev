# Criando seu Banco de Dados

Uma vez que você esteja conectado ao seu servidor SQL, utilize o campo de **edição de querys **para realizar os primeiros comandos de criação.

Para efeitos **didáticos **iremos utilizar um banco com tabelas e dados fictícios.

Utilizamos o comando CREATE DATABASE para definir o nome do banco.

> **CREATE DATABASE universidade;**

Em seguida, utilizamos o comando USE para definirmos em qual banco iremos trabalhar.

> **USE universidade;**

Depois disso criaremos a primeira tabela **aluno**,  que contém todos os seus atributos:

> **CREATE TABLE aluno \(**
>
> **    id INT NOT NULL AUTO\_INCREMENT,**
>
> **    nome VARCHAR\(255\) DEFAULT NULL,**
>
> **    matricula INT DEFAULT NULL,**
>
> **    cpf VARCHAR\(13\) DEFAULT NULL,**
>
> **    telefone VARCHAR\(100\) DEFAULT NULL,**
>
> **    endereco VARCHAR\(255\) DEFAULT NULL,**
>
> **    periodo INT DEFAULT NULL,**
>
> **    PRIMARY KEY \(id\)**
>
> **\);**

&lt;script src="https://gist.github.com/quisen/bab74027c8140efd34c70c8e9feaf350.js"&gt;&lt;/script&gt;

https://gist.github.com/quisen/bab74027c8140efd34c70c8e9feaf350



