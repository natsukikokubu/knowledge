.
├── .github            # プルリクエストのテンプレなどGitHubの設定
├── .husky             # husky(commit時のGitフック)の設定
├── .scaffdog          # scaffdog(コンポーネント自動生成)の設定
├── .storybook         # Storybookの設定
├── .vscode            # ワークスペースのVSCode設定
├── assets             # 画像などの静的ファイル
├── docs               # API仕様書などのドキュメント
├── env                # 環境変数ファイル
├── src
│   ├── components     # Atomic Designによるコンポーネント管理
│   ├── hooks          # 汎用的なカスタムフック
│   ├── libs           # node_modulesのライブラリをラップしたwrapper関数
│   ├── routes         # ルーティング設定
│   ├── schemas        # バリデーションスキーマ
│   ├── stores         # グローバルstate(Recoil)
│   ├── styles         # 汎用的なスタイル
│   ├── utils          # 汎用的な関数
│   └── main.jsx       # Reactのルートコンポーネント
├── .eslintrc.json     # ESLintの設定ファイル
├── .gitignore         # gitの管理対象から外すファイルを記載
├── .lintstagedrc.js   # lint-staged設定ファイル
├── .prettierrc.json   # Prettierの設定ファイル
├── .stylelintrc.json  # Stylelintの設定ファイル
├── index.html
├── jsconfig.json      # JavaScriptの設定ファイル
├── package-lock.json  # インストールしたパッケージの詳細なバージョンを記載
├── package.json       # インストールするパッケージのバージョン範囲とその他設定
└── vite.config.js     # Viteの設定ファイル

srcの中身
### src
#### components: Atomicデザインで作成されている
* atoms: 最小単位の UI コンポーネント ex: Button, Heading,それぞれstories.jsxに使い方とかが書いてある
  - Button
  - ButtonLink: import{Link}
  - Heading: 見出し
  - Icon:
  - Input
  - Label
  - Loading
  - Select
  - Text
  - TextLink
  - WysiWyg

* molecules: atoms を組み合わせた UI コンポーネント
  - DropDown
  - HeadingWithButtonLink
  - InputField
  - Modal
  - NaviItem
  - PageBack
  - SelectField
  - Toast
  - WysiWygField

* organisms: ドメイン知識を持つ API 通信によりリソースのやり取りを行う
  - Article
  - Category
  - LoginForm
  - TheHeader
  - TheSidebar

* templates: organisms を使用した再利用可能なテンプレート
  - HeaderWithSidebar

* pages: URL に対応した一意なページ React Router の path が各ページにマッピングされる
  - Article
  - Category
  - Home
  - Login

### その他
* hooks
* routes
* stores
  - categoryState.js


### わからない用語、調べたいものなど
* clsx
* oneOfTypes
* oneOf
* const minLevel ~
* parameters,docs,description
* memo useMemo
* BiCategory
