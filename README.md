# quiosque-ms-registry
Passo a passo da configuração de um ambiente quiosque usando o Registro do Windows 10 (MS Registry)

## 1. Criação de um usuário local, sem acesso de admin e sem senha
[Criar um usuário local ou conta de administrador no Windows](https://support.microsoft.com/pt-br/windows/criar-um-usu%C3%A1rio-local-ou-conta-de-administrador-no-windows-20de74e0-ac7f-3502-a866-32915af2a34d#:~:text=11Windows%2010-,Criar%20uma%20conta%20de%20usu%C3%A1rio%20local,outro%20usu%C3%A1rio%2C%20selecione%20Adicionar%20conta.)

## 2. Fazer uma backup do registro atual

[Como fazer o backup e a restauração do Registro no Windows](https://support.microsoft.com/pt-br/topic/como-fazer-o-backup-e-a-restaura%C3%A7%C3%A3o-do-registro-no-windows-855140ad-e318-2a13-2829-d428a2ab0692)

## 3. Configurações iniciais do ambiente

[Preparar um dispositivo para a configuração de quiosque](https://learn.microsoft.com/pt-br/windows/configuration/kiosk-prepare)

### Desabilitar Notificações de Atualização do Windows

Localização no Registro:

HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate

Configurações:

SetUpdateNotificationLevel (DWORD 32 bits): 1

UpdateNotificationLevel (DWORD 32 bits): 2

### Substituir Tela Azul por Tela em Branco para Erros do Sistema Operacional

Localização no Registro: 

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl

Configuração:

DisplayDisabled (DWORD 32 bits): 1

### Auto Login ao Reiniciar o Computador

Localização no Registro: 

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon

Configurações:

AutoAdminLogon: 1

DefaultUserName: quiosque

DefaultPassword: 

### Desabilitar a Mídia Removível

Localização no Registro: 

Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DeviceInstall\Restrictions

Configuração:

DenyRemovableDevices: 1

### 4. Restrições ao usuário quiosque

[Políticas impostas em dispositivos de quiosque](https://learn.microsoft.com/pt-br/windows/configuration/kiosk-policies)

Primeiro, é necessário verificar qual o ID do usuário quisoque, para definir qual será o path do HKEY_USERS a ser configurado. Isso é possível rodando este comando no cmd dentro da sessão do usuário quiosque:

whoami /user

No caso exemplicado, temos:

HKEY_USERS\S-1-5-21-2276003742-892242862-2213583387-1002


#### Retrições de explorador de arquivos, barra de tarefas e página incial
- **Localização no Registro**: `Computer\HKEY_USERS\[User SID]\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer`
- **Configurações**:
  - Remover o acesso aos menus de contexto para a barra de tarefas: `NoTrayContextMenu` (DWORD 32 bits): `1`
  - Limpar o histórico de documentos abertos recentemente ao sair: `ClearRecentDocsOnExit` (DWORD 32 bits): `1`
  - Impedir que os usuários personalizem a tela inicial: `NoChangeStartMenu` (DWORD 32 bits): `1`
  - Remover o comando Executar do menu Iniciar: `NoRun` (DWORD 32 bits): `1`
  - Bloquear todas as configurações da barra de tarefas: `TaskbarLockAll` (DWORD 32 bits): `1`
  - Bloquear a Barra de Tarefas: `LockTaskbar` (DWORD 32 bits): `1`
  - Impedir que os usuários redimensionem a barra de tarefas: `TaskbarNoAddRemoveToolbar` (DWORD 32 bits): `1`
  - Remover a lista de programas frequentes do menu Iniciar: `NoStartMenuMFUprogramsList` (DWORD 32 bits): `1`
  - Remover o ícone de Segurança e Manutenção: `HideSCAHealth` (DWORD 32 bits): `1`
  - Remover a lista Todos os Programas do menu Iniciar: `NoStartMenuMorePrograms` (DWORD 32 bits): `1`
  - Impedir o acesso a unidades de Meu Computador: `NoViewOnDrive` (DWORD 32 bits): `67108863`

- **Localização no Registro**: `Computer\HKEY_USERS\[User SID]\SOFTWARE\Policies\Microsoft\Windows\Explorer`
- **Configurações**:
  - Impedir que os usuários desinstalem aplicativos da tela inicial: `NoUninstallFromStart` (DWORD 32 bits): `1`
  - Não permitir a fixação de itens nas Listas de Atalhos: `NoPinningToDestinations` (DWORD 32 bits): `1`
  - Não permitir a fixação de programas na Barra de Tarefas: `NoPinningToTaskbar` (DWORD 32 bits): `1`
  - Não exibir nem controlar itens nas Listas de Atalhos a partir de locais remotos: `NoRemoteDestinations` (DWORD 32 bits): `1`
  - Remover Notificações e a Central de Ações: `DisableNotificationCenter` (DWORD 32 bits): `1`
  - Remover os programas fixados da barra de tarefas: `TaskbarNoPinnedList` (DWORD 32 bits): `1`

#### Políticas de Sistema (System Policies)
- **Localização no Registro**: `Computer\HKEY_USERS\[User SID]\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
- **Configurações**:
  - Remover o Gerenciador de Tarefas: `DisableTaskMgr` (DWORD 32 bits): `1`
  - Remover a opção de Alterar Senha na interface do usuário de Opções de Segurança: `DisableChangePassword` (DWORD 32 bits): `1`


