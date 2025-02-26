## Recoilとは何か
  - メタが開発した状態管理ライブラリ
  - Atomとselectorを使用してデータを管理することができる
  - Atom: データ(global state)の保存場所
  - Selector: Atomの値から計算された別の値

 Recoil の基本概念
Atom（状態の単位）
Atom はアプリ全体で共有する状態の「単位」です。今回のコードでは、todoState がその役割を果たしています。ここには、Todo の配列が格納され、どこからでも読み取りや更新が可能です。

Selector（派生状態）
Selector は、atom などの状態から新しい情報を算出するためのものです。今回の例では、incompleteTodoListState や completedTodoListState が使われており、todoState の値からそれぞれ未完了と完了の Todo をフィルターして返します。

* Atomにはkeyとdefaultの2つのプロパティを設定しなければならない
* key === Atomを一意に識別するための文字列
* default === 初期値を設定するためのプロパティ

Atomは、Global Stateとして扱われるため、複数のコンポーネントで同じAtomを使用することで、コンポーネント間で同じ状態を共有することができます。

import { atom } from "recoil"

const counterState = atom({
  key: "counterState",
  default: 0,
})

### 使用方法
<!-- import{useRecoilState}from"recoil"
const Counter = () => {
  const [counter, setCounter] = useRecoilState(counterState)

  return (
    <>
      <p>{counter}</p>
      <button onClick={() => setCounter(counter => counter + 1)}>+</button>
      <button onClick={() => setCounter(counter => counter - 1)}>-</button>
    </>
  )
} -->

## todos と setTodos の流れ
todos の読み取り
コンポーネント内部で useRecoilValue(incompleteTodoListState) を使うことで、Recoil の状態から「読み取り専用」の todos を取得しています。この読み取りは、表示やロジックで「最新の状態」を反映するために必要です。

setTodos の書き込み
useSetRecoilState(todoState) を使って、Recoil の atom に対して「書き込み」を行います。これによって、Todo の追加、更新、削除といった操作で状態を変更することが可能になります。

## なぜ読み取りと書き込みが分かれているのか？
単一の情報源の確保
Recoil では、atom を唯一の状態の情報源（ソース・オブ・トゥルース）としています。これにより、複数のコンポーネントが同じデータを参照・更新でき、状態の一貫性が保たれます。

読み取り専用のフック
useRecoilValue は状態を「読む」ためのフックです。これを使うことで、コンポーネントは必要な情報だけを取得し、直接変更しないので、予期せぬ状態の変更を防ぐことができます。

書き込み専用のフック
一方、useSetRecoilState は状態を「更新する」ためのフックです。状態の変更（例えば、Todo の追加や更新）を一元的に管理でき、どこで変更が発生してもその結果が自動的に他のコンポーネントに反映されます。

useRecoilState: どちらもできる: 無駄な再レンダリングが発生してしまう

メリット

状態の分離: 読み取りと書き込みが分かれていることで、データフローが明確になり、コードの保守性が向上します。
パフォーマンス: Recoil は依存関係を追跡し、必要なコンポーネントだけが再レンダリングされるため、効率的な更新が可能です。


## 実際の流れの例
初回レンダリング:
useEffect で API から Todo を取得し、setTodos を使って todoState に保存します。これにより、全てのコンポーネントは最新の Todo 一覧を参照できます。

Todo の追加:
フォームで新しい Todo を作成すると、API の結果を setTodos を使って既存の配列に追加します。これにより、状態が更新され、自動的に todos の一覧が再レンダリングされます。

Todo の更新や削除:
同様に、編集や削除の際にも API の応答を setTodos を使って反映させ、最新の状態を全体に伝えます。


## Local State
  - 範囲が限定される
      - ローカルステートは、特定のコンポーネントや関数内だけで管理される状態です。たとえば、Reactのコンポーネント内で使われるuseStateで定義するステートは、そのコンポーネント内に閉じた情報となります。
  - 用途
      - ボタンのオンオフ状態や入力フォームの値のように、特定の部分だけで完結する情報を管理するのに向いています。
  - メリット
      - 影響範囲が狭いため、変更が他の部分に及ばず、予測しやすい状態管理が可能です。


 ## Global State
  - 全体で共有する状態
    グローバルステートは、アプリ全体または複数のコンポーネント間で共有される状態です。たとえば、ユーザーのログイン情報やテーマ設定など、どのコンポーネントからもアクセスが必要なデータが該当します。
  - 用途
    多くの部分で同じデータを参照または更新する必要がある場合に使います。ReactではContext APIやReduxなどを利用して実現されることが多いです。
  - メリット
    一箇所で状態を管理することで、全体の一貫性を保ちやすくなります。ただし、管理が複雑になりがちなので、必要な部分に限定して使用するのがベストです。
