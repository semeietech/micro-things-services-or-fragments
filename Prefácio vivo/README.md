
# Prefácio vivo

A proposta do prefácil é proporcionar o conteudo das discuções com o maximo de claresa possivel.

## Conversa: Domingo, 7 de Abril de 2019

### 1 - Rodrigo Pádua | @rodrigoclp at 3:54 pm
> Pessoal, em caso de micro services com bases sql privadas, como fica a questão da integridade referencial? Ocorre a troca e armazenamento de primary keys? Em que momento é realizada verificação de existência, ou não, do registro? A exclusão de registro deve ser eliminada, passando apenas para inativações (paranoid delete)? Agradeço pelos esclarecimentos

### 2 - @reply(1) - Danilo Bredaa | github.com/danilobreda at 4:49 pm
> A não exclusão de um registro fica em aberto pois depende da sua regra de negócio, eu recomendo nunca deletar um registro do banco para log :/ Apenas inativar :D

### 3 @reply(1) - Danilo Bredaa | github.com/danilobreda at 4:51 pm
> Na questão de cada microservice ter seu banco de dados, é algo da base conceitual de microservices, E a chave primaria é armazenada no outro banco sim, é necessaria ter essa referencia! https://microservices.io/patterns/data/database-per-service.html


### 4 - Rodrigo Pádua | @rodrigoclp at 4:52 pm
> ok(com o icone do polegar para cima)

### 5 - Danilo Bredaa | github.com/danilobreda at 4:53 pm
Porem a complexidade aumenta e começa a ter a necessidade de realizar transações distribuidas o que, no final das contas, acaba que sendo superado pelo uso de SAGAS (por conta do teorema de CAP) https://microservices.io/patterns/data/saga.html

### 6 - Danilo Bredaa | github.com/danilobreda at 4:55 pm
a existencia ou não do registro pode ser verificado de varias maneiras, ai fica aberto essa discussão pois o saga mesmo ja seria uma solução, porem pode ser atraves de uma requisição via api para verificar se existe, porem cria se um problema de dependencia com outro microservice :/


### 7 - Danilo Bredaa | github.com/danilobreda at 4:56 pm
No final das contas, uma arquitetura orientada a eventos é inevitável inicialmente https://microservices.io/patterns/data/event-driven-architecture.html


### 8 - jeffotoni/GoLambdaMan | @jeffotoni at 5:01 pm
uau... .:scream:

### 9 - Rodrigo Pádua | @rodrigoclp at 5::03 pm

Muito bom. Valew

### 10 - reply(3) - jeffotoni/GoLambdaMan | @jeffotoni at 5:01 pm

só uma pequena observação, você não precisa provisionar um servidor de banco de dados para cada serviço. Existe cenários e necessidades que são necessários e em outros não . :see_no_evil:

### 11 - reply(10) - Rodrigo Pádua | @rodrigoclp at 9:01 pm

Mas não fica meio estranho uma base única? Acesso a disco é meio q o gargalo dos sistemas e se eu não "fraciono" a base, fica meio q serviços acessando uma base única q vai acabar se sobre carregando. Qual seria a solução nesse caso? Teria replicações de base com uma espécie de tow way data bind para atualização das réplicas?

### 12 - reply(10) - Luiz Carlos Faria | @luizcarlosfaria at 9:36 pm

Exato, quanto a serviços não há nenhuma restrição sobre compartilhar ou não bases.

No entanto, se falarmos de microserviços, compartilhar base faz ele não ser um microserviço.

### 13 - Alexandre Silva | @AlexandreSilvaDev at 9:38 pm

Verdade, para microservices tem que separar totalmente, mesmo que seja necessária replicar o dado, cada microservice deve ser totalmente independente...

### 14 - Danilo Breda | @github.com/danilobreda at 9:43 pm

Existe a parte de separação lógica de banco de dados e fisico(servidor e instancias) sendo que os 2 casos necessarios para microservices. O lógico força a separaçao que cada microservice tem seu banco, impossivel executar join entre eles forçando assim o desaclopamento entre eles

### 15 - Danilo Breda | @github.com/danilobreda at 9:43 pm

Na questao fisica voce resolve o problema de um unico ponto de falha , caso o bd cair voce matou todos os microservices e ai pra que microservices correto? Esse é o ponto

### 16 - Danilo Breda | @github.com/danilobreda at 9:44 pm

Porem se pode começar com uma unica instancia de banco de dados fisico e banco de dados logico por api, pois essa separaçao futura ê facil de executar

### 17 - Danilo Breda | @github.com/danilobreda at 9:45 pm
Lembrando que aws aurora e cosmos db prega conceitos diferentes na questão física


### 18 - Danilo Breda | @github.com/danilobreda at 9:46 pm
Por serem DBaaS

### 19 -  Danilo Breda | @github.com/danilobreda at 9:50 pm

@AlexandreSilvaDev o aws aurora é considerado auto gerenciado? Tipo ele cria um ec2?

### 20 -  Danilo Breda | @github.com/danilobreda at 9:50 pm
O aws dynamodb eu sei que é

### 21 - Marcelo Avancini | at 9:59 pm
Existe o aurora serverless

### 22 - Marcelo Avancini | at 10:00 pm
Esse é auto gerenciado, diferente do aurora "tradicional" que você define as instâncias

### 23 - reply(22) - Danilo Breda | @github.com/danilobreda at 10:00 pm
Hum entendi nunca entendi mto bem o aws aurora :/

### 24 - reply(13) - jeffotoni/GoLambdaMan | @jeffotoni at 10:19 pm
Desculpa mas permite discordar ? Como dizia o Raul
"Eu prefiro ser essa metamorfose ambulante. Do que ter aquela velha opinião formada sobre tudo"
:joy::joy:

### 25 - reply(12) -  jeffotoni/GoLambdaMan | @jeffotoni at 10:21 pm
não necessariamente :grin:

### 26 - jeffotoni/GoLambdaMan | @jeffotoni at 10:21 pm
@AlexandreSilvaDev Nem vou colocar minha opinião só vou referenciar a do Chris Richardson que por sinal descreveu muito bem sobre este assunto aqui..

### 27 - jeffotoni/GoLambdaMan | @jeffotoni at 10:23 pm

![anexo com citação](./files/anexo-27.jpeg)


### 28 - jeffotoni/GoLambdaMan | @jeffotoni at 10:24 pm

![anexo com citação](./files/anexo-28.jpeg)

### 29 - jeffotoni/GoLambdaMan | @jeffotoni at 10:24 pm
:point_up_2: este texto é a referência que o colega Danilo  postou acima..
Super apoio o que ele disse e descreveu em seus posts.

### 30 -  Danilo Breda | @github.com/danilobreda at 10:29 pm
Eu faço assim, uma única instancia fisica de banco de dados, com cada api seu database, estou suscetível a uma falha gigantesca? Sim! Pois tenho uma único ponto de falha. Um dos motivos de microservices é disponibilidade, entao acredito que seja importante mas nao viavel em alguns casos por ser caro mesmo

### 31 - reply(25) - Luiz Carlos Faria | @luizcarlosfaria at 10:34 pm
Estou me referindo ao compartilhamento de objetos, pra ser mais pragmático: tabelas...
Se há compartilhamento de tabelas, não há microserviço

### 32 - Luiz Carlos Faria | @luizcarlosfaria at 10:35 pm
Segregação física e lógica, é outro assunto.


### 33 - reply(30) - jeffotoni/GoLambdaMan | @jeffotoni at 10:49 pm
entendo, é uma das formas de construir.


