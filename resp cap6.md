# Lista de Exercícios — Padrões de Projeto (GoF)

## 1. 

(F) 
(V) 
(V) 
(V) 


## 2. 


- Fachada 
- Singleton
- Builder
- Adaptador 
- Fábrica Abstrata 
- Fábrica 
- Proxy
- Decorador 
- Iterador 
- Strategy
- Visitor
- Observador 
- Template Method

## 3. 

- Abstract Factory
- Singleton
- Builder
- Fábrica

## 4. 

- **Aberto/Fechado (Open/Closed):** Strategy, Decorador, Observador e Visitor
- **Desacoplamento:** Fábrica, Proxy e Observador
- **Coesão (Single Responsibility):** Proxy e Visitor, pois ajudam a tirar responsabilidades "extras" da classe principal
- **Simplificação:** Fachada

## 5. 

Os três se parecem porque todos usam composição  — ou seja, um objeto intermediário — em vez de mexer direto na classe base. A diferença está no objetivo de cada um:

- O **Proxy** serve mais para controle de acesso (funciona como um "segurança" na frente do objeto real, ou faz cache, por exemplo).
- O **Decorador** serve para adicionar funcionalidades novas em tempo de execução, sem precisar usar herança.
- O **Visitor** serve para adicionar operações novas numa hierarquia de classes sem precisar alterar o código dela, simulando o famoso *double dispatch*.

## 6. Adaptador (ProjetorLG)

A ideia aqui é implementar a interface que o sistema já espera (`Projetor`) e, por dentro, chamar o método específico do fabricante. No caso do projetor da LG, o manual dizia que passar `0` já liga o projetor na hora:

```java
class AdaptadorProjetorLG implements Projetor {
  private ProjetorLG projetor;

  AdaptadorProjetorLG(ProjetorLG projetor) {
    this.projetor = projetor;
  }

  public void liga() {
    // No manual da LG diz que 0 é pra ligar imediatamente
    projetor.enable(0);
  }
}
```

## 7. 

Usando herança eu precisaria criar 16 subclasses (2⁴), já que teria que cobrir todas as combinações possíveis das 4 funcionalidades (tipo `A_F1`, `A_F1_F2`, `A_F1_F2_F3` e assim por diante). É a clássica explosão combinatória que o livro comenta. No decorador preciso de apenas 4 classes decoradoras (uma para cada funcionalidade) mais a classe base. O usuário final é quem decide como combinar essas funcionalidades, simplesmente encadeando os decoradores no `new`.

## 8. LogChannel (Decorador de Log)

Segui a mesma lógica do `ZipChannel` visto em aula, só que em vez de compactar, dou um `println` mostrando a mensagem que está passando:

```java
class LogChannel extends ChannelDecorator {
  public LogChannel(Channel c) {
    super(c);
  }

  public void send(String msg) {
    System.out.println("Enviando agora: " + msg);
    super.send(msg); // Passa pro próximo da fila
  }

  public String receive() {
    String msg = super.receive();
    System.out.println("Acabei de receber: " + msg);
    return msg;
  }
}
```

## 9. `notifyObservers`

Basta percorrer a lista de observadores e avisar cada um.

```java
public void notifyObservers() {
  for (Observer observer : observers) {
    observer.update(this);
  }
}
```

## 10. 

Se eu fosse juntar as classes de arquivo e buffer numa coisa só, eu adicionaria um parâmetro booleano no construtor (tipo `boolean usarBuffer`). Por padrão ele viria como `true`, mas se por algum motivo específico alguém quisesse desligar o buffer, bastaria passar `false`.

## 11. 

O que acontece basicamente é uma "conversa" entre o `main`, o `veiculo` e o `visitor`:

1. O `main` chama `veiculo.accept(visitor)`.
2. O `veiculo` (por exemplo, um `Carro`) responde chamando `visitor.visit(this)`.
3. O `visitor`, que agora sabe exatamente que está lidando com um `Carro`, executa a lógica específica para esse tipo (imprimir, salvar, etc).

## 12. 

É um padrão em que se cria um objeto "neutro", que implementa a interface esperada mas não faz nada de fato. A grande vantagem é que ele evita que o código fique cheio de verificações `if (obj != null)` espalhadas por todo lugar. Em vez de retornar `null`, a gente retorna esse objeto neutro, e o sistema continua funcionando normalmente sem correr risco de dar `NullPointerException`.