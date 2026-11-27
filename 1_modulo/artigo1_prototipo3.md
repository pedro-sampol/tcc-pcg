# O que é Geração Procedural de Conteúdo (PCG)?

**Resumo.** Imagine um jogo que cria sozinho as fases, os mapas e até parte das missões, e que a cada partida entrega algo diferente. Isso é Geração Procedural de Conteúdo, ou PCG (do inglês _Procedural Content Generation_). Neste artigo você vai ver o que é PCG, por que os desenvolvedores usam, que tipos de conteúdo um jogo consegue gerar, quais são as vantagens e os problemas dessa técnica e por que PCG não é a mesma coisa que IA generativa.

---

## 1. A ideia central: guardar a receita, não o bolo

Pense em um mapa de jogo como um bolo. A forma tradicional de fazer jogos é assar o bolo na produção: um artista ou designer desenha o mapa, o mapa vai para o disco e todo jogador recebe exatamente o mesmo mapa. No PCG o jogo guarda a **receita** (um algoritmo, ou seja, um conjunto de regras) e, na hora de jogar, o computador segue a receita e monta o mapa.

Na prática, **PCG é a criação automática ou semiautomática de partes de um jogo por meio de algoritmos**. "Semiautomática" porque, na maioria dos casos, humanos ainda definem as regras, escolhem o que entra e ajustam o resultado.

Para o computador "sortear" sem se perder, ele costuma partir de um número inicial chamado **seed** (semente). A mesma seed leva sempre ao mesmo resultado, e uma seed diferente leva a outro. O jogo não precisa guardar o mundo inteiro, só a receita e esse número.

> **Exemplo real | Elite (1984)**
> No clássico _Elite_, o jogador viaja entre planetas em uma galáxia enorme para os computadores da época. O truque: cada planeta (posição, preços, nome) sai de um número semente passado por um algoritmo fixo, então o jogo não gasta memória guardando os planetas e, mesmo assim, cada um é único e sempre igual. São oito galáxias com 256 planetas cada, e o jogo inteiro cabia em cerca de 22 KB. Os criadores queriam ainda mais galáxias, mas a publicadora preferiu limitar o universo.

---

## 2. Por que usar PCG?

### Um pouco de história

O PCG começou a aparecer nos anos 1970 e 1980, em jogos como _Beneath Apple Manor_ (1978) e _Rogue_ (1980), este último tão influente que deu nome a um gênero inteiro, os _roguelikes_. O motivo era bem prático: os computadores da época tinham pouquíssima memória e armazenamento. Gerar o conteúdo na hora era uma forma de entregar mundos grandes sem precisar guardá-los.

### Por que continua sendo usado hoje

Memória deixou de ser o principal problema, mas outras razões ocuparam o lugar:

- **Menos custo e esforço de produção.** Jogos modernos têm uma quantidade absurda de conteúdo. Deixar o computador cuidar de partes repetitivas, como espalhar árvores ou montar variações de fases, economiza tempo da equipe.
- **Vontade de jogar de novo (rejogabilidade).** Se cada partida é diferente, o jogador tem motivo para voltar, sem que a equipe precise desenhar cada variação.
- **Trabalho em parceria com humanos.** Existem ferramentas em que o algoritmo propõe estruturas (um mapa, um layout) e o designer escolhe, corrige e refina. Essa ideia é chamada de _iniciativa mista_ (_mixed-initiative_).

Até grandes produções atuais, como _Diablo IV_ e _Starfield_, usam PCG em partes dos seus ambientes.

> **Exemplo real | No Man's Sky (2016) e Minecraft (2011)**
> _No Man's Sky_ anuncia cerca de 18 quintilhões de planetas. Ninguém desenhou isso à mão: o jogo usa uma seed de 64 bits, e quando você chega a um planeta ele roda os valores daquele planeta pelo algoritmo, que devolve o terreno, o clima, as plantas e os bichos. Em _Minecraft_ a lógica é a mesma, e dois jogadores que usam a mesma seed (na mesma versão do jogo) caem no mesmo mundo.

---

## 3. O que um jogo consegue gerar?

Quase tudo pode ser gerado, de uma textura até uma missão. Para organizar essa bagunça, os estudos da área dividem o conteúdo em categorias. Aqui usamos uma divisão em cinco grupos, do mais simples ao mais complexo (algumas taxonomias separam mais um grupo, o de _sistemas_, como ecossistemas e comportamento de multidões; veja as Referências).

```
Conteúdo gerado em um jogo
│
├── 1. Game Bits (peças básicas)
│       texturas, sons, modelos 3D, plantas
│
├── 2. Game Space (o espaço do jogo)
│       terrenos, mapas, rios, estradas
│
├── 3. Game Scenarios (cenários e progressão)
│       fases, missões, quebra-cabeças, história
│
├── 4. Game Design (regras e sistemas)
│       onde surgem inimigos, dificuldade, habilidades
│
└── 5. Derived Content (conteúdo derivado)
        diálogos extras, textos de ambientação, notícias dentro do jogo
```

1. **Game Bits (peças básicas).** São os tijolos do jogo: texturas, efeitos sonoros, modelos 3D de árvores e construções, tabelas com as propriedades de objetos. No _No Man's Sky_, por exemplo, as criaturas e as plantas de cada planeta são montadas a partir de regras e variações.
2. **Game Space (espaço de jogo).** É o formato dos lugares onde você se move: terrenos, mapas de altura (_heightmaps_), rios, estradas. O mundo de _Minecraft_ é o exemplo mais conhecido.
3. **Game Scenarios (cenários).** É o que está ligado à progressão e à história: o arranjo das fases, as missões, os quebra-cabeças e a ordem em que as partes se conectam.
4. **Game Design (mecânicas e regras).** Aqui o algoritmo decide coisas do próprio funcionamento do jogo, como onde e quando surgem inimigos e itens (os pontos de _spawn_), o nível de dificuldade ou uma tabela de habilidades.
5. **Derived Content (conteúdo derivado).** Não é obrigatório para terminar o jogo, mas dá vida ao mundo: conversas secundárias de NPCs, textos de ambientação (_lore_), notícias que existem só dentro do universo do jogo.
<!-- TODO: incluir um exemplo real de jogo para Derived Content (candidatos a verificar: Dwarf Fortress, Shadow of Mordor / sistema Nemesis). -->

> **Exemplo real | Dead Cells (2018)**
> _Dead Cells_ mistura conteúdo feito à mão com geração. A equipe desenhou várias salas à mão e, para cada fase (bioma), criou um "grafo" que descreve a estrutura da fase: quantas salas, que tipos, como se ligam. O algoritmo então escolhe e encaixa as salas seguindo esse desenho. O resultado é uma fase diferente a cada partida, mas com cara de fase bem planejada. A própria equipe defende que depender demais de geração procedural deixa o nível sem graça e bagunçado.

> **Exemplo real | Left 4 Dead (2008)**
> Em _Left 4 Dead_, o "AI Director" acompanha a situação de cada jogador e adiciona ou remove inimigos e itens para manter a tensão, de forma que cada partida seja diferente. Ele é um bom exemplo de PCG na categoria Game Design: o conteúdo gerado não é um mapa, é o _ritmo_ do jogo.

---

## 4. Quatro perguntas sobre como um gerador funciona

Além de _o que_ é gerado, dá para classificar um gerador respondendo a quatro perguntas simples.

**Quando o conteúdo é gerado? (_online_ ou _offline_)**
Na geração _online_, o algoritmo roda enquanto o jogo está em execução, gerando o conteúdo na hora em que o jogador precisa. Na geração _offline_, o conteúdo é gerado durante a produção e já vai pronto no jogo final. _No Man's Sky_ é online: o planeta aparece quando você chega nele.

**O conteúdo é essencial? (necessário ou opcional)**
É _necessário_ quando o jogador precisa dele para progredir, como o caminho entre a entrada e a saída de uma fase. É _opcional_ quando é só enfeite ou bônus, como a vegetação em volta do caminho.

**O resultado se repete? (estocástico ou determinístico)**
Um gerador _estocástico_ usa números pseudoaleatórios para variar o resultado. Um _determinístico_ devolve sempre a mesma saída para os mesmos dados de entrada. Aqui entra uma pegadinha: o "sorteio" do computador é controlado pela seed, então se você guardar a seed, consegue repetir o mesmo resultado. É assim que todos os jogadores de _No Man's Sky_ encontram o mesmo planeta no mesmo lugar.

**O resultado é testado? (construtivo ou gerar-e-testar)**
Um método _construtivo_ monta o conteúdo de uma vez e já sai certo, sem verificação posterior. Um método _gerar-e-testar_ cria uma versão candidata, confere se ela presta (por exemplo, se dá para ir do início ao fim da fase) e descarta se não passar.

<!-- TODO: achar um jogo comercial bem documentado como exemplo de gerar-e-testar. Por enquanto fica só a analogia. -->

> **Exemplo real | Spelunky**
> Em _Spelunky_, cada fase é uma grade de 16 salas (4 por 4). O gerador sorteia uma sala da fileira de cima como entrada e vai sorteando para onde o caminho segue (esquerda, direita ou para baixo) até chegar à fileira de baixo e marcar a saída. O resultado é um caminho garantido, que dá para atravessar sem cordas nem bombas: esse caminho é o conteúdo **necessário**. As outras salas são só complemento, e algumas nem têm saída: esse é o conteúdo **opcional**. Como o caminho já nasce certo, sem etapa de teste, o método é **construtivo**.

---

## 5. Vantagens e desvantagens do PCG

### Vantagens

- **Escala.** Dá para ter mundos enormes sem uma equipe enorme. É o caso do _No Man's Sky_, feito por um estúdio pequeno.
- **Economia de armazenamento.** Guardar regras e seeds ocupa muito menos espaço que guardar tudo pronto.
- **Variedade e rejogabilidade.** Cada partida pode ser nova, o que é essencial em gêneros como o roguelike, em que você morre e recomeça várias vezes.
- **Menos trabalho repetitivo.** A equipe pode gastar energia em ideias e em acabamento, e não em copiar variações.
- **Surpresa para quem cria.** Às vezes o gerador cria combinações que nenhum designer pensaria.

### Desvantagens

- **Variedade que parece repetição.** Um gerador pode produzir milhões de resultados diferentes que, para o jogador, parecem todos iguais, como uma sequência de tigelas de mingau com cobertura diferente. Muita gente comenta que, em _No Man's Sky_, os planetas começam a parecer parecidos depois de um tempo, apesar do número gigantesco.
- **Menos controle sobre o resultado.** O designer define as regras, mas não vê cada resultado. Pode sair uma fase injusta, impossível ou sem graça.
- **Teste e correção mais difíceis.** Se existem bilhões de variações, não dá para jogar todas. Em _Elite_, por exemplo, a geração às vezes criava sistemas estelares difíceis de alcançar, algo que um universo desenhado à mão teria evitado.
- **Dá trabalho de planejamento.** Um bom gerador exige que os desenvolvedores ensinem ao algoritmo o que é um bom design, e isso é difícil. Por isso muitos jogos misturam as duas coisas, como _Dead Cells_ e _Spelunky_.
- **Pode perder a "mão do autor".** Narrativa e momentos emocionais específicos costumam funcionar melhor quando planejados.

---

## 6. Para fechar

PCG é um jeito de criar conteúdo de jogos usando algoritmos, e vai de uma textura até uma missão. Ele surgiu da falta de memória nos computadores antigos, e hoje é usado também para reduzir custos, aumentar a rejogabilidade e abrir espaço para criar em parceria com ferramentas. Saber o que se quer gerar e como o gerador vai funcionar é o primeiro passo para escolher o método certo, e é isso que o próximo artigo vai explorar: dos métodos clássicos até os que usam aprendizado de máquina.

---

## Referências
