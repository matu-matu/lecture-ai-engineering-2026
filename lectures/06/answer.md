# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：glossary
- STEP 2 の [tool call]：[tool call]   glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：[tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：[tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング 概要'})

- 気づき・疑問：
ステップ3の時に、uv run python -m app.mcp_agent --trace "https://modelcontextprotocol.io/ の冒頭を日本語で3行に要約して"　このコマンドを実行したら、  File "/workspaces/lecture-ai-engineering-2026/.venv/lib/python3.12/site-packages/pydantic_ai/mcp.py", line 1506, in _call_tool_checked…というようなのがたくさん出て結局3行に要約せずに終了したことがあった。生成AIに聞いてみると、fetch_fetchツールの失敗によるリトライを何度もしているようで、回数を数えると60回以上リトライしているようだ。リトライ失敗後に再度コマンドを実行したら成功したのだが、リトライ回数の上限はあるのか気になった。


## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎 
- やったこと：app/mcp_server.pyにserver.tool()(tools.get_lecture_summary)を追加し、ツールが増えたことを確認する
- 結果：app/mcp_server.pyにserver.tool()(tools.get_lecture_summary)を追加するとモデルに見えているツール: 8 個となり、[glossary_get_lecture_summary]が増えている
- 気づき・疑問：講義の中ではapp/mcp_servers.jsonにツールを追加していたが、app/mcp_server.pyに追加してもツールが追加されることがわかった。どちらからでもツールが追加されることがわかったが、実装するとなったらどちらに追加した方がいいというのはあるのだろうか。
