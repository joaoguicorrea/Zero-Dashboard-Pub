# Privacidade e tratamento de dados

[← Documentação](./) · [English](../en/security.md)

O Zero Dashboard roda inteiramente na sua máquina e fala direto com as APIs dos
provedores de nuvem. Esta página descreve exatamente o que ele lê, o que grava
e o que sai do seu computador — para você poder verificar a afirmação em vez de
acreditar nela.

---

## O que sai da sua máquina

**Requisições para os provedores de nuvem, e nada mais.**

- As chamadas vão para AWS, Azure, OCI e Cloudflare pelos SDKs oficiais e APIs
  HTTPS, autenticadas com as suas próprias credenciais.
- Não há **analytics, telemetria, relatório de erro, verificação de
  atualização** nem "phone home" de espécie nenhuma.
- Não há **servidor no meio**. O projeto mantém um site, e o site só serve os
  instaladores e esta documentação — a aplicação nunca fala com ele.
- O app não abre **porta de rede nenhuma**. Nada consegue se conectar a ele.

A única hora em que um navegador abre é quando você pede: um fluxo de login, ou
o botão "Abrir no console".

## O que ele lê

| Caminho | Para quê |
|---|---|
| `~/.aws/config`, `~/.aws/credentials` | Perfis AWS — os mesmos arquivos da AWS CLI |
| `~/.oci/config` e o arquivo de chave apontado nele | Autenticação OCI |
| Cache de login da Azure CLI | Autenticação Azure, quando você usa esse método |
| `~/.config/oci_gui/` | As configurações do próprio app |

Ele lê esses arquivos; não manda nenhum deles para lugar nenhum.

## O que ele grava

Tudo que o app guarda fica em **`~/.config/oci_gui/`**
(`%USERPROFILE%\.config\oci_gui\` no Windows):

| Arquivo | Conteúdo |
|---|---|
| `app.ini` | Preferências de interface, como o idioma escolhido |
| `cloudflare.ini` | Seu API token da Cloudflare |
| `azure_profiles.ini` | Perfis de service principal do Azure |
| `aws.ini` | Último profile e região usados |
| `activity_log.json` | Histórico de operações concluídas, mantido 30 dias |

Os arquivos que guardam segredo são criados com **permissão só para o dono
(`0600`)** desde o momento em que existem — o app define um umask restritivo
antes de criar, em vez de afrouxar a permissão depois.

Quando você cria um profile AWS pelo app, ele é gravado em `~/.aws/` no formato
padrão, então a AWS CLI enxerga também.

## O que ele nunca guarda

- **Chaves privadas.** Ao obter a senha de Administrator de uma EC2 Windows, a
  chave privada do key pair é usada em memória e descartada. O app nunca a
  grava em disco.
- **Senhas que você digita** para uma VM nova no Azure. Vão para a API do Azure
  e não ficam localmente.
- **Suas credenciais de nuvem**, além dos arquivos de perfil descritos acima —
  que são os formatos das próprias CLIs, nos locais das próprias CLIs.

---

## Permissões que o app precisa

O Zero Dashboard só consegue fazer o que a sua credencial permite. Ele não pede
nada além das chamadas de API por trás da tela que você está olhando.

Se quiser escopar uma credencial de forma restrita, dê leitura nos serviços que
você pretende navegar e escrita só onde pretende alterar. Um perfil
somente-leitura produz um painel somente-leitura, e essa é uma forma legítima
de rodar — boa parte das abas é de conferência, não de alteração.

## Operações destrutivas

O app consegue apagar e alterar infraestrutura de verdade, porque é para isso
que ele serve. Toda ação destrutiva pede confirmação antes, e a confirmação diz
qual é a consequência em vez de só perguntar se você tem certeza — por exemplo,
que apagar um NAT gateway faz as instâncias em subnet privada perderem a saída,
e que um novo vai subir com outro IP.

## Assinatura de código

O instalador do Windows **não é assinado** com certificado comercial de
assinatura de código, e é por isso que o SmartScreen mostra aviso na primeira
execução. Veja
[Instalação](installation.md#o-aviso-azul-do-smartscreen).

Cada versão publica um `SHA256SUMS.txt` junto dos instaladores. Ele confirma
que o download chegou íntegro; não é assinatura e não prova autoria.

## Relatando um problema de segurança

Encontrou alguma coisa? Por favor mande um e-mail para
**contato@zerodashboard.com.br** em vez de abrir issue pública, para que possa
ser corrigido antes de ser descrito em público.
