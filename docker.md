## Docker


**Cheat Sheet**

- Running a container

```
docker run -dit -v source-dir:target-dir -p host-port:container-port --name container-name image-id

where:
  -d or --detach: run container in background mode.
  -i or --interactive: keep STDIN open even if not attached
  -t or --tty: allocate a pseudo terminal
  -v or --volume list: bind mount a volume
  source-dir: source directory informed as source of the volume.
  target-dir: target directory into the container.

e.g.:
  docker run -dit -v $(pwd):/go/src -p 8080:8080 --name golang dd6f
```

- Removing a container

```
docker rm -f <container>
```  
  
- Removing an image

```
docker rmi <image>
```
