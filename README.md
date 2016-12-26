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

By default (as in docker-compose), the name of the main container of an app is assumed to be the name of the actual working directory. You can change this behavior configuring the attribute *main* of the docker-app.yml file:

```yaml
main: 'name_of_container'
```

The file docker-app.yml should be placed on the root directory of your app.

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

You can also do:

```
docker-app stop
```

By default, this command only stops the containers from the docker-compose.yml file in the actual application. If you want to stop the actual app with all the containers in its dependencies, run:

```
docker-app stop --all
```

The same is valid for the restart command:

```
docker-app restart # or restart --all for all the dependencies
```

### Executing stuff on containers

You can use the *exec* command of docker-app:

```
docker-app exec command
docker-app exec command -c another_container # run the command in another container
```

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

NOTE: if you want to launch a command using exec bash (as in the original docker-compose) you need to use the exec wrapper of docker-app (because the -c flag on bash is going to interfere with the -c flag on docker-app):

```
docker-app exec bash "-c bundle exec rails console"
docker-app exec bash "-c bundle exec rails console" -c another_container
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

 - test!
