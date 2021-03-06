# Installing Clear Containers 3.0 on Fedora

Note:

If you are installing on a system that already has Clear Containers 2.x
installed, first read [the upgrading document](upgrading.md).

Clear Containers **3.0** is available for Fedora\* versions **24** , **25** and **26**.

This step is only required in case Docker is not installed on the system.
1. Install the latest version of Docker with the following commands:

```
$ sudo dnf -y install dnf-plugins-core
$ sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
$ sudo dnf makecache
$ sudo dnf -y install docker-ce
```

For more information on installing Docker please refer to the
[Docker Guide](https://docs.docker.com/engine/installation/linux/fedora)

2. Install the Clear Containers 3.0 components with the following commands:

```
$ source /etc/os-release
$ sudo -E VERSION_ID=$VERSION_ID dnf config-manager --add-repo \
http://download.opensuse.org/repositories/home:/clearcontainers:/clear-containers-3/Fedora\_$VERSION_ID/home:clearcontainers:clear-containers-3.repo
$ sudo -E dnf -y install cc-runtime cc-proxy cc-shim
```

3.  Configure Docker to use Clear Containers by default with the following commands:

```
$ sudo mkdir -p /etc/systemd/system/docker.service.d/
$ cat << EOF | sudo tee /etc/systemd/system/docker.service.d/clear-containers.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D --add-runtime cc-runtime=/usr/bin/cc-runtime --default-runtime=cc-runtime
EOF
```

4. Restart the Docker and Clear Containers systemd services with the following commands:

```
$ sudo systemctl daemon-reload
$ sudo systemctl enable docker.service
$ sudo systemctl restart docker
$ sudo systemctl enable cc-proxy.socket
$ sudo systemctl start cc-proxy.socket
```

5. Run Clear Containers 3.0.

   You are now ready to run Clear Containers 3.0:

   ```
   $ sudo docker run -ti busybox sh
   ```
