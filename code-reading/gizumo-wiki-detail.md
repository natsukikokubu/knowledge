### API通信
atom = 初期値を定義している
* categoryState = []
* isLoadingState = false
* isErrorState = false

useCategoryState: 読み出すためのステート
* categories: categoryState
* isLoading: isLoadingState
* isError: isErrorState

useCategoryMutators: 書き込むためのステート
const { success, error } = useNotification() これわからない
* setCategories = useSetRecoilState(categoryState)
* const setIsLoading = useSetRecoilState(isLoadingState)
* const setIsError = useSetRecoilState(isErrorState)　

getCategoryList: isNeedLoadingがtrueになったら(サイドバーのカテゴリー一覧を押した時、新しいカテゴリーを追加した時)API通信をしてくれる
* finally
  - ページの更新が必要なかったかどうかに関わらず非同期処理が終わったことを示すために実行される
  - 非同期処理が完了したのでローディング状態がオンだったら必ずオフになる
  - 非同期処理の結果に関わらず、常に最後に実行される処理を設定する
* getCategoryListはgetCategoryListをreturnする

### organisms API通信で得たgetCategoryListを活用している
#### CategoryList
* categoryDetail = カテゴリーのステート管理(name, id)
* getCategoryList,deleteCategory = useCategoryMutatorsを呼び出し、その戻り値からgetCategoryListとdeleteCategoryという関数を取得
* categories,isLoading,isError = useCategoryStateのカスタムフックを使用して、カテゴリに関する状態を取得
* categoryRows =カスタムフックuseCategoryListを使って、categoryRowsという関数または値を取得
 - categoryRowsはフックスのファイルで取得したカテゴリー一覧からmapでnameとidを取り出して配列を返している
* memoizedCategoryRows = useMemoフックを使用してcategoryRowsの結果をメモ化してキャッシュしている
 - 第二引数の依存配列categoryRows、categoriesに変更がなければ再計算が行われずパフォーマンスが向上する
* useEffect = フックの開始部分
- 引数にtrueを渡すことでカテゴリリストを取得する動作を行う
- 空配列を第二引数にすることで初回レンダリング時のみ実行される


### CategoryEdit
使用しているprops
* className: コンポーネントに追加するクラス名
* categoryId: 編集対象のカテゴリーのID
* onSuccess: 更新が成功した時に呼び出される関数

#### 状態管理
const [categoryInfo, setCategoryInfo] = useState({
  name: '',
  category_id: null,
})
const [errors, setErrors] = useState({
  name: [],
  category_id: [],
})
const [selectedCategoryInfo, setSelectedCategoryInfo] = useState(null)
* categoryInfo: 入力されたカテゴリー情報(名前とID)を保持
* errors: 入力エラーのメッセージを管理
* selectedCategoryInfo: カテゴリーが選択された場合の情報を管理

#### APIから情報を取得
useEffect(() => {
  getCategoryDetail(categoryId).then((data) => {
    setCategoryInfo({
      name: data.name,
      categoryId: data.category?.id,
    })
    setSelectedCategoryInfo(
      data.category
        ? { value: data.category, label: data.category.name }
        : null
    )
  })
  // eslint-disable-next-line
}, [])
* コンポーネントが初めて描画されたときにgetCategoryDetail関数を使用してカテゴリーの詳細情報を取得している
* 取得したデータに基づいてcategoryInfoとselectedCategoryInfoの状態を更新している

#### 入力変更時の処理
const handleInputChange = useCallback((event) => {
  setCategoryInfo((prev) => ({ ...prev, name: event.target.value }))
  if (event.target.value) setErrors((prev) => ({ ...prev, name: [] }))
}, [])
* handleInputChange: フィールドに文字を入力するたびに呼ばれる関数
* 入力値をsetCategoryInfoに反映し、もし入力があればエラーメッセージをクリアしている

#### フォーム送信時の処理
const handleEditCategorySubmit = useCallback(
  (event) => {
    event.preventDefault()
    const { success, error } = editCategorySchema.safeParse(categoryInfo)
    if (error) {
      setErrors(error.flatten().fieldErrors)
      return
    }
    if (success) {
      setErrors({
        category_id: [],
        name: [],
      })
      updateCategory(categoryId, categoryInfo)
        .then(() => {
          onSuccess()
        })
        .catch((err) => setErrors({ name: [err.message] }))
    }
  },
  [categoryInfo, categoryId, updateCategory, onSuccess]
)
* event.preventDefault(): フォームの送信時のページリロードを防ぐ
* バリデーション: editCategorySchema.safeParseを使用して入力ちの検証を行う
* エラーがあればエラーメッセージを設定して処理を中断する
* 更新処理: 入力だ正しい場合updateCategory関数を呼び出してカテゴリー情報を更新
* 成功時にonSuccessを実行する、エラー時はエラーメッセージを表示する

#### JSX部分(レンダリング)
<!-- return (
  <form onSubmit={handleEditCategorySubmit} className={className}>
    <div className={styles['form-wrapper']}>
      <InputField
        className={styles['input-field']}
        label='カテゴリー名'
        htmlFor='category-name'
        type='text'
        value={categoryInfo.name}
        onChange={handleInputChange}
        errorText={errors.name ? errors.name[0] : ''}
      />
      <Button
        className={styles['button-wrapper']}
        buttonStyle='secondary'
        type='submit'
        disabled={isLoading}
      >
        保存
      </Button>
    </div>
  </form>
) -->
* formタグのonSubmitに送信処理を設定し、ユーザーが保存ボタンをクリックしたときに処理が走る
* InputField:入力フィールドのコンポーネント、カテゴリー名を入力するために使用されている、エラーがあればメッセージを表示する
* button: 保存ボタン、フォーム送信時にクリックされAPI呼び出しが実行される、isLoadingにより処理中はボタンが無効化される

#### PropTypesとDefault
CategoryEdit.propTypes = {
  className: PropTypes.string,
  categoryId: PropTypes.number.isRequired,
  onSuccess: PropTypes.func.isRequired,
}
CategoryEdit.defaultProps = {
  className: '',
}
* コンポーネントに渡されるプロパティの型定義
* defaultProps: classNameのデフフォルト値は空
