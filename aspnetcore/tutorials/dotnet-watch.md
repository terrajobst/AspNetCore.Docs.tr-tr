---
title: Dosya İzleyici kullanarak ASP.NET Core uygulamaları geliştirme
author: rick-anderson
description: Bu öğretici, aracı yükleme ve .NET Core CLI dosya İzleyici (dotnet izleme) bir ASP.NET Core uygulamada kullanma gösterilmektedir.
manager: wpickett
ms.author: riande
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: 016ee107ae646ed43d8a98e97fd2d5b41356910e
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341853"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a>Dosya İzleyici kullanarak ASP.NET Core uygulamaları geliştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [kazanan Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişiklik kaynak dosyaları, komut. Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtım tetikleyebilir.

Bu öğretici varolan bir web API iki uç nokta ile kullanır: biri toplam ve bir ürün döndüren bir döndürür. Ürün yöntemi Bu öğreticide sabit bir hata var.

Karşıdan [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). İki projeden oluşan: *WebApp* (bir ASP.NET Core web API) ve *WebAppTests* (web API'si için birim testleri).

Komut kabuğu'na gidin *WebApp* klasör. Şu komutu çalıştırın:

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

Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`). Döndürdüğü `9`değil `20` beklediğiniz gibi. Öğreticide daha sonra bu sorun düzeltilmiştir.

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a>Ekleme `dotnet watch` bir proje

`dotnet watch` Dosya İzleyici aracı .NET Core SDK 2.1.300 sürümü ile eklenmiştir. Aşağıdaki adımlar .NET Core SDK'ın önceki bir sürümünü kullanırken gerekli değildir.

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

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a>.NET Core CLI komutları kullanarak çalıştırın `dotnet watch`

Tüm [.NET Core CLI komutu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`. Örneğin:

| Komut | İzleme komut |
| ---- | ----- |
| dotnet çalıştırın | dotnet izleme çalıştırın |
| dotnet -f netcoreapp2.0 çalıştırın | -f netcoreapp2.0 çalıştırmak dotnet izleme |
| -f netcoreapp2.0--çalıştırmak dotnet--arg1 | -f netcoreapp2.0--çalıştırmak dotnet izleme--arg1 |
| DotNet test | DotNet izleme test |

Çalıştırma `dotnet watch run` içinde *WebApp* klasör. Konsol çıktısı gösterir `watch` başlatıldı.

## <a name="make-changes-with-dotnet-watch"></a>İle değişiklik `dotnet watch`

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

## <a name="run-tests-using-dotnet-watch"></a>Kullanarak testleri çalıştırma `dotnet watch`

1. Değişiklik `Product` yöntemi *MathController.cs* toplamı döndürme geri dön. Dosyayı kaydedin.
1. Komut kabuğu'na gidin *WebAppTests* klasör.
1. Çalıştırma [dotnet geri yükleme](/dotnet/core/tools/dotnet-restore).
1. Çalıştırma `dotnet watch test`. Bir test başarısız olduğunu ve İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Düzeltme `Product` yöntemi kod ürün döndürecek şekilde. Dosyayı kaydedin.

`dotnet watch` dosya değişikliği algılar ve testleri yeniden çalıştırır. Konsol çıktısı testleri geçirilen gösterir.

## <a name="customize-files-list-to-watch"></a>İzlemek için dosya listesini Özelleştir

Varsayılan olarak, `dotnet-watch` aşağıdaki glob desenleri eşleşen tüm dosyaları izler:

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

Daha fazla öğe düzenleyerek izleme listesine eklenebilir *.csproj* dosya. Öğeleri tek tek veya glob desenler kullanılarak belirtilebilir.

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a>İzlemeniz gereken dosyaları çevirin

`dotnet-watch` varsayılan ayarlarını yoksaymak için yapılandırılabilir. Belirli dosyaları yoksaymak için ekleme `Watch="false"` öğe tanımında özniteliğini *.csproj* dosyası:

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a>Özel İzleme projeleri

`dotnet-watch` C# projeleri için sınırlı değildir. Özel İzleme projeleri farklı senaryolar işlemek için oluşturulabilir. Aşağıdaki proje düzeni göz önünde bulundurun:

* **Test /**
  * *UnitTests/UnitTests.csproj*
  * *IntegrationTests/IntegrationTests.csproj*

Her iki proje izlemek için hedef ise, her iki proje izlemek için yapılandırılmış bir özel proje dosyası oluşturun:

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

Her iki projelerde izleme dosyasını başlatmak için değiştirin *test* klasör. Aşağıdaki komutu yürütün:

```console
dotnet watch msbuild /t:Test
```

Herhangi bir dosya ya da test projesinde değiştiğinde VSTest yürütür.

## <a name="dotnet-watch-in-github"></a>`dotnet-watch` Github'da

`dotnet-watch` GitHub parçası olan [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).
