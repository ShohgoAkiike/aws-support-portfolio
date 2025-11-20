# S3 アップロード制御に関する IAM 設計  

## ■ 問い合わせ概要

お客様より以下の確認依頼を受領：

- 検証環境で S3 へのファイルアップロードをデフォルト禁止にしたい
- ReadOnlyAccess＋IAM 制限ポリシーの挙動確認
- AdministratorAccess＋S3ReadOnlyAccess の組み合わせの可否を確認したい

---

## ■ サポートエンジニア側の回答（AWS公式ドキュメントに基づく）

### ① ReadOnlyAccess ＋ IAM 制限ポリシー

AWS公式のポリシー評価ロジック  
**「明示的 Deny ＞ Allow ＞ 暗黙的 Deny」**  
に基づき回答。

- ReadOnlyAccess と IAM 制限ポリシーでは  
  **S3 アップロードは暗黙的 Deny → アップロード不可**
- お客様の認識は正しいと確認

---

### ② AdministratorAccess ＋ S3ReadOnlyAccess

- AdministratorAccess に **明示的 Deny が無い場合、アップロードは許可される**
- S3ReadOnlyAccess は「追加の許可を増やす」だけで  
  **Admin の権限を制限するものではない**

---

### ③ S3 アップロード禁止を確実に行う方法

**推奨構成：**

- AdministratorAccess または PowerUserAccess  
- ＋ アップロード操作（s3:PutObject など）を **明示的 Deny** するカスタムポリシー

→ 明示的 Deny が最優先で評価されるため、アップロード禁止が確実。

---

### ④ Admin を使わせたくない場合の代替

- PowerUserAccess は IAM 管理権限を持たないため安全性が高い
- Admin の代替ロールとして公式ドキュメントでも有効

---

## ■ 最終結論（お客様側）

- 「PowerUserAccess＋S3アップロード禁止のカスタムポリシー」で対応する方針にてクローズ

---
## 📚 参考資料

### ■ IAM ポリシー関連

- **[1] ポリシーの評価論理**  
  https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_evaluation-logic.html

- **[2] PowerUserAccess**  
  https://docs.aws.amazon.com/ja_jp/aws-managed-policy/latest/reference/PowerUserAccess.html


### ■ S3 / アクセス権限関連

- **[1] AmazonS3ReadOnlyAccess**  
  https://docs.aws.amazon.com/ja_jp/aws-managed-policy/latest/reference/AmazonS3ReadOnlyAccess.html

- **[2] 明示的な拒否と暗黙的な拒否の違い**  
  https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_evaluation-logic_AccessPolicyLanguage_Interplay.html

