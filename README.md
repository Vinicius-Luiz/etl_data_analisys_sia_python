# ETL e Análise de dados - Sistema de Informação Ambulatorial
Extração, Transformação, Carregamento e Análise da base de dados do Sistema de Informação Ambulatorial (SIA) no estado de Pernambuco



## Descrição da base de dados

O Sistema de Informação Ambulatorial (SIA) foi implantado nacionalmente na década de noventa (Julho de 1994), visando o registro dos atendimentos realizados no âmbito ambulatorial, por meio do Boletim de Produção Ambulatorial (BPA). Ao longo dos anos, o SIA vem sendo aprimorado para ser efetivamente um sistema que gere informações referentes ao atendimento ambulatorial e que possa subsidiar os gestores estaduais e municipais no monitoramento dos processos de planejamento, programação, regulação, avaliação e controle dos serviços de saúde, na área ambulatorial.

Com a evolução do SUS para uma gestão cada vez mais descentralizada, o Ministério da Saúde (MS), necessitou de dispor de um sistema de informação para o registro dos atendimentos ambulatoriais, padronizado em nível nacional, que possibilitasse a geração de informações facilitando o processo de planejamento, controle, avaliação e auditoria. 

### Contexto da coleta dos dados

**Entradas**

- **SIGTAP (Sistema de Gerenciamento da Tabela de Procedimentos, Medicamentos e Órteses, Próteses e Materiais Especiais do SUS):** Sistema fundamental para o gerenciamento e a atualização da tabela de procedimentos, medicamentos e OPM (e Procedimentos, Medicamentos e Órteses, Próteses e Materiais Especiais) utilizados pelo SUS.
- **CNES (Cadastro Nacional de Estabelecimentos de Saúde):** Sistema fundamental para o gerenciamento e a atualização do cadastro de todos os estabelecimentos de saúde do país, contribuindo para o planejamento, a gestão e o controle dos serviços de saúde em todo o território nacional.
- **FPOmag**: Fundo que apoia a execução de projetos estratégicos que promovam o desenvolvimento sustentável nessas regiões.

**Saídas**

- **TABNET:** Instrumento online tabulador de diversas informações de saúde. Há um módulo específico desta ferramenta na página do DATASUS para consulta da produção ambulatorial. 
- **TABWIN:** Aplicativo tabulador de informações de saúde para Windows. Todos os arquivos de configuração (DEF/CNV) e de produção (PA) necessários para que o Tabwin consulte a produção ambulatorial estão disponíveis no site do DATASUS. Quando esta ferramenta é utilizada para este fim é também denominada TABSIA.
- **MSBBS/DATASUS:** Os arquivos de produção ambulatorial (PA) podem ser obtidos sem necessidade de utilizar o Tabwin. Por serem arquivos Dbase (DBF) compactados, podem ser importados e tratados por outras ferramentas de banco de dados.

**Disponibilidade dos dados**

- **Tempo Previsto:** A divulgação nacional dos resultados é realizada mensalmente.
- **Nível de Divulgação:** Nacional, com detalhamento no nível estadual, municipal e de estabelecimentos.
- **Formas de Disseminação:** Internet, boletins, anuários, CD-ROM.

### Desafios existentes nos dados nesse Sistema de Informação Ambulatorial (SIS)

- **Qualidade dos dados:** Os dados podem estar incompletos, inconsistentes ou incorretos, o que pode comprometer sua utilidade para a tomada de decisão em saúde.
- **Padronização dos dados:** Isso pode dificultar a análise dos dados e a comparação entre diferentes regiões ou unidades de saúde.
- **Subnotificação:** A subnotificação de casos e procedimentos é outro desafio que pode comprometer a precisão dos dados do SIA.

## Levantamento da necessidade

1. Identificar os **procedimentos** realizados por **profissionais** não especializados
2. Identificar os municípios recebem valor de **verba** inconsistente em relação a quantidade de **atendimentos realizados**.

## Desafios do projeto

- Base de dados só pode ser extraída em no máximo **duas dimensões;**
- Dados não seguem **padrões de relacionamento.**

## Descrição das variáveis utilizadas

| Variável                  | Descrição                                                    |
| ------------------------- | ------------------------------------------------------------ |
| Município                 | Município de Pernambuco                                      |
| Profissional - CBO        | Profissional segundo a Classificação Brasileira de Ocupações |
| Grupo Procedimento        | Grupo do procedimento realizado, de acordo com a Tabela de Procedimentos, Medicamentos, Órteses e Próteses e Materiais Especiais do SUS |
| Subgrupo Procedimento     | Subgrupo do procedimento realizado, de acordo com a Tabela de Procedimentos, Medicamentos, Órteses e Próteses e Materiais Especiais do SUS |
| Complexidade              | Subgrupo do procedimento realizado, de acordo com a Tabela de Procedimentos, Medicamentos, Órteses e Próteses e Materiais Especiais do SUS |
| Ano/Mês processamento     | Ano e Mês de processamento/movimento dos dados.              |
| Qtd. aprovada             | Quantidade de procedimentos aprovados para pagamento pelas Secretarias de Saúde. |
| Valor aprovado            | Valor aprovado para pagamento pelas Secretarias de Saúde.    |
| Indicadores de evolução   | Indicadores utilizados para monitorar a evolução e as atividades dos pacientes (Encerramento, Alta, Transferência, Óbito) |
| Informações de residência | Informações de residência do paciente (Estado ou Município do paciente é diferente da localização do estabelecimento) |

## Coleta e tratamento de dados do SIA

### Fase 1: Extração de ocorrências do SIA - PySUS

***Arquivo: 001_SIA_PySUS.ipynb***

1. Instalado a biblioteca `pysus` usando o comando `!pip install pysus`.
2. Importado as funcionalidades relacionadas ao SIA do pacote `pysus`.
3. Definido o período de anos (2021, 2022 e 2023) e meses (de 1 a 12) para extração dos dados.
4. Iterado pelos anos e meses desejados.
5. Para cada mês e ano, foi baixado os dados do SIA para o estado de Pernambuco (PE) usando a função `download`. Os dados foram obtidos através do tipo de dado Produção Ambulatorial **(group='PA')**
6. Se os dados estiveram disponíveis, foi convertido de formato Parquet em um DataFrame do Pandas
7. Selecionado apenas as colunas relevantes para análise a partir de um dicionário chamado `selected_columns`
8. Transformado a coluna de data **'PA_CMP' (Data da Realização do Procedimento)** de formato *YYYYMM* para *DD/MM/YYYY*
9. Criado colunas adicionais **'PA_G_PROC_ID' (Procedimento-Grupo)** e **'PA_SG_PROC_ID' (Procedimento-Subgrupo)** com base na coluna **'PA_PROC_ID' (Procedimento realizado)**
10. Convertido os valores contábeis em formato string para números inteiros **'PA_QTDAPR' (Quantidade aprovada)** e números de ponto flutuante **'PA_VALAPR' (Valor aprovado)**
11. Foi salvo DataFrame em um arquivo CSV com um nome específico para o mês e ano correspondentes no caminho especificado ***(Ex: SIA_2023_08.csv)***.

### Fase 2: Extração da descrição dos códigos - TabNet

1. Os dados descritivos foram obtidos a partir do TabNet do DataSUS.

2. Inicialmente, foi selecionado a variável que desejamos na opção de **Linha.**

   <img src="_images/tabnet_001.png" width="70%"></img>

3. Ao inspecionar o elemento que contém o texto csv, é extraído os dados.

   <img src="_images/tabnet_002.png" width="70%"></img>

4. É realizado um breve tratamento na remoção de aspas no arquivo.

   <img src="_images/tabnet_003.png" width="70%"></img>

5. Foi feita uma operação para criar duas novas colunas no DataFrame **(<variavel>**e **DS_<variavel>)**. Que contém o código e a descrição numérico do variável, respectivamente.

### Fase 3: Conversão e manipulação dos arquivos CSV para Parquet
A Fazer

## Referências

*Produção Ambulatorial do SUS por local de atendimento – a partir de 2008 - Notas Técnicas. Disponível em: http://tabnet.datasus.gov.br/cgi/sia/Prod_amb_loc_atend_2008.pdf*

*AUDITORIA NO SUS: NOÇÕES BÁSICAS SOBRE SISTEMAS DE INFORMAÇÃO. Disponível em: https://bvsms.saude.gov.br/bvs/publicacoes/auditoria_sus.pdf*

*Auditoria no SUS: noções básicas sobre Sistemas de Informações. Disponível em: https://bvsms.saude.gov.br/bvs/publicacoes/auditoria_sus2004_2.pdf*

*Disseminação de Informações do Sistema de Informações Ambulatoriais (SIA). Disponível em: http://w3.datasus.gov.br/siasih/Arquivos/SIASUS_Informe_T%C3%A9cnico_2008-01.pdf*

*ESTUDO DE VIABILIDADE DE REÚSO DA ONTOLOGIA PLP (Patient Life Path) PARA INTEGRAÇÃO SEMÂNTICA DE DADOS ABERTOS DA ÁREA DA SAÚDE. Disponível em: https://repositorio.utfpr.edu.br/jspui/bitstream/1/23715/1/CT_COSIS_2019_2_07.pdf*
