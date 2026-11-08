# FXルール検証&シェア(スマホアプリ版)

## ファイル
index.html(アプリ本体) / config.js(共有機能の設定) / manifest.webmanifest・sw.js・icon-*.png(スマホアプリ化用)

## 1. ルール検証だけ使う場合(設定不要)
フォルダごと下の「2. 置き場」に置くだけで使えます。共有機能は「未設定」と表示されます。

## 2. 置き場(無料のサイト置き場に置く)
いずれか1つを選び、フォルダの中身をアップロードします。
- Netlify Drop(netlify.com/drop): フォルダをドラッグするだけ。いちばん簡単です。
- Cloudflare PagesまたはGitHub Pages
アップロードすると https://〜 のURLが発行されます。

## 3. 友達との写真共有を使う場合(Supabase・無料枠あり)
1. supabase.com で無料アカウントを作り、New projectを作成。
2. SQL Editorに次を貼り付けて実行。
```sql
create table posts(
  id bigint generated always as identity primary key,
  room text not null, nick text, title text, img text, ts bigint);
create index on posts(room, ts desc);
alter table posts enable row level security;
create policy "read" on posts for select to anon using (true);
create policy "write" on posts for insert to anon with check (true);
```
3. Project Settings → API の「Project URL」と「anon public」キーを、config.js に貼り付ける。
4. config.js を保存して、置き場に上げ直す。
5. 友達全員が同じ「合言葉」を入力すると、同じ合言葉の投稿が見えます。

### 注意(セキュリティ)
- 合言葉は簡易的な絞り込みです。URLとキーを知っている人は、他の合言葉の投稿も取得できます。ログインはありません。
- 投稿の削除機能はありません。不要な投稿は、Supabaseの管理画面(Table Editor)から消してください。
- 写真には口座番号や個人情報が写らないようにしてください。

## 4. スマホに入れる
- iPhone: Safariでアプリ(URL)を開く → 共有ボタン → ホーム画面に追加
- Android: Chromeで開く → メニュー → ホーム画面に追加(またはアプリをインストール)
