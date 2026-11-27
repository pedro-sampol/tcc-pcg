# Artigo 3: Especificidades do PCG em Jogos Cooperativos vs. Single-Player

#### Resumo

Este artigo analisa criticamente as diferenças conceituais e algorítmicas entre a Geração Procedural de Conteúdo (PCG) aplicada a jogos _single-player_ e a jogos cooperativos _multiplayer_. Enquanto o PCG tradicional para um único jogador foca em garantir a acessibilidade física, curvas de desafio individual e variabilidade estética, a geração para ambientes cooperativos exige a incorporação de restrições de coordenação intersubjetiva e padrões formais de design cooperativo. Analisam-se exemplos da indústria comercial e aprofundam-se as arquiteturas de _benchmarks_ acadêmicos recentes, detalhando métricas de avaliação quantitativas e simulações com agentes virtuais.

---

### 1. Introdução: O Paradigma Individual vs. a Dinâmica Interpessoal

A maioria dos algoritmos clássicos e modernos de PCG foi formulada sob a premissa do jogo _single-player_. Nesse paradigma, o espaço de busca do algoritmo é delimitado por restrições topológicas unidirecionais: garantir que exista ao menos um caminho navegável entre a entrada e a saída (_solvability_), distribuir itens de forma progressiva e equilibrar a dificuldade em relação às capacidades de um único agente.

Por outro lado, quando o ambiente virtual é projetado para múltiplos jogadores atuando de forma não-adversarial e com objetivos compartilhados, o espaço de design torna-se substancialmente mais restritivo. A simples presença de múltiplos avatares em um mapa procedurally gerado não transforma a experiência em cooperativa. Para que a cooperação emerja de forma estrutural, o algoritmo gerador precisa considerar a interdependência entre os jogadores, a divisão espacial de tarefas e o suporte a mecânicas de coordenação mútua.

---

### 2. PCG em Jogos Single-Player: Arquiteturas e Exemplos Reais

Nos jogos _single-player_, o PCG atua primordialmente como um mecanismo de expansão de conteúdo e incremento de rejogabilidade. A literatura documenta um amplo espectro de aplicações comerciais e acadêmicas consolidadas:

- **Roguelikes e Dungeon Crawlers:** Títulos clássicos como _Rogue_ (1980) e contemporâneos como _The Binding of Isaac_ e _Spelunky_ utilizam métodos construtivos baseados em grafos, Particionamento Binário de Espaço (BSP) e Autômatos Celulares para sintetizar masmorras com dependências de chaves e portas.
- **Mundos Abertos e Exploração de Terrenos:** Jogos como _No Man's Sky_ aplicam composições fractais e funções de ruído continuas (Perlin e Simplex) para gerar planetas inteiros de forma determinística a partir de _seeds_ numéricas, enquanto abordagens profundas com módulos de _outpainting_ permitem terrenos infinitos com suporte a Níveis de Detalhe (LOD) progressivos.
- **Plataformas 2D e Síntese de Sequências:** Jogos de plataforma 2D representam o _benchmark_ mais frequente da literatura, com _Super Mario Bros._ sendo amplamente utilizado para treinar modelos de Redes Neurais Recorrentes (LSTMs), GANs e modelos de linguagem como o _MarioGPT_.

Nesses cenários, a validação do conteúdo gerado apoia-se em algoritmos determinísticos de busca de caminho (_pathfinding_ A\*) para confirmar se o jogador individual possui os recursos geométricos e mecânicos necessários para concluir o nível.

---

### 3. As Especificidades do PCG em Jogos Cooperativos

A geração procedural para jogos cooperativos introduz desafios que não podem ser resolvidos por algoritmos de acessibilidade _single-player_. A revisão sistemática conduzida por Rocha e Prada revela que a área possui um número reduzido de estudos acadêmicos dedicados, identificando uma lacuna entre a produção industrial e a formalização teórica.

#### 3.1. Cooperação Implícita vs. Cooperação Explícita

A literatura estabelece uma distinção entre dois modos fundamentais de design de níveis cooperativos:

- **Design Implícito (Cooperação Não-Obrigatoria):** Presente em títulos comerciais populares como _Minecraft_, _Risk of Rain_, _Valheim_ e _Deep Rock Galactic_. Nesses jogos, o PCG sintetiza o mapa sem impor que os desafios sejam resolvidos em equipe. A cooperação emerge de forma espontânea porque a presença de outros jogadores torna as tarefas mais céleres ou fáceis, mas o conteúdo gerado não impede que um jogador individual conclua os objetivos isoladamente (_efeito lobo solitário_).
- **Design Explícito (Cooperação Obrigatória):** O algoritmo de PCG incorpora restrições mecânicas e espaciais que tornam a conclusão do nível impossível sem a atuação coordenada de dois ou mais jogadores.

#### 3.2. Padrões de Design Cooperativo (_Cooperative Game Design Patterns_)

Para que o PCG explícito opere com precisão, a geração deve ser guiada por padrões formais de design cooperativo extraídos da literatura de interação humano-computador:

1. **Complementaridade (_Complementarity_):** Geração de papéis ou habilidades assimétricas que se completam para a resolução de um obstáculo.
2. **Metas Compartilhadas (_Shared Goals_) e Puzzles Compartilhados (_Shared Puzzles_):** Estruturação de objetivos não-competitivos e quebra-cabeças que exigem ações simultâneas de múltiplos avatares.
3. **Mecanismos de Portas (_Gates_):**
   - _Separation Gate:_ O algoritmo divide fisicamente os jogadores em caminhos isolados, exigindo que um acione mecanismos para libertar o outro.
   - _Gathering Gate:_ O nível força o alinhamento espacial dos jogadores em um ponto específico antes de permitir a abertura da próxima seção.
4. **Concorrência (_Concurrency_) e Paralelização (_Parallelization_):** Exigência de acionamento simultâneo de interfaces (ex.: pressionar dois botões distantes no mesmo intervalo de tempo) ou divisão eficiente de trabalho para superar restrições temporais.
5. **Interação com o Mesmo Objeto (_Interacting with the same object_):** Implementação de ativos interativos que requerem manipulação coordenada de dois ou mais jogadores para movimentação ou operação.

---

### 4. Arquiteturas e Métodos em Benchmarks Cooperativos Reais

A análise dos estudos aceitos na literatura científica demonstra o uso de diferentes metaheurísticas e modelos generativos adaptados para impor restrições cooperativas.

#### 4.1. _Overcooked!_ como Benchmark de Simulação de Cozinha

O jogo _Overcooked!_ consolidou-se como o principal ambiente de teste acadêmico para PCG cooperativo devido à sua dependência de coordenação espacial e divisão de tarefas:

- **Abordagem por Algoritmo Genético (Baek et al.):** Os autores parametrizaram a função de aptidão de um Algoritmo Genético para controlar a proporção de blocos funcionais (balcões, panelas, pratos e recipientes de ingredientes) e a topologia das salas. No estudo de validação com usuários, foram comparados quatro arranjos gerados: _Messy_ (distribuição aleatória sem restrições), _IM1_ e _IM2_ (layouts com cooperação implícita por agrupamento de tarefas ou divisões parciais) e _EX_ (layout com cooperação explícita, onde uma parede física divide a cozinha e força a passagem de ingredientes por um único balcão central).
- **Abordagem Híbrida GAN + MIP + CMA-ME (Fontaine et al.):** Para garantir simultaneamente a fidelidade estética, a jogabilidade e a diversidade de coordenação em equipes formadas por humanos e robôs (_human-robot coordination_), os autores desenvolveram um _pipeline_ em três etapas:
  1. Uma Redes Adversárias Generativa Convolucional (GAN) sintetiza layouts de cozinhas fotorrealistas treinada em níveis criados por humanos.
  2. Um solucionador de Programação Linear Mista (MIP) repara o mapa gerado para garantir que ele seja fisicamente jogável.
  3. O algoritmo _CMA-ME_ (_Covariance Matrix Adaptation MAP-Elites_) explora o espaço latente da GAN e executa simulações com agentes virtuais para gerar um arquivo de níveis diversos em relação à quantidade de coordenação exigida.

#### 4.2. _Geometry Friends_ e Plataformas de Quebra-Cabeça Assimétricos

Em _Geometry Friends_, um jogo de plataforma 2D com personagens assimétricos (um círculo com alta mobilidade de salto e um quadrado capaz de alterar sua forma), o trabalho de Passos / Ramos aplicou uma abordagem de _gerar-e-testar_ guiada por um algoritmo de alcançabilidade (_reachability algorithm_) em grafos. O algoritmo verifica quais zonas do mapa podem ser atingidas por cada personagem isoladamente ou apenas por meio do suporte mútuo (ex.: o quadrado servindo de plataforma para o salto do círculo), alocando os colecionáveis (_gems_) estritamente nas áreas que satisfazem o nível de cooperação desejado.

#### 4.3. Jogos de Labirinto e Ambientes de Realidade Virtual (VR)

- **Ambientes de Realidade Virtual (Picariello et al.):** Em um jogo de fuga de labirinto em VR projetado para integração de equipes (_team building_), o mapa principal é gerado pelo algoritmo de Prim Modificado e Autômatos Celulares. Em seguida, mapas parciais distorcidos são criados para os demais participantes fora da VR via algoritmo _Hill-Climber_, forçando a comunicação verbal para a navegação do jogador imerso.
- **Maze Maestro (Sjölund et al.):** Utiliza o algoritmo _Hunt-Kill_ baseado em múltiplas caminhadas aleatórias (_Random Walks_) para gerar sub-segmentos de labirintos em dispositivos móveis que devem ser fisicamente encostados para formar o mapa completo.

#### 4.4. Otimização por Simulated Annealing e Gramáticas Regradas

- **Criminal Chase e Dungeon Delver (Heffron):** Em dois jogos cooperativos 3D em VR, utilizou-se a otimização por _Simulated Annealing_ para concatenar blocos pré-desenhados (_patches_). A função de custo era composta pelo custo do layout geométrico e por um parâmetro ajustável de cooperação alvo que alterava a densidade de inimigos e itens essenciais.
- **Puzzle-Platformers 2D (Arkel et al.):** Aplicaram uma sequência de gramáticas generativas operada em quatro etapas (_geração de caminho, definição de segmentos, aplicação de padrões de design e ajustes finais_), injetando obrigatoriamente áreas de cooperação em regiões predefinidas do nível.

---

### 5. Métricas de Avaliação de Desempenho Cooperativo (CPMs)

Avaliar a qualidade de um gerador procedural em jogos _single-player_ envolve medir métricas como a taxa de conclusão (_completion rate_), a distribuição de dificuldade e o alcance expressivo (_Expressive Range Analysis_). Em jogos cooperativos, essas métricas são insuficientes para capturar a natureza da interação humana.

#### 5.1. Métricas Comportamentais Subjetivas e Objetivas

A literatura adota um conjunto de Métricas de Desempenho Cooperativo (_Cooperative Performance Metrics_ — CPMs), propostas e validadas por Seif et al., para quantificar a emergência de cooperação em sessões de teste:

- **Métricas Subjetivas/Observacionais:** Anotação de marcadores comportamentais durante a partida, como a frequência com que os jogadores risadas em conjunto, formularam estratégias locais/globais, prestaram ajuda mútua, esperaram uns pelos outros ou atrapalharam a navegação do colega.
- **Métricas Objetivas Contínuas (Liu et al.):**

  $$\text{Degree of Collaboration} = \frac{\text{Tempo total decorrido dentro de zonas de colaboração}}{\text{Tempo total de duração da partida}}$$

  $$\text{Collaboration Time} = \text{Duração absoluta em segundos gasta em ações cooperativas necessárias}$$

Essas métricas objetivas foram validadas por Liu et al. em ambientes virtuais compartilhados, demonstrando correlação direta com a percepção subjetiva de trabalho em equipe relatada pelos jogadores.

#### 5.2. Avaliação por Simulação com Agentes Virtuais (AI Agents)

Devido ao alto custo de conduzir testes de usuário contínuos durante a fase de otimização de geradores procedurais, a pesquisa contemporânea utiliza simulações com agentes de Inteligência Artificial para avaliar a jogabilidade cooperativa.

Como demonstrado por Liu et al. e Fontaine et al., agentes de Aprendizado por Reforço treinados em pares atuam como substitutos eficientes de testadores humanos. Ao registrar o tempo e a taxa de sucesso com que a dupla de agentes supera os segmentos procedurais, o sistema calcula autonomamente o grau de cooperação exigido por um mapa antes de submetê-lo a usuários reais.

---

### 6. Considerações Finais do Módulo de Fundamentos

A geração procedural de conteúdo para jogos cooperativos exige uma mudança de perspectiva na Ciência da Computação: o algoritmo deixa de ser um mero arranjador de geometria navegável e passa a atuar como um mediador de interações sociais. A integração de padrões formais de design cooperativo, a parametrizacao da cooperação em metaheurísticas de busca e a validação por simulações de agentes virtuais constituem a base metodológica para o avanço desse campo.

Com a conclusão deste artigo, encerra-se o **Módulo 1 (Fundamentos teóricos e taxonômicos)** da nossa trilha de aprendizado. No **Módulo 2**, iniciaremos as abordagens práticas e computacionais, explorando a implementação interativa de **Funções de Ruído e Mapas de Altura (Perlin, Simplex e Worley Noise)** para a síntese de terrenos digitais.
