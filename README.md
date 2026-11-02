# Projeto 2 — Aprendizado de Máquina: Olist

**Disciplina:** T326 - Ciência dos Dados · **Professor:** Caio Ponte · **Turma:** 16/17

Prever o **tempo de entrega de pedidos** da Olist (regressão) a partir do dataset público *Brazilian E-Commerce Public Dataset*.

## Estrutura

```
Trabalho Caio AV2 Olist/
├── Projeto 2 - Aprendizado de Máquina_ Olist.pdf   # enunciado
├── Projeto2_Olist.ipynb                            # notebook do projeto
├── README.md
└── dados/                                          # 9 CSVs do Kaggle
    ├── olist_orders_dataset.csv
    ├── olist_order_items_dataset.csv
    ├── olist_order_payments_dataset.csv
    ├── olist_order_reviews_dataset.csv
    ├── olist_customers_dataset.csv
    ├── olist_sellers_dataset.csv
    ├── olist_products_dataset.csv
    ├── olist_geolocation_dataset.csv
    └── product_category_name_translation.csv
```

## Etapa 1 — Definição do Problema ✅

Feito no notebook (`Projeto2_Olist.ipynb`):

| Item | Conteúdo |
|---|---|
| **Contexto** | Olist = ecossistema brasileiro de e-commerce/logística (ERP, marketplaces, envios). Prever o prazo é crítico para NPS, custo e retenção de vendedores. |
| **Problema** | Regressão. **Amostra** = 1 pedido (`order_id`). **Alvo** = dias entre `order_purchase_timestamp` e `order_delivered_customer_date`. |
| **Dados** | 9 tabelas carregadas; núcleo = `orders` (99.441 pedidos), joins com `items`, `customers`, `sellers`, `products`, `payments`. |
| **Amostras úteis** | **96.478** pedidos com status `delivered` (com alvo calculável). |
| **Desafio** | Sazonalidade, heterogeneidade da rede logística, distância origem→destino, prazo prometido x real. |

**Tabelas escolhidas:** `orders` (núcleo) + `items`, `customers`, `sellers`, `products`, `payments`.
`geolocation` → EDA (mapas); `reviews` → pós-entrega (não usar como feature para não vazar o alvo); `categories` → tradução PT→EN.

## Etapa 2 — Pré-processamento ✅

Feito no notebook:

| Item | Conteúdo |
|---|---|
| **Engenharia de features** | 21 features via joins: itens/preço/frete (`items`), peso/dimensões/categoria (`products`), pagamento (`payments`), distância Haversine vendedor→cliente (`geolocation`), estados, sazonalidade (mês/dia/hora), prazo estimado |
| **Limpeza** | só `delivered`; removidos 8 sem alvo + 960 outliers > p99 (46 dias) → **95.033 pedidos** |
| **Valores faltantes** | `SimpleImputer` — mediana (num) / moda (cat) |
| **Normalização** | `RobustScaler` nas 17 numéricas |
| **Categóricas** | `OneHotEncoder` em categoria, tipo de pagamento, estados → **143 features** finais |
| **Pipeline** | `ColumnTransformer` pronto para o treino |

Arquivos gerados: `dados/df_modelo.csv` (base limpa), `dados/resumo_features.csv`.

## Próximas etapas

1. ~~Definição do problema~~ ✅
2. ~~Pré-processamento e features~~ ✅
3. EDA (10%)
4. Treinamento de ≥3 modelos + Grid/RandomizedSearch (30%)
5. Avaliação (RMSE/MAE/R²), benchmark e conclusão (20%)

## Rodar

```bash
jupyter notebook Projeto2_Olist.ipynb
```
# Olist
