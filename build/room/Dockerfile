FROM node:16-alpine as builder

RUN apk --no-cache add git

WORKDIR /work/repo
ARG EXCALIDRAW_ROOM_REV=master
RUN git init
RUN git remote add origin https://github.com/excalidraw/excalidraw-room.git
RUN git fetch --depth 1 origin "${EXCALIDRAW_ROOM_REV}"
RUN git checkout FETCH_HEAD

WORKDIR /work/app
RUN cp /work/repo/package.json  .
RUN cp /work/repo/yarn.lock     .
RUN yarn

RUN cp    /work/repo/tsconfig.json     .
RUN cp -R /work/repo/src/              .
RUN yarn build


FROM node:16-alpine

WORKDIR /app

COPY --from=builder /work/app  .

ENV PORT=8080
EXPOSE 8080

CMD ["yarn", "start"]
