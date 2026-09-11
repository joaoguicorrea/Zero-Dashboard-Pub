# Configurando perfis de nuvem

[← Documentação](./) · [English](../en/cloud-profiles.md)

O Zero Dashboard não guarda credencial de nuvem própria. Ele lê os mesmos
arquivos de configuração que as CLIs oficiais usam, então o que você já tem
configurado funciona na hora — e o que você criar no app funciona na CLI
também.

| Provedor | De onde ele lê |
|---|---|
| AWS | `~/.aws/config` e `~/.aws/credentials` |
| Azure | Login da Azure CLI, ou um service principal cadastrado no app |
| OCI | `~/.oci/config` |
| Cloudflare | Um API token que você cola no app |

No Windows, `~` é `%USERPROFILE%`.

---

## AWS

O app cria os perfis para você — **Conexão → AWS → Profiles…** — de três
formas.

### Pelo portal SSO (recomendado para conta de empresa)

Se a sua empresa usa o AWS IAM Identity Center, é esta. Você precisa da **URL
do access portal**, algo como `https://d-xxxxxxxxxx.awsapps.com/start`, que
está no convite que a empresa mandou.

O app abre a página de autorização, mostra o código, e depois que você aprova
lista todas as contas e permission sets a que você tem acesso. Marque os que
quiser e ele grava os perfis. Não exige a AWS CLI.

### Com chave de acesso

Cole a access key e a secret key. Session token só é necessário para credencial
temporária (STS).

A chave vai para `~/.aws/credentials` com permissão `0600` e a região para o
`~/.aws/config` — a mesma separação que a AWS CLI faz.

### Assumindo uma role

Aponte um perfil de origem que tenha credencial de verdade e informe o ARN da
role. O perfil de origem pode ser de SSO, e nesse caso o login dele vale para
os dois.

### Fazendo na mão

```ini
# ~/.aws/credentials
[producao]
aws_access_key_id = AKIA...
aws_secret_access_key = ...

# ~/.aws/config
[profile producao]
region = us-east-1
```

📖 [Documentação de configuração da AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

---

## Azure

Duas opções, ambas em **Conexão → Azure**.

### Login por navegador (device code)

O app mostra um código, você aprova no navegador, e o login fica salvo na
máquina até expirar. **Isso não exige a Azure CLI.**

Preencha o Tenant ID se você é convidado em mais de um tenant — sem ele o login
cai no tenant padrão e pode não enxergar a subscription que você procura.

### Service principal

Para automação, ou quando login por navegador não é opção: tenant ID, client ID
e client secret. Perfil de service principal autentica sozinho e nunca abre
navegador.

📖 [Documentação de autenticação da Azure CLI](https://learn.microsoft.com/cli/azure/authenticate-azure-cli)

---

## OCI

O Zero Dashboard lê o `~/.oci/config`. Se você não tem um, o caminho mais
rápido é a OCI CLI:

```bash
oci setup config
```

Ou escreva na mão:

```ini
[DEFAULT]
user=ocid1.user.oc1..aaaa...
fingerprint=aa:bb:cc:...
key_file=~/.oci/oci_api_key.pem
tenancy=ocid1.tenancy.oc1..aaaa...
region=sa-saopaulo-1
```

O arquivo da chave privada tem que ser legível só por você:

```bash
chmod 600 ~/.oci/oci_api_key.pem
```

📖 [Configuração de SDK e CLI da OCI](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm)

---

## Cloudflare

Crie um API token no painel da Cloudflare
(**My Profile → API Tokens → Create Token**) e cole em
**Conexão → Cloudflare**.

Dê só as permissões que você precisa. Para as abas que o app oferece, em geral:

| Para usar | O token precisa de |
|---|---|
| Zones e DNS Records | `Zone:Read`, `DNS:Edit` |
| Firewall Events e IP Access Rules | `Zone:Read`, `Firewall Services:Edit` |
| WAF Custom Rules | `Zone:Read`, `Zone WAF:Edit` |
| Tunnels e Access | `Account:Cloudflare Tunnel:Edit`, `Access: Apps and Policies:Edit` |

O token fica em `~/.config/oci_gui/cloudflare.ini` com permissão `0600`, e
aparece mascarado na interface.

📖 [Documentação de API token da Cloudflare](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)

---

## Boas práticas

- **Escope a permissão.** O Zero Dashboard só consegue fazer o que a credencial
  permite. Um perfil somente-leitura te dá um painel somente-leitura, o que é
  uma forma perfeitamente razoável de usar.
- **Mantenha os arquivos de credencial em `0600`.** O app faz isso no que ele
  grava; os arquivos escritos pelas CLIs são responsabilidade delas.
- **Prefira credencial temporária.** Login por SSO e device code expira
  sozinho; access key de vida longa não.
