# RDS MySQL マイナーバージョン自動アップグレードに関する問い合わせ

## ■ お客様からの問い合わせ内容  
RDS for MySQL のサポート終了対象マイナーバージョン（8.0.36〜8.0.32）において、  
「自動マイナーバージョンアップがいつ適用されるか」  
「自動アップグレード設定の変更が停止を伴うか」  
についてお客様から問い合わせ受領。

---

## ■ 調査内容  
- AWS公式ドキュメントにて以下を確認  
  - 自動マイナーバージョンアップの実施タイミング
  - マイナーバージョン自動アップグレード設定の変更時の挙動（停止有無）  
- 過去事例として、直近メンテナンスウィンドウで必ず適用されず、数日〜1週間の遅延が発生するケースがあることを確認。

---

## ■ 回答 （AWS公式ドキュメントに基づく）
### ● 自動マイナーバージョンアップの実行タイミング  
- 原則「直近のメンテナンスウィンドウ」で実施される。  
- ただし AWS 内部処理に依存するため **必ずそのタイミングで実行される保証はない**（遅延する場合あり）。

 公式ドキュメント引用（[1] より）
> DB インスタンスの作成または変更時に次の設定を指定すると、DB インスタンスを自動的にアップグレードできます。

> ・[マイナーバージョンの自動アップグレード] の設定は有効です。

> ・バックアップ保持期間の設定は 0 より大きいです。

> Amazon RDS でマイナーバージョンのテストと承認が完了すると、メンテナンスウィンドウの間にマイナーバージョンアップグレードが自動的に行われます。

 公式ドキュメント引用（[2] より）
> RDS では、メンテナンスウィンドウ中にアップグレードが自動で実行されるようスケジュールされます。

### ● 設定変更時の影響  
- 「自動マイナーバージョンアップ：有効／無効」の変更では  
  **RDS の停止は発生しない。アクセス影響もなし。**

  公式ドキュメント引用（[3] より）
> マイナーバージョン自動アップグレード
> 変更はただちに発生します。この設定は、[Apply immediately (すぐに適用)] 設定を無視します。
> この変更時にダウンタイムは発生しません。

### ● 推奨事項  
- 予期せぬダウンタイムを避けるため  
  **自動アップグレードは無効化し、計画的に手動アップグレードを実施する** ことを推奨。

---

## ■ 最終結論（お客様側） 
- お客様側にて自動アップグレードを無効化にした上で任意のタイミングにてアップグレードを実施する旨の返信あり、問い合わせクローズ。

---

## 📚 参考資料（AWS公式ドキュメント）

- **[1] 自動マイナーバージョンアップグレードの仕組み**  
  [https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Upgrading.html#USER_UpgradeDBInstance.Upgrading.scheduled](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Upgrading.html#USER_UpgradeDBInstance.Upgrading.scheduled)

- **[2] RDS for MySQL のマイナーバージョンの自動アップグレード**  
  [https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.MySQL.Minor.html](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.MySQL.Minor.html)

- **[3] DB インスタンスの設定**  
  [https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_ModifyInstance.Settings.html](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_ModifyInstance.Settings.html)
