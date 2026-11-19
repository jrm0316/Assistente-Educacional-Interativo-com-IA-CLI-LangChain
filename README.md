# Assistente Educacional Interativo com IA - Python + LangChain + Groq

Assistente educacional desenvolvido em **Python**, utilizando **LangChain**, **PromptTemplate** e **Groq API** para gerar explicações didáticas sobre assuntos informados pelo usuário diretamente no terminal.

O sistema recebe um tema, constrói dinamicamente um prompt e utiliza um modelo de linguagem (LLM) para produzir uma explicação organizada e acessível para iniciantes.

## Objetivo do projeto

Demonstrar a integração de modelos de linguagem com aplicações Python que recebem entradas dinâmicas do usuário.

O projeto utiliza técnicas de **Prompt Engineering** para orientar a IA na geração de conteúdos educacionais, mantendo uma estrutura de resposta definida.

Essa abordagem pode servir como base para ferramentas educacionais, assistentes virtuais e aplicações CLI com Inteligência Artificial.

## Funcionalidades

- Entrada interativa de temas pelo terminal.
- Captura de informações utilizando `input()`.
- Construção de prompts dinâmicos com `PromptTemplate`.
- Integração com a Groq API por meio do LangChain.
- Geração de explicações sobre diferentes assuntos.
- Orientação das respostas para iniciantes.
- Organização das explicações em quatro partes.
- Exibição do conteúdo gerado diretamente no terminal.
- Carregamento da chave de API por variável de ambiente.

## Como a IA organiza as respostas

O prompt orienta o modelo a apresentar a explicação em quatro partes:

1. **O que é?** — definição simples do tema.
2. **Para que serve?** — aplicações e utilidade.
3. **Exemplo prático** — demonstração do conceito.
4. **Resumo final** — síntese dos principais pontos.

Essa estrutura é definida por instruções no prompt. O formato é seguido pelo modelo durante a geração, mas não existe validação automática que garanta sua aplicação em todas as respostas.

## Tecnologias utilizadas

- Python 3.12
- LangChain Core
- LangChain Groq
- PromptTemplate
- Groq API
- Modelo `openai/gpt-oss-120b`
- Python Dotenv
- Prompt Engineering
- Interface de Linha de Comando (CLI)

## Estrutura do projeto

```text
Assistente-Educacional-Interativo-com-IA-CLI-LangChain/
│
├── nivel5_Prompt+input_do_usuario_OK.py
├── README.md
└── .env
```

O arquivo `.env` contém a chave de acesso à Groq API e deve permanecer apenas no ambiente local.

O diretório `.venv` é criado durante a configuração do ambiente Python.

**Importante:** não publique `.env` nem `.venv` no GitHub. Recomenda-se incluir esses nomes no arquivo `.gitignore`.

## Como funciona

O programa realiza as seguintes etapas:

### 1. Carregamento das variáveis de ambiente

```python
from dotenv import load_dotenv

load_dotenv()
```

O `python-dotenv` carrega as configurações armazenadas no arquivo `.env`.

### 2. Inicialização do modelo de linguagem

```python
from langchain_groq import ChatGroq

llm = ChatGroq(
    model="openai/gpt-oss-120b",
    temperature=0
)
```

O LangChain utiliza a integração `ChatGroq` para acessar o modelo disponibilizado pela Groq.

O parâmetro `temperature=0` busca reduzir a aleatoriedade das respostas, embora não garanta resultados idênticos em todas as execuções.

### 3. Entrada dinâmica do usuário

```python
tema = input("Digite um tema para a IA explicar: ")
```

O usuário informa o assunto que deseja estudar.

O programa não depende de um tema fixo definido no código.

### 4. Construção do PromptTemplate

```python
from langchain_core.prompts import PromptTemplate

prompt = PromptTemplate(
    input_variables=["tema"],
    template="""
Você é um professor de informática que explica assuntos
de maneira simples e didática para iniciantes.

Explique o seguinte tema:

Tema: {tema}

Organize a resposta em quatro partes:

1. O que é?
2. Para que serve?
3. Exemplo prático
4. Resumo final

Utilize linguagem simples, evite termos técnicos
desnecessários e mantenha a explicação objetiva.
"""
)
```

A variável `{tema}` funciona como um espaço reservado que será preenchido com o assunto digitado pelo usuário.

Isso permite reutilizar o mesmo prompt para diferentes temas.

### 5. Envio do prompt e geração da resposta

```python
prompt_final = prompt.format(tema=tema)

resposta = llm.invoke(prompt_final)

print("\nResposta da IA: \n")
print(resposta.content)
```

O programa substitui a variável do template, envia o prompt ao modelo de linguagem e exibe o conteúdo retornado no terminal.

## Como executar o projeto

### 1. Clonar o repositório

```bash
git clone https://github.com/jrm0316/Assistente-Educacional-Interativo-com-IA-CLI-LangChain.git
```

Entre no diretório:

```bash
cd Assistente-Educacional-Interativo-com-IA-CLI-LangChain
```

### 2. Criar o ambiente virtual

Recomenda-se utilizar Python 3.12.

No Windows, execute:

```powershell
py -3.12 -m venv .venv
```

Ative o ambiente virtual:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Instalar as dependências

```powershell
python -m pip install langchain-groq langchain-core python-dotenv
```

### 4. Configurar a chave da Groq

Crie um arquivo `.env` na raiz do projeto:

```env
GROQ_API_KEY=sua_chave_da_api_aqui
```

A chave pode ser obtida no [Groq Console](https://console.groq.com/).

### 5. Executar o programa

```powershell
python ".\nivel5_Prompt+input_do_usuario_OK.py"
```

O programa solicitará um tema:

```text
Digite um tema para a IA explicar:
```

Digite, por exemplo:

```text
O que é uma variável em Python?
```

Pressione Enter e aguarde a resposta.

É necessária conexão com a internet e uma chave válida da Groq API.

## Testes funcionais realizados

O projeto foi testado no Windows, utilizando o terminal PowerShell do Cursor, com ambiente virtual Python 3.12.

### Teste 1 — Explicação sobre FastAPI

**Entrada:**

```text
FastAPI
```

**Resultado:**

A IA gerou uma explicação abrangente sobre o framework, incluindo:

- Definição e aplicações do FastAPI.
- Instalação e criação de uma API.
- Rotas e métodos HTTP.
- Validação de dados com Pydantic.
- Documentação automática.
- Exemplos de código.

**Status:** aprovado.

O teste confirmou a captura do tema, a comunicação com a Groq e a geração de uma resposta educacional.

### Teste 2 — Variáveis em Python

**Entrada:**

```text
O que é uma variável em Python?
```

**Resultado:**

A IA apresentou uma explicação com analogias, exemplos de atribuição, tipos de dados e regras de nomenclatura.

**Status:** aprovado.

O teste confirmou que o programa também consegue abordar conceitos básicos de programação.

A resposta, entretanto, ficou mais extensa do que o necessário para uma explicação introdutória.

### Teste 3 — Prompt educacional estruturado

Após ajustar o `PromptTemplate`, o programa foi executado novamente com o tema:

```text
O que é uma variável em Python?
```

**Resultado:**

A IA organizou a explicação nas quatro partes solicitadas:

1. O que é?
2. Para que serve?
3. Exemplo prático.
4. Resumo final.

A resposta ficou mais objetiva e adequada ao formato educacional desejado.

**Status:** aprovado.

O teste demonstrou como alterações nas instruções do prompt podem influenciar a organização e o nível de detalhamento das respostas.

## Conceitos demonstrados

### Entrada interativa via CLI

O programa utiliza `input()` para receber informações durante a execução, permitindo que o usuário escolha o assunto a ser explicado.

### Prompt Engineering

As instruções do prompt definem o papel educacional do modelo, o público-alvo e a organização desejada para a resposta.

### PromptTemplate

O template permite separar o texto de instrução do valor informado pelo usuário, tornando a construção do prompt reutilizável.

### Integração com LLM

A aplicação utiliza o LangChain para enviar prompts ao modelo de linguagem por meio da Groq API.

### Geração dinâmica de conteúdo

As explicações são geradas conforme o tema solicitado, sem depender de respostas previamente cadastradas.

## Limitações atuais

- A aplicação funciona exclusivamente pelo terminal.
- Processa um tema por execução.
- Não mantém histórico de conversas.
- Não possui memória contextual entre perguntas.
- Não utiliza banco de dados.
- Não implementa arquitetura RAG ou busca vetorial.
- Não possui interface web ou API REST.
- Não realiza validação automática do conteúdo educacional gerado.
- Não garante que o modelo siga sempre a estrutura solicitada.
- Não implementa tratamento específico para falhas de conexão ou indisponibilidade da API.
- As respostas podem conter informações imprecisas ou desatualizadas.

## Possíveis melhorias futuras

- Permitir várias perguntas durante a mesma execução.
- Implementar histórico de conversas.
- Adicionar níveis de dificuldade para diferentes públicos.
- Permitir selecionar o estilo da explicação.
- Criar exercícios e perguntas de revisão.
- Implementar tratamento de erros.
- Adicionar validação estruturada das respostas.
- Desenvolver uma interface web utilizando Streamlit ou FastAPI.
- Integrar documentos educacionais por meio de RAG.

Essas funcionalidades representam possibilidades de evolução e não fazem parte da implementação atual.

## Relevância técnica

O projeto demonstra a criação de uma aplicação interativa com IA, utilizando Python, LangChain e um modelo de linguagem para responder dinamicamente a solicitações do usuário.

Além da integração com LLMs, o projeto explora a importância do Prompt Engineering na definição do comportamento da IA e na organização das respostas.

Esses conceitos podem ser utilizados como base para assistentes educacionais, ferramentas internas e aplicações backend com Inteligência Artificial.

## Autor

**Juliano Rodrigues Madeira**

Projeto desenvolvido para estudo e aplicação prática de Python, LangChain, Prompt Engineering e integração com modelos de linguagem.

GitHub: [jrm0316](https://github.com/jrm0316)
