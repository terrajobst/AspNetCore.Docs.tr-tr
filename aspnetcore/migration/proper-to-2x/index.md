---
title: ASP.NET'ten ASP.NET Core geçişi
author: isaac2004
description: ASP.NET Core.web için geçirme mevcut ASP.NET MVC veya Web API uygulamaları için yol gösteren yönergeler alır
ms.author: scaddie
ms.date: 08/27/2017
uid: migration/proper-to-2x/index
ms.openlocfilehash: 2f42ca6f9da8d9941e5bab40afc36c95360c3550
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342191"
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a>ASP.NET'ten ASP.NET Core geçişi

Tarafından [Isaac Levin](https://isaaclevin.com)

Bu makalede, ASP.NET Core geçirme ASP.NET uygulamaları için bir başvuru kılavuzu olarak görev yapar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

## <a name="target-frameworks"></a>Hedef Çerçeve

ASP.NET Core projeleri, geliştiricilerin .NET Core, .NET Framework veya her ikisi de hedefleme esnekliği sunar. Bkz: [sunucu uygulamaları için .NET Core ve .NET Framework arasında seçim](/dotnet/standard/choosing-core-framework-server) hangi hedef çerçeveden en uygun olduğunu belirlemek için.

.NET Framework'ü hedefleyen, projeler tek NuGet paketlerini başvurmanız gerekir.

.NET Core'u hedefleyen ASP.NET Core sayesinde çok sayıda açık paket başvuruları ortadan olanak tanır [metapackage](xref:fundamentals/metapackage). Yükleme `Microsoft.AspNetCore.All` metapackage projenizdeki:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Metapackage kullanıldığında metapackage içinde başvurulan paket uygulamayla birlikte dağıtılır. .NET Core çalışma zamanı Store bu varlıkları içerir ve bunlar performansını artırmak için önceden derlenmiş. Bkz: [Microsoft.AspNetCore.All metapackage ASP.NET Core 2.x](xref:fundamentals/metapackage) daha fazla ayrıntı için.

## <a name="project-structure-differences"></a>Proje yapısına

*.Csproj* dosya biçimi, ASP.NET Core basitleştirilmiştir. Bazı önemli değişiklikler şunları içerir:

- Açık içerme dosyaları projenin bir parçası olarak değerlendirilmesi için gerekli değildir. Büyük takımlar üzerinde çalışırken bu XML birleştirme çakışmalarını riskini azaltır.
- Hangi dosya okunabilirliği iyileştirilebileceği diğer projelere GUID tabanlı başvuru vardır.
- Dosya, Visual Studio'da kaldırmadan düzenlenebilir:

    ![Visual Studio 2017'de CSPROJ bağlam menüsü seçeneği Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Global.asax dosyası değiştirme

ASP.NET Core önyükleme bir uygulama için yeni bir mekanizma sunmuştur. ASP.NET uygulamaları için giriş noktası *Global.asax* dosya. Yol yapılandırması ve filtre ve alan kayıtları gibi görevleri işlenir *Global.asax* dosya.

[!code-csharp[](samples/globalasax-sample.cs)]

Bu yaklaşım couples: uygulama ve sunucu, bir uygulama ile uğratan şekilde dağıtılır. İçinde ayırmak için çaba [OWIN](http://owin.org/) birden çok çerçeveyi birlikte kullanmak için temiz bir yol sağlamak üzere kullanılmıştır. OWIN yalnızca gerekli modül eklemek için bir işlem hattı sağlar. Barındırma ortamı alır bir [başlangıç](xref:fundamentals/startup) hizmetler ve uygulamanın istek ardışık düzenini yapılandırmak için işlevi. `Startup` Ara yazılım bir dizi uygulama ile kaydeder. Her istek için uygulamanın baş işleyicileri var olan bir dizi bağlı bir liste işaretçisi ile Ara yazılım bileşenlerinin her biri çağırır. Her bir ara yazılım bileşeni ardışık düzen işleme isteği için bir veya daha fazla işleyicileri ekleyebilirsiniz. Bu, listenin başındaki yeni işleyici için bir başvuru döndürerek gerçekleştirilir. Her işleyici anımsanmasını ve listedeki sonraki işleyicisini çağırarak sorumludur. ASP.NET Core ile bir uygulama için giriş noktasıdır `Startup`, ve bir bağımlılık artık sahip *Global.asax*. OWIN .NET Framework ile kullanırken, şunun gibi bir işlem hattı kullanın:

[!code-csharp[](samples/webapi-owin.cs)]

Bu, varsayılan yollarını yapılandırır ve XmlSerialization için varsayılan olarak Json. (Hizmetler, yapılandırma ayarlarını, statik dosyalar, vb. yükleniyor.) gerektiği gibi diğer ara yazılımdan bu ardışık düzenine ekleyin.

ASP.NET Core, benzer bir yaklaşım kullanır, ancak giriş işlemek için OWIN üzerinde içermez. Bunun yerine, aracılığıyla gerçekleştirilir *Program.cs* `Main` yöntemi (konsol uygulamaları için benzer şekilde) ve `Startup` orada yüklenir.

[!code-csharp[](samples/program.cs)]

`Startup` içermelidir bir `Configure` yöntemi. İçinde `Configure`, gerekli bir ara yazılım ardışık düzenine ekleyin. (Şablondan varsayılan web sitesi) aşağıdaki örnekte, birkaç genişletme yöntemleri için destek ile işlem hattını yapılandırmak için kullanılır:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Hata sayfaları
* Statik dosyalar
* ASP.NET Core MVC
* Kimlik

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Konak ve uygulama, gelecekte farklı bir platform için taşıma esnekliği sağlayan ayrılmış.

> [!NOTE]
> ASP.NET Core başlangıç ve ara yazılım için daha ayrıntılı bir başvuru için bkz: [ASP.NET Core başlangıç](xref:fundamentals/startup)

## <a name="store-configurations"></a>Store yapılandırmaları

ASP.NET depolama ayarlarını destekler. Bu ayar, örneğin, uygulama dağıtılıp dağıtılmadığını ortamı desteklemek için kullanılır. Tüm özel anahtar-değer çiftlerini depolamak için ortak bir uygulama olan `<appSettings>` bölümünü *Web.config* dosyası:

[!code-xml[](samples/webconfig-sample.xml)]

Uygulamaları kullanarak bu ayarları okuma `ConfigurationManager.AppSettings` koleksiyonda `System.Configuration` ad alanı:

[!code-csharp[](samples/read-webconfig.cs)]

ASP.NET Core uygulaması için yapılandırma verilerini herhangi bir dosyayı depolayabilir ve ara yazılımı önyüklemesi bir parçası olarak yükleyin. Proje şablonlarında kullanılan varsayılan dosya *appsettings.json*:

[!code-json[](samples/appsettings-sample.json)]

Bu dosya bir örneğine yüklenirken `IConfiguration` uygulamanızı yapılır iç *Startup.cs*:

[!code-csharp[](samples/startup-builder.cs)]

Uygulama okur `Configuration` ayarları almak için:

[!code-csharp[](samples/read-appsettings.cs)]

Uzantı işlemi kullanma gibi daha sağlam hale getirmek için bu yaklaşımın [bağımlılık ekleme](xref:fundamentals/dependency-injection) bu değerlere sahip bir hizmet yüklenemedi (dı). Yapılandırma nesneleri türü kesin belirlenmiş bir dizi DI yaklaşımdır.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

> [!NOTE]
> ASP.NET Core yapılandırma daha ayrıntılı bir başvuru için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Yerel bağımlılık ekleme

Büyük, ölçeklenebilir uygulamalar oluştururken bir önemli bileşenlerin ve hizmetlerin gevşek eşleştirme hedeftir. [Bağımlılık ekleme](xref:fundamentals/dependency-injection) bunu elde etmek için yaygın olarak kullanılan bir tekniktir ve onu bir ASP.NET Core, yerel bileşenidir.

ASP.NET uygulamalarında bağımlılık ekleme uygulamak için bir üçüncü taraf kitaplığı geliştiriciler kullanır. Bir tür kitaplığı [Unity](https://github.com/unitycontainer/unity), Microsoft Patterns ve uygulamalar tarafından sağlanan.

Unity ile bağımlılık ekleme ayarlama örneği uygulama `IDependencyResolver` sonuna geldik bir `UnityContainer`:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Öğesinin bir örneğini oluşturur, `UnityContainer`, hizmetinizi kaydedin ve bağımlılık çözümleyicisini ayarlamak `HttpConfiguration` için yeni bir örneğini `UnityResolver` kapsayıcınız için:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Ekleme `IProductRepository` gerektiğinde:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Bağımlılık ekleme ASP.NET Core parçası olduğundan, hizmetinizde ekleyebilirsiniz `ConfigureServices` yöntemi *Startup.cs*:

[!code-csharp[](samples/configure-services.cs)]

Depoya her yerden, Unity ile doğru şekilde yerleştirilebilir.

> [!NOTE]
> Bağımlılık ekleme hakkında daha fazla bilgi için bkz. [bağımlılık ekleme](xref:fundamentals/dependency-injection).

## <a name="serve-static-files"></a>Statik dosyaları işleme

Web geliştirme önemli bir parçası, statik, istemci tarafı varlıklar sunabilme özelliğini ' dir. En yaygın statik dosyalar HTML, CSS, Javascript ve görüntüleri örnekleridir. Bu dosyaları ve uygulama (CDN) yayımlanan konumda kaydedilebilir ve bir istek tarafından yüklenebilmesi için başvurulan gerekir. Bu işlem, ASP.NET Core değişti.

ASP.NET'te, statik dosyalar çeşitli dizinlerinde depolanan ve görünümleri başvuru.

ASP.NET Core, statik dosyalar "web root" depolanır (*&lt;içerik kök&gt;/wwwroot*), aksi şekilde yapılandırılmadıkça. İstek ardışık düzende çağırarak dosyalar yüklenir `UseStaticFiles` şuradan genişleme metodu `Startup.Configure`:

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> .NET Framework'ü hedefleyen, NuGet paketini yüklemek `Microsoft.AspNetCore.StaticFiles`.

Örneğin, bir görüntü varlığı *wwwroot/görüntülerinden* klasörünün erişilebilir bir konumda tarayıcıya gibi `http://<app>/images/<imageFileName>`.

> [!NOTE]
> ASP.NET core'da statik dosyaları sunma daha ayrıntılı başvuru için bkz: [statik dosyalar](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Ek kaynaklar

- [.NET Core kitaplıklarını taşıma](/dotnet/core/porting/libraries)
