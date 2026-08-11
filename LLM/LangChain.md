# LangChain Components  
LangChain is built around 6 core components — think of them as the organs of a body, each with a specific job, all working together:

1. 🤖 Models
2. 💬 Prompts
3. 🔗 Chains
4. 🧠 Memory
5. 📚 Indexes — The Power of RAG
6. 🤖 Agents



## 1. 🤖 Models
```python
# OpenAI via LangChain
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv

load_dotenv()
model = ChatOpenAI(model='gpt-4', temperature=0)
result = model.invoke("How divide the result by 1.5?")
print(result.content)
```


```python
# Anthropic Claude via LangChain — same interface, different model!
from langchain_anthropic import ChatAnthropic
from dotenv import load_dotenv

load_dotenv()
model = ChatAnthropic(model='claude-3-opus-20240229')
result = model.invoke("Hi who are you")
print(result.content)
```

```python
# Google Gemini via LangChain
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv
load_dotenv()
model = ChatGoogleGenerativeAI(model='gemini-2.0-flash', temperature=0)
result = model.invoke("Hi who are you")
print(result.content)
```


```python
# Local model via Ollama — no API key needed
from langchain_ollama import ChatOllama
model = ChatOllama(model='llama3.1', temperature=0)
result = model.invoke("Hi who are you")
print(result.content)
```



💡 Key idea: Swap ChatOpenAI for ChatAnthropic and everything else stays the same. That's model-agnostic development.


## 2. 💬 Prompts

Reusable, dynamic templates for talking to LLMs.

### Dynamic & Reusable Prompts
```python
from langchain_core.prompts import PromptTemplate

prompt = PromptTemplate.from_template('Summarize {topic} in {emotion} tone')
print(prompt.format(topic='Cricket', length='fun'))
```

### Role-Based Prompts
```python
from langchain_core.prompts import ChatPromptTemplate

chat_prompt = ChatPromptTemplate.from_template([
    ("system", "Hi you are a experienced {profession}"),
    ("user", "Tell me about {topic}"),
])

formatted_messages = chat_prompt.format_messages(
    profession="Doctor", 
    topic="Viral Fever"
)
```

### Few Shot Prompting
```python
from langchain_core.prompts import FewShotPromptTemplate

examples = [
    {"input": "I was charged twice for my subscription this month.", "output": "Billing Issue"},
    {"input": "The app crashes every time I try to log in.", "output": "Technical Problem"},
    {"input": "Can you explain how to upgrade my plan?", "output": "General Inquiry"},
    {"input": "I need a refund for a payment I didn't authorize.", "output": "Billing Issue"},
]

example_template = """
Ticket: {input}
Category: {output}
"""

few_shot_prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=PromptTemplate(
        input_variables=["input", "output"], 
        template=example_template
    ),
    prefix="Classify the following customer support tickets into one of the categories: "
           "'Billing Issue', 'Technical Problem', or 'General Inquiry'.\n\n",
    suffix="User_input:\nCategory:",
    input_variables=["user_input"],
)
```


## 3. 🔗 Chains
Chains = Pipelines. They are the heart of LangChain (hence the name!). Instead of calling a model once, you chain multiple calls and operations together into a workflow.  

🟢 Sequential Chains — Steps run one after another:
Example: Translate 1000-word English text → Hindi summary (100 words)
[image]


🟠 Parallel Chains — Multiple LLMs run simultaneously, results combined:
Example: Generate a report from two expert LLMs simultaneously, then merge
[image]


🟣 Conditional Chains — Route based on output:
Example: AI feedback agent — good feedback → "Thank you!", bad feedback → send email alert
[image]


## 4. 🧠 Memory
Without memory, every API call is stateless — like talking to someone with amnesia who forgets you the moment you stop speaking.


## 5. 📚 Indexes 
Connecting your LLM to external knowledge.


## 6. 🤖 Agents
Agents are AI systems that combine:

🧠 Reasoning capabilities (the LLM brain — chain of thought)
🔧 Tools (external actions it can call)

