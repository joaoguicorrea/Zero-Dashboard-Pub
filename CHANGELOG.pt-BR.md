# Changelog

[English](CHANGELOG.md)

Todas as mudanças relevantes do Zero Dashboard, da mais nova para a mais
antiga. Os downloads da versão atual estão em
[zerodashboard.com.br](https://zerodashboard.com.br/#downloads).

---

## v0.5.0 — 9 de setembro de 2026

**A interface agora existe em inglês.**

- A aplicação abre em **inglês (EUA)** por padrão, com uma bandeira no canto
  superior esquerdo para trocar para **português (Brasil)**. A escolha fica
  salva entre execuções.
- Todas as telas foram traduzidas — abas, diálogos, subdiálogos, cabeçalhos de
  coluna, mensagens de status e as mensagens de erro que vêm dos provedores de
  nuvem.
- O instalador do Windows mostra a licença em inglês quando roda em inglês.

**Correções**

- Três sub-abas (Azure Virtual WAN, rede da Azure e rede da AWS) não abriam
  com a interface em inglês.
- O filtro "todos os logs" no Search Logs parava de filtrar sem avisar.

## v0.4.1 — 5 de setembro de 2026

**Corrige o app Linux, que não abria.**

A v0.4.0 saiu com o binário Linux quebrado — abria e morria na sequência. O
Linux passou a ser compilado dentro de um container Rocky Linux 9, com os
componentes casando entre si.

De quebra, o piso de glibc caiu de 2.35 para 2.34, então o `.rpm` passa a rodar
em **RHEL, Rocky e AlmaLinux 9** — o que nunca tinha acontecido. O mesmo
binário foi testado em Rocky 9, Ubuntu 22.04, Debian 12 e Ubuntu 24.04.

Nenhuma mudança na aplicação em si.

## v0.4.0 — 5 de setembro de 2026

**AWS e Azure completos, e o app passa a ser gratuito.**

- **AWS** — login SSO pelo próprio app, sem depender da AWS CLI; criar e listar
  profiles pela interface (portal SSO, chave de acesso, assumir role); criar
  instância EC2, com download da chave no Linux e senha de Administrator no
  Windows; NAT gateways, VPN, Transit Gateway attachments, Direct Connect, e
  detalhes e edição de VPC e subnet.
- **Azure** — login por navegador sem a Azure CLI; criar VM; edição de VNet e
  NSG; detalhes de instância mostrando IP privado, IP público e qual security
  group responde por ela; Virtual WAN.
- **Licenciamento** — o plano comercial foi abandonado. O app é gratuito, com
  opção de doação voluntária.
- **Tempo de abertura** caiu de cerca de 35 segundos para aproximadamente 1
  segundo.

## v0.3.0 — 4 de setembro de 2026

- Criar instância OCI pela aba Compute, com compartimento de rede independente
  e IP privado fixo opcional, validado quanto à disponibilidade.
- Criar e apagar pastas em buckets.
- Volume Groups passam a mostrar nome e capacidade de cada volume.
- IPSec Connections e LPGs com detalhes e edição.
- Corrigido o filtro de Status no Search Logs, que nunca retornava resultado.
- Interface alinhada à identidade visual do site.

## v0.2.1 — 3 de setembro de 2026

A distribuição deixou de ser um zip solto:

- **Windows** — instalador de verdade (Inno Setup) com tela de licença, entrada
  no Menu Iniciar, atalho, desinstalador e suporte a instalação silenciosa.
- **Linux** — AppImage de arquivo único, sem exigir root.
- Ícone próprio do app no executável, no instalador e nos atalhos.

Sem mudanças de funcionalidade em relação à v0.2.0.

## v0.2.0 — 19 de agosto de 2026

- Administração **AWS** completa (EC2, S3, VPC, ELB, Route 53, Elastic IP, NAT,
  Direct Connect).
- Administração **Azure** completa (VMs, Blob, VNet, Load Balancer, Application
  Gateway, DNS, Public IP).
- Validação de IP e CIDR nas regras de firewall.
- Suíte de testes automatizados cobrindo a camada de serviço.

> Esta versão também incluía uma integração com NetBox, que foi **removida em
> versão posterior** e não faz parte da aplicação atual.
