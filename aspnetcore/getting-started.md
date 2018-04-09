---
title: ASP.NET çekirdeği ile çalışmaya başlama
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET çekirdeği ile çalışmaya başlama

> [!NOTE]
> ASP.NET Core en son sürümü için bu yönergeleri verilmiştir. Bkz: [ASP.NET Core 1.1 ile çalışmaya başlama](xref:getting-started-1.1) bu belgenin 1.1 sürümünü için.

1. Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Yeni bir .NET Core projesi oluşturun.

   MacOS ve Linux üzerinde bir terminal penceresi açın. Windows, bir komut istemi açın. Aşağıdaki komutu girin:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Uygulamayı çalıştırın.

    Uygulamayı çalıştırmak için aşağıdaki komutları kullanın:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Göz atın [http://localhost:5000](http://localhost:5000)

5. Açık <em>Pages/About.cshtml</em> ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world! Sunucusundaki zamandır @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.

### <a name="next-steps"></a>Sonraki adımlar

Başlama öğreticileri için bkz: [ASP.NET Core öğreticileri](tutorials/index.md)

ASP.NET temel kavramlar ve mimari bir giriş için bkz: [ASP.NET Core giriş](index.md) ve [ASP.NET Core Temelleri](fundamentals/index.md).

Bir ASP.NET Core uygulama .NET Core veya .NET Framework temel sınıf kitaplığı ve çalışma zamanı kullanabilirsiniz. Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
