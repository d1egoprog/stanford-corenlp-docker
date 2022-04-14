# Stanford NLP Core

This Dockerfile downloads, configure, and runs the 
[Stanford CoreNLP server](http://stanfordnlp.github.io/CoreNLP/corenlp-server.html) as a Docker Image. This build is intended to be used on a personal or small research team projects.

Currently the builded version is 4.1.0.

## Deploy Alternatives

For more imformation on the configuration and funcionality of the [Stanford OpenNLP Server](https://stanfordnlp.github.io/CoreNLP/corenlp-server.html) use the official documentation.

To build the image locally clone the repository: 

```
git clone https://github.com/d1egoprog/stanford-corenlp-docker.git
```
### Deploy using docker CLI

Use the `docker` command cli tool and run:

```
docker build -t stanford-corenlp:4.1.0 stanford-corenlp/.
docker run -p 9000:9000 stanford_corenlp:4.1.0
```

All the JVM parameters can be accesed by editing the Dockerfile and rebuilding the image, by default the parameters configured are:

```
ENV JAVA_XMX 8G
ENV ANNOTATORS all
ENV TIMEOUT_MILLISECONDS 60000
ENV THREADS 5
ENV MAX_CHAR_LENGTH 100000
ENV PORT 9000
```

If you not want to edit the Dockerfile the environment variables can be overwriten from the `docker run` command, eg. changing the JVM memory parameter `JAVA_XMX` to reserve more memory. 

```
docker run -e JAVA_XMX=12g -p 9000:9000 -ti stanford_corenlp:4.1.0
```

### Deploy using docker-compose

If is prefered a docker compose file also is also availabe with the standard build from the docker file and an override configuration (same parameters from Dockerfile), this should be changed to set your desired annotators and specific computing requirements. To run the service run the command:

```
docker-compose up -d
```

Or use the compose file pulling the Docker Hub image, storing the following into a new `docker-compose.yml` file.

```
version: '3.7'

services:
  stanford_corenlp:
    image: d1egoprog/stanford-corenlp:4.1.0
    ports:
      - "9000:9000"
    restart: always
```

Check the the prebuilt version of the [image](https://hub.docker.com/r/d1egoprog/stanford-corenlp) from Docker Hub.

## Testing the Installation

To check the funcionallity, you can open a web browser window to your docker engine `IP` and the choosen service eg. `PORT=9000` , normally on [localhost:9000](http://localhost:9000). 

### Consuming by HTTP request

Also to test your local or remote service just send the following curl request into your prefered CLI.

```
curl --data 'The quick brown fox jumped over the lazy dog.' 'http://localhost:9000/?properties={%22annotators%22%3A%22tokenize%2Cssplit%2Cpos%22%2C%22outputFormat%22%3A%22json%22}' -o -
```

### Consuming by Library

To use the official phython library [`stanza`](https://stanfordnlp.github.io/stanza/) a small example has been prepared in a jupyter notebook, [stanza-example](https://github.com/d1egoprog/stanford-corenlp-docker/blob/main/stanza-example.ipynb)
