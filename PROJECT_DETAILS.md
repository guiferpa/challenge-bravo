# What is a command?

Well, the command's a component commonly looks in Golang apps directory structures, take a look in few Golang projects that use this approach.

- [Kubernetes](https://github.com/kubernetes/kubernetes/tree/a054010d032b301e495d1a421f53b9a37a0a0109/cmd)
- [Tsuru](https://github.com/tsuru/tsuru/tree/86132787ea4fa5cb2e6ce8ea99520441fd4df569/cmd)
- [Docker](https://github.com/docker/docker-ce/tree/ab9188d5fd82bf7fcacf4cb5b625d15f50edf939/components/engine/cmd)

The commands can be founded at `./cmd` folder by default. For this project, we just have two commands called **api** and **doc** which is an entry point for compilation of project. It's a good practice you have a folder with entry point files of your Golang application for easier the compilation step.

We could compile differents binaries with this command directory structure as the Kubernetes project do. All directories below which are a different binary could find in `./cmd` at Tsuru source code.

```bash
...
├── tsurud
│   ├── api.go
│   ├── api_test.go
│   ├── checker.go
│   ├── checker_test.go
│   ├── command.go
│   ├── command_test.go
│   ├── gandalf.go
│   ├── main.go
│   ├── main_test.go
│   ├── migrate.go
│   ├── migrate_test.go
│   ├── signals.go
│   ├── signals_notsupported.go
│   ├── suite_test.go
│   ├── testdata
│   │   ├── tsuru2.conf
│   │   └── tsuru.conf
│   ├── token.go
│   └── token_test.go
├── utils.go
└── utils_test.go
```


# Design of source code from project

This project uses basically [ports and adapters architecture](http://www.dossier-andreas.net/software_architecture/ports_and_adapters.html) for code structure.

Ports and adapters are a design based on hexagonal architecture where you basically have **port** which is an interface with a contract defined by your business model and **adapter** which is an **port** implementation. With this architecture the application have more flexibility for plugin and unplugin any adapter.

Let's think in a scene where there'is a problem and the engineers are thinking about some PoC for differents databases. This scene at this architecture is simple because of the flexibility that it come in. I am free to plug a adapter for MySQL, MongoDB, PgSQL and etc. And it's possible because of the **ports** and **adapters**.

# Structure of directories

```bash

├── adapter
│   ├── primary
│   │   └── http
│   │       ├── rest
│   │       │   ├── currency.go
│   │       │   └── router.go
│   │       └── static
│   │           ├── router.go
│   │           └── statik.go
│   └── secondary
│       └── http
│           ├── coinbase
│           │   ├── coinbase.go
│           │   └── repository.go
│           ├── exchangerates
│           │   ├── exchangerates.go
│           │   └── repository.go
│           └── mock
│               └── repository.go
|
├── cmd
│   ├── api
│   │   └── main.go
│   └── doc
│       └── main.go
|
├── pkg
│   ├── config
│   │   └── config.go
│   ├── currency
│   │   ├── currency.go
│   │   ├── port.go
│   │   ├── quota.go
│   │   ├── service.go
│   │   └── service_test.go
│   └── httputil
│       └── write.go
|
└── platform
    └── docker
        ├── api
        │   └── Dockerfile
        └── doc
            └── Dockerfile
```

- **platform** - All recipes and files about platform of application is there, for example Terraform files, Docker files and etc.

- **pkg** - All core application source code is there, for example business rules, domains and extendable helpers.
- **cmd** - All entrypoint to compile the code is there.
- **adapter/primary** - All adapeters for primary ports
- **adapter/secondary** - All adapeters for secondary ports
