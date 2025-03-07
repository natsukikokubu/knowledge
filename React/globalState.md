useContext、Recoil、Reduxの違い

https://blog.uhy.ooo/entry/2021-07-24/react-state-management/

https://zenn.dev/kazukix/articles/react-state-management-libraries

#### Redux
* 大規模なアプリケーションでは良い
  - アプリケーション全体の状態を一元管理して複数のコンポーネント間で状態共有や更新を効率的に行える
* コードの一貫性、保守性向上
* デバッグしやすい


#### Recoil
* Meta社が開発していることもありReactとの相性良い
* Redux のように特定のアーキテクチャを強制されない

* 小〜中規模のアプリに向きなので大規模だとエコシステム的にサポート体制が低い

#### useContext
* Reactの基本的なフックであるためシンプル
* 比較的小規模なプロジェクトが向いてる

* 状態が複雑になったりコンポーネントツリーが深くなると状態管理が難しい
* 小〜中規模のアプリに向きなので状態の一元管理には向いていない
