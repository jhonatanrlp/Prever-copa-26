# Previsão Copa do Mundo 2026

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-orange)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Projeto de ciência de dados que explora o histórico de partidas e o ranking
FIFA das seleções para entender o que costuma diferenciar campeãs de Copa do
Mundo, e usa esses padrões para treinar um modelo que estima a probabilidade
de cada seleção ser campeã da Copa do Mundo de 2026.

## Sobre o projeto

O trabalho é dividido em duas etapas, uma por notebook:

1. **Análise exploratória** — consolidação dos históricos de títulos, finais,
   partidas e ranking FIFA em uma base única por seleção, usada para entender
   correlações (ex.: ranking FIFA vs. taxa de vitórias) antes de qualquer
   modelagem.
2. **Modelagem** — engenharia de features por seleção/ano, treino de um
   classificador para estimar a probabilidade de título, e aplicação desse
   modelo às eliminatórias da CONMEBOL e da UEFA para gerar um ranking de
   favoritas ao título de 2026.

Este é um projeto de estudo (ciclo básico de um curso de dados) — o objetivo é
praticar o fluxo completo de um problema de classificação (dados → features →
modelo → avaliação), não produzir uma previsão oficial ou definitiva.

## Dados

Os dados (histórico de futebol internacional, sem nenhuma informação
sensível ou pessoal) **não fazem parte deste repositório** — a pasta
`dados/` é local e ignorada pelo git (veja `.gitignore`). Para rodar os
notebooks, monte a pasta `dados/` na raiz do projeto com esta estrutura:

```
dados/
├── partidas/
│   ├── matches_1930_2022.csv
│   ├── decision.csv
│   ├── fifa_results.csv
│   └── penalty_kicks.csv
├── ranking/
│   ├── fifa_ranking_2022-10-06.csv
│   └── fifa_ranking-20XX-XX-XX.csv  (snapshots adicionais, opcionais)
└── copas/
    └── world_cup.csv
```

| Arquivo | Conteúdo |
| --- | --- |
| `copas/world_cup.csv` | Uma linha por edição da Copa do Mundo: sede, campeã, vice, artilheiro, público. |
| `partidas/matches_1930_2022.csv` | Resultados detalhados de todas as partidas de Copas do Mundo (1930–2022), incluindo xG, escalações e cartões. |
| `partidas/decision.csv` | Histórico amplo de partidas internacionais (mandante/visitante, torneio, cidade, campo neutro). |
| `partidas/fifa_results.csv` | Log de gols por partida (artilheiro, minuto, gol contra, pênalti). |
| `partidas/penalty_kicks.csv` | Resultados de disputas de pênaltis em partidas internacionais. |
| `ranking/fifa_ranking_2022-10-06.csv` | Snapshot do ranking FIFA (posição e pontuação) usado na análise exploratória. |
| `ranking/fifa_ranking-*.csv` | Snapshots adicionais do ranking FIFA (2023–2024), ainda não usados nos notebooks. |

## Como executar

1. Crie e ative um ambiente virtual (recomendado Python 3.10+):
   ```
   python -m venv .venv
   .venv\Scripts\activate   # Windows
   source .venv/bin/activate  # Linux/Mac
   ```
2. Instale as dependências:
   ```
   pip install -r requirements.txt
   ```
3. Monte a pasta `dados/` na raiz do projeto conforme a estrutura acima.
4. Abra os notebooks no Jupyter ou VS Code e execute as células em ordem,
   a partir da raiz do repositório (os notebooks leem os CSVs com caminhos
   relativos a `dados/`).

## Notebooks

- [`analise_exploratoria.ipynb`](analise_exploratoria.ipynb) — consolidação
  dos dados históricos, estatísticas descritivas e visualizações (títulos,
  ranking FIFA, desempenho por seleção e por década).
- [`modelo.ipynb`](modelo.ipynb) — engenharia de features, treino e
  avaliação do modelo, aplicação às eliminatórias de 2026 e ranking final de
  probabilidade de título.

## Metodologia

A partir de `matches_1930_2022.csv` e `decision.csv`, cada seleção/ano vira
uma linha com gols, xG, vitórias/empates/derrotas, se sediou o torneio, e se
foi campeã naquele ano (variável alvo). Os dados são filtrados para jogos de
Copa do Mundo e suas eliminatórias, e balanceados com SMOTE antes do treino
(campeãs são uma classe rara). Três classificadores foram comparados
(Regressão Logística, Gradient Boosting e Random Forest); o Random Forest foi
escolhido como modelo principal.

Para gerar o ranking de 2026, as estatísticas das eliminatórias da CONMEBOL e
da UEFA (inseridas manualmente a partir das tabelas de classificação) são
convertidas para o mesmo formato de features e alimentadas ao modelo já
treinado, com as probabilidades calibradas via `CalibratedClassifierCV`.

## Resultados

Na base de teste, o Random Forest obteve acurácia de 0,95 (ROC-AUC 0,96),
próximo da Regressão Logística (ROC-AUC 0,97) e à frente do Gradient Boosting
(ROC-AUC 0,77) na comparação entre modelos. As features mais relevantes para
o modelo foram o número de derrotas, o saldo de gols e o número de vitórias
históricas.

Aplicado às eliminatórias de 2026, o modelo apontou a Espanha (≈75%), a
França (≈71%) e a Inglaterra (≈69%) como as maiores favoritas ao título,
com o Brasil em probabilidade intermediária (≈19%).

## Limitações

- As estatísticas de eliminatórias da CONMEBOL/UEFA usadas no ranking final
  foram inseridas manualmente como um snapshot anterior ao início do
  torneio — para reproduzir com dados atualizados, é preciso substituir os
  dicionários `stats_eliminatorias_conmebol` / `stats_eliminatorias` em
  `modelo.ipynb`.
- A base de treino tem poucas campeãs históricas (uma por Copa), o que limita
  o quanto o modelo consegue generalizar mesmo com balanceamento via SMOTE.
- Os snapshots adicionais de ranking em `dados/ranking/` ainda não são usados
  no pipeline.

## Estrutura do repositório

```
.
├── analise_exploratoria.ipynb
├── modelo.ipynb
├── dados/            # local, não versionado — veja "Dados" acima
├── requirements.txt
├── LICENSE
└── README.md
```

## Licença

Este projeto está sob a licença MIT. Consulte [`LICENSE`](LICENSE).
