# context-engine-auth

`context-engine`（[masakihnw/context-engine](https://github.com/masakihnw/context-engine)、private repo）の
リモート MCP サーバ（`mcp-server` Edge Function）が使う **OAuth 認可フォームの静的表示専用ページ**。

## これは何か

Supabase Edge Functions のデフォルトドメイン（`*.supabase.co`）は、GET リクエストが返す
`text/html` レスポンスを自動的に `text/plain` へ書き換える仕様がある（フィッシング対策目的）。
この制限のため、OAuth の `/authorize`（GET）で認可フォーム（パスワード入力画面）を
直接レンダリングできない。

このリポジトリの `index.html` は GitHub Pages（`text/html` を正しく配信できる）でホストし、
その表示だけを肩代わりする。**シークレットの検証・PKCE・トークン発行などの実処理は
一切ここでは行わず、全て Supabase Edge Function 側（サーバ）で完結する**。このページは
URL クエリパラメータをフォームの hidden input に転記して Edge Function の `/authorize`
（POST）へそのまま中継するだけの、ロジックを持たない UI シェル。

## セキュリティ上の位置づけ

- 秘密情報は一切含まない・保持しない（本人確認シークレットはブラウザ→Edge Function 間で
  直接やり取りされ、この静的ページを経由してサーバへ送信も記録もされない）。
- public repo で問題ない（中身は誰が見ても実害のない転記フォームのみ）。
