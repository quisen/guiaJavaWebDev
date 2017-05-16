# Limpeza de Cache

Durante o desenvolvimento de aplicações podem ocorrer inconsistências e bugs que são facilmente corrigidos apenas realizando a limpeza do cache.

Para este projeto utilizamos duas ferramentas que podem apresentar erros relacionados ao cache da aplicação, são elas: NetBeans e Glassfish.



# GlassFish

O procedimento para a limpeza do cache GlassFish é relativamente simples. Primeiro interrompa o funcionamento do servidor caso ele esteja em execução. 

Depois navegue até o diretório onde o GlassFish está instalado e procure pelo diretório abaixo: **/glassfish4/glassfish/domains/domain1**

Agora você deve apagar 3 diretórios dentro de /domain1, sendo eles: **applications**, **generated** e **osgi-cache**.

![](/assets/Captura de tela de 2017-05-16 16:18:18.png)

Pronto, agora seu GlassFish está com o cache apagado.



# NetBeans



