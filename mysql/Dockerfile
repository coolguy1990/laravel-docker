FROM mysql:5.7

MAINTAINER Kunal Mangaraj <mangaraj.kunal@gmail.com>

RUN chown -R mysql:root /var/lib/mysql

ADD my.cnf /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306


# docker run -v ${cwd}/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 --name mysql mysql
# docker build --no-cache -t mysql .
