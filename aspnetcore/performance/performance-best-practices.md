---
title: ASP.NET Core performansı en iyi uygulamalar
author: mjrousos
description: Genel performans sorunlarını önleme ve ASP.NET Core uygulamaları performansını artırmak için ipuçları.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 05/10/2019
uid: performance/performance-best-practices
ms.openlocfilehash: 7651dff18f98c60057660c8946c3daa66d272f6a
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536079"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core performansı en iyi uygulamalar

Tarafından [Mike Rousos](https://github.com/mjrousos)

Bu konu, ASP.NET Core ile en iyi performans için yönergeler sağlar.

<a name="hot"></a>

Bu belgede bir *etkin kod yolu* sıkça çağrılan ve yürütme süresi çoğunu oluştuğu bir kod yolu olarak tanımlanır. Sık erişimli kod yollarını genellikle uygulama ölçeklendirme ve performans sınırlayın.

## <a name="cache-aggressively"></a>Agresif bir biçimde önbelleğe alma

Önbelleğe alma, bu belgenin birden fazla bölümde ele alınmıştır. Daha fazla bilgi için bkz. <xref:performance/caching/response>.

## <a name="avoid-blocking-calls"></a>Çağrıları engellemekten kaçınacak

ASP.NET Core uygulamaları aynı anda birçok istekleri işlemek için tasarlanmış olmalıdır. Zaman uyumsuz API'leri çağrıları engellemeyi beklenmiyor tarafından binlerce eş zamanlı istekleri işlemek için iş parçacığı oluşan küçük bir havuz sağlar. Tamamlanması uzun süre çalışan zaman uyumlu görevde beklemek yerine, iş parçacığı başka bir istek üzerinde çalışabilir.

ASP.NET Core uygulamalarında ortak bir performans sorunu, zaman uyumsuz çağrılar engelliyor. Çoğu zaman uyumlu engelleme çağrıları neden [iş parçacığı havuzu starvation](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) ve yanıt sürelerini düşürülmüş.

**Sağlamadığı**:

* Zaman uyumsuz yürütme engelleme çağırarak [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) veya [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Ortak kod yollarını kilitler edinin. ASP.NET Core, çoğu kod paralel olarak çalıştırmak için tasarlanmış, yüksek performanslı uygulamalardır.

**Yapmak**:

* Olun [sık erişimliye kod yollarını](#hot) zaman uyumsuz.
* Veri erişimi ve uzun süre çalışan işlemleri API zaman uyumsuz olarak çağırın.
* Denetleyici/Razor sayfa eylemleri zaman uyumsuz olarak yapın. Bütün çağrı yığını sayesinde bir avantaj elde için zaman uyumsuz [async/await](/dotnet/csharp/programming-guide/concepts/async/) desenleri.

Profil Oluşturucu, bir gibi [PerfView](https://github.com/Microsoft/perfview), sık eklenen iş parçacıklarını bulmak için kullanılan [iş parçacığı havuzu](/windows/desktop/procthread/thread-pools). `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` Olay iş parçacığı havuzuna eklenmiş bir iş parçacığı gösterir. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>Büyük nesne ayırma simge durumuna küçült

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core -->
[.NET Core çöp toplayıcı](/dotnet/standard/garbage-collection/) ayırma ve serbest bırakma bellek ASP.NET Core uygulamaları otomatik olarak yönetir. Otomatik çöp toplama genellikle geliştiriciler nasıl veya ne zaman bellek serbest bırakılır hakkında endişelenmeniz gerekmez anlamına gelir. Böylece geliştiriciler ayırma nesneler en aza indirmeniz gerekir ancak başvurulmayan nesnelerin temizlenmesi CPU süresi alan [sık erişimliye kod yollarını](#hot). Çöp toplama, özellikle büyük nesneler (> 85 K bayt) pahalıdır. Büyük nesneler üzerinde depolanan [büyük nesne yığını](/dotnet/standard/garbage-collection/large-object-heap) ve (2. nesil) tam çöp toplama temizlemek için. Nesil 0 ve 1. nesil koleksiyonlar farklı olarak, 2. nesil koleksiyonu geçici bir uygulamanın yürütülmesini askıya alınması gerekir. Sık ayırmayı ve ayırmayı kaldırma büyük nesnelerin yetersiz performansa neden olabilir.

Öneriler:

* **Yapmak** sık kullanılan büyük nesneleri önbelleğe almayı düşünün. Büyük nesnelerin önbelleğe alma, pahalı ayırmaları engeller.
* **Yapmak** kullanarak arabellek havuzu bir [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) büyük dizileri depolamak için.
* **Sağlamadığı** birçok, kısa süreli büyük nesneler şirket ayrılamadı [sık erişimliye kod yollarını](#hot).

Bellek sorunları, önceki örneğin atık toplama (GC) istatistikleri de gözden geçirerek tanı koydu [PerfView](https://github.com/Microsoft/perfview) inceleyerek:

* Çöp toplama duraklatma süresi.
* Yüzde işlemci zamanı, çöp toplama harcanır.
* Kaç çöp koleksiyonları kuşak 0, 1 ve 2 ' dir.

Daha fazla bilgi için [atık toplama ve performans](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Veri erişimini iyileştirmek

Bir veri deposu ve diğer uzak Hizmetleri ile etkileşim genellikle en yavaş bir ASP.NET Core uygulaması bölümlerdir. Verimli veri yazma ve okuma için iyi bir performans önemlidir.

Öneriler:

* **Yapmak** tüm veri erişimi API'leri zaman uyumsuz olarak çağırın.
* **Sağlamadığı** gerekli olandan daha fazla veri alın. Geçerli HTTP isteği için gerekli olan verileri döndürmek için sorgular yazarsınız.
* **Yapmak** biraz güncelliğini yitirmiş verileri kabul edilebilir olup olmadığını bir veritabanı veya uzak hizmetinden alınan verileri erişilen sık önbelleğe almayı düşünün. Senaryoya bağlı olarak kullanan bir [MemoryCache](xref:performance/caching/memory) veya [DistributedCache](xref:performance/caching/distributed). Daha fazla bilgi için bkz. <xref:performance/caching/response>.
* **Yapmak** en aza gidiş dönüş ağ. Çeşitli çağrılar yerine tek bir çağrı gerekli verileri almak üzere hedeftir.
* **Yapmak** kullanın [Hayır izleme sorguları](/ef/core/querying/tracking#no-tracking-queries) salt okunur amacıyla verilere erişirken Entity Framework Core içinde. EF Core Hayır izleme sorguların sonuçlarını daha verimli bir şekilde döndürebilirsiniz.
* **Yapmak** filtre ve toplama LINQ sorguları (ile `.Where`, `.Select`, veya `.Sum` deyimleri, örneğin) ve böylece filtreleme işlemi veritabanı tarafından gerçekleştirilir.
* **Yapmak** EF Core bazı sorgu işleçleri verimsiz sorgu yürütülmesine neden olabilir istemcide çözümler göz önünde bulundurun. Daha fazla bilgi için [istemci değerlendirme performans sorunlarını](/ef/core/querying/client-eval#client-evaluation-performance-issues).
* **Sağlamadığı** "N + 1" yürütülmesi sonucunda koleksiyonlarda yansıtma sorguları kullanmak SQL sorguları. Daha fazla bilgi için [bağıntılı alt sorgularda en iyi duruma getirilmesi](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Bkz: [EF yüksek performanslı](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) büyük ölçekli uygulamalarda performansı iyileştirebilir yaklaşımlar için:

* [DbContext havuzu](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Açıkça derlenmiş sorgular](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Kod tabanının gerçekleştirmeden önce önceki yüksek performanslı yaklaşımları etkisini ölçülmesine öneririz. Derlenmiş sorgular ek karmaşıklığını performans artışını Yasla değil.

Sorgu zaman inceleyerek sorunları algılanamıyor harcanan erişen verilerle [Application Insights](/azure/application-insights/app-insights-overview) veya profil oluşturma araçları ile. Çoğu veritabanı istatistikleri de kullanılabilir sık yürütülen sorgular ilgili olun.

## <a name="pool-http-connections-with-httpclientfactory"></a>Havuz HTTP bağlantılarıyla HttpClientFactory

Ancak [HttpClient](/dotnet/api/system.net.http.httpclient) uygulayan `IDisposable` arabirimi, yeniden kullanılmak üzere tasarlanmıştır. Kapalı `HttpClient` örnekleri yuva açık bırakın `TIME_WAIT` kısa bir süre için durum. Oluşturan ve siler, bir kod yolu varsa `HttpClient` nesneler sık kullanılan, uygulamanın kullanılabilir yuva tüketebilir. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) ASP.NET Core 2.1 içinde bu soruna bir çözüm olarak sunulmuştur. Bu, performansı ve güvenilirliği iyileştirmek için havuzu HTTP bağlantılarını işler.

Öneriler:

* **Sağlamadığı** oluşturun ve elden `HttpClient` doğrudan örnekler.
* **Yapmak** kullanın [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) alınacak `HttpClient` örnekleri. Daha fazla bilgi için [dayanıklı HTTP isteklerini uygulamak için kullanım HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Ortak kod yollarını hızlı tutun

En iyi duruma getirmek için en önemli olan tüm kod yolları hızlı, sık çağrılması için kodunuzu istediğiniz:

* Ara yazılım bileşenleri uygulamanın istek işleme ardışık düzeninde özellikle ara yazılımı erken işlem hattında çalıştırın. Bu bileşenlerin performans üzerinde büyük etkiye sahip.
* Her istek için veya birden çok kez istek başına yürütülen kod. Örneğin, özel günlük kaydı, yetkilendirme işleyicileri veya geçici Hizmetleri başlatma.

Öneriler:

* **Sağlamadığı** özel bir ara yazılım bileşenleri ile uzun süre çalışan görevleri kullanın.
* **Yapmak** performans profil oluşturma araçları, aşağıdaki gibi kullanın [Visual Studio tanılama araçları](/visualstudio/profiling/profiling-feature-tour) veya [PerfView](https://github.com/Microsoft/perfview)) tanımlamak için [sık erişimliye kod yollarını](#hot).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>HTTP istekleri dışında görevler uzun süreli tamamlayın

ASP.NET Core uygulaması için en çok istekte bir denetleyici veya gerekli hizmetleri çağırmak ve bir HTTP yanıtı döndüren sayfa modeli tarafından işlenebilir. Uzun süre çalışan görevleri içeren bazı istekler için tüm istek-yanıt işlemini zaman uyumsuz kolaylaştırmak iyidir.

Öneriler:

* **Sağlamadığı** sıradan HTTP istek işlemenin bir parçası olarak tamamlanması uzun süre çalışan görevler için bekleyin.
* **Yapmak** uzun süren istekleri işleme göz önünde bulundurun [arka plan Hizmetleri](xref:fundamentals/host/hosted-services) veya işlem dışında bir [Azure işlevi](/azure/azure-functions/). İş dışı işlem Tamamlanıyor, CPU yoğunluklu görevler için özellikle yararlıdır.
* **Yapmak** gibi gerçek zamanlı iletişim seçenekleri kullanın [SignalR](xref:signalr/introduction), zaman uyumsuz olarak istemcilerle iletişim kurmak için.

## <a name="minify-client-assets"></a>İstemci varlıklar küçültün

ASP.NET Core uygulamaları karmaşık ön uç ile sık birçok JavaScript, CSS veya görüntü dosyaları işlevi görür. İlk yükleme istekleri performansını tarafından geliştirilebilir:

* Paketleme, birden çok dosyayı tek bir araya getiren.
* Küçültme, boşluk ve açıklamalar kaldırarak dosyaların boyutunu azaltır.

Öneriler:

* **Yapmak** kullanan ASP.NET Core'nın [yerleşik destek](xref:client-side/bundling-and-minification) paketleme ve küçültme istemci varlıklar için.
* **Yapmak** diğer üçüncü taraf araçları gibi düşünün [Web](https://webpack.js.org/), karmaşık istemci varlık yönetimi.

## <a name="compress-responses"></a>Yanıtları sıkıştırma

 Yanıt boyutu genellikle azaltma, uygulama yanıt verme hızını genellikle önemli ölçüde artırır. Yük boyutları azaltmak için bir uygulamanın yanıtları sıkıştırma yoludur. Daha fazla bilgi için [yanıt sıkıştırma](xref:performance/response-compression).

## <a name="use-the-latest-aspnet-core-release"></a>ASP.NET Core en son sürümü kullan

ASP.NET Core her yeni sürümü, performans iyileştirmeleri içerir. .NET Core ve ASP.NET Core iyileştirmeler, daha yeni sürümleri genellikle eski sürümleri daha iyi performans gösterir, anlamına gelir. Örneğin, .NET Core 2.1 gelen benefitted ve derlenmiş normal ifadeler için destek eklendi [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx). HTTP/2 desteği ASP.NET Core 2.2 eklendi. Bir öncelik performans ise ASP.NET Core geçerli sürümüne yükseltmeyi göz önünde bulundurun.

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>Özel durumları en aza indirin

Özel durumlar seyrek olmalıdır. Oluşturmak ve özel durumları yakalamak, diğer kod akış desenlerini göre yavaştır. Bu nedenle, özel durumlar programın normal akışını denetlemek için kullanılmamalıdır.

Öneriler:

* **Sağlamadığı** oluşturma ve yakalama özel durumlar normal program akışının bir araç özellikle kullanımı [sık erişimliye kod yollarını](#hot).
* **Yapmak** algılar ve bir özel durum neden olan koşulları işlemek için uygulamada mantığı içerir.
* **Yapmak** throw veya catch özel durumları için olağan dışı ya da beklenmeyen koşulları.

Uygulama tanılama araçları, Application Insights gibi performansını etkileyebilecek bir uygulamada sık karşılaşılan özel durumlar belirlemeye yardımcı olabilir.
