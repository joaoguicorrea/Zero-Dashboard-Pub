# Perguntas frequentes

[← Documentação](./) · [English](../en/faq.md)

### É gratuito mesmo?

É. Sem custo, sem período de teste, sem chave de ativação, sem funcionalidade
travada. É gratuito hoje e continua gratuito.

### É código aberto?

Não. Gratuito e código aberto são coisas diferentes: o Zero Dashboard não custa
nada, mas os direitos sobre ele continuam com o autor e o código não é
publicado. Repassar o instalador original, sem modificação e sem cobrar, é
permitido. Veja a [licença](../../LICENSE.txt).

### Por que o Windows mostra um aviso azul?

Porque o instalador não é assinado com certificado comercial pago de assinatura
de código. Ele custa centenas de dólares por ano, e é uma das coisas para as
quais as doações vão. Os passos para prosseguir estão em
[Instalação](installation.md#o-aviso-azul-do-smartscreen).

### Ele manda meus dados para algum lugar?

Não. Sem telemetria, sem analytics, sem verificação de atualização. Suas
credenciais ficam na sua máquina e cada chamada vai direto para o provedor de
nuvem. Os detalhes estão em
[Privacidade e tratamento de dados](security.md).

### Preciso ter a AWS CLI ou a Azure CLI instalada?

Não. O app consegue logar nas duas sozinho, pelo navegador. As CLIs são
suportadas se você já usa — o app lê os mesmos arquivos de perfil — mas não são
requisito.

### Preciso criar uma conta?

Não. Não existe conta do Zero Dashboard. A autenticação é inteiramente contra
os seus próprios provedores de nuvem.

### Funciona no macOS?

Por enquanto não. Só Windows e Linux.

### Quais distribuições Linux são suportadas?

Qualquer uma com glibc 2.34 ou mais nova: RHEL/Rocky/Alma 9, Ubuntu 22.04+,
Debian 12+, Fedora 36+, openSUSE Leap 15.4+. A tabela está em
[Instalação](installation.md#linux).

### Dá para usar com credencial somente-leitura?

Dá, e é uma forma razoável de rodar. O app só consegue fazer o que a sua
credencial permite; com acesso somente-leitura você tem um painel
somente-leitura. Boa parte da aplicação é de conferência, não de alteração.

### Dá para trocar o idioma da interface?

Dá. Inglês (EUA) e português (Brasil), trocados pela bandeira no canto superior
esquerdo. O inglês é o padrão. A escolha fica salva, e as telas já abertas
continuam no idioma anterior até você reabrir o app.

### Suporta mais de uma conta por provedor?

Suporta. Os perfis são trocados na aba Conexão, e cada provedor mantém a sua
própria seleção — dá para estar com a AWS numa conta e a OCI em outra.

### Achei um problema, ou tenho uma ideia

Abra uma issue neste repositório, ou mande e-mail para
[contato@zerodashboard.com.br](mailto:contato@zerodashboard.com.br).
Para problema de segurança, por favor use o e-mail em vez de issue pública.
