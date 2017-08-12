---
title: 安装Devstack和Skydive遇到错误的解决办法
date: 2017-08-12 11:15:11
tags:
---
- 小组三个人装skydive装了一个星期。不完全总结一下遇到的坑。。

# devstack

- 代理问题

```bash
#proxy
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

git config --global --unset http.proxy
git config --global --unset https.proxy
```

- 权限问题

```
/opt/stack/devstack/lib/neutron-legacy: line 668: /opt/stack/neutron/tools/generate_config_file_samples.sh: Permission denied

#solve1
chown -R stack devstack
chmod 770 devstack
./clean.sh
./stack.sh

#solve2
sudo chmod -R 777 xxx
```

<!--more-->

- centos系统可能与Ubuntu出现的问题还不一样

```
##centos
generate subunit:command not found

#solve
sudo yum install python-pip
sudo pip install -U os-testr
```

---


# skydive

### make install

- docker的github仓库名称变了，导致下载失败。手动下载

```
stack@ubuntu:~/skydive$ make install
go get github.com/kardianos/govendor
/opt/stack/go/bin/bin/govendor sync
# cd .; git clone https://github.com/docker/docker /opt/stack/go/.cache/govendor/github.com/docker/docker

#solve
stack@ubuntu:~/go/.cache/govendor/github.com/docker$ git clone https://github.com/moby/moby /opt/stack/go/.cache/govendor/github.com/docker/docker

```

- 应该是网络问题

```bash
/opt/stack/go/bin/bin/govendor sync
# cd .; git clone https://go.googlesource.com/text /opt/stack/go/.cache/govendor/golang.org/x/text
Cloning into '/opt/stack/go/.cache/govendor/golang.org/x/text'...
fatal: unable to access 'https://go.googlesource.com/text/': gnutls_handshake() 

# slove
git clone https://go.googlesource.com/text /opt/stack/go/.cache/govendor/golang.org/x/text
```

- 安装skydive前要把依赖包安装好 

```
[helloc@helloc-ubuntu skydive]# make install
go get github.com/kardianos/govendor
/usr/local/go/bin/bin/govendor sync
go get github.com/golang/protobuf/proto
go get github.com/golang/protobuf/protoc-gen-go
go get github.com/jteeuwen/go-bindata/...
protoc --go_out . flow/flow.proto flow/set.proto flow/request.proto
protoc-gen-go: program not found or is not executable
--go_out: protoc-gen-go: Plugin failed with status code 1.
make: *** [.proto] Error 1

#solve
安装protoc
```

```
protoc:error while loading shared libraries...
#solve
sudo ldconfig
```


### Skydive start error

```
helloc@helloc-ubuntu:~$ skydive agent
2017-08-03T21:55:36.647-0400	INFO	cmd/agent.go:46 glob..func1	helloc-ubuntu:agent Skydive Agent v0.12.0 starting...
2017-08-03T21:55:36.710-0400	INFO	agent/probes.go:37 NewTopologyProbeBundleFromConfig	helloc-ubuntu:agent Topology probes: [ovsdb]
2017-08-03T21:55:36.710-0400	ERROR	agent/agent.go:111 (*Agent).Start	helloc-ubuntu:agent Unable to instantiate topology probes: NetNS probe has to be run as root
```
```
helloc@helloc-ubuntu: sudo skydive allinone
skydive: command not found

# slove
sudo $GOPATH/bin/skydive allinone
```


```
ERROR	http/wsclient.go:139 (*WSAsyncClient).connect	helloc-ubuntu:agent Unable to create a WebSocket connection ws://127.0.0.1:8082/ws : Authentication failed: dial tcp 127.0.0.1:8082: getsockopt: connection refused

#solve
$skydive analyzer
error: permission denied.
chmod -R 777  /var/lib

```

```bash
helloc@helloc-ubuntu:~$ sudo su - stack
>>> /etc/sudoers.d/50_stack_sh: syntax error near line 1 <<<
>>> /etc/sudoers.d/50_stack_sh: syntax error near line 2 <<<
>>> /etc/sudoers.d/50_stack_sh: syntax error near line 3 <<<
sudo: parse error in  near line 1
sudo: no valid sudoers sources found, quitting
sudo: unable to initialize policy plugin

# solve
>>> su
>>> pkexec visudo -f /etc/sudoers.d
#change into:
stack ALL=(ALL) NOPASSWD: ALL
Defaults secure_path=/sbin:/usr/sbin:/usr/bin:/bin:/usr/local/sbin:/usr/local/bin
Defaults !requiretty
##(ctrl+o) save!
```
