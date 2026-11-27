# Artigo 2: Taxonomia Metodológica em Geração Procedural de Conteúdo: Das Abordagens Construtivas aos Modelos Generativos Híbridos

#### Resumo

Este artigo realiza uma análise taxonômica das metodologias computacionais empregadas na Geração Procedural de Conteúdo (_Procedural Content Generation_ — PCG) em jogos digitais. Com base em revisões fundamentais e levantamentos recentes da literatura, mapeiam-se cinco grandes famílias algorítmicas: métodos construtivos clássicos, abordagens baseadas em busca e otimização (_Search-Based PCG_), aprendizado de máquina em PCG (_PCGML_), modelos de linguagem de larga escala (_LLMs_) sob o paradigma _Model-as-a-Service_ (MaaS) e arquiteturas combinadas ou híbridas. Por fim, apresenta-se o roadmap das próximas publicações práticas e conceituais que compõem a trilha de aprendizado do projeto de extensão universitário.

---

### 1. Introdução e Panorama Metodológico

A evolução dos algoritmos de PCG reflete as transformações históricas da própria Ciência da Computação. Inicialmente caracterizada por abordagens constritivas e determinísticas orientadas à economia de memória, a área expandiu-se com a incorporação de metaheurísticas de otimização, aprendizado profundo (_Deep Learning_) e, mais recentemente, arquiteturas baseadas em mecanismos de atenção e modelos generativos comerciais.

A classificação das técnicas computacionais em PCG requer a superação de visões simplistas que reduzem a geração procedural à simples amostragem pseudoaleatória. Na literatura acadêmica, os algoritmos são categorizados segundo sua arquitetura lógica, o nível de controle exercido sobre o espaço de busca e o grau de dependência em relação a dados prévios de treinamento.

```
                                ┌──────────────────────────────────────────┐
                                │   Taxonomia Metodológica de PCG          │
                                └────────────────────┬─────────────────────┘
                                                     │
   ┌───────────────────┬────────────────────┼────────────────────┬───────────────────┐
   ▼                   ▼                    ▼                    ▼                   ▼
Métodos Construtivos  Busca e Otimização   Aprendizado de       Modelos de Linguagem  Arquiteturas
Clássicos             (Search-Based)        Máquina (PCGML)      de Larga Escala      Combinadas/Híbridas
┌───────────────┐     ┌───────────────┐     ┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│ • PRNG / Noise│     │ • Alg. Genét. │     │ • Markov/N-Gram│   │ • Transformers│    │ • LVE / GAN+GA│
│ • Gramáticas  │     │ • WFC / CSP   │     │ • GANs e VAEs │    │ • GPT / LLMs  │    │ • LLM + RL    │
│ • Autômatos C.│     │ • MCTS / SA   │     │ • PCGRL / MDP │    │ • MaaS (APIs) │    │ • Repair/MIP  │
└───────────────┘     └───────────────┘     └───────────────┘    └───────────────┘    └───────────────┘
```

---

### 2. Métodos Construtivos Clássicos e Baseados em Regras

Os métodos construtivos sintetizam artefatos em uma única passagem computacional ou por meio de regras diretas, sem submeter o resultado a funções de avaliação pós-geração. Caracterizam-se por reduzida complexidade de processamento e alta previsibilidade estrutural.

#### 2.1. Geradores de Números Pseudoaleatórios (PRNGs) e Funções de Ruído

A forma mais elementar de geração procedural apoia-se em Geradores de Números Pseudoaleatórios (PRNGs). Quando combinados a equações matemáticas contínuas, os PRNGs originam as chamadas funções de ruído de gradiente, entre as quais se destacam o Ruído Perlin e o Ruído Simplex. Essas funções geram matrizes de valores suaves que são mapeadas em mapas de altura (_heightmaps_) para a modelagem de terrenos 2D e 3D. Abordagens fractais, como o algoritmo _Diamond-Square_ e a Subdivisão Estocástica, exploram a autossimilaridade natural para adicionar rugosidade em diferentes escalas de detalhe (LOD). A principal limitação dessas técnicas reside no fato de que amostragens puramente estatísticas não possuem consciência topológica ou semântica do jogo.

#### 2.2. Gramáticas Generativas e Grafos

Inspiradas nos trabalhos formais de Noam Chomsky e Aristid Lindenmayer, as gramáticas generativas utilizam sistemas de reescrita de símbolos orientados por regras de substituição. Os _L-Systems_ (Sistemas de Lindenmayer) são amplamente aplicados na modelagem de estruturas botânicas e na representação formal de corredores e ramificações de masmorras. Para superar a natureza estritamente linear das gramáticas textuais, a literatura introduziu as gramáticas de grafos (_graph grammars_). Nelas, nós representam salas ou estados narrativos e arestas representam passagens ou requisitos de chaves, permitindo a síntese de redes topológicas não-lineares.

#### 2.3. Autômatos Celulares

Os Autômatos Celulares (CA) são sistemas discretos operados em grades bidimensionais ou tridimensionais. Cada célula altera seu estado a cada iteração com base nas condições de sua vizinhança local (tipicamente as vizinhanças de Moore ou Von Neumann). Em PCG, regras de transição baseadas em limiares de densidade (_Caves Rules_) são aplicadas para suavizar matrizes aleatórias iniciais, simulando processos naturais de erosão e escavação de cavernas orgânicas.

---

### 3. Métodos Baseados em Busca e Otimização (Search-Based PCG — SBPCG)

Formulado por Togelius et al., o paradigma _Search-Based PCG_ (SBPCG) enquadra a geração de conteúdo como um problema de otimização estocástica ou busca em espaço de estados. Ao contrário das abordagens construtivas puras, o SBPCG opera sob o princípio de _gerar-e-testar_, onde o algoritmo produz candidatos e os avalia quantitativamente por meio de uma função de aptidão (_fitness function_).

```
  ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
  │ Candidate State │ ───►  │ Fitness Evaluation │ ──►│ Accept / Mutate │
  │ (Representation)│       │  (Objective Func.) │    │   / Recombine   │
  └─────────────────┘       └─────────────────┘       └─────────────────┘
           ▲                                                   │
           └───────────────────────────────────────────────────┘
```

#### 3.1. Algoritmos Evolutivos e Genéticos

Os Algoritmos Genéticos (GAs) mantêm uma população de instâncias de conteúdo codificadas como cromossomos. A cada geração, os indivíduos são avaliados por funções de aptidão que medem propriedades como jogabilidade, simetria e acessibilidade. Os indivíduos de maior pontuação são selecionados para cruzamento (_crossover_) e mutação estocástica, refinando iterativamente a população.

#### 3.2. Wave Function Collapse (WFC) e Satisfação de Restrições

O algoritmo _Wave Function Collapse_ (WFC), adaptado da síntese de texturas por Efros e Leung, opera como um solucionador de Problemas de Satisfação de Restrições (CSP). Inicialmente, todas as células da grade encontram-se em um estado de superposição de todos os _tiles_ possíveis. O algoritmo seleciona a célula de menor entropia de Shannon, colapsa seu estado para uma única opção e propaga recursivamente as restrições de adjacência para as células vizinhas. O WFC garante que o conteúdo gerado respeite rigorosamente as regras locais de encaixe presentes nos exemplos de entrada.

#### 3.3. Outras Metaheurísticas de Busca

Outras técnicas estocásticas de otimização incluem o _Monte Carlo Tree Search_ (MCTS), que explora árvores de decisão combinatórias por meio de simulações estatísticas; o _Simulated Annealing_ (SA), que aceita soluções piores com uma probabilidade decrescente para evitar máximos locais; e a _Particle Swarm Optimization_ (PSO), baseada no comportamento de enxames em espaços de busca contínuos.

---

### 4. Aprendizado de Máquina em PCG (PCGML)

A inclusão do Aprendizado de Máquina em PCG (_Procedural Content Generation via Machine Learning_ — PCGML), sumarizada por Summerville et al., marca a transição da codificação manual de regras para a extração automática de padrões a partir de corpora de dados existentes.

#### 4.1. Modelos Estatísticos e Sequenciais

- **Cadeias de Markov e N-Grams:** Modelam a probabilidade de transição entre estados de conteúdo (como a sequência de blocos em um nível) com base nos estados imediatamente anteriores.
- **Redes Neurais Recorrentes (RNNs) e LSTMs:** Arquiteturas projetadas para processar sequências temporais ou espaciais ordenadas. Níveis de jogos 2D são representados como sequências de caracteres ou _strings_, permitindo que a rede aprenda a sintaxe de posicionamento de plataformas e inimigos.

#### 4.2. Modelos Generativos Visuais

- **Generative Adversarial Networks (GANs):** Compostas por um modelo gerador e um discriminador treinados em um jogo min-max adversarial. As GANs sintetizam arranjos de _tiles_ ou matrizes de arte a partir de vetores de ruído, capturando o estilo visual de biomas complexos.
- **Autoencoders e Variational Autoencoders (VAEs):** Redes de compressão e reconstrução de dados. Os VAEs projetam o conteúdo em um espaço latente contínuo e estruturado. A navegação e a interpolação nesse espaço latente permitem a fusão de estilos e o _blending_ de domínios distintos de jogos.

#### 4.3. Aprendizado por Reforço Procedural (PCGRL)

No PCGRL, o problema de geração é formulado como um Processo de Decisão de Markov (MDP). Um agente computacional é treinado para atuar como um designer, executando ações discretas de edição sobre o mapa (inserir, remover ou alterar blocos) para maximizar uma função de recompensa baseada em critérios de solvabilidade e qualidade funcional.

---

### 5. Modelos de Linguagem de Larga Escala (LLMs) e o Paradigma MaaS

O surgimento de arquiteturas Transformer baseadas em mecanismos de auto-atenção provocou uma mudança paradigmática na pesquisa de PCG. Como apontado por Maleki & Zhao, a introdução de modelos de linguagem pré-treinados comercializados como serviço (_Model-as-a-Service_ — MaaS) via APIs fechadas alterou o foco acadêmico do treinamento de redes proprietárias para a engenharia de prompts e a integração sistêmica.

```
  ┌──────────────────┐       ┌──────────────────┐       ┌──────────────────┐
  │ Natural Language │ ───►  │  LLM Agent/MaaS  │ ──►│ Structured Level │
  │ Instruction/Text │       │ (OpenAI GPT /...)│       │ (JSON / Grid)    │
  └──────────────────┘       └──────────────────┘       └──────────────────┘
```

Os LLMs destacam-se por sua versatilidade em interpretar contextos em linguagem natural e sintetizar artefatos em múltiplos domínios:

- **Geração de Narrativas e Diálogos:** Aplicações como _Dungeon 2_ e _1001 Nights_ utilizam LLMs para criar ficção interativa de fluxo contínuo.
- **Construção de Cenas e Mundos:** Frameworks como o _SCENECRAFT_ e o _MarioGPT_ convertem instruções textuais de alto nível em arquivos de configuração estruturados (como JSON ou matrizes de caracteres) que descrevem níveis, emoções e gestos de NPCs.
- **Assistência de Co-Criação:** Sistemas como o _CALYPSO_ utilizam LLMs como assistentes para Mestres de Jogo (_Dungeon Masters_) em RPGs de mesa, gerando encontros e descrições ambientais sob demanda.

---

### 6. Arquiteturas Combinadas e Híbridas: A Fronteira do PCG

Embora cada paradigma isole virtudes específicas, métodos isolados apresentam limitações estruturais: GANs podem gerar mapas visualmente fotorrealistas porém fisicamente inatingíveis; LLMs podem hallucinar restrições de jogabilidade; e algoritmos construtivos puros carecem de variabilidade criativa.

A literatura contemporânea converge para o desenvolvimento de **métodos combinados ou híbridos**, considerados a direção mais promissora para a resolução de problemas complexos de geração:

1. **Evolução em Espaço Latente (Latent Variable Evolution — LVE):** Integra a capacidade de representação visual de GANs ou VAEs com o rigor de otimização de Algoritmos Evolutivos (como a combinação CPPN2GAN). O GA navega no espaço latente da GAN para encontrar vetores que resultem em níveis comprovadamente jogáveis.
2. **Híbridos LLM + Aprendizado por Reforço / Algoritmos Genéticos:** Arquiteturas onde o LLM propõe a estrutura narrativa ou o layout inicial do mapa, e um agente de Aprendizado por Reforço Profundo ou um Algoritmo Genético valida, repara ou adapta os parâmetros de dificuldade em tempo real.
3. **Sistemas de Reparo Procedural:** Uso de Autoencoders, solvers de Programação Linear Mista (MIP) ou verificadores de acessibilidade para filtrar e corrigir inconsistências físicas em artefatos gerados por modelos profundos.

---

### 7. Roadmap do Módulo de Fundamentos e Próximos Passos

Este panorama taxonômico encerra a base teórico-conceitual necessária para a navegação nas demais etapas do projeto de extensão. O cronograma de publicações seguirá a sequência planejada:

- **Artigo 3 (Módulo 1):** _Especificidades do PCG em Jogos Cooperativos vs. Single-Player_ (Análise detalhada do estado da arte em ambientes multiplayer, padrões de design cooperativo e métricas de desempenho).
- **Módulo 2 (Algoritmos Construtivos Clássicos):** Tutoriais práticos e implementações interativas cobrindo Funções de Ruído (Página 3), Autômatos Celulares (Página 4), BSP & L-Systems (Página 5 e 6) e Marching Cubes (Página 7).
- **Módulos 3 a 6:** Aprofundamento prático em WFC, SBPCG, PCGML, LLMs e Métricas de Avaliação de Conteúdo.
