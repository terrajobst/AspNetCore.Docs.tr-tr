---
title: ASP.NET Core 2.0 yenilikleri
author: rick-anderson
description: ASP.NET Core 2.0 yenilikleri hakkında bilgi edinin.
ms.author: riande
ms.date: 07/10/2017
uid: aspnetcore-2.0
ms.openlocfilehash: 07e490fd844f0f9044afce37651ef680ed0c894d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898355"
---
# <a name="whats-new-in-aspnet-core-20"></a>ASP.NET Core 2.0 yenilikleri

Bu makalede, ASP.NET Core 2. 0'da, en önemli değişiklikler ile ilgili belgelere bağlantılar vurgulanır.

## <a name="razor-pages"></a>Razor Pages

Razor sayfaları, ASP.NET Core MVC sayfası odaklı senaryolar daha kolay ve daha üretken kodlama sağlayan, yeni bir özelliktir.

Daha fazla bilgi için giriş ve öğreticiye bakın:

* [Razor Pages’e giriş](xref:razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>ASP.NET Core metapackage

Yeni bir ASP.NET Core metapackage yapılan ve ASP.NET Core ve Entity Framework Core takımlar, iç ve 3. taraf bağımlılıklarıyla tarafından desteklenen paketler içerir. Artık tek tek ASP.NET Core, paketi tarafından özellikleri seçmeniz gerekebilir. Tüm özellikler dahil [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paket. Varsayılan şablonlar bu paketi kullanın.

Daha fazla bilgi için [Microsoft.AspNetCore.All metapackage ASP.NET Core 2.0 için](xref:fundamentals/metapackage).

## <a name="runtime-store"></a>Çalışma zamanı Store

Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak yeni bir .NET Core çalışma zamanı Store avantajlarından yararlanın. Store, ASP.NET Core 2.0 uygulamaları çalıştırmak için gerekli olan tüm çalışma zamanı varlıkları içerir. Kullanırken `Microsoft.AspNetCore.All` metapackage, hiçbir varlıklarından başvurulan bir ASP.NET Core NuGet paket, uygulama ile dağıtılır, çünkü bunlar zaten hedef sistemde bulunurlar. Çalışma zamanı Store varlıkları önceden derlenmiş uygulama başlatma çıkış süresini kısaltın.

Daha fazla bilgi için [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store)

## <a name="net-standard-20"></a>.NET Standard 2.0

ASP.NET Core 2.0 paketleri .NET Standard 2.0 hedefleyin. Paketleri diğer .NET Standard 2.0 kitaplıkları tarafından başvurulabilir ve .NET, .NET Core 2.0 ve .NET Framework 4.6.1 dahil olmak üzere, .NET Standard 2.0 uyumlu uygulamalar üzerinde çalıştırabilirler. 

`Microsoft.AspNetCore.All` İle .NET Core 2.0 çalışma zamanı Store kullanılmaya yönelik olduğundan metapackage .NET Core 2.0 yalnızca hedefler.

## <a name="configuration-update"></a>Yapılandırma güncelleştirmesi

Bir `IConfiguration` örneği, varsayılan olarak ASP.NET Core 2.0 Hizmetleri kapsayıcıya eklenir. `IConfiguration` Hizmetleri kapsayıcı kapsayıcıdan yapılandırma değerlerini almak uygulamaları kolaylaştırır.

Planlanan belgeleri durumu hakkında daha fazla bilgi için bkz [GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/3387).

## <a name="logging-update"></a>Güncelleştirme günlüğü

ASP.NET Core 2.0 sürümünde, günlüğe kaydetme varsayılan olarak bağımlılık ekleme (dı) sisteme eklenmiştir. Sağlayıcıları ekleyin ve filtrelemeyi yapılandırma *Program.cs* dosyası içinde yerine *Startup.cs* dosya. Ve varsayılan `ILoggerFactory` hem sağlayıcı çapraz filtreleme ve özel sağlayıcı filtreleme için esnek bir yaklaşım sağlayan bir şekilde filtrelemeyi kullanın.

Daha fazla bilgi için [günlük giriş](xref:fundamentals/logging/index).

## <a name="authentication-update"></a>Kimlik doğrulama güncelleştirmesini

Yeni bir kimlik doğrulama modeli DI kullanarak bir uygulama için kimlik doğrulamasını yapılandırmak kolaylaştırır.

Yeni şablonlar web apps için kimlik doğrulaması yapılandırmak için kullanılabilir ve web API'leri [Azure AD B2C] kullanarak (https://azure.microsoft.com/services/active-directory-b2c/).

Planlanan belgeleri durumu hakkında daha fazla bilgi için bkz [GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/3054).

## <a name="identity-update"></a>Kimlik güncelleştirme

Oluşturmayı kolay güvenli kolaylaştırdık web API'leri kimliğini kullanarak ASP.NET Core 2. 0 '. Web API'leri kullanarak erişmek için erişim belirteçleri edinebilir [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).

Kimlik doğrulaması değişiklikleri 2.0 hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Hesap onaylama ve parola kurtarma ASP.NET Core](xref:security/authentication/accconfirm)
* [ASP.NET core'da authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme](xref:security/authentication/identity-enable-qrcodes)
* [ASP.NET Core 2.0 kimlik doğrulaması ve kimlik geçirme](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>SPA şablonları

Tek sayfa uygulama (SPA) proje şablonları Angular, Aurelia Knockout.js React.js ve Redux ile React.js için kullanılabilir. Angular şablonu Angular 4'e güncelleştirilmiştir. Angular ve React şablonları, varsayılan olarak mevcuttur; diğer şablonları alma hakkında daha fazla bilgi için bkz: [yeni bir SPA proje oluşturma](xref:client-side/spa-services#creating-a-new-project). ASP.NET core'da bir SPA oluşturma hakkında daha fazla bilgi için bkz: [oluşturma tek sayfalı uygulamalar için JavaScriptServices kullanma](xref:client-side/spa-services).

## <a name="kestrel-improvements"></a>Kestrel'i geliştirmeleri

Kestrel'i web sunucusu, Internet'e yönelik olarak daha uygun hale getiren yeni özelliklere sahiptir. İçinde sunucu kısıtlaması yapılandırma seçeneklerinin bir sayı eklenir `KestrelServerOptions` sınıf yeni `Limits` özelliği. Aşağıdaki sınırlar ekleyin:

* En fazla istemci bağlantısı
* En fazla istek gövdesi boyutu
* En az bir istek gövdesi veri hızı

Daha fazla bilgi için [Kestrel web sunucusu uygulaması ASP.NET core'da](xref:fundamentals/servers/kestrel).

## <a name="weblistener-renamed-to-httpsys"></a>WebListener HTTP.sys için yeniden adlandırıldı

Paketleri `Microsoft.AspNetCore.Server.WebListener` ve `Microsoft.Net.Http.Server` yeni bir paket birleştirilir `Microsoft.AspNetCore.Server.HttpSys`. Ad alanlarını eşleşecek şekilde güncelleştirildi.

Daha fazla bilgi için [HTTP.sys sunucu uygulamasından ASP.NET Core web](xref:fundamentals/servers/httpsys).

## <a name="enhanced-http-header-support"></a>HTTP üst bilgisi için gelişmiş destek

MVC aktarmaya kullanırken bir `FileStreamResult` veya `FileContentResult`, şimdi Ayarla seçeneğine sahip bir `ETag` veya `LastModified` , iletme içeriğe tarih. Bu değerleri üzerinde kod döndürülen içerikle aşağıdakine benzer ayarlayabilirsiniz:

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

Ziyaretçilerinizin için döndürülen dosya için uygun HTTP üst bilgileri ile donatılmış `ETag` ve `LastModified` değerleri.

Bir uygulama ziyaretçi isteği içeriği, bir aralık isteği üst bilgisi ile ASP.NET Core istek tanır ve üst bilgi olarak işler. Talep edilen içeriği kısmen edinebilir, ASP.NET Core, uygun şekilde atlar ve yalnızca istenen bayt kümesini döndürür. Herhangi bir özel işleyicileri uyarlayabilir veya bu özelliği işlemek için yöntemlerinizi yazmanız gerekmez; Bu otomatik olarak sizin yerinize gerçekleştirilir.

## <a name="hosting-startup-and-application-insights"></a>Başlangıç ve Application Insights'ı barındırma

Barındırma ortamları artık ek paket bağımlılıkları ekleme ve kod uygulama başlatma sırasında açıkça bir bağımlılık olması veya herhangi bir yöntem çağrısı gerek olmadan uygulamayı çalıştırın. Bu özellik, söz konusu ortama "ışık yukarı" özelliklere belirli ortamlara uygulamanın önceden bilmenize gerek olmadan etkinleştirmek için kullanılabilir. 

ASP.NET Core 2.0 sürümünde bu özellik otomatik olarak (sonra seçim) ve Visual Studio'da hata ayıklama sırasında Application Insights Tanılama'yı etkinleştirmek için kullanılan Azure uygulama hizmetleri çalışırken. Sonuç olarak, proje şablonları artık Application Insights paketlerini ve kod varsayılan olarak ekleyin.

Planlanan belgeleri durumu hakkında daha fazla bilgi için bkz [GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/3389).

## <a name="automatic-use-of-anti-forgery-tokens"></a>Sahteciliğe karşı koruma belirteçleri otomatik kullanımı

ASP.NET Core, siteler arası istek sahteciliği (XSRF) saldırılarını HTML olarak kodlanacak içeriği varsayılan olarak, ancak fazladan bir adım yardımcı olması için geçen yeni sürüm ile her zaman olmuştur. ASP.NET Core, artık varsayılan olarak sahteciliğe karşı koruma belirteçleri yayma ve bunları form POST eylemlerinin ve ek yapılandırma olmaksızın sayfaları doğrulayabilirsiniz.

Daha fazla bilgi için [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).

## <a name="automatic-precompilation"></a>Otomatik kod ön derlemesi

Razor görünümü önceden derleme yayımlama sırasında uygulama ve yayımlama boyutu azaltma, varsayılan olarak etkin başlangıç zamanı.

Daha fazla bilgi için [Razor görünüm derlemesi ve içinde ASP.NET Core ön derleme](xref:mvc/views/view-compilation).

## <a name="razor-support-for-c-71"></a>C# 7.1 Razor desteği

Razor görünüm altyapısı yeni Roslyn derleyicisi ile çalışacak şekilde güncelleştirildi. C# 7.1 varsayılan ifadeleri, Çıkarsanan demet adları ve türlerle desen eşleştirme gibi özellikler için destek içerir. Projenizde C# 7.1 kullanmak için proje dosyanızda aşağıdaki özelliği ekleyin ve ardından çözümü yeniden yükleyin:

```xml
<LangVersion>latest</LangVersion>
```

C# 7.1 özelliklerini durumu hakkında daha fazla bilgi için bkz. [Roslyn GitHub deposunu](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).

## <a name="other-documentation-updates-for-20"></a>2.0 için diğer belge güncelleştirmeleri

* [Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı](xref:host-and-deploy/visual-studio-publish-profiles)
* [Anahtar Yönetimi](xref:security/data-protection/implementation/key-management)
* [Facebook kimlik doğrulamasını yapılandırma](xref:security/authentication/facebook-logins)
* [Twitter kimlik doğrulamasını yapılandırma](xref:security/authentication/twitter-logins)
* [Google kimlik doğrulamasını yapılandırma](xref:security/authentication/google-logins)
* [Microsoft Account kimlik doğrulamasını yapılandırma](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a>Geçiş Kılavuzu

ASP.NET Core 1.x ASP.NET Core 2.0 uygulamaları geçirme konusunda yönergeler için aşağıdaki kaynaklara bakın:

* [ASP.NET Core geçiş 1.x ASP.NET Core 2.0](xref:migration/1x-to-2x/index)
* [ASP.NET Core 2.0 kimlik doğrulaması ve kimlik geçirme](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>Ek Bilgiler

Değişikliklerin tam listesi için bkz. [ASP.NET Core 2.0 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.0.0).

ASP.NET Core geliştirme takımın ilerleme durumu ve planları ile bağlanmak için etkinliğindeki [ASP.NET topluluğu toplantısında](https://live.asp.net/).
