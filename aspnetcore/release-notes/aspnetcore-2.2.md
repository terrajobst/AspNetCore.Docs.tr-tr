---
title: ASP.NET Core 2.2 içinde yenilikler nelerdir?
author: tdykstra
description: ASP.NET Core 2.2 yeni özellikler hakkında bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/03/2018
uid: aspnetcore-2.2
ms.openlocfilehash: 7429cb725a630c080690d81ec75d291bf48be2fc
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862829"
---
# <a name="whats-new-in-aspnet-core-22"></a>ASP.NET Core 2.2 içinde yenilikler nelerdir?

Bu makalede, ASP.NET Core 2.2 ile ilgili belgelere bağlantılar en önemli değişiklikleri vurgular.

## <a name="open-api-analyzers--conventions"></a>Açık API Çözümleyicileri & kuralları

Açık API (Swagger da bilinir), REST API'leri açıklayan bir dilden belirtimdir. Açık API ekosistemi, keşfetme, test ve belirtimi kullanılarak istemci kodu oluşturmayı sağlayan araçlara sahiptir. Oluşturma ve ASP.NET Core MVC açık API belgelerinde görselleştirmek için destek projeleri gibi temelli topluluk sağlanan [NSwag](https://github.com/RSuter/NSwag), ve [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore). ASP.NET Core 2.2 Gelişmiş araç sağlar ve açık API belgeleri oluşturmak için çalışma zamanı karşılaşır.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* <xref:tutorials/web-api-help-pages-using-swagger>
* [ASP.NET Core 2.2.0-preview1: açık API Çözümleyicileri & kuralları](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/).

## <a name="endpoint-routing"></a>Uç noktası yönlendirme

ASP.NET Core 2.2 kullanan yeni bir *uç noktası yönlendirme* sistemi için geliştirilmiş istekleri gönderme. Değişiklikler yeni nesil API üyeleri bağlayın.

Daha fazla bilgi için [IRouter ve uç nokta tabanlı yönlendirme arasındaki farklar](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing).

## <a name="health-checks"></a>Sistem durumu denetimleri

Yeni bir sistem durumu hizmeti, Kubernetes gibi sistem durumu denetimleri gerektiren ortamlar içinde ASP.NET Core kullanılmalı kolaylaştırır denetler. Durum denetimleri ara yazılım ve tanımlayan bir dizi içeren bir `IHealthCheck` soyutlama ve hizmet.

Sistem durumu denetimleri veya yük dengeleyici hızlı bir şekilde bir sistemi istekleri için normalde yanıt verdiğini belirlemek için bir kapsayıcı Düzenleyicisi tarafından kullanılır. Kapsayıcı Düzenleyicisi başarısız durum yanıt dağıtım çalışırken veya kapsayıcı yeniden durdurma tarafından kontrol edin. Bir yük dengeleyici, hizmetin başarısız örneğini ayrılmak yönlendirme trafiği için sistem durumu denetimi yanıt verebilir.

Sistem durumu denetimleri, izleme sistemlerinden tarafından kullanılan bir HTTP uç noktası olarak bir uygulama tarafından sunulur. Sistem durumu denetimleri, gerçek zamanlı izleme senaryoları ve izleme sistemlerinden çeşitli için yapılandırılabilir.

Daha fazla bilgi için [durum denetimleri ASP.NET Core](xref:host-and-deploy/health-checks).

## <a name="http2-in-kestrel"></a>HTTP/2'de Kestrel

ASP.NET Core 2.2 HTTP/2 desteği ekler. 

HTTP/2 HTTP protokolü, büyük bir düzeltme'dir. HTTP/2'in önemli özelliklerinden bazıları, üst bilgi sıkıştırma desteği olan ve tam olarak tek bir bağlantı üzerinden akışları multiplexed. HTTP/2 (HTTP üstbilgileri, yöntemler vb.) HTTP'ın semantiğini korur ancak bu verilerin nasıl Çerçeveli ve kablo üzerinden gönderilen üzerinde HTTP/1.x bozucu değişiklik var.

Söz konusu kümelerdeki bu değişikliği çerçeveleme, sunucular ve istemciler kullanılan protokolü sürümü anlaşma gerekir. Uygulama katmanı protokol anlaşması (ALPN) sunucu ve istemci, TLS anlaşması bir parçası olarak kullanılan protokolü sürümü anlaşma izin veren bir TLS uzantısıdır. Sunucu ve istemci arasındaki önceki bilgisine protokolüne sahip mümkün olsa da, tüm bilinen tarayıcılar HTTP/2 bağlantı kurmak için tek yolu ALPN destekler.

Daha fazla bilgi için [HTTP/2 desteği](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support).

## <a name="kestrel-configuration"></a>Kestrel'i yapılandırma

ASP.NET Core önceki sürümlerinde çağırarak Kestrel seçenekleri yapılandırıldıktan `UseKestrel`. 2.2 içinde çağırarak Kestrel seçenekleri yapılandırıldıktan `ConfigureKestrel` üzerinde konak Oluşturucusu. Bu değişiklik düzende bir sorunu giderir `IServer` işlem içi barındıran kayıtları. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Useıis çakışma azaltın](https://github.com/aspnet/KestrelHttpServer/issues/2760)
* [İle ConfigureKestrel Kestrel sunucu seçeneklerini yapılandır](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a>İşlem içi IIS barındırma

ASP.NET Core önceki sürümlerinde, IIS ters bir proxy olarak görev yapar. 2.2 içinde ASP.NET Core modülü CoreCLR önyükleme ve IIS çalışan işlemi içinde bir uygulamayı barındırmak (*w3wp.exe*). Barındırma işlemi içinde önemli bir performans ve tanılama kazançlar IIS ile çalıştırırken sağlar.

Daha fazla bilgi için [IIS işlem içi barındırma](xref:fundamentals/servers/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model).

## <a name="signalr-java-client"></a>SignalR Java istemci

ASP.NET Core 2.2 SignalR için Java istemcisi sunar. Bu istemci, Android uygulamaları dahil olmak üzere Java koddan bir ASP.NET Core SignalR sunucusuna bağlanmayı destekler.

Daha fazla bilgi için [ASP.NET Core SignalR Java istemci](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2).

## <a name="cors-improvements"></a>CORS geliştirmeleri

ASP.NET Core önceki sürümlerinde, CORS ara yazılım sağlar `Accept`, `Accept-Language`, `Content-Language`, ve `Origin` içinde yapılandırılan değerlere bakılmaksızın gönderilecek üstbilgileri `CorsPolicy.Headers`. Üstbilgileri gönderilen 2.2 içinde bir CORS ara yazılımı ilke eşleşmesi yalnızca mümkün olduğunda `Access-Control-Request-Headers` belirtilen üst bilgilerin tam olarak eşleşmesi `WithHeaders`.

Daha fazla bilgi için [CORS ara yazılımı](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers).

## <a name="response-compression"></a>Yanıt sıkıştırma

ASP.NET Core 2.2 yanıtlarıyla sıkıştırabilir [Brotli sıkıştırma biçimini](https://tools.ietf.org/html/rfc7932).

Daha fazla bilgi için [yanıt sıkıştırma ara yazılımı destekler Brotli sıkıştırma](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider).

## <a name="project-templates"></a>Proje şablonları

ASP.NET Core web projesi şablonları güncelleştirildi [önyükleme 4](https://getbootstrap.com/docs/4.1/migration/) ve [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4). Yeni bir görünüme görsel olarak basittir ve uygulama önemli yapıları görmeyi kolaylaştırır.

![Giriş ya da dizin sayfası](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## <a name="validation-performance"></a>Doğrulama performans

MVC'nin doğrulama sistemi, genişletilebilir ve esnek, belirli bir model için doğrulayıcıları uygulanacak istek başına temelinde belirlemenize olanak sağlayan olacak şekilde tasarlanmıştır. Bu, karmaşık doğrulama sağlayıcılarının yazmak için idealdir. Ancak, en sık karşılaşılan bir uygulama yalnızca kullandığı yerleşik doğrulayıcıları durumda ve bu ek esneklik gerekmez. Yerleşik doğrulayıcıları DataAnnotations gibi dahil [gerekli] ve [StringLength] ve `IValidatableObject`.

Belirtilen model grafik doğrulama gerektirmeyen belirlerse, ASP.NET Core 2.2 içinde doğrulama MVC iki. Doğrulama sonuçları önemli geliştirmeler, tüm doğrulayıcıları yok veya model doğrulanırken atlanıyor. Bu temel elemanlar koleksiyonlar gibi nesneleri içerir (gibi `byte[]`, `string[]`, `Dictionary<string, string>`), veya karmaşık nesne grafiklerini olmadan birçok doğrulayıcıları.

## <a name="http-client-performance"></a>HTTP istemci performansı

ASP.NET Core 2.2, performansını, `SocketsHttpHandler` Çekişme kilitleme bağlantı havuzu azaltarak geliştirildi. Bazı mikro hizmet mimarileri gibi birçok giden HTTP isteklerini, olun uygulamalar için aktarım hızı geliştirildi. Yük altında `HttpClient` aktarım hızı geliştirilmiş masraflarını % 60 oranında Linux üzerinde ve Windows üzerinde %20.

Daha fazla bilgi için [bu geliştirmesi yaptık çekme isteği](https://github.com/dotnet/corefx/pull/32568).

## <a name="additional-information"></a>Ek bilgiler

Değişikliklerin tam listesi için bkz. [ASP.NET Core 2.2 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.2.0).
