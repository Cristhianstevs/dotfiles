# 🚀 Setup: Ambiente de Desenvolvimento

Este documento contém o passo a passo exato para configurar um computador Windows zerado para desenvolvimento web moderno utilizando Node.js, Next.js, React e ferramentas de altíssimo desempenho como Biome e pnpm. Se estiver em um ambiente corporativo veja a [Nota de Segurança em Ambiente Corporativo](#-13-nota-de-segurança-em-ambiente-corporativo)

<br />

## ✨ Motivação

Configurar uma nova máquina de desenvolvimento costuma ser um processo tedioso, manual e sujeito a inconsistências. A ideia deste repositório é criar uma **fonte única de verdade** (_Single Source of Truth_) para o ambiente de trabalho. O objetivo é transformar horas de downloads soltos e configurações perdidas na memória em um processo rápido, documentado, previsível e escalável.

<br />

## 🎯 Dores que este projeto soluciona

Para quem chega de fora, adotar esta arquitetura resolve imediatamente problemas clássicos e desgastantes do dia a dia de um desenvolvedor web:

- **O fim do "Na minha máquina funciona":** Padroniza versões do Node (via NVM) e garante que todos usem o mesmo gerenciador de pacotes na mesma versão (via Corepack/pnpm).

- **A paz na formatação do HTML:** Resolve o pesadelo de quebra de tags vazias e conflitos de indentação ao adotar uma arquitetura de "melhor dos dois mundos" (Biome governando o JavaScript/TypeScript e Prettier diagramando o HTML/CSS).

- **Fim do esforço manual:** Adoção da filosofia _Format on Save_ e _Auto Fix_. O desenvolvedor apenas foca na lógica, aperta `Ctrl + S`, e o editor magicamente formata o documento, resolve quebras de linha e organiza as importações.

- **Isolamento de preferências:** Divide claramente o que é gosto pessoal (fontes, temas e configurações visuais no `user/settings.json`) do que é regra ditatorial da equipe (`.vscode/settings.json` e `.editorconfig`).

<br />

## 🧠 A Experiência e o Conceito

Este ambiente foi desenhado com a mentalidade de **Consistência acima da Preferência**. Tudo acontece de maneira intencionalmente controlada. A escolha de priorizar ferramentas escritas em Rust (como o Biome) garante uma performance absurda no linting e formatação de projetos React massivos.

Além disso, o repositório foi arquitetado com uma forte preocupação voltada para a **Experiência do Desenvolvedor (DX)** aliada ao **Compliance Corporativo**, garantindo que o fluxo seja moderno, mas seguro o suficiente para rodar em redes empresariais rígidas sem ferir regras de InfoSec ou LGPD.

<br />

---

<br />

### 💡 Convenção de Terminais

Para evitar erros de permissão e falhas de instalação, todos os blocos de código deste guia possuem uma "tag" na primeira linha indicando o nível de privilégio exigido:

- `$admin` → Indica que o PowerShell deve ser aberto como **Administrador** (Clique com o botão direito no menu Iniciar > Terminal como Administrador).
- `$user` → Indica que o PowerShell deve ser aberto **Normalmente** (Permissões padrão do seu usuário).

<br />

## 📦 1. Instalações Base

Abra o PowerShell com privilégios elevados e rode os comandos abaixo para instalar os motores principais silenciosamente:

```powershell
$admin

# Instala o NVM
winget install -e --id CoreyButler.NVMforWindows

# Instala o Git
winget install -e --id Git.Git

# Instala o GitHub Desktop
winget install -e --id GitHub.GitHubDesktop

# Instala o Python 3
winget install -e --id Python.Python.3

# Instala o Visual Studio Code
winget install -e --id Microsoft.VisualStudioCode
```

**NVM**: Gerenciador de versões do Node. <br/>
**Git**: Obrigatório para versionamento. <br/>
**GitHub Desktop**: Interface visual oficial para o Git. <br/>
**Python 3**: Motor da linguagem (Opcional). <br/>
**Visual Studio Code**: Nosso editor de código oficial.

**MUITO IMPORTANTE**: Após rodar os comandos acima, FECHE O POWERSHELL. Abra um novo PowerShell (agora como `$user`) para que o Windows reconheça as variáveis de ambiente recém-instaladas.

<br />

## 🟢 2. Instalação do Node.js (via NVM)

Com as ferramentas base instaladas, vamos baixar o ecossistema do JavaScript de forma que possamos trocar as versões no futuro sem quebrar a máquina:

```powershell
$user

nvm install lts
nvm use lts
```

**Install LTS**: Baixa a versão Long Term Support (mais estável) do Node.js. <br />
**Use LTS**: Ativa a versão instalada como padrão do sistema.

<br />

## 🔓 3. Permissões e Gerenciador de Pacotes (pnpm)

Agora precisamos liberar o Windows para rodar nossos scripts de desenvolvimento e configurar nosso gerenciador de pacotes:

```powershell
$user

# 1
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned

# 2
corepack enable

# 3
corepack prepare pnpm@latest --activate

# 4
pnpm setup
```

**Execution Policy**: Libera a execução de scripts locais (Se pedir confirmação, digite 'A' ou 'S' e dê Enter). <br />
**Corepack Enable**: Ativa o gerenciador nativo que já vem embutido no Node moderno. <br />
**Corepack Prepare**: Baixa e ativa o pnpm na sua versão mais recente. <br />
**pnpm Setup**: Configura o diretório global nas variáveis do Windows.

<br />

## 🚨 4. Pnpm não foi reconhecido?

Se o Windows não reconhecer o comando acima, o terminal não atualizou as variáveis de ambiente.

1. Feche o PowerShell completamente.
2. Abra um novo PowerShell.
3. Rode novamente: `pnpm --version`

Se ainda assim não funcionar, reinicie o computador e tente novamente.

<br />

## 🌍 5. Ferramentas Globais de Desenvolvimento

Com o pnpm ativado, vamos instalar as ferramentas globais que vão ditar a qualidade do nosso código:

```powershell
$user

# Instala o Typescript
pnpm add -g typescript

# Instala o Biome
pnpm add -g @biomejs/biome
```

**TypeScript**: Instala o compilador oficial globalmente na máquina. <br />
**Biome**: Instala o motor de formatação e linting de alta performance (fallback global).

<br />

## 🔄 6. Atualização do Ambiente (Update)

Sempre que quiser atualizar seu ambiente, utilize os comandos abaixo. Dividimos em dois blocos para respeitar a arquitetura de permissões do sistema:

### Programas do Sistema

```powershell
$admin

winget upgrade --id Git.Git
winget upgrade --id Microsoft.VisualStudioCode
winget upgrade --id CoreyButler.NVMforWindows
winget upgrade --id GitHub.GitHubDesktop
```

**Winget Upgrade**: Atualiza ferramentas base. Recomendamos explicitar os IDs para não atualizar aplicativos paralelos do PC sem querer.

### Ecossistema de Desenvolvimento

```powershell
$user

# Atualiza o Node.js
nvm install lts
nvm use lts

# Atualiza o pnpm
corepack prepare pnpm@latest --activate

# Atualiza ferramentas
pnpm update -g typescript @biomejs/biome
```

**NVM**: Atualiza para o Node.js LTS do momento. <br />
**Corepack**: Puxa a versão mais recente do pnpm. <br />
**Pnpm update**: Atualiza as nossas ferramentas globais de código.

<br />

## ✅ 7. Verificação de Sucesso (Check-up)

Para garantir que tudo foi instalado perfeitamente, verifique a versão de cada motor. Se algum comando retornar "não reconhecido", reinicie o computador.

```powershell
$user

node -v
npm -v
pnpm -v
tsc -v
biome --version
python --version
code --version
git -v
```

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
  Oficial da biomejs _(Clique em Switch to Pre-Release Version)_. O coração do nosso JS/TS.

- **Prettier - Code formatter** <br />
  O mestre da diagramação para HTML, CSS e Markdown.

- **ESLint** <br />
  Padrão da indústria para linting (crucial para projetos com Next.js).

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

- **Auto Rename Tag** <br />
  Quando você altera a tag de abertura no HTML/JSX (ex: de `div` para `span`), ele altera a de fechamento junto.

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
3. Apague tudo que estiver lá, cole o código que está no arquivo [user/settings.json](user/settings.json)

### 10.2 Projeto (Regras da Equipe)

Estas regras forçam os formatadores (Biome e Prettier) a agirem nas linguagens corretas em qualquer máquina.

1. Crie uma pasta chamada `.vscode` na raiz do projeto.
2. Dentro da pasta, crie um arquivo `settings.json` e cole o código que está em [.vscode/settings.json](.vscode/settings.json).
3. Crie um arquivo `extensions.json` para recomendar extensões automaticamente para a equipe e cole o código de [.vscode/extensions.json](.vscode/extensions.json).

<br />

## 🚀 11. Configuração de Formatadores e Padronização

Regra de Ouro: Ferramentas devem viver dentro do projeto. Copie os arquivos listados abaixo para a raiz de todo novo projeto que você iniciar.

### 11.1 O Formatador JavaScript/React (`biome.jsonc`)

Cuida da performance e linting de todo o ecossistema JS.

1. Crie o arquivo `biome.jsonc` na raiz
2. Cole o código do nosso [biome.jsonc](biome.jsonc).

_Nota_: Caso o framework já tenha gerado um `biome.json`, renomeie para `.jsonc` e substitua o conteúdo. Não pode haver duplicidade.

### 11.2 O Formatador HTML/CSS (`.prettierrc`)

O Biome é o rei do JS, mas o Prettier é o mestre do design visual para tags web.

1. Crie o arquivo `.prettierrc` na raiz
2. Cole o código do nosso [.prettierrc](.prettierrc).

### 11.3 O Acordo de Paz Universal (`.editorconfig`)

Garante que o tamanho do TAB (2 espaços) funcione em qualquer editor de código do mundo (WebStorm, Sublime, etc).

1. Crie o arquivo `.editorconfig` na raiz
2. Cole o código do nosso [.editorconfig](.editorconfig).

### 11.4 Prevenção de Bugs de Sistema (`.gitattributes`)

Impede que o Windows mude silenciosamente a quebra de linha dos arquivos, o que quebraria os formatadores.

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

Se você está configurando este ambiente em um **computador da empresa**, por favor, leia atentamente antes de prosseguir. Este _dotfiles_ foi montado para máxima produtividade, mas ambientes corporativos possuem regras estritas de Segurança da Informação (InfoSec) e LGPD:

<br />

1. **🛑 Validação Obrigatória (InfoSec):** Antes de realizar qualquer download, importação de configurações ou execução dos comandos deste repositório na rede da empresa, **envie o link deste projeto para o setor de Segurança da Informação (ou TI) para validação prévia.**

2. **⚠️ Execução de Scripts e Permissões de Admin:** A Seção 1 exige privilégios de Administrador. Além disso, o comando `Set-ExecutionPolicy` (Seção 3) altera as políticas de execução do Windows. Em redes corporativas, isso geralmente é bloqueado e monitorado, podendo gerar alertas graves na TI. Não force a execução sem permissão.

3. **🤖 Inteligência Artificial (Código Proprietário):** Extensões como o GitHub Copilot enviam contexto do seu código para servidores externos. **NÃO FAÇA LOGIN** nestas extensões com contas pessoais/estudante sem a aprovação explícita do seu Tech Lead. O vazamento de regras de negócio ou dados de clientes é uma violação gravíssima.

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


$user

# Instala o Notion
winget install -e --id Notion.Notion
```

**Everything**: Motor de busca instantânea de arquivos no disco (Exige Admin). <br />
**Notion**: Plataforma líder para anotações, wikis e organização de projetos.

### 🎧 Entretenimento

```powershell
$user

# Instala o Spotify
winget install -e --id Spotify.Spotify
```

**Spotify**: Player de músicas e podcasts. (Nota: O instalador do Spotify possui uma restrição que proíbe sua execução como Administrador).
