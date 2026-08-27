
# Catálogo atual em lista

Atualmente, o catálogo de uma empresa de dados é disponibilizado em forma de lista. O sistema carrega todas as informações referentes ao catálogo (itens do catálogo e legendas) ao carregar a página.

---


# ADR-001: Adoção do MongoDB

## Contexto

Atualmente, a empresa tem um foco em manter custos baixos. A feature que deu origem a esta ADR é de agrupamento dinâmico do catálogo de dados. A estrutura que atende esta feature é um objeto com propriedade com listas com nível N de profundidade. A área de negócio pode agrupar alterando a profundidade deste aninhamento de objetos.

## Decisão

Decidimos adotar o MongoDB para armazenar este catálogo.

## Justificativa

O MongoDB foi escolhido pois lida de forma simples com objetos com estas características e é um dos bancos de dados padrão da empresa. Além disso, não traria custos muito maiores pois foi decidido usar um Mongo que é utilizado pela BU.

**Foram consideras as seguintes opções:**
- Como o serviço atual utiliza PostgreSQL, consideramos a opção de criar tabelas para aninhar esses agrupamentos e deixar de forma fixa a profundidade da estrutura do catálogo, no entanto, descobrimos que seria necessária a flexibilidade. Então, consideramos a opção de criar uma tabela para realizar consultas de forma recursiva, mas seriam necessárias muitas consultas no banco apenas para carregar um dado json.
- Foi considerado o uso de um banco de dados in-memory, mas conversando com o time de SRE eles informaram que esta opção acarretaria em um custo de >430 reais por mês, e nós iriamos apenas armazenar um único JSON, temporariamente, e se tornaria futuramente mais catálogos.

## Status

Aceita

## Consequências

### Positivas

- Esta se mostrou a opção de menor custo e atendeu os requisitos de negócio.
- O uso do MongoDB será expandido para outras features e a infra está pronta para desenvolvimentos futuros.

### Negativas

- Esta conexão aumenta a dependência entre serviços, apesar de uma coleção ser exclusiva de um único serviço. No entanto os recursos são compartilhados com outros serviços.
