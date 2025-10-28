# Manual Setup

Document này hướng dẫn setup Kubernetes mà không sử dụng K8s, tất cả mọi configuration đều được thực hiện thủ công. Đầu tiên tạo ra 5 server (sử dụng image `ubuntu:22.04`) gồm 2 controller và 3 worker:
```bash
docker run -it -d --privileged --name controller-1 ubuntu:22.04
docker run -it -d --privileged --name controller-2 ubuntu:22.04
docker run -it -d --privileged --name worker-1 ubuntu:22.04
docker run -it -d --privileged --name worker-2 ubuntu:22.04
docker run -it -d --privileged --name worker-3 ubuntu:22.04
```
