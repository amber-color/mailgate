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
| `workday(N, format)` | N営業日後（負で前、土日スキップ） |
| `today` | 今日 |
| `yesterday` | 前日 |
| `tomorrow` | 翌日 |
| `lastmonth` | 先月1日 |
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
[workday(-1, yyyy/mm/dd)]   → 前営業日
[workday(+1, m月d日(aaa))]  → 翌営業日（曜日付き）
[workday(0, yyyy/mm/dd)]    → 今日（土日なら直前の金曜）
[tomorrow(mm/dd)]           → 05/26
[lastmonth(yyyy年m月)]      → 2025年4月
[nextmonth(yyyy年m月)]      → 2025年6月
```

---

## カスタム変数 `[param:xxx]`

URLのクエリパラメータに任意のキーと値を追加すると、`[param:キー名]` という形式で件名・本文に埋め込めます。

```
[param:name]    → URLに &name=田中様 を追加すると「田中様」に置換
[param:project] → URLに &project=ECサイト を追加すると「ECサイト」に置換
```

テンプレートを1つ作り、宛名や案件名だけ変えて使い回す用途に便利です。

```
https://mailgate.pages.dev/?to=boss@example.com&subject=[today(mm/dd)] [param:project]打ち合わせ&body=[param:name]%0Aいつもお世話になっております。&name=田中様&project=ECサイト
```
[→ 試す](https://mailgate.pages.dev/?to=boss@example.com&subject=%5Btoday(mm/dd)%5D%20%5Bparam:project%5D打ち合わせ&body=%5Bparam:name%5D%0Aいつもお世話になっております。&name=田中様&project=ECサイト)

URLジェネレーター（パラメータなしでアクセス）の変数パネルからカスタム変数を追加すると、URLに自動で付加されます。

---

## URLサンプル

シンプルな使用例：

```
https://mailgate.pages.dev/?to=example@example.com&subject=ご連絡&body=お世話になっております。
```
[→ 試す](https://mailgate.pages.dev/?to=example@example.com&subject=ご連絡&body=お世話になっております。)

件名に今日の日付を入れる：

```
https://mailgate.pages.dev/?to=boss@example.com&subject=[today(yyyy/mm/dd)] 日報&body=[today(m月d日(aaa))]の作業報告です。
```
[→ 試す](https://mailgate.pages.dev/?to=boss@example.com&subject=%5Btoday(yyyy/mm/dd)%5D%20日報&body=%5Btoday(m月d日(aaa))%5Dの作業報告です。)

前営業日を本文に入れる：

```
https://mailgate.pages.dev/?to=team@example.com&subject=議事録送付&body=[yesterday(yyyy年m月d日(aaa))]の会議議事録を添付します。
```
[→ 試す](https://mailgate.pages.dev/?to=team@example.com&subject=議事録送付&body=%5Byesterday(yyyy年m月d日(aaa))%5Dの会議議事録を添付します。)

先月・来月を件名に使う：

```
https://mailgate.pages.dev/?to=accounting@example.com&subject=[lastmonth(yyyy年m月)]請求書について&body=[nextmonth(m月)]分の請求書をご確認ください。
```
[→ 試す](https://mailgate.pages.dev/?to=accounting@example.com&subject=%5Blastmonth(yyyy年m月)%5D請求書について&body=%5Bnextmonth(m月)%5D分の請求書をご確認ください。)
