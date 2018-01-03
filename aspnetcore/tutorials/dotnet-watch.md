---
title: "DotNet Gözcü kullanarak ASP.NET Core uygulamaları geliştirme"
author: rick-anderson
description: "Bu öğretici, yükleme ve ASP.NET Core uygulamada .NET Core CLI dosya İzleyici (dotnet izleme) aracı kullanma gösterilmektedir."
keywords: ASP.NET Core, dotnet izleme kullanma
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 1b26beaa41f4b38e0cfd2c8300cb521a3dcce47d
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>DotNet Gözcü kullanarak ASP.NET Core uygulamaları geliştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [kazanan Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch`çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişiklik kaynak dosyaları, komut. Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtım tetikleyebilir.

Bu öğreticide, var olan bir Web API uygulaması ile iki uç nokta kullanırız: toplam ve bir ürün döndüren bir döndüren bir. Ürün yöntemi bu öğreticinin bir parçası olarak düzeltme bir hata içeriyor.

Karşıdan [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). İki proje içerir: *WebApp* (ASP.NET çekirdek Web API) ve *WebAppTests* (Web API için birim testleri).

Komut kabuğu'na gidin *WebApp* klasörü ve aşağıdaki komutu çalıştırın:

```console
dotnet run
```

Konsol çıktısı aşağıdakine benzer mesajlarını (uygulama çalıştıran ve istekleri bekleyen gösteren):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`. Sonucu görmelisiniz `9`.

Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`). Döndürdüğü `9`değil `20` beklediğiniz gibi. Biz bu öğreticide daha sonra düzeltmesi.

## <a name="add-dotnet-watch-to-a-project"></a>Ekleme `dotnet watch` bir proje

1. Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini referansı *.csproj* dosyası:

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. Yükleme `Microsoft.DotNet.Watcher.Tools` aşağıdaki komutu çalıştırarak paketi:
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>.NET Core CLI komutları kullanarak çalıştırma`dotnet watch`

Tüm [.NET Core CLI komutu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`. Örneğin:

| Komut | İzleme komut |
| ---- | ----- |
| dotnet çalıştırın | dotnet izleme çalıştırın |
| dotnet -f netcoreapp2.0 çalıştırın | -f netcoreapp2.0 çalıştırmak dotnet izleme |
| -f netcoreapp2.0--çalıştırmak dotnet--arg1 | -f netcoreapp2.0--çalıştırmak dotnet izleme--arg1 |
| DotNet test | DotNet izleme test |

Çalıştırma `dotnet watch run` içinde *WebApp* klasör. Konsol çıktısı gösterir `watch` başlatıldı.

## <a name="making-changes-with-dotnet-watch"></a>Değişiklikleri yapma`dotnet watch`

Emin olun `dotnet watch` çalışıyor.

Hata düzeltme `Product` yöntemi *MathController.cs* ürünü ve toplam döndürecek şekilde:

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Dosyayı kaydedin. Konsol çıktısı belirten `dotnet watch` bir dosya değişiklik algılandı ve uygulamayı yeniden.

Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.

## <a name="running-tests-using-dotnet-watch"></a>Kullanarak testleri çalıştırma`dotnet watch`

1. Değişiklik `Product` yöntemi *MathController.cs* geri toplamı döndürmek için ve dosyayı kaydedin.
1. Komut kabuğu'na gidin *WebAppTests* klasör.
1. Çalıştırma `dotnet restore`.
1. Çalıştırma `dotnet watch test`. Bir test başarısız oldu ve bu İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Düzeltme `Product` yöntemi kod ürün döndürecek şekilde. Dosyayı kaydedin.

`dotnet watch`dosya değişikliği algılar ve testleri yeniden çalıştırır. Konsol çıktısı testleri geçirilen gösterir.

## <a name="dotnet-watch-in-github"></a>github'da dotnet izleme

DotNet izleme GitHub bir parçasıdır [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).

[MSBuild bölüm](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) , [dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) dotnet izleme izleniyor MSBuild proje dosyasından nasıl yapılandırılabilir açıklar. [Dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) Bu öğreticide kapsanmayan dotnet izleme hakkında bilgi içerir.
