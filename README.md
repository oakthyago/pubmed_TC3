Normalização e Curadoria do PubMedQA para Fine-tuning Clínico
Visão geral

Este repositório documenta o processo de curadoria, normalização e preparação de dados clínicos a partir do dataset PubMedQA, com o objetivo de viabilizar o fine-tuning de um modelo de linguagem (LLM) para atuar como assistente médico de apoio à decisão, respeitando limites de segurança clínica.

O foco do trabalho não é ensinar conhecimento médico novo ao modelo, mas especializar o comportamento das respostas, tornando-as:

concisas,

objetivas,

clinicamente seguras,

baseadas exclusivamente em evidência textual fornecida.

Datasets utilizados
1. PubMedQA – PQAL (≈ 1.000 Q&A)

Subconjunto curado e balanceado

Possui rótulo final_decision (yes / no / maybe)

Foi o primeiro dataset normalizado

Utilizado como prova de conceito do pipeline

2. PubMedQA – A (Annotated) (≈ +5.000 Q&A)

Dataset annotated, com supervisão explícita

Mantém a mesma estrutura lógica:

QUESTION

CONTEXTS

LONG_ANSWER

LABELS

MESHES

final_decision

Está sendo processado com o mesmo pipeline de normalização, garantindo consistência metodológica

Observação: o dataset PubMedQA-U (Unlabeled) foi deliberadamente excluído desta fase por conter alto volume, ausência de rótulos e maior heterogeneidade textual. Seu uso é considerado apenas para etapas futuras de escala.

Problemas do dado bruto

Os dados originais do PubMedQA, embora cientificamente ricos, não são adequados para fine-tuning direto, pois apresentam:

respostas longas e narrativas;

mistura de métodos, resultados e discussão;

grande variação de estilo e tamanho;

ausência de padronização clínica;

ruído textual que prejudica o aprendizado do modelo.

Treinar diretamente sobre esse material induz o modelo a aprender prolixidade acadêmica, em vez de respostas clínicas úteis e seguras.

Estratégia de normalização

Cada par pergunta–resposta passou por um processo de reescrita controlada por LLM, com instruções explícitas para:

resumir a resposta de forma objetiva;

remover detalhes metodológicos irrelevantes;

não prescrever tratamentos;

não fornecer dosagens;

não emitir diagnósticos;

basear-se exclusivamente no contexto fornecido.

O resultado final de cada exemplo é:

{
  "pmid": "...",
  "question": "...",
  "normalized_answer": "...",
  "final_decision": "yes | no | maybe"
}


O pmid original é forçado pelo pipeline, evitando qualquer inconsistência gerada pelo modelo.

Por que dados normalizados são melhores para fine-tuning

Fine-tuning em LLMs modernos é mais eficaz quando utilizado para ajuste comportamental, e não para simples ingestão de texto.

A normalização:

reduz drasticamente o ruído;

aumenta a densidade de sinal clínico;

padroniza o estilo de resposta;

reforça limites de segurança;

melhora a convergência do treinamento.

Na prática:

1.000 exemplos bem normalizados são mais eficazes do que dezenas de milhares de exemplos brutos.

Volume de dados e justificativa

Até o momento:

~1.000 Q&A (PQAL) já normalizadas

~5.000 Q&A (PubMedQA-A) em normalização

Total planejado: ≈6.000 exemplos curados

Esse volume é:

suficiente para instruction tuning leve;

adequado para demonstrar especialização clínica;

compatível com custos controlados;

ideal para o escopo acadêmico do Tech Challenge – Fase 3.

Não há ganho proporcional em incluir dados não curados nesta etapa, especialmente em um contexto clínico sensível.

Controle de custo e reprodutibilidade

Os datasets brutos (ori_pqaa.json, etc.) são ignorados no Git (.gitignore)

Apenas:

scripts,

pipeline,

exemplos normalizados
são versionados

Isso garante:

reprodutibilidade,

rastreabilidade,

controle de custo,

organização do repositório

Próximos passos previstos

Finalização da normalização do PubMedQA-A

Avaliação qualitativa das respostas

Integração com pipeline de fine-tuning

Uso do modelo ajustado em fluxo com LangChain / LangGraph

Implementação de logging e validação humana

Conclusão

A abordagem adotada privilegia qualidade, segurança e controle, alinhando-se às boas práticas de IA aplicada à saúde. A normalização não é apenas uma etapa técnica, mas um componente central para garantir que o modelo treinado atue como assistente clínico confiável, e não como um agente prescritivo ou especulativo.