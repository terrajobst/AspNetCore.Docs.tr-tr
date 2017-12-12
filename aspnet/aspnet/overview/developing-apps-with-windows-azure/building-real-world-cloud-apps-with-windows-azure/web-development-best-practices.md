---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: "Web geliştirme en iyi yöntemler (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: a40a3779ddc416e141dd27b665f43830a43590b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web geliştirme en iyi yöntemler (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


İlk üç desenleri bir Çevik Geliştirme işlemi ayarlama vardı; rest mimarisi ve kod hakkında verilmiştir. Bu bir web geliştirme en iyi yöntemler koleksiyonudur:

- [Durum bilgisiz web sunucuları](#stateless) akıllı yük dengeleyici arkasında.
- [Oturum durumu kaçının](#sessionstate) (veya onu yoksayılamaz, veritabanı yerine dağıtılmış önbellek kullanın).
- [CDN kullanan](#cdn) kenar önbellek statik dosya varlıklara (görüntüleri, komut dosyaları).
- [.NET 4.5'in zaman uyumsuz destek kullanma](#async) çağrıları engellemekten kaçınacak şekilde.

Bu uygulamalar yalnızca bulut uygulamaları için tüm web geliştirme için geçerli olan, ancak özellikle bulut uygulamaları için önemli. Bunlar en iyi kullanımı bulut ortamı tarafından sunulan çok esnek ölçeklendirme yapmanıza yardımcı olmak için birlikte çalışır. Bu yöntemler takip etmiyorsa, uygulamanızın ölçeğini çalıştığınızda sınırlamaları çalıştıracaksınız.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Durum bilgisiz web katmanı akıllı yük dengeleyici arkasında

*Durum bilgisiz web katmanı* web sunucusu bellek veya dosya sisteminde uygulama verilerini depolamak yok anlamına gelir. Web katmanı durum bilgisiz tutma, hem daha iyi bir müşteri deneyimi sağlar ve paradan tasarruf sağlar:

- Web katmanı durum bilgisiz ise ve bir yük dengeleyicinin arkasına bulunur, hızlı bir şekilde uygulama trafiğini değişiklikleri dinamik olarak ekleyerek veya kaldırarak sunucuları yanıt verebilirsiniz. Bunları gerçekten kullandığınız sürece Burada, yalnızca sunucu kaynaklarını ödeme bulut ortamında talep değişikliklerine yanıt verme olanağı büyük tasarruflar çevirebilir.
- Durum bilgisiz web katmanı uygulamanın ölçeğini genişletmek mimari çok daha kolaydır. Çok daha hızlı ölçeklemeye gereksinimlerini yanıt ve geliştirme ve test etme işleminde küçük para harcamanız sağlar.
- Şirket içi sunucular gibi bulut sunucularına düzeltme eki ve bazen yeniden gerekir; ve web katmanı durum bilgisiz ise, bir sunucu geçici olarak devre dışı kaldığında trafiğin yeniden yönlendirilmesini hataları veya beklenmeyen davranışlara neden olmaz.

Çoğu gerçek uygulamalar için bir web oturum durumunu depolamak gerekir; Ana burada web sunucusunda depolamamayı noktasıdır. İstemcide tanımlama bilgilerini veya işlemi sunucu tarafı önbellek sağlayıcısını kullanarak ASP.NET oturum durumu gibi farklı yollarla durumu depolayabilirsiniz. Dosyaların depolanacağı [Windows Azure Blob Depolama](unstructured-blob-storage.md) yerine yerel dosya sistemi.

Web katmanı durum bilgisiz ise, bir uygulama Windows Azure Web sitelerini ölçeklendirir ne kadar kolay olduğunu bir örnek bkz **ölçek** sekmesinde bir Windows Azure Web sitesi için Yönetim Portalı'nda:

![Ölçek sekmesi](web-development-best-practices/_static/image1.png)

Web sunucuları eklemek istiyorsanız, yalnızca örnek sayısı kaydırıcıyı sağa sürükleyebilirsiniz. 5 olarak ayarlayın ve tıklayın **kaydetmek**, ve saniye içinde Windows Azure web sitenizin trafiği işleme 5 web sunucuları sahiptir.

![Beş örnekleri](web-development-best-practices/_static/image2.png)

Örnek sayısı 3 Aşağı kaydırın veya geri 1 aşağıya doğru kolayca ayarlayabilirsiniz. Geri ölçeklendirdiğinizde paradan tasarruf Başlat hemen Windows Azure olmayan saate göre dakikaya göre ücretlendirdiği.

Ayrıca, otomatik olarak artırın veya CPU kullanımına bağlı web sunucularının sayısını azaltmak için Windows Azure anlayabilirsiniz. Aşağıdaki örnekte, CPU kullanımı % 60 gittiğinde, web sunucularının sayısını en az 2 düşürür ve CPU kullanımı % 80 ' kalırsa, web sunucuları sayısı en fazla 4 kadar artırılır.

![CPU kullanımına göre ölçeklendirme](web-development-best-practices/_static/image3.png)

Veya ne sitenizi yalnızca çalışma saatleri içinde meşgul olacağını bildiğiniz? Birden çok sunucu sırasında daytime çalıştırmak ve tek bir sunucu nights, Akşamları ve hafta sonları azaltmak için Windows Azure anlayabilirsiniz. Aşağıdaki ekran görüntüleri bir dizi 8: 00'dan çalışma saatleri-17: 00 saatleri sırasında bir sunucuya dışı saatlerde ve 4 sunucuları çalıştırmak için web sitesi kurmak nasıl gösterir.

![Zamanlamaya göre ölçeklendirme](web-development-best-practices/_static/image4.png)

![Zamanlama saatleri ayarlayın](web-development-best-practices/_static/image5.png)

![Daytime zamanlama](web-development-best-practices/_static/image6.png)

![Weeknight zamanlama](web-development-best-practices/_static/image7.png)

![Hafta sonu zamanlama](web-development-best-practices/_static/image8.png)

Ve Elbette tüm komut dosyaları da portal olduğu gibi yapılabilir.

Dinamik olarak ekleme veya web katmanı durum bilgisiz tutarak server Vm'lerinin kaldırma engelleri kaçının sürece, uygulamanızın ölçeğini Windows Azure'da neredeyse sınırsız özelliğidir.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Oturum durumu kaçının

Bunu önlemek için bir kullanıcı oturum durumu çeşit depolamak için bir gerçek bulut uygulamasında pratik çoğunlukla değildir, ancak bazı yaklaşımlar diğerlerinden daha fazla performans ve ölçeklenebilirliği etkileyen. Durumunu depolamak varsa, durum miktarını küçük tutun ve tanımlama bilgilerini depolamak için en iyi çözüm olur. Uygun değilse, ASP.NET oturum durumu için bir sağlayıcı ile kullanmak için sonraki en iyi çözüm olduğunu [dağıtılmış, bellek içi önbellek](distributed-caching.md#sessionstate). Kötü bir performans ve ölçeklenebilirlik açısından bir veritabanını kullanmak için oturum durumu sağlayıcısı yedeklenen çözümüdür.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Statik dosya varlıklarını önbelleğe almak için bir CDN kullanma

CDN içerik teslim ağı için bir kısaltmasıdır. Bir CDN sağlayıcısına görüntüler ve komut dosyaları gibi statik dosya varlıklar sağlar ve böylece uygulamanız kişiler erişim olduğunda, oldukça hızlı yanıt ve düşük gecikme süresi için önbelleğe alınan elde sağlayıcı bu veri merkezlerinde dünyanın dosyaları önbelleğe varlıklar. Bu sitenin genel yükleme süresini hızlandırır ve web sunucuları üzerindeki yükü azaltır. CDN'ler yaygın coğrafi olarak dağıtılmış bir izleyici ulaşıyor durumunda özellikle önemlidir.

Windows Azure CDN sahiptir ve Windows Azure veya barındırma ortamı web çalışır bir uygulamada diğer CDN'ler kullanabilirsiniz.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>.NET 4.5'in zaman uyumsuz destek aramalarını engelleyip önlemek için kullanın

.NET 4.5 C# ve VB programlama dillerini görevleri zaman uyumsuz olarak işlemek daha kolay yapmak için geliştirilmiştir. Yalnızca aynı anda birden çok web hizmeti çağrıları devre dışı kazandırın istediğinizde gibi paralel işleme durumlar için zaman uyumsuz programlama yararı değil. Ayrıca daha verimli bir şekilde gerçekleştirmek, web sunucusu sağlar ve yüksek yük koşullarında güvenilir. Bir web sunucusu yalnızca sınırlı sayıda kullanılabilir iş parçacığına sahip ve tüm iş parçacıklarının kullanımda olduğunda yüksek yük koşullarında gelen istekleri yukarı serbest iş parçacığı kadar beklemek zorunda. Uygulama kodunuz zaman uyumsuz olarak veritabanı sorguları ve web hizmeti çağrıları gibi görevleri işleyemez, sunucunun bir g/ç yanıt bekleme sırasında birçok iş parçacığı gereksiz yere bağlıdır. Bu sunucunun yüksek yük koşullarında kabul edebileceği trafik miktarını sınırlar. Zaman uyumsuz programlama ile bir web hizmeti veya verileri döndürmek için veritabanı için bekleyen iş parçacığı kadar veri hizmeti yeni istekleri kadar kurtulurlar alınır. Meşgul web sunucusu, yüzlerce veya binlerce isteği sonra derhal, aksi takdirde yukarı boşaltılması için iş parçacığı bekliyor olabilir işlenebilir.

Daha önce gördüğünüz gibi bunları artırmak için olduğu gibi web sitenizi işleme web sunucularının sayısını azaltmak kadar kolaydır. Bu nedenle bir sunucu büyük verimi elde edebilirsiniz, aksi takdirde yaptığınız daha az sayıda sunucu verilen trafik hacmi için gerektiğinden bunları ve birçoğu, maliyetlerinizi düşürebilir gibi ihtiyacınız yoktur.

.NET 4.5 zaman uyumsuz programlama modeli için destek ASP.NET 4.5 Web formları, MVC ve Web API bulunmaktadır; Entity Framework 6 hem de [Windows Azure depolama API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>ASP.NET 4.5 içinde zaman uyumsuz desteği

ASP.NET 4.5 içinde zaman uyumsuz programlama dili için yalnızca aynı zamanda MVC, Web formları ve Web API çerçeveleri eklendi desteği. Örneğin, bir ASP.NET MVC denetleyici eylem yöntemi bir web isteğinden verileri alır ve tarayıcıya gönderilecek olan HTML oluşturan bir görünüme veri geçirir. Sık eylem yöntemi bir web sayfasında görüntülemek için veya bir web sayfasında girilen verileri kaydetmek için bir veritabanı veya web hizmetinden veri al gerekiyor. Bu senaryolarda eylem yöntemini zaman uyumsuz hale kolaydır.: döndürme yerine bir *ActionResult* nesnesi, dönmek *görev&lt;ActionResult&gt;*  ve yöntemi işaretleyin ile *zaman uyumsuz* anahtar sözcüğü. Bekleme süresi içerir bir işlem bir kod satırı devreye zaman yönteminin içinde bekleme anahtar sözcüğü ile işaretleyin.

Bir veritabanı sorgusu için depo yöntemini çağıran bir basit eylem yöntemi şöyledir:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Ve veritabanı çağrısı zaman uyumsuz olarak işler aynı yöntemi şöyledir:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Perde arkasında derleyici uygun zaman uyumsuz kod oluşturur. Ne zaman uygulamanın yaptığı çağrısı `FindTaskByIdAsync`, ASP.NET yapar `FindTask` isteği çalışan iş parçacığı unwinds ve başka bir isteği işlemek için kullanılabilir hale getirir. Zaman `FindTask` istek gerçekleştirilir, o çağrısından sonra gelen kod işleme devam etmek için bir iş parçacığı yeniden. Ne zaman arasında geçiş sırasında `FindTask` isteği başlatılır ve veri döndürüldüğünde, yanıt bekleme bağlanması Aksi takdirde, yararlı işlerini gerçekleştirmek kullanılabilir bir iş parçacığı vardır.

Zaman uyumsuz kodu için bazı ek yoktur, ancak düşük yük koşullar altında bu ek yük, sırasında kullanılabilir iş parçacıkları için bekleyen tutulan isteklerini işlemek için yüksek yük koşullarında önemsizdir.

Bu tür bir ASP.NET 1.1 sürümünden itibaren zaman uyumsuz programlama yapılması mümkün olmuştur, ancak yazmak zor, hatasız ve hata ayıklamak zordur. Bunun için ASP.NET 4.5 kodlama basitleştirdik, artık yapmaması için bir sebep yoktur.

### <a name="async-support-in-entity-framework-6"></a>Zaman uyumsuz destek Entity Framework 6

4.5 içinde zaman uyumsuz desteği bir parçası olarak biz web hizmeti çağrıları, yuva ve dosya sistemi g/ç için zaman uyumsuz destek sevk edilen ancak en yaygın Düzen web uygulamaları için bir veritabanı isabet yöneliktir ve bizim veri kitaplıkları zaman uyumsuz desteklemekteydi. Şimdi Entity Framework 6 veritabanı erişimi için async desteği ekler.

Entity Framework 6 sorgu veya komut veritabanına gönderilmesine neden tüm yöntemleri zaman uyumsuz sürümlere sahip. Örnek zaman uyumsuz sürümünü gösterir *Bul* yöntemi.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Ve bu zaman uyumsuz desteği yalnızca ekler, siler, güncelleştirmeleri ve basit bulur için çalışır, LINQ sorgularını ile de çalışır:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Var olan bir `Async` sürümü `ToList` yöntemi bu kodda, veritabanına gönderilmek üzere bir sorgu neden yöntemi olduğundan. `Where` Ve `OrderByDescending` yöntemlerini yalnızca sorgu yapılandırma sırasında `ToListAsync` yöntemi sorguyu çalıştırır ve yanıtta depolar `result` değişkeni.

## <a name="summary"></a>Özet

Tüm web programlama çerçevesi ve tüm bulut ortamı Burada özetlenen web geliştirme en iyi yöntemleri uygulayabilirsiniz, ancak kolaylaştırmak için ASP.NET ve Windows Azure Araçları sunuyoruz. Bu düzenleri izlerseniz, web katmanı genişletmek kolayca ölçeklendirebilirsiniz ve her sunucu daha fazla trafik işlemek mümkün olduğundan, harcamalarınızı en aza.

[Sonraki bölümde](single-sign-on.md) nasıl bulut tek oturum açma senaryolara olanak sağlar arar.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Durum bilgisiz web sunucuları:

- [Microsoft Patterns and Practices - otomatik ölçeklendirmeyi Kılavuzu](https://msdn.microsoft.com/en-us/library/dn589774.aspx).
- [Devre dışı bırakma ARR ait örnek benzeşimi Windows Azure Web siteleri,](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Blog gönderisi Erez Benari tarafından oturum benzeşimi Windows Azure Web siteleri açıklar.

CDN:

- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin video serisi dokuz bölümü. Bölüm 3 1:34: 00'dan başlayarak, CDN tartışma bakın.
- [Microsoft Patterns ve yöntemleri statik içerik barındırma düzeni](https://msdn.microsoft.com/en-us/library/dn589776.aspx)
- [CDN incelemeler](http://www.cdnreviews.com/). Birçok CDN'ler genel bakış.

Zaman uyumsuz programlama:

- [ASP.NET MVC 4'te zaman uyumsuz yöntemleri kullanarak](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Öğretici Rick Anderson tarafından.
- [Zaman uyumsuz programlama ile Async ve Await (C# ve Visual Basic)](https://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx). Zaman uyumsuz programlama için stratejinin, ASP.NET 4.5 içinde nasıl çalıştığı ve uygulamak için kodunun nasıl yazılacağını açıklayan MSDN teknik incelemesini.
- [Entity Framework zaman uyumsuz sorgu ve kaydetme](https://msdn.microsoft.com/en-us/data/jj819165)
- [Zaman uyumsuz kullanarak ASP.NET Web uygulamaları oluşturmak nasıl](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Video sunumu Rowan Mert tarafından. Grafik Tanıtımı içerir, ne zaman uyumsuz programlama yüksek yük koşullar altında web sunucusu verimliliği artışlar kolaylaştırabilir.
- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin video serisi dokuz bölümü. Zaman uyumsuz programlama ölçeklenebilirlik üzerinde etkisi hakkında daha fazla tartışma için bkz: Bölüm 4 ve bölüm 8.
- [ASP.NET 4.5 yanı sıra önemli bir sorunu zaman uyumsuz yöntemler kullanma Sihirli](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). ASP.NET Web Forms uygulamalarında async kullanma hakkında öncelikle Blog Gönderisi Scott Hanselman tarafından.

Ek web geliştirme en iyi yöntemler için aşağıdaki kaynaklara bakın:

- [Düzeltme uygulama - en iyi uygulamaları örnek](the-fix-it-sample-application.md#bestpractices). Bu e-kitap eke Düzelt uygulamada uygulanan en iyi yöntemler sayısını listeler.
- [Web geliştirici denetim listesi](http://webdevchecklist.com/asp.net)

>[!div class="step-by-step"]
[Önceki](continuous-integration-and-continuous-delivery.md)
[sonraki](single-sign-on.md)
