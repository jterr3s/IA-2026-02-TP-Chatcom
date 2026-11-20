# Declaração de uso de Inteligência Artificial generativa

> **Regra do trabalho:** usar IA generativa é **permitido e incentivado**, mas
> **todo e qualquer uso** precisa estar declarado e detalhado em uma seção própria
> do artigo e na apresentação (vídeo). Uso não declarado é considerado **plágio** e
> leva à **nota zero** no trabalho para todo o grupo.

## 1. Ferramentas usadas

| Ferramenta (nome e versão/modelo) | Como acessaram | Quem usou |
|---|---|---|
| OpenRouter, modelo `openai/gpt-6-luna` | Chave da API do grupo, via scripts Python (`chatbot/llm.py` e `scripts/gerar_dados_llm.py`) | Todos os integrantes |
| Google Gemini | Interface web (navegador) | [Natan Alves Teodoro da Silva, Vinícius Fialho Cominetti] |

## 2. Usos, um por linha

| # | Etapa | O que a IA fez | Prompt (ou resumo + link para o registro) | O que o grupo fez com o resultado |
|---|---|---|---|---|
| 1 | Dados de treino | Gerou variações sintéticas de perguntas para compor o `dados/treino.csv` | Prompts estruturados por intenção via `scripts/gerar_dados_llm.py` | Revisamos manualmente todas as frases geradas, descartando duplicatas/ambíguas e corrigindo rótulos antes de treinar os modelos |
| 2 | Código e Classificadores (SVM e Naive Bayes) | Sugeriu exemplos de uso das bibliotecas do `scikit-learn` (`LinearSVC`, `CalibratedClassifierCV`, `MultinomialNB`, `TfidfVectorizer`) e estruturação da busca de hiperparâmetros (`GridSearchCV`) | Consultas sobre como configurar pipelines TF-IDF com SVM calibrado e Naive Bayes, e como montar a grade de hiperparâmetros (`C`, `alpha`, `ngram_range`) | Adaptamos o código ao notebook do projeto, executamos os experimentos de validação cruzada no `treino.csv` e selecionamos os modelos e parâmetros finais |
| 3 | Base de conhecimento (RAG) | Auxiliou na síntese e organização do resumo estruturado dos cursos atuais da FACOM (`cursos_graduacao_facom.txt`) | Solicitação para organizar a lista oficial dos 6 cursos vigentes da FACOM e seus códigos do sistema de ensino | Conferimos cada código e link diretamente nas grades oficiais da UFMS (`ensino.ufms.br`) e removemos PDFs duplicados do índice |
| 4 | Configuração, testes e documentação | Auxiliou na estruturação do `grupo.yaml`, depuração de comandos de avaliação/indexação e organização deste documento | Consultas iterativas para configurar classificadores, diagnosticar recuperação do RAG e revisar arquivos obrigatórios | Executamos e validamos todos os scripts localmente (`avaliar.py`, `conversar.py`, `indexar_documentos.py`), conferindo cada métrica e arquivo 
| 5 | Depuração de erros | Ajudou a diagnosticar e corrigir erros pontuais de execução e sintaxe ao rodar os scripts e o código do notebook | Envio de mensagens de erro/tracebacks do terminal e do Jupyter para identificar causas e ajustes | Aplicamos as correções, reexecutamos os scripts e validamos o funcionamento sem erros |

## 3. O que NÃO foi feito com IA

Foram realizados integralmente pelo grupo, sem geração automática por IA:
- A definição do escopo do chatbot (Chatcom), a escolha das 9 intenções de domínio e a definição da persona e das respostas fixas em `grupo.yaml`;
- A coleta, seleção e organização dos Projetos Pedagógicos de Curso (PPCs) e documentos oficiais da FACOM/UFMS na pasta `documentos/`;
- A coleta das frases reais do conjunto `dados/teste.csv` por meio de formulário aplicado a pessoas externas, bem como a rotulagem manual e a conferência de ausência de vazamento de dados com o treino;
- A decisão final pelos modelos vencedores de escopo e tema, a definição do limiar de confiança (`limiar_tema = 0.45`), a análise crítica dos erros ponta a ponta e as conclusões do trabalho.

## 4. Reflexão

A IA generativa acelerou significativamente a criação de variações linguísticas para o treinamento dos classificadores, a escrita de rotinas de busca de hiperparâmetros com `scikit-learn` (SVM e Naive Bayes) e a redação natural das respostas do chatbot baseadas nos PPCs da FACOM. Por outro lado, observamos na prática que o LLM pode alucinar ou misturar informações históricas quando o contexto recuperado contém documentos duplicados ou cita cursos já extintos (como Tecnologia em Redes de Computadores), além de cortar frases quando `max_tokens` é restrito. Para mitigar esses riscos, validamos todas as informações contra as grades oficiais da UFMS, limpamos duplicatas da base vetorial, testamos o código dos classificadores passo a passo no notebook e validamos tanto o modo integrado quanto o modo offline (`--sem-llm`).
