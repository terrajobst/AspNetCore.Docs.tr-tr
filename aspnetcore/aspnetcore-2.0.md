---
title: ASP.NET Core 2. 0 ' yenilikler nelerdir?
author: rick-anderson
description: ASP.NET Core 2.0 yeni özellikler hakkında bilgi edinin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/10/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.0
ms.openlocfilehash: 0382b12b03828204c4d5c48bc9ac147ee1d7c388
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="whats-new-in-aspnet-core-20"></a>ASP.NET Core 2. 0 ' yenilikler nelerdir?

Bu makalede, ASP.NET Core 2. 0'da, en önemli değişiklikler ile ilgili belgelere bağlantılar vurgular.

## <a name="razor-pages"></a>Razor sayfalarının

Razor sayfalarının sayfa odaklı senaryoları daha kolay ve daha üretken kodlama yapar ASP.NET Core MVC yeni özelliğidir.

Daha fazla bilgi için bkz: giriş ve öğretici:

* [Razor sayfalarının giriş](xref:mvc/razor-pages/index)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>ASP.NET Core metapackage

Yeni bir ASP.NET Core metapackage yapılan ve iç ve 3. taraf bağımlılıklarını birlikte ASP.NET Core ve Entity Framework Çekirdek ekipleri tarafından desteklenen paketleri içerir. Artık tek tek ASP.NET Core paketiyle özellikleri seçmek gerekmez. Tüm özellikleri dahil edilen [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paket. Bu paket varsayılan şablonları kullanın.

Daha fazla bilgi için bkz: [Microsoft.AspNetCore.All metapackage ASP.NET Core 2.0 için](xref:fundamentals/metapackage).

## <a name="runtime-store"></a>Çalışma zamanı deposu

Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından yeni bir .NET çekirdeği çalışma zamanı deposu. Depo ASP.NET Core 2.0 uygulamaları çalıştırmak için gereken tüm çalışma zamanı varlıklarını içerir. Kullandığınızda `Microsoft.AspNetCore.All` metapackage, başvurulan ASP.NET Core NuGet paketlerini hiçbir varlıklarından, uygulama ile dağıtılır, hedef sistemde zaten bulundukları olduğundan. Çalışma zamanı deposu varlıkları, ayrıca uygulama başlangıç zamanını geliştirmek için önceden derlenmiş.

Daha fazla bilgi için bkz: [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store)

## <a name="net-standard-20"></a>.NET standart 2.0

ASP.NET Core 2.0 paketleri .NET standart 2.0 hedefleyin. Paketleri diğer .NET standart 2.0 kitaplıkları tarafından başvurulabilir ve .NET, .NET Core 2.0 ve .NET Framework 4.6.1 dahil olmak üzere, .NET standart 2.0 ile uyumlu uygulamalar üzerinde çalışabilir. 

`Microsoft.AspNetCore.All` , .NET Core 2.0 çalışma zamanı deposuyla kullanılmaya olduğundan metapackage .NET Core 2.0 yalnızca hedefler.

## <a name="configuration-update"></a>Yapılandırma güncelleştirmesi

Bir `IConfiguration` örneği, varsayılan ASP.NET Core 2.0 olarak Hizmetleri kapsayıcıya eklenir. `IConfiguration` Hizmetler kapsayıcısı kapsayıcıdan yapılandırma değerlerini almak üzere uygulamalar için kolaylaştırır.

Planlanan belgelerine durumu hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/aspnet/Docs/issues/3387).

## <a name="logging-update"></a>Güncelleştirme günlüğe kaydetme

ASP.NET Core 2. 0'günlüğe kaydetme varsayılan olarak bağımlılık ekleme (dı) sisteme eklenmiştir. Sağlayıcıları eklemek ve filtrelemeyi yapılandırmak *Program.cs* dosyası içinde yerine *haline* dosya. Ve varsayılan `ILoggerFactory` olanak sağlayan bir şekilde filtrelenmesini destekler, hem sağlayıcı çapraz filtreleme ve özel sağlayıcı filtreleme için esnek bir yaklaşım kullanın.

Daha fazla bilgi için bkz: [günlük giriş](xref:fundamentals/logging/index).

## <a name="authentication-update"></a>Kimlik doğrulama güncelleştirme

Yeni bir kimlik doğrulama modeli dı kullanarak bir uygulama kimlik doğrulamasını yapılandırmak daha kolay hale getirir.

Yeni şablonlar web uygulamaları için kimlik doğrulaması yapılandırmak için kullanılabilir ve [Azure AD B2C] kullanarak API web (https://azure.microsoft.com/services/active-directory-b2c/).

Planlanan belgelerine durumu hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/aspnet/Docs/issues/3054).

## <a name="identity-update"></a>Kimlik güncelleştirme

Bu yapı daha kolay güvenli yaptık ASP.NET Core 2. 0 ' kimliğini kullanarak API web. Web API kullanarak erişmek için erişim belirteçleri elde edebilirsiniz [Microsoft kimlik doğrulama kitaplığı (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).

2.0 kimlik doğrulama değişiklikleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Hesap doğrulama ve ASP.NET Core parola kurtarma](xref:security/authentication/accconfirm)
* [ASP.NET Core Doğrulayıcı uygulamalar için QR kodu oluşturmayı etkinleştir](xref:security/authentication/identity-enable-qrcodes)
* [Kimlik doğrulama ve kimlik ASP.NET Core 2.0 geçirme](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>SPA şablonları

Angular, Aurelia, Knockout.js, React.js ve React.js Redux ile tek sayfa uygulama (SPA) proje şablonları kullanılabilir. Açısal şablon Açısal 4'e güncelleştirilmiştir. Angular ve tepki şablonları varsayılan olarak bulunur; diğer şablon alma hakkında daha fazla bilgi için bkz: [yeni SPA projesi oluşturma](xref:client-side/spa-services#creating-a-new-project). ASP.NET Core içinde SPA oluşturma hakkında daha fazla bilgi için bkz: [tek sayfa uygulamaları oluşturma için kullanım JavaScriptServices](xref:client-side/spa-services).

## <a name="kestrel-improvements"></a>Kestrel geliştirmeleri

Kestrel web sunucusu Internet'e yönelik sunucu daha uygun sağlayan yeni özellikler vardır. Sunucu kısıtlaması yapılandırma seçenekleri sayısı olarak eklenen `KestrelServerOptions` sınıfı yeni `Limits` özelliği. Aşağıdakiler için sınırları ekleyin:

- En fazla istemci bağlantıları
- En büyük istek gövdesi boyutu
- En düşük istek gövdesi veri oranı

Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel).

## <a name="weblistener-renamed-to-httpsys"></a>WebListener HTTP.sys'ye yeniden adlandırıldı

Paketleri `Microsoft.AspNetCore.Server.WebListener` ve `Microsoft.Net.Http.Server` yeni bir pakete birleştirilmiş `Microsoft.AspNetCore.Server.HttpSys`. Ad alanları eşleşecek şekilde güncelleştirildi.

Daha fazla bilgi için bkz: [HTTP.sys web server ASP.NET Core uygulamasında](xref:fundamentals/servers/httpsys).

## <a name="enhanced-http-header-support"></a>Gelişmiş HTTP üstbilgisi desteği

MVC iletmek için kullanılırken bir `FileStreamResult` veya `FileContentResult`, şimdi ayarlama seçeneğiniz vardır bir `ETag` veya `LastModified` , iletme içerik tarihi. Bu değerleri kod ile döndürülen içeriği aşağıdakine benzer ayarlayabilirsiniz:

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

Ziyaretçileri döndürülen dosya için uygun HTTP üst bilgileri ile donatılmış `ETag` ve `LastModified` değerleri.

Bir uygulama ziyaretçi istekleri içeriği, bir aralık istek üstbilgisi ile ASP.NET, tanıyabilir ve bu başlığı tanıtıcı. İstenen içerik kısmen alınabilir, ASP.NET uygun şekilde atlayın ve bayt yalnızca istenen kümesini döndürür.  Tüm özel işleyicileri uyum veya bu özelliği işlemek için yöntemlerin içine yazma gerekmez; Bu otomatik olarak sizin için gerçekleştirilir.

## <a name="hosting-startup-and-application-insights"></a>Başlatma ve Application Insights barındırma

Barındırma ortamları artık ek paket bağımlılıkları ekleme ve kod uygulama başlatma sırasında uygulama açıkça bir bağımlılık alın veya herhangi bir yöntem çağrısı gerek yürütün. Bu özellik, uygulamanın önceden bilmenize gerek olmadan bu ortam için "ışık li" özelliklere belirli ortamlara etkinleştirmek için kullanılabilir. 

ASP.NET Core 2. 0'bu özelliği otomatik olarak (sonra seçim) ve Visual Studio'da hata ayıklama sırasında Application Insights Tanılama'yı etkinleştirmek için kullanılan Azure App Services çalıştırırken. Sonuç olarak, proje şablonları artık Application Insights paketler ve kod varsayılan olarak ekleyin.

Planlanan belgelerine durumu hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/aspnet/Docs/issues/3389).

## <a name="automatic-use-of-anti-forgery-tokens"></a>Sahteciliğe karşı koruma belirteçlerini otomatik olarak kullanılmasını

ASP.NET Core her zaman siteler arası istek sahtekarlığı (XSRF) saldırılarını HTML olarak kodlanacak içerik varsayılan olarak, ancak yardımcı olmak için fazladan bir adım geçen yeni sürümle Yardım. ASP.NET Core artık varsayılan olarak sahteciliğe karşı koruma belirteçleri yayma ve bunları form POST eylemleri ve ek yapılandırma olmaksızın sayfaları doğrulayın.

Daha fazla bilgi için bkz: [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).

## <a name="automatic-precompilation"></a>Otomatik ön derlemesi

Razor görünüm ön derleme yayımlama sırasında Yayımla çıkış boyutu ve uygulama azaltma varsayılan olarak etkin başlangıç zamanı.

Daha fazla bilgi için bkz: [Razor görünüm derleme ve ASP.NET Core içinde ön derlemesi](xref:mvc/views/view-compilation).

## <a name="razor-support-for-c-71"></a>C# 7.1 Razor desteği

Razor görüntüleme altyapısı, yeni Roslyn derleyicisi ile çalışmak için güncelleştirilmiştir. Varsayılan ifadeleri, olayla tanımlama grubu adları ve desen eşleştirme genel türlerle gibi C# 7.1 özellikleri için destek içerir. C# 7.1 projenizde kullanmak için aşağıdaki özellik proje dosyanıza ekleyin ve çözümü yeniden yükle:

```xml
<LangVersion>latest</LangVersion>
```

C# 7.1 özellikleri durumu hakkında daha fazla bilgi için bkz: [Roslyn GitHub deposuna](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).

## <a name="other-documentation-updates-for-20"></a>Diğer belge güncelleştirmeleri 2.0

* [Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için](xref:host-and-deploy/visual-studio-publish-profiles)
* [Anahtar Yönetimi](xref:security/data-protection/implementation/key-management)
* [Facebook kimlik doğrulamasını yapılandırma](xref:security/authentication/facebook-logins)
* [Twitter kimlik doğrulamasını yapılandırma](xref:security/authentication/twitter-logins)
* [Google kimlik doğrulamasını yapılandırma](xref:security/authentication/google-logins)
* [Microsoft Account kimlik doğrulamasını yapılandırma](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a>Geçiş Kılavuzu

ASP.NET Core 1.x uygulamaları ASP.NET Core 2.0 geçirme hakkında yönergeler için aşağıdaki kaynaklara bakın:

* [ASP.NET çekirdek geçirmek 1.x ASP.NET Core 2.0 için](xref:migration/1x-to-2x/index)
* [Kimlik doğrulama ve kimlik ASP.NET Core 2.0 geçirme](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>Ek Bilgiler

Değişiklikleri tam listesi için bkz: [ASP.NET Core 2.0 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.0.0).

ASP.NET Core geliştirme takımın ilerleme durumunu ve planları ile bağlanmak için ince ayar [ASP.NET topluluk Standup](https://live.asp.net/).
