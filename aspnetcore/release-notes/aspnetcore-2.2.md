---
title: ASP.NET Core 2,2 ' deki yenilikler
author: rick-anderson
description: ASP.NET Core 2,2 ' deki yeni özellikler hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: aspnetcore-2.2
ms.openlocfilehash: 97deafd520926476f7653fc3de40d577b394734b
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829133"
---
# <a name="whats-new-in-aspnet-core-22"></a>ASP.NET Core 2,2 ' deki yenilikler

Bu makalede, ASP.NET Core 2,2 ' deki en önemli değişiklikler, ilgili belgelerin bağlantılarıyla vurgulanır.

## <a name="openapi-analyzers--conventions"></a>Openapı Çözümleyicileri & kuralları

Openapı (eski adıyla Swagger), REST API 'Leri açıklamak için dilden bağımsız bir belirtimdir. Openapı ekosistemi, belirtimi kullanarak istemci kodu bulma, test etme ve oluşturmaya imkan tanıyan araçlara sahiptir. ASP.NET Core MVC 'de Openapı belgelerini oluşturma ve görselleştirmeye yönelik destek, [Nswag](https://github.com/RicoSuter/NSwag) ve [swashbuckle. aspnetcore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)gibi topluluk odaklı projeler aracılığıyla sağlanır. ASP.NET Core 2,2, Openapı belgelerini oluşturmak için geliştirilmiş araç ve çalışma zamanı deneyimleri sağlar.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* <xref:web-api/advanced/analyzers>
* <xref:web-api/advanced/conventions>
* [ASP.NET Core 2.2.0-preview1 'i: Openapı Çözümleyicileri & kuralları](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/)

## <a name="problem-details-support"></a>Sorun ayrıntıları desteği

ASP.NET Core 2,1, HTTP yanıtıyla bir hatanın ayrıntılarını taşıyan [RFC 7807](https://tools.ietf.org/html/rfc7807) belirtimine göre `ProblemDetails`tanıtılmıştır. 2,2 ' de `ProblemDetails`, `ApiControllerAttribute`öznitelikli denetleyicilerde istemci hata kodları için standart yanıttır. İstemci hata durum kodu (4xx) döndüren bir `IActionResult`, artık bir `ProblemDetails` gövdesi döndürüyor. Sonuç Ayrıca, istek günlüklerini kullanarak hatayı ilişkilendirmek için kullanılabilecek bir bağıntı KIMLIĞI içerir. İstemci hataları için `ProducesResponseType` varsayılan olarak yanıt türü olarak `ProblemDetails` kullanın. Bu, NSwag veya swashbuckle. AspNetCore kullanılarak oluşturulan OpenAPI/Swagger çıktısında belgelenmiştir.

## <a name="endpoint-routing"></a>Uç nokta yönlendirme

ASP.NET Core 2,2, isteklerin gelişmiş şekilde gönderilirken yeni bir *uç nokta yönlendirme* sistemi kullanır. Değişiklikler yeni bağlantı oluşturma API 'SI üyelerini ve yol parametresi dönüştürücüler içerir.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [2,2 içinde Endpoint Routing](https://blogs.msdn.microsoft.com/webdev/2018/08/27/asp-net-core-2-2-0-preview1-endpoint-routing/)
* [Yol parametresi dönüştürücüler](https://www.hanselman.com/blog/ASPNETCore22ParameterTransformersForCleanURLGenerationAndSlugsInRazorPagesOrMVC.aspx) (bkz. **yönlendirme** bölümü)
* [IRouter ve uç nokta tabanlı yönlendirme arasındaki farklar](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing)

## <a name="health-checks"></a>Sistem durumu denetimleri

Yeni bir sistem durumu denetimleri hizmeti, Kubernetes gibi sistem durumu denetimleri gerektiren ortamlarda ASP.NET Core kullanmayı kolaylaştırır. Sistem durumu denetimleri, ara yazılım ve bir `IHealthCheck` soyutlama ve hizmet tanımlayan bir kitaplık kümesi içerir.

Sistem durum denetimleri bir kapsayıcı Orchestrator veya yük dengeleyici tarafından, sistemin isteklere normal olarak yanıt verip vermediğini hızlı bir şekilde tespit etmek için kullanılır. Bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir. Yük dengeleyici, trafiği hizmetin başarısız olan örneğinden dışarıda yönlendirerek bir sistem durumu denetimine yanıt verebilir.

Sistem durumu denetimleri, bir uygulama tarafından, izleme sistemleri tarafından kullanılan bir HTTP uç noktası olarak sunulur. Sistem durumu denetimleri, çeşitli gerçek zamanlı izleme senaryoları ve izleme sistemleri için yapılandırılabilir. Sistem durumu denetimleri hizmeti, [Beatpulse projesiyle](https://github.com/Xabaril/BeatPulse)tümleştirilir. Bu, düzinelerce popüler sistemler ve Bağımlılıklar için denetim eklemenizi kolaylaştırır.

Daha fazla bilgi için bkz. [ASP.NET Core durum denetimleri](xref:host-and-deploy/health-checks).

## <a name="http2-in-kestrel"></a>Kestrel içinde HTTP/2

ASP.NET Core 2,2, HTTP/2 desteği ekliyor.

HTTP/2, HTTP protokolünün önemli bir düzeltmesidir. HTTP/2 ' nin önemli özellikleri şunlardır:

* Üst bilgi sıkıştırma desteği.
* Tek bir bağlantı üzerinden tamamen çoğullanmış akışlar.

Http/2, HTTP 'nin semantiğini (örneğin, HTTP üst bilgileri ve yöntemleri) korur, ancak istemci ve sunucu arasında verilerin nasıl çerçeveli ve gönderilir olduğunu HTTP/1. x ' den önemli bir değişiklik olur.

Çerçeveleme içindeki bu değişikliğin bir sonucu olarak, sunucuların ve istemcilerin kullanılan protokol sürümünü anlaşması gerekir. Uygulama katmanı protokol anlaşması (ALPN), sunucunun ve istemcisinin TLS el sıkışmasının bir parçası olarak kullanılan protokol sürümünü anlaşmasına izin veren bir TLS uzantısıdır. Sunucu ve protokoldeki istemci arasında bir önceki bilgi sahibi olmak mümkün olsa da, tüm büyük tarayıcılarda bir HTTP/2 bağlantısı kurmak için tek yol olarak tüm büyük tarayıcılar desteklenir.

Daha fazla bilgi için bkz. [http/2 desteği](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support).

## <a name="kestrel-configuration"></a>Kestrel yapılandırması

ASP.NET Core önceki sürümlerinde, Kestrel seçenekleri `UseKestrel`çağırarak yapılandırılır. 2,2 ' de, Kestrel seçenekleri konak Oluşturucu üzerinde `ConfigureKestrel` çağırarak yapılandırılır. Bu değişiklik, işlem içi barındırma için `IServer` kayıtları sırasıyla bir sorunu çözer. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Useııs çakışmasını azaltma](https://github.com/aspnet/KestrelHttpServer/issues/2760)
* [ConfigureKestrel ile Kestrel Server seçeneklerini yapılandırma](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a>IIS işlem içi barındırma

ASP.NET Core önceki sürümlerinde IIS, ters proxy işlevi görür. 2,2 ' de ASP.NET Core modülü CoreCLR 'yi önyükleyebilir ve IIS çalışan işleminin (*W3wp. exe*) içinde bir uygulamayı barındırabilir. İşlem içi barındırma, IIS ile çalışırken performans ve tanılama kazançları sağlar.

Daha fazla bilgi için bkz. [IIS için işlem içi barındırma](xref:host-and-deploy/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model).

## <a name="opno-locsignalr-java-client"></a>SignalR Java istemcisi

ASP.NET Core 2,2 SignalRiçin bir Java Istemcisi sunar. Bu istemci, Android uygulamaları dahil olmak üzere Java kodundan bir ASP.NET Core SignalR sunucusuna bağlanmayı destekler.

Daha fazla bilgi için bkz. [ASP.NET Core SignalR Java istemcisi](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2).

## <a name="cors-improvements"></a>CORS geliştirmeleri

ASP.NET Core önceki sürümlerinde, CORS ara yazılımı `Origin` yapılandırılan değerlere bakılmaksızın `Accept`, `Accept-Language`, `Content-Language`ve `CorsPolicy.Headers`üst bilgilerinin gönderilmesini sağlar. 2,2 ' de, bir CORS ara yazılım ilkesi eşleşmesi yalnızca `Access-Control-Request-Headers` ' de gönderilen üstbilgiler `WithHeaders`belirtilen üstbilgileriyle tam olarak eşleşiyorsa mümkündür.

Daha fazla bilgi için bkz. [CORS ara yazılımı](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers).

## <a name="response-compression"></a>Yanıt sıkıştırma

ASP.NET Core 2,2, yanıtları [Brotli sıkıştırma biçimiyle](https://tools.ietf.org/html/rfc7932)sıkıştırabilir.

Daha fazla bilgi için bkz. [Yanıt sıkıştırma ara yazılımı Brotli sıkıştırmayı destekler](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider).

## <a name="project-templates"></a>Proje şablonları

ASP.NET Core Web projesi şablonları, [önyükleme 4](https://getbootstrap.com/docs/4.1/migration/) ve [angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4)' ya güncelleştirildi. Yeni görünüm görsel açıdan basittir ve uygulamanın önemli yapılarını görmeyi kolaylaştırır.

![Giriş veya dizin sayfası](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## <a name="validation-performance"></a>Doğrulama performansı

MVC 'nin doğrulama sistemi genişletilebilir ve esnek olacak şekilde tasarlanmıştır. bu sayede, belirli bir modele yönelik doğrulayıcıların hangi istek tabanlı olarak uygulanacağını belirlemenizi sağlar. Bu, karmaşık doğrulama sağlayıcıları yazmak için idealdir. Ancak, en yaygın durumda bir uygulama yalnızca yerleşik Doğrulayıcıları kullanır ve bu fazladan esneklik gerektirmez. Yerleşik doğrulayıcılar, [gerekli] ve [StringLength] gibi veri açıklamalarını içerir ve `IValidatableObject`.

ASP.NET Core 2,2 ' de, MVC, belirli bir model grafiğinin doğrulama gerektirmediğini belirlerse kısa devre doğrulaması yapabilir. Doğrulama sonuçları, hiçbir doğrulayıcıya sahip olmayan modelleri doğrularken önemli geliştirmelere karşı atlanıyor. Bu, çok sayıda Doğrulayıcıları olmayan temel elemanlar koleksiyonları (`byte[]`, `string[]`, `Dictionary<string, string>`) veya karmaşık nesne grafikleri gibi nesneleri içerir.

## <a name="http-client-performance"></a>HTTP Istemci performansı

ASP.NET Core 2,2 ' de, bağlantı havuzu kilitleme çakışması azaltılarak `SocketsHttpHandler` performansı geliştirilmiştir. Bazı mikro hizmet mimarileri gibi birçok giden HTTP isteğini yapan uygulamalar için üretilen iş geliştirildi. Yük altında `HttpClient` işleme, Linux üzerinde %60 ' e kadar ve Windows üzerinde %20 oranında artırılabilir.

Daha fazla bilgi için [Bu geliştirmeyi yapan çekme isteğine](https://github.com/dotnet/corefx/pull/32568)bakın.

## <a name="additional-information"></a>Ek bilgiler

Değişikliklerin tamamı listesi için [ASP.NET Core 2,2 sürüm notlarına](https://github.com/dotnet/aspnetcore/releases/tag/2.2.0)bakın.
