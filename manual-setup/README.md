# Manual Setup

## Setup ETCDs

Hệ thống ETCD được triển khai trên hai container Ubuntu 22.04. File binary ETCD được copy sẵn vào từng container và cài đặt thủ công. Sau đó cấu hình các tham số kết nối để hai node có thể giao tiếp và thiết lập replication giữa chúng. Cuối cùng, sử dụng các lệnh như etcdctl member list, etcdctl put, và etcdctl get để kiểm tra hoạt động của cụm và xác nhận replication hoạt động chính xác.

**Step 1**: Tạo hai container Ubuntu 22.04, sau đó copy file binary ETCD đã chuẩn bị sẵn vào từng container để sẵn sàng cho quá trình cài đặt.
```bash
# etcd 1
docker run -it -d -p 6000:2379 --privileged --name etcd-1 ubuntu:22.04
docker exec -it etcd-1 mkdir app/
docker cp ./etcd-v3.6.5-linux-amd64.tar.gz etcd-1:app/

# etcd 2
docker run -it -d -p 6001:2379 --privileged --name etcd-2 ubuntu:22.04
docker exec -it etcd-2 mkdir app/
docker cp ./etcd-v3.6.5-linux-amd64.tar.gz etcd-2:app/
```

**Step 2**: Truy cập vào bash của từng container, thực hiện cài đặt ETCD từ file binary và cấu hình các tham số cần thiết cho từng node.

```bash
# setup ETCD 1
docker exec -it etcd-1 bash
tar xzf /app/etcd-v3.6.5-linux-amd64.tar.gz -C /app
cp /app/etcd-v3.6.5-linux-amd64/etcd* /usr/local/bin

# setup ETCD 2
docker exec -it etcd-2 bash
tar xzf /app/etcd-v3.6.5-linux-amd64.tar.gz -C /app
cp /app/etcd-v3.6.5-linux-amd64/etcd* /usr/local/bin
```

**Step 3**: Khởi động dịch vụ ETCD trên cả hai container, sau đó kiểm tra trạng thái hoạt động để đảm bảo hai server hoạt động ổn định và replication hoạt động chính xác.

```bash
docker exec -d etcd-1 \
  etcd \
  --data-dir=/var/lib/etcd \
  --listen-client-urls=http://0.0.0.0:2379 \
  --advertise-client-urls=http://0.0.0.0:2379

docker exec -d etcd-2 \
  etcd \
  --data-dir=/var/lib/etcd \
  --listen-client-urls=http://0.0.0.0:2379 \
  --advertise-client-urls=http://0.0.0.0:2379
```

**Step 3**: Cấu hình replication giữa hai server bằng cách thiết lập các tham số peer URL và cluster, sau đó thực hiện kiểm tra bằng các lệnh `etcdctl put` và `etcdctl get` để xác nhận dữ liệu được đồng bộ thành công giữa các node.

## Setup Control-Panel Nodes

Hệ thống control-plane node được triển khai trên hai container Ubuntu 22.04. Các file binary của kube-apiserver, kube-controller-manager, và kube-scheduler được copy sẵn vào từng container để cài đặt và khởi chạy. Cuối cùng, thực hiện các lệnh như `kubectl get componentstatuses` hoặc `curl localhost:6443/healthz` để kiểm tra và xác nhận hai server hoạt động ổn định.

**Step 1**: Tạo hai container Ubuntu 22.04 và copy các file binary của kube-apiserver, kube-controller-manager, và kube-scheduler vào từng container.

**Step 2**: Truy cập vào bash của từng container, thực hiện cài đặt và khởi động các service tương ứng.

**Step 3:** Kiểm tra trạng thái hoạt động của các service bằng các lệnh như `kubectl get componentstatuses` hoặc `curl localhost:6443/healthz` để xác nhận chúng đã chạy ổn định.
