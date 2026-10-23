# Respostas — Fundamentos de Manutenção de Software

# 1. Escalabilidade horizontal

É aumentar a capacidade de um sistema adicionando mais máquinas ou servidores para dividir o trabalho.

Por exemplo: se um servidor não consegue atender todos os acessos de uma loja virtual, podemos adicionar outros servidores e distribuir as requisições entre eles.

Na vertical, aumentamos os recursos de uma máquina, como memória e processador. Na **horizontal**, aumentamos a quantidade de máquinas trabalhando juntas.

---

# 2. Sprouts e wrappers sem testes de unidade

Sim.

 Eles separam as novas funcionalidades em métodos menores, facilitando a compreensão e a manutenção.

Além disso, evitam aumentar a complexidade do código antigo e facilitam a inclusão de testes futuramente.

---

# 3. Método público no sprout

Para permitir que o teste chame calcularDescontoFrequente diretamente, sem executar todo o método legado.

Se fosse privado, uma classe de teste externa não poderia acessá-lo diretamente. Portanto, essa visibilidade atende à estratégia de teste do exemplo.

---

# 4. Testes controlados em produção

1. Dark launch: executa a funcionalidade em produção, mantendo-a oculta dos usuários. Permite avaliar desempenho, carga e erros antes de disponibilizar sua interface.

2. Execução paralela: executa as versões antiga e nova com as mesmas entradas, compara os resultados e mantém a resposta antiga para o usuário. Ajuda a detectar divergências e regras esquecidas.

---

# 5. Preferência entre sprout e wrapper

Preferiria o wrapper, pois permite adicionar comportamento antes ou depois da execução do legado, preservando seu código interno.

O sprout exige modificar o legado para chamá-lo, aumentando a exposição a regressões.

---

# 6. Dark launch e release canário

No dark launch, a funcionalidade é executada em produção, mas permanece invisível para os usuários.

No release canário, a nova versão é disponibilizada para uma pequena parcela dos usuários, que realmente a utiliza. Se os resultados forem satisfatórios, a liberação é ampliada gradualmente.

---

# 7. Branch por abstração

Criaria uma interface comum, implementada por ServicoPagamento e ServicoPagamentoNovo, com as operações de pagamento.

Os clientes dependeriam dessa interface. Inicialmente, receberiam a implementação antiga; após a validação, poderiam receber a nova sem alterar suas chamadas.

---

# 8. Isolamento do cálculo de frete

injeção de dependência.

Exemplo:

```c
int processar_pedido(int (*calcular_frete)(...)) {
    ...
    int frete = calcular_frete(...);
    ...
    return status;
}
```

Assim, durante os testes, seria possível substituir a função legada por um stub que retorna um valor conhecido.

Isso evita acessar o banco de dados antigo e permite testar `processar_pedido` de forma isolada, rápida e previsível.