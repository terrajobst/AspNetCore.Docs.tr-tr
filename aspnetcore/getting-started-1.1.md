---
title: "ASP.NET Core 1.1 ile çalışmaya başlama"
author: rick-anderson
description: "Oluşturma ve ASP.NET Core 1.1 kullanarak basit bir Hello World uygulamasını çalıştırmak için hızlı Bu öğreticiyi izleyin."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: df34c93de265c3e69877c9f68dd69126bb1b850d
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-aspnet-core-11"></a>ASP.NET Core 1.1 ile çalışmaya başlama

> [!NOTE]
> Bu yönergeler için ASP.NET Core 1.1 içindir. En son sürümünü mü istiyorsunuz? Bkz: [geçerli sürümü Bu öğretici,](xref:getting-started).

1. .NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [1.0.5 & 1.1.2 .NET Core SDK 1.0.4 indirmeler sayfası](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

2. Yeni bir .NET Core proje için bir klasör oluşturun.

   MacOS ve Linux üzerinde bir terminal penceresi açın. Windows, bir komut istemi açın.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Yeni bir .NET Core projesi oluşturun.

   ```terminal
   dotnet new web
   ```
   
3.  Paketler geri yükleyin.

    ```terminal
    dotnet restore
    ```

4. Uygulamayı çalıştırın.

   [Çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) gerekirse komutu uygulamayı ilk kez oluşturur.

   ```terminal
   dotnet run
   ```

5. Göz atın `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Sonraki adımlar

Başlama öğreticileri için bkz: [ASP.NET Core öğreticileri](tutorials/index.md)

ASP.NET temel kavramlar ve mimari bir giriş için bkz: [ASP.NET Core giriş](index.md) ve [ASP.NET Core Temelleri](fundamentals/index.md).

Bir ASP.NET Core uygulama .NET Core veya .NET Framework temel sınıf kitaplığı ve çalışma zamanı kullanabilirsiniz. Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
