# Quarkusの開発環境のためのVSCodeのDevContainer設定

```
├─ .devcontainer/
│  ├─　devcontainer.json
│  ├─docker-compose.yaml
│  ├─Dockerfile
```

# ネイティブイメージのビルド

https://code.quarkus.io/d?e=io.quarkus:quarkus-resteasy-reactive のサンプルアプリcode-with-quarkusの場合。

ローカル実行
```
./mvnw package -Pnative
./target/code-with-quarkus-1.0.0-SNAPSHOT-runner
```

Quarkus Builder Imageの利用
```
./mvnw package -Pnative -Dquarkus.native.container-build=true -Dquarkus.native.builder-image=quay.io/quarkus/ubi-quarkus-mandrel-builder-image:jdk-21
./target/code-with-quarkus-1.0.0-SNAPSHOT-runner
```



# 参考にしたコンテンツ
* [mandrel 23.1.2.0-Final, JDK21](https://github.com/graalvm/mandrel/releases/tag/mandrel-23.1.2.0-Final)
* [How to use Podman inside of a container](https://www.redhat.com/sysadmin/podman-inside-container)