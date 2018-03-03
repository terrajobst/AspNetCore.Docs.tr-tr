---
title: "ASP.NET ASP.NET Core 2.0 geçirme"
author: isaac2004
description: "Mevcut ASP.NET MVC veya Web API uygulamaları geçirme ASP.NET Core 2.0 için yönergeler alırsınız."
manager: wpickett
ms.author: scaddie
ms.date: 08/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc2
ms.openlocfilehash: 9424234011525afdba35824b6b324f5175ba023f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>ASP.NET ASP.NET Core 2.0 geçirme

Tarafından [Isaac Levin](https://isaaclevin.com)

Bu makalede, ASP.NET Core 2.0 geçirme ASP.NET uygulamaları için bir başvuru kılavuzu olarak görev yapar.

## <a name="prerequisites"></a>Önkoşullar

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.

## <a name="target-frameworks"></a>Hedef Çerçeve
ASP.NET Core 2.0 projeleri geliştiriciler .NET Core, .NET Framework veya her ikisini hedefleme esnekliği sunar. Bkz: [sunucu uygulamaları için .NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) hangi hedef Framework'ü en uygun olduğunu belirlemek için.

.NET Framework hedeflerken projeleri tek tek NuGet paketlerini başvurmanız gerekir.

.NET Core hedefleme ASP.NET Core 2.0 sayesinde çok sayıda açık paket referanslarını ortadan kaldırmanıza olanak tanır [metapackage](xref:fundamentals/metapackage). Yükleme `Microsoft.AspNetCore.All` projenizdeki metapackage:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Metapackage kullanıldığında, metapackage başvurulan hiç paket uygulamayla dağıtılır. Bunlar performansını artırmak için önceden derlenmiş ve bu varlıkları .NET çekirdeği çalışma zamanı deposu içerir. Bkz: [ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x](xref:fundamentals/metapackage) daha fazla ayrıntı için.

## <a name="project-structure-differences"></a>Proje yapısı farklar
*.Csproj* dosya biçimi ASP.NET Core basitleştirilmiştir. Bazı önemli değişiklikler şunları içerir:
- Açık dosyaları edilmesi bunları projenin bir parçası olarak kabul edilmesi gerekli değildir. Bu büyük takımlar temelinde çalışırken XML birleştirme çakışmaları riskini azaltır.
- Hangi dosya okunabilirliğini artırır diğer projeler için GUID tabanlı başvuru vardır.
- Visual Studio'da kaldırılması olmadan dosya düzenlenebilir:

    ![Visual Studio 2017 CSPROJ bağlam menüsü seçeneğini Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Global.asax dosyası değiştirme
ASP.NET Core uygulama Önyüklemesi için yeni bir mekanizma sunar. ASP.NET uygulamaları için giriş noktası *Global.asax* dosya. Yol yapılandırması ve filtre ve alan kayıtlar gibi görevleri işlenir *Global.asax* dosya.

[!code-csharp[](samples/globalasax-sample.cs)]

Bu yaklaşım, uygulama ve sunucunun, bir uygulama ile uğratan şekilde dağıtıldığı couples. Aynı şekilde çaba içinde [OWIN](http://owin.org/) birden çok çerçeveyi birlikte kullanmak için temiz bir şekilde sağlamak için sunulmuştur. OWIN yalnızca gerekli modülleri eklemek için bir kanal sağlar. Barındırma ortamı geçen bir [başlangıç](xref:fundamentals/startup) Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırmak için işlevi. `Startup` Ara yazılım bir dizi uygulama ile kaydeder. Her istek için uygulama işleyicileri var olan bir dizi bağlantılı listesinin baş işaretçisi ile Ara yazılım bileşenlerinin her biri çağırır. Her ara yazılım bileşeni ardışık düzen işleme isteği için bir veya daha fazla işleyicileri ekleyebilirsiniz. Bu listeye yeni başı olan işleyici başvuru döndürerek gerçekleştirilir. Her işleyici anımsama ve listedeki sonraki işleyicisi çağırma sorumludur. ASP.NET Core ile bir uygulama için giriş noktasıdır `Startup`, ve bir bağımlılık artık sahip *Global.asax*. OWIN .NET Framework ile kullanırken, aşağıdakine benzer bir ardışık düzen halinde kullanın:

[!code-csharp[](samples/webapi-owin.cs)]

Bu, varsayılan yolların yapılandırır ve XmlSerialization için Json üzerinde varsayılan olarak. (Hizmetleri, yapılandırma ayarlarını, statik dosyalar, vb. yükleniyor.) gerektiği gibi diğer ara yazılımdan bu ardışık düzene ekleyin.

ASP.NET Core benzer bir yaklaşım kullanır, ancak giriş işlemek OWIN kullanmaz. Bunun yerine, aracılığıyla yapılır *Program.cs* `Main` yöntemi (konsol uygulamaları gibi) ve `Startup` orada yüklenir.

[!code-csharp[](samples/program.cs)]

`Startup` içermelidir bir `Configure` yöntemi. İçinde `Configure`, gerekli ara yazılım ardışık düzene ekleyin. (Şablondan varsayılan web sitesi) aşağıdaki örnekte, birkaç uzantı yöntemleri için destek ile ardışık düzenini yapılandırmak için kullanılır:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Hata sayfaları
* Statik dosyalar
* ASP.NET Core MVC
* Kimlik

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Ana bilgisayar ve uygulama, gelecekte farklı bir platform için taşıma esneklik sağlayan ayrılmış.

**Not:** ASP.NET Core başlatma ve ara yazılımı için daha kapsamlı bir başvuru için bkz: [ASP.NET Core başlangıç](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Depolama yapılandırmaları
ASP.NET depolama ayarlarını destekler. Bu ayar, örneğin, uygulamaları dağıtılıp dağıtılmadığını ortamını desteklemek için kullanılır. Tüm özel anahtar-değer çiftlerini depolamak için ortak bir uygulama kullanılmasıydır `<appSettings>` bölümünü *Web.config* dosyası:

[!code-xml[](samples/webconfig-sample.xml)]

Uygulamaları kullanarak bu ayarları okuma `ConfigurationManager.AppSettings` koleksiyonunda `System.Configuration` ad alanı:

[!code-csharp[](samples/read-webconfig.cs)]

ASP.NET Core, herhangi bir dosyada uygulama için yapılandırma verilerini depolamak ve bunları ara yazılım önyüklemesinden bir parçası olarak yükleyin. Proje şablonlarını kullanılan varsayılan dosyası *appsettings.json*:

[!code-json[](samples/appsettings-sample.json)]

Bu dosyayı bir örneğine yüklemesini `IConfiguration` uygulamanızı yapılır iç *haline*:

[!code-csharp[](samples/startup-builder.cs)]

Uygulama okur `Configuration` ayarları almak için:

[!code-csharp[](samples/read-appsettings.cs)]

Bu yaklaşımın işlemi kullanarak gibi daha sağlam hale uzantıları vardır [bağımlılık ekleme](xref:fundamentals/dependency-injection) bu değerleri içeren bir hizmeti yüklemek için (dı). DI yaklaşım yapılandırma nesnelerini kesin türü belirtilmiş bir dizi sağlar.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Not:** ASP.NET Core yapılandırma daha kapsamlı bir başvuru için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Yerel bağımlılık ekleme
Büyük, ölçeklendirilebilir uygulamalar oluştururken bir önemli bileşenleri ve hizmetlerin gevşek bağlantı hedeftir. [Bağımlılık ekleme](xref:fundamentals/dependency-injection) bunu elde etmek için yaygın olarak kullanılan bir tekniktir ve ASP.NET Core yerel bir bileşeni olduğu.

ASP.NET uygulamalarında geliştiriciler bağımlılık ekleme uygulamak için bir üçüncü taraf kitaplığı kullanır. Bu tür bir kitaplığıdır [Unity](https://github.com/unitycontainer/unity), Microsoft Patterns & yöntemler tarafından sağlanan. 

Bir bağımlılık ekleme Unity ile ayarlama örneği uygulama `IDependencyResolver` , saran bir `UnityContainer`:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Bir örneğini oluşturun, `UnityContainer`, hizmetiniz kaydetmek ve bağımlılık çözümleyicisini ayarlayın `HttpConfiguration` yeni örneğine `UnityResolver` , kapsayıcı için:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Eklenmeye `IProductRepository` gerektiğinde:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Bağımlılık ekleme ASP.NET Core parçası olduğundan, hizmetinizi ekleyebilirsiniz `ConfigureServices` yöntemi *haline*:

[!code-csharp[](samples/configure-services.cs)]

Depo her yerden, Unity ile doğru şekilde yerleştirilebilir.

**Not:** ASP.NET Core bağımlılık ekleme bir ayrıntılı başvuru için bkz: [ASP.NET Core bağımlılık ekleme](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Statik dosyaları sunma
Önemli, bir web geliştirme statik, istemci-tarafı varlıklar sunabilme özelliğini parçasıdır. En yaygın statik dosyaları HTML, CSS, Javascript ve görüntüleri gösterilebilir. Bu dosyalar veya uygulama (CDN) yayımlanan konumda kaydedilir ve bir istek tarafından yüklenebilmesi için başvurulan gerekir. Bu işlem, ASP.NET Core değişti.

ASP.NET, statik dosyalar çeşitli dizinlerde depolanan ve görünümlerde başvurulan.

ASP.NET çekirdek statik dosyaları "web root" depolanır (*&lt;içerik kök&gt;/wwwroot*), aksi belirtilmedikçe. Dosyalar istek ardışık düzenine çağırarak yüklenir `UseStaticFiles` uzantısı yönteminden `Startup.Configure`:

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**Not:** .NET Framework'ü hedefleme NuGet paketini yükleyin `Microsoft.AspNetCore.StaticFiles`.

Örneğin, bir görüntü varlığı *wwwroot/görüntüleri* klasördür erişilebilir bir konumda tarayıcıya gibi `http://<app>/images/<imageFileName>`.

**Not:** ASP.NET Core içinde statik dosyaları sunma daha ayrıntılı başvuru için bkz: [ASP.NET Core statik dosyaları ile çalışmaya giriş](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Ek kaynaklar

* [.NET Core kitaplıklara bağlantı noktası oluşturma](/dotnet/core/porting/libraries)
