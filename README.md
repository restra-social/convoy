# convoy-daemon

Runs [convoy](https://github.com/rancher/convoy) in a container.

# Usage

The container needs to be able to configure itself as a plugin and communicate with the docker socket.

E.g. to use the VFS/NFS backend:

```bash
docker run -d -v /var/run/docker.sock:/var/run/docker.sock -v /etc/docker/plugins:/etc/docker/plugins -v /opt/storage:/opt/storage fahim74/convoy daemon --drivers vfs --driver-opts vfs.path=/opt/storage/convoy-volumes
```
E.g to use with Device Mapper

```bash
truncate -s 100G data.vol
truncate -s 1G metadata.vol
sudo losetup /dev/loop5 data.vol
sudo losetup /dev/loop6 metadata.vol

docker run -d -v /var/run/docker.sock:/var/run/docker.sock -v /etc/docker/plugins:/etc/docker/plugins -v /opt/storage:/opt/storage fahim74/convoy daemon --drivers devicemapper --driver-opts dm.datadev=/dev/loop5 --driver-opts dm.metadatadev=/dev/loop6

```
# Extra

```bash
alias convoy='docker exec convoy-nfs convoy'
convoy list
```