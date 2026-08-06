# ⚽ Dashboard Copa do Mundo 2026 — Dados do Futebol transformados em Insights com Power BI

<p align="left">
  <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=000000" alt="Power BI"/>

  <img src="https://img.shields.io/badge/DAX-7F8C8D?style=for-the-badge&logoColor=000000" alt="DAX"/>

  <img src="https://img.shields.io/badge/Power%20Query-00B7C3?style=for-the-badge&logoColor=000000" alt="Power Query"/>

  <img src="https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=000000" alt="Excel"/>

  <img src="https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen?style=for-the-badge" alt="Status: Concluído"/>
</p>

Projeto de análise de dados desenvolvido no **Power BI** para explorar os principais acontecimentos da Copa do Mundo de 2026. O dashboard reúne indicadores, rankings, comparações e insights sobre **partidas, seleções, jogadores, estádios, gols, assistências, cartões e desempenho esportivo**.

O projeto foi criado para portfólio e aplica, de ponta a ponta, conceitos de **coleta e organização de dados, modelagem dimensional, Power Query, DAX, visualização de dados e experiência do usuário**.

---

## Sumário

- [Visão geral](#-visão-geral)
- [Objetivos do projeto](#-objetivos-do-projeto)
- [Fonte e preparação dos dados](#-fonte-e-preparação-dos-dados)
- [Modelagem de dados](#-modelagem-de-dados)
- [Principais medidas DAX](#-principais-medidas-dax)
- [Páginas do dashboard](#-páginas-do-dashboard)
- [Principais insights](#-principais-insights)
- [Destaques técnicos](#-destaques-técnicos)
- [Tecnologias utilizadas](#-tecnologias-utilizadas)
- [Como executar](#-como-executar)
- [Estrutura do repositório](#-estrutura-do-repositório)
- [Limitações dos dados](#-limitações-dos-dados)
- [Próximas melhorias](#-próximas-melhorias)
- [Aviso](#-aviso)
- [Autor](#-autor)

---

## Visão geral

A Copa do Mundo gera uma grande quantidade de dados sobre equipes, partidas, jogadores e estádios. Quando esses registros são analisados separadamente, torna-se mais difícil identificar padrões, comparar desempenhos e transformar números em conclusões úteis.

Este projeto organiza essas informações em um dashboard interativo capaz de responder perguntas como:

- Qual seleção marcou mais gols?
- Quais equipes conquistaram mais vitórias?
- Qual seleção apresentou o melhor aproveitamento?
- Quem foi o artilheiro e quem liderou em assistências?
- Quais partidas tiveram maior volume ofensivo?
- Quais estádios receberam mais jogos e gols?
- Existe relação entre finalizações e gols marcados?
- Qual equipe apresentou maior eficiência ofensiva?
- Qual seleção teve o melhor equilíbrio entre ataque e defesa?

---

## Objetivos do projeto

- Estruturar dados esportivos em um modelo dimensional.
- Consolidar o desempenho das seleções como mandantes e visitantes.
- Criar medidas DAX reutilizáveis e responsivas aos filtros.
- Desenvolver rankings, indicadores e cartões com textos dinâmicos.
- Comparar o desempenho de seleções, jogadores, partidas e estádios.
- Transformar métricas em insights claros e visualmente acessíveis.
- Construir uma experiência de navegação consistente entre as páginas.

---

## Fonte e preparação dos dados

Os dados foram organizados a partir dos **relatórios oficiais das partidas — Full Time Match Report da FIFA**. Entre as informações utilizadas estão:

- resultados e placares;
- gols e assistências;
- cartões amarelos e vermelhos;
- posse de bola;
- finalizações e chutes no alvo;
- escanteios;
- seleções e jogadores;
- estádios;
- grupos, rodadas e fases da competição.

Os registros foram reunidos manualmente em uma planilha do **Microsoft Excel**, tratados no **Power Query** e carregados no Power BI para modelagem e análise.

---

## Modelagem de dados

O modelo foi estruturado com tabelas de **dimensão** e **fato**, mantendo os dados descritivos separados dos registros de eventos e resultados.

| Tabela                | Tipo / função |
|-----------------------|---------------|
| `DimSelecao`          | **Dimensão.** Armazena nome da seleção, continente, confederação, ranking FIFA e quantidade de títulos. Também relaciona os jogadores às suas seleções. |
| `DimSelecaoCasa`      | **Dimensão de papel.** Representa a seleção mandante e mantém ativo o relacionamento com `FatoPartidas[id_selecao_casa]`. |
| `DimSelecaoFora`      | **Dimensão de papel.** Representa a seleção visitante e mantém ativo o relacionamento com `FatoPartidas[id_selecao_fora]`. |
| `DimJogador`          | **Dimensão.** Contém o identificador, o nome e a seleção dos jogadores presentes nos registros de eventos. |
| `DimEstadio`          | **Dimensão.** Reúne nome oficial, nome utilizado na competição, cidade, país e capacidade dos estádios. |
| `FatoPartidas`        | **Tabela fato.** Registra cada partida, incluindo data, fase, grupo, estádio, seleções, placar e estatísticas coletivas. |
| `FatoPartidasJogador` | **Tabela fato.** Registra eventos individuais por partida, como gols, assistências, cartões e gols contra. |

### Relacionamentos principais

```text
DimSelecaoCasa[id_selecao]
1 ─── * FatoPartidas[id_selecao_casa]

DimSelecaoFora[id_selecao]
1 ─── * FatoPartidas[id_selecao_fora]

DimEstadio[id_estadio]
1 ─── * FatoPartidas[id_estadio]

FatoPartidas[id_jogo]
1 ─── * FatoPartidasJogador[id_jogo]

DimJogador[id_jogador]
1 ─── * FatoPartidasJogador[id_jogador]

DimSelecao[id_selecao]
1 ─── * DimJogador[id_selecao]
```

Os relacionamentos foram configurados com cardinalidade **um para muitos**, direção de filtro **única** e status **ativo**.

### Decisão de modelagem

As tabelas `DimSelecaoCasa` e `DimSelecaoFora` foram criadas porque uma seleção pode assumir dois papéis em uma partida: mandante ou visitante. Essa abordagem, conhecida como **role-playing dimension**, permite manter os dois relacionamentos ativos e evita caminhos ambíguos no modelo.

---

## Principais medidas DAX

As medidas DAX foram desenvolvidas para produzir indicadores confiáveis e dinâmicos em diferentes contextos de filtro.

### Indicadores gerais

- total de jogos;
- total e média de gols;
- total de finalizações;
- chutes no alvo e precisão;
- conversão de finalizações em gols;
- escanteios;
- cartões amarelos e vermelhos.

### Indicadores por seleção

- jogos, vitórias, empates e derrotas;
- gols marcados e sofridos;
- saldo de gols;
- pontos e aproveitamento;
- média de gols;
- posse média;
- finalizações e chutes no alvo;
- eficiência ofensiva;
- cartões.

### Indicadores de jogadores e estádios

- artilheiro;
- líder de assistências;
- participações em gols;
- cartões por jogador;
- estádio com mais jogos;
- estádio com mais gols;
- média de gols por estádio;
- maior capacidade.

### Funções DAX utilizadas

- agregação: `SUM`, `COUNTROWS`, `DISTINCTCOUNT`;
- proporções e médias: `DIVIDE`, `AVERAGEX`;
- contexto de filtro: `CALCULATE`, `FILTER`, `VALUES`, `ALL`, `ALLSELECTED`;
- relacionamento virtual: `TREATAS`;
- tabelas virtuais: `ADDCOLUMNS`, `SELECTCOLUMNS`;
- rankings e extremos: `TOPN`, `MAXX`, `MINX`;
- tratamento de valores vazios: `COALESCE`, `ISBLANK`;
- textos dinâmicos: `FORMAT`, `CONCATENATEX`;
- organização dos cálculos: `VAR` e `RETURN`.

As medidas de destaque também tratam **empates na liderança**, exibindo todas as equipes ou jogadores que compartilham o maior valor.

---

## Páginas do dashboard

### 1️⃣ Visão Geral

Resumo executivo da competição, com:

- campeão;
- total de jogos e gols;
- média de gols por partida;
- finalizações, chutes no alvo e precisão;
- conversão ofensiva;
- escanteios e cartões;
- gols por grupo;
- evolução dos gols por dia;
- distribuição por confederação;
- mapa das seleções participantes.

### 2️⃣ Seleções

Comparação do desempenho coletivo:

- seleções com mais gols e vitórias;
- melhor saldo de gols;
- melhor aproveitamento;
- rankings por gols e aproveitamento;
- tabela com jogos, vitórias, empates, derrotas, gols, saldo, pontos e aproveitamento.

### 3️⃣ Jogadores

Análise dos destaques individuais:

- artilheiro;
- líder de assistências;
- maior participação em gols;
- gols, assistências, cartões e gols contra por jogador.

### 4️⃣ Partidas

Análise detalhada jogo a jogo:

- placar;
- posse de bola;
- finalizações;
- chutes no alvo;
- escanteios;
- cartões;
- eventos individuais dos jogadores;
- filtros por partida, fase, grupo, estádio e seleções.

### 5️⃣ Estádios

Comparação dos locais da competição:

- estádio com mais jogos;
- estádio com mais gols;
- maior capacidade;
- média de gols por estádio;
- jogos, gols e capacidade por estádio;
- tabela com cidade, país e indicadores de utilização.

### 6️⃣ Insights

Página dedicada a conclusões analíticas, sem repetir apenas os rankings das demais páginas:

- seleção com maior eficiência ofensiva;
- equipe com melhor defesa;
- seleção com maior saldo médio por partida;
- partida com maior diferença de gols;
- relação entre finalizações e gols marcados;
- comparação entre gols marcados e sofridos;
- equilíbrio entre desempenho ofensivo e defensivo.

---

## Principais insights

O dashboard permite identificar padrões como:

- uma equipe pode finalizar mais e ainda apresentar baixa eficiência ofensiva;
- a seleção com mais gols não necessariamente possui o melhor aproveitamento;
- o artilheiro pode não ser o jogador com mais participações em gols;
- uma equipe ofensiva pode também sofrer muitos gols;
- o estádio com mais partidas pode não possuir a maior média de gols;
- o volume de finalizações pode apresentar relação positiva com a quantidade de gols, mas não garante conversão eficiente.

A página de Insights utiliza cartões textuais e gráficos de dispersão para transformar essas relações em conclusões mais fáceis de interpretar.

---

## Destaques técnicos

- Construção de modelo dimensional com tabelas fato e dimensão.
- Uso de dimensões de papel para seleções mandantes e visitantes.
- Consolidação das estatísticas das equipes nos dois contextos da partida.
- Medidas DAX sensíveis a filtros de seleção, fase, grupo, partida e estádio.
- Rankings com filtro Top N.
- Tratamento de empates em medidas de liderança.
- Cartões com frases dinâmicas.
- Tratamento de valores nulos com `COALESCE`.
- Comparações por meio de gráficos de dispersão.
- Navegação entre páginas e segmentações interativas.
- Padronização visual e preocupação com hierarquia de informação.

---

## Tecnologias utilizadas

| Tecnologia           | Aplicação no projeto                                           |
|----------------------|----------------------------------------------------------------|
| **Power BI Desktop** | Modelagem, criação dos visuais e desenvolvimento do dashboard. |
| **Power Query**      | Limpeza, transformação e preparação dos dados.                 |
| **DAX**              | Criação de medidas, rankings, indicadores e textos dinâmicos.  |
| **Microsoft Excel**  | Organização inicial e armazenamento da base de dados.          |
| **Canva**            | Criação autoral dos backgrounds e assets visuais do dashboard  |
| **Git**              | Versionamento do projeto.                                      |
| **GitHub**           | Documentação e publicação do portfólio.                        |

---

## Como executar

1. Clone ou baixe este repositório.
2. Abra o arquivo `.pbix` no **Power BI Desktop**.
3. Caso necessário, altere o caminho da planilha em **Transformar dados → Configurações da fonte de dados**.
4. Clique em **Atualizar** para recarregar os dados.
5. Navegue pelas páginas e utilize os filtros para explorar o relatório.

> É necessário ter o Power BI Desktop instalado para abrir o arquivo `.pbix`.

---

## Estrutura do repositório

```text
copa-do-mundo-2026-power-bi/
│
├── assets/
│   ├── Visão Geral.png
│   ├── Seleções.png
│   ├── Jogadores.png
│   ├── Partidas.png
│   ├── Estádios.png
│   └── Insights.png
│
├── dashboard-copa-do-mundo.pbix
│
├── CopaMundo2026_DadosPowerBI.xlsx
│
└── README.md
```
---

## Limitações dos dados

- A coleta foi realizada manualmente a partir dos relatórios oficiais das partidas.
- A tabela `FatoPartidasJogador` contém apenas jogadores com algum evento registrado, como gol, assistência, cartão ou gol contra.
- Por esse motivo, o projeto não analisa participação completa do elenco, minutos jogados ou jogadores sem eventos.
- Resultados e insights dependem da integridade e atualização da planilha utilizada como fonte.

---

## Aviso

Este é um projeto acadêmico e independente, desenvolvido para fins de estudo e portfólio.

O projeto não possui vínculo, patrocínio ou associação oficial com a FIFA ou com os organizadores da competição.

---

## Autor

Desenvolvido por **Jorge Gabriel Modrow**.

Estudante de Tecnologia em Análise e Desenvolvimento de Sistemas na **Universidade Federal do Paraná — UFPR**.

- LinkedIn: [Jorge Gabriel Modrow](https://www.linkedin.com/in/jorgemodrow/)
- GitHub: [Jorge Gabriel Modrow](https://github.com/jorgemodrow)
