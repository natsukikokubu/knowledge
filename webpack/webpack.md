## webpackとは(webpackの役割)
モジュールバンドラーで複数のファイルをまとめてくれてブラウザ読み込む回数を減らしてくれている
- モジュール:機能ごとに分割されたファイルのことjsファイルとか
- バンドル: 機能ごとに分割された複数のファイルを束ねてくれるもの

- ユーザーに早く提供できる
    - 仮にjsファイルが10個あったとするとHTTPリクエストを10回送信しなければならないがバンドルしてあれば1回
- 依存関係を解決してくれる
    - 様々なライブラリやモジュールが増えると読み込む順番によってエラーが生じたりするがscriptの中がシンプルにbundle.jsになる　ためライブラリごとの依存関係を気にしなくても良い

## entry pointsとは
- バンドルの構築を開始するファイルを指す
- entry pointsを起点にそのファイルをimportしているかどうかで依存関係をバンドルの対象を決める

## outputとは
- バンドルされたファイルの出力先
- 絶対パスを指定する必要がある

## loaderとは
- webpackはデフォルトでjsファイルとJSONファイルのみしか扱うことができない
- ローダーによりcssファイルや画像ファイル,多言語などをjsにトランスパイルしてくれる
- ローダーには様々な種類がある
- rulesの中に書いてある

## pluginsとは
- バンドル時に実行される様々な処理
- ビルドツールやバンドラーツールを拡張してくれるもの

- mini-css-extract-plugin・・・バンドル時にcssファイルを生成
- html-webpack-plugin・・・バンドル時にhtmlファイルを生成

## modeとは
- バンドル結果をどの環境向けに出力するかを決める
- モードの設定は必須でdevelopment,production,none
- development:開発モードでのビルド,ファイルの圧縮されない
- production:本番モードのビルドになるファイルの圧縮をするため開発モードよりもファイルは小さい
- none:バンドルの最適化を無効にするモード

阿部寛
早く読み込める:SEOの観点:検索エンジン:早いものほど出てきやすい


#### 現在のモジュールの場所を基準にして、src と public（dist）というディレクトリの絶対パスを取得する仕組み
### const filename = fileURLToPath(import.meta.url);
- import.meta.url このファイル自体のURlを表す
- fileURLToPath関数はこのURLをファイルシステム上のパスに変換する
- ファイルの絶対パスを代入している

### const dirname = path.dirname(filename);
- path.dirname 関数は、与えられたファイルパスからディレクトリ部分のみを抽出する
- 先ほど取得したファイルパス（filename）からそのファイルが存在するディレクトリパスを dirname に代入している

### const src = path.resolve(dirname, './src');
- 複数のパス要素を結合して絶対パスを生成している
- 先ほどのディレクトリパス（dirname）と相対パス './src' を結合し、src ディレクトリの絶対パスを作成している

### const dist = path.resolve(dirname, './public');
- dirname と相対パス './public' を結合して、public ディレクトリ（よく配布用ファイルが置かれるディレクトリ）の絶対パスを生成し、dist に代入して

import path from 'path';
<!-- Node.jsの組み込みモジュールである path を読み込むための文 -->
import { fileURLToPath } from 'url';
import MiniCssExtractPlugin from 'mini-css-extract-plugin';


varの理由: 昔のブラウザ対応のため

### [name]: この書き方について調べる
- プレースホルダーで(雛形的なやつ)この場合エントリーポイントの名前を代入できる

### babel-loaderもともと含まれているのか
- 基本package.jsonに含まれているから改めてinstallしなくても良い

plugins: [
    new MiniCssExtractPlugin({
      filename: 'css/[name].css',
    }),
  ], これは何

なぜ開発モードの時は圧縮されない方がいいのか
