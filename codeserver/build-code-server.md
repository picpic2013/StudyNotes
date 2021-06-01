## 使用 docker 构建 vs code

~~~bash
docker pull aouos/code-server

docker commit --change="WORKDIR /root" -c 'CMD ["code-server"]' codeserver aouos/code-server

docker run -d -p 8080:8080 -p 25500:5500 --name codeserver aouos/code-server

docker exec -it codeserver bash

apt install vim

vim ~/.config/code-server/config.yaml
~~~

~~~
bind-addr: 0.0.0.0:8080
auth: password
password: 密码
cert: false
~~~

