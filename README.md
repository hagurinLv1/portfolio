# Spark　〜一瞬で繋がるグループメッセンジャーアプリ〜
Spark は VOYAGE GROUP のインターンシップにて作成したグループメッセンジャーアプリです。

## 概要
Spark は勉強会、インターンシップ、同窓会など、現実世界で会っているのに連絡先を知らない人同士が気軽に繋がることのできるアプリです。  
通常のメッセンジャー機能に加え、現在地周辺で同時にSpark（タップor端末シェイク）したユーザをグルーピングする機能をもたせたことで、多人数でも気軽に連絡先を交換することが可能になりました。

## 画面遷移

## API仕様
```
###【スプラッシュ画面】  
・セッションによるログイン状態チェック  
　↑　GET /api/check_login  
　↓　status code : 200 | 400 | 500  
  
・UUIDによるログイン（セッション関連生成）  
　↑　POST /api/login  
　　　”UUID” => “(端末の固有ID)”  
　↓　status code : 200 | 400 | 500  
  
###【ログイン画面】  
・ユーザ新規登録（ログインせずに始める機能）  
　↑　POST /api/register  
　　　”UUID” => “(端末の固有ID)”, “name” => “任意のユーザ名”  
　↓　status code 200 | 400 | 500  
  
・SNSログイン（未登録の場合は新規登録込み）  
　↑　POST /api/login  
　　”sns” => “(facebook | twitter)”, “sns_id” => “(snsでユーザをユニークに決める値)”, “user_name => “(snsのユーザ名)”, ”profile_img” => “”  
　↓　  
　　[{“user_id”: “(ユーザID int)”}]  
   
###【グループ一覧画面】  
　↑　GET /api/groups  
　↓　status code 200 | 400 | 500  
　　 [{”group_id” : “(グループを特定するユニークな値 int)”, “img” : “(画像のデータを何らかの形で)”, “group_name” : “(グループの名前)”, “latest_message : (最新コメントテキスト varchar)”, “latest_message_time” : “(時間 datetime)”}]  
    
###【guest/host選択画面】  
　→ユーザがguestなのかhostなのかの値を保持して【スパーク画面】に遷移  
  
###【スパーク画面】  
・guestの処理  
　↑　POST /api/sparking   
　　　“latitude” => (緯度), “longitude”=>(経度)  
　↓　status code 200 | 400 | 500  
   
・hostの処理  
　→　緯度経度を保持して【host用メンバ管理画面】に遷移  
  
###【host用メンバ管理画面】  
・メンバ一覧更新機能（hostが自分のデータを元に検索）  
　↑　GET /api/guests  
　　　latitude = “(緯度)”, longitude = “(経度)”	  
　↓　status code 200 | 400 | 500  
　　　[{“user_id” : “(ユーザID)”, “user_name” : “(ユーザ名)”, “user_img”:”(ユーザの画像)”},....]  
  
・グループを作る機能（ボタン）  
　↑　POST /api/grouping  
　　　user_ids = “グループ化するユーザIDの配列”, group_name = “グループ名”  
　↓　status code 200 | 400 | 500  
　　　[{“group_id” : (作られたグループのID)}]  
  
・やりなおす（ボタン）  
　→　【スパーク画面】  
  
###【guest用待機画面】  
　↑　GET /api/state_grouping   
　↓　status code 200 | 400 | 500  
　　　[{“group_id” : “(新しく作成されたグループのID int)”}]  
   
###【グループトーク画面】  
・タイムライン自動更新機能  
　↑　GET /api/messages  
　　　“group_id” => (グループID int), “updated_datetime” => (最終更新日時)  
　↓　status code 200 | 400 | 500  
　　　[{“body” : “(投稿内容)”, “user_id” : “(ユーザID int)”, “user_name” : “(ユーザ名 string)”, “profile_img” : “(プロフ画像)”, “updated_at“ : “(投稿された時間)”},.....]  
  
・メッセージ投稿機能（ボタン）  
　↑  POST /api/message  
　　　“body” => “(投稿内容)”, “group_id”=>”(グループID int)”  
　↓　status code 200 | 400  | 500  
  
・SNS情報投稿機能  
　未定  
  
###【プロフィール画面】  
　↑　GET /api/profile  
　↓　status code 200 | 400 | 500  
　　　[{“user_name” : “(ユーザ名)”, “profile_img” : “(プロフ画像)”}]  
  
###【プロフィール編集画面】  
　↑　POST /api/profile  
　　　”user_id =>”, “user_name => “(snsのユーザ名)”, ”profile_img” => “”  
　↓　status code 200 | 400 | 500 
```

## 担当
４人チームで開発し、全員で企画・画面遷移・API設計・DB設計を行いました。  
その後、フロントエンド２人・バックエンド２人に分業しました。  
私はバックエンドを担当し、PHPのフレームワークであるSlimを用いて、主にグルーピング・トーク用のAPIを開発しました。
