
# Prefácio vivo

A proposta do prefácil é proporcionar o conteudo das discuções com o maximo de claresa possivel.

## Conversa

```
----------------------------------
Domingo, 7 de Abril de 2019
----------------------------------
```

### 1 - o estopim
```
Rodrigo Pádua | @rodrigoclp at 3:54 pm
-----------------
Pessoal, em caso de micro services com bases sql privadas, como fica a questão da integridade referencial? Ocorre a troca e armazenamento de primary keys? Em que momento é realizada verificação de existência, ou não, do registro? A exclusão de registro deve ser eliminada, passando apenas para inativações (paranoid delete)? Agradeço pelos esclarecimentos
```
### 2

```
Danilo Bredaa | github.com/danilobreda at 4:49 pm
---
@reply(1)
A não exclusão de um registro fica em aberto pois depende da sua regra de negócio, eu recomendo nunca deletar um registro do banco para log :/ Apenas inativar :D
```

### 3 @reply(1)
```
Danilo Bredaa | github.com/danilobreda at 4:51 pm
--------------------------------------------------
Na questão de cada microservice ter seu banco de dados, é algo da base conceitual de microservices, E a chave primaria é armazenada no outro banco sim, é necessaria ter essa referencia! https://microservices.io/patterns/data/database-per-service.html
```


### 4
```
Rodrigo Pádua | @rodrigoclp at 4:52 pm
---------------------------------------
ok(com o icone do polegar para cima)
```

### 5

```
Danilo Bredaa | github.com/danilobreda at 4:53 pm
--------------------------------------------------
Porem a complexidade aumenta e começa a ter a necessidade de realizar transações distribuidas o que, no final das contas, acaba que sendo superado pelo uso de SAGAS (por conta do teorema de CAP) https://microservices.io/patterns/data/saga.html
```

### 6
```
Danilo Bredaa | github.com/danilobreda at 4:55 pm
--------------------------------------------------
a existencia ou não do registro pode ser verificado de varias maneiras, ai fica aberto essa discussão pois o saga mesmo ja seria uma solução, porem pode ser atraves de uma requisição via api para verificar se existe, porem cria se um problema de dependencia com outro microservice :/
```

### 7
```
Danilo Bredaa | github.com/danilobreda at 4:56 pm
--------------------------------------------------
No final das contas, uma arquitetura orientada a eventos é inevitável inicialmente https://microservices.io/patterns/data/event-driven-architecture.html
``` 

### 8
```
jeffotoni/GoLambdaMan | @jeffotoni at 5:01 pm
---------------------------------------------
uau... .:scream:
```

### 9
```
Rodrigo Pádua | @rodrigoclp at 5::03 pm
---------------------------------------
Muito bom. Valew
```

### 10 - reply(3)
```
jeffotoni/GoLambdaMan | @jeffotoni at 5:01 pm
---------------------------------------------
só uma pequena observação, você não precisa provisionar um servidor de banco de dados para cada serviço. Existe cenários e necessidades que são necessários e em outros não . :see_no_evil:
```

### 11 - reply(10)
```
Rodrigo Pádua | @rodrigoclp at 9:01 pm
---------------------
Mas não fica meio estranho uma base única? Acesso a disco é meio q o gargalo dos sistemas e se eu não "fraciono" a base, fica meio q serviços acessando uma base única q vai acabar se sobre carregando. Qual seria a solução nesse caso? Teria replicações de base com uma espécie de tow way data bind para atualização das réplicas?
```
