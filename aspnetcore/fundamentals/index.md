---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamaları oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/index
ms.openlocfilehash: a1fed574db0baab391ebb9cfc44664ceddbfa69b
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809295"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core temelleri

Bu makalede, ASP.NET Core uygulamaları geliştirmek nasıl anlamaya yönelik önemli konular bir genel bakıştır.

## <a name="the-startup-class"></a>Başlangıç sınıfı

`Startup` Sınıftır yeri:

* Uygulama tarafından gereken diğer hizmetler yapılandırılır.
* Ardışık Düzen işleme isteği tanımlanır.

* Kod yapılandırmak için (veya *kaydetme*) Hizmetleri eklenir `Startup.ConfigureServices` yöntemi. *Hizmetleri* uygulama tarafından kullanılan bileşenlerdir. Örneğin, bir Entity Framework Core bağlam nesnesi bir hizmettir.
* Ardışık Düzen işleme isteği yapılandırmak için kod eklenir `Startup.Configure` yöntemi. İşlem hattı, bir dizi olarak oluşan *ara yazılım* bileşenleri. Örneğin, bir ara yazılım bir statik dosyaları için istekleri işleyecek veya HTTP isteklerini HTTPS'ye yönlendiriyor. Zaman uyumsuz işlemler gerçekleştirir her bir ara yazılım bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.

Bir örneği aşağıdadır `Startup` sınıfı:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

## <a name="dependency-injection-services"></a>Bağımlılık ekleme (hizmetler)

ASP.NET Core yerleşik bağımlılık ekleme (dı) framework uygulamanın sınıfları için kullanılabilir yapar yapılandırılmış hizmet vardır. Bir sınıf içinde bir hizmet örneği yapmanın bir yolu, gerekli türünde bir parametre ile bir oluşturucu oluşturmaktır. Parametresi, hizmet türü veya arabirim olabilir. DI sistemi hizmeti zamanında sağlar.

Bir Entity Framework Core bağlam nesnesini almak için DI kullanan bir sınıf aşağıda verilmiştir. Vurgulanan satırı Oluşturucu ekleme örneği verilmiştir:

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

DI oluşturulmuş olsa da tercih ederseniz üçüncü taraf bir denetimi tersine çevirme (IOC) kapsayıcı takın izin verecek şekilde tasarlanmıştır.

Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Ara yazılım

Ardışık Düzen işleme isteği ara yazılımı bileşenleri bir dizi olarak oluşur. Zaman uyumsuz işlemler gerçekleştirir her bileşen bir `HttpContext` ve ardışık düzende sonraki ara yazılımı çağırır veya istek sonlandırır.

Kural gereği, harekete geçirerek bir ara yazılım bileşeni ardışık düzenine eklenen kendi `Use...` uzantı yönteminde `Startup.Configure` yöntemi. Örneğin, statik dosyaları işleme olanağı çağrı `UseStaticFiles`.

Aşağıdaki örnekte vurgulanmış kodu ardışık düzen işleme isteği yapılandırır:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

ASP.NET Core içeren zengin bir yerleşik ara yazılım ve özel bir ara yazılım yazabilirsiniz.

Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

<a id="host"/>

## <a name="the-host"></a>Ana bilgisayar

ASP.NET Core uygulaması derleme bir *konak* başlangıç. Ana bilgisayar gibi tüm uygulamanın kaynakları yalıtan bir nesne şöyledir:

* Bir HTTP sunucusu uygulamasını
* Ara yazılım bileşenleri
* Günlüğe Kaydetme
* DI
* Yapılandırma

Tüm bağımlı kaynakları uygulamanın bir nesnesinde de dahil olmak üzere ana nedeni ömrü yönetimi,: uygulama başlatma ve normal şekilde kapatılmasını üzerinde denetim.

Bir ana bilgisayar oluşturmak için kod `Program.Main` ve izleyen [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern). Konak bir parçası olan her bir kaynak yapılandırmak için yöntem çağrılır. Bu istek için bir oluşturucu yöntemi çağrılır hep birlikte ve konak nesnesi örneği oluşturun.

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 3.0 veya üstü, genel ana bilgisayar (`Host` sınıfı) veya Web ana bilgisayarı (`WebHost` sınıfı) bir web uygulamasında kullanılabilir. Genel konak önerilir ve Web ana bilgisayarı, kullanılabilir geriye dönük uyumluluk.

Bir çerçeve sağlar `CreateDefaultBuilder` ve `ConfigureWebHostDefaults` kullanılan aşağıdaki gibi seçeneklere sahip bir konak için yaygın olarak ayarlamak için yöntemleri:

* Kullanım [Kestrel](#servers) web sunucusu ve etkin IIS tümleştirme olarak.
* Yük yapılandırmasından *appsettings.json*, *appsettings. { Ortam adı} .json*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynaklarını.
* Günlük çıktısı, konsol ve hata ayıklama sağlayıcılarına gönderin.

Bir konak yapılar örnek kod aşağıda verilmiştir. Yaygın olarak kullanılan seçenekler konakla ayarlama yöntemleri vurgulanmıştır:

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/web-host>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 2.x kullanan Web ana bilgisayarı (`WebHost` sınıfı) web uygulamaları için. Bir çerçeve sağlar `CreateDefaultBuilder` seçenekleri, aşağıdakiler gibi yaygın olarak konakla ayarlamak için kullanılan:

* Kullanım [Kestrel](#servers) web sunucusu ve etkin IIS tümleştirme olarak.
* Yük yapılandırmasından *appsettings.json*, *appsettings. { Ortam adı} .json*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynaklarını.
* Günlük çıktısı, konsol ve hata ayıklama sağlayıcılarına gönderin.

Bir konak yapılar örnek kod aşağıda verilmiştir:

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.

::: moniker-end

### <a name="advanced-host-scenarios"></a>Gelişmiş Ana senaryoları

::: moniker range=">= aspnetcore-3.0"

Genel konak kullanmak .NET Core için herhangi bir uygulama kullanılabilir&mdash;yalnızca ASP.NET Core uygulamaları. Genel ana bilgisayar (`Host` sınıfı) günlüğe kaydetme, DI, yapılandırma ve uygulama ömrü yönetimi gibi çapraz kesme framework uzantıları kullanmak için uygulama diğer türleri sağlar. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Web ana bilgisayarı, hangi .NET uygulamalarının diğer türleri için gerekli olmayan bir HTTP sunucusu uygulamasını dahil etmek için tasarlanmıştır. ASP.NET Core 2.1 genel konak başlatılıyor (`Host` sınıfı) bir .NET Core uygulaması kullanmak kullanılabilir&mdash;yalnızca ASP.NET Core uygulamaları. Genel konak günlüğe kaydetme, DI, yapılandırma ve uygulama ömrü yönetimi gibi çapraz kesme framework uzantıları kullanmak için uygulama diğer türleri sağlar. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.

::: moniker-end

Konak, arka plan görevleri çalıştırmak için de kullanabilirsiniz. Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.

## <a name="servers"></a>Sunucular

ASP.NET Core uygulaması, HTTP isteklerini dinlemek için bir HTTP sunucusu uygulamasını kullanır. Uygulamaya bir dizi olarak sunucu yüzeyleri istekleri [istek özellikleri](xref:fundamentals/request-features) içine oluşan bir `HttpContext`.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:

* *Kestrel'i* bir platformlar arası web sunucusudur. Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/). ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.
* *IIS HTTP sunucusu* IIS kullanan bir windows Server'de olduğu. IIS ve ASP.NET Core uygulaması, bu sunucu ile aynı işlem içinde çalıştırın.
* *HTTP.sys* , IIS ile kullanılmayan Windows için sunucusudur.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması. ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir. Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması. ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir. Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core aşağıdaki sunucu uygulamaları sağlar:

* *Kestrel'i* bir platformlar arası web sunucusudur. Kestrel'i, kullanılarak bir ters proxy yapılandırma genellikle çalıştırılır [IIS](https://www.iis.net/). ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir.
* *HTTP.sys* , IIS ile kullanılmayan Windows için sunucusudur.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması. ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir. Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](https://nginx.org) veya [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core sağlar *Kestrel* platformlar arası sunucusu uygulaması. ASP.NET Core 2.0 veya sonraki sürümlerde, Kestrel doğrudan Internet'e açık genel kullanıma yönelik bir uç sunucusu olarak çalıştırılabilir. Kestrel'i bir ters proxy yapılandırması ile çalıştırmak genellikle [Ngınx](http://nginx.org) veya [Apache](https://httpd.apache.org/).

---

::: moniker-end

Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Yapılandırma

ASP.NET Core ayarları ad-değer çiftleri olarak sıralı bir dizi yapılandırma sağlayıcıları alır bir yapılandırma çerçevesi sağlar. Yerleşik yapılandırma sağlayıcıları vardır, kaynakları çeşitli gibi *.json* dosyaları *.xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri. Özel yapılandırma sağlayıcıları da yazabilirsiniz.

Örneğin, bu yapılandırma geldiği belirtebilirsiniz *appsettings.json* ve ortam değişkenleri. Sonra ne zaman değerini *ConnectionString* istenildiğinde, framework ilk olarak görünür *appsettings.json* dosya. Değer var. bulunursa, aynı zamanda bir ortam değişkeni, ortam değişkeninin değerini öncelik kazanır.

ASP.NET Core sağlar parolalar gibi gizli yapılandırma verilerini yönetmek için bir [gizli dizi Yöneticisi aracını](xref:security/app-secrets). Üretim gizli öğeleri için öneririz [Azure anahtar kasası](xref:security/key-vault-configuration).

Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

## <a name="options"></a>Seçenekler

Mümkünse, ASP.NET Core aşağıdaki *seçenekleri deseni* depolamak ve yapılandırma değerleri almak için. Seçenekleri deseni sınıfları, ilgili ayar gruplarını temsil etmek için kullanır.

Örneğin, aşağıdaki kodu WebSockets seçeneklerini ayarlar:

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.

## <a name="environments"></a>Ortamlar

Yürütme ortamlarını gibi *geliştirme*, *hazırlama*, ve *üretim*, ASP.NET Core, birinci sınıf bir kavram olan. Bir uygulama ortamı çalışır ayarlayarak belirtebilirsiniz `ASPNETCORE_ENVIRONMENT` ortam değişkeni. ASP.NET Core uygulaması başlatma sırasında bu ortam değişkenini okur ve değerini depolayan bir `IHostingEnvironment` uygulaması. Ortam nesnesi herhangi bir uygulamayı DI aracılığıyla kullanıma sunulmuştur.

Aşağıdaki örnek, koddan `Startup` sınıfı yalnızca geliştirme çalıştığında, ayrıntılı hata bilgileri sağlamak için uygulama yapılandırır:

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

## <a name="logging"></a>Günlüğe Kaydetme

ASP.NET Core çeşitli günlük yerleşik ve üçüncü taraf sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler. Kullanılabilir sağlayıcılar şunları içerir:

* Konsol
* Hata ayıklama
* Windows olay izleme
* Windows olay günlüğü
* TraceSource
* Azure uygulama hizmeti
* Azure Application Insights

Yazma günlükleri öğesinden herhangi bir uygulamanın kodu alarak bir `ILogger` DI ve günlük metotları çağırma nesnesi.

İşte kullanan örnek kodu bir `ILogger` Oluşturucu ekleme ve vurgulanmış günlük yöntem çağrılarını içeren bir nesne.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

`ILogger` Arabirimi herhangi bir sayıda alanlar için oturum açma sağlayıcısı geçirmenize olanak tanır. Alanları bir ileti dizesi oluşturmak için yaygın olarak kullanılır, ancak sağlayıcısı Ayrıca bunları ayrı alanlar olarak bir veri deposuna gönderebilir. Bu özelliği uygulamak günlük sağlayıcıları için mümkün kılar [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

## <a name="routing"></a>Yönlendirme

A *rota* bir işleyici eşlenmiş bir URL deseni şudur. İşleyici, bir Razor sayfası, bir MVC denetleyicisi veya bir ara yazılım bir eylem yöntemi genellikle oluşur. Uygulamanız tarafından kullanılan URL'leri üzerinde denetim ASP.NET Core yönlendirme sağlar.

Daha fazla bilgi için bkz. <xref:fundamentals/routing>.

## <a name="error-handling"></a>Hata işleme

ASP.NET Core gibi hataları işlemek için yerleşik özelliklere sahiptir:

* Bir geliştirici özel durumu sayfası
* Özel hata sayfaları
* Statik durumu kod sayfaları
* Başlangıç özel durum işleme

Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.

## <a name="make-http-requests"></a>HTTP isteğinde bulunma

Uygulanışı `IHttpClientFactory` oluşturmak için kullanılabilir `HttpClient` örnekleri. Fabrika:

* Adlandırma ve mantıksal yapılandırmak için merkezi bir konum sağlar `HttpClient` örnekleri. Örneğin, bir *github* istemci kayıtlı ve GitHub erişim sağlamak için yapılandırılmış. Varsayılan istemci diğer amaçlar için kaydedilebilir.
* Kayıt ve giden bir istek ara yazılım ardışık düzenini oluşturmak için birden fazla temsilci işleyicileri zincirleme destekler. Bu düzen, ASP.NET Core gelen ara yazılım ardışık benzerdir. Desen etrafında HTTP isteklerini, önbelleğe alma, hata, seri hale getirme, işleme ve günlüğe kaydetme gibi çapraz kesme konuları yönetmek için bir mekanizma sağlar.
* Tümleşik şekilde çalışarak *Polly*, geçici hata işlemeye yönelik popüler bir üçüncü taraf kitaplığı.
* Havuzu ve arka plandaki, yaşam süresini yöneten `HttpClientMessageHandler` el ile yönetilmesi sırasında oluşan Genel DNS sorunları önlemek için örnekleri `HttpClient` yaşam süresi yok.
* Yapılandırılabilir günlük deneyimi ekler (aracılığıyla `ILogger`) fabrikası tarafından oluşturulan istemcileri aracılığıyla gönderilen tüm istekler için.

Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.

## <a name="content-root"></a>İçerik kök

Temel yol, Razor dosyaları gibi uygulama tarafından kullanılan herhangi bir özel içerik için içerik köküdür. Varsayılan olarak, içerik kök uygulama barındırma yürütülebilir dosya için taban yoludur. Alternatif bir konuma olabilir belirtilen [konak oluşturmaya](#host).

::: moniker range=">= aspnetcore-3.0"

Daha fazla bilgi için [içerik kök](xref:fundamentals/host/generic-host#content-root).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Daha fazla bilgi için [içerik kök](xref:fundamentals/host/web-host#content-root).

::: moniker-end

## <a name="web-root"></a>Web kökü

Web kökü (diğer adıyla *webroot*) genel, statik kaynakları, CSS, JavaScript ve görüntü dosyaları gibi temel yolu. Statik dosya ara yazılımı, varsayılan olarak yalnızca bir hizmet web kök dizinine (ve alt dizinleri) dosyalarından verecektir. Varsayılan olarak web kök yolu *{içerik kök} / wwwroot*, ancak farklı bir konuma A'da belirtildiği zaman [konak oluşturmaya](#host).

Razor'daki (*.cshtml*) dosyaları, eğik çizgi tilde `~/` web kök dizinine işaret eder. İle başlayan yollar `~/` sanal yol adlandırılır.

Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.
