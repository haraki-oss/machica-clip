# Machica Clip

Machica Collection で気に入った地域カードを「クリップ」して、自分のリストに整理する専用アプリです。

## 役割
- Machica Collection（本体サイト）で `📎 クリップに追加` を押したカードをここで保存
- ユーザー単位で複数の「リスト」を作成・編集・削除
- クリップしたカードを後から詳細表示（Phase 3）
- そのカードの本体ページ（Collection）への直リンク

## 技術スタック
- HTML / Vanilla JavaScript / Tailwind CSS（Play CDN）
- Supabase（PostgreSQL + Auth + Storage）
- ホスティング: Vercel（GitHub 連携で自動デプロイ）
- フォント: Afacad / Zen Kaku Gothic New

## 主なファイル
| ファイル | 役割 |
|---|---|
| `index.html` | 全ビュー（ログイン・マイページ・リスト詳細・カード詳細・カード追加）の DOM |
| `app.js` | SPA ルーティング・Supabase 連携・各ビューのロジック |
| `styles.css` | 軽い独自スタイル（フェードイン等） |

## ハッシュルーティング
| Hash | 役割 |
|---|---|
| `#login` | ログイン / 新規登録 |
| `#mypage` | プロフィール + リスト一覧（デフォルト） |
| `#list?id=<list_id>` | リスト詳細（クリップ済みカード一覧） |
| `#card?id=<collected_card_id>` | クリップしたカードの詳細表示（Phase 3） |
| `#add?card_id=<id>&title=<t>&image=<url>` | Collection からの「クリップに追加」遷移先 |

## Supabase テーブル
| テーブル | 主なカラム |
|---|---|
| `lists` | id, user_id, name, created_at |
| `collected_cards` | id, user_id, list_id, original_card_id, title, image_url, created_at |

`original_card_id` で Collection 側 `cards` テーブル（同一 Supabase プロジェクト）を参照し、詳細画面でリッチ情報をフェッチします。

## Collection との連携
Collection 側 `app.js` の `clipBtn` ハンドラから

```
<machica-clip ホスト>/index.html#add?card_id=<id>&title=<エンコード済タイトル>&image=<エンコード済画像URL>
```

の URL に遷移してくる前提です。Collection 側にこの URL を設定する必要があります。

## ローカル開発
このリポジトリにはビルドツールが無いため、`index.html` を任意の静的サーバーで配信するだけで動きます。例:

```powershell
cd C:\Users\AR0262\Documents\machica-clip
python -m http.server 8080
# http://localhost:8080 を開く
```
