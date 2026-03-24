# ALB配下のEC2環境で発生したHTTP 408エラーに関する問い合わせ  

## ■ お客様からの問い合わせ内容
お客様よりALB＋EC2構成のWebサイトにて「HTTP 408 Request Timeout」が発生。  
原因調査および再発防止策の問い合わせを受領。

---

## ■ 調査内容

### ① CloudWatch メトリクス確認

対象インスタンスのステータスチェックを調査し、
インスタンスステータスチェックの失敗を検知していることを確認。

システムステータスチェックの失敗は検知していないことからAWS 基盤側の問題ではないと判断。

---

### ② お客様へ情報提供依頼

提供内容：

- 過去にも同様事象あり
- 使用しているWebサーバ：Apache + Tomcat（apache-tomcat-9.0.71）
- ALBアクセスログ（該当時間帯）
- EC2ログ一式

### ③ ALBアクセスログ、Webサーバログ分析

- request_processing_time：0.000
- target_processing_time：100秒以上

    公式ドキュメント引用（[1] より）
> request_processing_time　ロードバランサーがリクエストを受け取った時点からターゲットにリクエストを送信するまでの合計経過時間
> 
> target_processing_time　ロードバランサーがターゲットにリクエストを送信した時点から、そのターゲットが応答ヘッダーの送信を開始した時点までの合計経過時間

→ ALBは即時転送  
→ ターゲット（EC2）の応答遅延が原因

- エラー：

```text
AH01030: ajp_ilink_receive() can't receive header
```
→ Apache ↔ Tomcat間のAJP通信タイムアウトと推測

---

### ④ AWSサポートへエスカレーション

上記調査結果について妥当性確認および見落としの有無を確認するため、
AWSサポートへエスカレーションを実施。以下回答受領。

- Tomcatが応答しなくなったことが原因である可能性が高い
- 改めて当該時間帯のAWS基盤側の障害について確認したが問題なし
- インスタンス内部（OS / ミドルウェア / アプリケーション）の問題と推測される

---

## ■ 回答（お客様への推奨対応）

- Tomcatログの詳細調査
- アプリケーション側の処理遅延の確認
- 必要に応じて以下の設定見直し
  - Tomcatのスレッド設定
  - タイムアウト設定

---

## ■ 最終結論（お客様側）

回答内容についてご納得いただけたとのことで問い合わせクローズ。

---
## 📚 参考資料（AWS公式ドキュメント）

- **[1] Application Load Balancer のアクセスログ**  
  [https://docs.aws.amazon.com/ja_jp/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-log-entry-format](https://docs.aws.amazon.com/ja_jp/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-log-entry-format)
