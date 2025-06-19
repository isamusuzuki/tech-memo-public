# LangChain

作成日 2025/05/06

## 1. LangChainとは

公式サイト（英語） => [LangChain](https://www.langchain.com/)

LangChainの実装は、PythonとJavaScriptの2つが提供されているが、機械学習の周辺分野ではよくあるように、Pythonのほうが開発が活発で、JavaScriptでは未実装な機能もあったりする

- Pythonのユーザーガイド ... [Introduction | 🦜️🔗 LangChain](https://python.langchain.com/docs/introduction/)
- JavaScriptのユーザーガイド ... [Introduction | 🦜️🔗 Langchain](https://js.langchain.com/docs/introduction/)

LangChainを利用すると、各種言語モデルを統一的なインターフェイスで扱うことができる。OpenAIのGPT-4o以外にも、AnthropicのClaudeや、GoogleのGemini、オープンモデルのLlamaなどを使うことも可能

Pythonのサンプルコード

```python
from langchain_core.messages import AIMessage, HumanMessage, SystemMessage
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)

messages = [
    SystemMessage("You are a helpful assistant."),
    HumanMessage("こんにちは！私はジョンと言います！"),
    AIMessage(content="こんにちは、ジョンさん！どのようにお手伝いできますか？"),
    HumanMessage(content="私の名前がわかりますか？"),
]

ai_message = model.invoke(messages)
print(ai_message.content)
```

## 2. LCELとは

LCEL = LangChain Expression Language

LangChainでのChainの記述方法。プロンプトやLLMを縦線(`|`)でつなげて書き、処理の連鎖(Chain)を実現する

Pythonのサンプルコード

```python
# Recipeクラスを定義して、output_parserを準備する
from langchain_core.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field

class Recipe(BaseModel):
    ingredients: list[str] = Field(description="ingredients of the dish")
    steps: list[str] = Field(description="steps to make the dish")

output_parser = PydanticOutputParser(pydantic_object=Recipe)

# promptを用意する
from langchain_core.prompts import ChatPromptTemplate

prompt = ChaptPromptTemplate.from_messages(
    [
        {"system", "ユーザーが入力した料理のレシピを考えてください。\n\n{format_instructions}"},
        {"human", "{dish}"},
    ]
)

prompt_with_format_instructions = prompt.partial(
    format_instructions=output_parser.get_format_instructions()
)

# modelを準備する
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o-mini", temperature=0).bind(
    response_format={"type": "json_object"}
)

# LCELの記法で、chainを作成する
chain = prompt_with_format_instructions | model | output_parser

# chainを実行する
recipe = chain.invoke({"dish": "カレー"})
print(type(recipe))
print(recipe)
```

## 3. LangChainとLM Studio

参照記事 => [第2回　StreamlitとLangChainとLM studioを使って自分専用LLMウェブアプリを作りたい！](https://qiita.com/semiramisu/items/1a9c70f8dfa2c06d7404)

- [Streamlit](https://streamlit.io/) ... PythonスクリプトのInput/Outputを簡単にWeb画面に仕立てあげるツール
- LangChain ... 各種言語モデルを統一的なインターフェイスで扱うツール
- LM Studio ... ローカルLLMを扱うのがとてもラクになる統合ツール（取得・サーバー・チャット画面）
