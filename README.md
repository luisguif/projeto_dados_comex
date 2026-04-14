# 🚢 Análise de Importações Brasileiras — Pipeline de Dados + Power BI

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=flat&logo=pandas&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=flat&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Concluído-1A6B3A?style=flat)
![Licença](https://img.shields.io/badge/Licença-MIT-blue?style=flat)

> Projeto completo de análise de dados para uma empresa brasileira de importação — da geração do dataset ao dashboard interativo no Power BI, passando por um pipeline estruturado em Python.

---

## 📋 Sobre o Projeto

Este projeto simula operações reais de uma empresa de importação brasileira, cobrindo o período de **2023 a 2024**. O objetivo foi construir um portfólio end-to-end que demonstre habilidades em engenharia de dados, tratamento com Python e visualização no Power BI.

**Domínio:** Comércio Exterior e Logística Internacional  
**Foco analítico:** Lead time, custos logísticos, eficiência de fornecedores e impacto cambial

---

## 🏗️ Arquitetura do Projeto

```
importacoes-brasileiras/
│
├── dados_brutos/                  # CSVs gerados (dataset sintético)
│   ├── dim_clientes.csv
│   ├── dim_fornecedores.csv
│   ├── dim_produtos.csv
│   ├── dim_tempo.csv
│   ├── fato_processos_importacao.csv
│   ├── fato_itens_processo.csv
│   └── fato_custos.csv
│
├── dados_tratados/                # CSVs processados — prontos para Power BI
│   ├── fato_processos_importacao.csv   ← fato principal (40 colunas)
│   ├── fato_itens_processo.csv
│   ├── fato_custos.csv
│   ├── dim_clientes.csv
│   ├── dim_fornecedores.csv
│   ├── dim_produtos.csv
│   ├── dim_tempo.csv
│   └── agregacoes/
│       ├── agg_lead_time_por_fornecedor.csv
│       ├── agg_impacto_cambial.csv
│       ├── agg_ranking_fornecedores.csv
│       ├── agg_custo_por_tipo_tempo.csv
│       ├── agg_performance_segmento.csv
│       └── agg_custo_atraso.csv
│
├── notebook/
│   └── pipeline_importacao.ipynb  # Pipeline completo de tratamento
│
├── powerbi/
│   ├── dashboard_importacoes.pbix  # Arquivo Power BI
│   └── tema_corporativo.json       # Tema dark corporativo customizado
│
├── docs/
│   ├── dicionario_de_dados.csv
│   ├── perguntas_de_negocio.csv
│   └── guia_powerbi.docx
│
├── assets/
│   ├── dashboard_1_executivo.png
│   ├── dashboard_2_eficiencia.png
│   ├── dashboard_3_custos.png
│   ├── dashboard_4_riscos.png
│   └── dashboard_5_performance.png
│
└── README.md
```

---

## 🔄 Etapas do Projeto

### Etapa 1 — Geração dos Dados com IA

Dataset sintético gerado com **Claude (Anthropic)**, estruturado no formato **Star Schema (Data Warehouse)**:

| Tabela | Tipo | Registros |
|--------|------|-----------|
| `dim_clientes` | Dimensão | 200 |
| `dim_fornecedores` | Dimensão | 100 |
| `dim_produtos` | Dimensão | 500 |
| `dim_tempo` | Dimensão | 730 dias |
| `fato_processos_importacao` | Fato | 1.000 |
| `fato_itens_processo` | Fato | ~2.950 |
| `fato_custos` | Fato | ~6.000 |

**Regras de negócio aplicadas:**
- Incoterms reais: FOB, CIF, EXW — com impacto direto nos custos de frete
- Modal marítimo (25–60 dias) e aéreo (5–15 dias)
- Taxa de câmbio USD/BRL variando entre R$ 4,80 e R$ 5,80
- 20% dos processos com atraso, distribuídos por confiabilidade do fornecedor
- NCMs válidos de 8 dígitos por categoria de produto

---

### Etapa 2 — Pipeline de Tratamento em Python

**Notebook:** [`pipeline_importacao.ipynb`](notebook/pipeline_importacao.ipynb)

Pipeline estruturado em 9 etapas:

```
01 → Configuração do ambiente e constantes
02 → Ingestão e inspeção inicial
03 → Validação de integridade relacional (FK/PK)
04 → Tipagem correta e padronização
05 → Limpeza e qualidade dos dados
06 → Filtro de período 2023–2024
07 → Engenharia de features
08 → Agregações analíticas
09 → Exportação para Power BI
```

**Features criadas na Etapa 7:**

| Feature | Descrição |
|---------|-----------|
| `lead_time_real_dias` | Dias entre embarque e chegada real |
| `desvio_lead_time_dias` | Desvio real vs previsto (+ = atraso) |
| `flag_atraso` | 1 se processo atrasou |
| `flag_atraso_grave` | 1 se atraso superior a 10 dias |
| `flag_cambio_alto` | 1 se câmbio ≥ R$ 5,50 |
| `custo_total_brl` | Soma de todos os custos do processo |
| `valor_total_brl` | Valor da mercadoria convertido para BRL |
| `custo_sobre_valor` | Ratio custo logístico / valor da mercadoria |
| `custo_por_kg_brl` | Custo logístico por kg importado |

---

### Etapa 3 — Dashboard no Power BI

5 dashboards com tema dark corporativo, navegação por abas e mais de 20 medidas DAX.

| Dashboard | Pergunta central | Visuais em destaque |
|-----------|-----------------|---------------------|
| **Executivo** | Como está a operação? | KPIs, linha mensal, donuts |
| **Eficiência** | Fornecedores entregam no prazo? | Scatter previsto vs real, heatmap, gauge SLA |
| **Custos** | Qual o impacto do câmbio? | Waterfall de custos, evolução cambial |
| **Riscos** | Onde estão os maiores riscos? | Tendência MoM com meta, top fornecedores |
| **Performance** | Quais segmentos geram mais valor? | Share por segmento, top NCMs |

---

## 📊 Prévia dos Dashboards

| Executivo | Eficiência |
|-----------|-----------|
| ![Dashboard Executivo](assets/dashboard_1_executivo.png) | ![Dashboard Eficiência](assets/dashboard_2_eficiencia.png) |

| Custos | Riscos |
|--------|--------|
| ![Dashboard Custos](assets/dashboard_3_custos.png) | ![Dashboard Riscos](assets/dashboard_4_riscos.png) |

<p align="center">
  <img src="assets/dashboard_5_performance.png" width="49%" alt="Dashboard Performance"/>
</p>

---

## 🛠️ Tecnologias Utilizadas

| Ferramenta | Uso |
|-----------|-----|
| **Python 3.11** | Pipeline de tratamento |
| **Pandas** | Manipulação e engenharia de features |
| **NumPy** | Cálculos e normalização |
| **Jupyter Notebook** | Desenvolvimento e documentação do pipeline |
| **Power BI Desktop** | Modelagem, DAX e dashboards |
| **Claude (Anthropic)** | Geração do dataset sintético |

---

## ⚙️ Como Reproduzir

### Pré-requisitos

```bash
Python 3.11+
Jupyter Notebook
Power BI Desktop (Windows)
```

### Instalação

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/importacoes-brasileiras.git
cd importacoes-brasileiras

# Instale as dependências
pip install pandas numpy jupyter
```

### Execução

```bash
# Abra o notebook
jupyter notebook notebook/pipeline_importacao.ipynb
```

> Execute as células em ordem. Os CSVs tratados serão exportados automaticamente para `dados_tratados/`.

### Power BI

1. Abra `powerbi/dashboard_importacoes.pbix`
2. Atualize o caminho dos dados em **Transformar Dados → Configurações da Fonte**
3. Aplique e feche — os dashboards serão carregados automaticamente

---

## 📁 Principais Arquivos

| Arquivo | Descrição |
|---------|-----------|
| `notebook/pipeline_importacao.ipynb` | Pipeline completo com 9 etapas documentadas |
| `powerbi/dashboard_importacoes.pbix` | Relatório Power BI com 5 dashboards |
| `powerbi/tema_corporativo.json` | Tema dark importável no Power BI |
| `docs/dicionario_de_dados.csv` | Dicionário com todas as 40 colunas documentadas |
| `docs/guia_powerbi.docx` | Guia completo: relacionamentos, DAX e estrutura |

---

## 💡 Aprendizados

- Engenharia de features no Python reduz drasticamente a complexidade do DAX
- Validação de FK com erro explícito evita problemas silenciosos no Power BI
- Cada visual deve responder uma pergunta — não decorar a tela
- Storytelling orientado ao público é tão importante quanto a técnica

---

## 🚀 Próximos Passos

- [ ] Análise preditiva: modelo de previsão de atrasos com Scikit-learn
- [ ] Recriar as agregações em SQL puro
- [ ] Publicar no Power BI Service com atualização agendada

---

## 📄 Licença

Distribuído sob a licença MIT. Consulte [`LICENSE`](LICENSE) para mais informações.

---

<p align="center">
  Desenvolvido como projeto de portfólio · Analista de Dados Júnior
</p>
