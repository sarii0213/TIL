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
- リスト内の各項目には、兄弟の中でそれを一意に識別するための文字列または数値を渡す必要がある `products.map(product => <li key={product.id}> ..)`
  - リストのアイテムを削除、並べ替え、項目の中身の更新を行なったかをプログラムは判断できない。プログラムはliのkeyプロパティを見て、コンポーネントを新規作成・破棄・移動などを行える。
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
2. 親コンポーネントから子コンポーネントにstateを渡す `<MyButton count={count} onClick={handleClick} />`
   1. 渡される情報 = `props`


### tic-tac-toe チュートリアル
#### ローカルで環境構築
- `npm start`時に発生したエラー：`Error message "error:0308010C:digital envelope routines::unsupported"`
- 原因：Node.js v17で、SSLに関する大幅な修正があった。Node.jsとSSL providerのversion不一致が原因。
- 解決策：`npm i react-scripts@latest`（上記の不一致に対応したバージョンのreact-scriptsをインストール）（[参考](https://stackoverflow.com/questions/69692842/error-message-error0308010cdigital-envelope-routinesunsupported/71334532#71334532)）

#### ポイント
- コンポーネントの呼び出し部分のJSXコードにて、state更新を含んだ関数を実行することはNG
  - 理由：子コンポーネント呼び出し部分での関数呼び出し（例：`<Square onSquareClick={handleClick(0)} />`）は、親コンポーネントのレンダーの一部として発生。それによって呼び出された`handleClick(0)`は、`setSomething`を呼び出してstateを更新するため、親コンポーネント全体が再レンダーされる。 -> 無限ループへ
- 

#### イミュータビリティ
- データを変更するための２つのアプローチ
  - ① データの値を直接書き換え（mutate）
  - ② 望ましい変更が施された新しいコピーで元のデータを置換 <- イミュータビリティ
- イミュータビリティの利点
  - 複雑な機能を簡単に実装できる
  - コンポーネントがデータが変更されたかどうかを低コストで比較できる（不必要な再レンダーをスキップするために必要）


### Reactフレームワーク
- Next.js