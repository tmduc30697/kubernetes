# Manual Setup

## Setup ETCDs

Hệ thống ETCD được triển khai trên hai container Ubuntu 22.04. File binary ETCD được copy sẵn vào từng container và cài đặt thủ công. Sau đó cấu hình các tham số kết nối để hai node có thể giao tiếp và thiết lập replication giữa chúng. Cuối cùng, sử dụng các lệnh như etcdctl member list, etcdctl put, và etcdctl get để kiểm tra hoạt động của cụm và xác nhận replication hoạt động chính xác.

Step 1: Tạo hai container Ubuntu 22.04, sau đó copy file binary ETCD đã chuẩn bị sẵn vào từng container để sẵn sàng cho quá trình cài đặt.
```shell
docker run -it -d --privileged --name etcd-1 ubuntu:22.04
docker run -it -d --privileged --name etcd-1 ubuntu:22.04
```

**Step 2**: Truy cập vào bash của từng container, thực hiện cài đặt ETCD từ file binary và khởi động dịch vụ, sau đó kiểm tra để đảm bảo cả hai server đã chạy thành công.

**Step 3**: Cấu hình replication giữa hai server bằng cách thiết lập các tham số peer URL và cluster, sau đó thực hiện kiểm tra bằng các lệnh `etcdctl put` và `etcdctl get` để xác nhận dữ liệu được đồng bộ thành công giữa các node.

## Setup Control-Panel Nodes
