# docker-app

Ease your flow with docker-compose.

# Description

docker-app is a small python script builded as a wrapper of docker-compose and its only goal is to reduce your typing and save your precious time.

# Installation

The easy way (using pip):

```
pip install docker-app
```

If everything goes ok, you should have a *docker-app* command

You also can install the command line utility directly using setuptools:

```
git clone https://github.com/diegoacuna/docker-app.git
cd docker-app
python setup.py install
```

## Use cases

### A set of dependent apps

Suppose you have several docker-compose apps that interacts with each other:

 * my_app_1
 * my_app_2
 * my_app_3 => this app depends on my_app_2 and my_app_1

**Without docker-app**

```
# on directory of my_app_1
docker-compose up -d
# now on directory of my_app_2
docker-compose up -d
# now on directory of my_app_3
docker-compose up -d
```

**With docker-app**

You define a set of dependencies in my_app_3 using a docker-app.yml file:

```yaml
dependencies:
  - my_app_1
  - my_app_2
```

and now:

```
docker-app up
```

docker-app is going to do a 'up -d' in all dependencies (in order of appearance) and then in the actual app.

### Launching a bash console in a container

**Without docker-app**

```
docker-compose exec container_name bash
```

**With docker-app**

```
docker-app bash
```

The last example is going to assume that the container to use has the same name that the actual directory where you executed docker-app. If you want to specify a specific container, you can use the *-c* (o **--container**) flag:

```
docker-app bash -c other_container_in_docker_compose
```

### Ruby/Rails integration

docker-app detects if the current app has a Gemfile and allows for a more concise syntax when using bundler or rails:

**Without docker-app**

```
docker-compose exec container_name bundle install
```

**With docker-app**

```
docker-app bundle install
```

**Without docker-app**

```
docker-compose exec container_name bundle exec rails console
```

**With docker-app**

```
docker-app rails console
```

Note that every rails command is executed using *bundle exec*.

The *-c* (*--console*) flag is available to the bundle/rails shortcut also:

```
docker-app rails console -c another_container
```

# TODO

 - Write a wrapper for the exec command
 - test!