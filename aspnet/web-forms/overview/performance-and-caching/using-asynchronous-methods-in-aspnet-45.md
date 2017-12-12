---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: "ASP.NET 4.5 içinde zaman uyumsuz yöntemlerle | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici ücretsiz bir Web için Visual Studio Express 2012 kullanarak zaman uyumsuz bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 62a32db0984cfc2a1f5fd8f9196aad9259d74f93
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>ASP.NET 4.5 içinde zaman uyumsuz yöntemler kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici zaman uyumsuz bir ASP.NET Web Forms uygulaması kullanılarak oluşturmaya temellerini öğretmek [için Visual Studio Express 2012 Web](https://www.microsoft.com/visualstudio/11/en-us), Microsoft Visual Studio ücretsiz sürümünü olduğu. Aynı zamanda [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us). Aşağıdaki bölümlerde, bu öğreticide dahil edilir.
> 
> - [İstekleri iş parçacığı havuzu tarafından nasıl işlenir](#HowRequestsProcessedByTP)
> - [Zaman uyumlu veya zaman uyumsuz yöntemleri seçme](#ChoosingSyncVasync)
> - [Örnek uygulama](#SampleApp)
> - [En zaman uyumlu sayfası](#GizmosSynch)
> - [Zaman uyumsuz en sayfası oluşturma](#CreatingAsynchGizmos)
> - [Paralel olarak birden çok işlemlerini gerçekleştirme](#Parallel)
> - [Bir iptal belirteci kullanma](#CancelToken)
> - [Yüksek eşzamanlılık/yüksek gecikme Web hizmeti çağrıları için sunucu yapılandırması](#ServerConfig)
> 
> Bu öğreticinin için tam bir örnek sağlanır  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) üzerinde [GitHub](https://github.com/) site.


ASP.NET 4.5 Web sayfaları birlikte [.NET 4.5](https://msdn.microsoft.com/en-us/library/w0x726c2(VS.110).aspx) bir nesne türü döndüren zaman uyumsuz yöntemler kaydetmenizi sağlayan [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx). .NET Framework 4 olarak adlandırılan bir zaman uyumsuz programlama kavram sunulan bir [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) ve ASP.NET 4.5 destekler [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx). Görevler tarafından gösterilen **görev** türü ve ilgili türlerinde [System.Threading.Tasks](https://msdn.microsoft.com/en-us/library/system.threading.tasks.aspx) ad alanı. Bu zaman uyumsuz desteği ile .NET Framework 4.5 derlemeler [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) çalışmak olun anahtar sözcükleri [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) daha önceki daha az karmaşık nesneleri zaman uyumsuz yaklaşımlar. [Await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) , paylaştırılabilen bir kod zaman uyumsuz olarak diğer bazı kod parçasına beklemesi gerektiğini belirten için söz dizimi toplu bir anahtardır. [Zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) anahtar sözcüğü yöntemleri görev tabanlı zaman uyumsuz yöntemleri olarak işaretlemek için kullanabileceğiniz bir ipucu temsil eder. Birleşimi **await**, **zaman uyumsuz**ve **görev** nesne sağlar, .NET 4.5 içinde zaman uyumsuz kod yazmak çok daha kolay. Zaman uyumsuz yöntemleri için yeni model adlı *görev tabanlı zaman uyumsuz desen* (**DOKUNUN**). Bu öğretici, zaman uyumsuz programing kullanma konusunda biraz bilgili varsayar [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) anahtar sözcükleri ve [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) ad alanı.

Kullanma hakkında daha fazla bilgi için [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) anahtar sözcükleri ve [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) ad alanı, aşağıdaki kaynaklara bakın.

- [Teknik İnceleme: Asynchrony .NET içinde](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Zaman uyumsuz/bekleme SSS](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio zaman uyumsuz programlama](https://msdn.microsoft.com/en-us/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>İstekleri iş parçacığı havuzu tarafından nasıl işlenir

Web sunucusunda .NET Framework ASP.NET isteklere hizmet vermek için kullanılan iş parçacığı havuzu tutar. Bir istek ulaştığında, bir iş parçacığı havuzundaki bu isteği işlemek için gönderilir. İstek zaman uyumlu olarak işlenir, isteği işleyen iş parçacığı isteği sırasında meşgul işlenmektedir ve iş parçacığı başka bir isteğe hizmet veremiyor olur.   
  
İş parçacığı havuzu pek çok meşgul iş parçacığı uyabilecek kadar büyük hale getirilebilir çünkü bu bir sorun olmayabilir. Ancak, iş parçacığı havuzu iş parçacıkları sayısı sınırlıdır (.NET 4.5 için en fazla 5000 varsayılandır). Uzun süre çalışan istekler yüksek eşzamanlılık ile büyük uygulamalar tüm kullanılabilir iş parçacıklarının meşgul olabilir. Bu durum, iş parçacığı yetersizliğini bilinir. Bu durum ulaşıldığında, web sunucusu isteklerinin sıralar. İstek sırası dolduğunda, web sunucusu (Sunucu meşgul) HTTP 503 durumuna sahip istekleri reddeder. CLR iş parçacığı havuzu üzerinde yeni iş parçacığı eklemelerini sınırlamaları vardır. Eşzamanlılık bursty ise (diğer bir deyişle, web sitenizi aniden çok sayıda istek alabilirsiniz) ve tüm kullanılabilir isteği iş parçacığı arka uç çağrıları nedeniyle yüksek gecikme süresiyle meşgul, sınırlı iş parçacığı ekleme oranı çok kötü yanıt uygulamanızı yapabilirsiniz. Ayrıca, iş parçacığı havuzuna eklenen her yeni iş parçacığı (örneğin, 1 MB yığın bellek) yüke sahiptir. İş parçacığı havuzu .NET 4.5 varsayılan olarak 5 maksimum büyür nerede hizmet yüksek gecikme çağrıları için zaman uyumlu yöntemleri kullanarak bir web uygulaması uygulamanın mümkün daha fazla bellek yaklaşık 5 GB 000 iş parçacığı kullanır aynı hizmet istekleri kullanma zaman uyumsuz yöntemleri ve yalnızca 50 iş parçacığı sayısı. Zaman uyumsuz iş yaparken, her zaman bir iş parçacığı kullanıyorsunuz. Zaman uyumsuz web hizmeti isteğine yaptığınızda, örneğin, ASP.NET tüm iş parçacıkları arasında kullanmadığınız **zaman uyumsuz** yöntem çağrısı ve **await**. Yüksek gecikme süresine sahip isteklere hizmet iş parçacığı havuzu kullanma büyük bellek kaplama alanı ve sunucu donanımı zayıf kullanımı için yol açabilir.

## <a name="processing-asynchronous-requests"></a>Zaman uyumsuz istek işleme

(Burada eşzamanlılık aniden artırır) bursty yükü çok sayıda eş zamanlı istekleri başlatma bakın veya web uygulamalarında zaman uyumsuz web hizmeti çağrıları yapma, uygulamanızın yanıt hızını artırır. Zaman uyumsuz bir istek aynı zaman uyumlu bir isteği işlemek için gereken süre. Örneğin, bir istek bir web hizmeti, çağrı yaparsa tamamlamak, isteği alır iki saniye eşzamanlı veya zaman uyumsuz olarak gerçekleştirilen olup olmadığını iki saniye gerektirir. Ancak, bir zaman uyumsuz çağrı sırasında ilk istek tamamlanması için beklerken diğer isteklere yanıt iş parçacığı engellenmez. Bu nedenle, uzun süre çalışan işlemleri çağırma çok sayıda eş zamanlı istekleri olduğunda zaman uyumsuz istekleri isteği sıraya alma ve iş parçacığı havuzu büyümesini engellemek.

## <a id="ChoosingSyncVasync"></a>Zaman uyumlu veya zaman uyumsuz yöntemleri seçme

Bu bölümde, zaman zaman uyumlu veya zaman uyumsuz yöntemler kullanmayı yönergeleri listelenir. Bunlar yalnızca yönergelerdir; tek tek zaman uyumsuz yöntemleri ile performansı Yardım olup olmadığını belirlemek için her bir uygulama inceleyin.

Genel olarak, aşağıdaki koşullar için zaman uyumlu yöntemleri kullanın:

- , Basit veya kısa süreli işlemleridir.
- Basitlik verimliliği daha önemlidir.
- Öncelikle CPU işlemleri kapsamlı disk veya ağ yükünü içeren işlemleri yerine işlemleridir. CPU bağımlı işlemlerini zaman uyumsuz yöntemlerle hiçbir yararları sağlar ve daha fazla bilgi için ek yükü sonuçlanır.

 Genel olarak, aşağıdaki koşullar için zaman uyumsuz yöntemleri kullanın:

- Zaman uyumsuz yöntemlerle tüketilebilir Hizmetleri arıyoruz ve .NET 4.5 veya üstünü kullanıyorsanız.
- , Ağ sınırını veya g/O-bağlı CPU bağımlı yerine işlemleridir.
- Paralellik kod kolaylık daha önemlidir.
- Uzun süre çalışan isteği iptal kullanıcıların olanak sağlayan bir mekanizma sağlamak istediğinizde.
- Ne zaman iş parçacığı çıkış geçiş avantajı, içerik anahtarı maliyetini ağırlık verir. Zaman uyumlu yöntemi hiçbir iş yaparken ASP.NET isteği iş parçacığı engelliyorsa genel olarak, bir yöntem zaman uyumsuz yapmanız. Arama zaman uyumsuz hale getirerek, ASP.NET isteği iş parçacığı için web hizmeti isteği tamamlamak beklerken hiçbir çalışarak engellenmez.
- Sınama engelleme işlemleri site performans bir performans sorunu olduğunu ve IIS daha fazla isteği bu engelleme çağrıları için zaman uyumsuz yöntemleri kullanarak hizmet gösterir.

 İndirilebilir örnekteki zaman uyumsuz yöntemleri etkili bir şekilde kullanmayı gösterir. Sağlanan örnek ASP.NET 4.5 içinde zaman uyumsuz programlama basit Tanıtımı sağlamak için tasarlanmıştır. Örnek ASP.NET zaman uyumsuz programlama için bir başvuru mimarisi olması amaçlanmamıştır. Örnek program çağrıları [ASP.NET Web API](../../../web-api/index.md) daha sırayla çağıran yöntemleri [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) uzun süre çalışan web hizmeti çağrıları benzetimini yapmak için. Üretim uygulamaların çoğu zaman uyumsuz yöntemler kullanma gibi belirgin avantajları göstermez.   
  
Birkaç uygulamalar tüm yöntemleri zaman uyumsuz olmasını gerektirir. Genellikle, birkaç zaman uyumlu yöntemleri zaman uyumsuz yöntemleri dönüştürme gerekli iş miktarı için en iyi verimliliği artırma sağlar.

## <a id="SampleApp"></a>Örnek uygulama

Örnek uygulamayı indirebilirsiniz [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) üzerinde [GitHub](https://github.com/) site. Depo üç projelerin oluşur:

- *WebAppAsync*: Web API'sini kullanan ASP.NET Web Forms proje **WebAPIpwg** hizmet. Bu öğreticide bunu için kod çoğu projesi.
- *WebAPIpgw*: uygulayan ASP.NET MVC 4 Web API projesi `Products, Gizmos and Widgets` denetleyicileri. İçin veri sağlar *WebAppAsync* proje ve *Mvc4Async* projesi.
- *Mvc4Async*: başka bir öğreticide kullanılan kod içeren ASP.NET MVC 4 projesinin. Web API çağrıları yapan **WebAPIpwg** hizmet.

## <a id="GizmosSynch"></a>En zaman uyumlu sayfası

 Aşağıdaki kodda gösterildiği `Page_Load` en listesini görüntülemek için kullanılan zaman uyumlu yöntemi. (Bu makalede, bir gizmo bir kurgusal mekanik aygıttır.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Aşağıdaki kodda gösterildiği `GetGizmos` gizmo hizmetinin yöntemi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos` Yöntemi en veri listesi döndüren bir ASP.NET Web API HTTP hizmeti için bir URI geçirir. *WebAPIpgw* projeyi içeren Web API uygulaması `gizmos, widget` ve `product` denetleyicileri.  
Aşağıdaki resimde örnek proje en sayfasından gösterir.

![En](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Zaman uyumsuz en sayfası oluşturma

Örnek yeni [zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ve [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) anahtar sözcükler (.NET 4.5 ve Visual Studio 2012'de kullanılabilir) karmaşık dönüştürmeleri için gerekli tutmakla derleyici izin vermek için zaman uyumsuz programlama. Derleyici, C# ' nin zaman uyumlu denetim akışı yapıları kullanılarak kod yazmanıza olanak veren ve derleyici iş parçacıkları engelleme önlemek için geri çağırmaları kullanmak için gerekli dönüşümleri otomatik olarak uygular.

ASP.NET zaman uyumsuz sayfaları içermelidir [sayfa](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) ile yönerge `Async` özniteliği "true". Aşağıdaki kodda gösterildiği [sayfa](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) ile yönerge `Async` özniteliği "true" için *GizmosAsync.aspx* sayfası.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Aşağıdaki kodda gösterildiği `Gizmos` zaman uyumlu `Page_Load` yöntemi ve `GizmosAsync` zaman uyumsuz sayfası. Tarayıcınız destekliyorsa [HTML 5 &lt;işaretlemek&gt; öğesi](http://www.w3.org/wiki/HTML/Elements/mark), değişiklikleri görürsünüz `GizmosAsync` sarı Vurgu içinde.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Zaman uyumsuz sürümü:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Aşağıdaki değişiklikleri izin vermek için uygulanan `GizmosAsync` sayfa zaman uyumsuz.

- [Sayfa](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) yönergesi olmalıdır `Async` özniteliği "true".
- `RegisterAsyncTask` Yöntemi, zaman uyumsuz olarak çalışan bir kod içeren zaman uyumsuz bir görevi kaydetmek için kullanılır.
- Yeni `GetGizmosSvcAsync` yöntemi ile işaretlenmiş [zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) gövde bölümlerinin geri aramalar oluşturun ve otomatik olarak oluşturmak için derleyici söyler anahtar sözcüğü bir `Task` , döndürülür.
- &quot;Zaman uyumsuz&quot; zaman uyumsuz yöntem adına eklenmiştir. "Zaman uyumsuz" ekleyerek gerekli değildir ancak zaman uyumsuz yöntemleri yazarken kuraldır.
- Dönüş türü yeni yeni `GetGizmosSvcAsync` yöntemi `Task`. Dönüş türü `Task` devam eden iş temsil eder ve zaman uyumsuz işlemin tamamlanması için beklenecek içinden işleyici ile yöntemini arayanlar sağlar.
- [Await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) anahtar sözcüğü, web hizmeti çağrısı uygulandı.
- Zaman uyumsuz web hizmeti API'si çağrıldı (`GetGizmosAsync`).

İçinde `GetGizmosSvcAsync` yöntemi gövde başka bir zaman uyumsuz yöntem `GetGizmosAsync` olarak adlandırılır. `GetGizmosAsync`hemen döndüren bir `Task<List<Gizmo>>` , sonuç tamamlanacak veriler kullanılabilir olduğunda. Gizmo verileri elde edene kadar başka bir şey yapmak istemeyeceğiniz için kod görev bekler (kullanarak **await** anahtar sözcüğü). Kullanabileceğiniz **await** anahtar sözcüğü ile Açıklama yöntemler **zaman uyumsuz** anahtar sözcüğü.

**Await** anahtar sözcüğü görevi tamamlanana kadar iş parçacığı engellemez. Görev üzerinde geri arama olarak yöntemi rest imzalar ve hemen döndürür. Awaited görevi sonunda tamamlandığında, bu geri çağırma ve böylece kaldığı yerden yöntemi sağ yürütülmesini Sürdür. Kullanma hakkında daha fazla bilgi için [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) anahtar sözcükleri ve [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) ad alanı, bkz: [zaman uyumsuz başvurular](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Aşağıdaki kodda gösterildiği `GetGizmos` ve `GetGizmosAsync` yöntemleri.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Zaman uyumsuz değişiklikler için yapılan benzer **GizmosAsync** üstünde. 

- Yöntem imzası ile ek açıklama [zaman uyumsuz](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) anahtar sözcüğü, dönüş türü için değiştirildi `Task<List<Gizmo>>`, ve *zaman uyumsuz* yöntemi adına eklenmiştir.
- Zaman uyumsuz [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx) sınıfı zaman uyumlu yerine kullanılır [WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient.aspx) sınıfı.
- [Await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) anahtar sözcüğü için uygulandığı [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/en-us/library/hh158944(VS.110).aspx) zaman uyumsuz yöntem.

Aşağıdaki resimde zaman uyumsuz gizmo görünümü gösterir.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

En verilerinin tarayıcılar sunumu eşzamanlı çağrı tarafından oluşturulan görünüm aynıdır. Zaman uyumsuz sürümü ağır yük altında daha fazla kullanıcı olabilir yalnızca farktır.

## <a name="registerasynctask-notes"></a>RegisterAsyncTask Notlar

Yöntemleri kancalanmış ile `RegisterAsyncTask` hemen sonra çalışır [PreRender](https://msdn.microsoft.com/en-us/library/ms178472.aspx). Zaman uyumsuz void Sayfa olaylarının, doğrudan aşağıdaki kodda gösterildiği gibi kullanabilirsiniz:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Zaman uyumsuz void olayları dezavantajı geliştiriciler artık olayları yürütülürken üzerinde tam denetim olmasıdır. Örneğin, her iki .aspx varsa ve bir. Ana tanımlamak `Page_Load` olayları ve biri veya her ikisi de zaman uyumsuz, yürütme sırasını garanti edilemez. Aynı indeterminiate sipariş olmayan olay işleyicileri için (gibi `async void Button_Click` ) uygulanır. Çoğu geliştiriciler için bu kabul edilebilir olmalıdır, ancak yürütme sırasını üzerinde tam denetim gerektirir olanlar gibi API'leri yalnızca kullanmanız gerekir `RegisterAsyncTask` bir görev nesnesi döndürmesi yöntemleri kullanabilir.

## <a id="Parallel"></a>Paralel olarak birden çok işlemlerini gerçekleştirme

Bir eylem birkaç bağımsız işlemler gerçekleştirdiğinizde gerekir zaman uyumsuz yöntemleri zaman uyumlu yöntemleri önemli bir avantajı vardır. Sağlanan örnek zaman uyumlu sayfa *PWG.aspx*(ürünleri, pencere öğeleri ve en) ürünleri, pencere öğeleri ve en listesini almak için üç web hizmeti çağrıları sonuçlarını görüntüler. [ASP.NET Web API](../../../web-api/index.md) bu sağlar proje hizmetleri kullanan [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) gecikme veya yavaş ağ benzetimi için çağırır. Gecikme 500 milisaniye, zaman uyumsuz ayarlandığında *PWGasync.aspx* sayfa geçen zaman uyumlu sırasında tamamlanması biraz 500 milisaniye `PWG` sürüm 1500 milisaniye alır. Zaman uyumlu *PWG.aspx* sayfasında, aşağıdaki kodda gösterilir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Zaman uyumsuz `PWGasync` arka plan kod aşağıda gösterilmektedir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Aşağıdaki resimde zaman uyumsuz döndürülen görüntüler *PWGasync.aspx* sayfası.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Bir iptal belirteci kullanma

Döndüren zaman uyumsuz yöntemleri `Task`aldıkları olduğundan iptal edilebilen, olan bir [CancellationToken](https://msdn.microsoft.com/en-us/library/system.threading.cancellationtoken(VS.110).aspx) ile sağlandığında parametresi `AsyncTimeout` özniteliği [sayfa](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) yönergesi. Aşağıdaki kodda gösterildiği *GizmosCancelAsync.aspx* bir zaman aşımı süresi ikinci üzerinde sayfası.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Aşağıdaki kodda gösterildiği *GizmosCancelAsync.aspx.cs* dosya.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

Sağlanan örnek uygulama seçme *GizmosCancelAsync* bağlama çağrıları *GizmosCancelAsync.aspx* sayfasında ve zaman uyumsuz çağrı iptal edilmesine (tarafından zaman aşımına uğramadan) gösterir. Gecikme süresi rastgele bir aralık içinde olduğundan, zaman aşımı hata iletisini almak için birkaç kez sayfayı yenilemeniz gerekebilir.

## <a id="ServerConfig"></a>Yüksek eşzamanlılık/yüksek gecikme Web hizmeti çağrıları için sunucu yapılandırması

Bir zaman uyumsuz web uygulaması faydaları hayata geçirmek için varsayılan sunucu yapılandırma bazı değişiklikler yapmanız gerekebilir. Aşağıdaki yapılandırırken unutmayın ve zaman uyumsuz web uygulamanızı test etme stres tutun.

- Windows 7, Windows Vista, Windows 8 ve tüm Windows istemci işletim sistemleri en fazla 10 eşzamanlı istek var. Zaman uyumsuz yöntemleri yüksek yük altında faydalarını görmek için bir Windows Server işletim sistemi gerekir.
- .NET 4.5 aşağıdaki komutu kullanarak yükseltilmiş komut isteminden IIS'ye kaydedin:  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 Bkz: [ASP.NET IIS Kayıt Aracı (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx)
- Artırmanız gerekebilir [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) 1.000 ile 5.000 varsayılan değerinden kuyruk sınırı. Ayar çok düşük ise görebileceğiniz [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) HTTP 503 durumu olan istekleri reddedecek. HTTP.sys kuyruk sınırı değiştirmek için:

    - IIS Yöneticisi'ni açın ve uygulama havuzları bölmesine gidin.
    - Hedef uygulama havuzunda sağ tıklayın ve **Gelişmiş ayarları**.  
        ![Gelişmiş](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - İçinde **Gelişmiş ayarları** iletişim kutusu, değişiklik *sırası uzunluğu* 5.000 için 1000'den.  
        ![Sırası uzunluğu](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 Not Yukarıdaki görüntüleri uygulama havuzu .NET 4.5 kullanıyor olsa da, .NET framework v4.0 listelenir. Bu uyuşmazlık anlamak için aşağıdakilere bakın:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/en-us/library/bb822049(VS.110).aspx)
- Uygulamanızın web hizmetlerini kullanarak veya HTTP üzerinden arka ucuyla iletişim System.NET artırmak gerekebilir [connectionManagement/maxconnection](https://msdn.microsoft.com/en-us/library/fb6y0fyc(VS.110).aspx) öğesi. ASP.NET uygulamaları için bu CPU sayısı 12 kat otomatik yapılandırma özelliği sınırlıdır. Bir dört proc üzerinde en fazla 12 olabileceği anlamına \* 4 = 48 IP uç noktası için eş zamanlı bağlantı. Bu bağlıdır çünkü [autoConfig](https://msdn.microsoft.com/en-us/library/7w2sway1(VS.110).aspx), artırmak için en kolay yolu `maxconnection` bir ASP.NET uygulaması ayarlamaktır [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programlı olarak gelen `Application_Start` yönteminde *global.asax* dosya. Bir örnek için karşıdan örneğine bakın.
- .NET 4.5, 5000 için varsayılan olarak [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) ince olmalıdır.

## <a name="contributors"></a>Katkıda Bulunanlar

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Zel Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
