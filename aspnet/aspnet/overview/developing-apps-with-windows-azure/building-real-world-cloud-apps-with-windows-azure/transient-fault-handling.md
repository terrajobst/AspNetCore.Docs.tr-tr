---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: "Geçici hata işleme (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: b743b04789c5e5ebf5ab922cf34a516a16a6d356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Geçici hata işleme (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Gerçek dünya bulut uygulama tasarlarken, yapılandırmanızda şeyleri geçici olarak hizmet kesintileri nasıl ele alınacağını biridir. Ağ bağlantıları ve dış hizmetler böylece bağımlı olduğundan bu sorunu bulut uygulamalarını benzersiz olarak önemlidir. Genellikle kendi kendini iyileştirme az hatalardan sık alabilir ve akıllıca işlemeye hazır değilseniz, müşterileriniz için hatalı bir deneyimi bunlar neden.

## <a name="causes-of-transient-failures"></a>Geçici hataları nedenleri

Bulut ortamında, başarısız olan ve bağlantıları düzenli aralıklarla meydana veritabanı bırakıldı bulabilirsiniz. Kısmen, şirket içi ortama karşılaştırıldığında daha fazla yük dengeleyici ile web sunucusu ve veritabanı sunucusu doğrudan fiziksel bağlantısına sahip olduğu oluşturacağız olmasıdır. Ayrıca, bazen bir çok kiracılı hizmetine bağımlı zaman başka birinin hizmet kullanan, yoğun basarsa için hizmet get yavaş veya zaman aşımına çağrıları görürsünüz. Diğer durumlarda hizmet çok sık basarsa kullanıcının olabilir ve hizmet kasıtlı olarak, – bağlantıları reddeder – diğer kiracılar hizmetinin olumsuz yönde etkilemesini önlemek için kısıtlar.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Geçici hataları etkisini azaltmak için Yeniden Dene'yi/geri alma akıllı mantığı kullanın

Bir özel durum atma ve müşterinize mevcut değil ya da hata sayfası görüntüleme, yerine genellikle geçici hataları tanıyabilir ve uzun başarılı olacak önce otomatik olarak hata ile sonuçlandı işlemi yeniden deneyin, planlamaktadır. Çoğu zaman ikinci denemede işlemi başarılı olur ve herhangi bir sorun oluştu kullanan bırakıldı müşteri hatadan kurtarmak.

Akıllı yeniden deneme mantığı uygulayabilirsiniz birkaç yolu vardır.

- Microsoft Patterns &amp; yöntemler grubuna sahip bir [geçici hata işleme uygulama bloğu](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) yapan her şeyi sizin için SQL veritabanı erişimi (değil aracılığıyla Entity Framework) için ADO.NET kullanıyorsanız. Bir sorguyu yeniden denemek için kaç kez yeniden deneme – ilke ayarlamanız yeterlidir veya komutunu ve ne kadar süre bekleneceğini çalıştığında – arasında kaydırma SQL kod içinde bir *kullanarak* bloğu.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH de destekler [Azure rol içi önbelleği](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) ve [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Entity Framework kullandığınızda genellikle doğrudan SQL bağlantıları ile bu desenleri ve uygulamalar paket kullanamaz, ancak Entity Framework 6 Bu tür bir yeniden deneme mantığı Framework'e sağ derlemeler için çalışma değil. Benzer şekilde yeniden deneme stratejisini belirlemek ve veritabanını eriştiğinde EF bu strateji kullanır.

    Düzelt uygulamada bu özelliği kullanmak için size gereken yapmak için tek şey türeyen bir sınıf ekleyin *DbConfiguration* ve yeniden deneme mantığına açın.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Genellikle geçici hataları olarak framework tanımlayan SQL veritabanı için özel durumlar, gösterilen kodu EF işlemi gecikmeyle bir üstel geri alma yeniden deneme ve en büyük gecikme 5 saniye arasında 3 kereye kadar yeniden denemek için bildirir. Üstel geri alma başarısız denemeler sonra bunu yeniden denemeden önce daha uzun bir süre için bekleyeceğini anlamına gelir. Bir satırda üç deneme başarısız olursa, bir özel durum oluşturur. Aşağıdaki bölümde hattı ayırıcıları hakkında üstel geri alma ve yeniden deneme sayısı sınırlı neden istediğiniz açıklanmaktadır.

    BLOB'lar için Düzelt uygulama yapar ve .NET depolama İstemcisi API zaten mantığı aynı türde uygulayan gibi Azure Storage hizmeti kullanıyorsanız benzer sorunlar olabilir. Yeniden deneme ilkesi yalnızca belirtin veya varsayılan ayarlarla memnun kaldıysanız, bunu yapmak bile yok.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Bağlantı hattı ayırıcıları

Çok fazla kez çok uzun bir süre içinde yeniden denemek için neden istemediğiniz birkaç nedeni vardır:

- Kalıcı olarak başarısız isteklerin yeniden deneniyor çok sayıda kullanıcı, diğer kullanıcıların deneyimini azaltabilir. Milyonlarca insan tüm yinelenen yapma isteği yeniden deneyin varsa, IIS gönderme sıralarını bağlamadan ve uygulamanızı, aksi halde başarıyla ele alabilecek istekleri bakım engelliyor.
- Çok sayıda istek, sıraya alınması herkes hizmet hatası nedeniyle, var. deniyor varsa kurtarmak başlatıldığında, hizmet taşan alır.
- Azaltma nedeniyle hatadır ve azaltma için hizmetin kullandığı zaman penceresi ise sürekli yeniden deneme bu pencereyi taşımak ve devam etmek azaltma neden.
- İşlemek için bir web sayfası için bekleyen bir kullanıcı olabilir. Yapma kişiler bekleme uzun daha bu oldukça hızlı bir şekilde bunları daha sonra yeniden denemek için bildiren sinir bozucu.

Üstel geri alma bazı Bu sorun bir hizmet uygulamanızdan alabilirsiniz yeniden deneme sıklığını sınırlayarak alır. Ayrıca sahip olmanız gerekir, ancak *hattı ayırıcıları*: Bu belirli bir uygulamanızı yeniden deneniyor durdurur ve aşağıdaki gibi diğer bazı eylem eşik yeniden deneme anlamına gelir:

- Özel geri dönüş. Reuters hisse senedi fiyatı alınamıyor, belki de, Bloomberg edinebilirsiniz; veya veritabanından veri alınamıyor, belki de, Önbelleği'nden edinebilirsiniz.
- Sessiz başarısız. Bir hizmetinden gerekenler uygulamanız için ya hep ya hiç değilse, verileri alınamıyor, yalnızca null döndürür. Düzelt görev görüntülediğinizden ve Blob hizmeti yanıt vermiyor, görüntü olmadan görev ayrıntılarını görüntüleyebilir.
- Hızlı başarısız. Hata hizmetiyle taşmasını önlemek için kullanıcı çıkışı oluşturulamadı. hizmet kesintisi diğer kullanıcılar için neden veya daraltma penceresi genişletmek isteklerinin yeniden deneyin. Kolay bir "daha sonra yeniden deneyin." iletisi görüntüleyebilirsiniz.

BT'ye yeniden deneme ilkesi bulunmamaktadır. Birden fazla kez yeniden deneyin ve burada bir kullanıcı için bir yanıt bekliyor. bir zaman uyumlu web uygulamasında yaptığınız daha uzun bir zaman uyumsuz arka planda çalışan işleminde bekleyin. Önbellek hizmeti için yaptığınız daha uzun bir ilişkisel veritabanı hizmeti için yeniden denemeler arasında bekleyebilirsiniz. İşte önerilen bazı örnek sayıları nasıl değişebilir bir fikir vermek için ilkeler yeniden deneyin. ("Hızlı ilk" gecikme ilk yeniden denemeden önce gelir.

![Örnek yeniden deneme ilkelerini](transient-fault-handling/_static/image1.png)

SQL veritabanı yeniden deneme ilkesi yönergeler için bkz [geçici hataları ve SQL veritabanına bağlantı hatalarıyla ilgili sorunları giderme](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Özet

Bir yeniden deneme/geri alma stratejisi geçici hataları görünmez müşteriye çoğu zaman hale gelmesine yardımcı olabilecek ve Microsoft ADO.NET, Entity Framework ya da Azure kullanıyorsanız olup olmadığını bir strateji uygulama çalışmanızı en aza indirmek için kullanabileceğiniz çerçeveleri sağlar Depolama hizmeti.

İçinde [sonraki bölümde](distributed-caching.md), performansı iyileştirmek nasıl tümleştirildiği incelenmektedir ve güvenilirlik kullanarak dağıtılmış önbelleğe alma.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

Belgeler

- [En iyi uygulamalar için Azure bulut hizmetleri üzerinde büyük ölçekli hizmetler tasarımını](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Teknik incelemesi işareti SIMM'lerinin ve Michael Thomassy tarafından. Hatasız serisi ancak yapılır Ayrıntılar gerçekleştirip gerçekleştirmeyeceğini benzer. Telemetri ve Tanılama bölümüne bakın.
- [Hatasız: Dayanıklı bulut mimarileri Kılavuzu](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Teknik incelemesi Marc Mercuri, Ulrich Homann ve Barış Townhill tarafından. Web sayfası hatasız video serisi sürümü.
- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Yeniden deneme bkz düzeni, Zamanlayıcı Aracısı yönetici düzeni.
- [Hataya dayanıklı Azure SQL veritabanında](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Blog gönderisi Tony Petrossian tarafından.
- [Entity Framework - bağlantı dayanıklılığı / yeniden deneme mantığı](https://msdn.microsoft.com/data/dn456835). Entity Framework 6 özelliği işleme geçici hata özelleştirmek ve kullanmak nasıl.
- [Bağlantı dayanıklılığı ve ASP.NET MVC uygulamasındaki Entity Framework komut kişiler tarafından ele](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Dördüncü dokuz bölümlü öğretici serisinde EF 6 bağlantı esnekliği özelliği ayarlamak için SQL Database gösterilmiştir.

Videolar

- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Dokuz parçalı seriyi Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin. Üst düzey kavramlarını ve mimari ilkeler çok erişilebilir ve ilginç yolla, Microsoft Müşteri danışma ekibi (CAT) deneyiminde gerçek müşterilerle çizilmiş hikayeleri sunar. Bağlantı hattı bölüm 3 40:55 başlatırken ayırıcıları tartışma bakın.
- [Yapı büyük: Azure müşterilerden - bölümü II dersleri öğrenilen](https://channel9.msdn.com/Events/Build/2012/3-030). Hata, geçici tasarlama hakkında işareti SIMM'lerinin konuşmaları işleme ve her şeyi düzenleme hata.

kod örneği

- [Bulut hizmeti temel bilgileri Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Örnek Microsoft Azure Müşteri danışma nasıl kullanılacağını gösteren ekibi tarafından oluşturulan uygulama [Kurumsal kitaplığı geçici hata Engellemesi işleme](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Daha fazla bilgi için bkz: [bulut hizmeti temelleri veri erişim katmanı – geçici hata işleme](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH (Entity Framework kullanarak doğrudan olmadan) ADO.NET kullanarak veritabanı erişimi için önerilir.

>[!div class="step-by-step"]
[Önceki](monitoring-and-telemetry.md)
[sonraki](distributed-caching.md)
