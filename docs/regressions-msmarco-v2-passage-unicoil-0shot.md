# Anserini: Regressions for [MS MARCO (V2) Passage Ranking](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)

This page describes regression experiments for passage ranking on the MS MARCO (V2) passage corpus using the dev queries, which is integrated into Anserini's regression testing framework.
Here, we cover experiments with the uniCOIL model trained on the MS MARCO V1 passage ranking test collection, applied in a zero-shot manner, with doc2query-T5 expansion.

The exact configurations for these regressions are stored in [this YAML file](../src/main/resources/regression/msmarco-v2-passage-unicoil-0shot.yaml).
Note that this page is automatically generated from [this template](../src/main/resources/docgen/templates/msmarco-v2-passage-unicoil-0shot.template) as part of Anserini's regression pipeline, so do not modify this page directly; modify the template instead.

## Indexing

Typical indexing command:

```
target/appassembler/bin/IndexCollection \
  -collection JsonVectorCollection \
  -input /path/to/msmarco-v2-passage-unicoil-0shot \
  -index indexes/lucene-index.msmarco-v2-passage-unicoil-0shot/ \
  -generator DefaultLuceneDocumentGenerator \
  -threads 18 -impact -pretokenized \
  >& logs/log.msmarco-v2-passage-unicoil-0shot &
```

The value of `-input` should be a directory containing the compressed `jsonl` files that comprise the corpus.
For additional details, see explanation of [common indexing options](common-indexing-options.md).

## Retrieval

Topics and qrels are stored in [`src/main/resources/topics-and-qrels/`](../src/main/resources/topics-and-qrels/).
These regression experiments use the [dev queries](../src/main/resources/topics-and-qrels/topics.msmarco-v2-passage.dev.txt) and the [dev2 queries](../src/main/resources/topics-and-qrels/topics.msmarco-v2-passage.dev2.txt).

After indexing has completed, you should be able to perform retrieval as follows:

```
target/appassembler/bin/SearchCollection \
  -index indexes/lucene-index.msmarco-v2-passage-unicoil-0shot/ \
  -topics src/main/resources/topics-and-qrels/topics.msmarco-v2-passage.dev.unicoil.0shot.tsv.gz \
  -topicreader TsvInt \
  -output runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev.unicoil.0shot.txt \
  -impact -pretokenized &
target/appassembler/bin/SearchCollection \
  -index indexes/lucene-index.msmarco-v2-passage-unicoil-0shot/ \
  -topics src/main/resources/topics-and-qrels/topics.msmarco-v2-passage.dev2.unicoil.0shot.tsv.gz \
  -topicreader TsvInt \
  -output runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev2.unicoil.0shot.txt \
  -impact -pretokenized &
```

Evaluation can be performed using `trec_eval`:

```
tools/eval/trec_eval.9.0.4/trec_eval -c -m recall.100 src/main/resources/topics-and-qrels/qrels.msmarco-v2-passage.dev.txt runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev.unicoil.0shot.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -m recall.1000 src/main/resources/topics-and-qrels/qrels.msmarco-v2-passage.dev.txt runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev.unicoil.0shot.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -M 100 -m map -c -M 100 -m recip_rank src/main/resources/topics-and-qrels/qrels.msmarco-v2-passage.dev.txt runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev.unicoil.0shot.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -m recall.100 src/main/resources/topics-and-qrels/qrels.msmarco-v2-passage.dev2.txt runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev2.unicoil.0shot.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -m recall.1000 src/main/resources/topics-and-qrels/qrels.msmarco-v2-passage.dev2.txt runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev2.unicoil.0shot.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -M 100 -m map -c -M 100 -m recip_rank src/main/resources/topics-and-qrels/qrels.msmarco-v2-passage.dev2.txt runs/run.msmarco-v2-passage-unicoil-0shot.unicoil-0shot.topics.msmarco-v2-passage.dev2.unicoil.0shot.txt
```

## Effectiveness

With the above commands, you should be able to reproduce the following results:

| MAP@100                                                                                                      | uniCOIL (zero-shot)|
|:-------------------------------------------------------------------------------------------------------------|-----------|
| [MS MARCO V2 Passage: Dev](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                      | 0.1485    |
| [MS MARCO V2 Passage: Dev2](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                     | 0.1561    |


| MRR@100                                                                                                      | uniCOIL (zero-shot)|
|:-------------------------------------------------------------------------------------------------------------|-----------|
| [MS MARCO V2 Passage: Dev](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                      | 0.1499    |
| [MS MARCO V2 Passage: Dev2](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                     | 0.1577    |


| R@100                                                                                                        | uniCOIL (zero-shot)|
|:-------------------------------------------------------------------------------------------------------------|-----------|
| [MS MARCO V2 Passage: Dev](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                      | 0.5518    |
| [MS MARCO V2 Passage: Dev2](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                     | 0.5661    |


| R@1000                                                                                                       | uniCOIL (zero-shot)|
|:-------------------------------------------------------------------------------------------------------------|-----------|
| [MS MARCO V2 Passage: Dev](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                      | 0.7616    |
| [MS MARCO V2 Passage: Dev2](https://microsoft.github.io/msmarco/TREC-Deep-Learning.html)                     | 0.7671    |