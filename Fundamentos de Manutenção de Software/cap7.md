# Lista de Exercícios — Dívida Técnica

## 1.

a) Uma consequência positiva é conseguir entregar uma funcionalidade mais rápido, aproveitando uma oportunidade de negócio. A equipe aceita uma solução provisória e deixa a melhoria para depois.

b) Uma consequência negativa é que essa solução pode dificultar futuras alterações. O tempo economizado no início acaba sendo gasto depois com manutenção e retrabalho, como se fossem os juros da dívida.

## 2.

De forma geral, devemos pagar primeiro a dívida que cobra mais juros, ou seja, a que mais atrapalha as mudanças no sistema. Normalmente ela está em partes que a equipe precisa alterar com frequência.

Também vale considerar a importância daquela parte para o sistema e os riscos de continuar adiando a correção.

## 3.

a) Planejada: para entregar um sistema de reservas no prazo, a equipe decide colocar as regras de reserva diretamente nas telas, mesmo sabendo que deveriam ficar em uma camada separada. Ela registra essa decisão e combina de organizar a arquitetura depois.

b) Não planejada: esse sistema foi criado para uma única biblioteca, mas depois precisa atender várias unidades com regras diferentes. A estrutura original passa a exigir adaptações espalhadas pelo código. Essa dificuldade não foi assumida de propósito, pois a nova necessidade não estava prevista.

## 4.

- SonarQube: analisa o código e aponta problemas que podem dificultar a manutenção, como trechos muito complexos e duplicação. Isso ajuda a encontrar possíveis dívidas de implementação.

- Checkstyle: verifica se o código Java segue as regras definidas pela equipe, como padrões de nomes e organização. Pode ajudar a perceber falta de padronização que dificulta a leitura e a manutenção.

Nenhuma delas entende o sistema inteiro ou as decisões do negócio. Então os resultados servem como apoio, e a equipe ainda precisa avaliar o que realmente representa uma dívida.

## 5.

Exemplo: um sistema de biblioteca que não possui testes automatizados nas reservas.

- What (o quê): faltam testes automatizados para as regras de reserva de livros.
- Where (onde): no módulo de reservas, principalmente nas verificações de disponibilidade e limite de reservas por aluno.
- Why (por quê): cada alteração exige conferir tudo manualmente, e já aconteceram casos de uma mudança quebrar uma regra que funcionava antes.
- When (quando): na próxima sprint, antes de adicionar a funcionalidade de lista de espera.
- Who (quem): a equipe responsável pelo módulo de reservas, com apoio do profissional de testes.

## 6.

Não dá pra dizer que é dívida técnica só porque o sistema está lento. A lentidão é um problema de desempenho, mas precisamos entender a causa.

Por exemplo, se as consultas estão espalhadas e duplicadas pelo código, dificultando qualquer melhoria, existe uma dívida técnica que pode estar contribuindo para o problema. Já uma lentidão causada por uma instabilidade temporária na rede não significa, por si só, que exista dívida técnica.

## 7.

Eu colocaria no quadrante de dívida deliberada e imprudente.

É deliberada porque a organização decidiu não implementar testes. É imprudente porque ela descarta o valor dos testes, sem avaliar direito os riscos e o retrabalho que isso pode trazer.

Não parece ser uma decisão de adiar alguns testes por um prazo específico e depois fazê-los. A organização simplesmente considera que eles não são necessários.