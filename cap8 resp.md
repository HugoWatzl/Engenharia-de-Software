# Respostas — Testes de Software

# 1. 

**e) unidade e integração**.

 
Porque verificara os componentes individualmente (**unidade**) e também as interfaces.

---

# 2. 

##### 1. Achar bugs cedo

Você encontra o erro logo na fase de desenvolvimento, o que é muito mais barato e fácil de consertar do que quando já está na mão do usuário.

##### 2. Rede de proteção contra regressões

Garante que, se você mexer no código hoje para fazer uma melhoria, não vai quebrar algo que já estava funcionando antes.

##### 3. Documentação viva

O teste serve como um manual. Se você quer saber o que uma classe faz, é só olhar os testes dela para ver como ela deve se comportar.


Os três principais benefícios são:

1. **Encontrar bugs cedo:** O erro é encontrado ainda na fase de desenvolvimento, quando é mais barato e fácil de corrigir.
2. **Proteção contra regressões:** Garante que uma alteração no código não quebre algo que já estava funcionando.
3. **Documentação viva:** Os testes mostram como cada classe ou método deve funcionar.

# 3.  Fibonacci (`fib(n)`)

```java
@Test
public void testFibonacci() {
    assertEquals(0, fib(0));
    assertEquals(1, fib(1));
    assertEquals(1, fib(2));
    assertEquals(2, fib(3));
    assertEquals(3, fib(4));
}
```

# 4. S

 retirar o `try-catch` e usar o atributo `expected` na anotação `@Test`:

```java
@Test(expected = java.util.EmptyStackException.class)
public void testEmptyStackException() {
    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.pop();
    stack.pop(); // Deve lançar a exceção
}
```

# 5. Reescrevendo a `ArrayList`

Transformando os `System.out.println` em asserções:

```java
public class ArrayListTest {

    List<Integer> s;

    @Before
    public void init() {
        s = new ArrayList<>();
    }

    @Test
    public void teste1e2() {
        assertTrue(s.isEmpty());

        s.add(1);

        assertFalse(s.isEmpty());
    }

    @Test
    public void teste3() {
        s.add(1);
        s.add(2);
        s.add(3);

        assertEquals(3, s.size());
        assertEquals(Integer.valueOf(1), s.get(0));
    }

    @Test
    public void teste4e5() {
        s.add(1);
        s.add(2);
        s.add(3);

        int elem = s.remove(2);

        assertEquals(3, elem);
        assertEquals(2, s.size());
    }

    @Test(expected = IndexOutOfBoundsException.class)
    public void teste6() {
        s.add(1);
        s.add(2);

        s.remove(2); // Só existem os índices 0 e 1
    }
}
```

# 6. 

Considerando que a função tem quatro comandos — os dois `ifs` e as duas atribuições — e quatro branches, sendo verdadeiro e falso para cada `if`:

| Chamada feita pelo teste | Cobertura de comandos | Cobertura de branches |
| --- | --- | --- |
| `f(0,0)` | 25% (executa somente o primeiro `if`) | 25% (cobre o falso do primeiro `if`) |
| `f(1,1)` | 100% (passa por todas as linhas) | 50% (cobre o verdadeiro do primeiro e do segundo `if`) |
| `f(0,0)` e `f(1,1)` | **100%** | **75%** (falta o caso `x > 0` e `y <= 0`) |

> **Nota:** Em `f(0,0)`, os comandos dentro do primeiro `if` não são executados. Por isso, a cobertura é baixa.

# 7. 

1. **A função tem um bug?**

   Sim. O requisito diz que a nota deve ser maior ou igual a 90 (`>= 90`), mas o código verifica apenas se ela é maior que 90 (`> 90`).

   O bug provoca uma falha quando a nota é exatamente **90**, pois o programa informa que ela não corresponde ao conceito A, embora devesse corresponder.

2. **Cobertura utilizando as notas 85 e 95**

   A cobertura de comandos será de **100%**: a nota 85 testa o `else` e a nota 95 testa o `if`.

   A cobertura de branches também será de **100%**, pois os dois caminhos da condição foram executados.

3. **100% de cobertura garante que não existem bugs?**

   **Falso.** O exemplo mostra que é possível alcançar 100% de cobertura sem testar a nota 90.

   A cobertura mostra quais partes do código foram executadas, mas não garante que a lógica esteja correta.

# 8. 

```java
public void test1() {
    LinkedList list = mock(LinkedList.class);

    when(list.size()).thenReturn(10);

    assertEquals(10, list.size());
}

public void test2() {
    LinkedList list = mock(LinkedList.class);

    when(list.get(0)).thenReturn("Engenharia");
    when(list.get(1)).thenReturn("Software");

    String result = list.get(0) + " " + list.get(1);

    assertEquals("Engenharia Software", result);
}
```

# 9. Cenário de bug escondido pelo Mock

Imagine que a classe **A** seja um `CarrinhoDeCompras` e a classe **B** seja um `BancoDeDados`.

- No **Banco de Dados (B)** real, existe uma regra que impede a inserção de dois produtos com o mesmo ID, pois o ID é uma chave primária. Caso isso aconteça, é lançada uma `SQLException`.

- O **Mock (B')** apenas guarda os produtos em uma lista e não verifica se existem IDs repetidos.

- Ao testar a classe **A** usando o Mock, a inserção de dois produtos com o mesmo ID funciona e o teste passa.

- No teste de integração com o banco de dados real, o sistema falha porque o banco lança uma exceção que não foi tratada pelo código.

Portanto, o Mock escondeu um comportamento importante do banco de dados real.