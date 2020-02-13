---
title: ASP.NET 'den ASP.NET Core 2,0 'ye geçiş
author: isaac2004
description: Mevcut ASP.NET MVC veya Web API uygulamalarını ASP.NET Core 2,0 ' ye geçirmeye yönelik rehberlik alın.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/mvc2
ms.openlocfilehash: 11bd3b948afaedc675ac4249099969382683f653
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172448"
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a>ASP.NET 'den ASP.NET Core 2,0 'ye geçiş

İle [Isaac Levi](https://isaaclevin.com) tarafından

Bu makale, ASP.NET uygulamalarını ASP.NET Core 2,0 ' ye geçirmeye yönelik bir başvuru kılavuzu görevi görür.

## <a name="prerequisites"></a>Önkoşullar

.Net downloads 'lerden **aşağıdakilerden birini** yükler [: Windows](https://www.microsoft.com/net/download/windows):

* .NET Core SDK
* Windows için Visual Studio
  * **ASP.net ve Web geliştirme** iş yükü
  * **.NET Core platformlar arası geliştirme** iş yükü

## <a name="target-frameworks"></a>Hedef çerçeveler

ASP.NET Core 2,0 projeleri, geliştiricilere .NET Core, .NET Framework veya her ikisinin de hedeflenme esnekliği sunar. En uygun hedef Framework 'ü belirlemek için, bkz. [sunucu uygulamaları için .NET Core ve .NET Framework arasından seçim yapma](/dotnet/standard/choosing-core-framework-server) .

.NET Framework hedeflenirken, projelerin ayrı NuGet paketlerine başvurması gerekir.

.NET Core 'u hedeflemek, ASP.NET Core 2,0 [metapackage](xref:fundamentals/metapackage)sayesinde çok sayıda açık paket başvurularını ortadan kaldırmanıza olanak tanır. `Microsoft.AspNetCore.All` metapackage 'i projenize yükler:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
</ItemGroup>
```

Metapackage kullanıldığında, metapackage içinde başvurulan hiçbir paket uygulamayla birlikte dağıtılır. .NET Core çalışma zamanı deposu bu varlıkları içerir ve performansı artırmak için önceden ön derlenmiş hale getiriyoruz. Daha fazla ayrıntı için bkz. <xref:fundamentals/metapackage>.

## <a name="project-structure-differences"></a>Proje yapısı farkları

*. Csproj* dosya biçimi ASP.NET Core basitleştirilmiştir. Bazı önemli değişiklikler şunları içerir:

* Dosyaların açıkça eklenmesi, projenin bir parçası olarak kabul edilmesi için gerekli değildir. Bu, büyük ekipler üzerinde çalışırken XML birleştirme çakışmalarının riskini azaltır.
* Diğer projelere GUID tabanlı başvurular yoktur ve bu da dosya okunabilirliğini geliştirir.
* Dosya Visual Studio 'da kaldırmadan düzenlenebilir:

  ![Visual Studio 2017 ' de CSPROJ bağlam menüsü seçeneğini Düzenle](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Global. asax dosyası değiştirme

ASP.NET Core, bir uygulamayı önyüklemeden yeni bir mekanizma getirmiştir. ASP.NET uygulamaları için giriş noktası *Global. asax* dosyasıdır. Rota yapılandırması ve filtre ve alan kayıtları gibi görevler, *Global. asax* dosyasında işlenir.

[!code-csharp[](samples/globalasax-sample.cs)]

Bu yaklaşım, uygulamayı ve dağıtıldığı sunucuyu uygulamayı kesintiye uğratan bir şekilde bağar. Bağımsız olarak, [Owin](https://owin.org/) , birden çok çerçeveyi birlikte kullanmanın bir temizleyici yolunu sağlamak için sunulmuştur. OWIN yalnızca gereken modülleri eklemek için bir işlem hattı sağlar. Barındırma ortamı, hizmetleri ve uygulamanın istek ardışık düzenini yapılandırmak için bir [Başlangıç](xref:fundamentals/startup) işlevi alır. `Startup` uygulamayla bir ara yazılım kümesini kaydeder. Her istek için, uygulama bir ara yazılım bileşeninin her birini bağlantılı listenin baş işaretçisi ile mevcut bir işleyici kümesine çağırır. Her bir ara yazılım bileşeni, istek işleme ardışık düzenine bir veya daha fazla işleyici ekleyebilir. Bu, listenin yeni başlığı olan işleyiciye bir başvuru döndürülerek gerçekleştirilir. Her işleyici, listedeki bir sonraki işleyiciyi hatırlayıp çağırmaktan sorumludur. ASP.NET Core, bir uygulamaya giriş noktası `Startup`ve artık *Global. asax*' a bağımlılığı yoktur. .NET Framework ile OWIN kullanırken, işlem hattı olarak aşağıdaki gibi bir şey kullanın:

[!code-csharp[](samples/webapi-owin.cs)]

Bu, varsayılan rotalarınızı yapılandırır ve varsayılan olarak JSON üzerinden XmlSerialization olur. Gerektiğinde bu işlem hattına başka bir ara yazılım ekleyin (Yükleme Hizmetleri, yapılandırma ayarları, statik dosyalar vb.).

ASP.NET Core benzer bir yaklaşım kullanır, ancak girişi işlemek için OWıN 'u kullanmaz. Bunun yerine, *Program.cs* `Main` yöntemi aracılığıyla yapılır (konsol uygulamalarına benzer) ve `Startup` bu şekilde yüklenir.

[!code-csharp[](samples/program.cs)]

`Startup` bir `Configure` yöntemi içermelidir. `Configure`, işlem hattına gerekli bir ara yazılım ekleyin. Aşağıdaki örnekte (varsayılan Web sitesi şablonundan), işlem hattını desteğiyle yapılandırmak için birkaç uzantı yöntemi kullanılır:

* [BrowserLink](https://vswebessentials.com/features/browserlink)
* Hata sayfaları
* Statik dosyalar
* ASP.NET Core MVC
* Kimlik

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Ana bilgisayar ve uygulama, gelecekte farklı bir platforma geçme esnekliği sağlayan bir şekilde ayrılmış.

ASP.NET Core başlatmaya ve ara yazılıma yönelik daha ayrıntılı bir başvuru için bkz. <xref:fundamentals/startup>.

## <a name="storing-configurations"></a>Yapılandırma depolama

ASP.NET, ayarları depolamayı destekler. Bu ayar, örneğin, uygulamaların dağıtıldığı ortamı desteklemek için kullanılır. Genel bir uygulama, *Web. config* dosyasının `<appSettings>` bölümünde tüm özel anahtar-değer çiftlerini depolandı:

[!code-xml[](samples/webconfig-sample.xml)]

Uygulamalar, `System.Configuration` ad alanındaki `ConfigurationManager.AppSettings` koleksiyonunu kullanarak bu ayarları okur:

[!code-csharp[](samples/read-webconfig.cs)]

ASP.NET Core, uygulama için yapılandırma verilerini herhangi bir dosyada depolayıp ara yazılım önyükleme 'nin bir parçası olarak yükleyebilir. Proje şablonlarında kullanılan varsayılan dosya *appSettings. JSON*' dır:

[!code-json[](samples/appsettings-sample.json)]

Bu dosyayı uygulamanızın içindeki bir `IConfiguration` örneğine yüklemek *Startup.cs*içinde yapılır:

[!code-csharp[](samples/startup-builder.cs)]

Uygulama, ayarları almak için `Configuration` 'dan okur:

[!code-csharp[](samples/read-appsettings.cs)]

Bu şekilde, bu değerlere sahip bir hizmeti yüklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) kullanma gibi işlemleri daha sağlam hale getirmek için bu yaklaşımın uzantıları vardır. Dı yaklaşımı, kesin türü belirtilmiş bir yapılandırma nesneleri kümesi sağlar.

```csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
```

**Note:** ASP.NET Core yapılandırmaya yönelik daha ayrıntılı bir başvuru için bkz. <xref:fundamentals/configuration/index>.

## <a name="native-dependency-injection"></a>Yerel bağımlılığı ekleme

Büyük, ölçeklenebilir uygulamalar, bileşenlerin ve hizmetlerin gevşek bağlantısı olan önemli bir hedef. [Bağımlılık ekleme](xref:fundamentals/dependency-injection) , bunu elde etmek için popüler bir tekniktir ve ASP.NET Core yerel bir bileşenidir.

ASP.NET uygulamalarında, geliştiriciler bağımlılık ekleme işlemini uygulamak için bir üçüncü taraf kitaplığı kullanır. Bu tür bir kitaplık [, Microsoft](https://github.com/unitycontainer/unity)düzenleri & uygulamalar tarafından sağlanır.

Unity ile bağımlılık ekleme ayarlamayı bir örnek, bir `UnityContainer`sarmalayan `IDependencyResolver` uygulamadır:

[!code-csharp[](samples/sample8.cs)]

`UnityContainer`bir örneğini oluşturun, hizmetinizi kaydedin ve `HttpConfiguration` bağımlılık çözümleyici 'yi Kapsayıcınız için yeni `UnityResolver` örneğine ayarlayın:

[!code-csharp[](samples/sample9.cs)]

Gerektiğinde `IProductRepository` ekle:

[!code-csharp[](samples/sample5.cs)]

Bağımlılık ekleme ASP.NET Core bir parçası olduğu için hizmetinizi `Startup.ConfigureServices`ekleyebilirsiniz:

[!code-csharp[](samples/configure-services.cs)]

Depo, Unity ile doğru olduğu için her yerde eklenebilir.

ASP.NET Core bağımlılık ekleme hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="serving-static-files"></a>Statik dosyaları sunma

Web geliştirmenin önemli bir bölümü, statik, istemci tarafı varlıkları sunma olanağıdır. Statik dosyaların en yaygın örnekleri HTML, CSS, JavaScript ve görüntülerdir. Bu dosyaların, uygulamanın yayınlanan konumuna (veya CDN) kaydedilmesi gerekir ve bu dosyalar bir istek tarafından yüklenebilmeleri için başvurulmalıdır. Bu işlem ASP.NET Core değişti.

ASP.NET ' de statik dosyalar çeşitli dizinlerde depolanır ve görünümlerde başvurulur.

ASP.NET Core, aksi belirtilmedikçe statik dosyalar "Web root" ( *&lt;içerik kökü&gt;/Wwwroot*) içinde depolanır. Dosyalar, `Startup.Configure``UseStaticFiles` uzantısı yöntemi çağrılarak istek ardışık düzenine yüklenir:

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**Note:** .NET Framework hedefliyorsanız, NuGet paketini `Microsoft.AspNetCore.StaticFiles`yüklemelisiniz.

Örneğin, *Wwwroot/görüntüler* klasöründeki bir görüntü varlığına, `http://<app>/images/<imageFileName>`gibi bir konumda tarayıcı tarafından erişilebilir.

**Note:** ASP.NET Core içinde statik dosyalar sunma hakkında daha ayrıntılı bir başvuru için bkz. <xref:fundamentals/static-files>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kitaplıkları .NET Core 'a taşıma](/dotnet/core/porting/libraries)
