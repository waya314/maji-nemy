# 寝室入室 予約フォーム（Private Bedroom Salon Booking）

サロン風の週間空き状況マトリクスから寝室への入室予約ができるWebアプリです。
**GitHub Pages** でそのまま公開でき、**Google スプレッドシート ＋ Google Apps Script (GAS)** と連携して予約データを永続保存・リアルタイム取得します。

## 主な機能

- **Google スプレッドシート永続化**：ページを開くと自動で最新データを取得し、予約・キャンセル・満席設定を即座にスプレッドシートへ保存（二重予約防止ロック付き）。未設定時はローカル保存モードで動作確認可能。
- **ビューポート動的フィット**：PC・タブレット・スマホの画面サイズを自動検知し、カレンダー行高や余白を可変調整して画面内にすっきり収まる設計。
- **5言語スイッチ対応**：日本語・English・한국어・中文・Tiếng Việt をヘッダーからワンタップ切替（設定はブラウザに保存）。
- **管理モード（パスワード保護）**：パスコード `1107` でロック解除。空き枠（`◎` / `×`）の切替、GAS連携URLの設定、データ初期化が可能。
- **LINE共有テキスト生成**：予約一覧からそのまま貼れる予約詳細テキストをワンタップコピー。

## ファイル構成

```text
bedroom-reservation-app/
├── index.html     # Webアプリ本体（GitHub Pages エントリーポイント）
├── gas/
│   └── Code.gs    # Google Apps Script バックエンド
├── README.md      # セットアップ＆公開手順
└── .gitignore
```

## セットアップ手順

### 1. Google スプレッドシート ＆ GAS のデプロイ

1. 予約保存用の Google スプレッドシートを用意します（タブ名：`bookings` / `blocked_slots`）。
   - タブが空でも、`Code.gs` の初回実行時にヘッダー付きで自動作成されます。
2. スプレッドシート上部メニューから **「拡張機能」→「Apps Script」** を開きます。
3. エディタに `gas/Code.gs` の内容をすべて貼り付けて保存します。
4. 右上の **「デプロイ」→「新しいデプロイ」** をクリックします。
   - 種類の選択：**ウェブアプリ**
   - 次のユーザーとして実行：**自分**
   - アクセスできるユーザー：**全員**
5. **「デプロイ」** を押し、権限を承認すると発行される **ウェブアプリ URL** をコピーします。

### 2. フロントエンドに GAS URL を設定

方法は2つあります。

- **GitHub Pages で全員に共有する場合（推奨）**
  `index.html` 内の `DEFAULT_GAS_WEB_APP_URL` にコピーしたウェブアプリ URL を貼り付けて保存します。
- **ブラウザ上ですぐ試したい場合**
  ページ右上の **「管理」** → パスワード `1107` を入力 → 管理バーの入力欄にウェブアプリ URL を貼り付けて **「GAS URLを保存して同期」** を押します。

### 3. GitHub Pages で公開

1. GitHub で新しいリポジトリを作成し、このフォルダ内のファイルをプッシュします。
   ```bash
   git init
   git add .
   git commit -m "Initial commit: bedroom reservation form"
   git branch -M main
   git remote add origin <作成したリポジトリのURL>
   git push -u origin main
   ```
2. GitHub リポジトリの **Settings → Pages** を開きます。
3. **Build and deployment → Source** を **Deploy from a branch** に設定し、Branch に `main` / `/ (root)` を選んで **Save** を押します。
4. 数十秒〜数分で GitHub Pages の公開URLからアクセスできるようになります。

## 管理モードのパスワード変更方法

初期パスワードは `1107` に設定されています。変更する場合は以下の2か所を同じ値に書き換えてください。

1. `index.html` の `const ADMIN_PASSCODE = '1107';`
2. `gas/Code.gs` の `const ADMIN_PASSWORD = '1107';`（変更後、GAS を再デプロイ）
