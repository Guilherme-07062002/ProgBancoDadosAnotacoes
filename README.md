# Programação com Banco de dados

## Introdução

O propósito deste texto é servir como um utilitário focado especificadamente para os estudos da disciplina de programação com banco de dados (TSI - 3° Período).

Portanto dessa forma subentende-se que aquele que lê esse documento já possui uma básica noção sobre SQL, que é a linguagens de consulta a banco de dados relacionais.

---

## Conteúdos abordados neste documento:

1. Functions
2. Stored Procedures
3. Triggers
4. Views

---
## Functions (Funções)

Uma function serve como uma forma de realizar o aproveitamento de código, de forma semelhante a programação estruturada.

Ou seja ao invés de repetir um trecho de código repetidas vezes, podemos apenas chamar a função que foi implementada anteriormente.

No Postgres podemos escrever as funções de duas formas, utilizando **SQL** ou a linguagem procedural **PLPGSQL**.

---

**Sintaxe SQL:**

```sql
CREATE FUNCTION nome_function(parametro tipo_parametro)
RETURNS tipo_retorno
AS $$
    SELECT campo FROM tabela
    WHERE...
$$ LANGUAGE SQL;
```

Com a linguagem SQL, declaramos a função e utilizamos os comandos SQL comuns dentro dela, podemos realizar operações de CRUD cujo resultado é retornado pela função quando esta é chamada.

---

**Sintaxe PLPGSQL:**

```sql
CREATE FUNCTION nome_function(parametro tipo_parametro)
RETURNS tipo_retorno
AS $$
DECLARE variavel tipo_variavel
BEGIN
    SELECT valor 
    INTO variavel
    FROM tabela...

    RETURN variavel
END
$$ LANGUAGE PLPGSQL;
```

Já com PLPGSQL, a estrutura da função é semelhante mas se difere pois dentro dela podemos utilizar uma linguagem procedural, semelhante a qualquer outra linguagem de progrmação como Java, Python, etc...

Ou seja por meio do PLPGSQL podemos declarar váriaveis, armazenar valores nelas e retorna-las na função.

---

**Sugestões**

Ao utilizar funções utilize a seguinte notação:

```sql
CREATE OR REPLACE FUNCTION
```

Dessa forma, sempre que você alterar a função não será necessário remover e criá-la novamente pois o proprio postgres atualiza o código da sua função.

---

Para mais exemplos do uso de functions consulte este [repositório](https://github.com/Guilherme-07062002/ProgBancoDeDadosLista1.git)

---

## Stored Procedures

O diferencial do stored procedures para uma function comum é que o procedure não possui retorno. Além disso um procedure deve ser escrito utilizando PLPGSQL.

Sintaxe com SQL

```sql
CREATE PROCEDURE nome_procedure(parametro tipo_parametro)
AS $$
BEGIN
    -- Código
END;
$$ LANGUAGE PLPGSQL;
```

---

## Triggers

A trigger é uma função que será executada quando um determinado evento ocorrer.

Sua estrutura é muito semelhante a criação de um function comum, a diferença é que a função retorna `trigger`.

Então antes de tudo deve-se criar a função que será executada quando o trigger for acionado.

```sql
CREATE OR REPLACE FUNCTION nome_function( parametro tipo_parametro)
RETURNS trigger AS $$
BEGIN
    -- Código
END;
$$ LANGUAGE PLPGSQL;
```

Após criada a função você deve "instancia-la", criando o gatilho que será executado, nele você especifica a função que será chamada e a condição que irá dispará-lo.

Ou seja, no exemplo abaixo a função será executada após cada inserção que for realizada em *tabela*:

```sql
CREATE TRIGGER nome_function AFTER INSERT ON tabela
FOR EACH ROW EXECUTE PROCEDURE nome_function();
```

---