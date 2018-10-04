FROM python:3.4-alpine

EXPOSE 8000

RUN pip install --upgrade pip
RUN pip install mkdocs
RUN pip install mdx_gh_links

WORKDIR /docroot
RUN mkdocs new .

CMD ["mkdocs", "serve", "-a", "0.0.0.0:8000"]
