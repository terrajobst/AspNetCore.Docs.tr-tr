---
title: ASP.NET Core performansı en iyi uygulamalar
author: mjrousos
description: Genel performans sorunlarını önleme ve ASP.NET Core uygulamaları performansını artırmak için ipuçları.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 1/9/2019
uid: performance/performance-best-practices
ms.openlocfilehash: 25aa4c1e22ead7db4775c6e5e81b6fd627c6d7a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099071"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core performansı en iyi uygulamalar

Tarafından [Mike Rousos](https://github.com/mjrousos)

Bu konu, ASP.NET Core ile en iyi performans için yönergeler sağlar.

<a name="hot"></a> Bu belgede, sık erişimli kod yolu sık çağrılır ve yürütme süresi çoğunu oluştuğu bir kod yolu olarak tanımlanır. Sık erişimli kod yollarını genellikle uygulama ölçeklendirme ve performans sınırlayın.

## <a name="cache-aggressively"></a>Agresif bir biçimde önbelleğe alma

Önbelleğe alma, bu belgenin birden fazla bölümde ele alınmıştır. Daha fazla bilgi için bkz. <xref:performance/caching/response>.

## <a name="avoid-blocking-calls"></a>Çağrıları engellemekten kaçınacak

ASP.NET Core uygulamaları aynı anda birçok istekleri işlemek için tasarlanmış olmalıdır. Zaman uyumsuz API'leri çağrıları engellemeyi beklenmiyor tarafından binlerce eş zamanlı istekleri işlemek için iş parçacığı oluşan küçük bir havuz sağlar. Tamamlanması uzun süre çalışan zaman uyumlu görevde beklemek yerine, iş parçacığı başka bir istek üzerinde çalışabilir.

ASP.NET Core uygulamalarında ortak bir performans sorunu, zaman uyumsuz çağrılar engelliyor. Birçok eş zamanlı engelleme çağrı müşteri adayları [iş parçacığı havuzu starvation](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) ve yanıt sürelerini önemli.

**Sağlamadığı**:

* Zaman uyumsuz yürütme engelleme çağırarak [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) veya [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Ortak kod yollarını kilitler edinin. ASP.NET Core, çoğu kod paralel olarak çalıştırmak için tasarlanmış, yüksek performanslı uygulamalardır.

**Yapmak**:

* Olun [sık erişimliye kod yollarını](#hot) zaman uyumsuz.
* Veri erişimi ve uzun süre çalışan işlemleri API zaman uyumsuz olarak çağırın.
* Denetleyici/Razor sayfa eylemleri zaman uyumsuz olarak yapın. Bütün çağrı yığını sayesinde bir avantaj elde için zaman uyumsuz olması gereken [async/await](/dotnet/csharp/programming-guide/concepts/async/) desenleri.

Bir profil oluşturucu ister [PerfView](https://github.com/Microsoft/perfview) sık eklenen iş parçacıkları aramak için kullanılan [iş parçacığı havuzu](/windows/desktop/procthread/thread-pool). `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` Olay iş parçacığı havuzuna eklenen bir iş parçacığı gösterir. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>Büyük nesne ayırma simge durumuna küçült

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core --> [.NET Core çöp toplayıcı](/dotnet/standard/garbage-collection/) ayırma ve serbest bırakma bellek ASP.NET Core uygulamaları otomatik olarak yönetir. Otomatik çöp toplama genellikle geliştiriciler nasıl veya ne zaman bellek serbest bırakılır hakkında endişelenmeniz gerekmez anlamına gelir. Böylece geliştiriciler ayırma nesneler en aza indirmeniz gerekir ancak başvurulmayan nesnelerin temizlenmesi CPU süresi alan [sık erişimliye kod yollarını](#hot). Çöp toplama, özellikle büyük nesneler (> 85 K bayt) pahalıdır. Büyük nesneler üzerinde depolanan [büyük nesne yığını](/dotnet/standard/garbage-collection/large-object-heap) ve (2. nesil) tam çöp toplama temizlemek için. Nesil 0 ve 1. nesil koleksiyonlar farklı olarak, uygulamanın yürütülmesini geçici olarak askıya alınması için 2. nesil koleksiyonu gerektirir. Sık ayırmayı ve ayırmayı kaldırma büyük nesnelerin yetersiz performansa neden olabilir.

Öneriler:

* **Yapmak** sık kullanılan büyük nesneleri önbelleğe almayı düşünün. Büyük nesnelerin önbelleğe alma, pahalı ayırmaları engeller.
* **Yapmak** kullanarak arabellek havuzu bir [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) büyük dizileri depolamak için.
* **Sağlamadığı** birçok, kısa süreli büyük nesneler şirket ayrılamadı [sık erişimliye kod yollarını](#hot).

Önceki çöp toplama (GC) istatistikleri de gözden geçirerek koydu gibi bellek sorunlarını [PerfView](https://github.com/Microsoft/perfview) inceleyerek:

* Çöp toplama duraklatma süresi.
* Yüzde işlemci zamanı, çöp toplama harcanır.
* Kaç çöp koleksiyonları kuşak 0, 1 ve 2 ' dir.

Daha fazla bilgi için [atık toplama ve performans](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Veri erişimini iyileştirmek

Bir veri deposunu veya diğer uzak Hizmetleri ile etkileşim genellikle en yavaş bir ASP.NET Core uygulaması parçasıdır. Verimli veri yazma ve okuma için iyi bir performans önemlidir.

Öneriler:

* **Yapmak** tüm veri erişimi API'leri zaman uyumsuz olarak çağırın.
* **Sağlamadığı** gerekli olandan daha fazla veri alın. Geçerli HTTP isteği için gerekli olan verileri döndürmek için sorgular yazarsınız.
* **Yapmak** önbelleğe sık erişilen biraz güncel olmayan verileri için kabul edilebilir ise bir veritabanı veya uzak hizmetinden alınan verileri göz önünde bulundurun. Senaryoya bağlı olarak kullanabileceğinize bir [MemoryCache](xref:performance/caching/memory) veya [DistributedCache](xref:performance/caching/distributed). Daha fazla bilgi için bkz. <xref:performance/caching/response>.
* Simge Durumuna Küçült gidiş dönüş ağ. Tek bir çağrıda gereken tüm verileri yerine çeşitli çağrılar alınacak hedeftir.
* **Yapmak** kullanın [Hayır izleme sorguları](/ef/core/querying/tracking#no-tracking-queries) salt okunur amacıyla verilere erişirken Entity Framework Core içinde. EF Core Hayır izleme sorguların sonuçlarını daha verimli bir şekilde döndürebilirsiniz.
* **Yapmak** filtre ve toplama LINQ sorguları (ile `.Where`, `.Select`, veya `.Sum` deyimleri, örneğin) ve böylece filtreleme işlemi veritabanı tarafından yapılır.
* **Yapmak** EF Core bazı sorgu işleçleri verimsiz sorgu yürütülmesine neden olabilir istemcide çözümler göz önünde bulundurun. Daha fazla bilgi için [istemci değerlendirme performans sorunları](/ef/core/querying/client-eval#client-evaluation-performance-issues)
* **Sağlamadığı** "N + 1" yürütülmesi sonucunda koleksiyonlarda yansıtma sorguları kullanmak SQL sorguları. Daha fazla bilgi için [bağıntılı alt sorgularda en iyi duruma getirilmesi](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Bkz: [EF yüksek performanslı](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) büyük ölçekli uygulamalarda performansı iyileştirebilir yaklaşımlar için:

* [DbContext havuzu](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Açıkça derlenmiş sorgular](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Kod tabanınızın gerçekleştirmeden önce önceki yüksek performanslı yaklaşımları etkisini ölçmek öneririz. Derlenmiş sorgular ek karmaşıklığını performans artışını Yasla değil.

Sorgu zaman inceleyerek sorunları algılanamıyor harcanan erişen verilerle [Application Insights](/azure/application-insights/app-insights-overview) veya profil oluşturma araçları ile. Çoğu veritabanı istatistikleri de kullanılabilir sık yürütülen sorgular ilgili olun.

## <a name="pool-http-connections-with-httpclientfactory"></a>Havuz HTTP bağlantılarıyla HttpClientFactory

Ancak [HttpClient](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) uygulayan `IDisposable` arabirimi, amacı, yeniden kullanılabilmeleri. Kapalı `HttpClient` örnekleri yuva açık bırakın `TIME_WAIT` kısa bir süre için durum. Sonuç olarak, bir kod yolu oluşturup, siler, `HttpClient` nesneler sık kullanılan, uygulamanın kullanılabilir yuva tüketebilir. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) ASP.NET Core 2.1 içinde bu soruna bir çözüm olarak sunulmuştur. Bu, performansı ve güvenilirliği iyileştirmek için havuzu HTTP bağlantılarını işler.

Öneriler:

* **Sağlamadığı** oluşturun ve elden `HttpClient` doğrudan örnekler.
* **Yapmak** kullanın [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) alınacak `HttpClient` örnekleri. Daha fazla bilgi için [dayanıklı HTTP isteklerini uygulamak için kullanım HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Ortak kod yollarını hızlı tutun

Tüm kodunuzu hızlı olmasını istediğiniz, ancak sık çağrılan kod yollarının en iyi duruma getirmek için en önemli olan:

* Ara yazılım bileşenleri uygulamanın istek işleme ardışık düzeninde özellikle ara yazılımı erken işlem hattında çalıştırın. Bu bileşenlerin performans üzerinde büyük etkiye sahip.
* Her istek için veya birden çok kez istek başına yürütülen kod. Örneğin, özel günlük kaydı, yetkilendirme işleyicileri veya geçici Hizmetleri başlatma.

Öneriler:

* **Sağlamadığı** özel bir ara yazılım bileşenleri ile uzun süre çalışan görevleri kullanın.
* **Yapmak** performans profil oluşturma araçlarını kullanın (gibi [Visual Studio tanılama araçları](/visualstudio/profiling/profiling-feature-tour) veya [PerfView](https://github.com/Microsoft/perfview)) tanımlamak için [sık erişimliye kod yollarını](#hot).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>HTTP istekleri dışında görevler uzun süreli tamamlayın

ASP.NET Core uygulaması için en çok istekte bir denetleyici veya gerekli hizmetleri çağırmak ve bir HTTP yanıtı döndüren sayfa modeli tarafından işlenebilir. Uzun süre çalışan görevleri içeren bazı istekler için tüm istek-yanıt işlemini zaman uyumsuz kolaylaştırmak iyidir.

Öneriler:

* **Sağlamadığı** sıradan HTTP istek işlemenin bir parçası olarak tamamlanması uzun süre çalışan görevler için bekleyin.
* **Yapmak** uzun süren istekleri işleme göz önünde bulundurun [arka plan Hizmetleri](/aspnet/core/fundamentals/host/hosted-services) veya işlem dışında bir [Azure işlevi](/azure/azure-functions/). İş dışı işlem Tamamlanıyor, CPU yoğunluklu görevler için özellikle yararlıdır.
* **Yapmak** gibi gerçek zamanlı iletişim seçenekleri kullanmak [SignalR](xref:signalr/introduction) istemcilerle zaman uyumsuz olarak iletişim kurmak için.

## <a name="minify-client-assets"></a>İstemci varlıklar küçültün

ASP.NET Core uygulamaları karmaşık ön uç ile sık birçok JavaScript, CSS veya görüntü dosyaları işlevi görür. İlk yükleme istekleri performansını tarafından geliştirilebilir:

* Paketleme, birden çok dosyayı tek bir araya getiren.
* Küçültme, dosyaları tarafından boyutunu azaltır.

Öneriler:

* **Yapmak** kullanan ASP.NET Core'nın [yerleşik destek](xref:client-side/bundling-and-minification) paketleme ve küçültme istemci varlıklar için.
* **Yapmak** gibi diğer üçüncü taraf araçları göz önünde bulundurun [Gulp](uid:client-side/bundling-and-minification#consume-bundleconfigjson-from-gulp) veya [Web](https://webpack.js.org/) daha karmaşık istemci varlık yönetimi.

## <a name="compress-responses"></a>Yanıtları sıkıştırma

 Yanıt boyutu genellikle azaltma, uygulama yanıt verme hızını genellikle önemli ölçüde artırır. Yük boyutları azaltmak için bir uygulamanın yanıtları sıkıştırma yoludur. Daha fazla bilgi için [yanıt sıkıştırma](xref:performance/response-compression).

## <a name="use-the-latest-aspnet-core-release"></a>ASP.NET Core en son sürümü kullan

ASP.NET her yeni sürümü, performans iyileştirmeleri içerir. .NET Core ve ASP.NET Core iyileştirmeler, daha yeni sürümleri eski sürümleri aşar anlamına gelir. Örneğin, .NET Core 2.1 gelen benefitted ve derlenmiş normal ifadeler için destek eklendi [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx). HTTP/2 desteği ASP.NET Core 2.2 eklendi. Bir öncelik performans ise ASP.NET Core en güncel sürümüne yükseltmeyi göz önünde bulundurun.

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>Özel durumları en aza indirin

Özel durumlar seyrek olmalıdır. Oluşturmak ve özel durumları yakalamak, diğer kod akış desenlerini göre yavaştır. Bu nedenle, özel durumlar programın normal akışını denetlemek için kullanılmamalıdır.

Öneriler:

* **Sağlamadığı** atma veya özel durumları yakalama, özellikle de sık erişimli kod yollarını normal program akışı yöntemi olarak kullanın.
* **Yapmak** algılar ve bir özel durum neden olan koşulları işlemek için uygulamada mantığı içerir.
* **Yapmak** throw veya catch özel durumları için olağan dışı ya da beklenmeyen koşulları.

Uygulama Tanılama Araçları (örneğin, Application Insights) performansını etkileyebilecek bir uygulama yaygın özel durumları belirlemeye yardımcı olabilir.
