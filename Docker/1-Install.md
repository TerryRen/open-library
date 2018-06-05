### What
[What is the Docker?](https://yeasy.gitbooks.io/docker_practice/introduction/what.html)

### Why
[Why use the Docker?](https://yeasy.gitbooks.io/docker_practice/introduction/why.html)

### Concept
+ Image
+ Container
+ Repository


### Ubuntu

#### Required version
+ Trusty 14.04 (LTS)
+ Xenial 16.04 (LTS)
+ Artful 17.10 +


#### Install

```bash
sudo apt-get update
sudo apt-get install docker-ce
```

#### Bootstrap

##### Ubuntu 16.04 +
```bash
sudo systemctl enable docker
sudo systemctl start docker
```

##### Ubuntu 14.04
```bash
sudo service docker start
```


#### Verify

```bash
sudo docker run hello-world
```