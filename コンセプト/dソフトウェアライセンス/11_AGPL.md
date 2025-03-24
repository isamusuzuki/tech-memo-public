# AGPL ソフトウェアライセンス

作成日 2025/01/21

## 1. 参照サイトを読み漁る

参照サイト => [AGPLを理解する](https://future-architect.github.io/articles/20220922a/)

> Affero General Public Licenseの略称であるAGPLはこれら（OSIが承認した80以上のライセンス）のライセンスの1つで、
> より具体的には強いコピーレフト・ライセンスであり、間違いなく最も誤解されているライセンスの1つでしょう。

GPLとの違い

> AGPLはGPLは以下の内容が第13節に追加されている点を除くとほとんど同じです。
>
> もしあなたがユーザにネットワークを通じてAGPLでライセンスされたソフトウェアにアクセスさせるならば、それは配布の一形態と見なすということです。
>
> ボブが開発したバイナリアプリケーション（ライブラリではない）を例にとって考えてみよう。表現の都合上、これをXBinと呼ぶことにします。
>
> ボブはGPLを使うことにしました。ユーザーはみな、バグを見つけたり、機能を追加してほしいときはいつでも、彼にパッチを送ることができ、ボブにとっては最高のライセンスでした。ボブは喜んで彼らのコードをマージし、幸せな気分に浸っていました。しかしある日、彼は大手クラウドプロバイダーであるProviderXが、XBinにより多くの機能を追加した上で、自社のプロジェクト管理スイートの一部として提供していることを知りました。しかし、XBinを改善するためのパッチをProviderXに送ってもらうことを望んでいたボブには嬉しくありません。GPLがネットワークを通じた配布を考慮していないため、今はボブには合法的に何かを行うことはできません。
>
> GPLライセンスの欠点に気づいたボブは、次のリリースからAGPLに切り替えました。これで、ProviderXが何か変更を加え、それをサービスとしてユーザに配布したときは、いつでも、同じライセンスのもとでソース形式で変更を利用できるようにしなければならなくなりました。したがって、ボブはProviderXによってなされた改良を彼自身のソースコードにマージできるのです。これはフェアプレーです。

まとめ

> もしあなたがAGPLでライセンスされたバイナリを「そのまま」、何の変更も加えずに使うのであれば、あなたはあまり考える必要はありません。

参照サイト2 => [GPL、LGPL、AGPLソフトウェアライセンスについての備忘録](https://qiita.com/tatsumi_t2/items/3da688a5123d37986331)

AGPLの特徴

> AGPLはソフトウェアを配布しなくても、ネットワーク越しに利用しているだけでライセンスの効力が生まれる点でGPLやLGPLと異なります。
>
> AGPLで作られたソフトウェアを使ってWebサービスを提供していて、そのソフトウェアを改変した場合、そのWebサービス利用者に対してソースコードを開示しなくてはなりません。

### 2. iTextの公式サイト（英語）を検証する

[Open Source AGPLv3 license](https://itextpdf.com/how-buy/AGPLv3-license)

```text
Only allowed in AGPL environments
It’s a legal violation to use iText Core/Community and our open source add-ons in a non-AGPL environment.

Modifications made to the source code
You must disclose any modifications made to iText.

Producer line and copyright
When using iText Core/Community under AGPL, you must prominently mention iText and include the iText copyright and AGPL license in output file metadata, and also retain the producer line in every PDF that is created or manipulated using iText.
```
