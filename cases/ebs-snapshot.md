# EBS スナップショット／ボリューム置換に関する問い合わせ  

## ■ お客様からの問い合わせ内容

EBS ボリューム拡張にあたり、以下を確認したいと相談を受領：

- スナップショットの取得方法
- 復元手順（スナップショット→ボリューム作成→置換）
- 実施時の注意点

想定されていた作業フロー：

1. EBS 管理画面からスナップショット作成
2. スナップショットから新規ボリュームを作成
3. 作成したボリュームをインスタンスへアタッチ  
4. 旧ボリュームをデタッチ  
5. OS 側で認識と設定の調整

---

## ■ 私の回答（AWS公式ドキュメントを確認のうえ回答）

### ① スナップショット作成時の注意点

- スナップショットは **非同期処理** → 完了まで pending が続く
- 含まれるデータは **作成リクエスト時点の状態**
- 一貫性のためには **書き込み停止が推奨**
- 書き込み停止が難しい場合  
  → ボリュームを一時アンマウントして作成する方法も公式で推奨

  公式ドキュメント引用（[1] より）
> Amazon EC2 のルートデバイスとして機能するボリュームのスナップショットを作成する場合、
> スナップショットを取る前にインスタンスを停止することをお勧めします。

---

### ② ルートボリュームを扱う場合

- ルートボリュームのスナップショットは **インスタンス停止後が推奨**
- ルートボリューム置換時は  
  **インスタンスが running 状態で作業され、自動再起動が発生**

    公式ドキュメント引用（[2] より）
> インスタンスは running の状態である必要があります。

> インスタンスは、プロセス中に自動的に再起動されます。
> メモリ (RAM) の内容は再起動中に消去されます。

---

### ③ ボリューム復元（置換）時の注意点

- EBS ボリュームは **AZ 固有リソース**  
  → インスタンスと同じ AZ を指定しないとアタッチ不可
- スナップショットの設定内容は基本引き継がれるが  
  **AZ のみ手動選択が必要**

    公式ドキュメント引用（[3] より）
> インスタンスと同じアベイラビリティーゾーンに新しい EBS ボリュームを作成する必要があります。
> ボリュームは、同じアベイラビリティーゾーンのインスタンスにアタッチする必要があります。

---

## ■ 結論（お客様側）

回答内容をもとに作業イメージを明確にし、  
社内作業手順の策定に進むとのことで問い合わせクローズ。

---
## 📚 参考資料（AWS公式ドキュメント）

- **[1] Amazon EBS スナップショットの作成**  
  [https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_evaluation-logic.html](https://docs.aws.amazon.com/ja_jp/ebs/latest/userguide/ebs-creating-snapshot.html)

- **[2] ルートボリュームを置き換える**  
  [https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/reference_policies_evaluation-logic_AccessPolicyLanguage_Interplay.html](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/replace-root.html#replace)

- **[3] スナップショットを使用した Amazon EBS ボリュームの置き換え**  
  [https://docs.aws.amazon.com/ja_jp/aws-managed-policy/latest/reference/PowerUserAccess.html](https://docs.aws.amazon.com/ja_jp/ebs/latest/userguide/ebs-restoring-volume.html)
