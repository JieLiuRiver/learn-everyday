
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


