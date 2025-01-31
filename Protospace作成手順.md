# 1. アプリケーションの新規作成手順 #
## 1-1. Railsアプリケーションの新規作成 ##
```
% cd ~/projects
% rails _7.1.0_ new protospace-テックマスターuser_id -d mysql
% cd protospace-”テックマスターuser_id”
```
## 1-2.データベースの作成 ##
database.ymlのencoding: utf8mb4をencoding: utf8へ修正後、
```
% rails db:create
```

# 　2.テーブルの設計 #
README.mdにテーブル設計を記述する。
## users テーブル

| Column             | Type   | Options                   |
| ------------------ | ------ | ------------------------- |
| email              | string | null: false, unique: true |
| encrypted_password | string | null: false               |
| name               | string | null: false               |
| profile            | text   | null: false               |
| occupation         | text   | null: false               |
| position           | text   | null: false               |

### Association

- has_many :prototypes
- has_many :comments

## prototypes テーブル

| Column     | Type       | Options                        |
| ---------- | ---------- | ------------------------------ |
| title      | string     | null: false                    |
| catch_copy | text       | null: false                    |
| concept    | text       | null: false                    |
| user       | references | null: false, foreign_key: true |

### Association

- belongs_to :user
- has_many :comments

## comments テーブル

| Column    | Type       | Options                        |
| --------- | ---------- | ------------------------------ |
| content   | text       | null: false                    |
| prototype | references | null: false, foreign_key: true |
| user      | references | null: false, foreign_key: true |

### Association

- belongs_to :user
- belongs_to :comment


# 3. #
prototypesコントローラーを作成した。
prototypesコントローラーにindexアクションを定義した（アクションの中には何も記述しない）。
viewsディレクトリの中の、prototypesコントローラーに対応するディレクトリにindex.html.erbを作成して、上記の記述をした。



# 4. #
ルートパスにアクセスしたときに、prototypesコントローラーのindexアクションを呼び出す記述をroutes.rbにした

# 5. #
- 既存のindex.html.erbを、配布済みのindex.html.erbに置き換えた
- 配布済みのstyle.cssを指示されたディレクトリに配置した
- 配布済みの画像を、指定されたディレクトリに配置した
- 既存のapplication.html.erbを、配布済みのapplication.html.erbに置き換えた
- サーバーを再起動して、ブラウザで表示を確かめた

# 6. Deviseを導入し、ユーザー用のモデルおよびテーブルを作成しましょう #
Gem Deviseを導入し、Userモデルを作成しましょう。モデルが作成できたら、usersテーブルを作成します。
作業チェック
- deviseをGemfileに記述し、bundle installを実行した
- rails g devise:installでdeviseのインストールをした
- rails g devise userでUserモデルを作成した
- マイグレーションファイルに、ユーザー名、プロフィール、所属、役職を追加した（メールアドレスとパスワードについてはデフォルトでマイグレーションファイルに記載されているため、追記の必要はない）
- rails db:migrateを実行し、Sequel Pro（windowsの方はDBeaver）でusersテーブルが存在することを確かめた

# 7. バリデーションを設定しましょう #
- Userモデルに各カラムのバリデーションを記述した（「emailとpasswordが空だと保存できない」というバリデーションは標準で用意されているため、記述する必要はない）


# 8. ビューファイルを適切なものに置き換えましょう #
- 配布済みのviews/deviseディレクトリを既存のviewsディレクトリに置き換えた

- deviseでログイン機能を実装すると、ログイン/サインアップ画面が自動的に生成されますがビューファイルとしては生成されません。
これは、deviseのGem内に存在するビューファイルを読み込んでいるためです。

deviseのビューファイルに変更を加えるためには、deviseのコマンドを利用して、ビューファイルを生成する必要があります。

deviseでログイン機能を実装すると、ログイン/サインアップ画面が自動的に生成されますがビューファイルとしては生成されません。
これは、deviseのGem内に存在するビューファイルを読み込んでいるためです。

deviseのビューファイルに変更を加えるためには、deviseのコマンドを利用して、ビューファイルを生成する必要があります。
```
% rails g devise:views
```

# 9. 新規登録ができるようにしましょう #
ユーザーの新規登録ができるように実装しましょう。usersテーブルに保存されていることを確認してください。


作業チェック
 ヘッダーの「新規登録」ボタンに適切なパスを記載した（devise/registrations#newに該当するパスを、rails routesを用いて確認する）
 registrations/new.html.erbのform_withのモデル名と新規登録機能へのパスを正しいものに修正した
 registrations/new.html.erbで「:hoge」と表記されている部分を、正しいものに修正した（PicTweetなどの新規登録ページも参考にする）
 application_controllerに、emailとpassword以外の値も保存できるように追記した（PicTweetなども参考にする）
 サーバーを再起動し、正しく新規登録ができることを確かめた
 Sequel Pro（Windowsの方はDBeaver）でusersテーブルに情報が保存されていることを確認した

# 10. ヘッダーの表示と、「こんにちは、〇〇さん」の表示がログイン状態か否かで変わるようにしましょう #
- ヘッダーの「ログイン」「新規登録」と、「ログアウト」「New Proto」を、ログインをしているときとしていないときで表示が変わるように条件分岐した
- ログインしているときは、トップページに「こんにちはユーザー名さん」が表示されるように条件分岐した
- 「こんにちはユーザー名さん」の「ユーザー名」の部分に、今ログインしているユーザーのユーザー名が表示されるようにした

# 11. ログイン・ログアウトができるようにしましょう #
- ヘッダーの「ログイン」ボタンに適切なパスを記載した（devise/sessions#newに該当するパスを、rails routesを用いて確認する）
- ヘッダーの「ログアウト」ボタンに適切なパスを記載した（devise/sessions#destroyに該当するパスを、rails routesを用いて確認する。HTTPメソッドの指定に注意。）
- sessions/new.html.erbのform_withのモデル名とログイン機能へのパスを正しいものに修正した
- sessions/new.html.erbで「:hoge」と表記されている部分を、正しいものに修正した（PicTweetなどのログインページも参考にする）
- サーバーを再起動し、ブラウザでログアウト/ログインができることを確認した
- 情報が正しくない、情報が欠けている場合は、新規登録・ログインができないことを確認した

# プロトタイプ情報投稿に関連する機能の実装 #

# Prototypeモデルおよびテーブルを作成しましょう #
- rails g model prototypeでPrototypeモデルを作成した
- マイグレーションファイルに、プロトタイプの名称、キャッチコピー、コンセプトのためのカラムを追加した
- マイグレーションファイルに、userを参照するための外部キーを記述した（references型を用いる）
- rails db:migrateを実行し、Sequel Pro（windowsの方はDBeaver）でprototypesテーブルが存在することを確かめた

# アソシエーションを記述しましょう #
- Prototypeモデルにアソシエーションを記述した
- Userモデルにアソシエーション記述した

# Active Storageを導入しましょう #
Gemを導入して、ActiveStorageの導入を行います。そして、Prototypeモデルにimageカラムとのアソシエーションの設定をしましょう。
- mini_magickとimage_processingのGemをGemfileに記述し、bundle installを実行した（参考カリキュラムのとおり、image_processingについてはバージョンの指定をする）
- rails active_storage:installでActive Storageを導入した
- rails db:migrateを実行した
- Prototypeモデルに、has_one_attachedを使用して imageカラムとのアソシエーションを記述した

# バリデーションを設定しましょう #
- Prototypeモデルに、プロトタイプの名称、キャッチコピー、コンセプト、画像に関するバリデーションを記述した

# アクションとルーティングを設定しましょう
今回の投稿機能に必要な、アクションとルーティングを設定しましょう。
- prototypesコントローラーにnewアクションとcreateアクションを設定した（まだアクション内の処理は書かない）- resourcesを用いて、上記で設定したnewアクションとcreateアクションに対するルーティングをroutes.rbに記述した
- rails routesを実行して、ルーティングが正しく設定できていることを確かめた

# 適切なビューファイルを設定しましょう #
すでにダウンロード済みのビューファイルから、新規投稿に関するビューファイルと投稿フォームに関する部分テンプレートを選択し、配置しましょう。
- views/prototypesの中に、配布済みのnew.html.erbと_form.html.erbを配置した

# 投稿機能を実装しましょう #
投稿機能を実装しましょう。このとき、必要な値が入力されておらずバリデーションによって保存が出来ない場合の処理についても実装します。具体的には、renderを用いて投稿ページのビューファイルを表示するようにします。注意点として、バリデーションによって保存ができなかった場合でも、画像以外の入力項目は消えないように実装しましょう。

- ヘッダーの「New Proto」ボタンから、新規投稿ページに遷移するようにパスを設定した（rails routesを用いて確認する）
- newアクションにインスタンス変数@prototypeを定義し、Prototypeモデルの新規オブジェクトを代入した
- new.html.erbから部分テンプレートである、_form.html.erbを呼び出す記述をした
- _form.html.erbのform_withのモデル名を正しいものに修正した
- _form.html.erbで「:hoge」と記載されている部分を、正しいものに修正した
- prototypesコントローラーのprivateメソッドにストロングパラメーターをセットし、特定の値のみを受け付けるようにした。且つ、user_idもmergeした
- createアクションにデータ保存のための記述をし、保存されたときはルートパスに戻るような記述をした
- createアクションに、データが保存されなかったときは新規投稿ページへ戻るようrenderを用いて記述した
- バリデーションによって保存ができず投稿ページへ戻ってきた場合でも、入力済みの項目（画像以外）は消えないことを確認した
- サーバーを再起動し、ブラウザで正しく動くか確認した
- Sequel Pro（Windowsの方はDBeaver）を確認して、正しく保存ができているか確認した

# 投稿したプロトタイプがトップページで表示されるようにしましょう #
テーブルに保存されているすべてのプロトタイプが、トップページに表示されるようにしましょう。各プロトタイプのリンクは、仮置きのままで問題ありません。

- 各プロトタイプを表示するための部分テンプレート_prototype.html.erbを、views/prototypesの中に配置した
- indexアクションに、インスタンス変数@prototypesを定義し、すべてのプロトタイプの情報を代入した
- index.html.erbから_prototype.html.erbを呼び出し、プロトタイプ毎に、画像・プロトタイプ名・キャッチコピー・投稿者の名前を表示できるようにした（renderメソッドにcollectionオプションを用いて実装する）
- 正しくプロトタイプの表示ができるように、_prototype.html.erbを編集した（ただし仮置きのリンクroot_pathはそのままで良い）
- ブラウザで正しく表示されるか確認した

# プロトタイプの詳細ページを実装しよう #
トップページのプロトタイプをクリックすると、以下のように詳細ページが表示されるようにしましょう。「編集」「削除」については、そのプロトタイプを投稿したユーザー以外には表示されません。

## アクションとルーティングを設定しましょう ##
今回の詳細ページ機能に必要な、アクションとルーティングを設定しましょう。
- prototypesコントローラーにshowアクションを設定した（まだアクション内の処理は書かない）
- resourcesを用いて、上記で設定したshowアクションに対するルーティングをroutes.rbに記述した
- rails routesを実行して、ルーティングが正しく設定できていることを確かめた

# 適切なビューファイルを設置しましょう #
すでにダウンロード済みのビューファイルから、詳細ページに関するビューファイルを選択し、配置しましょう。
- views/prototypesの中に、配布済みのshow.html.erbを配置した

# 詳細ページで投稿の情報が表示されるようにしましょう #
遷移先の詳細ページで、そのプロトタイプの情報が過不足無く表示されるようにしましょう。

- showアクションにインスタンス変数@prototypeを定義した。且つ、Pathパラメータで送信されるID値で、Prototypeモデルの特定のオブジェクトを取得するように記述し、それを@prototypeに代入した
- show.html.erbにおいて、プロトタイプの「プロトタイプ名」「投稿者」「画像」「キャッチコピー」「コンセプト」が表示されるように記述を変更した
- 詳細ページに遷移し、プロトタイプの「プロトタイプ名」「投稿者」「画像」「キャッチコピー」「コンセプト」が正しく表示されることを確認した

# 投稿したユーザーだけに「編集」「削除」のボタンが表示されるようにしましょう #
投稿者だけに「編集」「削除」のボタンが表示されるようにしましょう。この時、リンク先は現状の仮置きのままで問題ありません。

- ログインしているユーザーがそのプロトタイプの投稿者であるときは、「編集」「削除」のボタンが表示されるように条件分岐した（ボタンのリンク先は仮置きのroot_pathのままでよい）
- ブラウザで正しく動くか確認した

# プロトタイプ情報の編集機能を実装しよう #
詳細ページからプロトタイプ編集ページに遷移し、正しく編集できると詳細ページに戻るように実装することをゴールとします。空の入力欄がある場合は、編集できずにそのページに留まるようにします。

## アクションとルーティングを設定しましょう ##
- prototypesコントローラーにeditアクションとupdateアクションを設定した（まだアクション内の処理は書かない）
- resourcesを用いて、上記で設定したeditアクションとupdateアクションに対するルーティングをroutes.rbに記述した
- rails routesを実行して、ルーティングが正しく設定できていることを確かめた

# 適切なビューファイルを設定しましょう #
すでにダウンロード済みのビューファイルから、編集機能に関するビューファイルを選択し、配置しましょう。
- views/prototypesの中に、配布済みのedit.html.erbを配置した

# 詳細ページから編集ページに遷移できるようにしましょう #
- show.html.erbの「編集する」ボタンから、編集ページに遷移するようにパスを設定した（パスはrails routesを用いて確認する）

