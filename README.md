# mailgate

URLのクエリパラメータからメール各項目を受け取り、日付プレースホルダーを今日の日付に置換してから `mailto:` URLを組み立て、即座にメールクライアントへリダイレクトする静的ページです。

**サンプル：** https://mailgate.pages.dev/

---

## 使い方

パラメータなしで開くと **URLジェネレーター** が起動します。フォームに入力すると mailgate URL が自動生成されるので、それをブックマークやリンクとして使います。

パラメータ付きでアクセスするとメールクライアントが即座に開きます。

---

## URLパラメータ

| パラメータ | 説明 |
|-----------|------|
| `to` | 宛先メールアドレス |
| `cc` | CC |
| `bcc` | BCC |
| `subject` | 件名 |
| `body` | 本文 |

すべて省略可能。各値に日付プレースホルダーを含めることができます。

---

## 日付プレースホルダー

### 記法

```
[キーワード(フォーマット)]
```

| キーワード | 基準日 |
|-----------|--------|
| `today` | 今日 |
| `yesterday` | 前日（月曜の場合は金曜） |
| `thismonth` | 今月1日 |
| `nextmonth` | 来月1日 |

### フォーマットトークン

| トークン | 説明 | 例 |
|---------|------|----|
| `yyyy` | 年4桁 | `2025` |
| `yy` | 年下2桁 | `25` |
| `mm` | 月2桁（ゼロ埋め） | `05` |
| `m` | 月 | `5` |
| `dd` | 日2桁（ゼロ埋め） | `09` |
| `d` | 日 | `9` |
| `dddd` | 曜日（英語フル） | `Sunday` |
| `ddd` | 曜日（英語略） | `Sun` |
| `aaaa` | 曜日（日本語フル） | `日曜日` |
| `aaa` | 曜日（日本語） | `日` |
| `HH` | 時2桁 | `09` |
| `H` | 時 | `9` |
| `MM` | 分2桁 | `05` |
| `M` | 分 | `5` |
| `SS` | 秒2桁 | `00` |
| `S` | 秒 | `0` |

### 例

```
[today(yyyy/mm/dd)]         → 2025/05/25
[today(yyyy年m月d日(aaa))]  → 2025年5月25日(日)
[yesterday(mm/dd)]          → 05/24
[thismonth(yyyy-mm)]        → 2025-05
[nextmonth(yyyy年m月)]      → 2025年6月
```

---

## URLサンプル

シンプルな使用例：

```
https://mailgate.pages.dev/?to=example@example.com&subject=ご連絡&body=お世話になっております。
```

件名に今日の日付を入れる：

```
https://mailgate.pages.dev/?to=boss@example.com&subject=[today(yyyy%2Fmm%2Fdd)]%20日報&body=[today(m月d日(aaa))]の作業報告です。
```

前営業日を本文に入れる：

```
https://mailgate.pages.dev/?to=team@example.com&subject=議事録送付&body=[yesterday(yyyy年m月d日(aaa))]の会議議事録を添付します。
```

今月・来月を件名に使う：

```
https://mailgate.pages.dev/?to=accounting@example.com&subject=[thismonth(yyyy年m月)]請求書について&body=[nextmonth(m月)]分の請求書をご確認ください。
```
