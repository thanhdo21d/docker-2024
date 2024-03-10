nguyên tắc chạy docker

- docker <Manamgent command> <Action command> [OPTION]

-- docker container run --help

-- docker container run --publish 8080:80 --name server1 -- detach nginx:1.20.1

-- docker container ls
-- docker container ls -a

<!-- --check logs -->

-- docker container logs <name>
-- docker container rm <id container>
-- docker container rm -f <id container> <!-- rm khi dang run -->

-- docker container top <id> <!-- vao trong container xem -->

-- docker container stats <id><!-- vao trong container xem cpu ram -->

-- docker container inspect <id> <!-- --check details -->

--ctrl + pq <!-- thoat ra khoi container ma khong exit -->
--exit <!-- thoat ra khoi container ma khong exit -->

//-- option -it

-- docker container run --name centos2 -it centos <!-- vao shell quan tri khi chưa có container -->

<!-- khi đã có  -->

-- docker container exec -it <id | name> <shell>
docker container exec -it 2312321 /bin/sh
docker container exec -it 2321321 ping 1.1.1.1
