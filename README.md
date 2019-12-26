e3w
===

etcd v3 Web UI based on [Golang](https://golang.org/) && [React](https://facebook.github.io/react/), copy from [consul ui](https://github.com/hashicorp/consul/tree/master/ui) :)

supporting hierarchy on etcd v3, based on [e3ch](https://github.com/xiaowei520/e3ch)

## 快速开始

```
git clone https://github.com/xiaowei520/e3w.git
cd e3w
docker-compose up
# open http://localhost:8080
```

Or use docker image by `docker pull xiaowei520/e3w`, more details in `Dockerfile` and `docker-compose.yml`

## Overview

KEY/VALUE

![](./images/kv.png)

MEMBERS

![](./images/members.png)

ROLES

![](./images/roles.png)

USERS

![](./images/users.png)

SETTING

![](./images/setting.png)

## 使用方式

1.拉取项目 `go get github.com/xiaowei520/e3w`


2.前端构建命令

```
cd static
//安装cnpm加速node依赖安装速度
npm install -g cnpm --registry=https://registry.npm.taobao.org

npm install

npm run publish
```

3.后端构建

a. 启动etcd, 比如 [goreman](https://go.etcd.io/etcd/#running-a-local-etcd-cluster)

b. 安装go dep  [dep](https://github.com/golang/dep) 如果需要的话~ `dep ensure`

c. 编辑 conf/config.default.ini .配置你自己的服务参数, `go build && ./e3w`

d. 权限:

```
ETCDCTL_API=3 etcdctl auth enable
# edit conf/config.default.ini[app]#auth
./e3w
# you could set your username and password in SETTING page
```


## Notice

- When you want to add some permissions in directories to a user, the implement of hierarchy in [e3ch](https://github.com/xiaowei520/e3ch) requires you to set a directory's READ permission. For example, if role `roleA` has the permission of directory `dir1/dir2`, then it should have permissions:

	```
	KV Read:
		dir1/dir2
		[dir1/dir2/, dir1/dir20) (prefix dir1/dir2/)
	KV Write:
		[dir1/dir2/, dir1/dir20) (prefix dir1/dir2/)
	```

	When `userA` was granted `roleA`, `userA` could open the by `http://e3w-address.com/#/kv/dir1/dir2` to view and edit the key/value

- Access key/value by etcdctl, [issue](https://github.com/xiaowei520/e3w/issues/3). But the best way to access key/value is using [e3ch](https://github.com/xiaowei520/e3ch).
