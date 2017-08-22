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
