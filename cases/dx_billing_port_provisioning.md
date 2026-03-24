# Direct Connect 利用料発生タイミングに関する問い合わせ  

## ■ お客様からの問い合わせ内容

お客様より、AWS Direct Connect（DX）の利用料がいつから発生するのかについて質問あり。
特に、

- 接続（ホスト型接続）承諾時か
- VIF 作成時か
- BGP ピアが UP した時か

どの時点で課金が開始されるのかを確認したい、との内容。

またDXにおけるポートが接続（コネクション）の理解で正しいかについても質問あり。

---

## ■ 調査内容

### ① 課金開始タイミング

- DXの課金は 「ポートがプロビジョニングされたタイミング」 から発生
- データ転送の有無や VIF の作成、BGP ピアの UP 状態に関わらず、ポート時間課金が開始される

  公式ドキュメント引用（[1] より）
> AWS リージョン (Amazon Virtual Private Cloud や AWS Transit Gateway など) またはローカルゾーンで実行されているリソースに接続する場合、
> AWS Direct Connect ロケーションを介して、容量、ポート時間、データ転送 (DTO) といった 3 つの要素が料金を決定します。

> 容量とは、ネットワーク接続でデータを転送できる最大レートのことです。AWS Direct Connect 接続の容量は、メガビット/秒 (Mbps) またはギガビット/秒 (Gbps) で計測されます。
> 1 ギガビット/秒、または 1 Gbps は、1,000 メガビット/秒 (1,000 Mbps) に相当します。

> ポート時間は、AWS Direct Connect ロケーション内で、AWS または AWS Direct Connect デリバリーパートナーのネットワーク機器で使用するためにポートがプロビジョンされている時間を測定します。
> ポートをデータが通過していなくても、ポート時間の料金は発生します。ポート時間料金は、接続タイプ (専用またはホスト型) によって決まります。

---

### ② ポート＝接続（コネクション）の理解

- ホスト型接続の場合、パートナーが接続をプロビジョニングし、ユーザーが承諾した時点で課金開始

公式ドキュメント引用（[2] より）
> 課金されるホスト型接続のポート時間はどのように定義されていますか?

> ホスト型接続を承認すると、ポート時間の課金が開始されます。
> ポート料金の請求は、ホスト型接続が利用のためにプロビジョニングされている限り、引き続き発生します。
> ホスト型接続への課金の停止をご希望の場合は、AWS Direct Connect パートナーに依頼して、対象のホスト型接続をキャンセルしてください。

---

## ■ 回答（AWS公式ドキュメントに基づく）

### 課金開始タイミング
- DXの課金は **「ポートがプロビジョニングされたタイミング」** から発生
- データ転送の有無や VIF の作成、BGP ピアの UP 状態に関わらず、ポート時間課金が開始される

### ポート＝接続（コネクション）の理解
- お客様の理解通りで相違なし
- ホスト型接続の場合、パートナーが接続をプロビジョニングし、ユーザーが承諾した時点で課金開始

### 補足事項
- VIF 作成や BGP ピア UP は課金条件ではない
- 料金算出には容量、ポート時間、データ転送量が影響するため、詳細はお客様側で料金見積りツール[3]にて見積もりしていただくよう案内
- 参考としてホスト型接続の受入れ手順[4]に関しても案内実施

---

## ■ 最終結論（お客様側）

回答内容についてご納得いただけたため、本問い合わせはクローズ。

---
## 📚 参考資料（AWS公式ドキュメント）

- **[1] AWS Direct Connect の料金**  
  [https://aws.amazon.com/jp/directconnect/pricing/](https://aws.amazon.com/jp/directconnect/pricing/)

- **[2] AWS Direct Connect のよくある質問**  
  [https://aws.amazon.com/jp/directconnect/faqs/](https://aws.amazon.com/jp/directconnect/faqs/)

- **[3] AWS 料金見積りツール**  
  [https://calculator.aws/#/](https://calculator.aws/#/)

- **[4] AWS Direct Connect ホスト接続を受け入れる**  
  [https://docs.aws.amazon.com/ja_jp/directconnect/latest/UserGuide/accept-hosted-connection.html](https://docs.aws.amazon.com/ja_jp/directconnect/latest/UserGuide/accept-hosted-connection.html)
