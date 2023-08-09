
```
docker stop "id-portainer-container"
docker pull portainer/helper-reset-password
docker run --rm -v portainer_data:/data portainer/helper-reset-password
docker start "id-portainer-container"
```