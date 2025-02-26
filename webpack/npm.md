## npmとは
「Node Package Manager」の略称でNode.jsのパッケージ管理ツールのこと
パッケージとは他のプログラムから使用するための関数とかクラスとかのこと
様々なパッケージを提供しているもの(npmレジストリで管理されている)
npm installするとpackage.jsonに追記される:パッケージの名前とバージョンが記載されている
node_modules:ここに中身がインストールされている

## npm scriptとは(設定する理由)
 - テストとかビルドとか簡単にコマンドを入力するだけで実行できるから
 - npm scriptに書かれたコマンドを実行するとnode_modules配下のパッケージを必ず使用してくれる
   - package.jsonに書かれているバージョンが実行されるためチームで同じバージョンが使用できる

## npm installとは(する理由)
プロジェクトごとにローカルインストールが一般的であるから
バージョンの違いで動作に差異があるためバージョン管理が必要であるから
開発者のバージョンを統一できるから
npm install ${パッケージ名}でローカルインストール
npm install -g ${パッケージ名}でグローバルにインストール


cross-env NODE_ENV=production webpack --progress
それぞれ何か

devDependencies
dependencies の違い

package-lock.jsonについて

reactTodoの時に使用していたパッケージ
