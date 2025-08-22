mkdir downloads

## To copy the rclone config
- cp ~/.config/rclone/rclone.conf ./confg/rclone.conf

## for permissions
- sudo chown -R 1000:1000 ./downloads
- sudo chmod -R 775 ./downloads

## to run the docker
- sudo docker compose up -d

## to stop the docker   
- sudodocker compose down


## All endpoints
- aria-ng: https://go.mstservices.tech
- filebrowser: https://go.mstservices.tech/files
- readonly: https://go.mstservices.tech/ro
- rclone: https://go.mstservices.tech/rclone
- Gdrive Index: https://luffy.mst-uploads.workers.dev/0:/