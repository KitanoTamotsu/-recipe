#### 開発メモ
[サンプル動画](https://user-images.githubusercontent.com/40127279/126054702-f0eb69c7-1ded-4978-ba3a-36a4bbd56076.mp4)
ワークフロー
<br><img width="600" src="https://user-images.githubusercontent.com/40127279/127757111-9c75bb4a-e944-4dfc-b251-c661a00f7122.png">

### 1.AlfredのJson出力フォーマットを10件以上で出力する
　特段のテクニックは不要です
<br>　Jsonフォーマットのitemsに10件以上、記述するだけです
<br>　Alfredでは9件しか出力できませんが、矢印キー（↑や↓）で表示をスクロールできます
<br>　最終行と開始行もシームレスにフォーカスが移動します
<br>　Alfredってほんとよくできています
### 2.htmlを解析する
　今回はレシピ検索を題材に選びました
<br>　『みんなのきょうの料理』というNHK料理番組のレシピサイトです
<br>　検索が細やかにできるのと、検索結果が20行なので、このサイトにしました
<br>
<br>　キーワード検索をしてみると、こんなURLになりした
<br>　https://www.kyounoryouri.jp/search/recipe?keyword=たまねぎ
<br>
<br>　次のページを見てみると
<br>　https://www.kyounoryouri.jp/search/recipe?keyword=たまねぎ&pg=2
<br>　
<br>　簡単ですね。
<br>　alfredの引数を2つにして、キーワードとページを入力する感じかな
<br>
<br>　ちなみに、初めのページは下記でもOKでした
<br>　https://www.kyounoryouri.jp/search/recipe?keyword=たまねぎ&pg=1
<br>　
<br>　ということで&pg=も全てくっつけても良さそうです
<br>　今回は、1ページ目は引数は1つだけ、2ページ目移行は2つの引数ありということにしたので
<br>　別々にURLを組み立てています
### 3.濁点、半濁点が検索されない事象を解消する
　テストをして、いきなり不具合にぶち当たりました
<br>　『玉子』のレシピは検索できますが、『たまご』や『タマゴ』ではできません
<br>　ひらがな、カタカナがNGなのかと思ったのですが、『チャーハン』や『ほうれんそう』は
<br>　検索できるようです
<br>　何種類もためしてみて、どうやら濁点『゛』、半濁点『゜』があると検索できないと
<br>　判別できました
<br>
<br>　ネットで調べてみると、文字コードの方式（NFCとNFD）に原因があるよう
<br>　一見に一文字に見えても、NFDは2文字を統合したコード、NFCは単一のコードとして
<br>　扱っているとのこと
<br>　iconvで簡単に変換できるとのことでやってみた
<br>
```
　iconv -f UTF-8-MAC -t UTF-8
```
<br>　こんなコードを入れたら、速解決
<br>　ん、ちょっと待てよ
<br>　Lesson4で%エンコードをするときに、iconvが使えなかったのでnkfをインストールしたの
<br>　ですが、使えたのですね。。。まぁいいか
<br>
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/127757130-5223039d-efdf-4bff-9264-49f6e6d1f59e.png">

#### 背景　
　Lesson14のRSSマニアのロジックの誤りで、AlfredのJSON出力フォーマットが9件以上でも
<br>　対応していることがわかったので、
<br>　検索結果が20件程度あるサイトを探して作ってみました
<br>　HTMLのパーズは簡単だったのですが、テストの際に、キーワードによって検索できない
<br>　不具合があり苦労しました
<br>　いままで気がつかなかったけど、他のワークフローでも同様の事象（濁音、半濁音問題）が
<br>　あったのかもしれませんね。まぁいいか
#### 取扱説明
### 機能:
　NHK『きょうの料理』の公式レシピサイトを検索
### インストール:
　1.[Alfredworkflow](https://github.com/KitanoTamotsu/recipe/releases/download/1.0/recipe.alfredworkflow.zip)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
### 使い方:
　キーワード　"レシピ"　＋　"食材・料理・講師名など" 
 <br>　検索結果表示後ブランクに続けてなんでも良い文字でページ制御
[トップページに戻る](https://kitanotamotsu.github.io/)

