# mailgate

URLのクエリパラメータからメール各項目を受け取り、日付プレースホルダーを今日の日付に置換してから `mailto:` URLを組み立て、即座にメールクライアントへリダイレクトする静的ページです。GitHub Pagesで配布できます。

## URLパラメータ

| パラメータ | 説明 |
|-----------|------|
| `to` | 宛先メールアドレス |
| `cc` | CCメールアドレス |
| `bcc` | BCCメールアドレス |
| `subject` | 件名 |
| `body` | 本文 |

すべてのパラメータは省略可能です。各値に日付プレースホルダーを含めることができます。

## 日付プレースホルダー

### 記法

```
[キーワード(フォーマット)]
```

| キーワード | 基準日 |
|-----------|--------|
| `today` | 今日 |
| `yesterday` | 昨日（月曜日の場合は金曜日） |
| `thismonth` | 今月1日 |
| `nextmonth` | 来月1日 |

### フォーマットトークン

| トークン | 説明 | 例 |
|---------|------|----|
| `yyyy` | 年4桁 | `2025` |
| `yy` | 年下2桁 | `25` |
| `mm` | 月2桁（ゼロ埋め） | `05` |
| `m` | 月1〜2桁 | `5` |
| `dd` | 日2桁（ゼロ埋め） | `09` |
| `d` | 日1〜2桁 | `9` |
| `dddd` | 曜日（英語フル） | `Sunday` |
| `ddd` | 曜日（英語略称） | `Sun` |
| `aaaa` | 曜日（日本語フル） | `日曜日` |
| `aaa` | 曜日（日本語） | `日` |
| `HH` | 時2桁 | `09` |
| `H` | 時1〜2桁 | `9` |
| `MM` | 分2桁 | `05` |
| `M` | 分1〜2桁 | `5` |
| `SS` | 秒2桁 | `00` |
| `S` | 秒1〜2桁 | `0` |

### 使用例

```
[today(yyyy/mm/dd)]        → 2025/05/25
[today(yyyy年m月d日(aaa))] → 2025年5月25日(日)
[yesterday(mm/dd)]         → 05/24
[thismonth(yyyy-mm)]       → 2025-05
[nextmonth(yyyy年m月)]     → 2025年6月
```

## URLサンプル

### シンプルな使用例

```
https://amber-color.github.io/mailgate/?to=example@example.com&subject=ご連絡&body=お世話になっております。
```

### 日付プレースホルダーを使った例

件名に今日の日付を入れる：

```
https://amber-color.github.io/mailgate/?to=boss@example.com&subject=[today(yyyy/mm/dd)]%20日報&body=[today(m月d日(aaa))]の作業報告です。
```

前営業日の日付を本文に入れる：

```
https://amber-color.github.io/mailgate/?to=team@example.com&subject=議事録送付&body=[yesterday(yyyy年m月d日(aaa))]の会議議事録を添付します。
```

今月・来月を件名に使う：

```
https://amber-color.github.io/mailgate/?to=accounting@example.com&subject=[thismonth(yyyy年m月)]請求書について&body=[nextmonth(m月)]分の請求書をご確認ください。
```

## 動作の流れ

1. ページが読み込まれる
2. クエリパラメータから `to`, `cc`, `bcc`, `subject`, `body` を取得
3. 各値に含まれる日付プレースホルダーをアクセス時の日付で置換
4. `mailto:` URLを組み立ててメールクライアントへリダイレクト
5. ページ上には置換結果のデバッグ情報と手動用リンクを表示

## GitHub Pagesでの配布

リポジトリのSettingsからGitHub Pagesを有効にし、`index.html` を配置するだけで使えます。
