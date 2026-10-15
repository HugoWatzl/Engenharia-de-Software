# Lista de Exercícios — Bugs

## 1.

- **Buffer Overflow:** o problema está no `i <= n`, que faz o laço dar uma volta a mais e escrever fora do vetor. Como as posições começam em 0, a última posição válida é `n - 1`. Então o certo seria usar `i < n`.

- **Use-after-free:** o programa libera a memória com `free(a)` e depois tenta acessar `a[6]`. Ou seja, ele tenta usar uma memória que já foi liberada, e não dá pra confiar no que vai acontecer.

## 2.

O bug é um **Buffer Overflow**. O nome `"Maria Jose"` tem 10 caracteres, mas em C precisa de mais um espaço para o `\0`, que indica onde a string termina.

Então o vetor deveria ter pelo menos 11 posições. Como só tem 10, o `strcpy` escreve além do limite e pode acabar alterando outros dados da memória, embora não seja garantido que altere o `id`.

## 3.

Um **dangling pointer** é basicamente um ponteiro que continua guardando o endereço de uma memória que já foi liberada, ou de um objeto que deixou de existir.

Na questão 1, por exemplo, depois do `free(a)`, o ponteiro `a` continua ali, mas a memória não está mais disponível para ele usar. Se tentar acessar essa memória, acontece o **use-after-free**.

## 4.

1. Isso acontece porque o `int` do Java vai de `-2147483648` até `2147483647`. Repare que o lado negativo tem um valor a mais. Então, quando o método tenta transformar `-2147483648` em positivo, o resultado não cabe em um `int`. Ocorre um overflow e, pela forma como esses números são representados, o valor continua negativo e igual ao original.

2. Não é um bug da implementação, porque a própria documentação do `Math.abs(int)` avisa que isso acontece nesse caso. Mesmo sendo um resultado estranho, ele está previsto. O problema seria o nosso código usar esse método assumindo que ele sempre retorna um número positivo ou zero.

## 5.

1. A linha extra faz o programa pular direto para `fail`, sem executar a terceira verificação. A indentação parece mostrar que ela está dentro do `if`, mas não está, porque não existem chaves. Se as duas primeiras etapas passarem, `err` continua valendo `0`, e a função acaba retornando sucesso sem verificar tudo.

2. Pode ter sido uma linha duplicada sem querer ao copiar e colar ou ao juntar alterações de outros desenvolvedores. Como ela está alinhada com a linha anterior, fica fácil olhar rápido e achar que faz parte do `if`. Só pelo código não dá pra saber exatamente como aconteceu.

3. Eu usaria chaves nos `if`, mesmo tendo só uma instrução, e faria uma revisão do código. Um analisador estático também poderia ajudar a perceber que a terceira etapa nunca é executada. Além disso, um teste em que as duas primeiras verificações passam e a terceira falha ajudaria a encontrar o problema.

## 6.

Porque, se eu entendi o requisito errado, posso escrever tanto o código quanto o teste seguindo essa mesma ideia errada. Aí o teste passa e parece que está tudo certo, mas não é o que o cliente pediu.

Então, só testar cada função não resolve. Também precisa conversar com o cliente ou PO e confirmar se o comportamento implementado é realmente o esperado.

## 7.

A ideia da curva é que, quanto mais tarde um defeito é descoberto, mais caro tende a ser corrigir. O **shift-left** tenta antecipar as verificações para encontrar esses problemas mais cedo.

Isso combina principalmente com **bugs de entendimento de requisitos**. Se uma regra foi entendida errado no início e só perceberam em produção, pode ser necessário refazer partes do projeto, do código e dos testes. No começo, talvez bastasse esclarecer a regra. Mas nem todo bug vai seguir exatamente essa curva.

## 8.

Porque às vezes o linter gera tantos avisos, incluindo falsos positivos, que acaba atrapalhando. O desenvolvedor perde tempo olhando coisas que não são problemas reais e começa a ignorar os alertas.

Eu seguiria duas boas práticas:

- **Começar com poucas regras importantes:** escolher as que fazem sentido para o projeto e ajustar as que só geram avisos pouco úteis.
- **Usar durante o desenvolvimento:** rodar a ferramenta com frequência, inclusive na integração contínua, para corrigir os problemas enquanto a alteração ainda está fresca na cabeça.