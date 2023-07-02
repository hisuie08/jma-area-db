# 気象庁API用地域データベース

これは気象庁提供のAPIから地区の気象予報を取得するために必要な気象台IDを中心に地域をグループ化して少しばかりの汎用性を付与したデータベースである


# テーブル構造

## City
市町村情報
一部市町村単位を更に分割されているもの有り

詳細は[気象庁公式ホームページ](https://www.jma.go.jp/jma/kishou/know/bosai/bunkatsu.html)を参照
| id | name | kana | prefecture | office | region |
| - | - | - | - | - | - |
|市町村ID|市町村名|読み仮名|都道府県ID|気象台ID|地方ID|

## Office
気象台情報

| id | name | area |
| - | - | - |
| 気象台ID | 気象台名 | 管轄地区 |

## Prefecture
都道府県情報

| id | name | kana |
|-|-|-|
|都道府県ID|都道府県名|読み仮名(ｶﾀｶﾅ)|

## Region
地方情報
|id|name|office|
|-|-|-|
|地方ID|地方名|管区気象台|


# 基本的な気象予報取得

エンドポイントの`{OFFICEID}`を市町村の気象台IDに置き換える

### 天気予報エンドポイント
https://www.jma.go.jp/bosai/forecast/data/forecast/{OFFICEID}.json

### 注意報・警報エンドポイント
https://www.jma.go.jp/bosai/warning/data/warning/{OFFICEIF}.json

### 天気概要エンドポイント
https://www.jma.go.jp/bosai/forecast/data/overview_forecast/{OFFICEID}.json


# 参考情報

## 都道府県及び市町村IDについて

気象庁で用いられている市町村IDは[総務省の全国地方公共団体コード](https://www.soumu.go.jp/denshijiti/code.html)がベースな模様

具体的には

チェックディジットである末尾1桁を除去した団体コードを右詰め7桁にゼロ埋めしたものが市町村ID

市町村IDの先頭2桁が当該市町村の都道府県識別子

である。

総務省情報では都道府県コードも7桁に揃えられているが、本データベースでは簡略化のため2桁に丸める仕様とした

## 気象台について
都道府県と管轄気象台は、北海道や離島地区で厳密に一致しないことに注意。
Cityテーブルにて対応する気象台IDの使用を推奨

管区気象台と地方気象台の関係については、管区気象台が存在しない地区に設置されているものが地方気象台・測候所である。

つまり利用の上では原則管区気象台は地方気象台の上位存在**ではない**

## 気象予報の発表区域について
各気象台は気象予報の発表区域を2種類用いており、天気予報は広域な「一次細分区域」、警報や注意報はよりピンポイントな「二次細分区域」を基準に配信している。
ちなみに一次細分区域は概ね都道府県を2-6分割程度の範囲、二次細分区域は概ね市町村単位と考えれば良い

なお気象庁のAPIを叩く際これらの細分区域を気にする必要はない。
市町村から当該地域の各種気象予報を入手したい場合必要なものは気象台IDのみである


# 出典
- [気象庁](https://www.jma.go.jp/jma/index.html)公式ホームーページ
  - [象警報・注意報や天気予報の発表区域](https://www.jma.go.jp/jma/kishou/know/saibun/index.html)
  - [分割して発表を行う市町村](https://www.jma.go.jp/jma/kishou/know/bosai/bunkatsu.html)


- [総務省](https://www.soumu.go.jp/)公式ホームページ
  - 提供[全国地方公共団体コード](https://www.soumu.go.jp/denshijiti/code.html)


# 免責等


気象庁API用地域データベース（以下本データベース）は2023年7月時点の気象庁及び総務省提供情報の各種情報を統合・加工したものである

情報は今後の気象庁等各団体の変更により正確性を欠く可能性がある

本データベースの利用は上記出典元のガイドラインに準拠するとする

[気象庁コンテンツ利用ガイドライン](https://www.jma.go.jp/jma/kishou/info/coment.html)

[総務省コンテンツ利用ガイドライン](https://www.soumu.go.jp/menu_kyotsuu/policy/tyosaku.html#tyosakuken)

作成者は本データベースについて、その利用・複製・改変等に新たな制限を設けない。
誰であっても出典元ガイドラインに反しない範囲で本データベースを自由に使用できるものとする

本データベースの利用・複製・改変等により生じた損害や不利益について、作成者はその責任を負わないものとする

