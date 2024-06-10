# gRPC

## Getting Started: gRPCサーバーの起動と起動確認方法

```sh
[ローカルマシン上]$ docker compose up -d
[ローカルマシン上]$ docker compose exec -it go-grpc bash
[Docker Container内]$ go run cmd/server/main.go
[ローカルマシン上] $ grpcurl -plaintext localhost:8080 list
# grpc.reflection.v1.ServerReflection
# grpc.reflection.v1alpha.ServerReflection
# myapp.GreetingService
```

## 参考

- [作ってわかる！ はじめてのgRPC - Zenn](https://zenn.dev/hsaki/books/golang-grpc-starting)
