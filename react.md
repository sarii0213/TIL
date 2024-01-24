## docs
- [React Official Ref](https://react.dev/learn)
- [React Github](https://github.com/facebook/react/)
- [CodeSandBox](https://codesandbox.io/p/sandbox/react-new)

## Udemy notes
- [JavaScript and Ruby on Rails with React, Angular, and Vue](https://www.udemy.com/course/ruby-on-rails-react-angular/learn/lecture/12208432)
- [update:2024/01/17] lesson 79で、ブラウザのボタンを押すと#root要素内が消えるエラーが。理解がないままエラー対処は困難＆udemyだと取り組む人の母数的に同事象を検索することも困難なので一旦公式チュートリアルを進めることに。

### setup
- Reactの[CDN links](https://legacy.reactjs.org/docs/cdn-links.html)をheadタグに設置
  ```html
  <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
  ```

### 一番簡単なReactコード
```jsx
// src/app.jsx
ReactDOM.render(
    <h1>Hello, world</h1>,
    document.getElementById('root')
);
```
- ↑ 第一引数を、第二引数で指定した場所にrenderする

```html
<body>
    <script src="app.js"></script>
</body>
```

- node.jsのインストール（app.jsxをビルドしてapp.jsにする）
  - nvmのインストール（[参考](https://stackoverflow.com/questions/44803721/how-to-brew-install-specific-version-of-node)）
  - `nvm install 19`(npmが一緒にインストールされるnode.jsのバージョンを指定)
  - `npm init -y`でpackage.jsonが作られる
  - `npm install babel-cli@6 babel-preset-react-app@3`でjsxのコンパイラとしてbabelをインストール
  - `npx babel --watch src --out-dir . --presets react-app/prod`で、babelによってsrcディレクトリ（app.jsxがある場所）を監視して、output dirとしてプロジェクトファイルを指定して、presetsに`react-app/prod`という設定を指定（→ src/app.jsxから、プロジェクトファイルにapp.jsが生成される）


### React Component
- 定義したclassで`{this.props.name}`とすると、そのインスタンスのname propertyを代入できる
- `class Todo extends React.Component...`のようにして定義して、ReactDOM.render内で`<Todo />`のようにしてインスタンス化
- class定義
  - render()の中で、letで変数定義して、returnでインスタンスとして返すHTML要素を定義

```jsx
class Todo extends React.Component {

    constructor(props){
        super(props);  // インスタンスのpropertyが取得できるように

        this.state = ...
        // ブラウザの状態も反映できるように。render return内もthis.state.<property>で値取得するようにする。

        this.handleClick = this.handleClick.bind(this);
        // thisをバインドして、event handler内でthisを使えるようにする
    }

    handleClick(event){
        this.setState(state => ({
            done: !state.done
        }));
    }

    render(){
        let ...
        // let: 変数定義

        return ... 
               <input type="checkbox" checked={this.state.done} onClick={this.handleClick} /> 
        // return: インスタンスとして返すHTML要素
    }
}
```

---
---

## official tutorial notes
- [official tutorial](https://ja.react.dev/learn)

### コンポーネント
- コンポーネント：マークアップを返すJavascript関数
- 関数やクラスで宣言したコンポーネントは、別のコンポーネントにネストできる
  ```jsx
  // app.jsx
  export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />  // 大文字 = コンポーネント, 小文字 = HTMLタグ
    </div>
  );
  }
  ```
- default export: 他ファイルでimportされた時に、default exportされたモジュール（↑`MyApp`）はimport元ファイル（↑`app.jsx`）の全体を指すもの（＝メイン関数）になる
- returnできる要素はひとつだけ。`<div>...</div>` や空の `<>...</>` ラッパのような共通の親要素で囲む必要あり。
- JSXの中にJavascriptのコードを入れる時は`{}`で囲む

### リストのレンダー
- リスト内の各項目には、兄弟の中でそれを一意に識別するための文字列または数値を`key`に渡す必要がある `products.map(product => <li key={product.id}> ..)`
  - 識別子がないと、リストのアイテムを削除、並べ替え、項目の中身の更新を行なったかをプログラムは判断できない。プログラムはliのkeyプロパティを見て、コンポーネントを新規作成・破棄・移動などを行える。
  - keyはコンポーネントとその兄弟間で一意であればOK

### イベントに応答
- コンポーネント内にイベントハンドラ関数を宣言して、return内でイベントハンドラ関数を渡す
- イベント発生時（ボタンクリック時など）に、イベントハンドラは呼び出される

### 画面の更新
- state: コンポーネントに情報を記憶させるために使う
- 準備：`import { useState } from 'react`をする
- `const [something, setSomething] = useState(0)`で、現在のstateと、それを更新するための関数が得られる（←`something`に0を渡している）
  - `setSomething`関数をコールすることで、Reactはそのコンポーネントのstateに変更があったことを検知し、`something`というstate変数を使用しているコンポーネントたちの再renderがtriggerされる

### フックの使用
- `use`で始まる関数はフック。フックはコンポーネントのトップレベルでのみ呼び出せる。

### コンポーネント間でデータを共有
1. 子コンポーネントから親コンポーネントにstateを移動させる = "stateのリフトアップ"
2. 親コンポーネントから子コンポーネントにstateの情報を渡す `<MyButton count={count} onClick={handleClick} />`
   1. 渡される情報 = `props`


### tic-tac-toe チュートリアル
#### ローカルで環境構築
- `npm start`時に発生したエラー：`Error message "error:0308010C:digital envelope routines::unsupported"`
- 原因：Node.js v17で、SSLに関する大幅な修正があった。Node.jsとSSL providerのversion不一致が原因。
- 解決策：`npm i react-scripts@latest`（上記の不一致に対応したバージョンのreact-scriptsをインストール）（[参考](https://stackoverflow.com/questions/69692842/error-message-error0308010cdigital-envelope-routinesunsupported/71334532#71334532)）

#### ポイント
- コンポーネントの呼び出し部分のJSXコードにて、state更新を含んだ関数を実行することはNG
  - 理由：子コンポーネント呼び出し部分での関数呼び出し（例：`<Square onSquareClick={handleClick(0)} />`）は、親コンポーネントのレンダーの一部として発生。それによって呼び出された`handleClick(0)`は、`setSomething`を呼び出してstateを更新するため、親コンポーネント全体が再レンダーされる。 -> 無限ループへ


#### イミュータビリティ
- データを変更するための２つのアプローチ
  - ① データの値を直接書き換え（mutate）
  - ② 望ましい変更が施された新しいコピーで元のデータを置換 <- イミュータビリティ
- イミュータビリティの利点
  - 複雑な機能を簡単に実装できる
  - コンポーネントがデータが変更されたかどうかを低コストで比較できる（不必要な再レンダーをスキップするために必要）


### props vs state
- Reactには props と state という２種類の"モデル"データがある
- props: 関数に渡す引数のようなもの。親コンポーネントから子コンポーネントにデータを渡し、その外観をカスタマイズするために使う。例：`From`は`Button`にpropsとして`color`を渡せる。
- state: コンポーネントのメモリのようなもの。コンポーネントが情報を追跡し、ユーザ操作に反応して変更できるようにする。例：`Button`が`isHovered`というstateを保持。
- 親コンポーネントが`state`として情報を保持し、その情報を子コンポーネントに`props`として渡す



### Reactフレームワーク
- 新しいアプリやサイトをフルでReactを使って構築する場合：フレームワークの使用がおすすめ
- フレームワークを使うと不要になる作業：
  - ルーティングやデータ取得、新しい機能の追加のたびにライブラリをバンドラに結合
  - アプリに必要な最小限のコードを１回のクライアント・サーバ間の往復で送信しつつ、並行してページ表示に必要なデータ送信の実装
  - ページが段階的に読み込まれ、JSコードが実行すらされないうちから操作可能になるように実装
  - どこにでもホストできJSが無効になっていても動作する完全に静的なHTMLなファイルが入ったフォルダの生成
- 既存のページにReactを追加する場合はフレームワークなしで使うのも○
- Next.js
  - フルスタックのReactフレームワーク
  - ほぼ静的なブログサイトから複雑でダイナミックなアプリまでどんな規模のReactアプリでも作成できる
  - `npx create-next-app@latest`でNext.jsアプリ新規作成

#### Next.js
- [(official)Learn Next.js](https://nextjs.org/learn/dashboard-app)

- create a new project
  - `npx create-next-app@latest nextjs-dashboard --use-npm --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example"`
  - ↑ starter exampleのコードでNext.jsプロジェクトを新規作成（というかclone?）
  
- install the project's packages
  - `npm i`

- start the development server
  - `npm run dev`

##### file structure
- `/app`: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.
- `/app/lib`: Contains functions used in your application, such as reusable utility functions and data fetching functions.
  - DBもしくはAPIがまだ使えない場合、JSON or JSオブジェクトとしてモックのデータを作っておける -> DBのシードとして使う
  - DBから返される予定のデータタイプを定義（TypeScriptによって、型違いのデータが渡されることを防げる）
- `/app/ui`: Contains all the UI components for your application, such as cards, tables, and forms.
- `/public`: Contains all the static assets for your application, such as images.
- `/scripts`: Contains a seeding script that you'll use to populate your database in a later chapter.
- Config Files: You'll also notice config files such as `next.config.js` at the root of your application. Most of these files are created and pre-configured when you start a new project using create-next-app.


#### css styling
- `/app/ui/global.css`を、コンポーネントにimportすればグローバルcssを適用できる。トップレベルコンポーネントのみでのimportが推奨。
- Tailwindを使うと、TSX markupにclassとして直接css stylingができる
- CSS Modulesを使うと、JSXとstylesを分けられる（home.module.cssに`.shape {...}`とcss書いて、`import styles from '.../home.module.css'`して、`className={styles.shape}`）
- `clsx`ライブラリ：stateなどに応じてstyleをtoggleするのに便利
- 他にも、sassやCSS-in-JSライブラリ（styled-jsx, styled-components, emotion）などの選択肢がある

- [official: Next.js Styling](https://nextjs.org/docs/app/building-your-application/styling)
 

#### optimizing fonts
- `next/font`モジュールを使うと、ビルド時にフォントファイルをダウンロードし、static assetsと一緒に提供
- -> フォントのために追加のリクエストが生じず、HTML要素がページ表示時にずれたりパフォーマンスが下がるのを防げる
- カスタムフォント適用例
  - `app/ui/fonts.ts`にて`import { Inter } from 'next/font/google'`して、`export const inter = Inter({ subsets: ['latin']})`
  - `app/page.tsx`にて`import { inter } from '@/app/ui/fonts.ts'`して、`className={inter.className}`

#### optimizing images
- `<Image>`コンポーネントによって、layout shiftの防止、デバイスによってimageのresize、デフォでlazy loading、webpなどのモダンな拡張子も対応
- `import Image from 'next/image'`して、`<Image src="/hero-desktop.png" width={1000} height={760} className="hidden md:block" alt="Screenshots of the dashboard project showing desktop version" />`のように指定
- `width`と`height`の指定によって、layout shiftを防げる


#### layouts & pages
- `page.tsx`：ディレクトリを作りそこに`page.tsx`をおくと、新規でdirectoryと同じpathのrouteが作られる。Reactコンポーネントをexportするファイル。`page`ファイルのみパブリックアクセス可能。
- `layout.tsx`: 複数ページで共有できるUIを作れる。コンポーネントをimportして、レイアウトパーツとして使ったり。`<Layout />`コンポーネントが受け取る`children`propは、layout.tsxが置かれたディレクトリ下のpagesが含まれる
  - partial-renderingがされる◎
  - root layout = `app/layout.tsx`。必須ファイル。


#### navigation between pages
- `<Link />`コンポーネント：<a>タグだとfull page refreshになってしまうのを最適化
- ナビゲーションのviewで、`import Link from 'next/link'`して、`<Link />`コンポーネント使う
- Next.jsはルーティングの区分によって、コードを分割してロードする -> エラー時にも影響が局所的に。
- 本番環境では、`<Link>`コンポーネントがブラウザのviewpointに現れるとすぐにリンク先のコードを取得しておくので、ページ遷移が激速
- `usePathname()`で、active pageがわかる
  - usePathnameのようなルーター関連のhookはClient Componentでなければならない
  - ルーター関連のhook: ユーザーがブラウザ内でページ間を移動する時にURLの変更や履歴の管理を行うために使用
  - client component: ユーザーのブラウザ内で実行されるコンポーネント
  - ルーター関連のhookがclient componentでないといけない理由：
    - ルーターはブラウザの履歴とURLを操作する必要があるため、ブラウザのクライアント側でのみ機能する
    - ルーターフックは、コンポーネントのライフサイクルに依存して動作し、コンポーネントがマウントされたりアンマウントされたりしたときに発火。（？）サーバーサイドコンポーネントでは、ブラウザのコンポーネントライフサイクルが存在しないため、このようなフックの使用は制限される
    - クライアント側でのナビゲーションとルーティングは、ユーザーインタラクションに対応し、動的なURLの変更やコンポーネントの再レンダリングを行う必要があるため、クライアントコンポーネントで管理されることが一般的
  - `'use client'`とファイルの先頭に書くことで、ファイルがclient componentになる
  - `clsx`を使って、current pathとlinkのhrefが等しいかどうかでcss classを分岐したり


#### setting up database
- github repoを作成
- vercelのページからimport git repo -> deploy
- githubのrepoにpushすると、Vercelがdeployしなおしてくれる
- databaseのセットアップ
  - Storage > Create a database > Postgres > Continue
- databaseにconnectしたら、.env.local内をコピーして、自分のプロジェクトの.envにペースト
- `npm i @vercel/postgres`実行して、vercel postgres sdkをインストール
- シードの投入
  - `placeholder-data.js`にシードデータがあり、それをもとに`seed.js`でSQL発行してDBにシード投入
  - `package.json`の`scripts`に`"seed": "node -r dotenv/config ./scripts/seed.js"`を追加し、`npm run seed`を実行


#### fetching data
- APIを使うケース: APIを提供するthird partyを使う場合。clientからデータを取得＆clientにDBの秘密情報が露出するのを防ぐためにサーバー上でAPI layerを持つ
- DBクエリを使うケース：APIエンドポイントを作成する時に、DBと連携するためにロジックを書く場合。React Server Componentsを使う場合（=サーバーでデータ取得）、API layerなしで直接DBにクエリを送れる。
- DBクエリ：SQL文を書く or PrismaなどのORMを使う
- Using Server Components
  - Next.jsでデフォルトで使う
  - promisesというシンプルな非同期処理を提供し、`async/await`を、フックやライブラリなしで使える
  - サーバーで実行されるので、重い処理をサーバーに任せて、結果だけクライアントに送れる
  - サーバーで実行されるので、追加のAPI layerなしでDBにクエリを送れる
- Using SQL

### TypeScript


#### React + Rails でアプリ
- [Next.js(React)×Ruby on Rails チュートリアル(TypeScript対応)](https://musclecoding.com/next-js-rails-todo-tutorial/)