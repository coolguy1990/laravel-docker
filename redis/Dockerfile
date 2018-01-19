FROM redis:latest

MAINTAINER Kunal Mangaraj <mangaraj.kunal@gmail.com>

VOLUME /data

EXPOSE 6379

CMD ["redis-server"]

# docker build --no-cache -t redis .
# docker run -d --name redis redis
#  docker run -d --link redis:redis redis redis-cli -h redis -p 6379 --name redis-cli
