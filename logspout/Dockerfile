FROM gliderlabs/alpine:3.3
#ENTRYPOINT ["/bin/logspout"]
VOLUME /mnt/routes
EXPOSE 80

COPY . /src
RUN cd /src && ./build.sh "$(cat VERSION)"
COPY ./dockerize-linux-amd64-v0.1.0.tar.gz /bin/dockerize-linux-amd64-v0.1.0.tar.gz
RUN cd /bin && tar -xzvf dockerize-linux-amd64-v0.1.0.tar.gz

ONBUILD COPY ./build.sh /src/build.sh
ONBUILD COPY ./modules.go /src/modules.go
ONBUILD RUN cd /src && ./build.sh "$(cat VERSION)-custom"


