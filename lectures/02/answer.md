# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 20,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
- 自分で学習を実行したか（`executed_training_myself`）：はい （実行できた範囲：上から全部行い、発展の実行まで　）
- 学習前：`format_rate` =0.09523809523809523　 / `content_rate` =　0.0
- 学習後：`format_rate` =　1.0 / `content_rate` =　0.7142857142857143
- 退行した質問：なし

- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）：根拠なしの質問に断定した
- 知識を問う質問（セクション 6）の結果（学習前の分類 → 学習後の分類）と気づいたこと：学習前はフォーマットエラーの分類だけだが、学習後はフォーマットエラーが一つもない
- 学習で変わったこと・変わらなかったこと：学習前は”needs_review”の出力を一切していないが、学習後は”needs_review”の出力をしている。学習後は根拠なしの質問に断定した出力をしたものが少なくなっている。

## 宿題

- 取り組んだレベル：基礎
- 変えた設定やデータ：num_train_epochs＝5、num_train_epochs＝100 
- 結果（比較表や数字）：
num_train_epochs＝5
format_rate＝1.0
content_rate＝0.7142857142857143
最終 loss＝1.569927
学習時間＝58 秒

num_train_epochs＝100
format_rate＝1.0
content_rate＝ 0.9047619047619048
最終 loss＝0.010557
学習時間＝ 1162 秒

- ファインチューニングが向く場面・向かない場面についての考察：
content_rateが、epoch数=20の時も、epoch数=5の時も0.7142857142857143だったので、content_rateを変えたい場合にはepoch数を多くまわさないと変わらないのかもしれない。epoch数が100の時も微々たるものではあるがlossが下がったので、epoch数は多ければ多い程ファインチューニングが良くなるのかもしれない。