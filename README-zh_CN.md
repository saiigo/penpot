<h1 align="center">
  <br>
  <img src="https://penpot.app/images/readme/readme-logo.jpg" alt="PENPOT">
</h1>

# 开源设计和原型平台Penpot 

## 项目启动

项目启动需要`Docker`，已经有装就不赘述。

[Penpot项目](https://github.com/penpot/penpot)下载下来之后，启动项目

```bash
docker compose -p penpot -f docker/images/docker-compose.yaml up -d
```

`docker`建了个分组`penpot`下面包含5个容器，通过 [http://localhost:9001](http://localhost:9001) 访问。前端项目是编译后的包，不能实时更新修改。所以需要开发模式启动项目：	

```bash
# 拉取docker镜像，所以不用每次执行
./manage.sh pull-devenv
# 运行
./manage.sh run-devenv
```


## Clojure源替换

`Docker`直接部署项目没有问题，但是开发模式启动，后台项目一直报错，只能对`Clojure`下载依赖更换国内源。

**tmux**：启动使用了`tmux`的终端复用工具，`Mac`使用`Control` + `b`  `w` 查看窗口进行切换。

后端项目修改`backend/build.clj`:

```bash
(ns build
;; 增加镜像源
  (:mirrors {"clojars" {:name "ustc"
                        :url "https://mirrors.ustc.edu.cn/clojars/"}})
  (:refer-clojure :exclude [compile])
  ...
```

直接修改deps.edn也可以，比如前端项目：

```bash
{:paths ["src" "vendor" "resources" "test"]
;; 修改镜像
 :mvn/repos
 {"central" {:url "https://repo1.maven.org/maven2/"}
  "clojars" {:url "https://mirrors.ustc.edu.cn/clojars/"}}
 :deps
 ...
```


## 参考

- [Penpot](https://penpot.app/)
- [Penpot Github](https://github.com/penpot/penpot)
- [Penpot Docs](https://help.penpot.app/technical-guide/)
- [Can I use Clojar mirrors in Clojure CLI?](https://clojureverse.org/t/can-i-use-clojar-mirrors-in-clojure-cli/5463)
- [Clojars 源使用帮助(leiningen)](https://mirrors.ustc.edu.cn/help/clojars.html)
- [Tmux](https://tmuxcheatsheet.com/)

