# tr-test

## これは firebase(RealtimeDatabase)＋ Nuxt.js(TypeScript) のプロジェクトです

## セットアップ

1. ローカルにコードを落とす

```
$ git clone https://github.com/Ryoka-Terada/tr-test.git
```

2. npm パッケージをインストール

```
$ npm install
```

3. 各種ファイルを作成

- .firebaserc ファイルをプロジェクトルートに作成

### .firebaserc

```
{
  "projects": {
    "default": "[firebaseのプロジェクト名]"
  }
}
```

- plugins フォルダをプロジェクトルートに作成。その下に firebase.js と vuetify.js を作成

### plugins/firebase.js

```
import { initializeApp } from "firebase/app"

const firebaseConfig = {
  apiKey: "XXXXXXXXXXXXXX",
  authDomain: "XXXXXXXXXXXXXX",
  databaseURL: "XXXXXXXXXXXXXX",
  projectId: "XXXXXXXXXXXXXX",
  storageBucket: "XXXXXXXXXXXXXX",
  messagingSenderId: "XXXXXXXXXXXXXX",
  appId: "XXXXXXXXXXXXXX",
  measurementId: "XXXXXXXXXXXXXX"
};

const firebaseApp = initializeApp(firebaseConfig);
export default (context, inject) => {
  inject('firebase', firebaseApp)
}
```

### vuetify.js

```
import Vuetify from 'vuetify/lib'
```

※補足・firebase の RealtimeDatabase を使っている。RealtimeDatabase の構成は以下。

```RealtimeDatabase
DB名
 └ books
    └ id
       ├ author: "data"
       ├ passage: "data"
       └ title: "data"
```

## サイトの確認

firebase でデプロイ済(2022/03/03)
以下から実際の動きを確認できます。
https://tr-test-ec184.web.app/

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out the [documentation](https://nuxtjs.org).

## Special Directories

You can create the following extra directories, some of which have special behaviors. Only `pages` is required; you can delete them if you don't want to use their functionality.

### `assets`

The assets directory contains your uncompiled assets such as Stylus or Sass files, images, or fonts.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/assets).

### `components`

The components directory contains your Vue.js components. Components make up the different parts of your page and can be reused and imported into your pages, layouts and even other components.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/components).

### `layouts`

Layouts are a great help when you want to change the look and feel of your Nuxt app, whether you want to include a sidebar or have distinct layouts for mobile and desktop.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/layouts).

### `pages`

This directory contains your application views and routes. Nuxt will read all the `*.vue` files inside this directory and setup Vue Router automatically.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/get-started/routing).

### `plugins`

The plugins directory contains JavaScript plugins that you want to run before instantiating the root Vue.js Application. This is the place to add Vue plugins and to inject functions or constants. Every time you need to use `Vue.use()`, you should create a file in `plugins/` and add its path to plugins in `nuxt.config.js`.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/plugins).

### `static`

This directory contains your static files. Each file inside this directory is mapped to `/`.

Example: `/static/robots.txt` is mapped as `/robots.txt`.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/static).

### `store`

This directory contains your Vuex store files. Creating a file in this directory automatically activates Vuex.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/store).
