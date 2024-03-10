nguyên tắc chạy docker

- docker <Manamgent command> <Action command> [OPTION]

-- docker container run --help

-- docker container run --publish 8080:80 --name server1 -- detach nginx:1.20.1
-- docker container run -rm <!-- xoá container sau khi nó dừng -->

-- docker container ls
-- docker container ls -a

<!-- --check logs -->

-- docker container logs <name>
-- docker container rm <id container>
-- docker container rm -f <id container> <!-- rm khi dang run -->

-- docker container top <id> <!-- vao trong container xem -->

-- docker container stats <id><!-- vao trong container xem cpu ram -->

-- docker container inspect <id> <!-- --check details cau hinh dang json-->

--ctrl + pq <!-- thoat ra khoi container ma khong exit -->
--exit <!-- thoat ra khoi container ma khong exit -->

//-- option -it

-- docker container run --name centos2 -it centos <!-- vao shell quan tri khi chưa có container -->

<!-- khi đã có  -->

-- docker container exec -it <id | name> <shell>
docker container exec -it 2312321 /bin/sh
docker container exec -it 2321321 ping 1.1.1.1

<!-- network -->

khi create container thì nên tạo network mới , network mặc định bridge không hỗ trợ trực tiêps khi container giao tiếp với nhau bằng name_network

kiểm tra thông tin network của container
-- docker container inspect <name>

kiểm tra network có bao nhiêu container connect đến
--docker network inspect <name>
docker network inspect bridge

tự tạo network
-- docker network create <name>
docker network create my_db

docker network create --help

connect container vào network , tạo container mới và kết nối nó
-- docker container run --name <name> -d --network <name_network> <image>:<version_image>
docker container run --name new_nginx -d --network my_db nginx:1.20.1

connect container đã chạy vào network
--docker network connect --help
--docker network connect <name_network> <name_container>
docker network connect my_db new_nginx

disconnect container trong network
-- docker network disconnect <name_network> <name_container>
docker network disconnect my_db new_nginx

giao tiếp giữa các container trong network
docker container run --name new_centos1 -it --network my_db centos <!-- khởi chạy container với option -it để vào trong bash shell của centos  , và gán nó vào network my_db -->

docker container run --name new_centos2 --network my_db centos <!-- tiếp tục khởi chạy container với option -it để vào trong bash shell của centos  , và gán nó vào network my_db -->

khi 2 container chung 1 network dùng ping

--ping <name_container>

<!-- đứng từ new_centos1 -->

ping new_centos2

<!-- đứng từ new_centos2 -->

ping new_centos1

--network-alias

<!-- ta có thể tạo network-alias cho container , mục đích nhiều container có thể dùng chung 1  network-alias name-->

docker container run --name new_nginx1 -d --network-alias webserver1 --network my_db nginx:1.20.1
docker container run --name new_nginx2 -d --network-alias webserver1 --network my_db nginx:1.20.1
docker container run --name new_nginx3 -d --network-alias webserver1 --network my_db nginx:1.20.1

<!-- chung 1 alias là webserver1 -->
<!-- áp dụng , khi vào centos ping đến  <name_container>  -->
<!-- load banace DNS -->

ping new_nginx3
ping new_nginx2
ping new_nginx1
và nếu ping đến network-alias thì nó sẽ trả ra ngẫu nhiên 1 container (mục đích cân bằng tải || load banance)
ping webserver1
