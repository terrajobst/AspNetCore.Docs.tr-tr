---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: ASP.NET MVC 4'te zaman uyumsuz metotlar kullanma | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, ücretsiz ve Web için Visual Studio Express 2012 kullanarak zaman uyumsuz bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 5eb1b6dc3e9166c1ecb8e0d1968e12e8fa07f985
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826406"
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>ASP.NET MVC 4'te zaman uyumsuz metotlar kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, zaman uyumsuz bir ASP.NET MVC Web uygulaması kullanılarak oluşturmaya ilişkin temel bilgileri sağlanır [Visual Studio Express 2012 için Web](https://www.microsoft.com/visualstudio/11), Microsoft Visual Studio ücretsiz bir sürümü olduğu. Ayrıca [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Github'da Bu öğretici için eksiksiz bir örnek sağlanır  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4 [denetleyicisi](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) sınıfı birlikte [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) bir nesne türü döndüren zaman uyumsuz eylem yöntemi yazmanızı sağlar [görev&lt;ActionResult&gt; ](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). .NET Framework 4 olarak adlandırılan bir zaman uyumsuz programlama konsepti sunulan bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) ve ASP.NET MVC 4'ü destekleyen [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Görevler tarafından temsil edilir **görev** türü ve ilgili türü [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) ad alanı. .NET Framework 4.5 ile bu zaman uyumsuz desteği geliştirir [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) çalışmak olun anahtar sözcükleri [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) önceki değerinden daha az karmaşık nesneler zaman uyumsuz yaklaşım. [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) anahtar sözcüğü, kod parçasını diğer bazı kod parçasına zaman uyumsuz olarak beklemesi belirten için söz dizimi toplu özellik. [Zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) anahtar sözcüğü, görev tabanlı zaman uyumsuz yöntemler olarak yöntemlerini işaretlemek için kullanabileceğiniz bir ipucu temsil eder. Birleşimi **await**, **zaman uyumsuz**ve **görev** nesnesi haline getirir, .NET 4.5 içinde zaman uyumsuz kod yazmayı sizin için çok daha kolay. Zaman uyumsuz yöntemler için yeni modeli denir *görev tabanlı zaman uyumsuz desen* (**DOKUNUN**). Bu öğreticide, zaman uyumsuz programlamayı kullanma konusunda biraz bilgili olduğunuz varsayılır [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) anahtar sözcükleri ve [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) ad alanı.

Kullanma hakkında daha fazla bilgi için [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) anahtar sözcükleri ve [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) ad alanı, aşağıdaki kaynaklara bakın.

- [Teknik İnceleme: .NET zaman uyumsuzluğu](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await ile ilgili SSS](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio zaman uyumsuz programlama](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  İstekleri iş parçacığı havuzu tarafından nasıl işlenir

Web sunucusunda ASP.NET isteklere hizmet vermek için kullanılan iş parçacığı havuzu .NET Framework tutar. Bir istek ulaştığında bu isteği işlemek için bir iş parçacığı havuzundaki gönderilir. İstek zaman uyumlu olarak işleniyorsa isteği işleyen istek sırasında meşgul işlenmekte olan ve iş parçacığı başka bir isteğin hizmet verilemiyor iş parçacığıdır.   
  
İş parçacığı havuzu çok meşgul iş parçacıkları tutabilecek kadar büyük hale getirilebilir çünkü bu bir sorun olmayabilir. Ancak, iş parçacığı havuzundaki iş parçacığı sayısı sınırlıdır (.NET 4.5 için en fazla 5000 varsayılandır). Uzun süreli istekler yüksek eşzamanlılık ile büyük uygulamalar için kullanılabilir tüm iş parçacıklarının meşgul olabilir. Bu durum, iş parçacığı starvation bilinir. Bu durum ulaşıldığında, web sunucusu istekleri sıralar. İstek sırası dolarsa, web sunucusu (Sunucu meşgul) HTTP 503 durumu istekleri reddeder. CLR iş parçacığı havuzu üzerinde yeni iş parçacığı eklemelerini sınırlamaları vardır. Eşzamanlılık yükselen ise (yani, web sitenizi aniden çok sayıda istek alabilirsiniz) ve tüm kullanılabilir istek iş parçacıkları arka uç çağrıları nedeniyle yüksek gecikme süresiyle meşgul, sınırlı bir iş parçacığı ekleme oranı düşük bir performans yanıt uygulamanızı hale getirebilirsiniz. Ayrıca, iş parçacığı havuzuna eklenen her yeni iş parçacığı (örneğin, 1 MB yığın bellek) yüke sahiptir. İş parçacığı havuzu .NET 4.5 varsayılan 5 en yüksek büyüdükçe burada hizmete yüksek gecikme çağrılar zaman uyumlu yöntemleri kullanarak bir web uygulaması, 000 iş parçacıkları yaklaşık 5 GB mümkün uygulama daha fazla bellek kullanılmasına neden olur aynı hizmet istekleri kullanma zaman uyumsuz yöntemler ve yalnızca 50 iş parçacığı. Zaman uyumsuz iş yaparken, bir iş parçacığı her zaman kullanıyorsunuz. Bir zaman uyumsuz web servisi isteği yaptığınız zaman, örneğin, ASP.NET arasında herhangi bir iş parçacığı kullanmaz **zaman uyumsuz** yöntem çağrısı ve **await**. Hizmet istekleri için iş parçacığı havuzu yüksek gecikme süresiyle kullanarak, bir büyük bellek Ayak izi ve sunucu donanımı kötü kullanımı neden olabilir.

## <a name="processing-asynchronous-requests"></a>Zaman uyumsuz istek işleme

Çok sayıda eş zamanlı istekleri başlangıç görür veya (burada eşzamanlılık aniden artırır) Yükselen bir yüke sahip bir web uygulamasında zaman uyumsuz web hizmeti çağrıları yapma, uygulamanın yanıt verme hızını artırır. Zaman uyumsuz isteği aynı zaman uyumlu bir isteği işlemek için gereken süre. Web hizmeti çağrısı, bir istek yapıyorsa tamamlamak iki saniye isteği alır, zaman uyumlu veya zaman uyumsuz olarak gerçekleştirilip gerçekleştirilmeyeceğini iki saniye gerektirir. Ancak zaman uyumsuz bir çağrı sırasında ilk isteği için beklediği sırada başka isteklere yanıt bir iş parçacığı bloke değildir. Bu nedenle, uzun süre çalışan işlemleri çağırmak birçok eş zamanlı istek olduğunda zaman uyumsuz istekler isteği sıraya alma ve iş parçacığı havuzu büyümesini engellemek.

## <a id="ChoosingSyncVasync"></a>  Zaman uyumlu veya zaman uyumsuz eylem yöntemlerini seçme

Bu bölümde, zaman zaman uyumlu veya zaman uyumsuz eylem yöntemini kullanmak yönergeleri listeler. Bunlar yalnızca yönergelerdir; tek tek zaman uyumsuz yöntemler performansı yardımcı olup olmadığını belirlemek için her bir uygulama inceleyin.

Genel olarak, zaman uyumlu metotlar için aşağıdaki koşulları kullanın:

- , Basit veya kısa süre çalışan işlemlerdir.
- Basitlik, verimliliği daha önemlidir.
- Öncelikle CPU işlemleri kapsamlı bir disk veya ağ yükünü ilgili işlemleri yerine işlemlerdir. CPU'ya bağlı işlemler üzerinde zaman uyumsuz eylem yöntemleri kullanılarak hiçbir avantaj sunar ve daha fazla ek yük oluşur.

  Genel olarak, aşağıdaki koşullar için zaman uyumsuz yöntemleri kullanın:

- Zaman uyumsuz yöntemler tüketilebilir Hizmetleri arıyoruz ve .NET 4.5 veya üzeri kullanıyorsanız.
- , Ağa bağlı veya miyim/O-bağlı CPU bağımlı yerine işlemlerdir.
- Paralellik kod basitliğinin daha önemlidir.
- Uzun süre çalışan isteğini iptal et kullanıcıların olanak sağlayan bir mekanizma sunmak istiyorsunuz.
- Ne zaman iş parçacıkları geçiş avantajı içerik anahtarının maliyetinden ağır. Zaman uyumlu yöntem, hiçbir iş yaparken ASP.NET istek iş parçacığında onay bekliyorsa genel olarak, bir yöntem zaman uyumsuz yapmanız. Arama zaman uyumsuz hale getirerek ASP.NET isteği iş parçacığı için web hizmeti isteğini tamamlamak beklerken hiçbir iş yapan durdu değil.
- Test engelleme işlemleri performans sitesini bir performans sorunu olduğunu ve IIS istek engelleme bu çağrılar için zaman uyumsuz yöntemler kullanarak hizmet gösterir.

  İndirilebilir örnek zaman uyumsuz eylem yöntemlerinin etkili bir şekilde nasıl kullanılacağını gösterir. Sağlanan örnek, .NET 4.5 kullanarak ASP.NET MVC 4'te zaman uyumsuz programlama basit bir gösterimini sağlamak için tasarlanmıştır. Örnek ASP.NET mvc'de zaman uyumsuz programlama için bir başvuru mimarisini olacak şekilde tasarlanmamıştır. Örnek program çağrıları [ASP.NET Web API](../../../web-api/index.md) sırayla çağırın yöntemleri [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) uzun süre çalışan web hizmeti çağrıları benzetimini yapmak için. Üretim uygulamalarının çoğu zaman uyumsuz eylem yöntemleri kullanarak açık tür avantajlar göstermez.   
  
Bazı uygulamalar, zaman uyumsuz olarak tüm eylem yöntemlerine gerektirir. Genellikle, zaman uyumsuz yöntemler için birkaç eşzamanlı eylem yöntemleri dönüştürme gereken iş miktarı en iyi verimliliği artırma sağlar.

## <a id="SampleApp"></a>  Örnek uygulama

Örnek uygulamayı indirebilirsiniz [ https://github.com/RickAndMSFT/Async-ASP.NET/ ](https://github.com/RickAndMSFT/Async-ASP.NET) üzerinde [GitHub](https://github.com/) site. Depo üç projelerin oluşur:

- *Mvc4Async*: Bu öğreticide kullanılan kod içeren bir ASP.NET MVC 4 projesi. Web API çağrıları yapan **WebAPIpgw** hizmeti.
- *WebAPIpgw*: uygulayan ASP.NET MVC 4 Web API projesi `Products, Gizmos and Widgets` denetleyicileri. İçin veri sağlar *WebAppAsync* proje ve *Mvc4Async* proje.
- *WebAppAsync*: ASP.NET Web formları projesi başka bir öğreticide kullanılır.

## <a id="GizmosSynch"></a>  Şeyler zaman uyumlu bir eylem yöntemi

 Aşağıdaki kodda gösterildiği `Gizmos` en listesi görüntülemek için kullanılan zaman uyumlu bir eylem yöntemi. (Bu makalede hayali bir mekanik cihaz bir gizmo içindir.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Aşağıdaki kodda gösterildiği `GetGizmos` gizmo hizmetinin yöntemi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

`GizmoService GetGizmos` Yöntemi bir URI şeyler veri listesini döndüren bir ASP.NET Web API HTTP hizmetine iletir. *WebAPIpgw* projesini içeren Web API uygulamasını `gizmos, widget` ve `product` denetleyicileri.  
Örnek Proje şeyler görünümünden aşağıdaki resimde gösterilmektedir.

![Şeyler](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Bir şeyler zaman uyumsuz eylem yöntemi oluşturma

Yeni örnek kullanır [zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) ve [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) anahtar sözcükleri (.NET 4.5 ve Visual Studio 2012'de kullanılabilir) karmaşık dönüştürmeleri için gerekli bakımından sorumlu derleyici izin vermek için zaman uyumsuz programlama. Derleyici C# ' nin zaman uyumlu denetim akışı yapılarını kullanılarak kod yazmanıza olanak veren ve derleyici iş parçacıkları engellenmesini önlemek için geri çağırmaları kullanmak gerekli dönüştürmeleri otomatik olarak uygular.

Aşağıdaki kodda gösterildiği `Gizmos` zaman uyumlu yöntem ve `GizmosAsync` zaman uyumsuz yöntem. Tarayıcınız destekliyorsa [HTML 5 `<mark>` öğesi](http://www.w3.org/wiki/HTML/Elements/mark), değişiklikleri görürsünüz `GizmosAsync` sarı Vurgu içinde.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 İzin vermek için aşağıdaki değişiklikleri uygulandı `GizmosAsync` zaman uyumsuz olması.

- Yöntem ile işaretlenmiş [zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) derleyiciye geri çağırmaları gövde bölümlerinin oluşturun ve otomatik olarak oluşturmak için anahtar sözcüğü bir `Task<ActionResult>` döndürülen.
- &quot;Zaman uyumsuz&quot; yöntem adına eklenmiştir. "Async" ekleyerek gerekli değildir, ancak zaman uyumsuz yöntemler yazarken kuralıdır.
- Dönüş türü değiştirildi `ActionResult` için `Task<ActionResult>`. Dönüş türünü `Task<ActionResult>` devam eden çalışmayı temsil eder ve bir işleyicisi üzerinden zaman uyumsuz işlemin tamamlanmasını bekle yapılacağı yöntem sağlar. Bu durumda, çağıranın web hizmetidir. `Task<ActionResult>` devam eden temsil eder bir sonucu ile çalışma `ActionResult.`
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) anahtar sözcüğü, web hizmeti çağrısı uygulandı.
- Zaman uyumsuz web hizmeti API'si çağrıldı (`GetGizmosAsync`).

İçine `GetGizmosAsync` yöntemi başka bir zaman uyumsuz yöntem, gövde `GetGizmosAsync` çağrılır. `GetGizmosAsync` hemen döndüren bir `Task<List<Gizmo>>` , sonunda tamamlanır veriler kullanılabilir olduğunda. Gizmo veri kadar başka bir şey yapmak istemediğiniz için kod görev bekler (kullanarak **await** anahtar sözcüğü). Kullanabileceğiniz **await** anahtar sözcüğü ile açıklanan yöntemleriyle **zaman uyumsuz** anahtar sözcüğü.

**Await** anahtar sözcüğü görev tamamlanana kadar iş parçacığını engellemez. Yöntemin geri kalanını görevi üzerinde bir geri arama olarak imzalar ve hemen döndürür. Beklenen görev sonunda tamamlandığında, bu geri çağırma ve bu nedenle yöntemi sağındaki kaldığı yerden sürdürebilir. Kullanma hakkında daha fazla bilgi için [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) ve [zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) anahtar sözcükleri ve [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) ad bkz [zaman uyumsuz başvuruları](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Aşağıdaki kodda gösterildiği `GetGizmos` ve `GetGizmosAsync` yöntemleri.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Zaman uyumsuz değişiklikleri yapılan kişilere benzer **GizmosAsync** yukarıda. 

- Yöntem imzası ile ek açıklamalı [zaman uyumsuz](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) anahtar sözcüğü, dönüş türü değiştirildi `Task<List<Gizmo>>`, ve *zaman uyumsuz* yöntem adına eklenmiştir.
- Zaman uyumsuz [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) sınıfı yerine kullanılır [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) sınıfı.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) anahtar sözcüğü uygulanmıştır [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) zaman uyumsuz yöntemler.

Aşağıdaki görüntüde zaman uyumsuz gizmo görünümü gösterir.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

Tarayıcılar sunu şeyler verilerin eş zamanlı çağrı tarafından oluşturulan görünüm aynıdır. Tek fark, zaman uyumsuz sürümü ağır yük altında daha fazla performansa sahip olabilir.

## <a id="Parallel"></a>  Paralel olarak birden çok işlem gerçekleştirme

Bir eylemi birden fazla bağımsız işlemler gerçekleştirmesi gereken durumlarda önemli bir avantaj üzerinden zaman uyumlu metotları zaman uyumsuz eylem yöntemi vardır. Sağlanan örnek zaman uyumlu yöntem içinde `PWG`(ürünleri, pencere öğeleri ve şeyler), ürünler, pencere öğeleri ve şeyler listesini almak için üç web hizmeti çağrıları sonuçlarını görüntüler. [ASP.NET Web API](../../../web-api/index.md) bu sağlayan proje hizmetleri kullanan [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) gecikme veya yavaş ağ benzetimini yapmak için çağırır. Gecikme aralığını 500 milisaniyenin için zaman uyumsuz ayarlandığında `PWGasync` yöntem zaman uyumlu sırasında tamamlanması biraz 500 milisaniye alır `PWG` sürüm 1.500 milisaniye cinsinden alır. Zaman uyumlu `PWG` yöntemi, aşağıdaki kodda gösterilmiştir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Zaman uyumsuz `PWGasync` yöntemi, aşağıdaki kodda gösterilmiştir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

Aşağıdaki görüntüde, döndürülen görüntüler **PWGasync** yöntemi.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>  Bir iptal belirteci kullanma

Zaman uyumsuz eylem yöntemleri `Task<ActionResult>`yapabilecekleri olduğundan iptal edilebilir, olan bir [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) ile sağlandığında parametresi [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) özniteliği. Aşağıdaki kodda gösterildiği `GizmosCancelAsync` yöntemi ile 150 milisaniyelik zaman aşımı.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Alan GetGizmosAsync aşırı yüklemesi, aşağıdaki kodda gösterildiği bir [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametresi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

Sağlanan örnek uygulamada seçerek *iptal belirteci tanıtım* bağlama çağrıları `GizmosCancelAsync` yöntemi zaman uyumsuz çağrı olabildiği gösterir.

## <a id="ServerConfig"></a>  Yüksek eşzamanlılık/yüksek gecikme süresi Web hizmeti çağrıları için sunucu yapılandırması

Bir zaman uyumsuz web uygulamasının avantajlardan faydalanmak için varsayılan sunucu yapılandırmasına bazı değişiklikler yapmanız gerekebilir. Aşağıdaki yapılandırırken unutmayın ve zaman uyumsuz web uygulamanızı stres tutun.

- Windows 7, Windows Vista ve tüm Windows istemci işletim sistemleri en fazla 10 eşzamanlı istek var. Yüksek yük altında zaman uyumsuz yöntemler avantajlarını görmek için bir Windows Server işletim sistemi gerekir.
- .NET 4.5, IIS ile yükseltilmiş bir komut isteminden kaydedin:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
  Bkz: [ASP.NET IIS Kayıt Aracı (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Artırmanız gerekebilir [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) kuyruk sınırından 1000 ila 5.000 varsayılan değeri. Çok düşük bir ayardır, görebileceğiniz [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) istekler HTTP 503 durumundaki reddet. HTTP.sys kuyruk sınırı değiştirmek için:

    - IIS Yöneticisi'ni açın ve uygulama havuzlarının bölmesine gidin.
    - Hedef uygulama havuzu üzerinde sağ tıklatın ve seçin **Gelişmiş ayarlar**.  
        ![Gelişmiş](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - İçinde **Gelişmiş ayarlar** iletişim kutusunda, değişiklik *kuyruk uzunluğu* 5.000 için 1000'den.  
        ![Kuyruk uzunluğu](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Not Yukarıdaki görüntüde uygulama havuzu .NET 4.5 kullanıyor olsa bile, .NET framework v4.0 listelenir. Bu tutarsızlık anlamak için aşağıdakilere bakın:

    - [.NET sürüm oluşturma ve Multi-Targeting'e - .NET 4.5 olduğu .NET 4.0 için yerinde yükseltme](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Bir IIS uygulama veya AppPool 2.0 yerine ASP.NET 3.5 kullanacak şekilde ayarlama](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [.NET framework sürümleri ve bağımlılıkları](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Uygulamanız, web hizmetlerini kullanarak veya HTTP üzerinden arka ucunuzla iletişim için System.NET artırmanız gerekebilir [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) öğesi. ASP.NET uygulamaları için bu otomatik yapılandırma özelliği CPU sayısını 12 kat sınırlıdır. Bir dört proc üzerinde en fazla 12 olabileceği anlamına \* 4 = 48 bir IP uç noktası için eş zamanlı bağlantı. Bu bağlıdır çünkü [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), artırmak için en kolay yolu `maxconnection` bir ASP.NET uygulaması ayarlamaktır [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programlı olarak gelen `Application_Start` yönteminde *global.asax* dosya. Bir örnek için indirme örneğine bakın.
- .NET 4.5, 5000 için varsayılan olarak [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) ince olmalıdır.
