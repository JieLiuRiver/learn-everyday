
### SWC
[SWC](https://swc.rs/)（Super-fast Web Compiler）是一个用于将 JavaScript 代码转换为优化的低级别代码的工具，其主要优点和评价可以包括以下几点：

- 极速编译：SWC 被设计成具有卓越的性能，以实现快速的 JavaScript 编译。其使用 Rust 语言编写，借助 Rust 的优秀性能和并行处理能力，SWC 可以在编译大型 JavaScript 项目时表现出很高的速度，有助于提高开发者的工作效率。

- 全面的语法支持：SWC 支持最新的 ECMAScript（ES）标准，并且对许多实验性的 JavaScript 提案也提供了支持。这使得开发者可以使用最新的 JavaScript 语法和功能，而无需担心兼容性问题。

- 强大的优化能力：SWC 提供了多种优化技术，包括代码压缩、去除无用代码、变量重命名、常量折叠等，这些优化操作有助于生成高效的低级别代码，从而提升 JavaScript 代码的性能。

- 灵活的配置选项：SWC 提供了丰富的配置选项，使得开发者可以根据项目的需求进行定制化配置。这包括对转换规则、优化选项、源映射生成等的灵活控制，从而满足不同项目的编译需求。

- 社区活跃：SWC 是一个开源项目，拥有活跃的社区和开发者社群。这意味着开发者可以在社区中获得支持、反馈问题并参与贡献，从而不断改进和完善 SWC。

### SWC 代替工具：

- Babel：Babel 是目前广泛使用的 JavaScript 编译工具，用于将较新版本的 JavaScript 代码转换为在旧版浏览器或环境中运行的兼容版本。SWC 也提供了类似的功能，包括对最新的 ECMAScript 标准的支持，因此可以作为 Babel 的替代品。

- TypeScript 编译器：TypeScript 是一种由 Microsoft 开发的 JavaScript 超集语言，它可以编译为纯 JavaScript 代码。SWC 可以作为 TypeScript 编译器的替代工具，用于将 TypeScript 代码转换为优化的低级别 JavaScript 代码。

- UglifyJS、Terser 等代码压缩工具：SWC 提供了代码压缩的功能，包括删除无用代码、变量重命名、常量折叠等优化操作，因此可以作为 UglifyJS、Terser 等代码压缩工具的替代品。

- 其他编译工具：SWC 还提供了丰富的配置选项，包括对转换规则、优化选项、源映射生成等的灵活控制，因此在某些情况下，可以作为其他 JavaScript 编译工具的替代品，例如 webpack、Rollup 等。

### build

`swc src -d dist --source-maps --copy-files` 是一个命令行指令，用于使用 SWC 工具将 src 目录中的源代码转换为 JavaScript 代码，并将转换后的代码输出到 dist 目录中，同时生成源映射文件，并拷贝其他文件。

--copy-files 参数表示在转换过程中，将除了 JavaScript 源码外的其他文件从源目录 (src) 拷贝到输出目录 (dist)


### dev & start
```json
"scripts": {
    "start": "npm run build && cross-env NODE_ENV=production node dist/server.js",
    "dev": "cross-env NODE_ENV=development nodemon",
}
```
`start` 用于部署上线的启动用
`dev` 开发阶段用


### nodemon.json
```json
{
    "watch": [
        "src",
        ".env"
    ],
    "ext": "js,ts,json",
    "ignore": [
        "src/logs/*",
        "src/**/*.{spec,test}.ts"
    ],
    "exec": "ts-node -r tsconfig-paths/register --transpile-only src/server.ts"
}
```

"exec": "ts-node -r tsconfig-paths/register --transpile-only src/server.ts"：定义运行应用程序的命令。在这个配置中，nodemon 会使用 ts-node 工具运行 src/server.ts 文件，并通过 -r 参数引入 tsconfig-paths/register 模块，用于支持 TypeScript 中的路径别名（在 tsconfig.json 中配置的路径别名）。--transpile-only 参数用于只进行 TypeScript 的编译而不进行类型检查，以提高应用程序的启动速度。

如果在 nodemon.json 文件中不定义 exec 字段，nodemon 工具会默认使用以下命令来运行应用程序：

```js
node <your entry point file>
```

其中 <your entry point file> 是指应用程序的入口文件，通常是一个 JavaScript 或 TypeScript 文件，例如 src/server.js 或 src/server.ts。这个命令会直接通过 Node.js 运行入口文件，并在文件发生变化时自动重启应用程序。

但是，如果你的应用程序需要使用 TypeScript 进行开发，并且希望在 nodemon 工具中支持 TypeScript 的路径别名等特性，那么建议在 exec 字段中定义对应的命令，例如 "exec": "ts-node -r tsconfig-paths/register --transpile-only src/server.ts"。这样可以确保 nodemon 在运行应用程序时能够正确处理 TypeScript 文件中的特性。

### tsconfig.json

```json
{
  "compileOnSave": false,
  "compilerOptions": {
    "target": "es2017",
    "lib": ["es2017", "esnext.asynciterable"],
    "typeRoots": ["node_modules/@types"],
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "module": "commonjs",
    "pretty": true,
    "sourceMap": true,
    "declaration": true,
    "outDir": "dist",
    "allowJs": true,
    "noEmit": false,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "importHelpers": true,
    "baseUrl": "src",
    "paths": {
      "@/*": ["*"],
      "@config": ["config"],
      "@controllers/*": ["controllers/*"],
      "@dtos/*": ["dtos/*"],
      "@exceptions/*": ["exceptions/*"],
      "@interfaces/*": ["interfaces/*"],
      "@middlewares/*": ["middlewares/*"],
      "@models/*": ["models/*"],
      "@routes/*": ["routes/*"],
      "@services/*": ["services/*"],
      "@utils/*": ["utils/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.json", ".env"],
  "exclude": ["node_modules", "src/http", "src/logs"]
}
```

`"include": ["src/**/*.ts", "src/**/*.json", ".env"]`

TypeScript 编译器对 .json 和 .env 文件不会做特殊处理，它只会将这些文件一同复制到编译输出目录（如 "outDir": "dist"）中。

对于 .json 文件，TypeScript 编译器会直接复制到编译输出目录中，不会对其内容进行任何处理。在运行时，你可以使用 Node.js 或其他方法加载这些 JSON 文件，并按照需要解析其内容。

对于 .env 文件，TypeScript 编译器同样会将其复制到编译输出目录中。然而，.env 文件通常是用于存储环境变量配置的文件，而不是用于直接加载和解析。在运行时，你需要使用相应的库或方法，如 dotenv 等，来加载和解析 .env 文件中的环境变量，并将其设置到应用程序的运行环境中。


### env

`config/index.ts`
```ts
import { config } from 'dotenv';
config({ path: `.env.${process.env.NODE_ENV || 'development'}.local` });

export const CREDENTIALS = process.env.CREDENTIALS === 'true';
export const { NODE_ENV, PORT, SECRET_KEY, LOG_FORMAT, LOG_DIR, ORIGIN } = process.env;

```

`.env.development.local`
```
# PORT
PORT = 3000

# TOKEN
SECRET_KEY = secretKey

# LOG
LOG_FORMAT = dev
LOG_DIR = ../logs

# CORS
ORIGIN = *
CREDENTIALS = true
```

`.env.production.local`
    ...
`.env.test.local`
    ...


### ts-alias
一个用于在 TypeScript 编译完成后将别名路径替换为相对路径的工具。它可以用于解决 TypeScript 项目中使用别名路径时在编译后可能导致路径错误的问题。

```json
"build:tsc": "tsc && tsc-alias",
```

使用了 tsc-alias 插件来在 TypeScript 编译完成后执行别名路径的替换。-p 参数后面可以指定 tsconfig.json 文件的路径，如果不指定，默认会在项目根目录下寻找 tsconfig.json 文件。


### pm2
```json
{
    "scripts": {
        "deploy:prod": "npm run build && pm2 start ecosystem.config.js --only prod",
        "deploy:dev": "pm2 start ecosystem.config.js --only dev"
    }
}
```

`ecosystem.config.js`
```
/**
 * @description pm2 configuration file.
 * @example
 *  production mode :: pm2 start ecosystem.config.js --only prod
 *  development mode :: pm2 start ecosystem.config.js --only dev
 */
 module.exports = {
  apps: [
    {
      name: 'prod', // pm2 start App name
      script: 'dist/server.js',
      exec_mode: 'cluster', // 'cluster' or 'fork'
      instance_var: 'INSTANCE_ID', // instance variable
      instances: 2, // pm2 instance count
      autorestart: true, // auto restart if process crash
      watch: false, // files change automatic restart
      ignore_watch: ['node_modules', 'logs'], // ignore files change
      max_memory_restart: '1G', // restart if process use more than 1G memory
      merge_logs: true, // if true, stdout and stderr will be merged and sent to pm2 log
      output: './logs/access.log', // pm2 log file
      error: './logs/error.log', // pm2 error log file
      env: { // environment variable
        PORT: 3000,
        NODE_ENV: 'production',
      },
    },
    {
      name: 'dev', // pm2 start App name
      script: 'ts-node', // ts-node
      args: '-r tsconfig-paths/register --transpile-only src/server.ts', // ts-node args
      exec_mode: 'cluster', // 'cluster' or 'fork'
      instance_var: 'INSTANCE_ID', // instance variable
      instances: 2, // pm2 instance count
      autorestart: true, // auto restart if process crash
      watch: false, // files change automatic restart
      ignore_watch: ['node_modules', 'logs'], // ignore files change
      max_memory_restart: '1G', // restart if process use more than 1G memory
      merge_logs: true, // if true, stdout and stderr will be merged and sent to pm2 log
      output: './logs/access.log', // pm2 log file
      error: './logs/error.log', // pm2 error log file
      env: { // environment variable
        PORT: 3000,
        NODE_ENV: 'development',
      },
    },
  ],
  deploy: {
    production: {
      user: 'user',
      host: '0.0.0.0',
      ref: 'origin/master',
      repo: 'git@github.com:repo.git',
      path: 'dist/server.js',
      'post-deploy': 'npm install && npm run build && pm2 reload ecosystem.config.js --only prod',
    },
  },
};

```

这段代码是一个用于配置 pm2 的配置文件 ecosystem.config.js，用于在生产和开发环境下部署和管理 Node.js 应用。其中 package.json 中定义了两个脚本命令 deploy:prod 和 deploy:dev，分别用于在生产环境和开发环境下启动应用。

ecosystem.config.js 中的配置项包括：

- apps：一个数组，包含了两个应用配置对象，分别对应生产环境和开发环境的应用。每个应用配置对象包含了以下属性：

  - name：应用的名称，用于在 pm2 中唯一标识应用。
  - script：应用的入口文件路径。
  - exec_mode：应用的执行模式，可以是 cluster（多进程模式）或者 fork（单进程模式）。
  - instance_var：用于标识实例的环境变量名称。
  - instances：实例的数量，对于多进程模式有效。
  - autorestart：是否自动重启应用，如果应用进程崩溃。
  - watch：是否监听文件变化并自动重启应用。
  - ignore_watch：需要忽略的文件变化。
  - max_memory_restart：当应用占用内存超过指定大小时自动重启应用。
  - merge_logs：是否合并标准输出和标准错误输出并发送到 pm2 日志。
  - output：标准输出日志文件路径。
  - error：标准错误输出日志文件路径。
  - env：应用的环境变量，可以在应用代码中通过 process.env 访问。
- deploy：一个对象，包含了生产环境部署相关的配置。其中包括了部署用户、主机、仓库地址、部署路径等信息。在部署完成后，会执行 post-deploy 脚本命令，用于安装依赖、构建应用并重新加载 pm2。具体的部署命令可以在 package.json 中的 deploy:prod 脚本中找到。

这段配置文件的作用是通过 pm2 来管理 Node.js 应用的启动、重启、日志等操作，并提供了生产环境下的部署配置，以便快速部署和管理生产环境中的 Node.js 应用。


### docker
`Dockerfile.prod`
```
# NodeJS Version 16
FROM node:16.18-buster-slim

# Copy Dir
COPY . ./app

# Work to Dir
WORKDIR /app

# Install Node Package
RUN npm install --legacy-peer-deps

# Set Env
ENV NODE_ENV production
EXPOSE 3000

# Cmd script
CMD ["npm", "run", "start"]

```

`.dockerignore`
```
# compiled output
.vscode
/node_modules

# code formatter
.eslintrc
.eslintignore
.editorconfig
.huskyrc
.lintstagedrc.json
.prettierrc

# test
jest.config.js

# docker
Dockerfile
docker-compose.yml
```

这是一个用于构建生产环境下 Docker 镜像的 Dockerfile 文件。它基于 Node.js 16 版本的官方镜像，并在镜像中执行以下操作：

使用 COPY 命令将当前目录下的所有文件复制到容器内的 /app 目录下，将应用的代码和文件复制到容器中。

使用 WORKDIR 命令将工作目录切换到 /app 目录，以便后续的命令在该目录下执行。

使用 RUN 命令运行 npm install 命令，安装应用的依赖包。其中使用了 --legacy-peer-deps 参数，用于处理旧版本 npm 在处理 Peer Dependencies 时可能出现的错误。

使用 ENV 命令设置 NODE_ENV 环境变量为 production，表示当前构建的是生产环境的镜像。

使用 EXPOSE 命令声明容器内的端口号为 3000，用于指定容器对外开放的端口号。

使用 CMD 命令指定容器启动时的默认命令，这里是运行 npm run start 命令，用于启动 Node.js 应用。

通过这个 Dockerfile 文件，可以构建一个基于 Node.js 16 的生产环境 Docker 镜像，该镜像包含了应用的代码、依赖包和环境配置，并且已经设置好了容器启动时的默认命令。可以使用 Docker 工具来构建和运行这个镜像，从而实现应用在生产环境中的容器化部署。


`docker-compose.yml`
```yml
version: "3.9"

services:
  proxy:
    container_name: proxy
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    restart: "unless-stopped"
    networks:
      - backend

  server:
    container_name: server
    build:
      context: ./
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - ./:/app
      - /app/node_modules
    restart: 'unless-stopped'
    networks:
      - backend

networks:
  backend:
    driver: bridge

volumes:
  data:
    driver: local

```

这是一个使用 Docker Compose 编排的服务配置文件 docker-compose.yml，用于定义一个包含 Nginx 反向代理和 Node.js 服务器的 Docker 服务栈。文件中的配置如下：

定义了 Docker Compose 文件的版本为 3.9，表示使用 Docker Compose 的 3.9 版本的语法。

定义了两个服务：proxy 和 server。

proxy 服务使用了 nginx:alpine 镜像，作为 Nginx 反向代理服务器。它设置了容器名称为 proxy，将容器的 80 端口映射到主机的 80 端口，指定了 ./nginx.conf 文件作为 Nginx 配置文件的挂载卷，并将容器连接到名为 backend 的网络中。

server 服务使用了 Dockerfile.dev 文件作为构建上下文来构建镜像。它设置了容器名称为 server，将容器的 3000 端口映射到主机的 3000 端口，指定了当前目录作为 /app 目录的挂载卷，并将容器连接到名为 backend 的网络中。此外，还挂载了 /app/node_modules 目录，以便保留宿主机上已安装的 Node.js 依赖包，从而避免在容器中重复安装。

定义了名为 backend 的网络，使用 bridge 驱动。

定义了名为 data 的卷，使用 local 驱动。

通过这个 docker-compose.yml 文件，可以使用 Docker Compose 工具一键启动整个服务栈，包括 Nginx 反向代理和 Node.js 服务器，从而实现一个完整的开发环境或测试环境。同时，通过挂载卷和网络的配置，可以实现容器和宿主机之间的文件共享和网络通信。


### nginx

`nginx.conf`
```
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    upstream api-server {
        server server:3000;
        keepalive 100;
    }

    server {
        listen 80;
        server_name localhost;

        location / {
		proxy_http_version 1.1;
            	proxy_pass         http://api-server;
        }

    }

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    keepalive_timeout  65;
    include /etc/nginx/conf.d/*.conf;
}
```

- 设置 Nginx 运行的用户为 nginx。

- 设置 Nginx 运行的 worker 进程数为 1。

- 配置 Nginx 错误日志的位置为 /var/log/nginx/error.log，日志级别为 warn。

- 配置 Nginx 进程 ID 文件的位置为 /var/run/nginx.pid。

- 定义了一个名为 events 的配置块，用于配置 Nginx 的事件处理，包括最大连接数为 1024。

- 定义了一个名为 http 的配置块，用于配置 HTTP 请求的处理。

- 包含了 /etc/nginx/mime.types 文件，用于配置 MIME 类型。

- 设置默认 MIME 类型为 application/octet-stream。

- 定义了一个名为 api-server 的 upstream，指向 server:3000，即 Node.js 服务器的地址。

- 定义了一个名为 server 的虚拟主机，监听 80 端口，并将所有请求代理到 api-server 上。

- 配置了日志格式为 main，包含了常见的日志字段。

- 配置了访问日志的位置为 /var/log/nginx/access.log。

- 开启了文件传输和长连接支持。

- 包含了 /etc/nginx/conf.d/*.conf 目录下的所有配置文件。

通过这个 nginx.conf 文件，可以配置 Nginx 反向代理服务器的行为，将请求转发到后端的 Node.js 服务器，并记录访问日志等操作。这个配置文件可以根据实际需求进行调整和扩展。