# VertoMarket — Dashboard Comercial de E-commerce 🛒

## 📋 Sobre o Projeto

Case autoral (fora de curso, dataset real do Kaggle) para praticar uma stack de dados completa: extração e tratamento em Python, banco de dados relacional em PostgreSQL, consultas analíticas em SQL, e um dashboard executivo interativo no Power BI, com layout prototipado previamente no Figma.

O dataset utilizado é uma versão do **Olist** (e-commerce brasileiro real), com pedidos, itens, produtos, clientes e pagamentos.

## 🛠️ Tecnologias Utilizadas

- **Python (Pandas):** extração e tratamento dos dados brutos.
- **PostgreSQL:** banco de dados relacional, com consultas SQL analíticas.
- **Power BI (DAX):** modelagem de dados e medidas de negócio, conectado diretamente ao PostgreSQL.
- **Figma:** prototipação do layout antes da construção do dashboard.

## 🗂️ Pipeline

1. **Python:** leitura e limpeza dos CSVs originais (Olist).
2. **PostgreSQL:** carga dos dados tratados em um schema relacional (`orders`, `order_items`, `products`, `customers`, `payments`), com chaves estrangeiras entre as tabelas.
3. **SQL:** consultas exploratórias e de validação sobre o banco.
4. **Power BI:** conexão direta ao PostgreSQL (sem intermediário de arquivo), modelagem do relacionamento entre tabelas e criação de uma tabela `d_Calendario` própria.

## 📐 Regra de Negócio — Receita

Assim como nos demais projetos, apenas pedidos com status `delivered` são considerados receita real (**Receita Realizada**), separando o dinheiro efetivamente concretizado da receita em pipeline (pedidos ainda em processamento, aprovados ou em trânsito).

## 📊 KPIs e Visuais

**KPIs:** Receita Realizada, Quantidade de Pedidos, Ticket Médio, Crescimento vs. Mês Anterior.

**Visuais:**
- Receita ao longo do tempo (linha).
- Top categorias de produto por receita (barras).
- Mapa de pedidos por estado + ranking dos top estados.
- Ticket médio por estado (barras).
- Tabela detalhada de pedidos, com filtro por período e estado.

## 🧠 Desafio Técnico — USERELATIONSHIP + DATESINPERIOD

A medida de Crescimento vs. Mês Anterior precisava comparar a receita de dois blocos de meses usando `order_approved_at`, coluna com uma relação **inativa** com `d_Calendario` (a relação ativa do modelo usa `order_purchase_timestamp`). A combinação `USERELATIONSHIP` + `DATESINPERIOD` no mesmo `CALCULATE` não retornava valor — a solução foi substituir `DATESINPERIOD` por um filtro manual de intervalo de datas (`FILTER` + comparação direta de `>=`/`<=`), mantendo o `USERELATIONSHIP` para ativar a relação correta apenas dentro da medida, sem alterar o modelo global.

## 📂 Estrutura do Repositório

- `python/`: scripts de extração e tratamento dos dados.
- `sql/`: schema do banco e queries analíticas.
- `figma/`: protótipo do layout.
- `dashboard/`: arquivo `.pbix` e imagem do dashboard final.

## 🧭 Principais Insights

- A categoria **Brinquedos** domina amplamente o faturamento (R$ 7,2 Mi), muito acima da segunda colocada — sinal de forte concentração de receita em uma única categoria, ponto de atenção para diversificação.
- **São Paulo** lidera com folga em volume de pedidos (11.302), mais que o triplo do segundo colocado (RJ).
- O ticket médio mais alto não vem dos estados com mais volume — estados como MA, AM e MS aparecem no topo do ticket médio, sugerindo perfis de compra diferentes por região.
