# Vite HMR Public Copy
## ⚡ Vite plugin to allow copying public files with HMR

> **Note**
> Currently, Vite does serve the publicDir files, so most people shouldn't need this plugin.

Vite has a configuration called [publicDir](https://vitejs.dev/guide/assets.html#the-public-directory) that allows simply copying all the files contained in that directory to the build.outDir. This is provided as a utility to allow simple files that don't need to be processed by vite to be replicated in the build directory. Unfortunately, Vite doesn't create those files in the outDir during HMR, it simply serves them (they are accessible by a URL in the browser).

Sometimes, though, we need the files to exist on disk at the same place as they will be after the build, all the while keeping HMR active, so this plugin simply copies the files at dev server start and reacts to HMR file changes so they're always on disk and updated at their final destination. This allows, for example, files like svg icons to be rendered inline directly by a proxied server or language files to be loaded by a server-side framework.


```shell
npm i -D vite-plugin-hmr-public-copy
```

or

```
yarn add -D vite-plugin-hmr-public-copy
```

## Usage

Add ```ViteHmrPublicCopy()``` to your Vite config

```js
import ViteHmrPublicCopy from 'vite-plugin-hmr-public-copy';

export default {
  plugins: [
    ViteHmrPublicCopy({
      copy: true,
      copyAtStart: true
    })
  ]
}
```

## Options

### `copy`

Type: `Boolean | Array`
Default: `true`

Boolean by default for simplicity and potentially allow shortcircuit by providing false. Leaving `true` simply tells the plugin to watch for all publicDir files recursively.
Passing an array allows to watch multiple directories, possibly to only watch some publicDir subdirectories instead. If passing an array, the paths will not be resolved automatically, so you have to to it yourself and build the globbing string.

```js
import { resolve } from 'path'

ViteHmrPublicCopy({
    copy: [
        `${resolve(__dirname, 'my/public/dir/subdirectory')}/**/*`
    ]
})
```

### `copyAtStart`

Type: `Boolean`
Default: `true`

Tells the plugin to copy the files as soon as the HMR server starts. Pass false to only copy files on HMR update event a.k.a. when they're edited.


## Contributions
This plugin has been quickly whipped together for my own needs, but if you want to help improve this, all pull requests are welcome. Typescript refactoring would be appreciated.
> **Warning** 
> Please do not raise issues without tested pull request as those will most likely be ignored or only reviewed when I have time.
