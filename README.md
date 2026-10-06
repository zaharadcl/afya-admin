# Afya Admin — Dashboard com Blazor WebAssembly e MudBlazor

## Identificação

| | |
|---|---|
| **Aluno(a)** | Zahara Lacerda |
| **Matrícula** | [2640742] |
| **Faculdade** | Centro Universitário São Lucas-Afya |
| **Curso** | Ciência da Computação |
| **Disciplina** | [Programação Web] |
| **Professor(a)** | [Liluyoud] |
| **Semestre** | 2026.2 |

## Objetivo do projeto

O projeto consiste no desenvolvimento do painel administrativo (dashboard) da plataforma fictícia "Afya Pedagógico". A aplicação centraliza a visualização operacional e estratégica da instituição, permitindo que gestores e coordenadores acompanhem métricas financeiras, retenção de usuários e o andamento de projetos acadêmicos e corporativos em uma interface única.

A tela reúne indicadores de desempenho (KPIs) com minigráficos de tendência (sparklines), gráficos de acompanhamento de receita versus meta e distribuição de segmentos de clientes, além de blocos dedicados à performance de tarefas em equipe, feed em tempo real de atividades recentes e uma listagem tabular dos projetos em execução.

Todo o projeto foi concebido seguindo os princípios de modularidade e separação de responsabilidades do Blazor WebAssembly, exercitando componentização, passagem de parâmetros, renderização responsiva em grid de 12 colunas e alternância dinâmica de tema claro e escuro. Todo o visual foi construído sem nenhuma linha de CSS próprio, aproveitando exclusivamente os componentes, o tema centralizado (`MudTheme`) e as classes utilitárias nativas do MudBlazor.

## Tecnologias utilizadas

- .NET 10 / Blazor WebAssembly (Standalone)
- C# 10+
- MudBlazor 9
- Material Design Icons
- Google Fonts (Inter)

## Como executar

Pré-requisito: [.NET SDK 10](https://dotnet.microsoft.com/) instalado no ambiente.

Clone o repositório e execute a aplicação em modo de observação contínua:

```bash
git clone [https://github.com/zaharadcl/afya-admin.git](https://github.com/zaharadcl/afya-admin.git)
cd afya-admin
dotnet watch
```

## Telas

### Tema claro
![Dashboard — tema claro](docs/prints/tema-claro.jpeg)

### Tema escuro
![Dashboard — tema escuro](docs/prints/tema-escuro.jpeg)

### Versão mobile
![Dashboard — celular](docs/prints/mobile.jpeg)

### HTML gerado (DevTools)
![Inspeção do HTML no DevTools](docs/prints/devtools.png)

Na inspeção do card de KPI (KpiCard), nota-se que o componente <MudPaper> foi renderizado pelo Blazor no DOM como uma tag <div> contendo as classes CSS mud-paper, mud-elevation-1 e pa-4. A classe utilitária pa-4, definida no código C#/Razor, é traduzida diretamente pelo MudBlazor em um preenchimento interno de 16px (padding). O elemento <MudAvatar> com a classe calculada dinamicamente por Ui.FundoSuave resultou em um container circular com fundo translúcido na cor da paleta do indicador, enquanto o <MudChart> gerou uma estrutura inteiramente baseada em vetores <svg> internos.

## Estrutura do projeto
afya-admin/
├── Components/         # Componentes visuais modulares e classes utilitárias de UI
├── Data/               # Modelos de domínio (records) e classes com dados fictícios
├── Layout/             # Estrutura mestra da aplicação (MainLayout e NavMenu)
├── Pages/              # Páginas roteáveis da aplicação (Dashboard e NotFound)
├── Properties/         # Configurações de inicialização e perfis locais (launchSettings.json)
├── docs/prints/        # Capturas de tela utilizadas na documentação do repositório
└── wwwroot/            # Arquivos estáticos servidos pelo navegador (index.html, imagens, favicon)

## Componentes criados

## Componentes criados

| Componente | Responsabilidade | Parâmetros que recebe |
|---|---|---|
| `CabecalhoPagina` | Renderiza o título, subtítulo e área flexível de botões de ação | `Titulo`, `Subtitulo`, `Acoes` (RenderFragment) |
| `SeletorPeriodo` | Menu em estilo botão para seleção de intervalo temporal de dados | `Opcoes`, `Valor`, `ValorChanged` (EventCallback) |
| `DashboardCard` | Estrutura base de card com elevação, cabeçalho, menu e área de conteúdo expansível | `Titulo`, `Subtitulo`, `Acoes`, `Menu`, `ChildContent` |
| `KpiCard` | Card métrico com avatar pastel, indicador de variação percentual e gráfico sparkline | `Kpi` |
| `GraficoReceita` | Renderiza o gráfico de linhas comparativo entre receita realizada e meta mensal | `Meses`, `Receita`, `Meta` |
| `GraficoDistribuicaoClientes` | Gráfico de rosca (donut) com total de clientes centralizado via SVG e legenda customizada | `Total`, `Segmentos` |
| `PerformanceProjetos` | Lista de projetos com barras de progresso lineares proporcionais e separadores alinhados | `Projetos` |
| `AtividadesRecentes` | Linha do tempo com avatares com iniciais, ícones de ação e carimbo de tempo | `Atividades` |
| `ProjetosRecentes` | Tabela detalhada de acompanhamento de projetos com chips de status e ações | `Projetos` |

## O que aprendi

## O que aprendi

1. **Como uma aplicação Blazor WebAssembly inicia no navegador? Qual é o papel do `index.html`, da `<div id="app">` e do `Program.cs`?**  
O fluxo se inicia quando o navegador requisita o arquivo estático `wwwroot/index.html`, onde reside a marcação `<div id="app">`. O script `_framework/blazor.webassembly.js` é carregado, baixando o runtime do .NET compilado para WebAssembly e as bibliotecas do projeto compiladas em `.dll`. O runtime executa o ponto de entrada em `Program.cs`, cuja instrução `builder.RootComponents.Add<App>("#app")` vincula o componente raiz `App.razor` ao elemento `#app`, substituindo a tela de carregamento pela aplicação interativa executada inteiramente no cliente.

2. **Qual é a diferença entre um Layout, uma Page e um Component neste projeto? Dê um exemplo de cada.**  
O **Layout** (`MainLayout.razor`) serve como a moldura persistente da aplicação que envolve as telas, contendo a AppBar, o menu lateral (`MudDrawer`) e a instrução `@Body`. A **Page** (`Dashboard.razor`) é um componente associado a uma rota específica do navegador pela diretiva `@page "/"`, servindo de ponto de entrada. Já o **Component** (`DashboardCard.razor` ou `KpiCard.razor`) é um bloco modular, reutilizável e parametrizável sem rota própria, instanciado dentro de páginas ou layouts para encapsular comportamento e interface específicos.

3. **O que é um `RenderFragment` e como o `DashboardCard` usa esse recurso para ser reutilizado por vários cards?**  
O `RenderFragment` é um tipo de delegado em C# que representa um bloco de marcação Razor/HTML repassado para outro componente como parâmetro. O `DashboardCard` utiliza parâmetros do tipo `RenderFragment` (`Acoes`, `Menu` e `ChildContent`) para atuar como um componente template estrutural com slots: ele padroniza o invólucro visual (bordas, elevação, sombra e cabeçalho), permitindo que a página pai decida o que renderizar dentro de cada área sem duplicar código estrutural.

4. **Como funciona o `@bind-Valor` no `SeletorPeriodo`? Qual é o papel do `ValorChanged`?**  
A sintaxe `@bind-Valor` no `SeletorPeriodo` implementa o padrão de two-way data binding do Blazor. Ela conecta a propriedade de entrada `Valor` ao delegate de evento `ValorChanged` (`EventCallback<string>`). Quando o usuário clica em um item do menu, o componente filho invoca `ValorChanged.InvokeAsync(opcao)`. Isso notifica a página pai (`Dashboard.razor`), que atualiza sua variável local `_periodo` e aciona o ciclo de re-renderização, mantendo o estado sincronizado.

5. **Por que os dados ficam na pasta `Data`, separados dos componentes? Que vantagem isso traz se, no futuro, os dados vierem de uma API?**  
A pasta `Data` concentra os modelos de domínio (`records`) e os dados mockados, respondendo "o que" o sistema exibe, enquanto a pasta `Components` define "como" esses dados são desenhados na tela. Essa separação garante baixo acoplamento: caso os dados passem a vir de uma API REST ou de um banco de dados real, basta alterar a camada de serviço/dados sem modificar uma única linha da marcação visual dos componentes.

6. **Como o `MudGrid` com `xs`, `sm` e `lg` faz os cards de KPI se reorganizarem em telas de tamanhos diferentes?**  
O `MudGrid` gerencia um grid flexível baseado em 12 colunas fracionadas. Através dos parâmetros de ponto de interrupção (`xs`, `sm`, `lg`) configurados em cada `MudItem`, a página se reorganiza de acordo com a largura da tela: no celular (`xs="12"`), cada card de KPI consome 12 colunas (ocupando a linha inteira); no tablet (`sm="6"`), cada card consome 6 colunas (dois cards por linha); e em desktops (`lg="3"`), consome 3 colunas (quatro cards dispostos lado a lado).

7. **Como foi possível estilizar a página inteira sem escrever CSS? Explique o papel do tema (`MudTheme`) e das classes utilitárias.**  
A customização visual foi alcançada combinando a definição central do `MudTheme` no `MainLayout` com o catálogo de classes utilitárias do MudBlazor. O `MudTheme` gera dinamicamente as variáveis de paleta, tipografia e raios de borda aplicadas nos componentes, garantindo a consistência dos modos claro e escuro. Em complemento, classes como `pa-4`, `d-flex`, `flex-column`, `flex-grow-1` e `mud-background-gray` resolveram o dimensionamento, espaçamento e alinhamento flexbox diretamente no HTML compilado.

8. **Por que o namespace do projeto é `afya_admin` e não `afya-admin`?**  
Na especificação da linguagem C#, identificadores de namespaces e variáveis não admitem hífens, visto que o caractere `-` é reservado como operador de subtração matemática. Ao criar um projeto nomeado como `afya-admin`, o compilador do .NET SDK substitui os caracteres especiais por sublinhados para gerar um identificador sintaticamente válido, resultando no namespace padrão `afya_admin`.

## Dificuldades e soluções

## Dificuldades e soluções

- **Visibilidade e proporção do sparkline nos KPIs:** No tamanho reduzido (110x50px), o traço padrão da linha ficava quase invisível devido à escala interna do SVG, e o gráfico ficava achatado. **Solução:** Aumentei o `LineStrokeWidth` para `12` para garantir cerca de 2px visíveis na tela, removi linhas de grade/eixos e desativei `YAxisRequireZeroPoint = false` para a curva ocupar toda a área útil do card.

- **Espaçamento vertical no card de Performance dos Projetos:** O bloco ficava com espaço vazio embaixo por estar ao lado do feed de atividades, e usar `MudDivider` fragmentava a distribuição flex. **Solução:** Apliquei `flex-grow-1` em cada linha de projeto e utilizei classes utilitárias de borda inferior (`border-b border-solid mud-border-lines-default`) como separadores, distribuindo as faixas de progresso uniformemente pela altura total do card.