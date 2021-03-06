# openattic-dev Docker image

This docker image will run openATTIC from the source. It will spawn all the
necessary services such as apache, and postgresql, and is already ready for
managing a Ceph cluster.

You can also run the openATTIC django unit tests using this image.

Please choose which distro you want to use for running openATTIC, a execute
the instructions below inside the distro directory.

## Usage instructions

### Image build

`docker build -t openattic-dev .`

### Running the openattic-dev container

* Clone the bitbucket openattic repo
`git clone https://bitbucket.org/openattic/openattic`

* Assuming openattic repo is located in `/home/oa/openattic` and the Ceph
configuration and keyring files are located in `/etc/ceph`, run the following
command:
```
docker run -t -v /home/oa/openattic:/srv/openattic \
		      -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
		      -v /etc/ceph:/etc/ceph \
		      --net=host --privileged \
		      --security-opt seccomp=unconfined \
		      --stop-signal=SIGRTMIN+3 \
		      --tmpfs /run/lock  \
		      openattic-dev [salt_api_shared_secret_key]
```

The container will populate the database, configure nagios monitoring, and
start apache.
In the end it will run the `grunt dev` command on the `openattic/webui`
directory to keep the changes of the ui in sync.

If you specify the optional argument `salt_api_shared_secret_key`, that will
be appended automatically to the openATTIC settings file to configure the
Salt-API connection.

After this step you can access openATTIC gui in http://localhost/openattic/

* When the container finishes emitting output, you can `Ctrl-C`. The container
will keep running nevertheless.

* To check the openAttic backend log run the following command:
`docker exec CONTAINER_ID tail -f /var/log/openattic/openattic.log`

### Running the django unit tests

* Clone the bitbucket openattic repo
`hg clone https://bitbucket.org/openattic/openattic`

* Assuming openattic repo is located in `/home/oa/openattic`:
```
docker run -t -v /home/oa/openattic:/srv/openattic \
		      -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
		      --net=host --privileged \
		      --security-opt seccomp=unconfined \
		      --stop-signal=SIGRTMIN+3 \
		      --tmpfs /run/lock --rm \
		      openattic-dev tests
```
This is will start a container that will run the django unit tests using the
source code present in `/home/oa/openattic`.

This container is removed automatically when it finishes the execution of the
unit tests.


## HTTP proxy

If you are behind an HTTP proxy server, for example in corporate settings, you will need to:

* Add proxy configuration to the `DockerFile`:
```
ENV http_proxy http://<proxy.server>:<proxy.port>
ENV https_proxy http://<proxy.server>:<proxy.port>
```
* Add `--network=host` option when running `docker build`

