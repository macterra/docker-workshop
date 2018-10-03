FROM ubuntu:bionic

RUN apt-get update -y
RUN apt-get install -y wget
RUN apt-get install -y vim
RUN apt-get install -y inetutils-ping

ENTRYPOINT ["ping", "-c", "3"]
CMD ["localhost"]
