Cours

Créer des applications d’IA agentique est aujourd’hui bien plus simple qu’il y a quelques années. Plutôt que de concevoir de zéro des boucles d’agent, des intégrations d’outils, la mémoire, la récupération et l’orchestration, les développeurs peuvent s’appuyer sur des frameworks Python qui intègrent déjà l’essentiel de ces composants.

J’ai utilisé bon nombre des frameworks présentés dans cet article pour concevoir des applications RAG, des agents autonomes, des systèmes multi-agents et des outils d’IA déployables.

J’utilise souvent LangChain pour des workflows d’agents flexibles, LlamaIndex et Haystack pour des applications connectées aux données, ainsi que le SDK OpenAI Agents ou Google ADK lorsque je veux intégrer rapidement des modèles fournis par ces éditeurs.

Certains de ces frameworks ont tellement progressé qu’une solution sur mesure est souvent inutile. Avec une clé d’API, quelques crédits de modèles et le bon framework, vous pouvez créer un agent, le connecter à des outils ou à des données privées, le tester, puis le déployer via une API ou une interface utilisateur.

Cet article compare 15 frameworks Python répartis en cinq catégories : frameworks d’agents généralistes, SDK officiels d’agents, frameworks d’orchestration multi-agents, frameworks orientés données et RAG, et frameworks légers pour modèles ouverts.

Chaque section inclut également un court exemple Python à copier-coller pour comprendre rapidement le fonctionnement du framework.

## Frameworks d’agents généralistes

Ces frameworks fournissent des blocs de construction complets pour créer des agents, connecter des outils, gérer l’état et piloter des workflows en plusieurs étapes.

### 1. LangChain

Pour de nombreux développeurs, le développement moderne d’applications LLM a commencé avec LangChain, un framework Python open source conçu pour connecter les modèles de langage à des données externes, des outils, des API et de la logique applicative. Il a été l’un des premiers frameworks largement adoptés pour utiliser l’API OpenAI avec des sites web, des documents, des bases vectorielles et d’autres sources de données afin de créer des applications plus contextuelles.

Aujourd’hui, LangChain a évolué en un écosystème complet pour créer des applications d’IA agentique de bout en bout, incluant des systèmes RAG, des agents capables d’utiliser des outils, des workflows, des intégrations, du monitoring et de l’évaluation. Je vous recommande de consulter le parcours AI Engineering with LangChain pour aller plus loin.

L’exemple suivant crée un agent LangChain simple capable d’utiliser un outil de recherche dans la documentation pour trouver des informations pertinentes avant de répondre.

```
# pip install -U langchain "langchain[openai]"
from langchain.agents import create_agent
def search_docs(query: str) -> str:
    """Search the company documentation."""
    return f"Documentation found for: {query}"
agent = create_agent(
    model="openai:gpt-5.5",
    tools=[search_docs],
    system_prompt="Use the documentation tool when needed.",
)
result = agent.invoke({
    "messages": [{
        "role": "user",
        "content": "What is our remote-work policy?"
    }]
})
print(result["messages"][-1].content)
```
### 2. LangGraph

Si LangChain facilite la création d’agents et d’outils, LangGraph apporte davantage de contrôle sur la façon dont ces agents fonctionnent. Il permet de structurer l’application comme un graphe d’étapes connectées, ce qui simplifie la maîtrise des boucles, des appels d’outils, de l’état partagé, des validations humaines et des workflows multi-agents.

Je le trouve particulièrement utile pour des agents autonomes qui doivent itérer : décider de la prochaine action, choisir le bon outil, vérifier le résultat et poursuivre jusqu’à ce que la tâche soit terminée. LangGraph gère aussi la persistance, le streaming, les checkpoints et la reprise pour les applications longues. Notre tutoriel LangGraph donne plus de détails.

L’exemple suivant crée un agent qui peut appeler à plusieurs reprises des outils de calcul jusqu’à disposer de suffisamment d’informations pour répondre :

```
# pip install -U langgraph langchain langchain-openai
from langchain.chat_models import init_chat_model
from langchain.tools import tool
from langgraph.graph import MessagesState, StateGraph, START
from langgraph.prebuilt import ToolNode, tools_condition
model = init_chat_model("openai:gpt-5.5", temperature=0)
@tool
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b
@tool
def multiply(a: int, b: int) -> int:
    """Multiply two numbers."""
    return a * b
tools = [add, multiply]
model_with_tools = model.bind_tools(tools)
def call_model(state: MessagesState):
    response = model_with_tools.invoke(state["messages"])
    return {"messages": [response]}
builder = StateGraph(MessagesState)
builder.add_node("agent", call_model)
builder.add_node("tools", ToolNode(tools))
builder.add_edge(START, "agent")
builder.add_conditional_edges("agent", tools_condition)
builder.add_edge("tools", "agent")
agent = builder.compile()
result = agent.invoke({
    "messages": [{
        "role": "user",
        "content": "Add 12 and 8, then multiply the result by 3."
    }]
})
print(result["messages"][-1].content)
```
Le graphe envoie la requête au modèle, exécute les outils nécessaires, puis reboucle vers le modèle jusqu’à produire une réponse finale.

### 3. Agno

Agno est un framework Python pour créer des agents, des équipes multi-agents et des workflows structurés. Il inclut aussi AgentOS, qui aide à exposer les agents via des API, stocker les sessions et traces, et gérer le tout en production. Comme il peut tourner sur votre propre infrastructure, les organisations gardent un meilleur contrôle sur leurs données et leur sécurité.

L’exemple suivant crée un agent de recherche capable d’explorer le web avant de produire une réponse concise.

```
# pip install -U agno ddgs openai
from agno.agent import Agent
from agno.models.openai import OpenAIResponses
from agno.tools.duckduckgo import DuckDuckGoTools
agent = Agent(
    name="AI Research Assistant",
    model=OpenAIResponses(id="gpt-5.5"),
    tools=[DuckDuckGoTools()],
    instructions="Search when needed and keep the answer concise.",
)
agent.print_response(
    "Find one recent development in open-source AI and summarize it.",
    stream=True,
)
```
L’agent utilise l’outil de recherche pour obtenir des informations à jour et diffuse un court résumé à l’utilisateur.

### 4. Pydantic AI

Pydantic AI est un framework Python-native, créé par l’équipe derrière Pydantic, pour construire des agents et des applications d’IA générative. L’expérience rappelle celle des modèles Pydantic ou de FastAPI : agents, outils, dépendances et sorties sont définis via des fonctions Python standards, des hints de types, des décorateurs et des classes BaseModel. Le framework s’appuie ensuite sur la validation Pydantic pour fiabiliser les arguments des outils et les sorties des modèles.

Je vous recommande le tutoriel Pydantic AI pour découvrir le framework.

L’exemple suivant crée un agent qui renvoie un plan d’article validé sous forme de modèle Pydantic.

```
# pip install -U pydantic-ai
from pydantic import BaseModel, Field
from pydantic_ai import Agent
class ArticlePlan(BaseModel):
    title: str
    key_points: list[str]
    reading_time_minutes: int = Field(ge=1)
agent = Agent(
    "openai:gpt-5.6-sol",
    output_type=ArticlePlan,
    instructions="Create concise article plans for technical readers.",
)
result = agent.run_sync(
    "Create a short article plan about building AI agents in Python."
)
print(result.output)
```
L’agent génère un objet ArticlePlan et valide chaque champ selon les types et règles Python requis avant de le retourner.

## SDK officiels d’agents

Les SDK officiels sont maintenus par les fournisseurs d’IA et offrent généralement l’intégration la plus simple avec leurs modèles, API, outils, systèmes de déploiement et plateformes d’observabilité.

### 5. OpenAI Agents SDK

Le SDK OpenAI Agents est un framework léger, pensé d’abord pour Python permettant de créer des applications mono-agent et multi-agents. Il fournit une boucle d’agent intégrée, des outils de fonction, des handoffs, des garde-fous, des sessions, des validations humaines et du tracing, sans surcharger d’abstractions.

J’apprécie particulièrement utiliser l’OpenAI Agents SDK car il est simple, rapide à intégrer et facile à comprendre. J’ai construit de nombreuses applications avec, sans problème majeur. Il crée également automatiquement des traces visibles dans le Dashboard OpenAI, ce qui facilite l’inspection des réponses du modèle, des appels d’outils, des handoffs et du flot d’exécution complet.

L’exemple suivant crée un agent d’histoire capable d’appeler une fonction Python pour récupérer un fait historique surprenant.

```
# pip install openai-agents
import asyncio
from agents import Agent, Runner, function_tool
@function_tool
def history_fun_fact() -> str:
    """Return a surprising historical fact."""
    return "The first computer programmer, Ada Lovelace, lived in the 1800s."
agent = Agent(
    name="History Assistant",
    instructions=(
        "Answer history questions clearly and briefly. "
        "Use history_fun_fact when it is helpful."
    ),
    tools=[history_fun_fact],
)
async def main():
    result = await Runner.run(
        agent,
        "Tell me something surprising about the history of computing.",
    )
    print(result.final_output)
if __name__ == "__main__":
    asyncio.run(main())
```
Le SDK exécute l’agent, appelle l’outil d’histoire si nécessaire, renvoie son résultat au modèle et produit la réponse finale. L’exécution complète peut aussi être inspectée via le trace viewer.

### 7. Google Agent Development Kit

L’Agent Development Kit de Google, ou ADK, est un framework open source pour créer, évaluer et déployer des agents individuels, des assistants utilisant des outils, des workflows en graphe et des systèmes multi-agents. Il fonctionne particulièrement bien avec les modèles Gemini, tout en prenant en charge d’autres fournisseurs.

ADK est devenu l’un de mes frameworks favoris, car il est simple et s’intègre naturellement avec Gemini.

Je l’ai utilisé pour créer plusieurs applications sans problème majeur. Même si je trouve encore le SDK OpenAI Agents plus simple à intégrer et plus flexible en matière de contrôles et de fonctionnalités natives, ADK est une excellente alternative, particulièrement compétitive pour des applications basées sur Gemini.

L’exemple suivant crée un assistant d’événements capable d’appeler une fonction Python pour récupérer l’heure de début d’un événement.

```
# pip install google-adk
from google.adk.agents.llm_agent import Agent
def get_event_time(city: str) -> dict:
    """Return the event starting time for a city."""
    return {
        "status": "success",
        "city": city,
        "time": "6:00 PM",
    }
root_agent = Agent(
    model="gemini-flash-latest",
    name="event_assistant",
    description="Provides information about events in different cities.",
    instruction=(
        "Answer event questions clearly. "
        "Use the get_event_time tool when the starting time is requested."
    ),
    tools=[get_event_time],
)
```
L’agent utilise la fonction comme un outil dès qu’il doit récupérer l’horaire d’un événement pour une ville donnée.

### 8. Claude Agent SDK

Le Claude Agent SDK est le framework Python et TypeScript d’Anthropic pour créer des agents autonomes en s’appuyant sur la même boucle d’agent, les mêmes outils et la même gestion du contexte que ceux de Claude Code. Il est particulièrement utile pour des agents devant lire des fichiers, éditer du code, exécuter des commandes, se connecter à des outils MCP et mener à bien des tâches longues.

Anthropic, OpenAI et Google intègrent étroitement leurs modèles à leurs propres frameworks d’agents, ce qui accélère la mise en place des outils, des sessions, du tracing et d’autres fonctionnalités avancées.

Cependant, le Claude Agent SDK est conçu spécifiquement pour Claude plutôt que pour des modèles open source ou hébergés localement. Vous pouvez en savoir plus dans notre tutoriel Claude Agent SDK.

L’exemple suivant crée un agent de code qui passe en revue un fichier Python, identifie les bugs et les corrige automatiquement.

```
# pip install claude-agent-sdk
import asyncio
from claude_agent_sdk import (
    AssistantMessage,
    ClaudeAgentOptions,
    ResultMessage,
    query,
)
async def main():
    async for message in query(
        prompt="Review app.py for errors that could cause crashes and fix them.",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit", "Glob"],
            permission_mode="acceptEdits",
        ),
    ):
        if isinstance(message, AssistantMessage):
            for block in message.content:
                if hasattr(block, "text"):
                    print(block.text)
                elif hasattr(block, "name"):
                    print(f"Tool used: {block.name}")
        elif isinstance(message, ResultMessage):
            print(f"Completed: {message.subtype}")
if __name__ == "__main__":
    asyncio.run(main())
```
Le SDK exécute la boucle de l’agent pendant que Claude lit le fichier, sélectionne les outils requis, modifie le code et diffuse sa progression jusqu’à la fin de la tâche.

## Frameworks d’orchestration multi-agents

Les frameworks multi-agents coordonnent plusieurs agents spécialisés, chacun étant assigné à un rôle, un objectif, un ensemble d’outils ou une étape d’un workflow plus large.

### 9. CrewAI

CrewAI est un framework Python populaire pour créer des équipes multi-agents. Les développeurs définissent des agents avec différents rôles, objectifs et tâches, puis les combinent au sein d’une équipe qui collabore de manière autonome. Les tâches peuvent s’enchaîner séquentiellement ou suivre un processus hiérarchique où un manager coordonne et délègue le travail aux agents les plus adaptés.

L’exemple Python ci-dessous crée un chercheur et un rédacteur qui collaborent pour produire un court rapport.

```
# pip install crewai
# Set OPENAI_API_KEY before running
from crewai import Agent, Crew, Process, Task
researcher = Agent(
    role="AI Researcher",
    goal="Find the key facts about {topic}",
    backstory="You research technical topics carefully.",
)
writer = Agent(
    role="Technical Writer",
    goal="Turn research into a clear summary",
    backstory="You explain complex topics simply.",
)
research_task = Task(
    description="Research {topic} and identify three key findings.",
    expected_output="Three concise findings.",
    agent=researcher,
)
writing_task = Task(
    description="Write a short summary using the research findings.",
    expected_output="A clear one-paragraph report.",
    agent=writer,
    context=[research_task],
)
crew = Crew(
    agents=[researcher, writer],
    tasks=[research_task, writing_task],
    process=Process.sequential,
)
result = crew.kickoff(inputs={"topic": "agentic AI"})
print(result.raw)
```
Le chercheur termine la première tâche, puis le rédacteur s’appuie sur sa sortie comme contexte pour rédiger le rapport final.

### 10. MetaGPT

MetaGPT est un framework multi-agents qui simule une entreprise logicielle via des agents spécialisés tels qu’un product manager, un architecte, un chef de projet et un ingénieur. Ces agents suivent des procédures opératoires structurées pour transformer un besoin succinct en plans, conceptions techniques, documentation et code opérationnel.

L’exemple suivant crée une équipe logicielle et lui demande de construire un simple gestionnaire de tâches en ligne de commande.

```
# pip install metagpt
# Configure a supported LLM API before running
import asyncio
from metagpt.roles import (
    Architect,
    Engineer,
    ProductManager,
    ProjectManager,
)
from metagpt.team import Team
async def startup(idea: str):
    company = Team()
    company.hire([
        ProductManager(),
        Architect(),
        ProjectManager(),
        Engineer(),
    ])
    company.invest(investment=3.0)
    company.run_project(idea=idea)
    await company.run(n_round=5)
if __name__ == "__main__":
    asyncio.run(
        startup("Build a simple command-line task manager")
    )
```
Les agents se répartissent les exigences selon leurs rôles et collaborent pour planifier et développer le projet logiciel.

### 11. AgentScope

AgentScope est un framework Python pour créer des agents et des applications multi-agents contrôlables et observables. Il propose des agents prêts à l’emploi, des outils, de la mémoire, du routage de messages, du streaming, des appels d’outils en parallèle, des workflows, du tracing et l’intervention humaine. AgentScope Studio permet également d’inspecter et de visualiser l’exécution des agents.

L’exemple suivant crée un agent ReAct capable d’écrire et d’exécuter du code Python pour réaliser un calcul.

```
# pip install agentscope
# Set DASHSCOPE_API_KEY before running
import asyncio
import os
from agentscope.agent import ReActAgent
from agentscope.formatter import DashScopeChatFormatter
from agentscope.memory import InMemoryMemory
from agentscope.message import Msg
from agentscope.model import DashScopeChatModel
from agentscope.tool import Toolkit, execute_python_code
async def main():
    toolkit = Toolkit()
    toolkit.register_tool_function(execute_python_code)
    agent = ReActAgent(
        name="Nova",
        sys_prompt="You are a helpful Python assistant named Nova.",
        model=DashScopeChatModel(
            model_name="qwen-max",
            api_key=os.environ["DASHSCOPE_API_KEY"],
            stream=True,
        ),
        formatter=DashScopeChatFormatter(),
        toolkit=toolkit,
        memory=InMemoryMemory(),
    )
    await agent(
        Msg(
            name="user",
            content="Use Python to calculate the sum of numbers from 1 to 100.",
            role="user",
        )
    )
if __name__ == "__main__":
    asyncio.run(main())
```
L’agent décide d’appeler l’outil d’exécution Python, lance le code généré et utilise le résultat pour répondre à l’utilisateur.

### 12. CAMEL-AI

CAMEL-AI est un framework Python open source pour créer des agents, des sociétés multi-agents et des simulations de rôles. Très orienté recherche, il offre des composants pour la collaboration entre agents, les outils, la mémoire, la récupération, la génération de données et la simulation de mondes. Il prend en charge les modèles cloud et les modèles hébergés localement via des plateformes comme Ollama, vLLM et SGLang.

L’exemple suivant crée un agent capable de rechercher sur le web avant de répondre à une question.

```
# pip install "camel-ai[web_tools]"
# Set OPENAI_API_KEY before running
from camel.agents import ChatAgent
from camel.models import ModelFactory
from camel.toolkits import SearchToolkit
from camel.types import ModelPlatformType, ModelType
model = ModelFactory.create(
    model_platform=ModelPlatformType.OPENAI,
    model_type=ModelType.GPT_5_5,
    model_config_dict={"temperature": 0.0},
)
agent = ChatAgent(
    system_message="You are a helpful AI research assistant.",
    model=model,
    tools=[SearchToolkit().search_duckduckgo],
)
response = agent.step(
    "What are the main uses of multi-agent AI systems?"
)
print(response.msgs[0].content)
```
L’agent utilise la recherche DuckDuckGo lorsqu’il a besoin d’informations à jour, puis renvoie la réponse finale.

## Frameworks orientés données et RAG

Ces frameworks connectent les LLM et les agents à des documents, bases de données, API et autres sources de données privées pour récupérer un contexte pertinent avant de répondre.

### 13. LlamaIndex

LlamaIndex est un framework Python pour créer des applications sensibles au contexte sur des données privées ou métier. Il propose des connecteurs, index, retrieveurs, moteurs de requêtes, agents et workflows pour des cas tels que le RAG, la recherche d’entreprise et les assistants de documents.

LlamaIndex a été l’un de mes frameworks préférés lorsque j’ai commencé à développer des applications RAG. Il simplifie énormément le chargement, l’indexation et l’interrogation des données, et je trouve souvent son code plus simple et plus court que l’équivalent avec LangChain. Vous pouvez suivre le cours LlamaIndex pour en savoir plus.

L’exemple suivant charge des documents depuis un dossier, crée un index interrogeable et répond à une question en s’appuyant sur le contexte récupéré.

```
# pip install llama-index
# Set OPENAI_API_KEY before running
from llama_index.core import SimpleDirectoryReader, VectorStoreIndex
documents = SimpleDirectoryReader("data").load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
response = query_engine.query(
    "What are the main findings in these documents?"
)
print(response)
```
LlamaIndex indexe les fichiers du dossier data et récupère les informations pertinentes avant de générer la réponse.

### 14. Haystack

Haystack est un framework Python open source pour créer des pipelines RAG prêts pour la production, des systèmes de recherche sémantique, des agents et des applications d’IA centrées sur la donnée. Sa structure modulaire en pipeline offre un contrôle clair sur la récupération d’information, son insertion dans le prompt et l’envoi au modèle.

J’ai utilisé Haystack pour construire des applications RAG et multi-agents sans problème majeur. Bien qu’il soit généraliste, il est particulièrement adapté aux applications qui relient les LLM à des documents, du texte, des systèmes de recherche et d’autres sources de données.

L’exemple suivant stocke quelques documents, récupère le plus pertinent et s’en sert pour répondre à une question.

```
# pip install -U haystack-ai
# Set OPENAI_API_KEY before running
from haystack import Document, Pipeline
from haystack.components.builders import ChatPromptBuilder
from haystack.components.generators.chat import (
    OpenAIResponsesChatGenerator,
)
from haystack.components.retrievers import InMemoryBM25Retriever
from haystack.dataclasses import ChatMessage
from haystack.document_stores.in_memory import InMemoryDocumentStore
document_store = InMemoryDocumentStore()
document_store.write_documents([
    Document(content="The London AI event starts at 6:00 PM."),
    Document(content="The Berlin AI event starts at 7:00 PM."),
])
template = [
    ChatMessage.from_system(
        "Answer using the following documents:\n"
        "{% for doc in documents %}{{ doc.content }}{% endfor %}"
    ),
    ChatMessage.from_user("{{ question }}"),
]
pipeline = Pipeline()
pipeline.add_component(
    "retriever",
    InMemoryBM25Retriever(document_store),
)
pipeline.add_component(
    "prompt_builder",
    ChatPromptBuilder(template=template),
)
pipeline.add_component(
    "llm",
    OpenAIResponsesChatGenerator(model="gpt-5.5"),
)
pipeline.connect("retriever", "prompt_builder.documents")
pipeline.connect("prompt_builder", "llm")
question = "When does the London AI event start?"
result = pipeline.run({
    "retriever": {"query": question},
    "prompt_builder": {"question": question},
})
print(result["llm"]["replies"][0].text)
```
Haystack récupère le document correspondant, l’ajoute au prompt et utilise GPT-5.5 pour produire une réponse étayée.

## Frameworks légers et modèles ouverts

Ces frameworks proposent des abstractions plus simples pour créer des agents avec des modèles ouverts hébergés ou locaux.

### 15. Hugging Face smolagents

Hugging Face smolagents est un framework Python léger pour créer des agents avec très peu de code. Son CodeAgent agit en écrivant du Python, tandis que ToolCallingAgent utilise des appels d’outils structurés. Il fonctionne particulièrement bien avec les modèles Hugging Face, les Inference Providers et les modèles open source hébergés localement.

L’exemple suivant crée un agent de recherche web, puis le confie à un agent manager qui décide quand déléguer la recherche.

```
# pip install -U "smolagents[toolkit]"
# Set HF_TOKEN before running
from smolagents import (
    CodeAgent,
    InferenceClientModel,
    ToolCallingAgent,
    WebSearchTool,
)
model = InferenceClientModel(
    model_id="Qwen/Qwen3-Next-80B-A3B-Thinking"
)
web_agent = ToolCallingAgent(
    tools=[WebSearchTool()],
    model=model,
    name="web_search_agent",
    description="Searches the web and returns useful information.",
)
manager_agent = CodeAgent(
    tools=[],
    model=model,
    managed_agents=[web_agent],
)
result = manager_agent.run(
    "Who created Hugging Face, and when was it founded?"
)
print(result)
```
Le manager décide quand il a besoin d’informations à jour, délègue la recherche à l’agent web spécialiste, puis produit la réponse finale.

## Comparatif rapide des frameworks

Le tableau ci-dessous compare la principale force de chaque framework et le type d’application d’IA agentique pour lequel il est le plus adapté.

| Framework | Atout principal | À choisir lorsque | 
| LangChain | Large écosystème d’intégrations | Vous avez besoin d’intégrations étendues, d’outils et de flexibilité | 
| LangGraph | Orchestration stateful par graphe | Vous avez besoin de boucles contrôlées, d’exécutions durables et de workflows complexes | 
| Agno | Plateforme d’agents complète | Vous voulez créer, auto-héberger et gérer des services d’agents | 
| Pydantic AI | Développement Python avec typage sûr | Vous avez besoin d’outils validés, de dépendances et de sorties structurées | 
| OpenAI Agents SDK | Développement d’agents et tracing simplifiés | Vous utilisez principalement les modèles OpenAI et voulez intégrer rapidement | 
| Google ADK | Développement natif pour Gemini | Vous utilisez Gemini, Google Cloud ou des workflows multi-agents | 
| Claude Agent SDK | Outils pour ordinateur, fichiers et commandes | Vous concevez des agents de code, de recherche ou longue durée avec Claude | 
| CrewAI | Équipes d’agents basées sur les rôles | Plusieurs agents spécialisés doivent collaborer sur un projet | 
| MetaGPT | Simulation d’entreprise logicielle | Vous voulez que des agents planifient et génèrent des projets logiciels complets | 
| AgentScope | Agents observables et contrôlables | Vous avez besoin d’agents traçables ou d’applications multi-agents | 
| CAMEL-AI | Recherche et simulation multi-agents | Vous étudiez le role-play, les sociétés d’agents ou de grands systèmes d’agents | 
| LlamaIndex | Applications connectées aux données et sensibles au contexte | Vos agents doivent récupérer et raisonner sur des documents privés | 
| Haystack | Pipelines RAG modulaires et transparents | Vous voulez contrôler la récupération, le prompting et le flux de données | 
| smolagents | Agents légers pour modèles ouverts | Vous souhaitez des abstractions minimales, des code agents ou le support de modèles locaux | 

## Derniers conseils

Si vous débutez avec l’IA agentique, commencez par un SDK officiel comme l’OpenAI Agents SDK, Google ADK ou le Claude Agent SDK. Ils fonctionnent de près avec leurs modèles et API, donc la mise en route est généralement simple. Ajoutez une clé d’API, créditez votre compte, et commencez à construire.

J’apprécie ces SDK car ils facilitent la création d’agents, la connexion d’outils et l’inspection de ce qui s’est passé pendant une exécution. Avec quelques lignes de Python, vous pouvez créer un agent utile, voire un petit système multi-agents.

Quand j’ai besoin de plus de flexibilité, je me tourne généralement vers LangChain ou LangGraph. LangChain est utile lorsque j’ai besoin de nombreuses intégrations, tandis que LangGraph m’offre plus de contrôle sur l’état, les boucles et les workflows longs.

Pour des équipes d’agents aux rôles variés, CrewAI est une bonne option. Pour le RAG, les documents privés et les applications connectées aux données, je privilégie LlamaIndex ou Haystack.

Enfin, smolagents est un excellent choix quand je veux quelque chose de léger, simple et facile à utiliser avec des modèles ouverts ou locaux.

Mon conseil est de démarrer avec le framework le plus simple qui réponde à votre besoin. Créez une version minimale, testez-la correctement, et n’ajoutez de complexité que lorsque c’est réellement nécessaire.

## FAQs

### Combien coûte l’exécution d’un agent d’IA par rapport à un prompt LLM standard ?

Si les frameworks Python eux-mêmes sont open source et gratuits, exécuter des agents peut coûter sensiblement plus cher que des appels API LLM standards. Les agents fonctionnent en boucle, renvoyant sans cesse au modèle le system prompt, les descriptions d’outils, les sorties d’outils précédentes et les étapes de raisonnement (comme ReAct) : la consommation de tokens s’accumule très vite. Une seule requête utilisateur peut déclencher cinq ou six appels LLM avant que l’agent n’aboutisse à une réponse finale. Pour maîtriser les coûts, les développeurs utilisent souvent des modèles plus petits et moins chers pour les tâches simples de routage, et réservent les modèles plus puissants au raisonnement complexe.

### Comment évaluer si mon agent fonctionne réellement bien ?

Tester des agents non déterministes demande plus que des tests unitaires classiques. Les développeurs recourent à des frameworks d’évaluation spécialisés de type « LLM-as-a-judge », comme **Ragas**, **TruLens** ou **DeepEval**. Ces outils exécutent votre agent sur un jeu de questions de test et notent les sorties selon des métriques spécifiques, par exemple :

- **Ancrage factuel :** l’agent a-t-il halluciné, ou la réponse repose-t-elle uniquement sur le contexte récupéré ?
- **Exactitude de la sélection d’outils :** l’agent a-t-il choisi l’outil approprié ?
- **Pertinence de la réponse :** la réponse finale traite-t-elle réellement la demande de l’utilisateur ?

### Quelle est la différence entre un pipeline RAG standard (LlamaIndex/Haystack) et un système « Agentic » RAG ?

Un pipeline RAG standard suit un chemin statique et codé en dur : il prend la requête utilisateur, interroge une base vectorielle, injecte les résultats dans un prompt et génère une réponse. Il s’exécute une fois puis s’arrête. Un système **Agentic RAG** donne à l’LLM de l’autonomie sur le processus de récupération. L’agent peut décider *s’il* doit rechercher ou non, générer plusieurs requêtes de recherche différentes pour élargir le contexte, évaluer l’utilité des documents récupérés et choisir de rechercher à nouveau si l’information est incomplète, avant de répondre finalement à l’utilisateur.

En tant que data scientist certifié, je suis passionné par l'utilisation des technologies de pointe pour créer des applications innovantes d'apprentissage automatique. Avec une solide expérience en reconnaissance vocale, en analyse de données et en reporting, en MLOps, en IA conversationnelle et en NLP, j'ai affiné mes compétences dans le développement de systèmes intelligents qui peuvent avoir un impact réel. En plus de mon expertise technique, je suis également un communicateur compétent, doué pour distiller des concepts complexes dans un langage clair et concis. En conséquence, je suis devenu un blogueur recherché dans le domaine de la science des données, partageant mes idées et mes expériences avec une communauté grandissante de professionnels des données. Actuellement, je me concentre sur la création et l'édition de contenu, en travaillant avec de grands modèles linguistiques pour développer un contenu puissant et attrayant qui peut aider les entreprises et les particuliers à tirer le meilleur parti de leurs données.