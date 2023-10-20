
This setup helps to build any Linux (Yocto, OpenWRT, etc ...) in an isolated environment with docker.
To build something inside an docker will preserve the build environment, especially when
the host machine is quite often to be upgraded.

This repo is insipired by https://github.com/nxp-imx/imx-docker and https://github.com/hnakayam/imx-docker-clone.

Prerequisites
=============

Install Docker
--------------

There are various methods of installing [docker], i.e. by docker script:
  ```{.sh}
  $ curl -fsSL https://get.docker.com -o get-docker.sh
  $ sudo sh get-docker.sh
  ```

Run docker without sudo
-----------------------

To work better with docker, without `sudo`, add your user to `docker group`.
  ```{.sh}
  $ sudo usermod -aG docker <your_user>
  ```

Log out and log back in so that your group membership is re-evaluated.

Set docker to work with proxy
-----------------------------

Create a docker config file at `~/.docker/config.json` and enter the following:

```{.sh}
{
"proxies":
    {
     "default":
         {
          "httpProxy":"http://proxy.example.com:80"
         }
    }
}
```
Note: replace the 'example' proxy with your proxy info.

Create docker service
---------------------
  ```{.sh}
  $ sudo mkdir -p /etc/systemd/system/docker.service.d
  $ sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf
  ```

add the following:

```{.sh}
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80/"
Environment="NO_PROXY=localhost,someservices.somecompany.com"
```

Restart Docker

```{.sh}
  $ sudo systemctl daemon-reload
  $ sudo systemctl restart docker
```

Set variables
-------------

Use `env.sh` to set variables for your build setup. Make sure you have 
created a working directory, owned by current user, on a larger partition.


[docker]: https://docs.docker.com/engine/install/ubuntu/ "DockerInstall/Ubuntu"
