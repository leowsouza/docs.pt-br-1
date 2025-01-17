---
title: Instalar o .NET Core no Ubuntu 16, 4 Package Manager-.NET Core
description: Use um Gerenciador de pacotes para instalar SDK do .NET Core e tempo de execução no Ubuntu 16, 4.
author: thraka
ms.author: adegeo
ms.date: 11/06/2019
ms.openlocfilehash: 2409dc9f5e480b309bf7c9aa09b733ded01d772f
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450950"
---
# <a name="ubuntu-1604-package-manager---install-net-core"></a>Gerenciador de pacotes do Ubuntu 16, 4 – instalar o .NET Core

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

Este artigo descreve como usar um Gerenciador de pacotes para instalar o .NET Core no Ubuntu 16, 4. Se você estiver instalando o tempo de execução, sugerimos que instale o [ASP.NET Core Runtime](#install-the-aspnet-core-runtime), pois ele inclui o .NET Core e ASP.NET Core Runtimes.

## <a name="register-microsoft-key-and-feed"></a>Registrar chave e feed da Microsoft

Antes de instalar o .NET, você precisará:

- Registrar a chave da Microsoft
- registrar o repositório do produto
- Instalar dependências necessárias

Isso só precisa ser feito uma vez por computador.

Abra um terminal e execute os comandos a seguir.

```bash
wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-core-sdk"></a>Instalar o SDK do .NET Core

Atualize os produtos disponíveis para instalação e, em seguida, instale o SDK do .NET Core. Em seu terminal, execute os comandos a seguir.

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-3.0
```

> [!IMPORTANT]
> Se você receber uma mensagem de erro semelhante a **não é possível localizar o pacote dotnet-SDK-3,0**, consulte a seção [solucionar problemas do Gerenciador de pacotes](#troubleshoot-the-package-manager) .

## <a name="install-the-aspnet-core-runtime"></a>Instalar o ASP.NET Core Runtime

Atualize os produtos disponíveis para instalação e instale o ASP.NET Core Runtime. Em seu terminal, execute os comandos a seguir.

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install aspnetcore-runtime-3.0
```

> [!IMPORTANT]
> Se você receber uma mensagem de erro semelhante a **não é possível localizar o pacote aspnetcore-Runtime-3,0**, consulte a seção [solucionar problemas do Gerenciador de pacotes](#troubleshoot-the-package-manager) .

## <a name="install-the-net-core-runtime"></a>Instalar o tempo de execução do .NET Core

Atualize os produtos disponíveis para instalação e, em seguida, instale o tempo de execução do .NET Core. Em seu terminal, execute os comandos a seguir.

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-runtime-3.0
```

> [!IMPORTANT]
> Se você receber uma mensagem de erro semelhante a **não é possível localizar o pacote dotnet-Runtime-3,0**, consulte a seção [solucionar problemas do Gerenciador de pacotes](#troubleshoot-the-package-manager) .

## <a name="how-to-install-other-versions"></a>Como instalar outras versões

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a>Solucionar problemas do Gerenciador de pacotes

Se você receber uma mensagem de erro semelhante a **não é possível localizar o pacote {The .NET Core Package}** , execute os comandos a seguir.

```bash
sudo dpkg --purge packages-microsoft-prod && sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install {the .NET Core package}
```

Se isso não funcionar, você poderá executar uma instalação manual com os comandos a seguir.

```bash
sudo apt-get install -y gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget -q https://packages.microsoft.com/config/ubuntu/16.04/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get install -y apt-transport-https
sudo apt-get update
sudo apt-get install {the .NET Core package}
```
