<div class="title" align=center>
    <img src="https://img.shields.io/github/license/Artrajz/pytorch-docker">
<a href="https://hub.docker.com/r/artrajz/pytorch"><img src="https://img.shields.io/docker/pulls/artrajz/pytorch"></a>
</div>



构建一个用于推理的torch环境，作为基础镜像在Dockerfile中使用，

CPU 支持amd64和arm64

```
FROM artrajz/pytorch:1.13.1-cpu-py3.10.11-ubuntu22.04
```

GPU 仅支持amd64

```
FROM artrajz/pytorch:1.13.1-cu117-py3.10.11-ubuntu22.04
```

