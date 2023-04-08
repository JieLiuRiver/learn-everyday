## Vite-plugin-pages

Nuxt.js 3 的 Pages Directory 功能是通过使用 Vite 插件 `vite-plugin-pages` 实现的。这个插件会自动扫描项目中的 `pages` 目录，根据目录结构和文件命名规则生成路由配置，并将路由配置注入到 Nuxt.js 3 应用程序中。

如果您想查看源码实现细节，可以先了解一下 Vite 的插件机制。Vite 插件通常会在构建时对代码进行转换或注入特定的功能，因此插件本身需要编写代码来实现自己的功能。

要查看 `vite-plugin-pages` 的源代码，可以前往插件的 GitHub 仓库：https://github.com/hannoeru/vite-plugin-pages。

在仓库中，您可以查看插件的核心代码，包括插件的入口文件、路由生成器、文件扫描器等。您还可以查看插件的依赖关系和相关文档，以更好地理解插件的实现原理。



```js
import Pages from 'vite-plugin-pages'

export default {
  plugins: [
    // ...
    Pages(),
  ],
}
```



## .npmrc

`.npmrc` 是 NPM 的配置文件，其中包含了 NPM 包管理器的配置选项。在该文件中设置 `shell-emulator=true`，将启用 NPM 的 Shell 模拟器功能。

Shell 模拟器是一个可以模拟 Linux Shell 环境的工具，它可以让您在 Windows 环境中使用 Linux Shell 命令。当您在 Windows 命令提示符或 PowerShell 中运行 NPM 命令时，启用 Shell 模拟器可以让 NPM 命令在 Windows 中以 Linux Shell 的方式执行，从而避免在 Windows 中遇到的一些问题。

启用 Shell 模拟器的方法是在 `.npmrc` 文件中添加 `shell-emulator=true` 选项，保存文件后重新运行 NPM 命令即可。需要注意的是，启用 Shell 模拟器会增加一些额外的开销，因此在性能要求较高的场景下，可能不适合启用 Shell 模拟器。



## workspace:*

当我们在一个 NPM 工作区中管理多个相关的包时，可以使用 `workspace:*` 语法将某个依赖项声明为一个工作区依赖项。这样，在工作区中的任何包中都可以使用同一个实例的该依赖项，而无需在每个包中分别安装和维护该依赖项的版本。

下面是一个示例，假设我们有一个名为 `my-workspace` 的 NPM 工作区，其中包含了两个包 `my-app` 和 `my-library`，并且它们都依赖了 `lodash` 这个 NPM 包。我们可以通过以下步骤来将 `lodash` 声明为一个工作区依赖项：

1. 在 `my-workspace` 根目录下创建一个 `package.json` 文件，并在其中定义工作区的依赖关系：

```json
{
  "name": "my-workspace",
  "version": "1.0.0",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "dependencies": {
    "lodash": "^4.17.21"
  }
}
```

在上述示例中，我们使用了 `workspaces` 字段指定了工作区中的包所在的目录，这里是 `packages` 目录。

1. 在 `my-app` 和 `my-library` 的 `package.json` 文件中声明 `lodash` 作为一个工作区依赖项：

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "lodash": "workspace:*"
  }
}
jsonCopy code
{
  "name": "my-library",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "lodash": "workspace:*"
  }
}
```

在上述示例中，我们使用了 `workspace:*` 语法声明了 `lodash` 作为一个工作区依赖项，这样 `my-app` 和 `my-library` 中都可以共享同一个实例的 `lodash`。

通过这样的设置，我们可以在工作区中共享 `lodash` 的实例，避免重复安装和维护多个版本的问题。同时，在项目根目录运行 `npm install` 命令时，NPM 会自动安装工作区中所有包的依赖项，包括共享的工作区依赖项 `lodash`



## tsup

`tsup` 是一个基于 `Rollup` 的构建工具，用于构建 TypeScript 应用和库。使用 `tsup` 可以方便地构建出 `CommonJS`、`ES module` 和 `UMD` 格式的代码，并生成相应的类型声明文件。

在一个 TypeScript 项目中，我们可以通过以下步骤来使用 `tsup`：

1. 安装 `tsup`：

```shell
npm install tsup --save-dev
```

1. 在 `package.json` 中添加 `build` 脚本：

```json
"scripts": {
  "build": "tsup"
}
```

1. 在项目根目录下创建 `tsup.config.ts` 文件，并在其中配置 `tsup`：

```ts
import { defineConfig } from 'tsup'

export default defineConfig({
  entry: ['src/index.ts'],
  format: ['cjs', 'esm'],
  dts: {
    resolve: true,
  },
  clean: true,
  sourcemap: true,
})
```

在上述示例中，我们使用 `defineConfig` 函数定义了 `tsup` 的配置项，包括：

- `entry`：指定入口文件路径；
- `format`：指定输出格式，包括 `cjs`、`esm` 和 `umd`；
- `dts`：指定是否生成类型声明文件，以及是否解析依赖项的类型声明文件；
- `clean`：指定是否清除输出目录；
- `sourcemap`：指定是否生成源代码映射文件。

1. 运行 `npm run build` 命令进行构建。

在构建完成后，`tsup` 会在项目根目录下生成一个 `dist` 目录，其中包含构建生成的代码和类型声明文件。我们可以将 `dist` 目录中的文件作为库的输出文件，或将其打包到一个应用中使用。





## "types": "dist/index.d.ts"

在 TypeScript 项目中，我们通常会为代码生成类型声明文件，以便在使用该代码时获得更好的类型支持。在使用 `tsup` 构建 TypeScript 库时，我们需要在 `package.json` 中添加 `types` 字段，指定类型声明文件的路径。

例如，如果我们的库代码被构建到了 `dist` 目录下，类型声明文件名为 `index.d.ts`，那么我们可以在 `package.json` 中添加如下配置：

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts"
}
```

在上述示例中，我们将类型声明文件的路径设置为 `dist/index.d.ts`，这样在其他 TypeScript 项目中使用该库时，就可以获得类型提示了。



## Exports Rule

`package.json`

```json
 "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "require": "./dist/index.js",
      "import": "./dist/index.mjs"
    },
    "./client": {
      "types": "./client.d.ts"
    },
    "./client-react": {
      "types": "./client-react.d.ts"
    },
    "./client-solid": {
      "types": "./client-solid.d.ts"
    }
 }
```

这样，当其他项目导入这个模块时，就可以根据这些规则进行导入了。如果使用 TypeScript 编写的项目中导入这个模块的默认导出项时，就会使用 `./dist/index.d.ts` 中指定的类型声明文件。如果使用 CommonJS 规范导入这个模块时，就会使用 `./dist/index.js` 中的代码实现。如果使用 ES Modules 规范导入这个模块时，就会使用 `./dist/index.mjs` 中的代码实现。



## 自动化版本号的管理

`package.json`

```json
"scripts": {
	"release": "bumpp --commit --tag --push"
},
"devDependencies": {
  "bumpp": "^9.1.0"
}
```

这段代码是一个 npm script，用于自动化版本号的管理。具体来说，这个脚本会执行一个名为 `bumpp` 的命令，该命令是一个第三方的 Node.js 模块，用于更新项目的版本号，然后将更新后的版本号提交到 Git 仓库并打上标签，最后将本地代码推送到远程 Git 仓库。

更详细地说，这个命令做了以下几个操作：

1. 通过 `bumpp` 命令更新项目的版本号。可以根据参数指定更新的方式，比如 `--patch` 表示更新小版本号，`--minor` 表示更新中版本号，`--major` 表示更新大版本号，具体可以参考 `bumpp` 命令的文档。
2. 执行 `git add .` 命令，将更新后的文件加入 Git 索引。
3. 执行 `git commit -m "Bump version to <新版本号>"` 命令，提交更新后的文件。
4. 执行 `git tag <新版本号>` 命令，打上新版本号的标签。
5. 执行 `git push` 命令，将本地代码推送到远程 Git 仓库。
6. 执行 `git push --tags` 命令，将新打的标签推送到远程 Git 仓库。

这样，就完成了自动化版本号管理的流程。在执行这个命令前，需要先安装 `bumpp` 模块，并在 `package.json` 文件中配置好相应的版本号信息。



## 自动发布 npm 包

`package.json`

```json
"scripts": {
	"publish:ci": "esno scripts/publish.ts"
},
"devDependencies": {
  "esno": "^0.16.3"
}
```



`publish.ts`

```ts
import { execSync } from 'child_process'
import { version } from '../package.json'

let command = 'pnpm publish --access public --no-git-checks'

if (version.includes('beta'))
  command += ' --tag beta'

execSync(command, { stdio: 'inherit' })
```



这段代码是一个 npm script，用于在 CI/CD 环境中自动发布 npm 包。具体来说，这个脚本会执行一个名为 `esno` 的命令，并将 `publish.ts` 文件作为参数传入。`esno` 是一个第三方 Node.js 模块，它允许在不编译 TypeScript 的情况下执行 TypeScript 文件。

`publish.ts` 文件中定义了一个变量 `version`，它从 `package.json` 文件中获取当前版本号。然后根据当前版本号是否包含 "beta" 字符串来判断是否发布 beta 版本。如果是 beta 版本，则添加 `--tag beta` 参数。

最后，使用 `execSync` 函数执行 `pnpm publish` 命令，并传入一些参数。`pnpm publish` 命令用于将当前项目打包为一个 npm 包，并上传到 npm 仓库。其中，`--access public` 参数表示发布公共包，`--no-git-checks` 参数表示不进行 git 相关的检查，如检查是否有未提交的代码等。`stdio: 'inherit'` 参数表示将子进程的输出打印到控制台。

通过这个 npm script，可以方便地在 CI/CD 环境中自动发布 npm 包，并且支持发布 beta 版本。需要注意的是，在使用这个脚本前，需要先安装 `esno` 和 `pnpm` 模块，并且在 `package.json` 文件中配置好相应的版本号信息。



## dirs & files

```ts
import { join } from 'path'
import { slash } from '@antfu/utils'
import fg from 'fast-glob'
import { extsToGlob } from './utils'

import type { PageOptions, ResolvedOptions } from './types'

/**
 * Resolves the page dirs for its for its given globs
 */
export function getPageDirs(PageOptions: PageOptions, root: string, exclude: string[]): PageOptions[] {
  const dirs = fg.sync(slash(PageOptions.dir), {
    ignore: exclude,
    onlyDirectories: true,
    dot: true,
    unique: true,
    cwd: root,
  })

  const pageDirs = dirs.map(dir => ({
    ...PageOptions,
    dir,
  }))

  return pageDirs
}

/**
 * Resolves the files that are valid pages for the given context.
 */
export function getPageFiles(path: string, options: ResolvedOptions, pageOptions?: PageOptions): string[] {
  const {
    exclude,
    extensions,
  } = options

  const ext = extsToGlob(extensions)
  const pattern = pageOptions?.filePatern ?? `**/*.${ext}`

  const files = fg.sync(pattern, {
    ignore: exclude,
    onlyFiles: true,
    cwd: path,
  }).map(p => slash(join(path, p)))

  return files
}
```

这段代码是一个 TypeScript 模块，其中包含了两个函数 `getPageDirs` 和 `getPageFiles`，这两个函数都是用于解析页面文件的。在使用这些函数时，需要传入一个 `PageOptions` 对象和一些其他的参数。

`getPageDirs` 函数接受一个 `PageOptions` 对象，以及项目的根目录和要排除的文件列表作为参数，然后使用 `fast-glob` 包中的 `sync` 方法获取所有满足条件的目录，并将每个目录作为一个 `PageOptions` 对象返回。

`getPageFiles` 函数接受一个路径字符串和一个 `ResolvedOptions` 对象作为参数，用于解析指定路径下的页面文件。在此过程中，该函数使用 `fast-glob` 包中的 `sync` 方法获取满足指定条件的所有文件，并返回这些文件的数组。