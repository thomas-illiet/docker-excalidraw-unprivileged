# Excalidraw multiple platforms Docker Image

This is a docker image of the online drawing tools [excalidraw](https://github.com/excalidraw/excalidraw) and [excalidraw-room](https://github.com/excalidraw/excalidraw-room).  
Multi-architecture image built with buildx.  

## Dockerfile

- [thomasilliet/excalidraw-front-unprivileged](https://github.com/thomasilliet/docker-excalidraw/tree/main/build/app)
- [thomasilliet/excalidraw-room-unprivileged](https://github.com/thomasilliet/docker-excalidraw/tree/main/build/room)

## Example

A simple local try docker compose file.

```yaml
version: '3.5'

services:
  app:
    image: thomasilliet/excalidraw-front-unprivileged:v0.17.0
    restart: unless-stopped
    ports:
      - "8001:8080"
    environment:
      - REACT_APP_WS_SERVER_URL=http://localhost:8002/

  collabo:
    image: thomasilliet/excalidraw-room-unprivileged:v2023.05.27
    restart: unless-stopped
    ports:
      - "8002:8080"
```

Run and access http://localhost:8001/  

```bash
$ docker-compose up
```

## Enviroment variables

- thomasilliet/excalidraw-front-unprivileged
    - `VITE_APP_WS_SERVER_URL`  
      Collaboration server URL.  
      This must always be specified, as the default is an invalid dummy URL.
