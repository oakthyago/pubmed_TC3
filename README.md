baixe [PQA-A](https://drive.google.com/open?id=15v1x6aQDlZymaHGP7cZJZZYFfeJt2NdS)

não necessita baixar o [PQA-U](https://drive.google.com/open?id=1RsGLINVce-0GsDkCLDuLZmoLuzfmoCuQ)

# PubMedQA – Curadoria e Normalização para Fine-tuning Clínico

Este repositório documenta o processo de **curadoria, normalização e preparação de dados clínicos** a partir do dataset **PubMedQA**, com foco no **fine-tuning de modelos de linguagem (LLMs)** para atuarem como **assistentes médicos de apoio à decisão**, respeitando limites de segurança clínica.

O projeto faz parte do **Tech Challenge – Fase 3** e prioriza **qualidade, controle e reprodutibilidade**, em vez de volume bruto de dados.

---

## Visão geral do PubMedQA

O PubMedQA é dividido em três subconjuntos principais:

| Split | Descrição |
|----|----|
| **PQA-L (Labeled)** | Subconjunto pequeno, balanceado e curado, usado para benchmark e avaliação |
| **PQA-A (Annotated)** | Conjunto maior, rotulado, com maior diversidade textual |
| **PQA-U (Unlabeled)** | Conjunto muito grande, sem rótulos, destinado a pré-treinamento |

Neste projeto, os datasets foram utilizados de forma **progressiva e controlada**.

---

## Etapa 1 — Normalização do PQA-L (≈ 1.000 Q&A)

O **PQA-L** foi o primeiro dataset processado por apresentar:
- alta qualidade,
- balanceamento entre `yes / no / maybe`,
- baixo custo computacional,
- maior controle experimental.

### Objetivo desta etapa
- Validar o pipeline de normalização
- Definir o padrão de resposta clínica
- Garantir segurança e consistência

Cada pergunta-resposta foi reescrita para gerar:

```json
{
  "pmid": "...",
  "question": "...",
  "normalized_answer": "...",
  "final_decision": "yes | no | maybe"
}
