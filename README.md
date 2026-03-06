# Previsao Copa do Mundo 2026

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-orange)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Projeto de ciencia de dados para explorar historicos de partidas e rankings
FIFA e construir um modelo preditivo para resultados na Copa do Mundo 2026.

## Objetivo
- Consolidar dados historicos de partidas e rankings.
- Explorar padroes e variaveis relevantes.
- Treinar e avaliar modelos de classificacao para previsao de resultados.

## Dados
Os dados estao na pasta `dados_ciclo_basic/` e incluem:
- Rankings FIFA (varios snapshots).
- Resultados historicos de partidas.
- Base de partidas de Copas do Mundo.

Observacao: os arquivos sao fornecidos no repositorio para reproducao local.

## Estrutura do repositorio
```
.
├── analise_exploratoria.ipynb
├── criacao_df_modelo.ipynb
├── dados_ciclo_basic/
│   ├── dados_copadomundo/
│   └── ... (csvs)
├── requirements.txt
├── LICENSE
└── CONTRIBUTING.md
└── README.md
```

## Estrutura sugerida de dados
Caso queira evoluir a organizacao, uma estrutura padrao ajuda na manutencao:
```
data/
├── raw/        # dados originais
├── interim/    # dados intermediarios
└── processed/  # dados prontos para modelagem
```
No momento, os dados estao centralizados em `dados_ciclo_basic/`.

## Como executar
1) Crie e ative um ambiente virtual (recomendado Python 3.10+).
2) Instale as dependencias:
```
pip install -r requirements.txt
```
3) Abra os notebooks no Jupyter ou VS Code.

## Notebooks
- `analise_exploratoria.ipynb`: analise inicial e visualizacoes.
- `criacao_df_modelo.ipynb`: engenharia de features, modelagem e avaliacao.

## Reprodutibilidade
Para as celulas que usam scraping (selenium/bs4), pode ser necessario:
- Google Chrome instalado.
- ChromeDriver compativel na PATH.

## Status
Em desenvolvimento.

## Contribuicao
Veja `CONTRIBUTING.md` para padroes e boas praticas.

## Licenca
Este projeto esta sob a licenca MIT. Consulte `LICENSE`.