## docs
- [React Official Ref](https://react.dev/learn)
- [React Github](https://github.com/facebook/react/)
- [CodeSandBox](https://codesandbox.io/p/sandbox/react-new)

## setup
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