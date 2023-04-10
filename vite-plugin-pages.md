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



## 原理

当执行 `yarn dev` 时，Vite 将启动开发服务器，并在插件的 `configureServer` 方法中设置热重载。具体的执行顺序如下：

1. Vite 启动开发服务器，加载插件。
2. 插件的 `configResolved` 方法被调用，用于解析插件选项和页面配置，打印调试信息。
3. 插件的 `configureServer` 方法被调用，用于设置热重载，并将生成路由配置的函数传递给它。
4. 当页面发生更改时，插件的 `configureServer` 方法会重新生成路由配置，并将其存储在变量中。
5. 当客户端请求页面时，插件的 `load` 方法会生成客户端代码，并将路由信息作为参数传递给 Vue Router。
6. 插件的 `generateBundle` 方法在构建后被调用，用于替换输出代码中的方括号。

总之，vite-plugin-pages 通过解析文件命名规则，生成路由配置，并在运行时生成客户端代码来提供基于文件的路由功能。在开发服务器启动后，插件会自动处理热重载，并根据需要重新生成路由配置。



## generate

```ts
import { parse } from 'path'
import { Route, ResolvedOptions, ResolvedPages } from './types'
import {
  isDynamicRoute,
  isCatchAllRoute,
} from './utils'
import { stringifyRoutes } from './stringify'
import { sortPages } from './pages'

function prepareRoutes(
  routes: Route[],
  options: ResolvedOptions,
  parent?: Route,
) {
  for (const route of routes) {
    if (route.name)
      route.name = route.name.replace(/-index$/, '')

    if (parent)
      route.path = route.path.replace(/^\//, '')

    if (!options.react)
      route.props = true

    if (options.react) {
      delete route.name
      route.routes = route.children
      delete route.children
      route.exact = true
    }

    if (route.children) {
      delete route.name
      route.children = prepareRoutes(route.children, options, route)
    }

    if (!options.react)
      Object.assign(route, route.customBlock || {})

    delete route.customBlock

    Object.assign(route, options.extendRoute?.(route, parent) || {})
  }

  return routes
}

export function generateRoutes(pages: ResolvedPages, options: ResolvedOptions): Route[] {
  const {
    nuxtStyle,
  } = options

  const routes: Route[] = []

  sortPages(pages).forEach((page) => {
    const pathNodes = page.route.split('/')

    // add leading slash to component path if not already there
    const component = page.component.startsWith('/') ? page.component : `/${page.component}`

    const route: Route = {
      name: '',
      path: '',
      component,
      customBlock: page.customBlock,
    }

    let parentRoutes = routes

    for (let i = 0; i < pathNodes.length; i++) {
      const node = pathNodes[i]
      const isDynamic = isDynamicRoute(node, nuxtStyle)
      const isCatchAll = isCatchAllRoute(node, nuxtStyle)
      const normalizedName = isDynamic
        ? nuxtStyle
          ? isCatchAll ? 'all' : node.replace(/^_/, '')
          : node.replace(/^\[(\.{3})?/, '').replace(/\]$/, '')
        : node
      const normalizedPath = normalizedName.toLowerCase()

      route.name += route.name ? `-${normalizedName}` : normalizedName

      // Check nested route
      const parent = parentRoutes.find(node => node.name === route.name)

      if (parent) {
        parent.children = parent.children || []
        parentRoutes = parent.children
        route.path = ''
      } else if (normalizedName.toLowerCase() === 'index' && !route.path) {
        route.path += '/'
      } else if (normalizedName.toLowerCase() !== 'index') {
        if (isDynamic) {
          route.path += `/:${normalizedName}`
          // Catch-all route
          if (isCatchAll)
            route.path += '(.*)*'
        } else {
          route.path += `/${normalizedPath}`
        }
      }
    }

    parentRoutes.push(route)
  })

  const preparedRoutes = prepareRoutes(routes, options)

  let finalRoutes = preparedRoutes.sort((a, b) => {
    if (a.path.includes(':') && b.path.includes(':'))
      return b.path > a.path ? 1 : -1
    else if (a.path.includes(':') || b.path.includes(':'))
      return a.path.includes(':') ? 1 : -1
    else
      return b.path > a.path ? 1 : -1
  })

  // replace duplicated cache all route
  const allRoute = finalRoutes.find((i) => {
    return isCatchAllRoute(parse(i.component).name, nuxtStyle)
  })
  if (allRoute) {
    finalRoutes = finalRoutes.filter(i => !isCatchAllRoute(parse(i.component).name, nuxtStyle))
    finalRoutes.push(allRoute)
  }

  return finalRoutes
}

export function generateClientCode(routes: Route[], options: ResolvedOptions) {
  const { imports, stringRoutes } = stringifyRoutes(routes, options)

  return `${imports.join(';\n')};\n\nconst routes = ${stringRoutes};\n\nexport default routes;`
}
```



这段代码是用于生成路由配置的。它主要包括两个函数：`generateRoutes` 和 `generateClientCode`。这段代码是用于 Vite 插件 Voie 的，用于自动生成 Vue 和 React项目的路由配置。

1. `generateRoutes` 函数接收两个参数：`pages` 和 `options`。`pages` 是一个包含项目中所有页面信息的数组，`options` 是一些配置选项。这个函数的主要目的是根据页面信息生成一个路由配置数组。
2. `generateClientCode` 函数接收两个参数：`routes` 和 `options`。`routes` 是由 `generateRoutes`生成的路由配置数组，`options` 是一些配置选项。这个函数的主要目的是将路由配置数组转换为可在客户端执行的 JavaScript代码。

让我们通过一个简单的例子来了解这段代码的工作原理。

假设我们有以下项目结构：

```
src/
 pages/
 index.vue
 about.vue
 user/
 _id.vue
```

`generateRoutes` 函数将接收以下输入：

```javascript
pages = [
 { route: 'index', component: '/src/pages/index.vue' },
 { route: 'about', component: '/src/pages/about.vue' },
 { route: 'user/_id', component: '/src/pages/user/_id.vue' },
];

options = {
 nuxtStyle: true, // 使用 Nuxt.js 风格的动态路由
};
```

`generateRoutes` 函数将生成以下路由配置数组：

```javascript
[
 {
   name: 'index',
   path: '/',
   component: '/src/pages/index.vue',
 },
 {
   name: 'about',
   path: '/about',
   component: '/src/pages/about.vue',
 },
 {
   name: 'user-id',
   path: '/user/:id',
   component: '/src/pages/user/_id.vue',
 },
];
```

接下来，`generateClientCode` 函数将接收这个路由配置数组和配置选项，并生成以下客户端代码：

```javascript
import _default_0 from '/src/pages/index.vue';
import _default_1 from '/src/pages/about.vue';
import _default_2 from '/src/pages/user/_id.vue';

const routes = [
 { name: 'index', path: '/', component: _default_0 },
 { name: 'about', path: '/about', component: _default_1 },
 { name: 'user-id', path: '/user/:id', component: _default_2 },
];

export default routes;
```

这段客户端代码可以直接在 Vue 或 React项目中使用，以自动配置路由。



## Webpack

要在 webpack 中实现类似于 vite-plugin-pages 的功能，你可以编写一个自定义的 webpack 插件。这个插件将在编译期间自动生成路由配置。以下是实现这个插件的整体思路：

1. 创建一个新的 webpack 插件类，例如 `WebpackPagesPlugin`。

2. 在插件类的 `apply` 方法中，监听 webpack 的 `compilation` 事件。这个事件在每次编译开始时触发。

3. 当 `compilation` 事件触发时，执行以下操作：

   a. 使用 webpack 提供的 `glob` 或其他文件查找库，查找项目中的所有页面组件。这些组件通常位于一个特定的目录，例如 `src/pages`。

   b. 分析找到的页面组件，提取路由信息。例如，从文件名和目录结构中提取动态路由参数。

   c. 根据提取的路由信息，生成一个路由配置数组。这个数组应该与 React Router v5 的路由配置格式兼容。

   d. 将路由配置数组转换为 JavaScript 代码字符串，并将其添加到一个新的虚拟文件中。这个虚拟文件可以使用 webpack 的 `compilation.assets` API 添加到编译输出中。

4. 在项目的入口文件（例如 `src/index.js`）中，导入并使用生成的虚拟文件。这将自动配置 React Router v5 的路由。

以下是一个简化的 `WebpackPagesPlugin` 示例：

```javascript
const glob = require('glob');
const path = require('path');

class WebpackPagesPlugin {
  constructor(options) {
    this.options = options || {};
  }

  apply(compiler) {
    compiler.hooks.compilation.tap('WebpackPagesPlugin', (compilation) => {
      // 查找所有页面组件
      const pages = glob.sync(path.join(this.options.pagesDir, '**/*.jsx'));

      // 生成路由配置数组
      const routes = generateRoutes(pages, this.options);

      // 转换路由配置数组为 JavaScript 代码字符串
      const clientCode = generateClientCode(routes, this.options);

      // 将生成的代码添加到虚拟文件中
      compilation.assets['generated-routes.js'] = {
        source: () => clientCode,
        size: () => clientCode.length,
      };
    });
  }
}

function generateRoutes(pages, options) {
  // 实现类似于 generateRoutes 的逻辑
}

function generateClientCode(routes, options) {
  // 实现类似于 generateClientCode 的逻辑
}
```

要使用这个插件，你需要在 webpack 配置中将其添加到 `plugins` 数组中：

```javascript
const WebpackPagesPlugin = require('./webpack-pages-plugin');

module.exports = {
  // ...
  plugins: [
    // ...
    new WebpackPagesPlugin({
      pagesDir: path.join(__dirname, 'src/pages'),
      // 其他配置选项
    }),
  ],
};
```

最后，在项目的入口文件中，导入并使用生成的虚拟文件：

```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import routes from './generated-routes';

function App() {
  return (
    <Router>
      <Switch>
        {routes.map((route, index) => (
          <Route key={index} path={route.path} component={route.component} />
        ))}
      </Switch>
    </Router>
  );
}

export default App;
```

这样，你就可以在 webpack 中实现类似于 vite-plugin-pages 的功能，自动生成并配置 React Router v5 的路由。