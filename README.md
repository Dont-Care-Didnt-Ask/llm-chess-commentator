# Chess Commentator

Chess beginners often have problems analyzing games by themselves - it is sometimes hard to understand, why chess engines propose this or that line.

Let's try to overcome this issue by using Mistral-7B to annotate chess games in a human-understandable way.

## Quickstart

Step 1: install [micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html).

One can also use conda or mamba, but in my experience, conda is sometimes too slow. Micromamba also has the benefit of being tiny.

Step 2: create an environment.
```
micromamba create -n llmcomment python=3.11 pytorch=2.1.2 pytorch-cuda=12.1 transformers=4.34 sentencepiece=0.1.99 accelerate=0.26.1 bitsandbytes=0.42.0 peft=0.7.1 datasets=2.14.7 matplotlib=3.8.2 pandas=2.2.0 jupyterlab=4.0.11 ipywidgets=8.1.1 jupyterlab_widgets=3.0.9 tqdm=4.66.1 -c pytorch -c nvidia -c conda-forge
micromamba activate llmcomment

which pip
# ensure that you are using pip installed in the environment
pip install chess
```

Step 3: open one of the notebooks and play around -- I suggest starting with `zero_and_few_shot`.


__Troubleshooting__. I had issues with getting `bitsandbytes` to work. Eventually, it helped to build it from source:
```
git clone https://github.com/TimDettmers/bitsandbytes.git
cd bitsandbytes/
CUDA_VERSION=121 CUDA_HOME=<your/path/to/cuda> make cuda12x
python setup.py install
python -m bitsandbytes
```

## TODO

- [x] Try zero-shot. 
- [x] Try few-shot.
- [ ] Establish manual evaluation pipeline -- select a fixed set of games, for which good annotations are available. _In progress, 3 games selected for now._
- [ ] Finetune with LoRA on available data. _In progress._
- [ ] Collect more data, and build a curated dataset of annotated chess games in PGN format.
- [ ] Try models other than Mistral.
- [ ] Try non-LLM approaches, or approaches incorporating a classical chess engine / chess neural network.


## Data
Here is a list of potential data sources. For now I only use a subset of the games from the first link.

Online:
- [PGN Library](https://www.angelfire.com/games3/smartbridge/) -- about 4000 annotated games in PGN (not all in English, though).
- [Quickest Games](https://www.chessgames.com/perl/chesscollection?cid=1000554) collection on chessgames.com -- some of the quickest games that end in a checkmate. All of them are in 6 moves or less. Original comments are limited, but it's easy to re-annotate short games manually.
- [Gameknot](https://gameknot.com/best-annotated-games.pl) -- annotated games from an old chess site. Comments are of varying quality, sometimes they contain spelling and grammatical errors.
- Simply [search results](https://www.chessgames.com/perl/ezsearch.pl?search=annotated) for annotated games on chessgames.com.

Books:
- Logical Chess: Move by Move, Irving Chernev
- Instructive Chess Miniatures, Alper Efe Ataman
- Understanding Chess Move by Move, John Nunn
- ...

## Acknowledgements

QLoRA finetuning script is build using [this](https://github.com/geronimi73/qlora-minimal/tree/main) example.
