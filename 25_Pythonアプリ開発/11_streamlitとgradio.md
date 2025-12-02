# streamlitとgradio

作成日 2025/07/01

どちらも、Webアプリを簡単に生成するツール

## 1. Streamlit

[Streamlit • A faster way to build and share data apps](https://streamlit.io/)

```python
import streamlit as st

st.write("""
# My first app
Hello *world!*
""")
```

## 2. Gradio

[Gradio](https://www.gradio.app/)

```python
import gradio as gr

def greet(name):
    return "Hello " + name + "!"

demo = gr.Interface(fn=greet, inputs="text", outputs="text")
demo.launch()
```

## 3. 比較記事を読む

[Streamlit vs Gradio: Pythonダッシュボードの究極の対決](https://myscale.com/blog/ja/streamlit-vs-gradio-ultimate-showdown-python-dashboards/)

[Streamlit vs Gradio in 2025: AIアプリフレームワークとしての比較](https://www.squadbase.dev/ja/blog/streamlit-vs-gradio-in-2025-a-framework-comparison-for-ai-apps)
