# 🚀 Setup: Ambiente de Desenvolvimento

Este documento contém o passo a passo exato para configurar um computador Windows zerado para desenvolvimento web moderno utilizando Node.js, Next.js, React e ferramentas de altíssimo desempenho como Biome e pnpm.

<br />

## 📦 1. Instalações Base (Programas Nativos)

Abra o **PowerShell como Administrador** (Menu Iniciar > clique com o direito > Executar como Administrador) e rode os comandos abaixo um por um para instalar os motores principais silenciosamente:

```powershell
# Instala o gerenciador de versões do Node (NVM)
winget install -e --id CoreyButler.NVMforWindows

# Instala o Git (Obrigatório para versionamento)
winget install -e --id Git.Git

# Instala o GitHub Desktop (Interface visual para o Git)
winget install -e --id GitHub.GitHubDesktop

# Instala o Python 3 (Opcional - instale apenas se for utilizar)
winget install -e --id Python.Python.3

# Instala o Visual Studio Code
winget install -e --id Microsoft.VisualStudioCode
```

🛑 MUITO IMPORTANTE: Após rodar os comandos acima, FECHE O POWERSHELL. Abra um novo PowerShell (agora pode ser como usuário normal) para que o Windows reconheça as variáveis de ambiente recém-instaladas.

<br />

## 🟢 2. Instalação do Node.js (via NVM)

Agora vamos instalar o Node.js da forma correta utilizando o NVM (isso permite trocar versões no futuro sem quebrar seu ambiente):

```powershell
# Instala a versão LTS mais recente do Node.js
nvm install lts

# Ativa a versão instalada
nvm use lts
```

<br />

## 🔓 3. Permissões e Gerenciador de Pacotes (pnpm)

No novo PowerShell aberto, precisamos liberar o Windows para rodar scripts de desenvolvimento e ativar nosso gerenciador de pacotes principal:

```powershell
# 1. Libera a execução de scripts (Se pedir confirmação, digite 'A' ou 'S' e dê Enter)
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned

# 2. Ativa o Corepack (já vem com o Node moderno)
corepack enable

# 3. Ativa o pnpm na versão mais recente
corepack prepare pnpm@latest --activate

# 4. Configura o diretório global do pnpm automaticamente
pnpm setup
```

<br />

## 🚨 4. Solução de Problemas: O comando pnpm não foi reconhecido?

Se o Windows não reconhecer o comando acima, é porque o terminal ainda não atualizou as variáveis de ambiente.

1. Feche o PowerShell completamente.
2. Abra um novo PowerShell.
3. Rode novamente: `pnpm --version`

Se ainda assim não funcionar, reinicie o computador e tente novamente.

<br />

## 🌍 5. Ferramentas Globais de Desenvolvimento

Com o pnpm instalado e funcionando, vamos adicionar o TypeScript e o "motor" do Biome globalmente na máquina:

```powershell
# Instala o compilador do TypeScript (opcional - pode ser usado localmente por projeto)
pnpm add -g typescript

# Instala o motor do Biome globalmente (fallback)
pnpm add -g @biomejs/biome
```

<br />

## 🔄 6. Atualização do Ambiente (Update)

Sempre que quiser atualizar seu ambiente de desenvolvimento, utilize os comandos abaixo. É recomendado atualizar pacotes de forma explícita para evitar que atualizações automáticas quebrem outros softwares do PC:

```powershell
# Atualiza os motores principais via winget
winget upgrade --id Git.Git
winget upgrade --id Microsoft.VisualStudioCode
winget upgrade --id CoreyButler.NVMforWindows
winget upgrade --id GitHub.GitHubDesktop

# Atualiza o Node.js para a versão LTS mais recente
nvm install lts
nvm use lts

# Atualiza o pnpm (via Corepack, mantendo a arquitetura oficial)
corepack prepare pnpm@latest --activate

# Atualiza ferramentas globais instaladas pelo pnpm
pnpm update -g typescript @biomejs/biome
```

<br />

## ✅ 7. Verificação de Sucesso (Check-up)

Para garantir que tudo foi instalado perfeitamente, rode os comandos abaixo. Todos devem retornar um número de versão. Se algum der erro de "comando não reconhecido", reinicie o computador e tente novamente.

```powershell
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

- **Biome** - Oficial da biomejs _(Clique em Switch to Pre-Release Version)_. O coração do nosso JS/TS.
- **Prettier - Code formatter** - O mestre da diagramação para HTML, CSS e Markdown.
- **ESLint** - Padrão da indústria para linting (crucial para projetos com Next.js).

### ⚛️ Ecossistema JS, React & Node

- **ES7+ React/Redux/React-Native snippets** - Atalhos rápidos para criar componentes React (ex: digite `rfce` e dê Tab).
- **Tailwind CSS IntelliSense** - Autocomplete, destaque de sintaxe e linting para classes do Tailwind.
- **DotENV** - Destaca a sintaxe de arquivos `.env` (variáveis de ambiente do Node).
- **Node.js Exec** - Executa o arquivo atual ou código selecionado no Node.js apertando F8.

### 🐙 Produtividade & IA

- **GitLens — Git supercharged** - Mostra quem escreveu cada linha de código e quando (Git Blame inline).
- **GitHub Copilot Chat** - Assistente de Inteligência Artificial integrado ao editor.
- **Error Lens** - Mostra as mensagens de erro e avisos na própria linha do código, sem precisar passar o mouse.
- **Turbo Console Log** - Automatiza a criação de `console.log` para debug rápido no JavaScript.

### 🎨 Visual, Utilitários & HTML

- **Material Icon Theme** - Deixa os ícones das pastas e arquivos maravilhosos e fáceis de identificar.
- **Color Highlight** - Pinta o fundo de códigos hexadecimais (ex: `#FFF`) com a própria cor no código.
- **Auto Rename Tag** - Quando você altera a tag de abertura no HTML/JSX (ex: de `div` para `span`), ele altera a de fechamento junto.
- **Live Server** - Cria um servidor local com recarregamento em tempo real para arquivos HTML puros.
- **CodeSnap** - Tira "fotos" lindas e polidas de trechos do seu código para compartilhar.
- **PowerShell** - Suporte avançado para os scripts de terminal no Windows.

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

1. Crie o arquivo `biome.jsonc` na raiz e cole o código do nosso [biome.jsonc](biome.jsonc).

_Nota_: Caso o framework já tenha gerado um `biome.json`, renomeie para `.jsonc` e substitua o conteúdo. Não pode haver duplicidade.

### 11.2 O Formatador HTML/CSS (`.prettierrc`)

O Biome é o rei do JS, mas o Prettier é o mestre do design visual para tags web.

1. Crie o arquivo `.prettierrc` na raiz e cole o código do nosso [.prettierrc](.prettierrc).

### 11.3 O Acordo de Paz Universal (`.editorconfig`)

Garante que o tamanho do TAB (2 espaços) funcione em qualquer editor de código do mundo (WebStorm, Sublime, etc).

1. Crie o arquivo `.editorconfig` na raiz e cole o código do nosso [.editorconfig](.editorconfig).

### 11.4 Prevenção de Bugs de Sistema (.gitattributes)

Impede que o Windows mude silenciosamente a quebra de linha dos arquivos, o que quebraria os formatadores.

1. Crie o arquivo `.gitattributes` na raiz e cole o código do nosso [.gitattributes](.gitattributes).

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
