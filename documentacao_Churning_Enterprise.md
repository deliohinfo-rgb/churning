
# Documentação Técnica do Modelo Power BI

Gerado em **2026-05-16 22:25:49.104032**

Modelo analisado: `Churning_Enterprise`

Versão do script: **1.7.1**


## Visão Geral

- Total de tabelas: **33**
- Total de relacionamentos: **30**
- Total de páginas do relatório: **10**

## Tabelas


### 0_Backlog_NProcessado

### Colunas
- **Marca** — `string`
- **'Grupo** — `string`
- **'Nome** — `string`
- **'Nro** — `string`
- **'ID** — `int64`
- **'Nome** — `string`
- **'Item** — `string`
- **'Data** — `dateTime`
- **Motivo** — `string`
- **'Causa** — `string`
- **SubTipo** — `string`
- **'Rateio** — `string`
- **'GRC** — `string`
- **'GRC** — `string`
- **'GRC** — `string`
- **Status** — `string`
- **SLA** — `double`
- **'Valor** — `double`
- **_SUBSUBTIPO** — `desconhecido`
- **_GR_DIRETO_INDIRETO** — `desconhecido`
- **DataReferencia** — `dateTime`
- **_AGING** — `int64`
- **_1_A_5** — `desconhecido`
- **_6_A_10** — `desconhecido`
- **_11_A_30** — `desconhecido`
- **_31_A_** — `desconhecido`
- **'GRC** — `string`
- **Ano_DataReferencia** — `desconhecido`

### Medidas

### Consultas M (Partitions)
#### Partição: 0_Backlog_NProcessado
**Etapas:**
- Documentos Compartilhados
- Documentos Compartilhados
- Backlog_NProcessado xlsx
- Pasta de Trabalho Importada do Excel
- Backlog_NProcessado xlsx
- Linhas Filtradas1
- Pasta de Trabalho Importada do Excel
- Data Expandido
- Linhas Filtradas1
- Cabeçalhos Promovidos
- Data Expandido
- Tipo Alterado
- Cabeçalhos Promovidos
- Colunas Removidas
- Tipo Alterado
- Linhas Filtradas
- Colunas Removidas
- Coluna Duplicada
- Linhas Filtradas
- Colunas Renomeadas
- Coluna Duplicada
- Colunas Renomeadas

**Fontes:**
- `Excel.Workbook(#"Backlog_NProcessado xlsx")`

**Dependências:**
- `BI_Churn`

**Código M Completo:**
```PowerQuery
let
				    Fonte = SharePoint.Contents("https://grupolinx.sharepoint.com/sites/Grupo_Suporte_EasyLinx", [ApiVersion = 15]),
				    #"Documentos Compartilhados" = Fonte{[Name="Documentos Compartilhados"]}[Content],
				    BI_Churn = #"Documentos Compartilhados"{[Name="BI_Churn"]}[Content],
				    #"Backlog_NProcessado xlsx" = BI_Churn{[Name="Backlog_NProcessado.xlsx"]}[Content],
				    #"Pasta de Trabalho Importada do Excel" = Excel.Workbook(#"Backlog_NProcessado xlsx"),
				    #"Linhas Filtradas1" = Table.SelectRows(#"Pasta de Trabalho Importada do Excel", each ([Name] = "Export")),
				    #"Data Expandido" = Table.ExpandTableColumn(#"Linhas Filtradas1", "Data", {"Column1", "Column2", "Column3", "Column4", "Column5", "Column6", "Column7", "Column8", "Column9", "Column10", "Column11", "Column12", "Column13", "Column14", "Column15", "Column16", "Column17", "Column18", "Column19"}, {"Data.Column1", "Data.Column2", "Data.Column3", "Data.Column4", "Data.Column5", "Data.Column6", "Data.Column7", "Data.Column8", "Data.Column9", "Data.Column10", "Data.Column11", "Data.Column12", "Data.Column13", "Data.Column14", "Data.Column15", "Data.Column16", "Data.Column17", "Data.Column18", "Data.Column19"}),
				    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(#"Data Expandido", [PromoteAllScalars=true]),
				    #"Tipo Alterado" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{{"Export", type text}, {"Marca", type text}, {"Grupo Econômico", type text}, {"Nome Cliente", type text}, {"Nro do Docto", type text}, {"ID da Solicitação", Int64.Type}, {"Nome GR", type text}, {"Item Fiscal", type text}, {"Data do Cadastro", type date}, {"Motivo", type text}, {"Causa da Solicitação de Redução", type text}, {"SubTipo", type text}, {"Rateio Centro De Custo", type text}, {"GRC - Grupo Produto", type text}, {"GRC - Area Cross", type text}, {"GRC - Area Organico", type text}, {"Status", type text}, {"SLA", type number}, {"Valor Redução", type number}, {"Export_1", type text}, {"Sheet", type text}, {"false", type logical}}),
				    #"Colunas Removidas" = Table.RemoveColumns(#"Tipo Alterado",{"Export", "Export_1", "Sheet", "false"}),
				    #"Linhas Filtradas" = Table.SelectRows(#"Colunas Removidas", each ([Grupo Econômico] <> null)),
				    #"Coluna Duplicada" = Table.DuplicateColumn(#"Linhas Filtradas", "Data do Cadastro", "Data do Cadastro - Copiar"),
				    #"Colunas Renomeadas" = Table.RenameColumns(#"Coluna Duplicada",{{"Data do Cadastro - Copiar", "DataReferencia"}})
				in
				    #"Colunas Renomeadas"
```

### 0_Cobranca

### Colunas
- **'Cod** — `int64`
- **CNPJ** — `string`
- **'Desc** — `string`
- **'Número** — `int64`
- **'Nome** — `string`
- **Emissão** — `dateTime`
- **Vencimento** — `dateTime`
- **'Vencimento** — `dateTime`
- **Contestacao** — `string`
- **Fatura** — `string`
- **'Id** — `string`
- **'Valor** — `double`
- **'Valor** — `double`
- **'Status** — `string`
- **'Valor** — `double`
- **_AGING** — `int64`
- **_31_A_60** — `desconhecido`
- **_90** — `desconhecido`
- **_61_A_90** — `desconhecido`
- **_AGING_CLASSIFICADO** — `desconhecido`

### Medidas

### Consultas M (Partitions)
#### Partição: 0_Cobranca
**Etapas:**
- Documentos Compartilhados
- Documentos Compartilhados
- Cobranca xlsx
- Pasta de Trabalho Importada do Excel
- Cobranca xlsx
- Pasta de Trabalho Importada do Excel
- Cabeçalhos Promovidos
- Tipo Alterado
- Cabeçalhos Promovidos
- Linhas Filtradas
- Tipo Alterado
- Linhas Filtradas

**Fontes:**
- `Excel.Workbook(#"Cobranca xlsx")`

**Dependências:**
- `BI_Churn`
- `Export_Sheet`

**Código M Completo:**
```PowerQuery
let
				    Fonte = SharePoint.Contents("https://grupolinx.sharepoint.com/sites/Grupo_Suporte_EasyLinx", [ApiVersion = 15]),
				    #"Documentos Compartilhados" = Fonte{[Name="Documentos Compartilhados"]}[Content],
				    BI_Churn = #"Documentos Compartilhados"{[Name="BI_Churn"]}[Content],
				    #"Cobranca xlsx" = BI_Churn{[Name="Cobranca.xlsx"]}[Content],
				    #"Pasta de Trabalho Importada do Excel" = Excel.Workbook(#"Cobranca xlsx"),
				    Export_Sheet = #"Pasta de Trabalho Importada do Excel"{[Item="Export",Kind="Sheet"]}[Data],
				    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(Export_Sheet, [PromoteAllScalars=true]),
				    #"Tipo Alterado" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{{"Cod Clifor", Int64.Type}, {"CNPJ", type text}, {"Desc Rateio Centro  Custo", type text}, {"Número NF-e", Int64.Type}, {"Nome Clifor", type text}, {"Emissão", type date}, {"Vencimento", type date}, {"Vencimento Real", type date}, {"Contestacao", type text}, {"Fatura", type text}, {"Id Parcela", type text}, {"Valor a Original", type number}, {"Valor a Receber", type number}, {"Status Negativação", type text}, {"Valor Vencido + Juros e Multa", type number}}),
				    #"Linhas Filtradas" = Table.SelectRows(#"Tipo Alterado", each ([CNPJ] <> null))
				in
				    #"Linhas Filtradas"
```

### 0_Perdas_Detalhadas_2024

### Colunas
- **'ID** — `int64`
- **'Data** — `dateTime`
- **'Prazo** — `int64`
- **'Usuario** — `string`
- **'Data** — `dateTime`
- **'Nome** — `string`
- **'Grupo** — `string`
- **CNPJ/CPF** — `string`
- **Contato** — `string`
- **'CNPJ** — `string`
- **Marca** — `string`
- **'Situação** — `string`
- **'Descrição** — `dateTime`
- **'Cód** — `int64`
- **'DPP** — `string`
- **'Item** — `string`
- **'Data** — `dateTime`
- **'Data** — `dateTime`
- **'Unidade** — `string`
- **Motivo** — `string`
- **'Causa** — `string`
- **SubTipo** — `string`
- **'Nome** — `string`
- **'Rateio** — `string`
- **'GRC** — `string`
- **'GRC** — `string`
- **'GRC** — `string`
- **'Valor** — `double`
- **'Qtde** — `int64`
- **DataReferencia** — `dateTime`

### Medidas

### Consultas M (Partitions)
#### Partição: 0_Perdas_Detalhadas_2024
**Etapas:**
- Documentos Compartilhados
- Documentos Compartilhados
- Perdas_detalhadas xlsx
- Pasta de Trabalho Importada do Excel
- Perdas_detalhadas xlsx
- Pasta de Trabalho Importada do Excel
- Cabeçalhos Promovidos
- Tipo Alterado
- Cabeçalhos Promovidos
- Coluna Duplicada
- Tipo Alterado
- Colunas Renomeadas
- Coluna Duplicada
- Linhas Filtradas para remover erros
- Colunas Renomeadas
- Linhas Filtradas para remover erros

**Fontes:**
- `Excel.Workbook(#"Perdas_detalhadas xlsx")`

**Dependências:**
- `BI_Churn`
- `Export_Sheet`

**Código M Completo:**
```PowerQuery
let
				    Fonte = SharePoint.Contents("https://grupolinx.sharepoint.com/sites/Grupo_Suporte_EasyLinx", [ApiVersion = 15]),
				    #"Documentos Compartilhados" = Fonte{[Name="Documentos Compartilhados"]}[Content],
				    BI_Churn = #"Documentos Compartilhados"{[Name="BI_Churn"]}[Content],
				    #"Perdas_detalhadas xlsx" = BI_Churn{[Name="Perdas_detalhadas_2024.xlsx"]}[Content],
				    #"Pasta de Trabalho Importada do Excel" = Excel.Workbook(#"Perdas_detalhadas xlsx"),
				    Export_Sheet = #"Pasta de Trabalho Importada do Excel"{[Item="Export",Kind="Sheet"]}[Data],
				    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(Export_Sheet, [PromoteAllScalars=true]),
				    #"Tipo Alterado" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{{"ID Solicitação", Int64.Type}, {"Data Inclusão", type date}, {"Prazo Aviso Prévio", Int64.Type}, {"Usuario Inclusao", type text}, {"Data Implantacao", type date}, {"Nome Cliente", type text}, {"Grupo Econômico", type text}, {"CNPJ/CPF", type text}, {"Contato", type text}, {"CNPJ Filial", type text}, {"Marca", type text}, {"Situação Cliente", type text}, {"Descrição Mês Ano", type date}, {"Cód Cliente", Int64.Type}, {"DPP - Produto", type text}, {"Item Fiscal", type text}, {"Data do Cadastro", type date}, {"Data Conclusão", type date}, {"Unidade Comercial", type text}, {"Motivo", type text}, {"Causa da Solicitação de Redução", type text}, {"SubTipo", type text}, {"Nome GR", type text}, {"Rateio Centro De Custo", type text}, {"GRC - Grupo Produto", type text}, {"GRC - Area Cross", type text}, {"GRC - Area Organico", type text}, {"Valor Redução", type number}, {"Qtde IDs Itens Redução", Int64.Type}}),
				    #"Coluna Duplicada" = Table.DuplicateColumn(#"Tipo Alterado", "Descrição Mês Ano", "Descrição Mês Ano - Copiar"),
				    #"Colunas Renomeadas" = Table.RenameColumns(#"Coluna Duplicada",{{"Descrição Mês Ano - Copiar", "DataReferencia"}}),
				    // Remover linhas que não tem dados. Como a tabela é importa do BI, ela tem no final algumas linhas com totalizador e filtros. Para não trazer esses dados, filtro criado.
				    #"Linhas Filtradas para remover erros" = Table.SelectRows(#"Colunas Renomeadas", each [Data Inclusão] <> null and [Data Inclusão] <> "")
				in
				    #"Linhas Filtradas para remover erros"
```

### 0_Perdas_Detalhadas_Atual

### Colunas
- **'ID** — `int64`
- **'Data** — `dateTime`
- **'Prazo** — `int64`
- **'Usuario** — `string`
- **'Data** — `dateTime`
- **'Nome** — `string`
- **'Grupo** — `string`
- **CNPJ/CPF** — `string`
- **Contato** — `string`
- **'CNPJ** — `string`
- **Marca** — `string`
- **'Situação** — `string`
- **'Descrição** — `dateTime`
- **'Cód** — `int64`
- **'DPP** — `string`
- **'Item** — `string`
- **'Data** — `dateTime`
- **'Data** — `dateTime`
- **'Unidade** — `string`
- **Motivo** — `string`
- **'Causa** — `string`
- **SubTipo** — `string`
- **'Nome** — `string`
- **'Rateio** — `string`
- **'GRC** — `string`
- **'GRC** — `string`
- **'GRC** — `string`
- **'Valor** — `double`
- **'Qtde** — `int64`
- **Tipo** — `desconhecido`
- **GR_DIRETO_INDIRETO** — `desconhecido`
- **REFERENCIA_PERIODO** — `desconhecido`
- **DataReferencia** — `dateTime`
- **'GRC** — `string`
- **OBS** — `string`
- **Ano_Descricao** — `string`
- **2024** — `desconhecido`
- **LegendaSemBranco** — `desconhecido`

### Medidas
#### ValorRescisao_2024
```DAX
VAR SomaContexto =
			    CALCULATE(
			        SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			        YEAR('0_Perdas_Detalhadas_Atual'[DataReferencia]) = 2024
			    )
			RETURN
			IF( SomaContexto > 0, SomaContexto, BLANK() )
```

### Consultas M (Partitions)
#### Partição: 0_Perdas_Detalhadas_Atual
**Etapas:**
- Documentos Compartilhados
- Documentos Compartilhados
- Perdas_detalhadas xlsx
- Pasta de Trabalho Importada do Excel
- Perdas_detalhadas xlsx
- Pasta de Trabalho Importada do Excel
- Cabeçalhos Promovidos
- Tipo Alterado
- Cabeçalhos Promovidos
- Linhas Filtradas para remover erros
- Tipo Alterado
- Consulta Acrescentada
- Linhas Filtradas para remover erros
- 0_Perdas_Detalhadas_2024
- Coluna Duplicada
- Consulta Acrescentada
- Colunas Removidas
- Coluna Duplicada
- Colunas Renomeadas
- Colunas Removidas
- Colunas Renomeadas

**Fontes:**
- `Excel.Workbook(#"Perdas_detalhadas xlsx")`

**Dependências:**
- `BI_Churn`
- `Export_Sheet`

**Código M Completo:**
```PowerQuery
let
				    Fonte = SharePoint.Contents("https://grupolinx.sharepoint.com/sites/Grupo_Suporte_EasyLinx", [ApiVersion = 15]),
				    #"Documentos Compartilhados" = Fonte{[Name="Documentos Compartilhados"]}[Content],
				    BI_Churn = #"Documentos Compartilhados"{[Name="BI_Churn"]}[Content],
				    #"Perdas_detalhadas xlsx" = BI_Churn{[Name="Perdas_detalhadas_Atual.xlsx"]}[Content],
				    #"Pasta de Trabalho Importada do Excel" = Excel.Workbook(#"Perdas_detalhadas xlsx"),
				    Export_Sheet = #"Pasta de Trabalho Importada do Excel"{[Item="Export",Kind="Sheet"]}[Data],
				    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(Export_Sheet, [PromoteAllScalars=true]),
				    #"Tipo Alterado" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{{"ID Solicitação", Int64.Type}, {"Data Inclusão", type date}, {"Prazo Aviso Prévio", Int64.Type}, {"Usuario Inclusao", type text}, {"Data Implantacao", type date}, {"Nome Cliente", type text}, {"Grupo Econômico", type text}, {"CNPJ/CPF", type text}, {"Contato", type text}, {"CNPJ Filial", type text}, {"Marca", type text}, {"Situação Cliente", type text}, {"Descrição Mês Ano", type date}, {"Cód Cliente", Int64.Type}, {"DPP - Produto", type text}, {"Item Fiscal", type text}, {"Data do Cadastro", type date}, {"Data Conclusão", type date}, {"Unidade Comercial", type text}, {"Motivo", type text}, {"Causa da Solicitação de Redução", type text}, {"SubTipo", type text}, {"Nome GR", type text}, {"Rateio Centro De Custo", type text}, {"GRC - Grupo Produto", type text}, {"GRC - Area Cross", type text}, {"GRC - Area Organico", type text}, {"Valor Redução", type number}, {"Qtde IDs Itens Redução", Int64.Type}}),
				    // Remover linhas que não tem dados. Como a tabela é importa do BI, ela tem no final algumas linhas com totalizador e filtros. Para não trazer esses dados, filtro criado.
				    #"Linhas Filtradas para remover erros" = Table.SelectRows(#"Tipo Alterado", each [Data Inclusão] <> null and [Data Inclusão] <> ""),
				    #"Consulta Acrescentada" = Table.Combine({#"Linhas Filtradas para remover erros", #"0_Perdas_Detalhadas_2024"}),
				    #"Coluna Duplicada" = Table.DuplicateColumn(#"Consulta Acrescentada", "Descrição Mês Ano", "Descrição Mês Ano - Copiar"),
				    #"Colunas Removidas" = Table.RemoveColumns(#"Coluna Duplicada",{"DataReferencia"}),
				    #"Colunas Renomeadas" = Table.RenameColumns(#"Colunas Removidas",{{"Descrição Mês Ano - Copiar", "DataReferencia"}})
				in
				    #"Colunas Renomeadas"
```

### 1_Calendario

### Colunas
- **Date** — `desconhecido`
- **Ano** — `string`
- **Mês** — `desconhecido`
- **Dia** — `desconhecido`
- **'Nome** — `desconhecido`
- **'Dia** — `desconhecido`
- **'Semana** — `desconhecido`
- **'Mes** — `dateTime`
- **'Dia** — `desconhecido`
- **'Mês** — `desconhecido`
- **'Mês** — `desconhecido`
- **Dia_Mes_Ano** — `desconhecido`
- **AnoLegenda** — `desconhecido`

### Medidas

### Consultas M (Partitions)
#### Partição: 1_Calendario
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery
ADDCOLUMNS(
				         CALENDAR(DATE(2020, 1, 1),MAX('0_Perdas_Detalhadas_Atual'[Descrição Mês Ano])),
				         "Ano", YEAR([Date]),
				         "Mês", MONTH([Date]),
				         "Dia", DAY([Date]),
				         "Nome do Mês", FORMAT([Date],"mmmm", "PT-BR"),
				         "Dia da Semana", FORMAT([Date], "dddd", "PT-BR"),
				         "Semana Ano", WEEKNUM([Date],2),
				         "Mes Ano", (MONTH([Date]) & "/" & YEAR([Date])),
				         "Dia Mês", (day([Date]) & "/" & MONTH([Date])),
				         "Mês Dia", ( "M:" & MONTH([Date]) & " / " & "D:" & day([Date])),
				         "Mês e ano nome", FORMAT([Date],"mmmm", "PT-BR") &" "& YEAR([Date]),
				         "Dia_Mes_Ano", (DAY([Date]) & "/" & MONTH([Date]) & "/" & YEAR([Date])
				         ))
```

### '5.1_1

### Colunas
- **'Proprietário** — `string`
- **'Número** — `string`
- **'Nome** — `string`
- **Status** — `string`
- **'Criado** — `string`
- **'Data** — `dateTime`
- **'Contagem** — `double`
- **'Data** — `dateTime`
- **Corpo** — `string`
- **Visibilidade** — `string`
- **'Data** — `string`
- **'Modificado** — `string`
- **'Unidade** — `string`
- **'Nível** — `string`
- **DataReferencia** — `desconhecido`

### Medidas

### Consultas M (Partitions)
#### Partição: '5.1_1
**Etapas:**
- 00O5f00000894gCEAQ
- 00O5f00000894gCEAQ

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery
let
				    Fonte = Salesforce.Reports("https://suportelinx.my.salesforce.com/", [ApiVersion=48]),
				    #"00O5f00000894gCEAQ" = Fonte{[Name="00O5f00000894gCEAQ"]}[Data]
				in
				    #"00O5f00000894gCEAQ"
```

### 5_Casos

### Colunas
- **Id** — `string`
- **IsDeleted** — `boolean`
- **CaseNumber** — `string`
- **ContactId** — `string`
- **AccountId** — `string`
- **Type** — `string`
- **Status** — `string`
- **Origin** — `string`
- **Subject** — `string`
- **Priority** — `string`
- **Description** — `string`
- **IsClosed** — `boolean`
- **ClosedDate** — `dateTime`
- **OwnerId** — `string`
- **CreatedDate** — `dateTime`
- **ContactPhone** — `string`
- **ContactEmail** — `string`
- **MilestoneStatus** — `string`
- **Assunto__c** — `string`
- **Categoria__c** — `string`
- **Resolvido_em__c** — `dateTime`
- **Segmento__c** — `string`
- **Data_Final_SLA__c** — `dateTime`
- **CNPJ__c** — `string`
- **ChamadoId__c** — `double`
- **Data_Inicio_SLA__c** — `dateTime`
- **Descricao_Encerramento__c** — `string`
- **NameAccount__c** — `string`
- **Nivel_1__c** — `string`
- **Nivel_2__c** — `string`
- **Nivel_3__c** — `string`
- **Nivel_4__c** — `string`
- **Unidade_de_Negocio_Taxonomia__c** — `string`
- **Unidade_de_Negocio__c** — `string`
- **Funcionalidade_Taxonomia__c** — `string`
- **Modulo_Taxonomia__c** — `string`
- **Numero_Chamado_WFW__c** — `string`
- **Produto_Taxonomia__c** — `string`
- **SubModulo_Taxonomia__c** — `string`
- **Unidade_Negocio_Fila__c** — `string`
- **Categoria_Dig__c** — `string`
- **Tema_Dig__c** — `string`
- **Email_da_Conta__c** — `string`
- **Telefone_da_Conta__c** — `string`
- **Telefone_do_Contato__c** — `string`
- **Solicitante__c** — `string`
- **NameAccountJira__c** — `string`
- **Responsavel_Caso__c** — `string`
- **Prioridade__c** — `string`
- **Servicos2__c** — `string`
- **Servicos__c** — `string`
- **Tipo_de_Solicitacao1__c** — `string`
- **Situacao1__c** — `string`
- **Idade_do_Caso_em_Dias__c** — `double`
- **Alias_Linx_do_Proprietario_do_Caso__c** — `string`
- **Produto_AdmFin__c** — `string`
- **Conta_Aberto__c** — `string`
- **Conta_Fechado__c** — `string`
- **Contato_Aberto__c** — `string`
- **Nivel_fila_owner__c** — `string`
- **Telefone_da_Conta_Completo__c** — `string`
- **Telefone_do_Contato_Completo__c** — `string`
- **Data_Final_SLA_Violado__c** — `boolean`
- **Tipo_de_Classificacao__c** — `string`
- **Celular_Completo__c** — `string`
- **Telefone_Completo__c** — `string`
- **Proprietario_Conta__c** — `string`
- **Resolvido_em_24h__c** — `boolean`
- **Multiplos_Agentes__c** — `boolean`
- **FCR_Formula__c** — `boolean`
- **Proprietario_Caso__c** — `string`
- **ID_Contestacao__c** — `string`
- **Valor_Contestacao__c** — `double`
- **Codigo_Conta__c** — `string`
- **Email_Owner__c** — `string`
- **GrupoEconomico__c** — `string`
- **Marca__c** — `string`
- **R_UnidadeNegocio** — `string`
- **_DataReferencia** — `desconhecido`
- **_Area** — `desconhecido`
- **LastModifiedDate** — `dateTime`
- **Conta_Portal_Maker__c** — `string`
- **Criado_Jira__c** — `string`
- **Knowledge__c** — `string`
- **sftv__tvCaseId__c** — `string`
- **sftv__tvCustomerLink__c** — `string`
- **sftv__tvExpirationDate__c** — `dateTime`
- **sftv__tvSupportLink__c** — `string`
- **sftv__txt_ContactFullName__c** — `string`
- **Resumo_Gerado__c** — `boolean`
- **Estimativa_Jira__c** — `string`
- **cliente_retido__c** — `string`
- **meses_retido__c** — `double`
- **passou_retencao__c** — `string`
- **redres_id__c** — `string`
- **Tipo_do_caso_VOC__c** — `string`

### Medidas

### Consultas M (Partitions)
#### Partição: 5_Casos
**Etapas:**
- Linhas Filtradas
- Unidade_de_Negocio__r Expandido
- Linhas Filtradas
- Colunas Renomeadas
- Unidade_de_Negocio__r Expandido
- Linhas Filtradas1
- Colunas Renomeadas
- Colunas Removidas
- Linhas Filtradas1
- Linhas Filtradas3
- Colunas Removidas
- Linhas Filtradas2
- Linhas Filtradas3
- Linhas Filtradas2

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery
let
				    Fonte = Salesforce.Data("https://suportelinx.my.salesforce.com/", [ApiVersion=48, CreateNavigationProperties=true]),
				    Case = Fonte{[Name="Case"]}[Data],
				    #"Linhas Filtradas" = Table.SelectRows(Case, each [Unidade_de_Negocio__c] = "a025f00000CTHhNAAX" or [Unidade_de_Negocio__c] = "a02TT000007Z3ewYAC"),
				    #"Unidade_de_Negocio__r Expandido" = Table.ExpandRecordColumn(#"Linhas Filtradas", "Unidade_de_Negocio__r", {"Name"}, {"Unidade_de_Negocio__r.Name"}),
				    #"Colunas Renomeadas" = Table.RenameColumns(#"Unidade_de_Negocio__r Expandido",{{"Unidade_de_Negocio__r.Name", "R_UnidadeNegocio"}}),
				    #"Linhas Filtradas1" = Table.SelectRows(#"Colunas Renomeadas", each true),
				    #"Colunas Removidas" = Table.RemoveColumns(#"Linhas Filtradas1",{"MasterRecordId", "SourceId", "ParentId", "SuppliedName", "SuppliedEmail", "SuppliedPhone", "SuppliedCompany", "RecordTypeId", "Reason", "Language", "IsEscalated", "IsStopped", "StopStartDate", "CreatedById", "LastModifiedById", "SystemModstamp", "ContactMobile", "ContactFax", "Comments", "LastViewedDate", "LastReferencedDate", "Demanda_Interna__c", "Funcionalidade__c", "Issue_Code_Jira__c", "Modulo__c", "Pesquisa_enviada__c", "Possui_Alternativa__c", "Product__c", "Produto__c", "Sistema_Indisponivel__c", "Sub_Funcionalidade__c", "Sub_Modulo__c", "Tema__c", "Visualizar_detalhes__c", "Nota_CSAT__c", "Resposta_Survey__c", "Data_Final_Resolucao_do_Caso__c", "Quantidade_de_chamados__c", "Tags__c", "Nivel_10__c", "Nivel_5__c", "Nivel_6__c", "Nivel_7__c", "Nivel_8__c", "Nivel_9__c", "Situacao_Jira__c", "Caso_Classificado__c", "Priority_Jira__c", "Assunto_Dig__c", "Legado__c", "chamado_transferido__c", "Data_Cadastro_WFW__c", "Data_Conclusao_WFW__c", "Data_Fim_Previsto_WFW__c", "Data_Inicio_Real_WFW__c", "Data_Inicio_WFW__c", "Codigo_Fila_WFW__c", "Capital__c", "Email__c", "Endereco__c", "Produto_Conec__c", "Responsavel_no_Local__c", "Telefone_da_loja__c", "Tipo_de_solicitacao__c", "Indisponibilidade__c", "Produto6__c", "Servicos1__c", "Situacao__c", "Tipo_de_Solicitacao_Encerramento__c", "Situacao_Encerramento__c", "Expediente__c", "Produto5__c", "Nivel_4_Encerramento__c", "Nivel_5_Encerramento__c", "Nivel_6_Encerramento__c", "Servicos_Encerramento__c", "Indisponibilidade1__c", "Aguardando_Cliente_Validar__c", "Aguardando_Informa_o_Externa__c", "Case__c", "team__c", "Team_Jira__c", "RCA_Dimensao__c", "RCA_Detalhe__c", "Caso_reaberto__c", "Tipo_de_solicitacao_tools__c", "Data_Fim_Jira__c", "Data_Inicio_Jira__c", "SLA_em_Minutos_Jira__c", "Issue_type_JIRA__c", "Tipo_da_Issue__c", "isReOpen_Jira__c", "isDone_jira__c", "E_mail_da_Conta__c", "Nivel_Encerramento_1__c", "Nivel_Encerramento_2__c", "Nivel_Encerramento_3__c", "Nivel_Encerramento_4__c", "Nivel_Encerramento_5__c", "Nivel_Encerramento_6__c", "Farma_Associacoes_e_Redes__c", "Delay_55_secs__c", "Nivel_Encerramento_10__c", "Nivel_Encerramento_7__c", "Nivel_Encerramento_8__c", "Nivel_Encerramento_9__c", "data_release_jira__c", "fix_version_jira__c", "URL_da_Base_de_Conhecimento_Utilizada__c", "base_de_conhecimento__c", "Motivo_Contestacao__c", "Acesso_ao_portal_dos_canais__c", "Chamado_Aberto__c", "Descricao_de_cancelamento__c", "Escalado__c", "Created_By_Viewer__c", "Link_Numero_Caso__c", "Related_contact_View__c", "DDDCelular__c", "DDD__c", "MobilePhone__c", "Phone__c", "Email_BigRetail__c", "Celular_Solicitante__c", "Email_Solicitante__c", "Email_do_contato__c", "Telefone_Solicitante__c", "Direcionado_Fila_Conta__c", "Sem_Fila_Conta__c", "Notas_Acima_de_4__c", "Delay_X_secs__c", "DataMesclagem__c", "Solicitante_Jira__c", "VersaoAfetada_Jira__c", "Criado_Visualizador__c", "Unidade_de_Negocio_Visivel__c", "Caso_original__c", "Data_da_Mesclagem__c", "WebCnpjCpf__c", "Unidade_de_Negocio_de_Abertura__c", "Vertical_de_Abertura__c", "DataDesvinculoIssue__c", "DataVinculoIssue__c", "Proprietario_Violacao_SLA__c", "Unidade_de_negocio_violacao_SLA__c", "Data_primeira_interacao_feed__c", "Data_ultima_interacao_feed__c", "Aberto_pelo_Franqueado__c", "IsFranquia__c", "Registro_de_Chamada__c", "fmlCaseSourceID__c", "fml_Registro_de_Chamada_c__c", "EnderecoConta__c", "infos_zabbix__c", "Classificacao_de_abertura__c", "Resolvido_por__c", "IdContaLinx__c", "UnidadeNegocioCasoPai__c", "WO_encerrada__c", "WO_relacionada__c", "Urljira__c", "EquipeProprietarioCaso__c", "CreateCase__c", "ProdutoConectividade__c", "Attachments", "Avalia_o_Atendimento_Knowledge_Base__r", "CaseArticles", "CaseComments", "CaseContactRoles", "CaseExternalDocuments", "CaseMilestones", "CaseSolutions", "Cases", "Caso_original__r", "Casos_Contestacoes_de_Valores__r", "Casos__r", "CombinedAttachments", "Conta_Aberto__r", "Conta_Fechado__r", "Contact", "ContactRequests", "Contato_Aberto__r", "ContentDocumentLinks", "CreatedBy", "EmailMessages", "Emails", "EventRelations", "Events", "FeedSubscriptionsForEntity", "Histories", "IdContaLinx__r", "LastModifiedBy", "LiveChatTranscripts", "MasterRecord", "MessagingSessions", "OpenActivities", "Owner", "Parent", "Posts", "ProcessInstances", "ProcessSteps", "Product__r", "RecordActionHistories", "RecordActions", "RecordAssociatedGroups", "RecordType", "Registro_de_Chamada__r", "Registros_de_Chamadas__r", "RelatedRecords", "Resolvido_por__r", "Responsavel_no_Local__r", "Shares", "Solicitante__r", "Source", "SurveySubjectEntities", "TaskRelations", "Tasks", "TeamMembers", "TeamTemplateRecords", "TopicAssignments", "UnidadeNegocioCasoPai__r", "WorkOrders", "ActivityHistories", "AttachedContentDocuments", "AttachedContentNotes", "ArtigoAvaliado__c", "YouComm_Ticket_Number__c", "Id_Chamado_Maker__c", "Integracao_CNPJ__c", "Tratativa_Externa_Maker__c", "nAcionarCaseAssignmentRule__c", "Aberto_pelo_Franqueado__r"}),
				    #"Linhas Filtradas3" = Table.SelectRows(#"Colunas Removidas", each ([Nivel_2__c] = "RESCISÕES/REDUÇÕES")),
				    #"Linhas Filtradas2" = Table.SelectRows(#"Linhas Filtradas3", each ([Status] = "Aguardando Cliente Validar" or [Status] = "Aguardando Informacao Externa" or [Status] = "Em aberto" or [Status] = "Em Andamento"))
				in
				    #"Linhas Filtradas2"
```

### DateTableTemplate_6fb6f099-1102-4f7d-b901-075798021227

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: DateTableTemplate_6fb6f099-1102-4f7d-b901-075798021227
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_0030387a-34c6-4254-867c-15230e723c10

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_0030387a-34c6-4254-867c-15230e723c10
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_0ee656bd-eb1a-4472-b3d3-144b461b0773

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_0ee656bd-eb1a-4472-b3d3-144b461b0773
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_1369fba6-908e-4b9b-83a0-f421c5372327

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_1369fba6-908e-4b9b-83a0-f421c5372327
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_1b5aaa00-b31d-40ad-834a-92b60de2ec36

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_1b5aaa00-b31d-40ad-834a-92b60de2ec36
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_439dc46a-df99-4d4d-a100-7a45f67568b5

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_439dc46a-df99-4d4d-a100-7a45f67568b5
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_44e784ab-533c-48e9-859e-775326ec442d

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_44e784ab-533c-48e9-859e-775326ec442d
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_5584f828-ab45-425c-a186-9de4e674bd5e

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_5584f828-ab45-425c-a186-9de4e674bd5e
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_57d7976b-f683-4858-a4e9-e7a933658f5e

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_57d7976b-f683-4858-a4e9-e7a933658f5e
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_63f10bc6-f2d6-4c17-8c21-ae9cb06edec6

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_63f10bc6-f2d6-4c17-8c21-ae9cb06edec6
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_7ab4d899-d413-4ae1-a2d5-ff82066b9eb7

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_7ab4d899-d413-4ae1-a2d5-ff82066b9eb7
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_7c80d8e1-4ea6-4a84-931b-5595b755a429

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_7c80d8e1-4ea6-4a84-931b-5595b755a429
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_8026b180-0064-4953-aac6-b67e5d8fc584

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_8026b180-0064-4953-aac6-b67e5d8fc584
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_89f5acc6-3e97-4c14-ac6c-66ae87dd61e6

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_89f5acc6-3e97-4c14-ac6c-66ae87dd61e6
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_90c33327-c3c5-4b0d-b518-51ec0a1ae9a0

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_90c33327-c3c5-4b0d-b518-51ec0a1ae9a0
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_b09814b0-8e40-4bc2-a448-70fc5b485702

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_b09814b0-8e40-4bc2-a448-70fc5b485702
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_b6233a99-0417-4441-9644-a4d5f4832cf9

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_b6233a99-0417-4441-9644-a4d5f4832cf9
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_b90a5ab9-548f-4547-bfa4-2c6cd886ed7a

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_b90a5ab9-548f-4547-bfa4-2c6cd886ed7a
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_c6cfe084-ac9b-436b-a890-653770c230e8

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_c6cfe084-ac9b-436b-a890-653770c230e8
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_ccdc4f8f-2670-432a-b6bb-cc45b13a8a97

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_ccdc4f8f-2670-432a-b6bb-cc45b13a8a97
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_db03d42c-7fab-492b-b928-26a68881f5c1

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_db03d42c-7fab-492b-b928-26a68881f5c1
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_dd17a6ff-c312-4626-a5b1-e0db65296ede

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_dd17a6ff-c312-4626-a5b1-e0db65296ede
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_eec94af9-deb0-434d-b2df-2640fc0e6027

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_eec94af9-deb0-434d-b2df-2640fc0e6027
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_f97f66da-59cb-4ec4-a354-40374af70e6a

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_f97f66da-59cb-4ec4-a354-40374af70e6a
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### LocalDateTable_fae336ed-4cbb-423a-abac-b310ee6d55b2

### Colunas
- **Date** — `dateTime`
- **Ano** — `int64`
- **MonthNo** — `int64`
- **Mês** — `string`
- **QuarterNo** — `int64`
- **Trimestre** — `string`
- **Dia** — `int64`

### Medidas

### Consultas M (Partitions)
#### Partição: LocalDateTable_fae336ed-4cbb-423a-abac-b310ee6d55b2
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

### Medidas

### Colunas
- **Value** — `desconhecido`

### Medidas
#### Valor_Churn_Futuro
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[Descrição Mês Ano]>TODAY()))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### Valor_Churn_Geral
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### Valor_Churn_Ano_Atual
```DAX
```
			
			var churn = CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual',AND('0_Perdas_Detalhadas_Atual'[Descrição Mês Ano]<=TODAY(),'0_Perdas_Detalhadas_Atual'[Descrição Mês Ano].[Ano]=YEAR( TODAY() ))))
			var zeros = IF(churn=BLANK(),0,churn) 
			return 
			zeros 
			```
		displayFolder: Churn_Acumulado
```
#### Valor_Churn_Ano_Anterior
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual',AND('0_Perdas_Detalhadas_Atual'[Descrição Mês Ano]<=TODAY(),'0_Perdas_Detalhadas_Atual'[Descrição Mês Ano].[Ano]=2024)))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### Versao_Churning
```DAX
"v0.1"
		displayFolder: Churn_Acumulado
```
#### Valor_Churn_Ano_Atual_e_Futuro
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[Descrição Mês Ano].[Ano]=2025))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### Churn_Ano_Atual_e_Futuro_Rescisao
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual', AND('0_Perdas_Detalhadas_Atual'[Descrição Mês Ano].[Ano]=YEAR( TODAY() ),'0_Perdas_Detalhadas_Atual'[Tipo]="RESCISÃO")))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### Churn_Ano_Atual_e_Futuro_Reducao
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual', AND('0_Perdas_Detalhadas_Atual'[Descrição Mês Ano].[Ano]=YEAR( TODAY() ),'0_Perdas_Detalhadas_Atual'[Tipo]="REDUÇÃO")))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### Churn_Acumulado_Reducao_Atual
```DAX
CALCULATE([YTD_Churn_Atual_2026],
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[Tipo]="REDUÇÃO"))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### Churn_Acumulado_Rescisao_Atual
```DAX
CALCULATE([YTD_Churn_Atual_2026],
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[Tipo]="RESCISÃO"))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### Versao_Churn
```DAX
"v1.5"
```
#### Churn_Acumulado_Direto_Atual
```DAX
CALCULATE([YTD_Churn_Atual_2026],
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[GR_DIRETO_INDIRETO]="DIRETO"))
		displayFolder: Churn_Acumulado
```
#### Churn_Acumulado_Indireto_Atual
```DAX
CALCULATE([YTD_Churn_Atual_2026],
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[GR_DIRETO_INDIRETO]="INDIRETO"))
		displayFolder: Churn_Acumulado
```
#### Churn_MTD_Acumulado_AnoAnterior
```DAX
CALCULATE([Valor_Churn_Geral],DATEADD('1_Calendario'[Date],-1,YEAR),ALL('1_Calendario'))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### Churn_MAtual-MAnoAnterior
```DAX
[YTD_Churn_Atual_2026]-[Churn_MTD_Acumulado_AnoAnterior]
		displayFolder: Churn_Acumulado
```
#### DifAatual_Apassado
```DAX
[YTD_Churn_Atual_2026]-[YTD_Churn_Atual_Geral]
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### YTD_Churn_Atual_2024
```DAX
IF(CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),FILTER('1_Calendario','1_Calendario'[Date].[Ano]=2024))> 0,
			CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),FILTER('1_Calendario','1_Calendario'[Date].[Ano]=2024)),
			BLANK())
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### YTD_Churn_Atual_Geral
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### YTD_Diferenca_Atual
```DAX
[YTD_Churn_Atual_2026] - [YTD_Churn_Atual_2025]
		displayFolder: Churn_Acumulado
```
#### YTD_DiferencaPercent_Atual
```DAX
([YTD_Churn_Atual_2026] - [YTD_Churn_Atual_Geral]) / [YTD_Churn_Atual_Geral]
		formatString: 0%;-0%;0%
		displayFolder: Churn_Acumulado
```
#### BacklogTotal
```DAX
CALCULATE(COUNT('5_Casos'[CaseNumber]))
		formatString: 0
		displayFolder: Backlog
```
#### IdadeCaso
```DAX
AVERAGE('5_Casos'[Idade_do_Caso_em_Dias__c])
		displayFolder: Backlog
```
#### BacklogProcessado
```DAX
```
			
			var backlog = COUNT('5.1_1 2Ent_Churn_Backlog_Geral_Processado'[Número do caso])
			var zeros = IF(backlog=BLANK(),0,backlog) 
			return 
			zeros 
			```
		formatString: 0
		displayFolder: Backlog
```
#### Diferenca_Atual
```DAX
[YTD_Churn_Atual_Geral] - [Valor_Churn_Ano_Anterior]
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### DiferencaPercent_Atual
```DAX
([YTD_Churn_Atual_Geral] - [Valor_Churn_Ano_Anterior]) / [Valor_Churn_Ano_Anterior]
		formatString: 0.00%;-0.00%;0.00%
		displayFolder: Churn_Acumulado
```
#### BNP_Reducao_Rescisao
```DAX
```
			
			var valor = 
			CALCULATE(SUM('0_Backlog_NProcessado'[Valor Redução]),
			FILTER('0_Backlog_NProcessado','0_Backlog_NProcessado'[_SUBSUBTIPO]="REDUÇÃO/RESCISÃO"))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Backlog_NProcessado
```
#### BNP_Inadimplencia
```DAX
```
			
			var valor = 
			CALCULATE(SUM('0_Backlog_NProcessado'[Valor Redução]),
			FILTER('0_Backlog_NProcessado','0_Backlog_NProcessado'[_SUBSUBTIPO]="INADIMPLÊNCIA"))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Backlog_NProcessado
```
#### BNP_GR_Direto
```DAX
```
			
			var valor = 
			CALCULATE(SUM('0_Backlog_NProcessado'[Valor Redução]),
			FILTER('0_Backlog_NProcessado','0_Backlog_NProcessado'[_GR_DIRETO_INDIRETO]="DIRETO"))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Backlog_NProcessado
```
#### BNP_GR_InDireto
```DAX
```
			
			var valor = 
			CALCULATE(SUM('0_Backlog_NProcessado'[Valor Redução]),
			FILTER('0_Backlog_NProcessado','0_Backlog_NProcessado'[_GR_DIRETO_INDIRETO]="INDIRETO"))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Backlog_NProcessado
```
#### BNP_Valor
```DAX
```
			
			var valor = 
			CALCULATE(SUM('0_Backlog_NProcessado'[Valor Redução]))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			valor 
			```
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Backlog_NProcessado
```
#### COB_Valor_Receber
```DAX
CALCULATE(SUM('0_Cobranca'[Valor a Receber]))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Cobrança
```
#### COB_Valor_Vencido
```DAX
CALCULATE(SUM('0_Cobranca'[Valor Vencido + Juros e Multa]))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Cobrança
```
#### COB_Contestacoes
```DAX
```
			
			var valor = CALCULATE(COUNT('0_Cobranca'[Contestacao]),
			FILTER('0_Cobranca','0_Cobranca'[Contestacao]="SIM"))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: 0
		displayFolder: Cobrança
```
#### COB_Faturas
```DAX
```
			
			var valor = CALCULATE(COUNT('0_Cobranca'[Fatura]))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: 0
		displayFolder: Cobrança
```
#### COB_Clientes
```DAX
```
			
			var valor = CALCULATE(DISTINCTCOUNT('0_Cobranca'[CNPJ]))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: 0
		displayFolder: Cobrança
```
#### COB_Valor_Contestacoes
```DAX
```
			
			var valor = CALCULATE(SUM('0_Cobranca'[Valor a Receber]),
			FILTER('0_Cobranca','0_Cobranca'[Contestacao]="SIM"))
			var zeros = IF(valor=BLANK(),0,valor) 
			return 
			zeros 
			```
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Cobrança
```
#### Valor_Churn_2025
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[Descrição Mês Ano].[Ano] = 2025))
		formatString: "R$"\ #,0;-"R$"\ #,0;"R$"\ #,0
		displayFolder: Churn_Acumulado
```
#### BNP_Valor_pTotal
```DAX
```
			
			DIVIDE(
			    [BNP_Valor],
			    CALCULATE(
			        [BNP_Valor],
			        ALLSELECTED('0_Backlog_NProcessado')
			    )
			) 
			
			```
		formatString: 0%;-0%;0%
		displayFolder: Backlog_NProcessado
```
#### Valor_Churn_2025_pTotal
```DAX
DIVIDE(
			    [Valor_Churn_2025],
			    CALCULATE(
			        [Valor_Churn_2025],
			        ALLSELECTED('0_Perdas_Detalhadas_Atual')
			    )
			)
		formatString: 0%;-0%;0%
		displayFolder: Churn_Acumulado
```
#### YTD_Churn_Atual_Geral_pTotal
```DAX
DIVIDE(
				[Valor_Churn_Geral],
				CALCULATE(
			        	[Valor_Churn_Geral],
				        ALLSELECTED('0_Perdas_Detalhadas_Atual')
			    )
			)
		formatString: 0.00%;-0.00%;0.00%
		displayFolder: Churn_Acumulado
```
#### YTD_Churn_Atual_2026
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),FILTER('1_Calendario','1_Calendario'[Date].[Ano]=2026))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### Valor_Churn_2026
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			FILTER('0_Perdas_Detalhadas_Atual','0_Perdas_Detalhadas_Atual'[Descrição Mês Ano].[Ano] = 2026))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### LegendaMedida
```DAX
```
			
			IF(
			    ISBLANK([YTD_Churn_Atual_2024]),
			    BLANK(),
			    "Churn Atual 2024"
			)
			
			```
		displayFolder: Churn_Acumulado
```
#### LegendaSemBranco
```DAX
```
			
			IF(
			    ISBLANK('Tabela'[SuaLegenda]) || 'Tabela'[SuaLegenda] = "",
			    BLANK(),
			    'Tabela'[SuaLegenda]
			)
			
			```
		displayFolder: Churn_Acumulado
```
#### Legenda_Rescisao_2024
```DAX
IF(
			    NOT ISBLANK([ValorRescisao_2024]),
			    SELECTEDVALUE('0_Perdas_Detalhadas_Atual'[Valor Redução]),
			    BLANK()
			)
		displayFolder: Churn_Acumulado
```
#### YTD_Churn_Atual_2025
```DAX
CALCULATE(SUM('0_Perdas_Detalhadas_Atual'[Valor Redução]),FILTER('1_Calendario','1_Calendario'[Date].[Ano]=2025))
		formatString: "R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00
		displayFolder: Churn_Acumulado
```
#### Valor_Churn_Geral_pTotal
```DAX
DIVIDE(
			    [Valor_Churn_Geral],
			    CALCULATE(
			        [Valor_Churn_Geral],
			        ALLSELECTED('0_Perdas_Detalhadas_Atual')
			    )
			)
		formatString: 0%;-0%;0%
		displayFolder: Churn_Acumulado
```
#### YTD_Churn_Atual_2025_pTotal
```DAX
DIVIDE(
				[YTD_Churn_Atual_2025],
				CALCULATE(
			        	[YTD_Churn_Atual_2025],
				        ALLSELECTED('0_Perdas_Detalhadas_Atual')
			    )
			)
		formatString: 0.00%;-0.00%;0.00%
		displayFolder: Churn_Acumulado
```

### Consultas M (Partitions)
#### Partição: Medidas
**Etapas:**

**Fontes:**

**Dependências:**

**Código M Completo:**
```PowerQuery

```

## Relacionamentos

- **[0_Perdas_Detalhadas_Atual.'Data Inclusão'] → [LocalDateTable_dd17a6ff-c312-4626-a5b1-e0db65296ede.Date]**
- **[0_Perdas_Detalhadas_Atual.'Data Implantacao'] → [LocalDateTable_c6cfe084-ac9b-436b-a890-653770c230e8.Date]**
- **[0_Perdas_Detalhadas_Atual.'Data do Cadastro'] → [LocalDateTable_db03d42c-7fab-492b-b928-26a68881f5c1.Date]**
- **[0_Perdas_Detalhadas_Atual.'Data Conclusão'] → [LocalDateTable_b6233a99-0417-4441-9644-a4d5f4832cf9.Date]**
- **[0_Perdas_Detalhadas_2024.'Data Inclusão'] → [LocalDateTable_b90a5ab9-548f-4547-bfa4-2c6cd886ed7a.Date]**
- **[0_Perdas_Detalhadas_2024.'Data Implantacao'] → [LocalDateTable_1b5aaa00-b31d-40ad-834a-92b60de2ec36.Date]**
- **[0_Perdas_Detalhadas_2024.'Data do Cadastro'] → [LocalDateTable_44e784ab-533c-48e9-859e-775326ec442d.Date]**
- **[0_Perdas_Detalhadas_2024.'Data Conclusão'] → [LocalDateTable_7ab4d899-d413-4ae1-a2d5-ff82066b9eb7.Date]**
- **[0_Perdas_Detalhadas_2024.'Descrição Mês Ano'] → [LocalDateTable_1369fba6-908e-4b9b-83a0-f421c5372327.Date]**
- **[0_Perdas_Detalhadas_Atual.'Descrição Mês Ano'] → [LocalDateTable_fae336ed-4cbb-423a-abac-b310ee6d55b2.Date]**
- **[5_Casos.ClosedDate] → [LocalDateTable_eec94af9-deb0-434d-b2df-2640fc0e6027.Date]**
- **[5_Casos.Resolvido_em__c] → [LocalDateTable_90c33327-c3c5-4b0d-b518-51ec0a1ae9a0.Date]**
- **[5_Casos.Data_Final_SLA__c] → [LocalDateTable_b09814b0-8e40-4bc2-a448-70fc5b485702.Date]**
- **[5_Casos.Data_Inicio_SLA__c] → [LocalDateTable_ccdc4f8f-2670-432a-b6bb-cc45b13a8a97.Date]**
- **[5_Casos.LastModifiedDate] → [LocalDateTable_439dc46a-df99-4d4d-a100-7a45f67568b5.Date]**
- **['5.1_1 2Ent_Churn_Backlog_Geral_Processado'.'Data de criação'] → [LocalDateTable_f97f66da-59cb-4ec4-a354-40374af70e6a.Date]**
- **['5.1_1 2Ent_Churn_Backlog_Geral_Processado'.'Data da última modificação'] → [LocalDateTable_8026b180-0064-4953-aac6-b67e5d8fc584.Date]**
- **[0_Backlog_NProcessado.'Data do Cadastro'] → [LocalDateTable_57d7976b-f683-4858-a4e9-e7a933658f5e.Date]**
- **[0_Cobranca.Vencimento] → [LocalDateTable_7c80d8e1-4ea6-4a84-931b-5595b755a429.Date]**
- **[0_Cobranca.'Vencimento Real'] → [LocalDateTable_63f10bc6-f2d6-4c17-8c21-ae9cb06edec6.Date]**
- **[5_Casos.CreatedDate] → [LocalDateTable_0030387a-34c6-4254-867c-15230e723c10.Date]**
- **[1_Calendario.Date] → [LocalDateTable_0ee656bd-eb1a-4472-b3d3-144b461b0773.Date]**
- **[0_Perdas_Detalhadas_2024.DataReferencia] → [1_Calendario.Date]**
- **[0_Perdas_Detalhadas_Atual.DataReferencia] → [1_Calendario.Date]**
- **[0_Backlog_NProcessado.DataReferencia] → [1_Calendario.Date]**
- **[5_Casos._DataReferencia] → [1_Calendario.Date]**
- **['5.1_1 2Ent_Churn_Backlog_Geral_Processado'.DataReferencia] → [5_Casos._DataReferencia]**
- **[0_Cobranca.Emissão] → [1_Calendario.Date]**
- **[5_Casos.sftv__tvExpirationDate__c] → [LocalDateTable_89f5acc6-3e97-4c14-ac6c-66ae87dd61e6.Date]**
- **[1_Calendario.'Mes Ano'] → [LocalDateTable_5584f828-ab45-425c-a186-9de4e674bd5e.Date]**

## Páginas do Relatório e Visuais


### Aba: Acumulado Dados

### Quantidade de visuais por tipo
- **None**: 4

### Aba: Acumulado Analítico

### Quantidade de visuais por tipo
- **None**: 5

### Aba: Versão

### Quantidade de visuais por tipo
- **None**: 1

### Aba: Capa

### Quantidade de visuais por tipo

### Aba: Backlog Não Processado Dados

### Quantidade de visuais por tipo
- **None**: 2

### Aba: Acumulado por Tipo

### Quantidade de visuais por tipo
- **None**: 18

### Aba: Cobrança

### Quantidade de visuais por tipo
- **None**: 28

### Aba: Backlog

### Quantidade de visuais por tipo
- **None**: 26

### Aba: Backlog Não Processado

### Quantidade de visuais por tipo
- **None**: 31

### Aba: Acumulado

### Quantidade de visuais por tipo
- **None**: 26
