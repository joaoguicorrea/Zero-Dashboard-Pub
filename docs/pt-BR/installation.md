# Instalação

[← Documentação](./) · [English](../en/installation.md)

Os downloads estão em **[zerodashboard.com.br/#downloads](https://zerodashboard.com.br/#downloads)**.
Não há mais nada para instalar — sem runtime Python, sem pacote adicional.

---

## Windows

1. Baixe o **`ZeroDashboard-Setup-x64.exe`**.
2. Execute. O instalador oferece português e inglês.
3. Por padrão instala em Arquivos de Programas e pede direito de administrador.
   Se você não tiver, o instalador permite instalar só para o seu usuário.

### O aviso azul do SmartScreen

Na primeira execução o Windows pode mostrar **"O Windows protegeu o seu
computador"**. Isso é esperado e não é aviso de vírus.

O SmartScreen marca qualquer instalador que não esteja assinado com certificado
comercial pago de assinatura de código (EV Code Signing). O Zero Dashboard é um
projeto independente e ainda não tem um — o certificado custa centenas de
dólares por ano, e é uma das coisas para as quais as doações vão.

Para prosseguir:

1. Clique em **Mais informações** na tela azul.
2. Clique em **Executar assim mesmo**.

Se preferir conferir o arquivo antes, veja
[Conferindo o download](#conferindo-o-download) abaixo.

---

## Linux

O build de Linux é compilado no **Rocky Linux 9**, e é isso que define a versão
mínima de biblioteca do sistema. Roda em:

| Distribuição | Versão mínima |
|---|---|
| RHEL, Rocky, AlmaLinux | 9 |
| Ubuntu | 22.04 LTS |
| Debian | 12 (Bookworm) |
| Fedora | 36 |
| openSUSE Leap | 15.4 |

Distribuição mais antiga não é suportada — o binário exige glibc 2.34 ou mais
nova, e não há contorno para isso sem um build separado.

### Debian, Ubuntu, Mint, Pop!_OS

```bash
sudo dpkg -i zerodashboard_amd64.deb
```

### Fedora, RHEL, Rocky, AlmaLinux, openSUSE

```bash
sudo rpm -i zerodashboard-x86_64.rpm
```

### AppImage (qualquer distribuição)

Sem instalação, sem root:

```bash
chmod +x ZeroDashboard-x86_64.AppImage
./ZeroDashboard-x86_64.AppImage
```

---

## Conferindo o download

Toda versão publica um **`SHA256SUMS.txt`** junto dos instaladores. Ele permite
confirmar que o arquivo chegou íntegro.

```bash
# Linux
sha256sum -c SHA256SUMS.txt --ignore-missing
```

```powershell
# Windows PowerShell
Get-FileHash .\ZeroDashboard-Setup-x64.exe -Algorithm SHA256
```

Compare o resultado com a linha correspondente no `SHA256SUMS.txt`.

> Checksum publicado no mesmo site do arquivo prova que o download não corrompeu
> no caminho. Não é assinatura, e não prova autoria — só um certificado de
> assinatura de código faria isso.

---

## Desinstalando

- **Windows** — Configurações → Aplicativos → Zero Dashboard → Desinstalar.
- **Debian/Ubuntu** — `sudo dpkg -r zerodashboard`
- **RHEL/Fedora** — `sudo rpm -e zerodashboard`
- **AppImage** — apague o arquivo.

Suas configurações ficam em `~/.config/oci_gui/`
(`%USERPROFILE%\.config\oci_gui\` no Windows) e não são removidas pelo
desinstalador. Apague essa pasta na mão se quiser começar do zero. Os arquivos
de credencial de nuvem — `~/.aws`, `~/.oci`, `~/.azure` — são das CLIs
respectivas e nunca são tocados.
