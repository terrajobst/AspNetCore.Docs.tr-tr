---
title: Web API Çözümleyicileri kullanma
author: pranavkm
description: ASP.NET Core MVC web API Çözümleyicileri paketi hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/05/2019
uid: web-api/advanced/analyzers
ms.openlocfilehash: 1568eb0304a58758caa5f82249dc42872f5c36b9
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384861"
---
# <a name="use-web-api-analyzers"></a>Web API Çözümleyicileri kullanma

ASP.NET Core 2,2 ve üzeri, Web API projeleriyle kullanılması amaçlanan bir MVC Çözümleyicileri paketi sağlar. Çözümleyiciler, [Web API kuralları](xref:web-api/advanced/conventions)üzerinde oluşturma sırasında <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>ile açıklanmış olan denetleyicilerle birlikte çalışır.

Çözümleyiciler paketi şu şekilde bir denetleyici eylemi bildirir:

* Bildirilmemiş bir durum kodu döndürür.
* Bildirilmemiş başarı sonucunu döndürür.
* Döndürülen bir durum kodunu belgeler.
* Açık model doğrulama denetimi içerir.

::: moniker range=">= aspnetcore-3.0"

## <a name="reference-the-analyzer-package"></a>Çözümleyici paketine başvur

ASP.NET Core 3,0 veya sonraki sürümlerde, çözümleyiciler .NET Core SDK dahil edilmiştir. Projenizdeki çözümleyici 'yi etkinleştirmek için, proje dosyasına `IncludeOpenAPIAnalyzers` özelliği ekleyin:

```xml
<PropertyGroup>
 <IncludeOpenAPIAnalyzers>true</IncludeOpenAPIAnalyzers>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

## <a name="package-installation"></a>Paket yüklemesi

Aşağıdaki yaklaşımlardan biriyle [Microsoft. AspNetCore. Mvc. api. çözümleyiciler](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet paketini yükler:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**Paket Yöneticisi konsol** penceresinde:
  * > **Diğer** Windows paket> **Yöneticisi konsolunu**görüntüle ' ye gidin.
  * *Apiconventions. csproj* dosyasının bulunduğu dizine gidin.
  * Aşağıdaki komutu yürütün:

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

* **Çözüm bölmesi** paket> **Ekle...** ' da paketler klasörüne sağ tıklayın.
* **Paket Ekle** penceresinin **kaynak** açılan penceresini "NuGet.org" olarak ayarlayın.
* Arama kutusuna "Microsoft. AspNetCore. Mvc. api. çözümleyiciler" yazın.
* Sonuçlar bölmesinden "Microsoft. AspNetCore. Mvc. api. çözümleyiciler" paketini seçin ve **paket Ekle**' ye tıklayın.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**Tümleşik terminalden**aşağıdaki komutu çalıştırın:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Şu komutu çalıştırın:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

::: moniker-end

## <a name="analyzers-for-web-api-conventions"></a>Web API kuralları için çözümleyiciler

Openapı belgeleri, bir eylemin döndürebildiği durum kodlarını ve yanıt türlerini içerir. ASP.NET Core MVC 'de, <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> ve <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> gibi öznitelikler bir eylemi belgelemek için kullanılır. <xref:tutorials/web-api-help-pages-using-swagger>Web API 'nizi belgeleme hakkında daha fazla ayrıntıya gider.

Paketteki çözümleyicilerden biri ile <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> açıklanmış denetimleri inceler ve yanıtlarını tamamen belgemeyen eylemleri tanımlar. Aşağıdaki örnek göz önünde bulundurun:

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=10)]

Yukarıdaki eylem HTTP 200 başarılı dönüş türünü belgeler, ancak HTTP 404 hata durumu kodunu belgeetmez. Çözümleyici, HTTP 404 durum kodu için eksik belgeleri uyarı olarak bildirir. Sorunu gidermeye yönelik bir seçenek sağlanır.

![Çözümleyici bir uyarı bildiriyor](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
