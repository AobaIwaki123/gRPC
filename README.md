# gRPC

[TOC]

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

## Streaming RPCの挙動

```sh
[ローカルマシン上]$ docker compose exec -it go-grpc bash
[Docker Container内]$ go run cmd/server/main.go
# gRPC Client CLI
start gRPC Client.
1: send Request
2: HelloServerStream
3: HelloClientStream
4: HelloBiStream
5: exit
#### Unary RPC ####
please enter >1
Please enter your name.
hogehoge
Hello, hogehoge!
#### Server Streaming RPC ####
please enter >2
Please enter your name.
hogehoge
message:"[0] Hello, hogehoge!"
message:"[1] Hello, hogehoge!"
message:"[2] Hello, hogehoge!"
message:"[3] Hello, hogehoge!"
message:"[4] Hello, hogehoge!"
#### Client Streaming RPC ####
please enter >3
Please enter 5 names.
hogehoge
hugahuga
bokeboke
pokepoke
kerokero
Hello, [hogehoge hugahuga bokeboke pokepoke kerokero]
#### Bidirectional Streaming RPC ####
please enter >4
Please enter 5 names.
hoge
Hello, hoge
huga
Hello, huga
boke
Hello, boke
poke
Hello, poke
poka
Hello, poka
```

## ゴールーチンセーフなgRPCの実装

### Client

- 同一のコネクションを用いて異なるゴールーチンから異なるサービスを扱うのはゴールーチンセーフ

### Stream

- 異なるゴールーチンから同一のストリームに対して、`SendMsg`または`RecvMsg`を複数回呼び出すのはゴールーチンセーフではない
- 但し、ゴールーチンAから`SendMsg`を呼び出し、ゴールーチンBから`RecvMsg`を呼び出すというようなケースはゴールーチンセーフ

## 参考

- [作ってわかる！ はじめてのgRPC - Zenn](https://zenn.dev/hsaki/books/golang-grpc-starting)
- [Concurrency - grpc-go Documentation](https://github.com/grpc/grpc-go/blob/master/Documentation/concurrency.md)
  - コネクションやストリームをごゴールーチンセーフに書くための公式ドキュメント(必読)
- [Application Load Balancer が、gRPC ワークロードをエンドツーエンドの HTTP/2 サポート対象に - AWS Blog](https://aws.amazon.com/jp/about-aws/whats-new/2020/10/application-load-balancers-enable-grpc-workloads-end-to-end-http-2-support/)
