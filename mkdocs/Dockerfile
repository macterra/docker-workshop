FROM python:3.4-alpine

EXPOSE 8000

RUN pip install --upgrade pip
RUN pip install mkdocs

WORKDIR /docroot
RUN mkdocs new .

CMD ["mkdocs", "serve", "-a", "0.0.0.0:8000"]
