---
title: Bir dosya İzleyicisi'ni kullanarak ASP.NET Core uygulamaları geliştirin
author: rick-anderson
description: Bu öğreticide, aracı yükleme ve .NET Core CLI'ın dosya İzleyicisi (dotnet watch) ASP.NET Core uygulaması kullanma gösterilmektedir.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: fc08efa433f688a0b9009aed35fdee2b0c228619
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063305"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a>Bir dosya İzleyicisi'ni kullanarak ASP.NET Core uygulamaları geliştirin

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişikliği kaynak dosyaları komutu. Örneğin, derleme, test yürütme ya da dağıtımı dosya değişikliği tetikleyebilirsiniz.

Bu öğreticide iki uç nokta ile mevcut bir web API'si:, toplam ve bir ürün döndüren bir döndürür. Bu öğreticide sabit bir hata, ürün yöntemi vardır.

İndirme [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). İki projeden oluşan: *WebApp* (bir ASP.NET Core web API'si) ve *WebAppTests* (web API'si için birim testleri).

Komut kabuğu'na gidin *WebApp* klasör. Şu komutu çalıştırın:

```console
dotnet run
```

Konsol çıktısı aşağıdakine benzer iletiler gösterir (uygulama çalıştıran ve istekleri bekleyen gösterir):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`. Sonucu görmeniz gerekir `9`.

Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`). Döndürür `9`değil `20` beklediğiniz gibi. Öğreticide daha sonra bu sorun düzeltilmiştir.

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a>Ekleme `dotnet watch` projeye

`dotnet watch` Dosya İzleyici Aracı sürümüyle .NET Core SDK'sını 2.1.300 dahildir. Aşağıdaki adımlar, .NET Core SDK'ın önceki bir sürümünü kullanırken gerekli değildir.

1. Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini başvuru *.csproj* dosyası:

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

Tüm [.NET Core CLI komutunu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`. Örneğin:

| Komut | İzleme komutu |
| ---- | ----- |
| dotnet çalıştırın | dotnet watch çalıştırın |
| dotnet -f netcoreapp2.0 çalıştırın | dotnet izleyin -f netcoreapp2.0 çalıştırın |
| dotnet -f netcoreapp2.0--çalıştırma--arg1 | dotnet izleyin -f netcoreapp2.0--çalıştırma--arg1 |
| DotNet testi | DotNet izleme testi |

Çalıştırma `dotnet watch run` içinde *WebApp* klasör. Konsol çıkışını gösterir `watch` başlatıldı.

## <a name="make-changes-with-dotnet-watch"></a>İle değişiklik `dotnet watch`

Emin `dotnet watch` çalışıyor.

Hatayı düzeltmek `Product` yöntemi *MathController.cs* ürün ve toplamı döndürecek şekilde:

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

Dosyayı kaydedin. Konsol çıkışını gösterir `dotnet watch` dosya değişikliği algılandı ve uygulamayı yeniden başlatılır.

Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.

## <a name="run-tests-using-dotnet-watch"></a>Kullanarak testleri çalıştırma `dotnet watch`

1. Değişiklik `Product` yöntemi *MathController.cs* toplamını döndüren geri dönün. Dosyayı kaydedin.
1. Komut kabuğu'na gidin *WebAppTests* klasör.
1. Çalıştırma [dotnet restore](/dotnet/core/tools/dotnet-restore).
1. Çalıştırma `dotnet watch test`. Bir testin başarısız olduğunu ve İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Düzeltme `Product` yöntemi kod ürün döndürecek şekilde. Dosyayı kaydedin.

`dotnet watch` dosya değişikliği algılar ve testleri yeniden çalıştırır. Konsol çıkışını sınamalarını geçtiğini gösterir.

## <a name="customize-files-list-to-watch"></a>İzlemek için dosyaları listesini özelleştirme

Varsayılan olarak, `dotnet-watch` aşağıdaki glob desenlerle eşleşen tüm dosyaları izler:

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

Daha fazla öğe düzenleyerek izleme listesine eklenebilir *.csproj* dosya. Öğeleri tek tek veya glob desenleri kullanılarak belirtilebilir.

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a>İzlemeniz gereken dosyaların çevirme

`dotnet-watch` varsayılan ayarlarını yok saymak için yapılandırılabilir. Belirli dosyaları yoksaymak için ekleme `Watch="false"` öznitelik bir öğenin tanımına *.csproj* dosyası:

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

Hedefi, her iki proje izlemek için ise, her iki proje izleme için yapılandırılmış bir özel proje dosyası oluşturun:

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

Dosya hem projelerde izlemeye başlamak için değiştirme *test* klasör. Aşağıdaki komutu yürütün:

```console
dotnet watch msbuild /t:Test
```

Herhangi bir dosya ya da test projesinde değiştiğinde VSTest yürütür.

## <a name="dotnet-watch-in-github"></a>`dotnet-watch` Github'da

`dotnet-watch` GitHub parçasıdır [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/master/src/dotnet-watch).
