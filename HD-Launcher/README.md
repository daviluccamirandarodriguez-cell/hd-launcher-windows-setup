# HD Launcher v1.6.0 — instalador Setup para Windows

Esta versão mantém o HD Launcher **nativo** com contas locais, HD Store gratuita, quatro jogos (Neon Dodge, Caça-Estrelas, Reflexo Turbo e The Cars HD) e oferece um instalador **`HDLauncher-Setup-v1.6.0.exe`**.

## O que o usuário final recebe

- Executa o **Setup.exe** e segue as telas em português do Brasil.
- HD Launcher é instalado na conta Windows atual (sem solicitar administrador).
- Menu Iniciar e atalho opcional na Área de Trabalho.
- A instalação inclui o aplicativo Python/PySide6 já empacotado, seus módulos e as artes; **não é necessário instalar Python ou Android Studio no computador de quem jogar**.
- Desinstalador em Configurações → Aplicativos instalados (ou Menu Iniciar do Windows).
- Contas e bibliotecas não são removidas durante instalação/desinstalação. Elas ficam em `%APPDATA%\HDLauncher\accounts.db`.

**O ZIP deste projeto não é um Setup.exe já compilado.** Para produzir o executável de instalação, use um dos métodos a seguir.

## Método A — GitHub Actions: você NÃO instala Python no PC

Este é o método recomendado. O servidor Windows do GitHub instala as ferramentas, compila o aplicativo e cria o Setup.exe por você.

1. Instale [GitHub Desktop](https://desktop.github.com/) e faça login na sua conta GitHub.
2. No GitHub Desktop, clique em **File → New repository**. Nome: `hd-launcher-windows-setup`; escolha uma pasta no computador, como Documents. Clique em **Create repository**. Use um repositório separado do projeto Android.
3. Extraia este ZIP. Abra a pasta interna **`HD-Launcher`**; copie **todo o conteúdo dela** para a pasta do repositório recém-criado, no mesmo nível do arquivo `.git` (não coloque uma pasta `HD-Launcher` extra ao redor). Inclua `.github`, `hd_launcher.py`, `setup.iss`, `assets` e os demais arquivos.
4. Volte ao GitHub Desktop. Informe uma mensagem e clique em **Commit to main → Publish repository**. Pode publicar como **Private**. Confira no site do repositório se `.github/workflows/gerar-setup-windows.yml` está no mesmo nível de `hd_launcher.py`.
5. No site do repositório, abra **Actions → Gerar Setup EXE do HD Launcher**. Se não iniciar automaticamente, use **Run workflow → Run workflow**.
6. Aguarde o sucesso (✅). Abra a execução e baixe o artefato **HDLauncher-Setup-v1.6.0** na seção **Artifacts**. Extraia esse ZIP do GitHub: dentro estará **`HDLauncher-Setup-v1.6.0.exe`**.
7. Teste o Setup.exe no seu Windows e instale pelo assistente.

Se ocorrer um erro, abra o job `Compilar Setup para Windows`, expanda a etapa vermelha e compartilhe a mensagem.

O GitHub Actions oferece cotas de uso/armazenamento de acordo com a conta e o tipo de repositório; confira no GitHub antes de executar builds repetidas.

## Método B — Build local no Windows (para desenvolvedores)

Requer **Python 3.12** e **[Inno Setup 6](https://jrsoftware.org/isinfo.php)** somente no computador usado para **criar** o instalador.

Execute `build_setup_windows.bat`. O script instala dependências, roda testes, cria o aplicativo com PyInstaller e compila o instalador com Inno Setup.

O resultado estará em `installer-output\HDLauncher-Setup-v1.6.0.exe`.

Quem baixa o Setup.exe não precisará de Python nem Inno Setup.

## Adicionar o instalador ao site oficial

Depois de obter o `HDLauncher-Setup-v1.6.0.exe` do GitHub:

1. Coloque o executável em `downloads/HDLauncher-Setup-v1.6.0.exe` no **site do HD Launcher** que será enviado ao Cloudflare Pages.
2. No botão de Windows do site, defina o link `downloads/HDLauncher-Setup-v1.6.0.exe`.
3. Publique o ZIP atualizado do site no Cloudflare Pages. Teste baixando e instalando num PC Windows.

**O link para baixar o instalador no site só funcionará após colocar o `.exe` gerado na pasta `downloads`**. Nunca publique um botão apontando para um arquivo que ainda não existe.

## Limitações e segurança

- O `.exe` ainda precisa ser **compilado em Windows** (no seu computador ou no servidor GitHub); não foi criado e testado neste ambiente Linux.
- Instalador sem assinatura digital pode gerar alerta do Microsoft SmartScreen; para distribuição pública profissional, é recomendável obter e usar assinatura de código. Não instrua usuários a ignorar alertas sem verificar a origem do arquivo.
- O programa para Windows é x64; não funciona nativamente em Android/iOS.
- O instalador **não** baixa jogos da internet: quatro jogos gratuitos estão embutidos no HD Launcher.
- Contas são **locais**, sem sincronização online e sem recuperação de senha.
- Não há atualização automática ainda; cada nova versão deverá gerar e publicar um novo Setup.

## Conteúdo importante

- `hd_launcher.py` — interface do launcher
- `cars_native.py` / `cars_physics.py` — corrida The Cars HD nativa, sem HTML
- `mini_games.py` — outros três jogos
- `storage.py` — contas e bibliotecas locais
- `assets/` — logo, ícone e capas de jogos
- `installer/` — artes do instalador
- `setup.iss` — definição do Setup Inno Setup
- `.github/workflows/gerar-setup-windows.yml` — build Windows sem Python instalado no seu computador
- `build_setup_windows.bat` — alternativa de compilação local

## Verificações

Os 17 testes existentes do launcher foram executados com sucesso e o fluxo de build foi verificado estaticamente. **A compilação real do Setup.exe e sua instalação no Windows ainda precisam acontecer**.
