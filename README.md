# NinePebbles
Nine Pebbles (Togyzkumalak) -  nomad's board game

## Abstract

## Background 
### About the game
Togyzkumalak, is a traditional board game played in Central Asia, particularly in Kazakhstan and Kyrgyzstan. The name "Togyzkumalak" translates to "nine pebbles" in the Kazakh language. The game is part of the mancala family, which includes various games played with small stones or seeds and rows of holes or pits on a board.

The game is typically played on a special board with two rows of nine pits or cups, each representing a player's side. Small stones, seeds, or other playing pieces are placed in the pits, and players take turns sowing the pieces around the board according to specific rules.

The objective of the game is to capture more seeds than your opponent. The game involves strategic thinking, as players must decide when to sow seeds and when to capture their opponent's seeds. Togyzkumalak has a rich cultural history and is often played in social gatherings, providing entertainment and a way for people to connect.

### About game theory

## Design overview
### Strategies
### Simulations
### Training
### Embedded model


## Implementation details
### Money Carlo Tree Search
### Apache Beam pipeline
Apache Beam Pipeline generates Monte Carlo Search Tree (most promissing game states with corresponding outcomes). See details in https://github.com/Parabul/game.
```java
    ExplorationPipelineOptions options =
        PipelineOptionsFactory.fromArgs(args).withValidation().as(ExplorationPipelineOptions.class);

    Pipeline pipeline = Pipeline.create(options);

    List<Integer> instances =
        IntStream.range(0, options.getNumSeeds()).boxed().collect(Collectors.toList());

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

    pipeline.run().waitUntilFinish();
```
### State Encoding
### Tensorflow model
### Flutter/Dart

## Model evaluation


<a href='https://play.google.com/store/apps/details?id=kz.ninestones.game_client&pcampaignid=pcampaignidMKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1'><img alt='Get it on Google Play' src='https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png' style="width:200px"/></a>
