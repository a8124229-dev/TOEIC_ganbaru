# くーちゃんTOEIC（ホーム画面アプリ版）

## 1. GitHub Pages に置く（これだけで使える）
1. GitHub で新しいリポジトリを作る（例：`kuchan-toeic`、Public）
2. このフォルダの中身（index.html / manifest.webmanifest / sw.js / icon-*.png）をそのままアップロード
3. Settings → Pages → Branch を `main` / `(root)` にして Save
4. 数分後に `https://<ユーザー名>.github.io/kuchan-toeic/` が開けるようになる
5. iPhone の Safari でそのURLを開く → 共有ボタン → 「ホーム画面に追加」
   → アイコンから開くと、ブラウザの枠なしの全画面で開きます

この状態では、チェックは「そのiPhoneの中」に保存されます。

## 2. けにゃにも見せたいとき（Supabase・任意）
1. Supabase で新しいプロジェクトを作る
2. SQL Editor で下を実行

```sql
create table progress (
  day text primary key,
  done jsonb not null default '{}',
  updated_at timestamptz default now()
);
alter table progress enable row level security;
create policy "read"   on progress for select using (true);
create policy "insert" on progress for insert with check (true);
create policy "update" on progress for update using (true);
```

3. Project Settings → API の「Project URL」と「anon public key」をコピー
4. index.html の中の次の行に貼り付けて、GitHub にアップし直す
   `const CFG={SUPABASE_URL:"", SUPABASE_KEY:""};`
5. けにゃ用のページは、URLの最後に `?kenya` をつけたもの
   例：`https://<ユーザー名>.github.io/kuchan-toeic/?kenya`
   （見るだけで、今日の進み具合とはなまるが表示されます。LINEで送る文にもこのURLが入ります）

※ anon key はページの中から見えるので、URLを知っている人ならチェックを書き換えることもできます。勉強記録なので実害は小さいですが、URLは広めないでください。
