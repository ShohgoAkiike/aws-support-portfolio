# Direct Connect 利用料発生タイミングに関する問い合わせ  

## ■ お客様からの問い合わせ内容

お客様より、AWS Direct Connect（DX）の利用料がいつから発生するのかについて質問あり。
特に、

- 接続（ホスト型接続）承諾時か
- VIF 作成時か
- BGP ピアが UP した時か

どの時点で課金が開始されるのかを確認したい、との内容。

---

## ■ 調査内容

### ① CloudWatch メトリクス確認・障害分析

対象インスタンスのステータスチェックを調査し、
StatusCheckFailed_System が 10:18〜10:34 の間で検知されていることを確認。
一時的な AWS 基盤側の問題（物理ホストのハードウェア/ネットワーク/電源関連問題）が要因と判断。

---

### ② 事象の性質説明とリスク評価

当該事象は自動復旧により正常化済み
10:34 以降の再発はなし
AWS基盤障害の詳細はAWS内部仕様で非公開である旨を説明

---

### ③ メンテナンス通知との関連性調査

お客様より当日届いた AWSからの通知「EC2 メンテナンス通知」について、以下を切り分けて説明：

通知の原因・・・物理ホストの劣化検知による計画メンテナンス
今回の疎通断・・・AWS基盤の一時的障害によるステータスチェック失敗
→ 性質は異なるが、同一ホスト上の劣化が今回の事象に影響した可能性はあると整合性を示し説明。

---

### ④ お客様への推奨対応

一時的障害の場合は通常継続利用で問題なし
ただし再発懸念がある場合は
インスタンスの停止・起動（＝新しい物理ホストへ移動）を推奨

  公式ドキュメント引用（[1] より）
> ユーザーがインスタンスを停止すると、インスタンスはシャットダウンされます。
> インスタンスを起動すると、通常、インスタンスは基盤となる新しいホストコンピュータに移行され、新しいパブリック IPv4 アドレスが割り当てられます。

---

## ■ 回答（AWS公式ドキュメントに基づく）

AWS基盤側の障害である旨回答。また再発時の懸念についてはインスタンスの停止・起動にて新しい物理ホストへ移動することを提案

---

## ■ 最終結論（お客様側）

回答内容についてご納得いただけたとのことで問い合わせクローズ。

---
## 📚 参考資料（AWS公式ドキュメント）

- **[1] AWS Direct Connect の料金**  
  [https://aws.amazon.com/jp/directconnect/pricing/](https://aws.amazon.com/jp/directconnect/pricing/)

- **[2] AWS 料金見積りツール**
  [https://calculator.aws/#/](https://calculator.aws/#/)

- **[3] AWS Direct Connect のよくある質問**  
  [https://aws.amazon.com/jp/directconnect/faqs/](https://aws.amazon.com/jp/directconnect/faqs/)

- **[4] AWS Direct Connect ホスト接続を受け入れる**  
  [https://docs.aws.amazon.com/ja_jp/directconnect/latest/UserGuide/accept-hosted-connection.html](https://docs.aws.amazon.com/ja_jp/directconnect/latest/UserGuide/accept-hosted-connection.html)
