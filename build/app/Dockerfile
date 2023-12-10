FROM node:18 as builder

RUN apt-get update
RUN apt-get install -y --no-install-recommends git

WORKDIR /work/repo
ARG EXCALIDRAW_REV=master
RUN git init
RUN git remote add origin https://github.com/excalidraw/excalidraw.git
RUN git fetch --depth 1 origin "${EXCALIDRAW_REV}"
RUN git checkout FETCH_HEAD

WORKDIR /work/app
RUN cp /work/repo/package.json  .
RUN cp /work/repo/yarn.lock     .
RUN yarn --ignore-optional --network-timeout 600000

ARG NODE_ENV=production
RUN cp    /work/repo/.env.production   .
RUN cp    /work/repo/.eslintrc.json    .
RUN cp    /work/repo/.npmrc            .
RUN cp    /work/repo/.nvmrc            .
RUN cp    /work/repo/.lintstagedrc.js  .
RUN cp    /work/repo/.watchmanconfig   .
RUN cp    /work/repo/*.json            .
RUN cp    /work/repo/*.ts              .
RUN cp    /work/repo/*.html            .
RUN cp -R /work/repo/public/           .
RUN cp -R /work/repo/src/              .
RUN cp -R /work/repo/scripts/          .
RUN cp -R /work/repo/excalidraw-app/   .

RUN sed -i -E 's|^(\s*"excalidraw\.production\.min"\s*:\s*)"\./entry\.js"|\1"../src/excalidraw-app/index.js"|'            ./src/packages/excalidraw/webpack.prod.config.js
RUN sed -i -E 's|^\s*VITE_APP_WS_SERVER_URL\s*=.*$|VITE_APP_WS_SERVER_URL=https://dummy-collaboration-server.example|'    ./.env.production
RUN yarn build:app:docker

FROM nginxinc/nginx-unprivileged:1.25

COPY --from=builder /work/app/build  /usr/share/nginx/html

COPY ./assets   /work

HEALTHCHECK CMD wget -q -O /dev/null http://localhost:8080 || exit 1

CMD ["sh", "/work/startup/run.sh"]
