# Official Elastic App Docker images' Ubuntu version

## Why I create this project

Now the elasitc team choose Centos as their base linux. Which caused lots of problem for my scripts. I hope that I can still use their latest app and scripts but to use Ubuntu as my base linux system. So I wrote this converter to generate ubuntu based Dockerfile.

## How to use

### Tools you need

1. ruby
2. python3.x
3. docker (with docker-compose)
4. jq

### Convert Dockerfile

```
./bin/generate <APP NAME> <APP BRANCH>

# e.g. ./bin/generate elasticsearch 5.5
```

### Build Image

```
cd <APP VERSION>
docker-compose build
```
