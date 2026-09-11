# Inventário de recursos

[← Documentação](./) · [English](../en/features.md)

O que a aplicação realmente faz, aba por aba. Esta é a lista detalhada; o
[README](../../README.pt-BR.md) tem o resumo.

Tudo que está aqui existe na versão distribuída. Onde a operação é destrutiva,
o app pede confirmação e diz qual é a consequência antes de executar.

---

## Conexão

O ponto de partida. O Zero Dashboard lê os perfis de nuvem já configurados na
sua máquina e permite alternar entre contas e regiões sem logar de novo.

- **AWS** — perfis de `~/.aws/config` e `~/.aws/credentials`, os mesmos
  arquivos que a AWS CLI usa. Perfil criado no app funciona na CLI também, e
  vice-versa. Três formas de criar:
  - **Portal SSO** — login por device code no navegador, com seleção de conta
    e permission set. Não exige a AWS CLI instalada.
  - **Chave de acesso** — access key comum, ou temporária com session token.
  - **Assumir role** — empresta a identidade de um perfil de origem para
    assumir a role.
- **Azure** — login por navegador (device code) ou service principal. O método
  por navegador não depende da Azure CLI estar instalada.
- **OCI** — lê o `~/.oci/config`.
- **Cloudflare** — API token, guardado localmente.

Valores sensíveis (ARN, OCIDs, fingerprint, API token) ficam mascarados na
interface, com um botão para revelar.

## Atividades

Painel central para tudo que demora ou precisa de acompanhamento:

- **Downloads em andamento**, com barra de progresso por bytes.
- **Restores de Archive Tier** sendo submetidos, em paralelo.
- **Restores aguardando confirmação**, com lembretes em 15, 30, 45 e 60
  minutos — um restore de Archive da OCI leva cerca de uma hora, e o app te
  lembra de voltar e conferir em vez de deixar no escuro.
- **Histórico** das operações concluídas, mantido entre execuções por 30 dias.
  Cada entrada pode ter **artefato baixável** — por exemplo, o CSV com quais
  IPs falharam numa operação em lote da Cloudflare.

---

## OCI

### Compute
Lista de instâncias, ações de ciclo de vida (iniciar, parar, reiniciar),
detalhes completos, edição de tags e **redimensionamento de shape Flex** com a
lista de shapes disponíveis no compartimento. A criação de instância cobre
imagem, shape, OCPU e memória para shapes Flex, compartimento de rede, VCN e
subnet, IP privado (próximo livre ou um específico, validado contra a subnet),
IP público e boot volume.

### VCN / Rede
Subnets, security lists, NSGs e route tables, com edição de regra em todos.

### Storage
Block volumes, boot volumes e volume groups; backups, restauração a partir de
backup e anexar volume a uma VM. Para buckets: navegador de objetos, criação e
exclusão de pastas, pre-authenticated requests (PARs), **download recursivo de
pasta** e soma de tamanho por prefixo.

O **restore de Archive Tier** roda em paralelo e reporta progresso — é essa a
operação que o painel de Atividades acompanha até o fim.

### VPN / DRG
DRGs, CPEs, conexões IPSec com o estado do túnel indicado por cor, detalhe e
edição de túnel, e local peering gateways (detalhes e edição). O arquivo de
configuração do CPE para o equipamento do outro lado pode ser baixado.

### Search Logs
Consultas ao OCI Logging Search, com buscas salvas, presets de Email Delivery,
intervalo de datas e exportação em CSV ou JSON.

### Email Delivery
Approved senders, domínios e seus DKIMs, e métricas separadas por data,
remetente e domínio.

### Suppression List
Listar, adicionar e remover supressões, com filtro curinga que aceita `*` em
qualquer posição do termo.

---

## AWS

### EC2
Instâncias com ações de ciclo de vida, detalhes e criação (AMI, tipo, subnet,
key pair, security groups, IP privado, volume). Em instância Windows, obtenção
da senha de Administrator usando a chave privada do key pair — a chave é usada
em memória e nunca é gravada.

### S3
Buckets, listagem e download de objetos.

### VPC / Rede
VPCs e subnets (detalhes e edição), security groups com edição de regra, route
tables, network ACLs, internet gateways, NAT gateways, VPC endpoints,
**Transit Gateways** com attachments, **VPN** (customer gateways, VPN gateways,
conexões e o arquivo de configuração do roteador) e **Direct Connect** com
virtual interfaces, incluindo as rotas anunciadas na VIF.

### Load Balancers
Application e Network Load Balancers, com listeners e target groups.

### Route 53
Hosted zones e records, com criação, edição e exclusão.

### Elastic IPs
Alocar, associar a uma instância, desassociar e liberar.

---

## Azure

### Virtual Machines
Listagem com ações de ciclo de vida (iniciar, desligar, desalocar, reiniciar).
Os detalhes mostram o **IP privado, o IP público e qual NSG responde pela
máquina** — inclusive as regras efetivas, que o Azure calcula combinando o NSG
da NIC com o da subnet. A criação de VM cobre resource group, tamanho, imagem,
disco, rede e credenciais — chave SSH no Linux, usuário e senha no Windows.

### Blob Storage
Storage accounts, containers, listagem e download de blobs.

### VNet / Rede
VNets e subnets com detalhes e edição, NSGs com edição de regra, e route
tables. Os detalhes de subnet incluem a contagem de IPs livres sobre
utilizáveis e a delegação, se houver.

### Virtual WAN
Virtual WANs, hubs, conexões de VNet, VPN site-to-site, VPN sites,
ExpressRoute, route tables e **rotas efetivas** — o equivalente ao "Effective
Routes" do portal, que é o único lugar onde a propagação fica visível.

### Load Balancer, Application Gateway, DNS e Public IPs
Detalhes de load balancer com frontend IPs e backend pools; Application Gateway
com iniciar e parar; zonas e records de DNS; listagem de IPs públicos.

---

## Cloudflare

- **Firewall Events** — com filtros e resumo dos principais IPs e ações.
- **Zones** e **DNS Records**, com criação, edição e exclusão.
- **WAF Custom Rules** — criação, edição, reordenação e remoção.
- **IP Access Rules**, incluindo **entrada em lote**: cole vários IPs de uma
  vez, escolha a ação e se vale para uma zona ou para a conta inteira. As
  chamadas rodam em paralelo, e as falhas voltam como CSV baixável no painel
  de Atividades.
- **Tunnels** e seus public hostnames.
- **Access Apps** e suas policies.
- **Usuários Zero Trust**, incluindo revogação de sessão.

---

## Em toda a aplicação

- **Dois idiomas** — inglês (EUA) e português (Brasil), trocados pela bandeira
  no canto.
- **Tabelas ordenáveis** — clique no cabeçalho para ordenar; copie uma célula,
  uma linha ou a tabela inteira.
- **Exportação CSV** com BOM UTF-8, para o texto acentuado abrir certo no Excel.
- **Confirmação antes de qualquer coisa destrutiva**, dizendo qual será a
  consequência — não só "tem certeza?".
- **Abrir no console do provedor** — quando você precisa do console oficial
  para algo que o app não cobre, ele te leva direto naquele recurso.
