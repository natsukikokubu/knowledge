#### categoryEdit/index.jsx
const { isLoading } = useCategoryState()
このように分割代入が使用されている部分がたくさんあった。。
分割代入とは何か、それが変数名はどこいった、となったので分割代入について改めて調べる必要があると感じた。

ざっと読んだ結果どうやらオブジェクトのkeyを変数名に指定するとそこにはそのオブジェクトのvalueが代入されるらしい。

const { success, error } = editCategorySchema.safeParse(categoryInfo)
じゃあこれは？
editCategorySchemaにはsuccessもerrorも定義されていない。safeParseの部分か？

ChatGTPに投げてみると
safeParse() は、メソッドではなく、返り値としてプロパティを持つオブジェクトを返します。
具体的には、返り値は以下のようなディスクリミネート・ユニオン型になっています:
成功例: { success: true, data: <parsed data> }
失敗例: { success: false, error: <ZodError instance> }

つまり、safeParse() 自体に success() や error() といったメソッドは存在せず、返されたオブジェクトのプロパティとして success（真偽値）や data／error を利用する形になります。

つまりsuccessとerrorもkey
