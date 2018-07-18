---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran bir hızlı öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "38216219"
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core kullanmaya başlayın

::: moniker range=">= aspnetcore-2.1"

1. Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Bir ASP.NET Core projesi oluşturun. Bir komut kabuğunu açın ve aşağıdaki komutu girin:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [! [] (~/İncludes/webapp-alias-notice.md) içerir [](~/includes/webapp-alias-notice.md)]

3. HTTPS geliştirme sertifikasına güvenmek:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. Uygulamayı çalıştırın:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Gözat [ http://localhost:5001 ](http://localhost:5001).  Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için. Bu uygulama, kişisel bilgileri tutmak değil.

6. Açık *Pages/About.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Gözat [ http://localhost:5001/About ](http://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Yeni bir ASP.NET Core projesi oluşturun.

   Bir komut kabuğunu açın. Aşağıdaki komutu girin:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Uygulama ile aşağıdaki komutları çalıştırın:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Gözat [ http://localhost:5000 ](http://localhost:5000).

5. Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world! Sunucusundaki zamandır @DateTime.Now":

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. .NET Core'u yükleme **SDK yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).

2. Yeni bir ASP.NET Core projesi için bir klasör oluşturun.

   Bir komut kabuğunu açın. Aşağıdaki komutları girin:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Makinenizde bir sonraki SDK sürümü yüklü değilse, oluşturun bir *global.json* 1.0.4 seçmek için dosya SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Yeni bir ASP.NET Core projesi oluşturun.

   ```console
   dotnet new web
   ```

5. Paketleri geri yükleyin.

    ```console
    dotnet restore
    ```

6. Uygulamayı çalıştırın.

   ```console
   dotnet run
   ```

   [Çalıştırma dotnet](/dotnet/core/tools/dotnet-run) gerekirse komut uygulamayı ilk olarak oluşturur.

7. Gözat `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
