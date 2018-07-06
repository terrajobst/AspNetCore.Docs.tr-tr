---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web geliştirme en iyi yöntemler (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 054fcad387608e136e8171f5d511ee7af1082bd0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824435"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web geliştirme en iyi yöntemler (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


İlk üç desenler bir Çevik Geliştirme sürecinize ayarlama hakkında vardı; rest, mimari ve kod hakkında verilmiştir. Bu bir web geliştirme en iyi aşağıdakilerden oluşur:

- [Durum bilgisi olmayan web sunucuları](#stateless) akıllı yük dengeleyicinin arkasına.
- [Oturum durumu önlemek](#sessionstate) (veya bunu yoksayılamaz, bir veritabanı yerine dağıtılmış önbellek kullanın).
- [CDN kullanan](#cdn) için edge önbelleği statik dosya varlıklarınızı (görüntüler, betikler).
- [.NET 4.5'ın zaman uyumsuz desteği kullanan](#async) çağrıları engellemekten kaçınacak şekilde.

Bu yöntemler tüm web geliştirme, yalnızca bulut uygulamaları için geçerlidir, ancak bunlar özellikle bulut uygulamaları için önemli. Bunlar bulut ortamı tarafından sağlanan son derece esnek ölçeklendirme, en iyi kullanabilmesine yardımcı olmak için birlikte çalışır. Bu yöntemler takip etmiyorsa, uygulamanızı ölçeklendirmeyi çalıştığınızda sınırlamaları çalıştıracaksınız.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Bir akıllı yük dengeleyicinin arkasına durum bilgisi olmayan web katmanı

*Durum bilgisi olmayan web katmanı* web sunucusu bellek veya dosya sistemindeki uygulama verilerini depolamak yok anlamına gelir. Durum bilgisi olmayan web katmanınızın saklama, hem daha iyi bir müşteri deneyimi sağlar ve paradan tasarruf etmenize olanak sağlar:

- Durum bilgisi olmayan web katmanı ve bir yük dengeleyici arkasında bulunur, hızla uygulama trafiği değişiklikleri dinamik olarak ekleyerek veya kaldırarak sunucuları yanıt verebilirsiniz. Bulut ortamında, bunları gerçekten kullandığınız sürece burada yalnızca sunucu kaynakları için ödeme yaparsınız tasarruflardan talepte değişikliklerine yanıt verme becerisini çevirebilir.
- Bir durum bilgisi olmayan web katmanı uygulamanın ölçeğini genişletmek mimari çok daha kolaydır. Bu çok daha hızlı bir şekilde ölçeklendirme gereksinimleri için'yanıt vermesine ve geliştirme ve test etme işleminde daha az para harcamanız sağlar.
- Düzeltme eki ve zaman zaman yeniden şirket içi sunucular gibi bulut sunucularına gerekir; ve web katmanı ve durum bilgisi olmayan bir sunucu geçici olarak arızalandığında trafiğin yeniden yönlendirilmesini hataları veya beklenmeyen davranışlara neden olmaz.

Çoğu gerçek uygulamalar için bir web oturum durumunu depolamak gerekir; Burada temel nokta web sunucusunda depolamamayı sağlamaktır. İstemcide tanımlama bilgilerini veya işlemi sunucu tarafı önbelleği sağlayıcısı kullanarak ASP.NET oturum durumu gibi farklı yollarla durumu depolayabilirsiniz. Dosyalarında depolayabilir [Windows Azure Blob Depolama](unstructured-blob-storage.md) yerel dosya sistemi yerine.

Durum bilgisi olmayan web katmanınızın ise Windows Azure Web sitelerinde bir uygulamayı ölçeklendirme ne kadar kolay olduğunu bir örnek bkz **ölçek** sekmesinde bir Windows Azure Web sitesi için Yönetim Portalı'nda:

![Ölçek sekmesini](web-development-best-practices/_static/image1.png)

Web sunucusu eklemek istiyorsanız, örnek sayısı kaydırıcıyı sağa yalnızca sürükleyebilirsiniz. 5 ayarlanmış ve tıklayın **Kaydet**, ve saniyeler içinde web sitenizin trafiği işleme Windows Azure'da 5 web sunucusuna sahip.

![Beş örnek](web-development-best-practices/_static/image2.png)

Örnek sayısı 3 aşağı veya geri 1 aşağı kolayca ayarlayabilirsiniz. Geri ölçeğini daralttığınızda, paradan tasarruf etmeye başlayın hemen Windows Azure değil saatlik olarak dakikaya göre ücretlendirdiği.

Ayrıca, otomatik olarak artırın veya CPU kullanımına göre web sunucularının sayısını azaltmak için Windows Azure söyleyebilirsiniz. Aşağıdaki örnekte, CPU kullanımı % 60 gittiğinde, web sunucu sayısını en az 2 azaltır ve CPU kullanımı % 80'in kalırsa, web sunucularının sayısı en fazla 4 adede kadar artırılır.

![CPU kullanımına göre ölçeklendirin](web-development-best-practices/_static/image3.png)

Veya sitenizin yalnızca çalışma saatlerinde meşgul olacağını bilmeniz ne olur? Birden çok sunucu sırasında daytime çalıştırmak ve tek bir sunucu Akşamları, nights ve hafta sonları azaltmak için Windows Azure söyleyebilirsiniz. Aşağıdaki ekran görüntüleri bir dizi, web sitesinin yedeklenmesi sırasında iş saat 8: 00-17 mesai saatleri dışında bir sunucu ve 4 sunucu çalıştırılacak ayarlama işlemi gösterilmektedir.

![Zamanlamaya göre ölçeklendirin](web-development-best-practices/_static/image4.png)

![Zamanlama saatleri ayarlayın](web-development-best-practices/_static/image5.png)

![Gündüz saatlerinde kullanılan zamanlama](web-development-best-practices/_static/image6.png)

![Weeknight zamanlaması](web-development-best-practices/_static/image7.png)

![Hafta sonu zamanlama](web-development-best-practices/_static/image8.png)

Ve Elbette tüm portal olduğu gibi betik de yapılabilir.

Dinamik olarak ekleme ya da durum bilgisi olmayan web katmanı tutarak server Vm'leri kaldırma engelleri önlemek sürece, uygulamanızın ölçeğini genişletmek için Windows Azure üzerinde neredeyse sınırsız yeteneğidir.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Oturum durumu kaçının

Bir kullanıcı oturumu için durum çeşit depolanmasını önlemek için bir gerçek hayatta kullanılan bulut uygulaması pratik değildir, ancak aşağıdaki yaklaşımlardan diğerlerinden daha fazla performans ve ölçeklenebilirlik etkisi. Durumunu depolamak varsa, durum miktarını küçük tutun ve tanımlama bilgilerini depolamak için en iyi çözüm olur. Bu uygun değilse, ASP.NET oturum durumu sağlayıcısı için kullanılacak sonraki en iyi çözüm olduğundan [dağıtılmış, bellek içi önbellek](distributed-caching.md#sessionstate). En kötü bir performans ve ölçeklenebilirlik açısından bir veritabanını kullanmak için oturum durumu sağlayıcısı desteklenen çözümdür.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Statik dosya varlıklarınızı önbelleğe almak için CDN kullanın

CDN, Content Delivery Network için kısaltmasıdır. Bir CDN sağlayıcısına görüntülerini ve komut dosyaları gibi statik dosya varlıklarınızı sağlayın ve böylece insanların uygulamanızı erişmek her yerde, görece hızlı yanıt ve düşük gecikme süresi için önbelleğe alınan aldıkları sağlayıcı dünyanın dört bir yanındaki veri merkezlerinde bu dosyaları önbelleğe alır varlıklar. Bu sitenin genel yük zaman hızlandırır ve web sunucularınızdaki yükü azaltır. CDN'ler coğrafi olarak dağıtılmış geniş bir kitleye ulaşıyor, özellikle önemlidir.

Windows Azure CDN sahiptir ve Windows Azure'da veya herhangi bir web barındırma ortamı içinde çalışan bir uygulama, diğer CDN'ler kullanabilirsiniz.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>.NET 4.5'ın zaman uyumsuz destek çağrıları engellemekten kaçınacak şekilde kullanın

.NET 4.5 C# ve VB programlama dillerine görevler zaman uyumsuz olarak işlemek çok daha kolay yapmak için geliştirilmiştir. Yalnızca aynı anda birden çok web hizmeti çağrıları devre dışı başlatmasını istediğinizde gibi paralel işleme durumlar için zaman uyumsuz programlama avantajlarını değil. Ayrıca, daha verimli bir şekilde gerçekleştirmek, web sunucusu sağlar ve yüksek yük koşullarında güvenilir. Bir web sunucusu, yalnızca sınırlı sayıda iş parçacığı kullanılabilir vardır ve tüm iş parçacıkları, kullanımda olduğunda yüksek yük koşullarında gelen istekleri iş parçacıkları serbest bırakılana kadar beklemek zorunda. Zaman uyumsuz olarak veritabanı sorguları ve web hizmeti çağrıları gibi görevleri, uygulama kodunu işlemezse, sunucunun bir g/ç yanıtı beklerken birçok iş parçacığı gereksiz yere bağlıdır. Bu sunucunun yüksek yük koşullarında işleyebilir trafik miktarını sınırlar. Zaman uyumsuz programlama ile bir web hizmeti veya veritabanı verileri döndürmek için bekleyen iş parçacıklarının veri dek kadar yeni isteklere hizmet kurtulurlar alınır. Meşgul bir web sunucusu, yüzlerce veya binlerce isteği daha sonra en kısa sürede hangi yukarı serbest iş parçacığı için bekleyeceğidir işlenebilir.

Daha önce bahsettiğim gibi web sunucuları, web sitenizi arttırmayı olduğu gibi işleme sayısını azaltmak oldukça kolaydır. Bir sunucu daha yüksek aktarım hızı elde edebilirsiniz, verilen trafik hacmi için başka bir duruma göre daha az sunucu gerektiğinden birçok bunları ve maliyetlerinizi azaltabilirsiniz olarak bu nedenle gerekmez.

.NET 4.5 zaman uyumsuz programlama modeli için destek ASP.NET 4.5 Web Forms, MVC ve Web API'si için dahildir; Entity Framework 6 hem de [Windows Azure depolama API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>ASP.NET 4.5 içinde zaman uyumsuz desteği

ASP.NET 4.5 içinde zaman uyumsuz programlama dili için yalnızca aynı zamanda MVC, Web Forms ve Web API çerçeveleri eklendi desteği. Örneğin, bir ASP.NET MVC denetleyici eylem yöntemi bir web isteğinde verileri alır ve ardından tarayıcıya gönderilecek HTML oluşturan bir görünüme veri geçirir. Sıklıkla bir eylem yönteminin bir web sayfasında görüntülemek için veya bir web sayfasına girilen verileri kaydetmek için bir veritabanı veya web hizmetinden veri al gerekir. Bu senaryolarda zaman uyumsuz eylem yönteminin yapmak kadar kolay olduğunu: döndürmek yerine bir *actionresult öğesini* nesne, iade ettiğiniz *görev&lt;actionresult öğesini&gt;*  ve yöntem işaretleyin ile *zaman uyumsuz* anahtar sözcüğü. Bir kod satırı devreye bekleme zamanı içeren bir işlem devre dışı olduğunda bir yöntemde, await anahtar sözcüğü ile işaretleyin.

Bir veritabanı sorgusu için bir depo yöntemi çağıran basit bir eylem yöntemi aşağıda verilmiştir:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Ve veritabanı çağrısı zaman uyumsuz olarak işleyen aynı yöntem şu şekildedir:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Arka planda, derleyici uygun zaman uyumsuz kod oluşturur. Uygulamanın çağrısı ne zaman yaptığı `FindTaskByIdAsync`, ASP.NET yapar `FindTask` istemek için iş parçacığı geriye doğru izler ve başka bir isteği işlemek kullanılabilir hale getirir. Zaman `FindTask` isteği yapılır, bu çağrıdan sonra gelen kodu işleme devam etmek için bir iş parçacığı başlatılır. Ne zaman arasında geçiş sırasında `FindTask` istek başlatılır ve döndürülen veriler, yanıt bekleyen bağlanması Aksi halde, faydalı bir iş gerçekleştirmek için kullanılabilecek bir iş parçacığı vardır.

Zaman uyumsuz kod için bazı ek yükler vardır, ancak düşük yük koşullarında, bu ek göz ardı edilebilir, çalışırken kadar kullanılabilir iş parçacığı bekleniyor tutulan istekleri işleyebilir yüksek yük koşullarında yüktür.

Bu tür bir ASP.NET 1.1 sürümünden itibaren zaman uyumsuz programlama yapmanız mümkün olmuştur, ancak zor yazmak, hata yapmaya açık ve hata ayıklamak zordur. Bunun için ASP.NET 4.5 içinde kodlama basitleştirdik, artık bunu için bir neden yoktur.

### <a name="async-support-in-entity-framework-6"></a>Entity Framework 6 zaman uyumsuz desteği

4.5 içinde zaman uyumsuz destek bir parçası olarak şu web hizmeti çağrıları, yuva ve dosya sistemi g/ç için zaman uyumsuz destek sevk edilen ancak web uygulamaları için en yaygın desen, bir veritabanı isabet sağlamaktır ve bizim veri kitaplıkları zaman uyumsuz desteklemedi. Artık, Entity Framework 6 veritabanı erişimi için async desteği ekler.

Entity Framework 6'da bir sorgu veya veritabanına gönderilecek komutu neden tüm yöntemler, zaman uyumsuz sürümü bulunuyor. Örnek zaman uyumsuz sürümünü gösterir *Bul* yöntemi.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Ve bu zaman uyumsuz destek yalnızca ekleme, silme, güncelleştirmeleri ve basit bulur için çalışır, ayrıca LINQ sorguları ile çalışır:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Var olan bir `Async` sürümünü `ToList` yöntemi bu kodda, veritabanına gönderilmek üzere bir sorgu neden yöntemi olduğundan. `Where` Ve `OrderByDescending` yöntemlerini sorgu yalnızca yapılandırma sırasında `ToListAsync` yöntemi sorguyu çalıştırır ve yanıtta depolar `result` değişkeni.

## <a name="summary"></a>Özet

Herhangi bir web çerçevesi ve herhangi bir bulut ortamında programlama Burada özetlenen web geliştirme en iyi yöntemleri uygulayabilirsiniz, ancak ASP.NET ve Windows Azure'da kolaylaştıran Araçlar sunuyoruz. Bu düzenleri izliyorsa, web katmanınızın kolayca ölçeklendirilebilir ve her sunucuda daha fazla trafik işlemek mümkün olacağından, harcamalarınızı en aza.

[Sonraki bölümde](single-sign-on.md) bulut tek oturum açma senaryoları nasıl sağladığını konumunda görünür.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Durum bilgisi olmayan web sunucuları:

- [Microsoft Patterns ve uygulamaları - otomatik ölçeklendirme Kılavuzu](https://msdn.microsoft.com/library/dn589774.aspx).
- [Devre dışı bırakma ARR'ın örnek benzeşimi Windows Azure Web sitelerinde](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Blog gönderisi Erez benari'nin oturum benzeşimi, Windows Azure Web siteleri açıklar.

CDN:

- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden Ulrich Homann, Marc Mercuri ve Mark Simms'in video serisi. 3. Bölüm 1:34: 00'dan başlayarak, CDN tartışmalara bakın.
- [Microsoft Patterns ve yöntemler statik içerik barındırma düzeni](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN incelemeleri](http://www.cdnreviews.com/). Birçok CDN'ler genel bakış.

Zaman uyumsuz programlama için:

- [ASP.NET MVC 4'te zaman uyumsuz metotlar kullanma](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Öğretici Rick Anderson tarafından.
- [Zaman uyumsuz ile zaman uyumsuz programlama (C# ve Visual Basic) await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). Zaman uyumsuz programlama için gerekçe, ASP.NET 4.5 içinde nasıl çalıştığını ve uygulamak için kodun nasıl yazılacağını açıklayan MSDN teknik incelemesi.
- [Entity Framework zaman uyumsuz sorgu ve tasarruf edin](https://msdn.microsoft.com/data/jj819165)
- [Zaman uyumsuz kullanarak ASP.NET Web uygulamalarının nasıl oluşturulacağını](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Video sunumu Rowan Miller tarafından. Grafik bir örnek içerir, ne zaman uyumsuz programlama, web sunucusu aktarım hızı yüksek yük koşullarında artışlar kolaylaştırabilir.
- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden Ulrich Homann, Marc Mercuri ve Mark Simms'in video serisi. Bölüm 4 ve 8. Bölüm ölçeklenebilirlik üzerinde zaman uyumsuz programlama etkisi hakkında daha fazla tartışma için bkz.
- [ASP.NET 4.5 ve önemli bir sorunu zaman uyumsuz metotlar kullanma Sihirli](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Öncelikli olarak ASP.NET Web Forms uygulamalarında async kullanma hakkındaki Blog gönderisini Scott Hanselman tarafından.

Ek web geliştirme en iyi uygulamalar için aşağıdaki kaynaklara bakın:

- [Düzelt örnek uygulaması - en iyi](the-fix-it-sample-application.md#bestpractices). Bu e-kitap ek Düzelt uygulamada uygulanan en iyi yöntemler sayısını listeler.
- [Web geliştirici denetim listesi](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Önceki](continuous-integration-and-continuous-delivery.md)
> [İleri](single-sign-on.md)
