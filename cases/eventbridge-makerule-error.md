# EventBridge ルール作成エラーおよびカスタムイベントバス動作に関する問い合わせ 

## ■ お客様からの問い合わせ内容

お客様より、EventBridge にてワイルドカード（*）を含むルールを作成しようとした際、
以下エラーが発生しルールを作成できないとの相談を受領。

The requested resource exceeds the maximum number of rules with wildcards allowed on an event bus.

また、カスタムイベントバスを作成し S3 のオブジェクト配置イベントを検知しようとしたが、
イベントが発火しないとの追加問い合わせを受領。

---

## ■ 私の回答（AWS公式ドキュメントを確認のうえ回答）

### ① 事象の正確な理解と誤認の解消

削除時の警告メッセージの意味を技術的に解説：

警告文は
“新規 EC2 起動時に VPC/サブネットを明示的に指定しない場合、起動できなくなる”
という意味であり、
既存の他VPCのインスタンスやリソースが利用できなくなるわけではない と説明。

    公式re:Post引用（[1] より）
> AWS アカウントを作成すると、アカウント内の各 AWS リージョンにデフォルトの VPC が配置されます。各リージョンにおいて、複数のデフォルト VPC を配置することはできません
> また、削除済みのデフォルト VPC を復元することはできません。

---

## ■ お客様への提案



---


## ■ 最終結論（お客様側）



---
## 📚 参考資料（AWS公式re:Post）

- **[1] Amazon VPC 内のデフォルト VPC を作成、削除、または復元する方法を教えてください。**  
  [https://repost.aws/ja/knowledge-center/deleted-default-vpc](https://repost.aws/ja/knowledge-center/deleted-default-vpc)
