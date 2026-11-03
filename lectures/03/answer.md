# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
　第1回の内容をきちんと覚えているわけではないが、一般的な内容ではなく演習の内容を尋ねると全然違う回答が返ってきたか、わからないという回答だったような気がする。第3回では、「AIE2026（AIエンジニア育成講座などの2026年目標のプログラムと推測されます）の宿題における「3レベル」という言葉は、多くの場合、**受講生のスキルや理解度、あるいは課題の難易度を3段階（初級・中級・上級など）に分けたもの**を指します。」と回答した。一般的なことを答えているような、AIE2026のことを答えているような、惜しいとこまできている。宿題の内容が3つの難易度に分かれていることが回答されているが、実際の宿題は基礎・実装・発展だが、LLMの回答は基礎・応用・発展となっている。

- `--trace` で見えたツール呼び出し（ツール名と引数）：
　[tool call]   make_quiz({'lecture': 2})

- 1つの質問での LLM 呼び出し回数（`requests`）：
[usage] RunUsage(cost=Decimal('0.0008838'), details={'text_prompt_tokens': 1196}, input_text_tokens=1196, input_tokens=1196, output_tokens=210, requests=2, tool_calls=2)

- 追加したツールと、docstring に書いた説明：
app/tools.py
def make_quiz(lecture: int) -> str:
    """指定した回の用語から 1つを選び、
    確認問題と答えを返す。
    「第2回の確認問題を出して」のように、
    復習用の問題を求められたときに使う。
    """
    # 1. load_glossary() から lecture が一致する用語を集める
    glossary = load_glossary()

    terms = [item for item in glossary if item.get("lecture") == lecture]

    if not terms:
        return json.dumps(
            {"error": f"第{lecture}回の用語が見つかりません。"},
            ensure_ascii=False
        )
    # 2. 1 つ選ぶ（random.choice ）
    term = random.choice(terms)
    
    # 3. 問題と答えを文字列で返す（ json.dumps など）
    result = {
        "question": f"{term['term']}の意味は？",
        "answer": term["definition"]
    }
    return json.dumps(result, ensure_ascii=False)


app/agent.py
agent.tool_plain(tools.make_quiz)

## 宿題

- 取り組んだレベル：基礎 

- やったこと：
　app/agent.py の INSTRUCTIONSから「用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する」を削除して検索
- 結果：
削除前
[model] gemini-3.5-flash-lite
第2回からの確認問題です！
**【確認問題】**
LoRAの意味（概要）は何でしょうか？
（考えてみてくださいね！第2回ではPEFTやLoRAについて扱います。）
---
関連する回：第2回
--- trace ---
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "LoRAの意味は？", "answer": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラメータが少なく、学習…
[usage] RunUsage(cost=Decimal('0.0004473'), details={'text_prompt_tokens': 916}, input_text_tokens=916, input_tokens=916, output_tokens=69, requests=2, tool_calls=1)

削除後
[model] gemini-3.5-flash-lite
第2回の内容から確認問題をお出しします！
**【確認問題】**
LoRAとは、どのような特徴を持つ学習手法でしょうか？
（答えを知りたいときや、ヒントが欲しいときはいつでもお声がけくださいね！）
（関連：第2回）
--- trace ---
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "LoRAの意味は？", "answer": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラメータが少なく、学習…
[usage] RunUsage(cost=Decimal('0.0004429'), details={'text_prompt_tokens': 868}, input_text_tokens=868, input_tokens=868, output_tokens=73, requests=2, tool_calls=1)

- 気づき・疑問：
「用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する」を削除前、削除後で比較すると、削除後はdata/glossary.jsonと関係ない答えが返ってくるのだろうと思っていたが、glossary.jsonと同じ答えが返ってきていた。一般的な質問なので、同じ回答が返ってきたのだろうか。削除しても、削除しなくてもglossaryを確認したような回答に見える。