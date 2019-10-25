---
title: Dosya İzleyicisi kullanarak ASP.NET Core uygulamalar geliştirme
author: rick-anderson
description: Bu öğreticide, .NET Core CLI dosya İzleyicisi (DotNet Watch) aracının bir ASP.NET Core uygulamasında nasıl yükleneceği ve kullanılacağı gösterilmektedir.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: a2a0bcdace67052975630f99aab23bbb0fd99bff
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816143"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a>Dosya İzleyicisi kullanarak ASP.NET Core uygulamalar geliştirme

By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

[DotNet Watch](https://www.nuget.org/packages/dotnet-watch) , kaynak dosyalar değiştiğinde [.NET Core CLI](/dotnet/core/tools) komutu çalıştıran bir araçtır. Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtımı tetikleyebilir.

Bu öğretici, iki uç nokta ile mevcut bir Web API 'SI kullanır: bir toplamı ve bir ürünü döndüren bir tane döndürür. Ürün yönteminde, bu öğreticide düzeltilen bir hata vardır.

[Örnek uygulamayı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)indirin. İki projeden oluşur: *WebApp* (bir ASP.NET Core Web API 'si) ve *webapptests* (Web API 'si için birim testleri).

Bir komut kabuğu 'nda *WebApp* klasörüne gidin. Şu komutu çalıştırın:

```dotnetcli
dotnet run
```

> [!NOTE]
> Çalıştırmak için bir proje belirtmek üzere `dotnet run --project <PROJECT>` kullanabilirsiniz. Örneğin, örnek uygulamanın kökünden `dotnet run --project WebApp` çalıştırmak *WebApp* projesini de çalıştırır.

Konsol çıktısı aşağıdakine benzer iletileri gösterir (uygulamanın çalıştığını ve istekleri beklediğini gösterir):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Bir Web tarayıcısında `http://localhost:<port number>/api/math/sum?a=4&b=5`' a gidin. `9`sonucunu görmeniz gerekir.

Ürün API 'sine gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`). Beklendiğinde `20` değil `9`döndürür. Bu sorun öğreticide daha sonra düzeltildi.

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a>Projeye `dotnet watch` ekleme

`dotnet watch` dosya İzleyici Aracı, .NET Core SDK sürüm 2.1.300 eklenir. .NET Core SDK önceki bir sürümü kullanılırken aşağıdaki adımlar gereklidir.

1. *. Csproj* dosyasına bir `Microsoft.DotNet.Watcher.Tools` paket başvurusu ekleyin:

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. Aşağıdaki komutu çalıştırarak `Microsoft.DotNet.Watcher.Tools` paketini yüklersiniz:

    ```dotnetcli
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a>`dotnet watch` kullanarak .NET Core CLI komutları çalıştırma

Tüm [.NET Core CLI komutları](/dotnet/core/tools#cli-commands) `dotnet watch`çalıştırılabilir. Örneğin:

| Komut | İzle komutu |
| ---- | ----- |
| dotnet run | DotNet izleme çalıştırması |
| DotNet Run-f netcoreapp 2.0 | DotNet Watch çalıştırması-f netcoreapp 2.0 |
| DotNet Run-f netcoreapp 2.0----arg1 | DotNet Watch-f netcoreapp 2.0----arg1 |
| dotnet test | DotNet izleme testi |

*WebApp* klasöründe `dotnet watch run` çalıştırın. Konsol çıktısı `watch` başlatıldığını gösterir.

> [!NOTE]
> İzlenecek projeyi belirtmek için `dotnet watch --project <PROJECT>` kullanabilirsiniz. Örneğin, örnek uygulamanın kökünden `dotnet watch --project WebApp run` çalıştırmak Ayrıca *WebApp* projesini de çalıştırır ve bu uygulamayı izleyebilir.

## <a name="make-changes-with-dotnet-watch"></a>`dotnet watch` değişiklik yap

`dotnet watch` çalıştığından emin olun.

*MathController.cs* `Product` yöntemindeki hatayı düzeltir, bu nedenle ürünü döndürür ve toplamı değil:

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

Dosyayı kaydedin. Konsol çıktısı, `dotnet watch` bir dosya değişikliği algıladığını ve uygulamayı yeniden başlatıldığını gösterir.

Verify `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucu döndürür.

## <a name="run-tests-using-dotnet-watch"></a>`dotnet watch` kullanarak testleri çalıştırma

1. *MathController.cs* `Product` yöntemini geri döndürmek için yeniden değiştirin. Dosyayı kaydedin.
1. Bir komut kabuğunda *Webapptests* klasörüne gidin.
1. [DotNet restore](/dotnet/core/tools/dotnet-restore)çalıştırın.
1. `dotnet watch test`'i çalıştırın. Çıktısı bir testin başarısız olduğunu ve izleyicinin dosya değişikliklerini beklediğini gösterir:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. `Product` yöntemi kodunu, ürünü geri döndürdüğünden düzeltir. Dosyayı kaydedin.

`dotnet watch` dosya değişikliğini algılar ve testleri yeniden çalıştırır. Konsol çıktısı, geçirilen testleri belirtir.

## <a name="customize-files-list-to-watch"></a>Dosya listesini izlemek için Özelleştir

Varsayılan olarak, `dotnet-watch` aşağıdaki glob desenleriyle eşleşen tüm dosyaları izler:

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

*. Csproj* dosyası düzenlenerek, izleme listesine daha fazla öğe eklenebilir. Öğeler tek tek veya glob desenleri kullanılarak belirtilebilir.

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a>İzlenen dosyaları kabul etme

`dotnet-watch`, varsayılan ayarlarını yoksayacak şekilde yapılandırılabilir. Belirli dosyaları yoksaymak için `Watch="false"` özniteliğini *. csproj* dosyasındaki bir öğenin tanımına ekleyin:

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

## <a name="custom-watch-projects"></a>Özel izleme projeleri

`dotnet-watch` C# projelerle sınırlı değildir. Özel izleme projeleri, farklı senaryoları işlemek için oluşturulabilir. Aşağıdaki proje mizanpajını göz önünde bulundurun:

* **sınamanız**
  * *UnitTests/UnitTests. csproj*
  * *Integrationtests/ıntegrationtests. csproj*

Hedef her iki projeyi de seyretmek istiyorsanız, her iki projeyi de izlemek için yapılandırılmış özel bir proje dosyası oluşturun:

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

Her iki projede de dosya izlemeye başlamak için, *Test* klasörüne geçin. Aşağıdaki komutu yürütün:

```dotnetcli
dotnet watch msbuild /t:Test
```

VSTest, her iki test projesinde herhangi bir dosya değiştiğinde yürütülür.

## <a name="dotnet-watch-in-github"></a>GitHub 'da `dotnet-watch`

`dotnet-watch` GitHub [ASPNET/AspNetCore deposunun](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch)bir parçasıdır.
