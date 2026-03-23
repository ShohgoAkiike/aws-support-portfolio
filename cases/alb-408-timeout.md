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

#### ③ ALBアクセスログ、Webサーバログ分析

- request_processing_time：0.000
- target_processing_time：100秒以上

→ ALBは即時転送  
→ ターゲット（EC2）の応答遅延が原因

- エラー：

```text
AH01030: ajp_ilink_receive() can't receive header
```
→ Apache ↔ Tomcat間のAJP通信タイムアウトと推測

---

### ④ AWSサポートへエスカレーション

上記の調査について認識齟齬ないか、ほかに原因がないか確認
⇒認識齟齬ない旨の回答あり。
　インスタンス内部の動作についてはAWS側ではわかりかねるため、
　お客様にてTomcatのログやシステムログを確認する旨の回答あり

---

## ■ 回答

上記を回答実施

---

## ■ 最終結論（お客様側）

回答内容についてご納得いただけたとのことで問い合わせクローズ。

---
## 📚 参考資料（AWS公式ドキュメント）

- **[1] Application Load Balancer のアクセスログ**  
  [https://docs.aws.amazon.com/ja_jp/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-log-entry-format](https://docs.aws.amazon.com/ja_jp/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-log-entry-format)
