# EventBridge ルール作成エラーおよびカスタムイベントバス動作に関する問い合わせ 

## ■ お客様からの問い合わせ内容

お客様より、EventBridge にてワイルドカード（*）を含むルールを作成しようとした際、
以下エラーが発生しルールを作成できないとの相談を受領。

> The requested resource exceeds the maximum number of rules with wildcards allowed on an event bus.

また、カスタムイベントバスを作成し S3 のオブジェクト配置イベントを検知しようとしたが、
イベントが発火しないとの追加問い合わせを受領。

---

## ■ 調査内容

### ① ワイルドカードルール作成エラー

- AWS公式ドキュメントより、イベントバスあたりワイルドカードを含むルールは最大30個 までという上限を確認。
- お客様環境では 40以上のルール が存在し、上限超過が原因でルール作成に失敗していた。
- 本クォータは 上限緩和不可 の仕様であることを公式ドキュメントで確認。

   公式ドキュメント引用（[1] より）
> ワイルドカードを含むルール
> サポートされている各リージョン: イベントバスあたり 30 ルール

> ワイルドカードを含むイベントフィルターを含めることができる、アカウントごとのイベントバスあたりの最大ルール数。このクォータは変更できません。


### ② カスタムイベントバスで S3 イベントが検知されない
AWS公式ドキュメントを参照し、
- AWSサービスが発行するイベントは必ずデフォルトイベントバスへ送信される
- カスタムイベントバスは 独自アプリケーションやカスタムイベントのみ受信可能である仕様を確認。

そのため、S3 の PutObject イベントを 直接カスタムイベントバスに送信することは不可 であることを確認。

 公式ドキュメント引用（[2] より）
> 多くの AWS サービスは、EventBridge が受け取るイベントを生成します。
> アカウントの AWS サービスが EventBridge にイベントを送信すると、アカウントのデフォルトのイベントバスに送信されます。

 公式ドキュメント引用（[3] より）
> Your account's default event bus receives events from AWS services.
> A custom event bus can receive events from your custom applications and services.

以下日本語訳
> アカウントのデフォルトのイベントバスは、AWS のサービスからイベントを受信します。
> カスタムイベントバスは、カスタムアプリケーションやサービスからイベントを受信できます。

要件を満たすには
**デフォルトイベントバス → カスタムイベントバス** へのルールによる転送設定
を行う必要がある。

---
## ■ 回答（AWS公式ドキュメントに基づく）

- ワイルドカードルールの上限超過であることを説明し、複数のイベントバスへルールを分散する案 を提案。
- S3 イベントがカスタムイベントバスで検知されない件については、仕様によりデフォルトイベントバスにしか送信されないため、転送ルールの設定が必要 と回答。

---

## ■ 最終結論（お客様側）

お客様側にて仕様を理解いただき、デフォルトイベントバス → カスタムイベントバスの転送設定を検討する との返信あり。
問い合わせクローズ。

---
## 📚 参考資料（AWS公式ドキュメント）

- **[1] EventBridge イベントバスのクォータ**  
  [https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-quota.html?#eb-limits](https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-quota.html?#eb-limits)

- **[2] AWS サービスからのイベント**  
  [https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-events.html#eb-service-event](https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-events.html#eb-service-event)

- **[3] EventBus**  
  [https://docs.aws.amazon.com/ja_jp/eventbridge/latest/APIReference/API_EventBus.html](https://docs.aws.amazon.com/ja_jp/eventbridge/latest/APIReference/API_EventBus.html)

