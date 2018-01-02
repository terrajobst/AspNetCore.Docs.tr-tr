---
title: "ASP.NET Core 2.0 ile çalışmaya başlama"
author: rick-anderson
description: "Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici."
keywords: "ASP.NET Core, öğretici, kullanmaya başlama"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 5b8c9f770e749c13bc562f157b4ebfd25a88a4e0
ms.sourcegitcommit: 019e5a0342fd49a94056d14fc7a1a1d0f81d2a39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET çekirdeği ile çalışmaya başlama

> [!NOTE]
> ASP.NET Core en son sürümü için bu yönergeleri verilmiştir. Önceki bir sürümü ile çalışmaya başlamak için mi arıyorsunuz? Bkz: [Bu öğretici 1.1 sürümünü](xref:getting-started-1.1).

1. Yükleme [.NET Core](https://www.microsoft.com/net/core/).

2. Yeni bir .NET Core projesi oluşturun.

   MacOS ve Linux üzerinde bir terminal penceresi açın. Windows, bir komut istemi açın. Aşağıdaki komutu girin:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. Uygulamayı çalıştırın.

    Uygulamayı çalıştırmak için aşağıdaki komutları kullanın:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. Gözat [http://localhost: 5000](http://localhost:5000)

6. Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world! Sunucusundaki zamandır @DateTime.Now ":

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Gözat [http://localhost: 5000/hakkında](http://localhost:5000/About) ve değişiklikleri doğrulayın.

### <a name="next-steps"></a>Sonraki adımlar

Başlama öğreticileri için bkz: [ASP.NET Core öğreticileri](tutorials/index.md)

ASP.NET temel kavramlar ve mimari bir giriş için bkz: [ASP.NET Core giriş](index.md) ve [ASP.NET Core Temelleri](fundamentals/index.md).

Bir ASP.NET Core uygulama .NET Core veya .NET Framework temel sınıf kitaplığı ve çalışma zamanı kullanabilirsiniz. Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
