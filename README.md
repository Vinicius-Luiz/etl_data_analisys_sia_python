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
