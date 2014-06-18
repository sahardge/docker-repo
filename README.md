docker-repo
===========

This is a repo which allows you to quickly start a private docker registry repository on your host/server.

Requires
===========
*Latest version of Docker installed on the machine you want the docker registry to be hosted on [code]docker pull registry[/code]

HOW TO USE 
===========
1. Run `docker pull registry`

2. If you want to keep your repository data to persist through reboots, make a directory wherever you want to keep this. For the purpose of this guide we'll put it in `/registry`

3. The registry configuration comes from a `config.yml` file, however at the time of writing the present version (0.7.2) does not come pre-configured to work as a standalone server. Luckily it only takes the change of one environment variable in the `config.yml` so its painless.

4. Put the following into a file called `config.yml` in your mounted host directory:
```dev:
    storage: local
    storage_path: /registry
    loglevel: debug
    standalone: true # do not use public index
    secret_key: test-registry
```
5. Run `docker run -p 5000:5000 -v /registry:/registry -e DOCKER_REGISTRY_CONFIG=/registry/config.yml registry`

6. You now have a private docker registry running in its own docker container! 

7. In order to test, on another host where docker is installed, pull any public docker image, such as `docker pull coreos/apache`

8. Run `docker tag coreos/apache "repo_host_ip"/apache`, where "repo_host_ip" being the IP address of the host where the docker registry is running.

9. Run `docker push "repo_host_ip"/apache`. This will push your image to your private registry.

10. In order to test pulling from it, delete the image you just pushed to your repo with `docker rmi "repo_host_ip"/apache`, and then subsequently run `docker pull "repo_host_ip/apache". This will pull the image you just removed, this time being from your private registry.

Credits and More Info
==========
The official docker registry repository image can be found at [github](https://github.com/dotcloud/docker-registry)



