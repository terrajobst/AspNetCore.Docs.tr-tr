---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: "İzleme ve Telemetri (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: dfb0158ec05c890ecf80571d95b22d8c791ba7fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>İzleme ve Telemetri (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Kişilerin çok müşterilerin kendi uygulama aşağı olduğunda bildirmek için Bel. Gerçekten en iyi uygulama her yerden ve özellikle olmayan bulutta. Hızlı bildirim garantisi yoktur ve bildirim alma, ne hakkında en az veya yanıltıcı verileri genellikle alın. İyi telemetri ile günlük sistemleri, uygulamanız ile bir şey olduğunda neler olduğunu bilmeniz, hemen öğrenmek ve çalışmak için yararlı sorun giderme bilgileri sahip yanlış gidin.

## <a name="buy-or-rent-a-telemetry-solution"></a>Satın alma veya bir telemetri çözümü kiralamak

> [!NOTE]
> Önce bu makalenin yazıldığı [Application Insights](https://azure.microsoft.com/services/application-insights/) yayımlanmıştır. Application Insights telemetri çözümleri için tercih edilen yaklaşım Azure üzerinde olur. Bkz: [ASP.NET Web siteniz için Application Insights'ı ayarlamak](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) daha fazla bilgi için.


Bulut ortamınız hakkındaki harika şeylerden satın almanız veya sırrı şekilde kiralamak gerçekten kolay olduğunu biridir. Telemetri bir örnektir. Çok fazla çaba gerçekten iyi telemetri sistem alabileceğiniz açık ve çalışıyor, çok düşük maliyetli şekilde. Bir grup Azure ile tümleştirmek harika iş ortakları ve temel telemetri için hiçbir şey alabilmek için bunlardan bazıları boş katmanları – sahiptir. Azure üzerinde şu anda kullanılabilir olanlarla birkaçı şunlardır:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Mart 2015'ten itibaren [Microsoft Visual Studio Online için Application Insights](https://azure.microsoft.com/en-us/documentation/articles/app-insights-get-started/) henüz yayınlanmamıştır ancak denemek için Önizleme'de kullanılabilir. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) İzleme özelliklerini de içerir.

Biz hızlı bir şekilde ne kadar kolay bir telemetri sistemi kullanmak olabilir göstermek için New Relic yapılandırılacağını adım adım.

Azure Yönetim Portalı'nda hizmet için kaydolun. Tıklatın **yeni**ve ardından **deposu**. **Bir eklenti seçin** iletişim kutusu görüntülenir. Aşağı kaydırın ve tıklatın **New Relic**.

![Bir eklenti seçin](monitoring-and-telemetry/_static/image1.png)

Sağ oka tıklayın ve istediğiniz hizmet katmanını seçin. Ücretsiz katmanı bu tanıtım için kullanacağız.

![Eklenti kişiselleştirme](monitoring-and-telemetry/_static/image2.png)

Sağ oka tıklayın, "Satın Al" onaylayın ve New Relic şimdi Portalı'nda bir eklenti olarak görüntülenir.

![Satın alma gözden geçirin](monitoring-and-telemetry/_static/image3.png)

![Yönetim Portalı'nda yeni Relic eklenti](monitoring-and-telemetry/_static/image4.png)

Tıklatın **bağlantı bilgisi**ve lisans anahtarı kopyalayın.

![Bağlantı bilgileri](monitoring-and-telemetry/_static/image5.png)

Git **yapılandırma** portal web uygulamanızda ayarlamak sekmesi **performans izleme** için **eklenti**ve ayarlayın **eklenti seçin** aşağı açılan listesine **New Relic**. Ardından **kaydetmek**.

![New Relic yapılandırma sekmesinde](monitoring-and-telemetry/_static/image6.png)

Visual Studio'da, uygulamanızda yeni Relic NuGet paketini yükleyin.

![Geliştirici analizleri Yapılandır sekmesi](monitoring-and-telemetry/_static/image7.png)

Uygulamayı Azure'a dağıtma ve kullanmaya başlayın. Bazı etkinliği izlemek New Relic için sağlamak üzere birkaç Düzelt Görevler oluşturun.

Daha sonra geri dönerek **New Relic** sayfasındaki **eklentileri** sekmesini tıklatın ve portal **Yönet**. Portal, çoklu oturum açma kimlik bilgilerinizi yeniden girmek zorunda kalmamak için kimlik doğrulaması kullanarak New Relic yönetim portalına gönderir. Genel bakış sayfasında performans istatistiklerini çeşitli gösterir. (Genel bakış sayfasında tam boyutlu görmek için resme tıklayın.)

[![Yeni Relic izleme sekmesi](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Aşağıda, gördüğünüz istatistikleri birkaçı verilmiştir:

- Günün farklı saatlerinde ortalama yanıt süresi.

    ![Yanıt süresi](monitoring-and-telemetry/_static/image10.png)
- Günün farklı saatlerinde işleme hızları (isteklerindeki dakikada).

    ![Üretilen iş](monitoring-and-telemetry/_static/image11.png)
- Farklı HTTP isteklerini işlemek için harcanan sunucu CPU süre.

    ![Web işlem süreleri](monitoring-and-telemetry/_static/image12.png)
- Uygulama kodu farklı kısımlarını harcanan CPU süresi:

    ![İzleme Ayrıntıları](monitoring-and-telemetry/_static/image13.png)
- Geçmiş performans istatistikleri.

    ![Geçmiş performans](monitoring-and-telemetry/_static/image14.png)
- Blob hizmeti ve nasıl güvenilir ve esnek ilgili istatistikler gibi dış hizmetler çağrıları hizmet olmuştur.

    ![Dış hizmetler](monitoring-and-telemetry/_static/image15.png)

    ![Dış hizmetler](monitoring-and-telemetry/_static/image16.png)

    ![Dış hizmeti](monitoring-and-telemetry/_static/image17.png)
- Dünya veya nereden ABD web uygulamasında trafiği geldiğini where hakkında bilgi.

    ![coğrafi konum](monitoring-and-telemetry/_static/image18.png)

Ayrıca, raporları ve olayları ayarlayabilirsiniz. Örneğin, hataları görmeye başlayacaksınız dilediğiniz zaman söyleyin, uyarı destek personeli sorun için bir e-posta gönderin.

![Raporlar](monitoring-and-telemetry/_static/image19.png)

New Relic telemetri sisteminin yalnızca bir örnektir; Tüm diğer Hizmetleri'nden alabilir. Bulut güzelliğinin herhangi bir kod yazmak zorunda kalmadan ve için olan en az veya hiçbir gider aniden uygulamanızın nasıl kullanıldığını ve hangi müşterilerin gerçekte yaşayan hakkında çok daha fazla bilgi edinebilirsiniz.

<a id="log"></a>
## <a name="log-for-insight"></a>Hakkında bilgi için günlük

İyi bir ilk adım bir telemetri pakettir ancak kendi kodunuzu işaretlemesini çözümlenmedi. Telemetri hizmet söyler bir sorun olduğunda ve ne söyler müşterilerinizin yaşamakta olduğu, ancak bu, çok sayıda kodunuzda neler olup bittiğini içine Insight vermeyebilir.

Uygulamanızı yaptıklarını görmek için üretim sunucusuna uzaktan zorunda istemezsiniz. Bir sunucu var olduğunda, ancak ne yüzlerce sunucu için genişletilmiş ve hangilerinin içine uzak ihtiyacınız bilmiyorsanız, pratik olabilir? Günlük hiçbir zaman uzak çözümlemek ve hata ayıklama için üretim sunucularına sahip yeterli bilgi vermelidir sorunları. Böylece yalnızca günlükleri aracılığıyla sorunları yalıtmak yeterli bilgi günlük kaydı.

### <a name="log-in-production"></a>Üretimde oturum

Kişilerin çok Aç üretimde izleme yalnızca bir sorun olduğunu ve hata ayıklamak istedikleri zaman. Bu sorun kullanan zamanla ilgili yararlı sorun giderme bilgileri almak zaman arasında önemli bir gecikme ortaya çıkarabilir. Ve size bilgi aralıklı hatalar için faydalı olmayabilir.

Depolama alanının ucuz olduğu bulut ortamında kullanmanızı öneririz, üretimde oturum açan her zaman bırakın olur. Böylece bunları oturum zaten yoksa ve yardımcı olabilecek geçmiş verileri hataları oluştuğunda zamanla geliştirmek veya farklı zamanlarda düzenli olarak gerçekleşecek sorunları çözümleyin. Eski günlükleri silmek için temizleme işlemini otomatikleştirmek, ancak günlükler tutmak için daha böyle bir işlemi kurmak daha pahalı olduğunu fark edebilirsiniz.

Günlük eklenen gider, zaman ve para bir şeyler yanlış gittiğinde zaten kullanılabilir gereken bilgilerin tümünü sağlayarak kaydedebilirsiniz sorun giderme miktarını karşılaştırılan kısmı oldukça kolaydır. Birisi, hangi rasgele hata süre yaklaşık olarak 8:00 son gece sahip oldukları, ancak hata hatırlama bildirdiğinde, ardından, kolaylıkla sorun neydi çıkışı bulabilirsiniz.

$4'ten az için günlükleri 50 gigabayt elde tutma ay ve günlük performans etkisi Önemsiz aklınızda--günlük kitaplığınızın zaman uyumsuz olduğundan emin olun, performans sorunlarını önlemek için bir şey tutmak sürece.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Eylem gerektiren günlükleri bildirmek günlükleri ayırt

Günlükleri (bir şey bilmenizi istiyorum) INFORM veya ACT (bir şey yapmanız istiyorum) yöneliktir. Yalnızca bir kişinin veya otomatik bir işlem eyleme genuinely gerektiren sorunları için ACT günlüklerini yazma konusunda dikkatli olun. Çok fazla ACT günlükleri arkasını tüm orijinal sorunları bulmak için incelersiniz çok fazla iş gerektiren gürültü oluşturur. Ve ACT günlüklerinizi otomatik olarak personeli desteklemek için e-posta gönderme gibi bazı eylemi harekete geçirirseniz, bu tür Eylemler tek bir sorundan tetiklenmesi binlerce izin vererek kaçının.

.NET System.Diagnostics izleme günlükleri hata, uyarı, bilgi ve hata ayıklama/ayrıntı düzeyi atanabilir. ACT günlükleri için hata düzeyi ayırma ve alt düzey için INFORM günlüklerini kullanarak INFORM günlüklerinden ACT ayırt edebilir.

![Günlük düzeyleri](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Çalışma zamanında günlük düzeylerini yapılandırma

Üretimde her zaman oturum açmak için faydalı olsa da, başka bir en iyi uygulama çalışma zamanında dağıtarak veya uygulamanızı yeniden başlatmayı olmadan, oturum açtığınızdan ayrıntı düzeyini ayarlamanıza olanak tanır günlük framework uygulamaktır. Örneğin, izleme olanağı kullandığınızda `System.Diagnostics` oluşturabilirsiniz hata, uyarı, bilgi ve hata ayıklama/Verbose günlüğe kaydeder. Hata, uyarı, her zaman oturum ve üretimde bilgileri günlüğe kaydeder ve bir olay temelinde sorun giderme için hata ayıklama/ayrıntılı günlük kaydını dinamik olarak eklemek için istersiniz öneririz.

Azure App Service'te Web uygulamalarını sahip yazmak için yerleşik destek `System.Diagnostics` dosya sistemi, tablo depolama veya Blob Depolama günlükleri. Her bir depolama hedefi için farklı günlük düzeylerini seçebilirsiniz ve uygulamanızı yeniden başlatmadan kolay bir şekilde günlük düzeyini değiştirebilir. Blob Depolama desteği çalıştırmayı kolaylaştırır [Hdınsight](https://docs.microsoft.com/azure/hdinsight/) analiz Hdınsight Blob storage ile doğrudan çalışmak nasıl bildiği için uygulama günlüklerinizin işler.

### <a name="log-exceptions"></a>Günlük özel durumları

Yalnızca koymayın *özel durum. ToString()* günlük kodunuzda. Bağlamsal bilgileri bırakır. SQL hataları söz konusu olduğunda SQL hata numarası bırakır. Tüm özel durumları için bağlam bilgileri, özel durumu ve sorun giderme için gereken her şeyi sağlanmaktadır emin olmak için iç özel durumları içerir. Örneğin, bağlam bilgilerini sunucu adı, bir işlem tanımlayıcısı ve bir kullanıcı adı (ancak parola veya tüm gizli!) içerebilir.

Günlüğü özel durumla doğru şey yapmak için her geliştirici bağlıdır, bazıları olmaz. Yolu her zaman yapılacağı sağa emin olmak için özel durum işleme sağ Günlükçü arabirimine yapı: özel durum nesnesi kendisini Günlükçü sınıfına geçirmek ve özel durum verileri düzgün Günlükçü sınıfında oturum.

### <a name="log-calls-to-services"></a>Hizmetler için günlük çağrıları

Bir hizmet için uygulamanızı çağırır her zaman bir veritabanı veya bir REST API veya herhangi bir dış hizmeti olup, bir günlük yazma önerilir. Yalnızca başarı veya başarısızlık ancak her istek ne kadar sürdü belirtisi, günlüklerinize içerir. Bulut ortamında genellikle tam kesintileri yerine slow-downs ilgili sorunları görürsünüz. Normalde 10 milisaniye götüren bir şey ikinci bir alma aniden başlayabilir. Birisi, uygulamanızı yavaş olduğunu bildirir, New Relic aramak kullanabilmek ister ya da sahip ve doğrulama hangi telemetri hizmeti deneyimlerinden ve ardından aramak kullanabilmek ister kendi günlükleri neden yavaş ayrıntılarını daha yakından inceleyin.

### <a name="use-an-ilogger-interface"></a>Bir ILogger arabirimini kullanma

Ne bir üretim uygulaması oluşturduğunuzda yapılması öneririz basit bir oluşturmaktır *ILogger* arabirimi ve bazı yöntemler içinde bağlı kalın. Bu, daha sonra günlük kaydı uygulamasını değiştirin ve yapmak için tüm kodunuz gitmek zorunda değil kolaylaştırır. Biz kullanabilecek `System.Diagnostics.Trace` sınıf Düzelt uygulama boyunca, ancak bunun yerine biz uygulayan günlük sınıfında kapsar altında kullandığınız *ILogger*, ve vermiyoruz *ILogger* yöntemini çağırır boyunca uygulama.

Bu şekilde, herhangi bir zamanda, günlük daha zengin, yapmak isterseniz değiştirebileceğiniz [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) ile günlük mekanizma istiyor. Uygulamanız büyüdükçe gibi daha kapsamlı bir günlük paketi gibi kullanmak istediğiniz karar verebilirsiniz [NLog](http://nlog-project.org/) veya [Kurumsal kitaplığı günlük uygulama bloğu](https://msdn.microsoft.com/en-us/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) başka bir popüler günlüğü çerçevesi, ancak zaman uyumsuz günlük yapmaz.)

Bir çerçeve NLog gibi kullanarak olası bir nedeni, ayrı yüksek hacimli ve yüksek değerli veri depolarına çıkış günlüğü bölerek kolaylaştırmaktır. Bu, büyük ACT verilere hızlı erişim korurken, hızlı sorguları yürütmek için gerekmeyen INFORM veri birimleri verimli bir şekilde depolamak için yardımcı olur.

### <a name="semantic-logging"></a>Semantik günlük kaydı

Görece olarak daha yeni bir yöntemle daha kullanışlı tanılama bilgileri üretebilir günlük yapmak bilgi için bkz: [Kurumsal kitaplığı semantik günlük uygulama bloğu (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). BLOK kullanan [Windows için olay izleme](https://msdn.microsoft.com/en-us/library/windows/desktop/bb968803.aspx) (ETW) ve [EventSource](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracing.eventsource.aspx) destekleyen .NET 4.5 daha fazla yapılandırılmış ve sorgulanabilir günlüklerini oluşturmanızı sağlar. Her yazma bilgi özelleştirmenize olanak tanır, oturum, olay türü için farklı bir yöntem tanımlayın. Örneğin, oturum çağrısı bir SQL veritabanı hatası için bir `LogSQLDatabaseError` yöntemi. Özel durum, bu tür için yöntemi imzada bir hata numarası parametresini ekleyin ve hata numarası yazdığınız günlük kaydı ayrı bir alana olarak kaydetmek için bir anahtar bilgi parçasını hata numarası olduğunu bilirsiniz. Ayrı bir alana olduğundan yalnızca bir ileti dizeye hata numarası birleştirme oranla SQL hata numarası dayalı raporlar daha kolay ve güvenilir bir şekilde elde edebilirsiniz.

## <a name="logging-in-the-fix-it-app"></a>Bu düzeltme günlüğü uygulama

### <a name="the-ilogger-interface"></a>ILogger arabirimi

Burada *ILogger* Düzelt uygulama arabirimi.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Bu yöntemler tarafından desteklenen aynı dört düzeylerinde günlükleri yazmanıza olanak *System.Diagnostics*. Gecikme süresi hakkında bilgi içeren dış hizmeti çağrıları günlüğe TraceApi yöntemleridir. Hata ayıklama/ayrıntı düzeyi için yöntemler kümesi de ekleyebilirsiniz.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Günlükçü uygulama ILogger arabirimi

Gerçekten basit arabirim uygulamasıdır. Temel olarak yalnızca standart çağırır *System.Diagnostics* yöntemleri. Aşağıdaki kod parçacığında üçünü bilgi yöntemleri ve her biri diğer gösterir.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>ILogger yöntemleri çağırma

Bir özel durum Düzelt uygulama kodunda yakalar her zaman, çağıran bir *ILogger* özel durum ayrıntıları oturum yöntemi. Ve veritabanı, Blob hizmeti veya bir REST API çağrısı yapar her zaman bir kronometre çağırmadan önce başlar, hizmeti döndürdüğünde kronometre durdurur ve başarı veya başarısızlık ilgili bilgilerin yanında geçen süre günlüğe kaydeder.

Günlük iletisi yöntemi adını ve sınıf adı içerdiğine dikkat edin. Günlük iletilerini uygulama kodu hangi kısmının bunları yazdı tanımladığından emin olmak için iyi bir uygulamadır.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Düzelt uygulama SQL veritabanı için bir çağrı yaptı şimdi her zaman için çağrılan yöntemi çağrısı görebilir ve tam olarak ne kadar zaman aldı.

![SQL veritabanı sorgusu günlüklerine](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Günlükleri ile gözatma gidin, veritabanı çağrılarını süreyi değişkeni olduğunu görebilirsiniz. Bilgiler yararlı olabilir: Bu tüm uygulama günlüklerini olduğundan veritabanı hizmeti zaman içinde nasıl işlemlerinde geçmiş eğilimleri çözümleyebilirsiniz. Örneğin, bir hizmet çoğu zaman hızlı olabilir ancak istekleri başarısız olabilir veya yanıtlarını günün belirli zamanlarında yavaşlayabilir.

Blob hizmeti için– de aynı uygulama yeni bir dosya yükler, bir günlük yoktur ve her dosyayı karşıya yüklemeyi tam olarak ne kadar sürdüğünü görebilirsiniz her zaman yapabileceğiniz.

![BLOB karşıya yükleme günlüğü](monitoring-and-telemetry/_static/image23.png)

Yalnızca birkaç ek kod satırı bulunmaktadır bir hizmeti çağırıp birisi bir sorunu yaşayıp çalıştırdıkları diyor olduğunda, artık tam olarak sorunu, bir hata oluştu veya yalnızca yavaş bile çalışıyorsa, neydi biliyorsunuz her zaman yazmak için değil. Uzak bir sunucuya içine gerek kalmadan sorunun kaynağını sabitleme ya da hata olur ve yeniden yaratmayı ümit sonra günlük özelliğini açın.

## <a name="dependency-injection-in-the-fix-it-app"></a>Bağımlılık ekleme düzeltme, uygulama

Nasıl yukarıda gösterilen örnek depo oluşturucuda arabirim uygulamasına günlükçüsü merak ediyor:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Arabirimi kullanımla uygulama kablo kullanan [bağımlılık ekleme](http://en.wikipedia.org/wiki/Dependency_injection)(dı) ile [AutoFac](http://autofac.org/). DI, bir arabirim kodunuzu tüm konumlarındaki göre bir nesne kullanın ve yalnızca tek bir yerde arabirimi örneği oluşturulduğunda kullanılan uygulaması belirtmek zorunda olanak sağlar. Bu uygulama değiştirmek kolaylaştırır: Örneğin, NLog Günlükçü ile System.Diagnostics Günlükçü değiştirmek isteyebilirsiniz. Veya otomatik test için Günlükçü sahte bir sürümüne değiştirmek isteyebilirsiniz.

Düzelt uygulama dı tüm depoları ve tüm denetleyicilerin kullanır. Denetleyici sınıfları oluşturucular almak bir *ITaskRepository* depoyu alır Günlükçü arabirimi aynı şekilde arabirim:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Otomatik olarak sağlamak için AutoFac dı kitaplığı uygulamanın kullandığı *TaskRepository* ve *Günlükçü* örnekleri bu oluşturucuları için.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Bu kod temelde herhangi bir yerden bir oluşturucu gerektiğini bildiren bir *ILogger* arabirimi, bir örnekte geçirin *Günlükçü* sınıfı ve gerekli olduğunda bir *IFixItTaskRepository*arabirimi, bir örnekte geçirin *FixItTaskRepository* sınıfı.

[AutoFac](http://autofac.org/) kullanabileceğiniz pek çok bağımlılık ekleme çerçeveleri biridir. Başka bir popüler olandır [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), hangi önerilen ve Microsoft Patterns and Practices tarafından desteklenir.

## <a name="built-in-logging-support-in-azure"></a>Azure içinde yerleşik günlük desteği

Azure şu tür destekler, [Azure App Service'te Web uygulamalarını için oturum açma](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics izleme (açıp kapatabilirsiniz ve site başlatmadan anında düzeyleri ayarlayın).
- Windows olayları.
- IIS günlükleri (HTTP/FREB).

Azure şu tür destekler, [bulut hizmetlerinde oturum](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics izleme.
- Performans sayaçları.
- Windows olayları.
- IIS günlükleri (HTTP/FREB).
- Özel dizin izleme.

Düzelt uygulama System.Diagnostics izleme kullanır. Bir web uygulamasında oturum System.Diagnostics etkinleştirmek için yapmanız gereken tek şey Portalı'nda bir anahtar Çevir veya REST API çağrısı. Portalı'nda tıklatın **yapılandırma** sekmesinde siteniz için ve görmek için aşağı kaydırmanız **uygulama tanılama** bölümü. Günlük kaydı Aç veya kapat ve istediğiniz günlük düzeyini seçin. Azure dosya sistemine veya bir depolama hesabına günlüklerini yazma olabilir.

![Uygulama tanılama ve yapılandırma sekmesinde site tanılama](monitoring-and-telemetry/_static/image24.png)

Azure'da oturum etkinleştirdikten sonra oluşturuldukları sırada günlüklerini Visual Studio çıktı penceresinde görebilirsiniz.

![Akış günlükleri menüsü](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Akış günlükleri menüsü](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Ayrıca, depolama hesabına yazılan günlükler olabilir ve bunları herhangi biriyle aracı görünümü erişim Azure depolama tablo hizmeti gibi **Sunucu Gezgini** Visual Studio veya [Azure Storage Gezgini](https://azure.microsoft.com/features/storage-explorer/).

![Sunucu Gezgininde günlükleri](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Özet

Giden kutusu telemetri sistem uygulamak, kendi kodunuzda oturum izleme ve Azure'da oturum açmayı yapılandırmak gerçekten basit bir işlemdir. Ve üretim sorunları varsa, telemetri sistem ve özel günlükler birleşimi müşterileriniz için önemli sorunları olduklarında önce sorunların hızla çözülmesine yardımcı olur.

İçinde [sonraki bölümde](transient-fault-handling.md) araştırmak için sahip üretim sorunları dönüşmez şekilde geçici hataları işlemek nasıl tümleştirildiği incelenmektedir.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Çoğunlukla telemetri ile ilgili belgeler:

- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/en-us/library/dn568099.aspx). İzleme ve Telemetri Kılavuzu, ölçüm hizmetini Kılavuzu, uç nokta sistem durumu izleme düzeni ve çalışma zamanı yeniden yapılandırma düzeni bakın.
- [Bulutta çimdik kuruş: Azure Web sitelerinde izleme New Relic performans etkinleştirme](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [En iyi uygulamalar için Azure bulut hizmetleri üzerinde büyük ölçekli hizmetler tasarımını](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). Teknik incelemesi işareti SIMM'lerinin ve Michael Thomassy tarafından. Telemetri ve Tanılama bölümüne bakın.
- [Application Insights ile yeni nesil geliştirme](https://msdn.microsoft.com/en-us/magazine/dn683794.aspx). MSDN dergisi makalesi.

Belge çoğunlukla günlüğü hakkında:

- [Semantik günlük uygulama bloğu (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie semantik günlük BLOK ile söz konusu gösterir.
- [Yapılandırılmış ve anlamlı günlüklerini semantik günlük kaydıyla oluşturma](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Jülyen Dominguez semantik günlük BLOK ile söz konusu gösterir.
- [EF6 SQL günlüğü – bölüm 1: Basit günlüğe kaydetme](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers EF 6 Entity Framework tarafından yürütülen sorgularını oturum gösterilmektedir.
- [Bağlantı dayanıklılığı ve ASP.NET MVC uygulamasındaki Entity Framework komut kişiler tarafından ele](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Dördüncü dokuz bölümlü öğretici serisinde EF 6 komutu kişiler tarafından ele özelliği veritabanına Entity Framework tarafından gönderilen SQL komutlarını günlüğe kaydetme için nasıl kullanılacağını gösterir.
- [C# 5.0 arayan bilgileri öznitelikleri kullanarak günlük artırmak](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Arama yöntemin adını sabit değişmez değerleri kodlama veya el ile almak için yansıma kullanarak günlüğe kolayca yapma.

Sorun giderme çoğunlukla hakkındaki belgelere:

- [Azure sorun giderme &amp; blog hata ayıklama](https://blogs.msdn.com/b/kwill/).
- [AzureTools – tanılama Azure Geliştirici destek ekibi tarafından kullanılan yardımcı programı'nı](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Tanıtır ve bir Azure VM indirin ve çok çeşitli tanılama ve izleme araçları çalıştırmak için kullanılan bir aracı için indirme bağlantısı sağlar. Belirli bir VM'de bir sorunu tanılamak gerektiğinde kullanışlıdır.
- [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). System.Diagnostics izleme ve uzaktan hata ayıklama ile çalışmaya başlama hakkında adım adım öğretici.

Videolar:

- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Dokuz parçalı seriyi Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin. Üst düzey kavramlarını ve mimari ilkeler çok erişilebilir ve ilginç yolla, Microsoft Müşteri danışma ekibi (CAT) deneyiminde gerçek müşterilerle çizilmiş hikayeleri sunar. 4. ve 9 bölümleri izleme ve telemetri hakkında ' dir. Bölüm 9 MetricsHub, AppDynamics, New Relic ve PagerDuty hizmetlerini izleme genel bir bakış içerir.
- [Yapı büyük: Azure müşterilerden - bölümü II dersleri öğrenilen](https://channel9.msdn.com/Events/Build/2012/3-030). İşareti SIMM'lerinin için hatası tasarlama ve her şeyi düzenleme hakkında alınmaktadır. Hatasız serisi ancak yapılır Ayrıntılar gerçekleştirip gerçekleştirmeyeceğini benzer.

Kod örneği:

- [Bulut hizmeti temel bilgileri Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Örnek uygulaması Microsoft Azure Müşteri danışma ekibi tarafından oluşturuldu. Telemetri ve günlüğe kaydetme yöntemler aşağıdaki makalelerde açıklanan gösterir. Örnek Uygulama günlüğünü kullanarak uygulayan [NLog](http://nlog-project.org/). İlgili belgeler için bkz: [telemetri ve günlüğe kaydetme hakkında dört TechNet wiki makaleleri dizi](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

>[!div class="step-by-step"]
[Önceki](design-to-survive-failures.md)
[sonraki](transient-fault-handling.md)
