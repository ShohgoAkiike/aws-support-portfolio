# AWS Technical Support Portfolio  

AWSリセラー企業にてテクニカルサポートを担当。  
年間◯◯件以上の問い合わせ対応を行い、EC2・VPC・IAM・S3・CloudWatchなどを中心に技術サポートを実施。

---

## 🛠 技術スタック（対応領域）
- EC2：起動トラブル調査、ネットワーク疎通検証
- VPC：SG/NACL調査、ルートテーブル分析、Reachability Analyzer利用
- IAM：ポリシー解析、認証エラーの切り分け
- S3：アクセス拒否、バケットポリシー分析
- CloudWatch：ログ調査、メトリクス分析

---

## 📚 ケーススタディ（匿名化済み）
### Case1：S3 のアップロード／ダウンロード権限の制御（IAM ポリシー検証）  

**概要**  
お客様より以下の問い合わせを受領：  
- 検証環境で S3 へのファイルアップロードをデフォルト禁止にしたい  
- ReadOnlyAccess やカスタム IAM ポリシーの組み合わせによる挙動を確認したい  
- AdministratorAccess＋S3ReadOnlyAccess の動作を確認したい  

AWS公式ドキュメントの「IAM ポリシー評価ロジック」を確認したうえで調査・回答を実施した。

---

## 📝 CUVIC on AWS 側の回答・調査結果

### **① ReadOnlyAccess ＋ IAM 制限ポリシーの挙動**  
IAM の評価ロジック（**明示的 Deny ＞ Allow ＞ 暗黙的 Deny**）に基づき回答：  

- ReadOnlyAccess と IAM 制限ポリシーの組み合わせでは、  
  **S3 アップロードは暗黙的 Deny となり禁止される**  
- お客様の認識どおりの挙動で問題なし

---

### **② AdministratorAccess ＋ S3ReadOnlyAccess の挙動**  
- AdministratorAccess に明示的 Deny が無い場合、  
  **S3 アップロードは許可される**  
- S3ReadOnlyAccess は追加的な Allow を制限するものではなく、  
  **Admin 権限が優先されるためアップロード可能**

---

### **③ S3 アップロード禁止を実現する正しい構成（推奨）**  
AWS公式仕様より、以下が最適と判断：  

- AdministratorAccess **または** PowerUserAccess に対して  
  **s3:PutObject などのアップロード操作を明示的に Deny するカスタムポリシーを付与する**  
- 明示的 Deny が最優先評価されるため、  
  **確実にアップロード禁止が可能**

---

### **④ Admin を使わせたくない場合の代替案**  
- **PowerUserAccess** は IAM 管理操作権限を持たないため安全性が高い  
- 管理者権限を与えたくないケースでは、  
  **PowerUserAccess + カスタム Deny ポリシー** が最適

---

## ✅ **結論（お客様側判断）**
- 提案内容を理解し、  
  **PowerUserAccess + S3 アップロード禁止ポリシー**  
  を採用する方針で決定  
- 問い合わせはクローズとなった

---

---

## 📄 添付資料
- `ポートフォリオ_秋池.pptx`（ポートフォリオ資料）
