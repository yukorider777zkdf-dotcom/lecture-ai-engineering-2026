# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
  素のLLMだと、講座の資料を確認せずにそれっぽい回答を長々と返してきていた。一方エージェントでは資料を読み込み、的確かつ簡潔に回答していた。
- `--trace` で見えたツール呼び出し（ツール名と引数）：
--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008256'), details={'text_prompt_tokens': 1352}, input_text_tokens=1352, input_tokens=1352, output_tokens=168, requests=2, tool_calls=2)
- 1つの質問での LLM 呼び出し回数（`requests`）：2
- 追加したツールと、docstring に書いた説明：
    """指定した回の用語から 1つを選び、
    確認問題と答えを返す。
    「第2回の確認問題を出して」のように、
    復習用の問題を求められたときに使う。
    """

## 宿題

- 取り組んだレベル：基礎 
- やったこと：INSTRUCTIONの "用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する。" という文言を消して、agentを動かした。
- 結果： 結果を次に示す。
[model] gemini-3.5-flash-lite
講座の用語集によると、**LoRA** と **QLoRA** の違いは以下の通りです。

* **LoRA**
  事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法です。学習するパラメータが少なく、学習結果も小さなファイルで保存できます。
* **QLoRA**
  ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習を行う手法です。これにより、より大きなモデルを小さなGPUでも扱えるようになります。

関係する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0008512'), details={'text_prompt_tokens': 1304}, input_text_tokens=1304, input_tokens=1304, output_tokens=184, requests=2, tool_calls=2)

traceの通りこの文言があってもなくてもsearch_glossary を使って検索をおこなって回答を行なった。
- 気づき・疑問：
システムプロンプトが多少雑でもdocstringやtoolの説明がきちんとしていれば、agentは有用なtoolを見つけ出して、タスクを行うのではないかと考えた。
