# NinePebbles
Nine Pebbles (Togyzkumalak) -  nomad's board game

<a href='https://play.google.com/store/apps/details?id=kz.ninestones.game_client&pcampaignid=pcampaignidMKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1'><img alt='Get it on Google Play' src='https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png' style="width:200px"/></a>

## Abstract

## Background 
### About the game
Togyzkumalak, is a traditional board game played in Central Asia, particularly in Kazakhstan and Kyrgyzstan. The name "Togyzkumalak" translates to "nine pebbles" in the Kazakh language. The game is part of the mancala family, which includes various games played with small stones or pebbles and rows of holes or pits on a board.

The game is typically played on a special board with two rows of nine pits or cups, each representing a player's side. Small stones, pebbles, or other playing pieces are placed in the pits, and players take turns sowing the pieces around the board according to specific rules.

The objective of the game is to capture more pebbles than your opponent. The game involves strategic thinking, as players must decide when to sow pebbles and when to capture their opponent's pebbles. Togyzkumalak has a rich cultural history and is often played in social gatherings, providing entertainment and a way for people to connect.

### Game theory
[Game theory](https://en.wikipedia.org/wiki/Game_theory), a branch of mathematics and economics, provides a valuable framework for understanding strategic decision-making in various interactive scenarios, including mobile gaming. At its core, game theory analyzes the choices of rational actors within competitive situations, where the outcome of one player's decision depends on the decisions of others. In the context of mobile gaming, developers often employ game theoretical concepts to design engaging gameplay mechanics, balance competition, and incentivize player interaction.

Moreover, the advancement of artificial intelligence (AI) in mobile gaming has significantly impacted gameplay [experiences](https://pixelplex.io/blog/how-ai-enhances-game-development/), many modern game engines utilize sophisticated AI algorithms like [AlphaZero](https://en.wikipedia.org/wiki/AlphaZero), [Leela Zero](https://en.wikipedia.org/wiki/Leela_Zero). These algorithms enable AI opponents to assess potential moves, predict outcomes, and make strategic decisions, enhancing the challenge and depth of gameplay for players. This article explores how game theory principles, coupled with AI techniques, can be leveraged in board games, shaping player experiences and strategic outcomes.

#### Evaluation function
In game theory, [an evaluation function](https://en.wikipedia.org/wiki/Evaluation_function) is a mathematical function used to assess the desirability of a particular game state or position. It assigns a numerical value to each possible state of the game, indicating how favorable or unfavorable it is for a player. The evaluation function typically takes into account various factors such as the current board configuration, the potential for future moves, the relative strength of pieces, and strategic considerations.

Evaluation functions are commonly employed in games with discrete states, such as chess, checkers, and Go, where it is not feasible to exhaustively analyze every possible move. Instead, an evaluation function guides the search process by providing a heuristic estimate of the value of a given position. In algorithms like Minimax and Monte Carlo Tree Search (MCTS), the evaluation function plays a crucial role in assessing the quality of game states and guiding decision-making.

The design of an effective evaluation function often requires domain-specific knowledge and expertise in the game being analyzed. It aims to capture important features of the game that influence the likelihood of winning or losing.

#### Minimax Theorem
Minimax is a decision rule used in game theory for minimizing the possible loss for a worst-case scenario. It is commonly employed in two-player zero-sum games, where one player's gain is equivalent to the other player's loss (e.g., chess, tic-tac-toe). The Minimax theorem asserts that in such games, each player selects moves to minimize the maximum possible loss. This concept is fundamental to creating AI agents that can play strategically against human players or other AI opponents.

#### Monte Carlo Tree Search (MCTS)
MCTS is a heuristic search algorithm used in decision processes, particularly in games with large branching factors and incomplete information. It builds a search tree by simulating random sequences of moves, evaluating the outcomes, and selecting the most promising moves based on the results of these simulations. Over multiple iterations, MCTS refines its search tree to focus on the most relevant parts of the game space, thereby improving decision-making efficiency.

While Minimax and MCTS are distinct techniques, they can be complementary, with Minimax providing principles for strategic decision-making and MCTS offering efficient exploration and exploitation of the game space. Integrating aspects of Minimax into MCTS enhances its performance in decision processes, especially in complex games. MCTS typically uses random simulations to evaluate nodes in the search tree, integrating a heuristic evaluation function guided by Minimax provides more accurate assessments of game states. This can guide MCTS towards more promising branches of the search tree.

### Integration of Neural Networks and Monte Carlo Tree Search
In the realm of strategic board games, the synergy between deep neural networks and Monte Carlo Tree Search (MCTS) has revolutionized gameplay. AlphaGo, a pioneering example, showcases this integration brilliantly. Neural networks, particularly deep ones, are trained to predict optimal moves and evaluate game states. Meanwhile, MCTS efficiently explores the game tree, providing valuable data for network training.

The process involves iteratively refining the neural network's understanding of the game through self-play and reinforcement learning. With each iteration, the network becomes more adept at predicting moves and assessing game states accurately. By combining the insights from neural networks with the search capabilities of MCTS, these systems achieve remarkable gameplay performance, outmaneuvering even top human players.

## Implementation details
### Money Carlo Tree Search
### Apache Beam pipeline
Apache Beam Pipeline generates Monte Carlo Search Tree (most promissing game states with corresponding outcomes). See details in https://github.com/Parabul/game.
```java
    ExplorationPipelineOptions options =
        PipelineOptionsFactory.fromArgs(args).withValidation().as(ExplorationPipelineOptions.class);

    Pipeline pipeline = Pipeline.create(options);

    List<Integer> instances =
        IntStream.range(0, options.getNumpebbles()).boxed().collect(Collectors.toList());

    pipeline
        .apply("Create seeds", Create.of(instances))
        .apply(
            "Generate Random Game States",
            MapElements.into(BeamTypes.stateProtos).via(i -> GameSimulator.randomState().toProto()))
        .apply("Expand Monte Carlo Search Tree", ParDo.of(new ExpandFn()))
        .apply(
            "Enrich Less Visited Nodes",
            MapElements.into(BeamTypes.stateNodes)
                .via(MonteCarloTreeSearchExplorationPipeline::enrich))
        .apply(
            "Encode As TensorFlow Example",
            MapElements.into(BeamTypes.examples)
                .via(MonteCarloTreeSearchExplorationPipeline::encode))
        .apply(
            "Map To ByteArrays", MapElements.into(BeamTypes.byteArrays).via(Example::toByteArray))
        .apply(
            "Write TFRecords",
            TFRecordIO.write()
                .withNumShards(options.getNumOutputShards())
                .to(options.getOutputPath()));

    pipeline.run();
```

### Tensorflow model
#### Model evaluation
### Flutter/Dart


### Strategies
### Simulations
### Training
### Embedded model


## Model evaluation



