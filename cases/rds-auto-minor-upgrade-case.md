# RDS MySQL マイナーバージョン自動アップグレードに関する問い合わせ

## ■ お客様からの問い合わせ内容  
RDS for MySQL のサポート終了対象マイナーバージョン（8.0.36〜8.0.32）において、  
「自動マイナーバージョンアップがいつ適用されるか」  
「自動アップグレード設定の変更が停止を伴うか」  
についてお客様から確認依頼があった。

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

### ● 設定変更時の影響  
- 「自動マイナーバージョンアップ：有効／無効」の変更では  
  **RDS の停止は発生しない。アクセス影響もなし。**

### ● 推奨事項  
- 予期せぬダウンタイムを避けるため  
  **自動アップグレードは無効化し、計画的に手動アップグレードを実施する** ことを推奨。

---

## ■ 最終対応  
- お客様側にて自動アップグレードを無効化する方針の確認が取れ、  
  本問い合わせはクローズとなった。

---

## 📚 参考資料（AWS公式ドキュメント）

- **[1] 自動マイナーバージョンアップグレードの仕組み**  
  [https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Upgrading.html#USER_UpgradeDBInstance.Upgrading.scheduled](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Upgrading.html#USER_UpgradeDBInstance.Upgrading.scheduled)

- **[2] RDS for MySQL のマイナーバージョンの自動アップグレード**  
  [https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.MySQL.Minor.html](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.MySQL.Minor.html)

- **[3] DB インスタンスの設定**  
  [https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_ModifyInstance.Settings.html](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/USER_ModifyInstance.Settings.html)
