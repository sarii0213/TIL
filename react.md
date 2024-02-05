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
- [official docs](https://nextjs.org/docs)

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
  - client componentでデータ取得が必要な場合、server componentで取得してそれをpropとしてclient componentに渡せばOK
- Using SQL
  - request waterfall 
    - = 一つ前のリクエストがデータを返すまで、次のリクエストを送れない (= sequential <-> parallel)  
    - waterfallである必要があるケースもあるが、意図せずパフォーマンスに影響を与えてしまわぬよう注意
  - parallel data fetching
    - `Pormise.all`の中で、リクエストを列挙すると、並行処理が可能


#### static & dynamic rendering
- static rendering
  - データ取得＆レンダリングのタイミング = ビルド時 or revalidation時（キャッシュが消えた時など）
  - データ取得後、CDNによって配布＆キャッシュされる
  - 利点：ぺージ表示が高速＆サーバー負荷軽い＆SEOに◎（ページロード時にコンテンツが既に表示されているのでクローラーがインデックスしやすい）
  - 向いてるもの：データがない or ユーザー共通で表示するデータ
- dynamic rendering
  - レンダリングのタイミング = ユーザーがページ訪問時
  - 利点：データのリアルタイム性＆ユーザー特有のコンテンツの提供に◎＆リクエスト時の情報（cookies, URL params）にアクセス可能
  - 難点：アプリケーション全体が、データ取得が最も遅いものに合わせたページ表示速度になってしまう -> streamingで対処
- staticからdynamicにする方法
  - `import { unstable_noStore as noStore } from 'next/cache'`
  - `export async function ..() {}`の中で、`noStore()`を最初に呼び、キャッシュをさせない


#### streaming
- routeをchunkに小分けして、サーバーからクライアントにできた順に送っていくデータ転送テクニック
- Reactにおいては、コンポーネントがchunkとして扱える◎
- streamingを実装する方法
  - ページレベルでは、`loading.tsx`ファイルを使用
    - `loading.tsx`: ロード中に表示するfallback UI
    - 静的コンテンツはそのまま表示され、動的コンテンツのみ、ロード中の表示に
    - loading skeleton: ロード中であることをユーザーに伝えるplaceholder
    - Loadingコンポーネント内にskeleton UIを配置すると、静的ファイルとして埋め込まれる（`Loading...`などと文字を表示するよりユーザーフレンドリーなUIに）
  - 特定のコンポーネントにおいては、`<Suspense>`を使用
    - `<Suspense fallback={<コンポーネントロード中に表示するskeleton />}><ロードするコンポーネント></Suspense>`のように書く


#### route groups
- URL path構造に影響を与えずに、ロジカルグループにファイルを整理できる
-  `(overview)`ディレクトリなど`()`をつけるtことで、URL pathに含まれないようにできる


#### search feature
- URL search paramsを使う
- URL paramsの利点
  - ユーザーが検索結果をブックマークしたりシェアできる
  - URL paramsはサーバーで直接利用され、初期状態をレンダリングされるので、server-side renderingがやりやすい
  - URLでのサーチクエリやフィルターは、ユーザーの行動をトラッキングしやすい（追加のclient-sideロジックが不要）
- 検索機能で使われるhook
  - `useSearchParams`: current URLのパラメータを返す
  - `usePathname`: current URLのパス名を返す
  - `useRouter`: client component内でルーティング間のナビゲーションが可能
    - `replace`メソッド：URLを引数に更新
- 実装手順
  - ユーザーのinputをcapture
  - search paramsでURLを更新
  - URLとinput fieldを同期して保持
  - サーチクエリを反映するようテーブルを更新
- ポイント
  - event lister, hookが使えるようにClient Component化 (`'use client`)
  - `URLSearchParams`: URL query parametersの操作に便利なWeb API。
  - `useSearchParams`で取得した、現在のURLのパラメータを保持しつつ、`?query=<検索文字>`をURLに追加している
  - `defaultValue`: `value`はstate使用時に使うもの。（=Reactがinput stateを管理）search queryをURLとして保存しており、state不使用なので`defaultValue`でOK（=inputそのものが自身のstateを管理）
  - client componentでは、clientからparamsにアクセスするために`useSearchParams()`hookを使用
  - server componentでは、pageからコンポーネントへ`searchParams`propを渡して使える
- Debouncing: 関数が発火する頻度を制限できる（ユーザーのタイピングが終わったタイミングのみ、など）
  - `npm i use-debounce`
  - `useDebouncedCallback`で発火頻度を制御したいfunctionをwrapして、指定したm秒数（=ユーザーがタイプをやめてからの時間）後に、コードを実行するようにできる
  - -> databaseへのリクエスト送信を減らせ、リソースを節約できる


#### manipulating data
- server actions
  - 非同期コードをサーバーで直接実行できる（データ操作のためのAPIエンドポイント不要）
  - セキュリティ対策も◎（POSTリクエスト、暗号化クロージャ、厳格な入力値チェック、エラーメッセージのハッシュ化、ホストの制限など）
- `form`タグの`action`属性にて、actionsを発火させられる（actionはデータを含んだFormDataオブジェクトを受け取る）
  - server actionsがPOST APIエンドポイントを作成してくれるので、手動でエンドポイントを書かなくてOK
  - formDataオブジェクトの値を、FormSchemaで定義されたバリデーションルールにしたがって解析している（parseメソッドにて）
- server component内でserver actionを発火させる利点：progressive enhancementなので、JSが無効なクライアントでも動く
  - progressive enhancement: まず基本的な機能がすべてのユーザーに提供され、それから対応可能なブラウジング環境やデバイスを持つユーザーに対して、追加の機能や高度なエクスペリエンスを提供。つまり、低い能力を持つデバイスや古いブラウザを使用するユーザーにもウェブサイトが動作することを確保しつつ、モダンなブラウザやデバイスを利用するユーザーにはより多くの機能を提供。
- `caching`との密接な関係：server actionを通してフォームが送信されると、APIを使って関連するキャッシュもrevalidateされる（`revalidatePath`, `revalidateTag`など）
- `'use server'`と書いてfunctionsを列挙すると、それらがclient/server componentsにimportされた時にserver functionsとして機能する
- バリデーション
  - `zod`というtypescriptのvalidation libraryを使ったり
- レコード生成
  - `@vercel/postgres`から`sql`をインポートして、sql文を書いてINSERT
- revalidate
  - 自動でキャッシュされたページを表示されないよう、`revalidatePath`で再評価するパスを指定 
  - -> サーバーから新たにデータ取得が行われる
- redirect（パスを指定）
- update function
  - dynamic route segmentを作成
    - ディレクトリ名を`[]`で囲むと、動的にルーティングが作れる
  - page componentでは、`searchParams`に加えて`params`をpropとして受け取れる
- 金額の扱い方
  - ドルではなくセントで扱う。Javascript floating-point errorsを排除するため。
- JSのbind
  - `updateInvoiceWithId = updateInvoice.bind(null, invoice.id)`
  - = `updateInvoice`関数の"this"の参照先をwindowに変え、関数への引数として`invoice.id`を渡した新しい関数を作成している
  - Nextjs tutorial 「JS `bind`を使って`id`をServer Actionに渡すことで、Server Actionに渡されたあらゆる値がエンコードされることを保証する」

#### handling errors
- try/catchの中にredirectは書かない
  - 一部のWebアプリフレームワークやライブラリでは、ページのリダイレクトを行うためにエラーをスローするようなメカニズムを用いていることがある
  - try内でエラーがスローされると、近くのcatchに移動してそのエラーを処理することになってしまう
  - 意図しないエラー処理を防ぐために、tryの中にredirectは書かないのが◎
  - (revalidatePathはtry/catchの中でも外でもどちらでもいいっぽい)
- error.tsx
  - needs to be a client component
  - catching **ALL** errors
  - two props; error, reset
    - error: JS native error objectのインスタンス
    - reset: error boundaryをリセットする関数。実行されると、route segmentをre-renderする。
- notFound function
  - `import { notFound } from 'next/navigation'`して、notfoundの対象がない分岐にて`notFound()`呼び出す
  - urlに沿った箇所に`not-found.tsx`ファイルを作成し、not found時のUI作成


#### improving accessibility
- accessibility check plugin: `eslint-plugin-jsx-a11y`
- form validation
  - client-side validation
    - input要素にrequired属性を追加（ブラウザでのバリデーション）
  - server-side validation
    - `useFormState`hookをimport。hookなのでclient componentに。
      - useFormState takes two arguments: `(action, initialState)`
        - `action`: バリデーション対象のaction
        - `initialState`: バリデーション結果を格納するobject（=state）の初期値。`{ errors?: {...}, message?: ... }`など自由に決められる。
      - useFormState returns two values: `[state, dispatch]` (form state, dispatch function)
        - `state`: formの状態のobject。`state.errors..`とバリデーション結果を取得できる。
        - `dispatch`: `action`を実行するための関数。formタグのactionに`dispatch`を指定すると、Next.jsが解析してaction属性の値を作ってくれる。
      - useFormState hookから渡されるstateを含んだ`prevState`は、バリデーション対象のactionにて必須のprop
      - `parse()`を`safeParse()`に変更 -> success or errorを含んだobjectを返す。try/catchが不要。
      - 上記objectがsuccessでなかったら、早期リターンしてerrors, messageを返す
        - `validatedFields.error`と`validatedFields.error.flatten()`で形がだいぶ変わる。flatten()はZodで用意された関数。
    - aria labels
      - `aria-describedby`: 入力欄（`aria-describedby`）とエラーメッセージ表示部分(`id`)のつながりを示す
      - `aria-live="polite"`: contentに変化があり（ユーザーがエラーを直すなど）、入力が止まっている時に、エラー内容の更新を通知
      - `aria-atomic`: assistive technologyが変化した部分の全て,もしくは変化した部分のみを通知するかの指定。true=変化した部位を含む要素全体が通知される。


#### adding authentication 
- authentication & authorization
  - authentication: checks who you are
  - authorization: determines what you can do or access in the app
- NextAuth.jsを使う
  - authenticationを楽に実装できる便利ライブラリ
  - `npm install next-auth@beta`(← Next.js 14と互換性あり)
  - `openssl rand -base64 32`: generate a secret key for the app. this key is for cookies which ensures the security of user sessions
  - `.env`に上記コードで生成したsecret keyをコピペ `AUTH_SECRET=<generated_secret_key>`
  - rootに`auth.config.ts`ファイルを作成
    - NextAuth.jsの設定オプションを含む`authConfig`objectをexportするファイル
    - protect routes with Next.js Middleware
      - `authorized` callback: Next.js Middlewareを経由してリクエストがページアクセス権限を持つかをチェック
      - `authorized`の引数 `auth`, `request`
        - `auth`: user sessionを含む
        - `request`: incoming requestを含む
      - `providers` option: 他のログインオプションを列挙する場所
  - import the `authConfig` object into a Middleware file `middleware.ts`
    - NextAuth.jsとともに`authConfig` objectをinitialze -> `auth` propertyをexport
    - Middlewareの`matcher`オプションを使って、特定のパスでのみ実行されるように指定
  - Middlewareを使うメリット
    - 保護されたroutesは、middlewareがauthenticationを検証するまで、レンダリングされないため、セキュリティ＆パフォーマンスに◎
- password hashing
  - password hashing に使うのは`bcrypt` (Node.js APIに依存)
  - `auth.ts`ファイルを作成し、`authConfig`を展開（Next.js Middleware内でNode.js APIは使えないため、`auth.config.ts`とは別で用意する必要あり）
    - `providers` optionにCredentials providerを追加（= username, passwordでlogin）
      - `OAuth` or `email` providers are recommended
    - `auth.ts`から`signIn`, `singOut`をimportすれば、ログイン・ログアウト処理が実装できる
- zodでemail, passwordのvalidation before checking if the user exists in the db
- getUser functionでemailが同じuserを取得
- `bcrypt.compare`でパスワード比較
- `login-form.tsx`コンポーネントにて、`useFormState`と`useFormStatus`（handle pending state）を使う

#### adding metadata
- metadata: SEO, SNSでの共有などで効果的
- metadataの種類
  - title metadata `<title>...</title>`
  - description metadata `<meta name="description" content="..." />`
  - keyword metadata `<meta name="keywords" content="..." />`
  - open graph metadata
    - ```html
      <meta property="og:title" content="Title Here" />
      <meta property="og:description" content="Description Here" />
      <meta property="og:image" content="image_url_here" />
      ```
  - favicon metadata `<link rel="icon" href="path/to/favicon.ico" />`
- two ways to add metadata to your app
  - config-based: export a static metadata object or a dynamic generateMetadata function in a `layout.js` or `page.js`
  - file-based:
    - `favicon.ico`, `apple-icon.jpg`, `icon.jpg`: for favicons and icons
    - `opengraph-image.jpg` and `twitter-image.jpg`: for social media images
    - `robots.txt`, `sitemap.xml`: for search engine crawler
- metadataの設定例
  - ```javascript
    import { Metadata } from 'next';

    export const metadata: Metadata = {
      title: {
        template: '%s | Acme Dashboard',
        default: 'Acme Dashboard',
      },
      description: 'The official Next.js Course Dashboard, built with App Router.',
      metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
    };
    ```


#### 疑問
- importで`{}`で囲むのは、default exportでない場合 or exportされたものが複数ある場合？
- UUIDs vs. Auto-incrementing Keysはどこで設定している？ -> create table時に、idをuuidにするかauto increment keyにするか指定
- `<UserCircleIcon className="pointer-events-none absolute left-3 top-1/2 h-[18px] w-[18px] -translate-y-1/2 text-gray-500" />` 18pxを`[]`で囲むのは何？
  - Tailwindでは、特定の値を指定するのに`[値]`という構文が使われる
- const user = await sql<User>`SELECT * FROM users WHERE email=${email}` `<User>`はなに？型指定？
  - Promise型（`Promise<User | undefined>`）を用いることで、非同期処理の実行完了後の値を定義することができる

### TypeScript
- Javascriptを拡張して作られたもの（コンパイルするとJSに変換される）
- 大人数のプログラマーが携わる場合でもエラーを防ぎやすいように設計されている
- JSは動的型付け、TSは静的型付け
  - 動的型付け：コード実行時にデータ型が自動で決まる。実行しないとエラーがわからない
  - 静的型付け：変数のデータ型をあらかじめ決められる。コンパイル時点でエラーがわかる、コードが読みやすい
- TODO: [TS入門 トラハック YouTube](https://www.youtube.com/watch?v=kd8VH10jXwc&list=PLX8Rsrpnn3IW0REXnTWQp79mxCvHkIrad)


#### React + Rails でアプリ
- [Next.js(React)×Ruby on Rails チュートリアル(TypeScript対応)](https://musclecoding.com/next-js-rails-todo-tutorial/)