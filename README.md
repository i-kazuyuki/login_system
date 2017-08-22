# README

* Ruby version ruby 2.4.0/Rails 5.1.3

gemを利用しないでログインシステムを構築した例です。

```
Userテーブル
name
email
password_digest
```

## Userモデルのバリデーションなど

`presence`
ユーザがデータベースに保存される前にname,emailが空でないことを確認。

`length`
入力文字数の制限

`format`
メールアドレスのフォーマットを検証

`uniqueness`
メールアドレスにユニーク制約を設定します。
しかし、このモデルのバリデーションだけでは、データベースレベルでのユニークである保証はないためemailのカラムにインデックスを追加する。

`before_save { email.downcase! }`
ユーザーをデータベースに保存する前にemail属性を強制的に小文字に変換する。


## パスワードのハッシュ化
パスワードのハッシュ化は`has_source_passwordメソッド`を使用する。

しかし `has_source_passwordメソッド` を使用する際は`password_digest`というカラムと`bcrypt`というGemが必要となる。
bcryptについての説明はここでは割愛します。


## sessionsコントローラについて
sessionsコントローラでログインとログアウトの機能を実装する。

```
newアクション :ログインページを出力する
createアクション :セッションを作成
destroyアクション :セッションを破棄
```

### newアクション
sessionにはSessionモデルはなく、そのため`@user`のようなインスタンス変数に相当するものもありません。
そのため、form_forヘルパーに追加の情報を独自に渡す必要があります。

```ruby
<%= form_for(:session, url: sessions_path) do |f| %>
```

### createアクション
パスワードとメールアドレスの組み合わせが有効かどうかを判定する。
有効であれば`sessionメソッド`を利用してログインし、有効でなければ再入力する。


### destroyアクション
セッションからユーザーIDを削除する処理を行う。

以上で簡単なログイン機能が実現できます。
