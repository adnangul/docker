FROM python:2.7-slim

RUN apk update && apk add iputils
RUN apt-get update && apt-get install -y net-tools iputils-ping

RUN mkdir /app
WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

LABEL maintainer="Nick Janetakis <nick.janetakis@gmail.com>" \
      version="1.0"

CMD flask run --host=0.0.0.0 --port=5000
