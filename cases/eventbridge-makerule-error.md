# EventBridge ルール作成エラーおよびカスタムイベントバス動作に関する問い合わせ 

## ■ お客様からの問い合わせ内容

お客様より、EventBridge にてワイルドカード（*）を含むルールを作成しようとした際、
以下エラーが発生しルールを作成できないとの相談を受領。

> The requested resource exceeds the maximum number of rules with wildcards allowed on an event bus.

また、カスタムイベントバスを作成し S3 のオブジェクト配置イベントを検知しようとしたが、
イベントが発火しないとの追加問い合わせを受領。

---

## ■ 私の対応内容

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

要件を満たすには
**デフォルトイベントバス → カスタムイベントバス** へのルールによる転送設定
を行う必要がある。

---

## ■ お客様への提案



---


## ■ 最終結論（お客様側）



---
## 📚 参考資料（AWS公式re:Post）

- **[1] EventBridge イベントバスのクォータ**  
  [https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-quota.html?#eb-limits](https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-quota.html?#eb-limits)

- **[2] AWS サービスからのイベント**
  [https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-events.html#eb-service-event](https://docs.aws.amazon.com/ja_jp/eventbridge/latest/userguide/eb-events.html#eb-service-event)

- **[3] EventBus**
  [https://docs.aws.amazon.com/ja_jp/eventbridge/latest/APIReference/API_EventBus.html](https://docs.aws.amazon.com/ja_jp/eventbridge/latest/APIReference/API_EventBus.html)
