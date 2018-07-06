---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Geçici hata işleme (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: e67f3106f060d52f90ba56d6684af64779009e39
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823384"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Geçici hata işleme (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Gerçek bulut uygulaması tasarlarken dikkat etmeniz gereken şeyleri geçici hizmet kesintilerini işlemek nasıl biridir. Bu nedenle ağ bağlantıları ve dış hizmetlere bağımlı olduğundan bu sorun bulut uygulamalarındaki benzersiz olarak önemlidir. Sık genellikle kendi kendine iyileştirme küçük hataları alabilirsiniz ve akıllıca işlemek hazır değilseniz, müşterileriniz için kötü bir deneyimle bunlar neden.

## <a name="causes-of-transient-failures"></a>Geçici hatalar nedenleri

Bulut ortamında, veritabanı bağlantıları düzenli aralıklarla gerçekleşecek olan ve bulabilirsiniz. Kısmen şirket içi ortama kıyasla daha fazla yük dengeleyicileri aracılığıyla doğrudan bir fiziksel bağlantı web sunucusu ve veritabanı sunucusuna sahip olduğu gideceğinizi olmasıdır. Ayrıca, çok kiracılı bir hizmet bağlı olduğunuzda bazen başka birisi hizmetin kullandığı, yoğun geldikçe için hizmet get daha yavaş veya zaman aşımı çağrı karşılaşırsınız. Diğer durumlarda, hizmet çok sık ulaşmaktan kullanıcının olabilir ve hizmetin kasıtlı olarak size – bağlantıları reddeder – diğer kiracıların hizmeti olumsuz yönde etkilemesini önlemek için kısıtlar.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Geçici hatalar etkisini azaltmak için akıllı yeniden deneme/geri alma mantığını kullanın

Bir özel durum ve kullanılabilir değil ya da hata sayfası görüntüleme müşterinize, yerine genellikle geçici hataları tanıyabilir ve kadar başarılı olacaktır önce otomatik olarak hatayla sonuçlanan işlemi yeniden deneyin, planlamaktadır. Çoğu zaman ikinci denemede işlemi başarılı olur ve hiç olmadığı kadar bir sorun oluştu uyumlu olan müşteri hatadan kurtarmayı.

Akıllı yeniden deneme mantığını uygulayabilir birkaç yolu vardır.

- Microsoft Patterns &amp; yöntemler grup olan bir [geçici hata işleme uygulama bloğu](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) , her şeyi sizin için yapar (değil ile Entity Framework) SQL veritabanı erişimi için ADO.NET kullanıyorsanız. Bir ilke için bir sorguyu yeniden denemek için kaç kez yeniden deneme – ayarlamanız yeterlidir veya komut ve ne kadar bekleneceğini çalıştığında – arasında kaydırma SQL kod içinde bir *kullanarak* blok.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Ayrıca TFH destekler [Azure rol içi önbellek](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) ve [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Varlık çerçevesi kullandığınızda bu desenleri ve uygulamaları paket kullanamaz, ancak Entity Framework 6 Bu tür bir yeniden deneme mantığı çerçeveye sağ oluşturur. Bu nedenle genellikle doğrudan SQL bağlantıları ile çalışmayan. Benzer şekilde, yeniden deneme stratejisi belirtin ve veritabanı eriştiğinde EF bu strateji kullanır.

    Düzelt uygulamasında bu özelliği kullanmak için biz yapmanız gereken tek şey, türetilen bir sınıf ekleyin *DbConfiguration* ve yeniden deneme mantığı üzerinde açın.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Framework genellikle geçici hata olarak tanımlayan SQL veritabanı için özel durumlar, gösterilen kod EF bir üstel geri alma arasında gecikme yeniden deneme sayısı, en fazla gecikme 5 saniye ile 3 kereye kadar yeniden yönlendirir. Üstel geri alma başarısız denemeler sonra bunu yeniden denemeden önce uzun bir süre beklemesini anlamına gelir. Bir satırdaki üç deneme başarısız olursa, bir özel durum oluşturur. Devre kesicilerle ilgili aşağıdaki bölümde, üstel geri alma ve sınırlı sayıda yeniden deneme nedeninizi açıklanmaktadır.

    Azure depolama hizmeti, BLOB'ları için Düzelt uygulamanın yapabildikleri ve .NET depolama istemci API'si zaten aynı türde bir mantık uygulayan kullanırken benzer sorunlar olabilir. Hemen yeniden deneme ilkesi belirtin veya varsayılan ayarlarla memnun kaldıysanız, bunu yapmak bile yok.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Devre Kesiciler

Çok fazla kez çok uzun bir dönem boyunca yeniden denemek için neden istemediğiniz birkaç nedeni vardır:

- Çok sayıda kullanıcı kalıcı olarak başarısız isteklerin yeniden deneniyor, diğer kullanıcıların deneyimini düşebilir. Milyonlarca insan tüm yinelenen yapmadan istekleri yeniden deneme kullanıyorsanız, IIS gönderme sıralarını tümleştirilerek ve uygulamanızı, aksi halde başarıyla ele alabilecek isteklere hizmet dan engelleyen.
- Çok sayıda istek, sıraya alınamadı herkesin bir hizmet hatası nedeniyle, orada deniyor varsa, kurtarılır başlatıldığında, hizmet yığılma alır.
- Azaltma nedeniyle hatadır ve bir pencere için azaltma hizmetin kullandığı süre yoksa sürekli yeniden deneme o pencereyi dışarı Taşı ve devam etmek azaltma neden.
- İşlenecek web sayfası için bekleyen bir kullanıcı olabilir. Yapma, kişiler, bekleme uzun daha bu görece hızlı bir şekilde bunları daha sonra yeniden denemek için bildiren sinir bozucu.

Üstel geri alma yeniden deneme uygulamanızdan bir hizmet alabilirsiniz sıklığını sınırlayarak bazı bu sorunu giderir. Ancak ayrıca ihtiyacınız *devre Kesiciler*: belirli bir uygulamanızı yeniden deneniyor durdurur ve aşağıdakilerden birini gibi diğer bazı işlem eşiği yeniden anlamına gelir:

- Geri dönüş özel. Belki de Reuters stok fiyatını alınamıyor, Bloomberg elde edeceğinizi; veya veritabanından veri alınamıyor, belki de önbellekten ulaşabilirsiniz.
- Sessiz başarısız. Bir hizmetten gerekenler uygulamanız için zorlamadan değilse, verilerini alamaması durumunda yalnızca null döndürür. Blob hizmeti yanıt vermiyor ve Düzelt görev görüntülediğiniz, görüntü olmadan görev ayrıntılarını görüntüleyebilir.
- Hızlı başarısız olma. Kullanıcının hizmete taşmasını önlemek için hata ya da diğer kullanıcılar için hizmet kesintisi neden daraltma penceresi genişletmek istekleri yeniden deneyin. Kolay bir "daha sonra yeniden deneyin" iletisi görüntüleyebilirsiniz.

BT'ye yeniden deneme ilkesi bulunmamaktadır. Daha fazla yeniden deneme ve bir kullanıcının yanıt burada bekleyen bir zaman uyumlu web uygulaması olduğu kadar temkinli daha uzun bir zaman uyumsuz bir arka plan çalışan işlemindeki bekleyin. Bir önbellek hizmeti için olduğu kadar temkinli artık bir ilişkisel veritabanı hizmeti için yeniden denemeler arasındaki bekleyebilirsiniz. İşte önerilen bazı örnek sayılar nasıl değişebilir bir fikir vermek için ilkeler yeniden deneyin. ("Hızlı ilk" ilk yeniden deneme yok gecikme anlamına gelir.

![Örnek yeniden deneme ilkeleri](transient-fault-handling/_static/image1.png)

SQL veritabanı yeniden deneme ilkesi yönergeler için bkz. [geçici hataların ve SQL veritabanı için bağlantı hatalarını giderme](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Özet

Bir yeniden deneme/geri alma stratejisi geçici hatalar görünmez müşteriye çoğu zaman hale getirmenize yardımcı olabilir ve Microsoft, ADO.NET, Entity Framework ya da Azure kullanıyor olmanızdan bağımsız bir stratejisi uygularken, çalışmayı en aza indirmek için kullanabileceğiniz çerçeveleri sağlar. Depolama hizmeti.

İçinde [sonraki bölümde](distributed-caching.md), performansı artırmak nasıl inceleyeceğiz ve güvenilirlik kullanarak dağıtılmış önbelleğe alma.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

Belgeler

- [Azure bulut Hizmetleri'nde büyük ölçekli hizmetler tasarlamak için en iyi uygulamalar](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Mark Simms'in ve Michael Thomassy teknik incelemesinde yanıtlanmıştır. Benzer şekilde hatasız serisi ancak daha fazla nasıl yapılır ayrıntıya gider. Telemetri ve Tanılama bölümüne bakın.
- [Hatasız: Dayanıklı bulut mimarileri Kılavuzu](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Andrew Townhill Marc Mercuri ve Ulrich Homann tarafından teknik incelemesinde yanıtlanmıştır. Web sayfası hatasız video serisi sürümü.
- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Bkz. yeniden deneme düzeni, Zamanlayıcı Aracısı Gözetmeni düzeni.
- [Azure SQL veritabanı'nda hata toleransı](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Blog gönderisi Tony Petrossian tarafından.
- [Entity Framework - bağlantı dayanıklılığı / yeniden deneme mantığı](https://msdn.microsoft.com/data/dn456835). Nasıl kullanılacağı ve geçici hata işleme özelliği, Entity Framework 6 özelleştirin.
- [Bağlantı dayanıklılığı ve komut durdurma bir ASP.NET MVC uygulamasındaki Entity Framework ile](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Dördüncü dokuz bölümlü öğretici serisinde, SQL veritabanı için EF 6 bağlantı dayanıklılığı özelliğini ayarlama işlemini göstermektedir.

Videolar

- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden oluşan Ulrich Homann, Marc Mercuri ve Mark Simms'in. Üst düzey kavramlarını ve mimari ilkeleri gerçek müşterilerle Microsoft Müşteri danışma ekibi (CAT) deneyiminden çizilmiş hikayeleri çok erişilebilir ve ilgi çekici bir biçimde sunar. 3. Bölüm 40:55 başlatırken devre Kesiciler tartışılması görürsünüz.
- [Yapı büyük: Azure müşterilerinin - Bölüm II dersler](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms'in konuşmalar geçici hata için tasarlama hakkında her şeyi izleme ve işleme hata.

Kod örneği

- [Bulut hizmeti temel bilgileri Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Örnek uygulama Microsoft Azure Müşteri danışmanlık nasıl kullanılacağını gösteren ekibi tarafından oluşturulan [Kurumsal kitaplığı geçici hata işleme bloğu](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Daha fazla bilgi için [bulut hizmeti temelleri veri erişim katmanı – geçici hata işleme](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). ADO.NET (Entity Framework kullanarak doğrudan olmadan) kullanarak veritabanı erişimi için TFH önerilir.

> [!div class="step-by-step"]
> [Önceki](monitoring-and-telemetry.md)
> [İleri](distributed-caching.md)
