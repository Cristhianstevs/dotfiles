# 🚀 Setup: Ambiente de Desenvolvimento

Este documento contém o passo a passo exato para configurar um computador Windows zerado para desenvolvimento web moderno utilizando Node.js, Next.js, React e ferramentas modernas como Biome e pnpm. Se estiver em um ambiente corporativo veja a [Nota de Segurança em Ambiente Corporativo](#-13-nota-de-segurança-em-ambiente-corporativo)

<br />

## ✨ Motivação

Configurar uma nova máquina de desenvolvimento costuma ser um processo tedioso, manual e sujeito a inconsistências. A ideia deste repositório é criar uma **fonte única de verdade** (_Single Source of Truth_) para o ambiente de trabalho. O objetivo é transformar horas de downloads soltos e configurações perdidas na memória em um processo rápido, documentado, previsível e escalável.

<br />

## 🎯 Dores que este projeto soluciona

Para quem chega de fora, adotar esta arquitetura resolve imediatamente problemas clássicos e desgastantes do dia a dia de um desenvolvedor web:

- **O fim do "Na minha máquina funciona":** Padroniza versões do Node (via NVM) e utiliza o pnpm como gerenciador de pacotes principal.

- **A paz na formatação do HTML:** Resolve o pesadelo de quebra de tags vazias e conflitos de indentação ao adotar uma arquitetura de "melhor dos dois mundos" (Biome governando o JavaScript/TypeScript e Prettier diagramando o HTML/CSS).

- **Fim do esforço manual:** Adoção da filosofia _Format on Save_ e _Auto Fix_. O desenvolvedor apenas foca na lógica, aperta `Ctrl + S`, e o editor magicamente formata o documento, resolve quebras de linha e organiza as importações.

- **Isolamento de preferências:** Divide claramente o que é gosto pessoal (fontes, temas e configurações visuais no `user/settings.json`) do que é regra do projeto (`.vscode/settings.json` e `.editorconfig`).

<br />

## 🧠 A Experiência e o Conceito

Este ambiente foi desenhado com a mentalidade de **Consistência acima da Preferência**. Tudo acontece de maneira intencionalmente controlada. A escolha de priorizar ferramentas escritas em Rust (como o Biome) garante uma excelente performance no linting e formatação de projetos React.

Além disso, o repositório foi arquitetado com uma forte preocupação voltada para a **Experiência do Desenvolvedor (DX)** aliada ao **Compliance Corporativo**, garantindo que o fluxo seja moderno, mas seguro o suficiente para rodar em redes empresariais rígidas sem ferir regras de InfoSec ou LGPD.

<br />

---

<br />

### 💡 Convenção de Terminais

Para evitar erros de permissão e falhas de instalação, todos os blocos de código deste guia possuem uma "tag" na primeira linha indicando onde o comando deve ser executado:

- `$admin` → Indica que o PowerShell deve ser aberto como **Administrador** (Clique com o botão direito no menu Iniciar > Terminal como Administrador).
- `$user` → Indica que o PowerShell deve ser aberto **Normalmente** (Permissões padrão do seu usuário).
- `$projeto` → Indica que o comando deve ser executado no terminal **dentro da pasta do projeto**.

> As tags acima servem apenas como indicação visual e não fazem parte dos comandos que devem ser executados.

<br />

## 📦 1. Instalações Base

Abra o PowerShell com privilégios elevados e rode os comandos abaixo para instalar os motores principais silenciosamente:

```powershell
$admin

# Instala VCRedist
winget install -e --id Microsoft.VCRedist.2015+.x64

# Instala o NVM
winget install -e --id CoreyButler.NVMforWindows

# Instala o Git
winget install -e --id Git.Git

# Instala o GitHub Desktop
winget install -e --id GitHub.GitHubDesktop

# Instala o Python 3
winget install -e --id Python.Python.3

# Instala o SDK do .NET 10 (LTS)
winget install -e --id Microsoft.DotNet.SDK.10

# Instala o Visual Studio Code
winget install -e --id Microsoft.VisualStudioCode --override "/verysilent /mergetasks=addcontextmenufiles,addcontextmenufolders"
```

**VCRedist**: Bibliotecas essenciais do Windows exigidas por diversas ferramentas e aplicações desenvolvidas em C/C++. <br />
**NVM**: Gerenciador de versões do Node.js. Permite instalar, atualizar e alternar entre diferentes versões do Node sem bagunçar o sistema. <br/>
**Git**: Obrigatório para versionamento e integração com repositórios. <br/>
**GitHub Desktop**: Interface visual oficial do GitHub para trabalhar com Git e repositórios. <br/>
**Python 3**: Motor da linguagem Python (Opcional). <br/>
**Microsoft .NET SDK 10**: Kit de desenvolvimento completo contendo o compilador e as bibliotecas necessárias para criar aplicações backend, APIs e sistemas utilizando C#. <br />
**Visual Studio Code**: Nosso editor de código principal, instalado com os menus de contexto do botão direito (`Abrir com Code`).

**MUITO IMPORTANTE**: Após rodar os comandos acima, FECHE O POWERSHELL. Abra um novo PowerShell normalmente para que o Windows reconheça corretamente as variáveis de ambiente e os programas recém-instalados.

<br />

## 🟢 2. Instalação do Node.js (via NVM)

Com as ferramentas base instaladas, vamos baixar o ecossistema do JavaScript de forma que possamos trocar ou atualizar as versões do Node no futuro sem quebrar a máquina:

```powershell
$user

nvm install lts
nvm use lts
```

**Install LTS**: Baixa a versão Long Term Support do Node.js, recomendada para desenvolvimento e maior estabilidade. <br />
**Use LTS**: Ativa a versão LTS recém-instalada como versão atual do sistema.

Para confirmar que tudo foi reconhecido corretamente:

```powershell
$user

node -v
npm -v
```

<br />

## 📦 3. Gerenciador de Pacotes (pnpm)

O Node.js já instala o **npm**, mas utilizaremos o **pnpm** como gerenciador de pacotes principal por ser rápido, econômico em espaço e funcionar muito bem em projetos modernos com React e Next.js.

**OBSERVAÇÃO:** Após a instalação abaixo, feche e abra novamente o PowerShell para que as alterações feitas no ambiente sejam carregadas e consiga ver a versão.

```powershell
$user

# Instala o pnpm
npx get-pnpm

# Verifica a versão instalada
pnpm -v
```

**npx get-pnpm**: Baixa e configura o pnpm utilizando o npm que já veio instalado junto do Node.js. <br />
**pnpm**: Será nosso gerenciador principal para instalar dependências, executar scripts e gerenciar os projetos JavaScript/TypeScript.

<br />

## 🚨 4. pnpm não foi reconhecido?

Se o Windows não reconhecer o comando `pnpm`, normalmente o terminal ainda não atualizou as variáveis de ambiente.

1. Feche o PowerShell completamente.
2. Abra um novo PowerShell.
3. Rode novamente: `pnpm -v`

Se ainda assim não funcionar, reinicie o computador e tente novamente.

Caso o próprio PowerShell bloqueie a execução de scripts e mostre um erro relacionado à **Execution Policy**, execute:

```powershell
$user

Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

Se pedir confirmação, digite `S` e aperte Enter.

**IMPORTANTE**: Não altere a Execution Policy sem necessidade. Em computadores corporativos, consulte primeiro a equipe de TI ou Segurança da Informação.

<br />

## 🌍 5. Ferramentas de Desenvolvimento

Ferramentas como **TypeScript**, **Biome**, **Prettier** e outras dependências de desenvolvimento devem preferencialmente viver dentro de cada projeto.

Isso garante que cada aplicação utilize exatamente as versões esperadas e evita diferenças de comportamento entre projetos antigos e novos.

Por exemplo:

```powershell
$projeto

# Instala o TypeScript
pnpm add -D typescript

# Instala o Biome
pnpm add -D -E @biomejs/biome

# Instala o Prettier
pnpm add -D -E prettier
```

**TypeScript**: Instala o compilador da linguagem apenas naquele projeto, permitindo que aplicações diferentes utilizem versões diferentes sem conflitos. <br />
**Biome**: Instala o motor de formatação e linting diretamente no projeto, garantindo consistência entre todas as máquinas que trabalharem naquele repositório.
**Prettier**: Instala o formatador de código diretamente no projeto, garantindo que todos os desenvolvedores sigam o mesmo estilo de formatação.
As configurações dessas ferramentas serão feitas posteriormente na seção de **Formatadores e Padronização**.

<br />

## 🔄 6. Atualização do Ambiente (Update)

Sempre que quiser atualizar seu ambiente, utilize os comandos abaixo. Dividimos a atualização em blocos para respeitar a arquitetura de permissões do sistema e separar ferramentas globais das dependências de cada projeto:

### Programas do Sistema

Primeiro, você pode consultar quais programas possuem atualizações disponíveis:

```powershell
$admin

winget upgrade
```

Depois, atualize apenas as ferramentas que desejar:

```powershell
$admin

winget upgrade -e --id Git.Git
winget upgrade -e --id Microsoft.VisualStudioCode
winget upgrade -e --id CoreyButler.NVMforWindows
winget upgrade -e --id GitHub.GitHubDesktop
winget upgrade -e --id Python.Python.3
winget upgrade -e --id Microsoft.DotNet.SDK.10
```

**Winget Upgrade**: Lista e atualiza programas instalados através do WinGet. Preferimos explicitar os IDs para ter controle sobre o que será atualizado, em vez de atualizar todos os aplicativos do computador de uma vez.

### Ecossistema de Desenvolvimento

```powershell
$user

# Atualiza o Node.js para a versão LTS mais recente
nvm install lts
nvm use lts

# Atualiza o pnpm
pnpm self-update
```

**NVM**: Instala a versão LTS mais recente do Node.js e a ativa no ambiente. <br />
**pnpm self-update**: Atualiza o pnpm para a versão mais recente disponível.

### Ferramentas do Projeto

Ferramentas como **TypeScript**, **Biome** e **Prettier** são instaladas localmente em cada projeto. Dessa forma, cada projeto mantém suas próprias versões e pode ser atualizado de forma independente.

```powershell
$projeto

# Verifica dependencias desatualizadas
pnpm outdated

# Atualiza o Biome
pnpm update --latest @biomejs/biome

# Atualiza o TypeScript
pnpm update --latest typescript

# Atualiza o Prettier
pnpm update --latest prettier
```

**pnpm outdated**: Verifica quais dependências do projeto possuem versões mais recentes disponíveis. <br />
**pnpm update --latest**: Atualiza as dependências especificadas para suas versões mais recentes, inclusive quando a nova versão está fora do intervalo definido atualmente no `package.json`.

**OBSERVAÇÃO:** TypeScript, Biome e Prettier não são atualizados globalmente. Como vivem dentro de cada projeto, a atualização deve ser executada individualmente nos projetos em que você deseja adotar as novas versões.

<br />

## ✅ 7. Verificação de Sucesso (Check-up)

Para garantir que tudo foi instalado corretamente, verifique a versão de cada ferramenta principal:

```powershell
$user

nvm --version
nvm default
node -v
npm -v
pnpm -v
git --version
python --version
dotnet --version
code --version
```

Cada comando deve retornar a versão instalada da respectiva ferramenta.

Se algum deles retornar **"não reconhecido"**, feche completamente o PowerShell e abra novamente para atualizar as variáveis de ambiente. Caso o problema continue, reinicie o computador e teste novamente.

<br />

## 🎨 8. Preparando o Editor (VS Code)

### Fonte: JetBrains Mono

O nosso VS Code usará uma fonte otimizada para leitura de código com "font ligatures" (que transforma `=>` em setas desenhadas).

1. Baixe a fonte oficial aqui: [JetBrains Mono](https://www.jetbrains.com/pt-br/lp/mono/)
2. Extraia o `.zip`
3. Selecione todos os arquivos `.ttf`
4. Clique com o botão direito e selecione **Instalar**.

<br />

## 🧩 9. Extensões Essenciais

Abra o VS Code, vá na aba de extensões (`Ctrl + Shift + X`) e instale as ferramentas abaixo. Elas foram divididas por domínio para facilitar o entendimento do nosso ecossistema:

### 🛠️ Motores e Formatadores

- **Biome** <br />
  Oficial da biomejs. O coração do nosso JS/TS, responsável pela formatação e linting.

- **Prettier - Code formatter** <br />
  Responsável pela formatação de HTML, CSS e Markdown.

- **ESLint (Opcional)** <br />
  Utilizado em projetos que já possuem ESLint ou dependem de regras/plugins específicos.

### ⚛️ Ecossistema JS, React & Node

- **ES7+ React/Redux/React-Native snippets** <br />
  Atalhos rápidos para criar componentes React (ex: digite `rfce` e dê Tab).

- **Tailwind CSS IntelliSense** <br />
  Autocomplete, destaque de sintaxe e linting para classes do Tailwind.

- **DotENV** <br />
  Destaca a sintaxe de arquivos `.env` (variáveis de ambiente do Node).

- **Node.js Exec** <br />
  Executa o arquivo atual ou código selecionado no Node.js apertando F8.

### 🐙 Produtividade & IA

- **GitLens — Git supercharged** <br />
  Mostra quem escreveu cada linha de código e quando (Git Blame inline).

- **GitHub Copilot Chat** <br />
  Assistente de Inteligência Artificial integrado ao editor.

- **Error Lens** <br />
  Mostra as mensagens de erro e avisos na própria linha do código, sem precisar passar o mouse.

- **Turbo Console Log** <br />
  Automatiza a criação de `console.log` para debug rápido no JavaScript.

### 🎨 Visual, Utilitários & HTML

- **Material Icon Theme** <br />
  Deixa os ícones das pastas e arquivos maravilhosos e fáceis de identificar.

- **Color Highlight** <br />
  Pinta o fundo de códigos hexadecimais (ex: `#FFF`) com a própria cor no código.

- **Live Server** <br />
  Cria um servidor local com recarregamento em tempo real para arquivos HTML puros.

- **CodeSnap** <br />
  Tira "fotos" lindas e polidas de trechos do seu código para compartilhar.

- **PowerShell** <br />
  Suporte avançado para os scripts de terminal no Windows.

<br />

## ⚙️ 10. Configuração do VS Code (settings.json)

### 10.1 Global (Preferências de Usuário)

Vamos forçar o VS Code a usar nossas regras de interface e comportamento.

1. No VS Code, aperte `F1` (ou `Ctrl + Shift + P`).
2. Digite `Open User Settings (JSON)` e dê Enter.
3. Em uma instalação nova, substitua o conteúdo pelo código do arquivo [user/settings.json](user/settings.json). Caso já possua configurações pessoais, mescle apenas o que desejar manter.

### 10.2 Projeto (Regras do Projeto)

Estas regras forçam os formatadores (Biome e Prettier) a agirem nas linguagens corretas em qualquer máquina.

1. Crie uma pasta chamada `.vscode` na raiz do projeto.
2. Dentro da pasta, crie um arquivo `settings.json` e cole o código que está em [.vscode/settings.json](.vscode/settings.json).
3. Crie um arquivo `extensions.json` para recomendar extensões automaticamente para o projeto e cole o código de [.vscode/extensions.json](.vscode/extensions.json).

<br />

## 🚀 11. Configuração de Formatadores e Padronização

Regra de Ouro: Ferramentas devem viver dentro do projeto. Copie os arquivos listados abaixo para a raiz de todo novo projeto que você iniciar.

### 11.1 O Formatador JavaScript/React (`biome.jsonc`)

Cuida da performance e linting de todo o ecossistema JS.

1. Crie o arquivo `biome.jsonc` na raiz
2. Cole o código do nosso [biome.jsonc](biome.jsonc).

_Nota_: Caso o framework já tenha gerado um `biome.json`, renomeie para `.jsonc` e substitua o conteúdo. Não pode haver duplicidade.

### 11.2 O Formatador HTML/CSS (`.prettierrc`)

O Biome cuida do nosso ecossistema JavaScript/TypeScript, enquanto o Prettier fica responsável pela formatação de HTML, CSS e Markdown.

1. Crie o arquivo `.prettierrc` na raiz
2. Cole o código do nosso [.prettierrc](.prettierrc).

### 11.3 O Acordo de Paz Universal (`.editorconfig`)

Garante que o tamanho do TAB (2 espaços) funcione em qualquer editor de código do mundo (WebStorm, Sublime, etc).

1. Crie o arquivo `.editorconfig` na raiz
2. Cole o código do nosso [.editorconfig](.editorconfig).

### 11.4 Prevenção de Bugs de Sistema (`.gitattributes`)

Padroniza as quebras de linha dos arquivos de texto em LF, evitando diferenças desnecessárias entre Windows, Linux e macOS.

1. Crie o arquivo `.gitattributes` na raiz
2. Cole o código do nosso [.gitattributes](.gitattributes).

<br />

## 🔄 12. Toque Final e Troubleshooting

Sempre que editar os arquivos de configuração do VS Code ou do Biome pela primeira vez, aperte `F1`, digite `Reload Window` e aperte Enter para o VS Code recarregar a memória e achar o motor local.

**A Mágica**: Você não precisa mais usar atalhos de formatação! Graças às nossas configurações, basta salvar o arquivo (`Ctrl + S` ou clicar fora dele) que os imports serão organizados e o código formatado instantaneamente.

### O arquivo não formatou? Verifique o log do Biome:

1. No canto superior esquerdo: `View > Terminal > Output` (ou aperte Ctrl + Shift + U).
2. No menu suspenso do painel (onde costuma estar escrito "Tasks" ou "Window"), troque para Biome. O erro exato estará descrito lá.

<br />

**Pronto!** A estrutura de pastas que você precisa ter no seu repositório do GitHub agora é simplesmente:

```
dotfiles/
├── .vscode/
│   ├── extensions.json
│   └── settings.json
├── user/
│   └── settings.json
├── .editorconfig
├── .gitattributes
├── .prettierrc
├── biome.jsonc
└── README.md (você está aqui!)
```

Ficou no ponto para usar em qualquer projeto ou máquina nova!

<br />

## 🏢 13. Nota de Segurança em Ambiente Corporativo

Se você está configurando este ambiente em um **computador da empresa**, por favor, leia atentamente antes de prosseguir. Este _dotfiles_ foi montado com foco em produtividade e no meu ambiente pessoal, mas computadores corporativos possuem regras próprias de Segurança da Informação (InfoSec) e LGPD.

<br />

1. **🛑 Validação Obrigatória (InfoSec):** Antes de realizar qualquer download, importação de configurações ou execução dos comandos deste repositório na rede da empresa, **envie o link deste projeto para o setor de Segurança da Informação (ou TI) para validação prévia.**

2. **⚠️ Execução de Scripts e Permissões de Admin:** A Seção 1 utiliza privilégios de Administrador. Além disso, o Troubleshooting da Seção 4 apresenta o comando `Set-ExecutionPolicy` caso o PowerShell bloqueie a execução de scripts. Em computadores corporativos, não altere políticas de execução ou configurações de segurança sem autorização da equipe responsável.

3. **🤖 Inteligência Artificial (Código Proprietário):** Ferramentas de IA podem processar contexto do código para fornecer sugestões e respostas. Em ambientes corporativos, utilize apenas ferramentas, contas e configurações previamente aprovadas pela empresa e pela equipe de Segurança da Informação.

4. **🛡️ Estabilidade de Software:** Evite usar versões _Pre-Release_ de extensões no horário de trabalho. Opte sempre pelas versões _Stable_ para evitar falhas inesperadas de produtividade.

<br />

**O Projeto pode Melhorar!** A arquitetura estrutural e a varredura de segurança inicial deste repositório foram construídas com o auxílio de inteligência artificial (**Gemini 3.1 Pro**), visto que não sou formado em _CyberSecurity_. Como a IA não substitui o olhar rigoroso de um profissional da área, este projeto está de portas abertas! _Issues_, _Pull Requests_ e feedbacks de engenheiros de segurança corporativa ou desenvolvedores da comunidade são extremamente bem-vindos para tornar este ambiente cada vez mais blindado e compatível com as exigências de mercado.

<br />

## 🤝 14. Ferramentas e Aplicativos Opcionais

Aqui estão comandos extras para instalar ferramentas de produtividade, outros navegadores e ecossistemas adicionais caso sejam necessários no futuro. Copie e cole no PowerShell apenas o que for utilizar.

### 🌐 Navegadores

```powershell
$admin

# Instala o Google Chrome
winget install -e --id Google.Chrome

# Instala o Opera GX
winget install -e --id Opera.OperaGX
```

**Google Chrome**: Principal navegador de testes da indústria. <br />
**Opera GX**: Navegador com limitadores nativos de uso de RAM e CPU.

### 🌐 DevOps & API

```powershell
$admin

# Instala o Docker Desktop
winget install -e --id Docker.DockerDesktop

# Instala o Postman
winget install -e --id Postman.Postman
```

**Docker Desktop**: Plataforma de containerização (exige WSL2). <br />
**Postman**: Interface para testes de chamadas de API e Rotas de Back-end.

### 🐍 Ecossistema Python

```powershell
$admin

# Instala o PyCharm
winget install -e --id JetBrains.PyCharm.Community
```

**PyCharm Community**: IDE oficial e gratuita da JetBrains para Python.

### 🗂️ Produtividade & Utilitários

```powershell
$admin

# Instala o Everything
winget install -e --id voidtools.Everything

# Instala o WinRAR
winget install -e --id RARLab.WinRAR

# Instala o Free Download Manager
winget install -e --id SoftDeluxe.FreeDownloadManager

$user

# Instala o Notion
winget install -e --id Notion.Notion
```

**Everything**: Motor de busca instantânea de arquivos no disco (Exige Admin). <br />
**WinRAR**: O clássico e poderoso compactador/descompactador de arquivos (Exige Admin). <br />
**Free Download Manager**: Acelerador e organizador gratuito de downloads, suporta torrents, arquivos e vídeos. <br />
**Notion**: Plataforma líder para anotações, wikis e organização de projetos.

### 🎧 Entretenimento

```powershell
$user

# Instala o Spotify
winget install -e --id Spotify.Spotify
```

**Spotify**: Player de músicas e podcasts. (Nota: O instalador do Spotify possui uma restrição que proíbe sua execução como Administrador).
