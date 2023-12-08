# run this file with: docker image build -t rvgpu/sphinx:latest .

FROM sphinxdoc/sphinx
LABEL maintainer="qihangkong@outlook.com"
RUN pip3 install recommonmark==0.7.1 sphinx_markdown_tables==0.0.17 sphinx-rtd-theme==2.0.0

WORKDIR /docs
ENTRYPOINT ["make", "html"]
