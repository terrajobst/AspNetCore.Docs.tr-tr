---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bağlantı dayanıklılığı ve ASP.NET MVC uygulamasındaki Entity Framework komut kişiler tarafından ele | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4a43a9120bf3fa69b00b234d65d0f59d3ce9975b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875350"
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Bağlantı dayanıklılığı ve ASP.NET MVC uygulamasındaki Entity Framework komut kişiler tarafından ele
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Şu ana kadar uygulamayı yerel olarak IIS Express'te geliştirme bilgisayarınızda çalıştığı. Internet üzerinden kullanmak diğer kişiler için gerçek bir uygulamada kullanabilmek için bir web barındırma sağlayıcısına dağıtmanız ve veritabanını bir veritabanı sunucusunu dağıtmak zorunda.

Bu öğretici kapsamında, bulut ortamı dağıtıyorsanız, özellikle iki Entity Framework 6 özelliklerini nasıl kullanacağınızı öğreneceksiniz: bağlantı dayanıklılığı (geçici hatalar için otomatik yeniden denemeler) ve komut kişiler tarafından ele (catch tüm SQL sorguları oturum veya bunları değiştirmek için veritabanına gönderilen).

Bu bağlantı dayanıklılık ve komut kişiler tarafından ele öğretici isteğe bağlıdır. Bu öğretici atlarsanız, birkaç küçük ayarlamalar sonraki öğreticilerde yapılması gerekir.

## <a name="enable-connection-resiliency"></a>Bağlantı dayanıklılığı etkinleştir

Windows Azure için uygulama dağıtırken, bir bulut veritabanı hizmeti Windows Azure SQL veritabanı için veritabanı dağıtacaksınız. Ne zaman, web sunucusu ve veritabanı sunucunuz doğrudan birlikte aynı veri merkezinde bağlı olan dan bir bulut veritabanı hizmeti bağlandığınızda geçici bağlantı hatalarını genellikle daha sık. Bir bulut web sunucusu ve bir bulut veritabanı hizmeti aynı veri merkezinde barındırılır olsa bile, yük dengeleyici gibi sorunlar ortaya çıkabilir aralarında daha fazla ağ bağlantıları vardır.

Ayrıca bir bulut hizmeti vermediğinin onlar tarafından etkilenebilir başka bir deyişle, diğer kullanıcılar tarafından genellikle da paylaşılır. Ve veritabanı erişiminizi azaltma tabi olabilir. Daha sık, hizmet düzeyi sözleşmesi (SLA) izin verilenden erişmeyi denediğinde anlamına gelir azaltma veritabanı hizmeti özel durumları oluşturur.

Bir bulut hizmeti erişirken birçok ya da çoğu bağlantı sorunlarını geçici, diğer bir deyişle, bunlar kendilerini kısa bir süre içinde çözümleyin. Bu nedenle veritabanı işlemi deneyin ve genellikle geçici hata türünü alın, kısa bekleyin ve işlemi başarılı olabilir sonra işlemi yeniden deneyin. Otomatik olarak yeniden deneyerek geçici hataları işlemek, kullanıcılarınız için bir çok daha iyi bir deneyim sağlayabilir bunların çoğu müşteri için görünmez yapma. Entity Framework 6 bağlantı dayanıklılığı özelliği, yeniden deneme işlemi SQL sorguları başarısız otomatikleştirir.

Bağlantı dayanıklılığı özelliği için belirli veritabanı hizmeti uygun şekilde yapılandırılması gerekir:

- Bu, hangi özel durum büyük olasılıkla geçici bilmesi gerekir. Ağ Bağlantısı'nda kaybı tarafından kaynaklanan hataları değil göre program hatalar, örneğin neden olduğu hata yeniden denemek istiyor.
- Uygun miktarda bir yeniden deneme başarısız işlemi arasındaki süre beklemeniz gerekir. Bir kullanıcı için bir yanıt burada bekleyen çevrimiçi bir web sayfası için daha uzun bir toplu işlem için yeniden denemeler arasında bekleyebilirsiniz.
- Uygun bir sayı vazgeçmeden önce kaç kez yeniden denemek vardır. Çevrimiçi bir uygulamada yaptığınız bir toplu işlemde birden fazla kez yeniden denemek isteyebilirsiniz.

Genellikle Windows Azure SQL veritabanı kullanan iyi bir çevrimiçi uygulama için iş varsayılan değerleri zaten yapılandırıldı, ancak bu ayarları el ile bir Entity Framework sağlayıcısı tarafından desteklenen tüm veritabanı ortamı için yapılandırabilirsiniz ve Bu, Contoso University uygulaması için uygulama ayarlardır.

Tüm bağlantı dayanıklılığı etkinleştirmek için yapmanız gereken olduğu Oluştur bir sınıfın türetildiği derlemenizi [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) sınıfı ve bu sınıfta kümesini SQL veritabanını *yürütme stratejisi*, EF olduğu başka bir dönem için *yeniden deneme ilkesi*.

1. DAL klasöründe adlı bir sınıf dosyası ekleyin *SchoolConfiguration.cs*.
2. Şablon kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework otomatik olarak bulduğu türeyen bir sınıf kodu çalıştırır `DbConfiguration`. Kullanabileceğiniz `DbConfiguration` Aksi takdirde yaptığınız kodda yapılandırma görevlerini yapmak için sınıf *Web.config* dosya. Daha fazla bilgi için bkz: [EntityFramework kod tabanlı yapılandırma](https://msdn.microsoft.com/data/jj680699).
3. İçinde *StudentController.cs*, ekleme bir `using` bildirimi `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Tüm değiştirme `catch` Bu catch blokları `DataException` özel durumlar böylece bunlar catch `RetryLimitExceededException` özel durumlar yerine. Örneğin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Kullanmakta olduğunuz `DataException` kolay bir "yeniden deneyin." iletisi sunmak için geçici olabilecek hataları belirlemeye çalışır. Ancak bir yeniden deneme ilkesi açtığınız, büyük olasılıkla geçici yalnızca hataların zaten alınan denedi ve birkaç kez başarısız oldu ve döndürülen özel durumu içinde kaydırılan `RetryLimitExceededException` özel durum.

Daha fazla bilgi için bkz: [Entity Framework bağlantı dayanıklılığı / yeniden deneme mantığı](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Komut kişiler tarafından ele etkinleştir

Nasıl bir yeniden deneme ilkesi açtığınız, beklendiği gibi çalıştığını doğrulamak için test? Gerçekleşecek şekilde, özellikle ne zaman, yerel olarak çalıştırıyorsanız ve gerçek geçici hataları otomatik birim testine tümleştirmek özellikle zor olurdu geçici bir hata zorlamak kolay değil. Bağlantı dayanıklılığı özelliğini sınamak için Entity Framework SQL Server'a gönderen sorguları müdahale ve SQL Sunucu yanıtı, genellikle geçici bir özel durum türü ile değiştirmek için bir yönteme ihtiyacınız vardır.

Sorgu kişiler tarafından ele bulut uygulamaları için en iyi uygulama uygulamak için de kullanabilirsiniz: [gecikme süresi ve başarı veya başarısızlık dış hizmetler yapılan tüm çağrıların oturum](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) veritabanı hizmetleri gibi. EF6 sağlayan bir [günlüğü API ayrılmış](https://msdn.microsoft.com/data/dn469464) kolaylaştırmak günlük yapmak, ancak öğreticinin bu bölümünde Entity Framework'ün kullanmayı öğreneceksiniz [kişiler tarafından ele özelliği](https://msdn.microsoft.com/data/dn469464) doğrudan, hem oturum açma ve geçici hataları benzetimini yapma.

### <a name="create-a-logging-interface-and-class"></a>Bir günlük arabirim oluşturup sınıfı

A [açısından en iyisi günlüğü için](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) bir arabirim kullanılarak yapmak için System.Diagnostics.Trace veya günlük sınıfı çağrıları sabit kodlama yerine. Bunun ihtiyacınız varsa daha sonra günlük mekanizmasını değiştirin kolaylaştırır. Bu bölümde günlük arabirim ve kendisine/p uygulamak için bir sınıf oluşturacaksınız şekilde > 

1. Projede bir klasör oluşturun ve adlandırın *günlüğü*.
2. İçinde *günlüğü* klasörünü adlı bir sınıf dosyası oluşturma *ILogger.cs*ve şablon kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Günlükleri göreceli önemini belirtmek için üç izleme düzeyleri ve veritabanı sorguları gibi dış hizmeti çağrıları için gecikme bilgileri sağlamak üzere tasarlanmış bir arabirim sağlar. Günlüğe kaydetme yöntemi bir özel durum geçirmenize olanak tanıyan aşırı vardır. Bu, böylelikle yığın izleme ve iç özel durumlar dahil olmak üzere özel durum bilgileri güvenilir bir şekilde, her günlük yöntem çağrısı uygulama boyunca gerçekleştirilen güvenmek yerine arabirimini uygulayan sınıf tarafından kaydedilir.

    TraceApi yöntemleri SQL veritabanı gibi dış bir hizmetine yapılan her çağrı gecikme izlemenize olanak sağlar.
3. İçinde *günlüğü* klasörünü adlı bir sınıf dosyası oluşturma *Logger.cs*ve şablon kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Uygulama System.Diagnostics izleme yapmak için kullanır. Bu, oluşturulması ve izleme bilgilerinin kullanılması daha kolay hale getirir .NET, yerleşik bir özelliğidir. "Günlüklerini dosyalara, örneğin, yazma veya Azure blob storage'da yazmak için System.Diagnostics izleme ile kullanabileceğiniz birçok dinleyicileri" vardır. Bazı seçenekler ve daha fazla bilgi için diğer kaynakların bağlantılarını görmek [Visual Studio'daki Azure Web siteleri sorun giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Bu öğretici, yalnızca bakabilir Visual Studio'da oturum için **çıkış** penceresi.

    Bir üretim uygulamasında izleme paketleri System.Diagnostics dışında düşünmek isteyebilirsiniz ve ILogger arabirimi görece için farklı izleme mekanizması, bunu yapmak karar verirseniz anahtar kolaylaştırır.

### <a name="create-interceptor-classes"></a>Dinleyiciyi sınıfları oluşturma

Ardından veritabanı, geçici hataları benzetimini yapmak için bir ve günlük kaydı yapmak için tek bir sorgu göndermeye geçiyor her zaman Entity Framework uygulamasına çağıracak sınıfları oluşturacaksınız. Bu dinleyiciyi sınıfları öğesinden türetilmelidir `DbCommandInterceptor` sınıfı. Bunları sorgu çalıştırılmak üzere olduğunda otomatik olarak adlandırılır yöntemi geçersiz kılmalar yazma. Bu yöntemleri inceleyin veya veritabanına gönderilen sorgu oturum ve veritabanına gönderilmeden önce sorguyu değiştirin ya da bir şey için Entity Framework kendiniz sorgu veritabanına bile geçmeden döndürür.

1. Veritabanına gönderilen her SQL sorgusu oturum dinleyiciyi sınıfı oluşturmak için adlı bir sınıf dosyası oluşturmanız *SchoolInterceptorLogging.cs* içinde *DAL* klasörü ve şablon kodu ile değiştir Aşağıdaki kod:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Başarılı sorgular veya komutlar için bu kodu bir bilgi günlüğü gecikme bilgilerle yazar. Özel durumlar için bir hata günlüğü oluşturur.
2. "Durum" girdiğinizde kukla geçici hataları oluşturacak dinleyiciyi sınıf oluşturmak için **arama** kutusunda, adlı bir sınıf dosyası oluşturma *SchoolInterceptorTransientErrors.cs* içinde*DAL* klasörü ve şablon kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Bu kod yalnızca geçersiz kılma `ReaderExecuting` verilerin birden çok satır döndürebilir sorgular için çağrılan yöntemi. Diğer sorgu türleri için bağlantı dayanıklılığı denetlemek istiyorsanız, ayrıca geçersiz kılmanız `NonQueryExecuting` ve `ScalarExecuting` yöntemleri günlük dinleyiciyi yapar.

    Öğrenci sayfayı çalıştırın ve "Durum" arama dizesini girin, bu kod hata numarası 20, genellikle geçici olarak bilinen bir türü için sahte SQL veritabanı özel durumu oluşturur. Şu anda geçici kabul edilen diğer hata numaraları 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 ve 40613, ancak bu SQL veritabanının yeni sürümlerde değiştirilebilir.

    Sorguyu çalıştırmak ve geri sorgu sonuçları geçirme yerine Entity Framework için özel durum kodu döndürür. Geçici özel durumu dört kez döndürülür ve sorgu veritabanına geçirme normal yordama kod döner.

    Her şeyi açmış olduğu için son başarılı önce dört kez sorguyu yürütmek Entity Framework çalışır ve yalnızca uygulamadaki sorgu sonuçlarını içeren bir sayfa işlemek için daha uzun sürer farktır görmeye devam.

    Entity Framework deneyecek sayısı yapılandırılabilir; SQL veritabanı yürütme ilkesi için varsayılan değeri olduğu için kod dört kez belirtir. Yürütme İlkesi değiştirirseniz, geçici hataları oluşturulan kaç kez belirten geçen kod de değişiklik gerekmektedir. Entity Framework atar böylece daha fazla özel durum oluşturmak için kodu aynı zamanda değişebilir `RetryLimitExceededException` özel durum.

    Arama kutusuna girdiğiniz değer olacaktır `command.Parameters[0]` ve `command.Parameters[1]` (bir kullanılan ad ve soyadı için). "% Throw % değerini" bulunduğunda, böylece bazı Öğrenciler bulundu ve döndürülen "Durum" Bu parametrelerinde "bir" değiştirilir.

    Bu uygulama kullanıcı Arabirimi için bazı giriş değişen göre bağlantı dayanıklılığı test etmek için yalnızca bir yoludur. Tüm sorgular veya güncelleştirmeleri için geçici hataları oluşturur kod hakkında yorumlar içinde açıklandığı gibi yazabilirsiniz *DbInterception.Add* yöntemi.
3. İçinde *Global.asax*, aşağıdakileri ekleyin `using` deyimleri:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Vurgulanan satırlar ekleme `Application_Start` yöntemi:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Bu kod satırları ne Entity Framework veritabanı için sorgu gönderdiğinde çalıştırılacak dinleyiciyi kodunuzu neden olan. Geçici hatanın benzetimi için ayrı dinleyiciyi sınıfları oluşturulur ve günlüğe kaydetme, bağımsız olarak etkinleştirebilir ve devre dışı olduğundan dikkat edin.

    Kullanarak dinleyiciler ekleyebilirsiniz `DbInterception.Add` kodunuzu; başka bir yerindeki yöntemi olması gerekmeyen `Application_Start` yöntemi. Bu kodu daha önce oluşturduğunuz yürütme ilkesini yapılandırmak için DbConfiguration sınıfına koyun başka bir seçenektir.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Yerde dikkat yürütmek için bu kodu put `DbInterception.Add` aynı dinleyiciyi için birden çok kez veya ek dinleyiciyi örnekleri elde edersiniz. Örneğin, günlük dinleyiciyi iki kez eklerseniz, her SQL sorgusu için iki günlüklerini görürsünüz.

    Dinleyiciler kayıt sırasına göre çalıştırılır (hangi sırayla `DbInterception.Add` yöntemi çağrılır). Sırada ne dinleyiciyi yaptığınız bağlı olarak önemli. Örneğin, bir dinleyiciden alır, SQL komutu değiştirebilirsiniz `CommandText` özelliği. SQL komutunun değiştirirseniz, sonraki dinleyiciyi değil özgün SQL komutu değiştirilen SQL komutu alır.

    Kullanıcı Arabiriminde farklı bir değer girerek geçici hataları neden olanak sağlayan bir şekilde geçici hata benzetimi kodu yazdığınız. Alternatif olarak, her zaman belirli bir parametre için denetlemeden geçici özel durumlar dizisi oluşturmak için dinleyiciyi kod yazabilirsiniz. Yalnızca geçici hataları oluşturmak istediğinizde dinleyiciyi sonra ekleyebilirsiniz. Bunu yaparsanız, veritabanı başlatma tamamlandıktan sonra ancak kadar dinleyiciyi eklemeyin. Geçici hatalar üretme başlamadan önce diğer bir deyişle, bir varlık kümelerinin bir sorgu gibi en az bir veritabanı işlemi yapın. Entity Framework veritabanı başlatma sırasında birkaç sorguları yürütür ve başlatma sırasında hatalar tutarsız bir duruma getirmek içerik neden olabilir, bu işlemde, yürütülen değil.

## <a name="test-logging-and-connection-resiliency"></a>Test günlüğe kaydetme ve bağlantı dayanıklılığı

1. Uygulamayı hata ayıklama modunda çalıştırın ve ardından için F5 tuşuna basın **Öğrenciler** sekmesi.
2. Ara Visual Studio'ya **çıkış** İzleme çıktısı penceresini. Yukarı doğru ilerleyin, Günlükçü tarafından yazılmış günlükleri almak için bazı JavaScript hataları geçmiş gerekebilir.

    Veritabanına gönderilen gerçek SQL sorguları görebilirsiniz dikkat edin. Bazı ilk sorguları ve veritabanı sürüm denetimi başlamak için Entity Framework mu komutları ve geçiş geçmiş tablosunu (sonraki öğreticide hakkında geçişler öğreneceksiniz) bakın. Ve bir sorgu vardır, kaç tane Öğrenciler bulmak için disk belleği için bkz: ve son olarak Öğrenci veri alır sorgu görürsünüz.

    ![Normal bir sorgu için günlüğe kaydetme](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. İçinde **Öğrenciler** sayfasında "Durum" arama dizesini girin ve tıklayın **arama**.

    ![Arama dizesi throw](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Tarayıcı Entity Framework birkaç kez sorgu deniyor sırasında birkaç saniye askıda görünüyor fark edeceksiniz. İlk yeniden denemeden sonra önce arttıkça ek her yeniden deneme bekleme çok hızlı bir şekilde gerçekleşir. Bu işlemi artık bekleyen denemeler çağrılmadan önce *üstel geri alma*.

    Sayfası görüntülendiğinde, sahip gösteren Öğrenciler "bir" adlarında, çıkış penceresine bakın ve aynı sorgu ilk dört kez geçici döndürme beş kez denendi görürsünüz özel durumları. Her geçici bir hata için geçici hata oluştururken yazma günlük göreceksiniz `SchoolInterceptorTransientErrors` sınıfı ("döndürme geçici hata komutu...") ve ne zaman yazılan günlük göreceksiniz `SchoolInterceptorLogging` özel durumu alır.

    ![Yeniden deneme gösteren günlük çıktısı](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Bir arama dizesi girdiğinden beri Öğrenci veri döndüren sorgu parametreli:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Parametre değerini oturum, ancak bunu. Parametre değerleri görmek istiyorsanız, parametre değerlerinin almak için günlük kodu yazabilirsiniz `Parameters` özelliği `DbCommand` dinleyiciyi yöntemlere alma nesnesi.

    Uygulamayı durdurun ve yeniden sürece bu test yinelenemez unutmayın. Bağlantı dayanıklılığı birden çok kez uygulamanın bir tek çalıştırmada test edebilmek istediyseniz, hata sayacı sıfırlamak için kod yazabilirsiniz `SchoolInterceptorTransientErrors`.
4. Farkı görmek için yürütme stratejisi (yeniden deneme ilkesi) yapar, yorum `SetExecutionStrategy` satırından *SchoolConfiguration.cs*Öğrenciler sayfanın hata ayıklama modunda yeniden çalıştırın ve "Throw için" yeniden arayın.

    Hemen ilk kez sorguyu yürütmek çalıştığında, bu süre ilk oluşturulan özel durum hata ayıklayıcı durdurur.

    ![Sahte özel durumu](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Açıklamadan çıkarın *SetExecutionStrategy* satırından *SchoolConfiguration.cs*.

## <a name="summary"></a>Özet

Bu öğreticide bağlantı dayanıklılığı etkinleştirmek ve Entity Framework oluşturur ve veritabanına gönderir. SQL komutlarını oturum öğrendiniz. Sonraki öğreticide veritabanını dağıtmak için Code First Migrations kullanarak Internet'e uygulama dağıtacaksınız.

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [sonraki](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
