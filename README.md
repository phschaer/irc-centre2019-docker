# IRC-CENTRE2019 Docker Image

TODO: Travis

[**Timo Breuer**](https://github.com/breuert/) and [**Philipp Schaer**](https://github.com/phschaer/)

This is the docker image for our replicated submission to [CENTRE@CLEF2019](http://www.centre-eval.org/clef2019/) conforming to the [OSIRRC jig](https://github.com/osirrc/jig/) for the [Open-Source IR Replicability Challenge (OSIRRC 2019) at SIGIR 2019](https://osirrc.github.io/osirrc2019/).
This image is available on [Docker Hub](https://hub.docker.com/r/osirrc2019/irc-centre2019) has been tested with the jig at commit [ca31987](https://github.com/osirrc/jig/commit/ca3198704795f2b6de8b78ed7a66bbdf1dccadb1) (6/5/2019).

+ Supported test collections: `core17`
+ Required training collections: `robust04`, `robust05`
+ Supported hooks: `init`, `index`, `search`

## Quick Start

Use the commands below to get the runs for [WCRobust04 and WCRobust0405](https://trec.nist.gov/pubs/trec26/papers/WaterlooCormack-CC.pdf) as they were replicated in the course of our participation in [CENTRE@CLEF19](http://www.centre-eval.org/clef2019/). <br>

The following `jig` command can be used to index the New York Times corpus and prepare training data for WCRobust04:

```
python run.py prepare \
    --repo osirrc2019/irc-centre2019 \
    --collections robust04=/path/to/robust04/=trectext \
                  core17=/path/to/core17/=trectext \
    --opts run="wcrobust04"
```

The argument `run` can be customized to `run`="wcrobust0405" in order to prepare training data for WCRobust0405.
In this case, the `robust05` corpus has to be mounted as an additional volume.

```
python run.py prepare \
    --repo osirrc2019/irc-centre2019 \
    --collections robust04=/path/to/robust04/=trectext \
                  robust05=/path/to/robust05/=trectext \
                  core17=/path/to/core17/=trectext \
    --opts run="wcrobust0405"
```

The following `jig` command can be used to perform a retrieval run on the New York Times depending on the previously defined training corpora.

```
python run.py search \
    --repo osirrc2019/irc-centre2019 \
    --collection core17 \
    --topic topics/topics.core17.txt \
    --output /path/to/output/ \
    --qrels qrels/qrels.core17.txt
```

## Expected Results

TODO: add outcomes

## Implementation

The following is a short summary of what happens in each of the scripts in this repo.

### Dockerfile

The `Dockerfile` installs `python3`, copies scripts for corresponding hooks and makes required directory. The working directory is set to  `/work/`

### init

The `init` [script](init) will download the code from a [repository](https://github.com/breuert/wcrobust0405-code/) and installs required Python packages from the requirements.txt [file](https://github.com/breuert/wcrobust0405-code/blob/master/requirements.txt). Depending on the specified run, scripts for WCRobust04 or WCRobust0405 will be prepared.

### index

The `index` [script](index) runs a subprocess which starts indexing.

### search

The `search` [script](search) will start the ranking depending on the previously specified run.
