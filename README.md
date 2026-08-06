# ⚽ Dashboard Copa do Mundo 2026 – Como transformei Dados do Futebol em Insights com Power BI
<p align="left">
  <img src="https://img.shields.io/badge/power_bi-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/status-em%20andamento-yellow?style=for-the-badge" alt="Status: Em andamento"/>
</p>

Projeto de dashboard sobre a Copa do Mundo de 2026, desenvolvido no Power BI com foco na análise de partidas, seleções, jogadores, estádios, gols, assistências, cartões e estatísticas de desempenho. O objetivo foi transformar os dados da competição em uma experiência interativa, capaz de explorar os principais acontecimentos do torneio por meio de indicadores, rankings, comparações e insights, respondendo a perguntas como quais seleções marcaram mais gols, conquistaram mais vitórias, apresentaram o melhor aproveitamento, tiveram maior eficiência ofensiva ou melhor equilíbrio entre ataque e defesa, além de identificar o artilheiro, o líder de assistências, as partidas com maior volume ofensivo e os estádios que receberam mais jogos e gols. Construído como projeto de portfólio, o dashboard aplica conceitos de modelagem dimensional, tratamento e organização de dados, criação de medidas DAX, visualização de dados e experiência do usuário.

---

## Fonte dos dados

Os dados foram organizados a partir dos relatórios oficiais das partidas (Full Time Match Report da FIFA), incluindo informações como:

- resultados;
- gols;
- assistências;
- cartões;
- posse de bola;
- finalizações;
- chutes no alvo;
- escanteios;
- estádios;
- seleções;
- jogadores;
- fases e grupos da competição.

Os dados foram reunidos e estruturados manualmente em uma planilha do Excel antes de serem carregados no Power BI.

---

## Tecnologias utilizadas

- **Microsoft Power BI Desktop**
- **DAX**
- **Power Query**
- **Microsoft Excel**
- **Git**
- **GitHub**
- **Modelagem dimensional**
- **Visualização e análise de dados**

---

## Contextualização

A Copa do Mundo reúne uma grande quantidade de dados relacionados a partidas, seleções, jogadores, estádios e desempenho esportivo. Quando essas informações são analisadas isoladamente, torna-se mais difícil identificar padrões, comparar equipes e compreender os principais acontecimentos da competição.

Este projeto foi desenvolvido com o objetivo de transformar esses dados em um dashboard interativo no Power BI, permitindo uma análise mais clara e organizada do torneio. A solução apresenta indicadores gerais, rankings, comparações e insights sobre gols, vitórias, aproveitamento, assistências, cartões, finalizações, posse de bola e desempenho das seleções ao longo da competição.

Antes da construção dos visuais, os dados foram organizados no Microsoft Excel e estruturados por meio de um modelo dimensional, separando as informações descritivas em tabelas de dimensão e os registros de acontecimentos em tabelas fato. Essa organização facilitou a criação dos relacionamentos, das medidas DAX e dos filtros utilizados no relatório.

## Estrutura das tabelas

| Nome da tabela | Tipo/Função |
|---|---|
| `DimSelecao` | **Dimensão.** Armazena as informações descritivas das seleções, como nome, continente, confederação, ranking FIFA e quantidade de títulos mundiais. Também é utilizada para relacionar cada jogador à sua respectiva seleção. |
| `DimSelecaoCasa` | **Dimensão de função.** Representa a seleção mandante de cada partida e permite manter ativo o relacionamento com `FatoPartidas[id_selecao_casa]`. |
| `DimSelecaoFora` | **Dimensão de função.** Representa a seleção visitante de cada partida e permite manter ativo o relacionamento com `FatoPartidas[id_selecao_fora]`. |
| `DimJogador` | **Dimensão.** Contém o identificador, o nome e a seleção de cada jogador presente nos registros de eventos da competição. |
| `DimEstadio` | **Dimensão.** Reúne informações sobre os estádios, como nome oficial, nome utilizado na competição, cidade, país e capacidade. |
| `FatoPartidas` | **Tabela fato.** Armazena os registros de cada partida, incluindo data, fase, grupo, estádio, seleções participantes, placar, posse de bola, finalizações, chutes no alvo, escanteios e cartões. |
| `FatoPartidasJogador` | **Tabela fato.** Registra os eventos individuais dos jogadores em cada partida, como gols, assistências, cartões amarelos, cartões vermelhos e gols contra. |

A separação entre dimensões e fatos permitiu que o modelo fosse construído com relacionamentos do tipo um para muitos, mantendo as tabelas descritivas no lado 1 e as tabelas com registros de acontecimentos no lado *.

As tabelas DimSelecaoCasa e DimSelecaoFora foram criadas porque uma mesma seleção pode assumir dois papéis diferentes em uma partida: mandante ou visitante. Com essa estrutura, os dois relacionamentos permanecem ativos ao mesmo tempo, evitando caminhos ambíguos no modelo do Power BI.

---

## Relacionamentos do modelo

Os principais relacionamentos utilizados foram:

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

## Principais Medidas DAX

As medidas DAX foram criadas para fornecer indicadores confiáveis sobre o desempenho da Copa do Mundo de 2026 e permitir análises dinâmicas em todo o dashboard. O principal objetivo foi calcular métricas relacionadas a **partidas, gols, vitórias, empates, derrotas, aproveitamento, saldo de gols, finalizações, posse de bola, cartões, assistências e desempenho individual de jogadores e seleções**.

Como uma seleção pode aparecer tanto como mandante quanto como visitante, várias medidas foram desenvolvidas considerando os dois contextos. Dessa forma, foi possível consolidar corretamente os resultados de cada equipe, independentemente da posição ocupada na partida.

Também foram criadas medidas textuais para apresentar informações completas em cartões, como:

- seleção com mais gols;
- seleção com mais vitórias;
- melhor saldo de gols;
- melhor aproveitamento;
- artilheiro;
- líder de assistências;
- estádio com mais jogos;
- estádio com mais gols;
- seleção com maior eficiência ofensiva;
- equipe com melhor defesa.

As medidas também tratam situações de empate na liderança, exibindo todas as seleções ou jogadores que possuem o mesmo resultado.

As funções DAX mais utilizadas no projeto incluem:

- agregações: `SUM`, `COUNTROWS`, `DISTINCTCOUNT`;
- proporções e médias: `DIVIDE`, `AVERAGEX`;
- controle de contexto: `CALCULATE`, `FILTER`, `VALUES`, `ALL`, `ALLSELECTED`;
- relacionamento virtual entre tabelas: `TREATAS`;
- criação de tabelas virtuais: `ADDCOLUMNS`, `SELECTCOLUMNS`;
- rankings e busca de maiores valores: `TOPN`, `MAXX`, `MINX`;
- tratamento de valores vazios: `COALESCE`, `ISBLANK`;
- criação de textos dinâmicos: `FORMAT`, `CONCATENATEX`;
- organização dos cálculos: `VAR` e `RETURN`.

---

## Páginas do Dashboard

### 1️⃣ Visão Geral

Apresenta uma visão consolidada dos principais indicadores da competição, incluindo:

- campeão;
- total de jogos;
- total de gols;
- média de gols por partida;
- total de finalizações;
- chutes no alvo;
- precisão dos chutes;
- conversão das finalizações em gols;
- escanteios;
- cartões amarelos;
- cartões vermelhos.

Também apresenta visualizações como:

- gols por grupo;
- evolução dos gols por dia;
- distribuição das seleções por confederação;
- mapa das seleções participantes.

---

### 2️⃣ Seleções

Página voltada para a comparação do desempenho das equipes.

Entre os principais indicadores estão:

- seleção com mais gols;
- seleção com mais vitórias;
- melhor saldo de gols;
- melhor aproveitamento;
- ranking de gols por seleção;
- ranking de vitórias;
- ranking de aproveitamento;
- saldo de gols;
- posse média;
- finalizações;
- gols sofridos.

A página também possui uma tabela completa com:

- seleção;
- jogos;
- vitórias;
- empates;
- derrotas;
- gols marcados;
- gols sofridos;
- saldo de gols;
- pontos;
- aproveitamento.

---

### 3️⃣ Jogadores

Página destinada à análise dos destaques individuais da competição.

Os principais indicadores apresentados são:

- artilheiro;
- líder de assistências;
- jogador com mais participações em gols;
- jogadores com mais cartões;
- gols contra.

A página também apresenta uma tabela com:

- jogador;
- seleção;
- gols;
- assistências;
- participações em gols;
- cartões amarelos;
- cartões vermelhos;
- gols contra.

---

### 4️⃣ Partidas

Permite analisar detalhadamente cada jogo da competição.

A página apresenta filtros por:

- partida;
- fase;
- grupo;
- estádio;
- seleção da casa;
- seleção visitante.

Também são exibidas comparações entre as equipes em indicadores como:

- placar;
- posse de bola;
- finalizações;
- chutes no alvo;
- escanteios;
- cartões amarelos;
- cartões vermelhos.

Além disso, uma tabela apresenta os eventos individuais dos jogadores em cada partida.

---

### 5️⃣ Estádios

Página dedicada à análise dos estádios utilizados durante a competição.

Entre os indicadores apresentados estão:

- estádio com mais jogos;
- estádio com mais gols;
- estádio com maior capacidade;
- média de gols por estádio.

As principais visualizações incluem:

- quantidade de jogos por estádio;
- total de gols por estádio;
- capacidade dos estádios;
- média de gols por partida;
- comparação entre cidades e países-sede.

A página também apresenta uma tabela com informações como:

- nome oficial do estádio;
- nome utilizado na competição;
- cidade;
- país;
- capacidade;
- quantidade de jogos;
- total de gols;
- média de gols por partida.

---

### 6️⃣ Insights

A página de Insights foi criada para apresentar conclusões obtidas a partir dos dados, evitando apenas repetir rankings e indicadores já apresentados nas outras páginas.

Entre as análises desenvolvidas estão:

- seleção com maior eficiência ofensiva;
- equipe com melhor defesa;
- seleção com maior saldo médio por partida;
- partida com maior diferença de gols;
- relação entre finalizações e gols marcados;
- comparação entre gols marcados e gols sofridos;
- equilíbrio entre desempenho ofensivo e defensivo.

Também foram utilizados gráficos de dispersão para analisar relações entre diferentes indicadores.

Exemplos:

- finalizações x gols;
- gols marcados x gols sofridos;
- volume ofensivo x aproveitamento;
- eficiência ofensiva x desempenho geral.

---

## Principais Insights

O dashboard permite identificar rapidamente padrões de desempenho e comparar seleções, jogadores, partidas e estádios de maneira mais clara.

### Eficiência ofensiva

Permite identificar quais seleções conseguiram transformar um maior percentual de suas finalizações em gols.

*Exemplo:* Uma seleção pode não ter liderado em número total de finalizações, mas ainda assim apresentar a melhor taxa de conversão ofensiva da competição.

### Volume ofensivo

A comparação entre finalizações e gols ajuda a avaliar se as equipes que mais atacaram também foram as que mais marcaram.

*Exemplo:* Uma equipe pode apresentar alto volume de finalizações, mas baixa eficiência, enquanto outra pode marcar mais gols mesmo finalizando menos.

### Desempenho das seleções

Os rankings de gols, vitórias, saldo e aproveitamento permitem identificar as seleções mais consistentes da competição.

*Exemplo:* Uma equipe pode liderar em gols marcados, enquanto outra apresenta melhor aproveitamento devido a um número maior de vitórias e menor quantidade de derrotas.

### Equilíbrio entre ataque e defesa

A comparação entre gols marcados e gols sofridos permite identificar seleções ofensivas, defensivas ou equilibradas.

*Exemplo:* Algumas equipes podem marcar muitos gols, mas também sofrer uma quantidade elevada, enquanto outras apresentam menor volume ofensivo e maior solidez defensiva.

### Destaques individuais

A análise dos jogadores permite identificar os principais responsáveis pelos gols, assistências e participações ofensivas.

*Exemplo:* O artilheiro da competição pode não ser o mesmo jogador com maior número de participações em gols, já que assistências também são consideradas.

### Estádios

A página de estádios permite comparar a quantidade de partidas, gols e capacidade de cada local.

*Exemplo:* Um estádio pode receber mais jogos, enquanto outro apresenta maior média de gols por partida.

### Partidas

A análise detalhada permite identificar jogos com maior volume ofensivo, maior diferença de gols ou maior quantidade de cartões.

*Exemplo:* A partida mais dominante pode ser definida pela maior diferença entre os gols marcados pelas duas equipes.

---

## Conclusões

Ao finalizar este dashboard, foi possível compreender como a organização dos dados e a utilização do Power BI podem transformar uma grande quantidade de registros esportivos em informações claras, interativas e relevantes.

O projeto começou com a coleta e organização dos dados em planilhas do Excel. Em seguida, as informações foram estruturadas em tabelas dimensão e fato, criando um modelo dimensional capaz de representar seleções, jogadores, partidas e estádios.

Um dos principais desafios foi tratar o fato de que uma mesma seleção pode aparecer como mandante ou visitante. Para resolver esse problema, foram criadas dimensões específicas para seleção da casa e seleção visitante, permitindo manter os relacionamentos ativos e evitar caminhos ambíguos no modelo.

Com as medidas DAX, foi possível consolidar os resultados das equipes nos dois contextos, criar rankings, calcular indicadores de desempenho e desenvolver textos automáticos para os cartões de destaque.

O projeto também reforçou a importância de não apenas apresentar números, mas transformar os resultados em análises compreensíveis. Por isso, a página de Insights foi desenvolvida para destacar relações entre finalizações, gols, eficiência ofensiva, equilíbrio defensivo e desempenho das seleções.

Este dashboard representa a aplicação prática de conceitos de **Business Intelligence, modelagem dimensional, análise de dados, DAX, visualização de informações e experiência do usuário**, resultando em um projeto completo para portfólio.

---

## Como Executar o Relatório

1. Baixe o arquivo `.pbix` e a planilha utilizada como fonte de dados.
2. Abra o arquivo no Power BI Desktop.
3. Caso necessário, atualize o caminho da fonte de dados.
4. Clique em **Atualizar** para carregar as informações.
5. Navegue pelas páginas utilizando o menu do dashboard.
6. Utilize os filtros e segmentações para explorar os dados.

---

## Tecnologias Utilizadas

- Power BI Desktop;
- Power Query;
- DAX;
- Microsoft Excel;
- modelagem dimensional;
- visualização de dados;
- Git;
- GitHub.

---

## Aviso

Este é um projeto acadêmico e independente, desenvolvido para fins de estudo e portfólio.

O projeto não possui vínculo, patrocínio ou associação oficial com a FIFA ou com os organizadores da competição.

---

## 📬 Contato

Desenvolvido por **Jorge Gabriel Modrow**.

Estudante de Tecnologia em Análise e Desenvolvimento de Sistemas na Universidade Federal do Paraná — UFPR.

🔗 LinkedIn: [Jorge Gabriel Modrow](https://www.linkedin.com/in/jorgemodrow/)

🔗 GitHub: [Jorge Gabriel Modrow](https://github.com/jorgemodrow)
