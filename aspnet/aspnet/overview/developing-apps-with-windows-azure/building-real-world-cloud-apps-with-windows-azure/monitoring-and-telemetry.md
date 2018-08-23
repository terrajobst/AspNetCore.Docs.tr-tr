---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: İzleme ve Telemetri (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: cec29e27e834317661f80eed93cb8b81a0786644
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752777"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>İzleme ve Telemetri (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Birçok kişinin bağımlı müşterilerin kendi uygulama kapalı olduğunda bildirin. Bu gerçekten en iyi uygulama her yerden ve özellikle değil bulutta değil. Hızlı bildirim dair bir garanti yoktur ve bildirim alın, ne hakkında en aza ya da yanıltıcı veriler genellikle alın. İyi çalışan telemetri ve günlüğe kaydetme sistemleriyle, uygulamanızda ve bir şey olduğunda neler olup bittiğini farkında olabilir, bilebilir ve çalışmak için yararlı sorun giderme bilgilerine sahip yanlış gidin.

## <a name="buy-or-rent-a-telemetry-solution"></a>Satın alma veya bir telemetri çözümü kiralayabilirler

> [!NOTE]
> Önce bu makalenin yazıldığı [Application Insights](https://azure.microsoft.com/services/application-insights/) yayınlanmıştır. Application Insights, Azure ile ilgili telemetri çözümleri için tercih edilen yaklaşım. Bkz: [ASP.NET Web siteniz için Application ınsights'ı ayarlama](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) daha fazla bilgi için.


Bulut ortamınız hakkındaki harika şeylerden biri, satın almanız veya sırrı şekilde kiralayabilirler gerçekten çok kolay olmasıdır. Telemetri bir örnektir. Çok fazla çaba, gerçekten iyi çalışan telemetri sistemi alabilirsiniz ve çalıştırma, çok düşük bir maliyetle. Azure ile tümleştirilmesini harika iş ortaklarından oluşan bir grup vardır ve hiçbir şey için temel telemetri alabilmeniz için bunlardan bazıları ücretsiz Katmanlar – sahip. Azure üzerinde şu anda kullanılabilir olanlarla birkaçı şunlardır:

- [Yeni Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Mart 2015 itibariyle [Microsoft Visual Studio Online için Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) henüz yayımlanmamış ancak denemek için Önizleme olarak kullanılabilir. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) İzleme özelliklerini de içerir.

Hızlı bir şekilde ne kadar kolay, telemetri sisteminin kullanılacak olabileceğini göstermek için New Relic ayarı aracılığıyla alacağız.

Azure Yönetim Portalı'nda hizmet için kaydolun. Tıklayın **yeni**ve ardından **Store**. **Eklenti Seç** iletişim kutusu görüntülenir. Aşağı kaydırıp tıklayın **New Relic**.

![Eklenti Seç](monitoring-and-telemetry/_static/image1.png)

Sağ oka tıklayın ve istediğiniz hizmet katmanını seçin. Bu Tanıtım için ücretsiz katman kullanacağız.

![Eklenti kişiselleştirin](monitoring-and-telemetry/_static/image2.png)

Sağ oka tıklayın, "Satın Al" onaylayın ve New Relic artık portalda eklentisi olarak gösterilir.

![Satın alma gözden geçirin](monitoring-and-telemetry/_static/image3.png)

![Yeni Relic eklenti Yönetim Portalı'nda](monitoring-and-telemetry/_static/image4.png)

Tıklayın **bağlantı bilgisi**ve lisans anahtarı kopyalayın.

![Bağlantı bilgileri](monitoring-and-telemetry/_static/image5.png)

Git **yapılandırma** portalında web uygulamanız için sekmesinde **performansı izleme** için **eklenti**, ayarlayıp **eklenti seçin** aşağı açılan listesine **New Relic**. Ardından **Kaydet**.

![Yeni Relic yapılandırma sekmesinde](monitoring-and-telemetry/_static/image6.png)

Visual Studio'da uygulamanızda yeni Relic NuGet paketini yükleyin.

![Yapılandırma sekmesinde Geliştirici analizi](monitoring-and-telemetry/_static/image7.png)

Uygulamayı Azure'a dağıtma ve kullanmaya başlayın. Bazı etkinliği izlemek New Relic için sağlamak için birkaç Düzelt görev oluşturun.

Ardından dönün **New Relic** sayfasını **eklentileri** sekmesini tıklatın ve portal **Yönet**. Portal, çoklu oturum açma kimlik bilgilerinizi yeniden girmek zorunda kalmaması için kimlik doğrulaması kullanarak New Relic yönetim portalına gönderir. Genel bakış sayfasında çeşitli performans istatistiklerini gösterir. (Genel bakış sayfasında tam boyutlu görmek için görüntüye tıklayın.)

[![Yeni Relic izleme sekmesi](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Gördüğünüz istatistikleri birkaçı şunlardır:

- Günün farklı saatlerinde ortalama yanıt süresi.

    ![Yanıt süresi](monitoring-and-telemetry/_static/image10.png)
- Günün farklı saatlerinde işleme hızları (içindeki bir dakikada gönderilen istek).

    ![Aktarım hızı](monitoring-and-telemetry/_static/image11.png)
- Farklı HTTP isteklerini işlemek için harcanan sunucu CPU süre.

    ![Web işlemi süresi](monitoring-and-telemetry/_static/image12.png)
- Uygulama kodu farklı kısımlarını harcanan CPU süresi:

    ![İzleme Ayrıntıları](monitoring-and-telemetry/_static/image13.png)
- Geçmiş performans istatistikleri.

    ![Geçmiş performans](monitoring-and-telemetry/_static/image14.png)
- Blob hizmeti ve güvenilir ve hızlı yanıt veren hakkında istatistikler gibi dış hizmetlerle çağrıları, hizmet olmuştur.

    ![Dış hizmetler](monitoring-and-telemetry/_static/image15.png)

    ![Dış hizmetler](monitoring-and-telemetry/_static/image16.png)

    ![Dış hizmet](monitoring-and-telemetry/_static/image17.png)
- Dünyanın veya nereden ABD web uygulaması trafiğini geldiğini nereye hakkında bilgiler.

    ![Coğrafi konum](monitoring-and-telemetry/_static/image18.png)

Ayrıca, raporları ve olayları da ayarlayabilirsiniz. Örneğin, hataları görmeye başlamak istediğiniz zaman varsayalım, uyarı destek personelinin sorunu bir e-posta gönderin.

![Raporlar](monitoring-and-telemetry/_static/image19.png)

Yeni Relic telemetri sisteminin yalnızca bir örnektir; Tüm diğer hizmetlerden alabilirsiniz. Herhangi bir kod yazmak zorunda kalmadan ve içindir bulut güzelliği en az veya hiçbir gider aniden uygulamanızın nasıl kullanıldığını ve ne müşterilerinizin gerçekten yaşayan hakkında daha fazla bilgi alabilirsiniz.

<a id="log"></a>
## <a name="log-for-insight"></a>İlgili ayrıntılı bilgi için günlük

İyi bir ilk adım bir telemetri pakettir, ancak kendi kodunuzu izleme çözümlenmedi. Telemetri hizmeti size bildirir. bir sorun olduğunda ne bildirir müşterilerin yaşıyorsanız, ancak bu, çok sayıda kodunuzda neler konusunda fikir verebilir.

Uygulamanız ne yaptığını görmek için bir üretim sunucusuna uzaktan olmasını istemezsiniz. Bir sunucu süreyi bulduğunuzda, ancak ne yüzlerce sunucu için ölçeği ve hangilerinin uzaktan ihtiyacınız bilmiyorsanız, kullanışlı olabilir? Günlüğe kaydetme, hiçbir zaman analiz etmek ve hata ayıklama için üretim sunucuları uzaktan sahip yeterli bilgi vermelidir sorunları. Böylece yalnızca günlükleri aracılığıyla sorunlarını yalıtmak yeterli bilgi günlük kaydı.

### <a name="log-in-production"></a>Üretimde uygulamaya kaydedin

Birçok kişinin açma üretimde izleme yalnızca bir sorun olduğunda ve hata ayıklamak istiyor. Bu sorun bilmediğiniz zaman ilgili yararlı sorun giderme bilgileri alışınızda arasında önemli bir gecikme ortaya çıkarabilir. Ve size bilgi aralıklı hatalar için yararlı olmayabilir.

Üretimde oturum her zaman bırakın olan depolama ucuz olduğu bulut ortamında ne öneririz. Bunları oturum zaten var ve yardımcı olabilecek bir geçmiş verilere sahip olduğunuz hataları meydana geldiğinde bu şekilde, zaman içinde geliştirin veya farklı zamanlarda düzenli aralıklarla gerçekleşecek sorunları çözümleyin. Eski günlüklerin silineceği bir temizleme işlemini otomatikleştirmek, ancak günlükleri tutmak olduğundan bu tür bir işlemi kurmak daha pahalı olduğunu düşünebilirsiniz.

Eklenen bir günlük zamandan ve paradan tasarruf bir şeyler yanlış gittiğinde zaten kullanılabilir ihtiyacınız olan tüm bilgileri sağlayarak kaydedebilirsiniz sorun giderme miktarını karşılaştırıldığında Önemsiz maliyetidir. Birisi, rastgele bir hata süre yaklaşık 8:00 gece son ekipleri vardı, ancak bunlar hatanın hatırlamıyorum bildirdiğinde, daha sonra kolayca sorunun ne olduğunu göz bulabilirsiniz.

Küçüktür 4 ABD doları için 50 gigabayta kadar günlükleri elde tutma ay ve günlüğe kaydetme performans etkisi Önemsiz aklımızdan--günlük kitaplığınızı zaman uyumsuz olduğundan emin olun, performans sorunlarını önlemek için bir şey tutmak sürece.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Eylem gerektiren günlüklerinden bildirmek günlükleri ayırt

Günlükleri (size bir şey bilmek istiyorum) INFORM veya YASASI (bir şey yapmanızı istiyorum) yöneliktir. Yalnızca gerçekten bir kişi veya otomatik bir işlem eylemi gerçekleştirmek için gereken sorunları için hareket günlüklerini yazma konusunda dikkatli olun. Çok fazla sayıda eylem günlükleri gürültü, hiç orijinal sorunları bulmak için geliştirmek üzere çok fazla iş gerektiren oluşturacaksınız. Ve hareket günlüklerinizi personel desteklemek için e-posta gönderme gibi bazı eylemleri otomatik olarak geçirirseniz, tek bir sorundan tetiklenmesinin gerektiği gibi işlemleri binlerce izin vererek kaçının.

.NET System.Diagnostics izleme günlükleri, hata, uyarı, bilgi ve hata ayıklama/ayrıntı düzeyi atanabilir. Eylem günlükleri için hata düzeyini ayırma ve düşük düzeylerde INFORM günlüklerini kullanarak INFORM günlüklerinden YASASI ayırt edebilirsiniz.

![Günlüğe kaydetme düzeyleri](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Çalışma zamanında günlüğe kaydetme düzeylerini yapılandırma

Üretimde her zaman oturum açmak için faydalı olsa da, başka bir en iyi uygulama, çalışma zamanında düzeyi oturum açtığınızdan ayrıntı, uygulamanızı yeniden başlatmayı veya yeniden dağıtmaya gerek kalmadan ayarlamanıza olanak sağlayan bir günlüğe kaydetme çerçevesi uygulamaktır. Örneğin bir izleme yerde kullandığınızda `System.Diagnostics` oluşturabilirsiniz hata, uyarı, bilgi ve hata ayıklama/Verbose günlüğe kaydeder. Her zaman hata, uyarı, oturum ve üretimde bilgileri günlüğe kaydeder ve olay temelinde sorun giderme için hata ayıklama/Verbose günlüğü dinamik olarak ekleme yapmak isteyeceksiniz öneririz.

Yazma için yerleşik desteğe sahiptir. Azure App Service'te Web Apps `System.Diagnostics` günlüklerinin dosya sistemi, tablo depolama veya Blob Depolama. Her Depolama hedefi farklı günlük düzeylerini seçebilirsiniz ve uygulamanızı yeniden başlatmadan hareket halindeyken günlüğe kaydetme düzeyini değiştirebilirsiniz. Blob Depolama desteği çalıştırmayı kolaylaştırır [HDInsight](https://docs.microsoft.com/azure/hdinsight/) analiz HDInsight Blob Depolama ile doğrudan çalışmak nasıl bildiğinden, uygulama günlüklerini, işler.

### <a name="log-exceptions"></a>Günlük özel durumları

Yalnızca koymayın *özel durum. ToString()* günlük kod. Bağlamsal bilgi bırakır. SQL hata olması durumunda, SQL hata numarası bırakır. Tüm özel durumlar için bağlam bilgisi, özel durumu ve sorun giderme için gereken her şeyi sağlanmaktadır emin olmak için iç özel durumlar içerir. Örneğin, sunucu adı, bir işlem tanımlayıcısı ve kullanıcı adı (ancak parola veya herhangi bir gizli anahtar!) bağlam bilgilerini içerebilir.

Her geliştirici, özel durum günlüğe kaydetme için doğru şeyi bağlıdır, bazıları olmaz. Özel durum işleme sağ Günlükçü arabirimine yolu her zaman sağ bitti emin olmak için derleme: özel durum nesnesi kendisini Günlükçü sınıfına geçirin ve özel durum verileri Günlükçü sınıfında düzgün bir şekilde oturum.

### <a name="log-calls-to-services"></a>Günlük aramaları Hizmetleri

Bir veritabanı veya bir REST API veya herhangi bir dış hizmeti için olup olmadığını her zaman bir hizmet için uygulamanızı çağırır günlüğünü yazdığınız önemle öneririz. Günlükleri yalnızca bir gösterge başarı veya başarısızlık ancak her isteğin ne kadar sürdüğünü içerir. Bulut ortamında tam kesintiler yerine slow-downs ilgili sorunlar genellikle görürsünüz. Normalde 10 milisaniyeden gereken bir şey aniden ikinci alma başlayabilir. Birisi, uygulamanızın yavaş olduğunu bildirir, New Relic göz atabilmek istediğiniz veya varsa ve doğrulamak istediğiniz telemetri hizmeti deneyimlerini ve ardından göz atabilmek istiyor musunuz neden yavaş olduğu ayrıntılara inin için kendi günlükleri.

### <a name="use-an-ilogger-interface"></a>Bir ILogger arabirimini kullanma

Ne bir üretim uygulaması oluşturduğunuzda yapılması önerilir basit oluşturmaktır *ILogger* arabirimi ve bazı yöntemler Yapıştır. Bu, daha sonra günlük kaydı uygulamasını değiştirin ve yapmak için tüm kodunuzu Git yok kolaylaştırır. Biz kullanabilecek `System.Diagnostics.Trace` sınıfı Düzelt uygulama boyunca, ancak bunun yerine, aslında uygulayan günlük sınıfında altında kullanıyoruz *ILogger*, ve vermiyoruz *ILogger* boyunca yöntemini çağırır uygulama.

Bu şekilde, şimdiye kadar günlüğe kaydetme daha zengin, hale getirmek isterseniz değiştirebilirsiniz [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) hangi günlüğe kaydetme mekanizmasıyla istediğiniz. Uygulamanız büyüdükçe, sizin gibi daha kapsamlı bir günlük paketi kullanmak istediğiniz karar verebilirsiniz [NLog](http://nlog-project.org/) veya [Kurumsal kitaplığı günlük uygulama bloğu](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) başka bir yaygın günlüğe kaydetme çerçevesi ancak zaman uyumsuz günlük kaydı yapmaz.)

Çıkış günlüğü ayrı yüksek hacimli ve yüksek değerli veri depolarına bölme kolaylaştırmak için bir çerçeve NLog gibi kullanarak olası bir nedeni var. Bu, büyük hacimli hızlı hareket verilerine erişim korurken, hızlı sorgu yürütmek için ihtiyacınız olmayan INFORM verileri verimli bir şekilde depolamanıza yardımcı olur.

### <a name="semantic-logging"></a>Semantik günlük kaydı

Nispeten yeni bir yolunu daha kullanışlı tanılama bilgileri üretmek günlük kaydı yapmak bilgi için bkz: [Kurumsal kitaplığı semantik günlük uygulama bloğu (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). BLOK kullanan [olay izleme için Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) ve [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) destekleyen .NET 4.5 içinde daha fazla yapılandırılmış ve sorgulanabilir günlükleri oluşturmanıza olanak sağlar. Her yazdığınız bilgileri özelleştirmenize olanak tanıyan, oturum, olay türü için farklı bir yöntem tanımlayın. Örneğin, bir SQL veritabanı hatası çağrı oturum için bir `LogSQLDatabaseError` yöntemi. Bu özel durum türü için hata sayı parametresi Yöntem imzasında içerir ve hata numarasını yazdığınız günlük kaydı ayrı bir alan olarak kaydetmek için hata numarası bir önemli bilgilerden biri olduğunu biliyorsunuz demektir. Ayrı bir alana olduğundan yalnızca bir ileti dizeye hata numarasını birleştirerek oranla SQL hatası rakamlara dayanan raporları daha kolay ve güvenilir bir şekilde elde edebilirsiniz.

## <a name="logging-in-the-fix-it-app"></a>Hatayı günlüğe kaydetme uygulama

### <a name="the-ilogger-interface"></a>ILogger arabirimi

İşte *ILogger* düzeltme uygulama arabirimi.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Bu yöntemler tarafından desteklenen aynı dört düzeylerinde günlüklerini yazma izni sağlayan *System.Diagnostics*. Gecikme süresi hakkında bilgi içeren dış hizmet çağrıları günlüğe TraceApi yöntemlerdir. Bir dizi hata ayıklama/ayrıntı düzeyi için bir yöntem de ekleyebilirsiniz.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Günlükçü uygulamasını ILogger arabirimi

Gerçekten Basit arabirimi uygulamasıdır. Temel olarak yalnızca standart çağırdığı *System.Diagnostics* yöntemleri. Aşağıdaki kod parçacığı, üç bilgi yöntemleri ve her biri diğerlerine gösterir.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>ILogger yöntemleri çağırma

Düzeltme uygulama kodunda bir özel durumu yakalar her zaman, onu çağıran bir *ILogger* yöntemi özel durum ayrıntıları oturum. Ve veritabanı, Blob hizmeti veya bir REST API'sine çağrıda bulunur her zaman bir kronometre çağırmadan önce başlatır ve hizmetin döndürdüğü kronometre durdurur ve geçen süreyi başarı veya başarısızlık durumu hakkında bilgi ile birlikte günlüğe kaydeder.

Günlük ileti sınıfı adı ve yöntem adı içerdiğine dikkat edin. Günlük iletilerini uygulama kodu hangi kısmının bunları yazdı tanımladığından emin olmak için iyi bir uygulamadır.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

SQL veritabanı için bir çağrı yaptığını düzeltme uygulama şimdi her zaman için tam olarak ne kadar zaman sürdü ve çağrı, çağıran yönteme görebilirsiniz.

![SQL veritabanı sorgu günlükleri](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Günlükleri göz atma gidin, veritabanı çağrıları geçirmeniz gereken süreyi değişkeni olduğunu görebilirsiniz. Bilgiler yararlı olabilir: Bu tüm uygulama günlüklerini olduğundan, veritabanı hizmeti zaman içinde nasıl performans gösterdiğini, geçmiş eğilimleri analiz edebilirsiniz. Örneğin, bir hizmet çoğu zaman hızlı olabilir ancak istekleri başarısız olabilir veya yanıtları günün belirli zamanlarında yavaşlayabilir.

Blob hizmeti için– de aynı uygulama yeni bir dosya yükler, bir günlük yoktur ve her dosyayı karşıya yüklemek için tam olarak ne kadar sürdüğünü görebilirsiniz her zaman yapabileceğiniz.

![Blobu karşıya yükleme günlüğü](monitoring-and-telemetry/_static/image23.png)

Bu, yalnızca birkaç ek bir hizmetini çağırmak, ve bunlar bir sorunla çalışan birisi diyor olduğunda, artık tam olarak bir sorun, bir hata oluştu veya yalnızca yavaş çalıştığı bile neydi bilmeniz her zaman yazmak için kod satırlarını olur. Sorunun kaynağının bir sunucuya uzaktan zorunda kalmadan belirlemenize veya hata olur ve yeniden yaratmayı ümit sonra oturum açın.

## <a name="dependency-injection-in-the-fix-it-app"></a>Bağımlılık ekleme düzeltme, uygulama

Yukarıda gösterilen örnek depo oluşturucuda Günlükçü arabirim uygulamasına nasıl alacağına merak ediyor:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Arabirimi oluşturan uygulama uygulamasına kablo kullanan [bağımlılık ekleme](http://en.wikipedia.org/wiki/Dependency_injection)(dı) ile [AutoFac](http://autofac.org/). DI, kodunuzun pek çok yerdeki bir arabirimdeki bağlı olarak bir nesnenin kullanın ve yalnızca tek bir yerde arabirimi örneği oluşturulduğunda kullanılan uygulamanız belirtmeniz gerekir sağlar. Bu sayede daha kolay uygulama değiştirmek: Örneğin, System.Diagnostics Günlükçü bir NLog Günlükçü ile değiştirmek isteyebilirsiniz. Veya otomatik test için Günlükçü sahte bir sürümüne yerine isteyebilirsiniz.

Düzeltme uygulama DI tüm depolar ve denetleyicilerden biri de kullanır. Oluşturucularını denetleyicisi sınıflarının bir *ITaskRepository* depoyu alır bir Günlükçü arabirimi aynı şekilde arabirim:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Uygulama, otomatik olarak sağlamak için AutoFac DI kitaplığı kullanan *TaskRepository* ve *Günlükçü* örnekleri için bu oluşturucular.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Bu kod temelde herhangi bir yerde bir oluşturucu gerektiğini bildiren bir *ILogger* arabirim, içinde bir örneğini geçirin *Günlükçü* sınıfı ve ihtiyaç duyduğu her bir *IFixItTaskRepository*arabirim, içinde bir örneğini geçirin *FixItTaskRepository* sınıfı.

[AutoFac](http://autofac.org/) kullanabileceğiniz birçok bağımlılık ekleme çerçeveleri biridir. Başka bir yaygın bir [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), hangi önerilen ve Microsoft Patterns and Practices tarafından destekleniyor.

## <a name="built-in-logging-support-in-azure"></a>Azure'da yerleşik günlük desteği

Azure aşağıdaki tür destekler, [Azure App service'taki Web Apps için oturum](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- (Açmak ve kapatmak ve siteyi yeniden başlatmanıza gerek kalmadan hızla düzeylerini ayarlama) System.Diagnostics izleme.
- Windows olayları.
- IIS günlükleri (HTTP/FREB).

Azure aşağıdaki tür destekler, [bulut hizmetlerinde oturum](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics izleme.
- Performans sayaçları.
- Windows olayları.
- IIS günlükleri (HTTP/FREB).
- Özel dizin izleme.

Düzeltme uygulama System.Diagnostics izleme kullanır. Tek bir web uygulamasında oturum System.Diagnostics etkinleştirmek için yapmanız gereken, portalda bir anahtara çevirmek veya REST API çağrısı. Portalında **yapılandırma** siteniz için sekmesinde ve görmek için aşağı kaydırmanız **Application Diagnostics** bölümü. Günlüğü Aç veya kapat ve istediğiniz günlüğe kaydetme düzeyini seçin. Azure günlüklerini dosya sistemine veya bir depolama hesabına yazma olabilir.

![Uygulama tanılama ve yapılandırma sekmesinde site tanılama](monitoring-and-telemetry/_static/image24.png)

Azure'da oturum etkinleştirdikten sonra oluşturuldukları sırada günlüklerini Visual Studio çıktı penceresinde görebilirsiniz.

![Akış günlükleri menüsü](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Akış günlükleri menüsü](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Depolama hesabınıza yazılır günlükleri de sahip olabilir ve bunları tüm aracı görünümü erişim sağlayabilir Azure depolama tablo hizmeti gibi **Sunucu Gezgini** Visual Studio'da veya [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

![Sunucu Gezgini'nde günlükleri](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Özet

Bir kullanıma hazır telemetri sistemi uygulamak, kendi kodunuzda oturum izleme ve Azure'da oturum açmayı yapılandırmak gerçekten çok basit bir işlemdir. Ve üretim sorun olduğunda, telemetri sisteminin ve özel günlükler birleşimi, müşterileriniz için önemli bir sorun haline gelmeden önce sorunları hızlıca çözmenize yardımcı olur.

İçinde [sonraki bölümde](transient-fault-handling.md) araştırmak için sahip olduğunuz üretim sorunlarını olursunuz yoksa, geçici hataları işlemek nasıl göz atacağız.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın.

Telemetri genellikle ilgili belgeler:

- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). İzleme ve Telemetri Kılavuzu, hizmet ölçümü Kılavuzu, sistem durumu uç nokta izleme düzeni ve çalışma zamanı yapılandırması düzeni bakın.
- [Bulutta hareketinden kuruş: Azure Web Siteleri'nde yeni Relic performans etkinleştirme](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Azure bulut Hizmetleri'nde büyük ölçekli hizmetler tasarlamak için en iyi uygulamalar](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Mark Simms'in ve Michael Thomassy teknik incelemesinde yanıtlanmıştır. Telemetri ve Tanılama bölümüne bakın.
- [Application Insights ile yeni nesil geliştirme](https://msdn.microsoft.com/magazine/dn683794.aspx). MSDN dergisi makalesi.

Esas olarak günlüğe kaydetme hakkında belgeler:

- [Semantik günlük uygulama bloğu (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Semantik günlük BLOK ile durum Neil Mackenzie sunar.
- [Semantik günlük kaydı ile yapılandırılmış ve anlamlı günlükleri oluşturma](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Jülyen Dominguez çalışması için BLOK ile anlamlı günlük kaydını gösterir.
- [EF6 SQL günlük – bölüm 1: Basit günlüğe kaydetme](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers EF 6 Entity Framework tarafından yürütülen sorgular oturum işlemi gösterilmektedir.
- [Bağlantı dayanıklılığı ve komut durdurma bir ASP.NET MVC uygulamasındaki Entity Framework ile](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Dördüncü dokuz bölümlü öğretici serisinde, veritabanına Entity Framework tarafından gönderilen SQL komutları oturum EF 6 komut durdurma özelliğini kullanmayı gösterir.
- [C# 5.0 arayan bilgisi özniteliklerini kullanarak günlük geliştirmek](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Nasıl kolayca oturum sabit değişmez değerleri kodlama veya el ile almak için yansıma kullanarak çağıran yöntemin adı.

Çoğunlukla sorun giderme hakkında belgeler:

- [Azure sorun giderme &amp; blog hata ayıklama](https://blogs.msdn.com/b/kwill/).
- [AzureTools – tanılama Azure Geliştirici Desteği ekibi tarafından kullanılan yardımcı programı'nı](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Tanıtır ve Azure sanal makinesine indirin ve çok çeşitli tanılama ve izleme araçları çalıştırmak için kullanılabilir bir aracı için indirme bağlantısı sağlar. Belirli bir VM'nin bir sorunu tanılamak gerektiğinde yararlıdır.
- [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). System.Diagnostics izleme ve uzaktan hata ayıklama ile çalışmaya başlama hakkında adım adım öğretici.

Videolar:

- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden oluşan Ulrich Homann, Marc Mercuri ve Mark Simms'in. Üst düzey kavramlarını ve mimari ilkeleri gerçek müşterilerle Microsoft Müşteri danışma ekibi (CAT) deneyiminden çizilmiş hikayeleri çok erişilebilir ve ilgi çekici bir biçimde sunar. Bölüm 4. ve 9, izleme ve telemetri hakkında ' dir. Bölüm 9 MetricsHub, AppDynamics, New Relic ve PagerDuty Hizmetleri'ni izlemeye genel bakış içerir.
- [Yapı büyük: Azure müşterilerinin - Bölüm II dersler](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms'in hata için tasarlama ve her şeyi izleme hakkında konuşuyor. Benzer şekilde hatasız serisi ancak daha fazla nasıl yapılır ayrıntıya gider.

Örnek kod:

- [Bulut hizmeti temel bilgileri Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Örnek uygulama, Microsoft Azure Müşteri danışma ekibi tarafından oluşturuldu. Aşağıdaki makaleler de açıklandığı gibi hem telemetri ve günlüğe kaydetme yöntemleri gösterir. Örnek Uygulama günlüğünü kullanarak uygulayan [NLog](http://nlog-project.org/). İlgili belgeler için bkz. [telemetri ve günlüğe kaydetme hakkında dört TechNet wiki makaleleri dizisi](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Önceki](design-to-survive-failures.md)
> [İleri](transient-fault-handling.md)
