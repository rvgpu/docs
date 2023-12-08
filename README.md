# Build
Build docker image use:
```
docker image build -t rvgpu/sphinx:latest .
```

Build docs in docker
```
docker run --rm -v .:/docs -u $(id -u):$(id -g) rvgpu/sphinx
```
