# Manual Setup

Document này hướng dẫn setup Kubernetes mà không sử dụng K8s, tất cả mọi configuration đều được thực hiện thủ công. Đầu tiên tạo ra 5 server (sử dụng image `ubuntu:22.04`) gồm 2 control-plane node và 3 worker node:
```bash
docker run -it -d --privileged --name controller-1 ubuntu:22.04
docker run -it -d --privileged --name controller-2 ubuntu:22.04
docker run -it -d --privileged --name worker-1 ubuntu:22.04
docker run -it -d --privileged --name worker-2 ubuntu:22.04
docker run -it -d --privileged --name worker-3 ubuntu:22.04
```

<img width="1636" height="339" alt="Screenshot 2025-10-28 at 23 34 09" src="https://github.com/user-attachments/assets/3ebc7f2a-0739-4578-bea2-4dcd7961979f" />
