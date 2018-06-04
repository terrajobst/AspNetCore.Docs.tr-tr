---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 1b4d765c08c67a65a8752e7f650d4e61ec0d2fbf
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687463"
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core kullanmaya başlayın

::: moniker range=">= aspnetcore-2.1"

1. Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Yeni bir .NET Core projesi oluşturun.

   MacOS ve Linux üzerinde bir terminal penceresi açın. Windows, bir komut istemi açın. Aşağıdaki komutu girin:

    ```terminal
    dotnet new webapp -o aspnetcoreapp
    ```

3. Uygulama ile aşağıdaki komutları çalıştırın:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Gözat [ http://localhost:5000 ](http://localhost:5000).

5. Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world! Sunucusundaki zamandır @DateTime.Now":

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Yeni bir .NET Core projesi oluşturun.

   MacOS ve Linux üzerinde bir terminal penceresi açın. Windows, bir komut istemi açın. Aşağıdaki komutu girin:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Uygulama ile aşağıdaki komutları çalıştırın:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Gözat [ http://localhost:5000 ](http://localhost:5000).

5. Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world! Sunucusundaki zamandır @DateTime.Now":

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. .NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).

2. Yeni bir .NET Core proje için bir klasör oluşturun.

   MacOS ve Linux üzerinde bir terminal penceresi açın. Windows, bir komut istemi açın.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Yeni bir .NET Core projesi oluşturun.

   ```terminal
   dotnet new web
   ```

5. Paketler geri yükleyin.

    ```terminal
    dotnet restore
    ```

6. Uygulamayı çalıştırın.

   ```terminal
   dotnet run
   ```

   [Çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) gerekirse komutu uygulama ilk olarak, derlemeler.

7. Gözat `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end