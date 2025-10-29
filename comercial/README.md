# Dashboard Comercial – Vendas e Tendência de Faturamento

Análise comercial com foco em faturamento, quantidade vendida, ticket médio, desempenho por produto e por loja. Modelagem e tratamento no Power BI (DAX e Power Query).

**Stack**: Power BI, DAX, Power Query (M)  

## KPIs
- Faturamento Total e variação vs. ano anterior
- Quantidade Total de Vendas
- Ticket Médio
- Top Produtos por Faturamento e Quantidade
- Faturamento por Loja/Unidade

## Principais insights
- Sazonalidade clara com picos em mar/out e quedas em abr/nov.
- Categorias e produtos premium concentram grande parte do faturamento.
- Lojas específicas respondem por parcela relevante da receita, sugerindo ações locais.

## Artefatos
- Arquivo Power BI: `dashboard/dashboard_comercial.pbix`
- Imagens: 
  - ![](docs/img/dashboard_comercial1.png)
  - ![](docs/img/dashboard_comercial2.png)
  - ![](docs/img/dashboard_comercial3.png)

## Medidas DAX (exemplos)
```DAX
Faturamento Total = SUM ( fVendas[valor_venda] )

Quantidade Total = SUM ( fVendas[qtd] )

Ticket Médio = DIVIDE ( [Faturamento Total], [Quantidade Total] )

Faturamento LY = CALCULATE ( [Faturamento Total], SAMEPERIODLASTYEAR ( dCalendario[data] ) )

Variação % Ano = DIVIDE ( [Faturamento Total] - [Faturamento LY], [Faturamento LY] )
```

## Power Query – Etapas base (M)
```M
let
    Fonte = Excel.Workbook(File.Contents("data_source.xlsx"), null, true),
    Tabela = Fonte{[Item="Tabela1",Kind="Table"]}[Data],
    #"Tipos Definidos" = Table.TransformColumnTypes(Tabela,{{"data", type date},{"valor", type number}}),
    #"Colunas Renomeadas" = Table.RenameColumns(#"Tipos Definidos",{{"valor","valor_bruto"}}),
    #"Valores Substituídos" = Table.ReplaceValue(#"Colunas Renomeadas",null,0,Replacer.ReplaceValue,{"valor_bruto"}),
    #"Coluna Ano" = Table.AddColumn(#"Valores Substituídos", "ano", each Date.Year([data]), Int64.Type),
    #"Coluna Mes" = Table.AddColumn(#"Colunas Renomeadas", "mes", each Date.Month([data]), Int64.Type)
in
    #"Coluna Mes"

```

## English summary
**Purpose**: Análise comercial com foco em faturamento, quantidade vendida, ticket médio, desempenho por produto e por loja. Modelagem e tratamento no Power BI (DAX e Power Query).  
**Stack**: Power BI, DAX, Power Query (M).  
**Key KPIs**: Faturamento Total e variação vs. ano anterior; Quantidade Total de Vendas; Ticket Médio; Top Produtos por Faturamento e Quantidade; Faturamento por Loja/Unidade.
