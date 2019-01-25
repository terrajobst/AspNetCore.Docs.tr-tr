---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Öğretici: Bağlantı dayanıklılığı ve komut durdurma EF ile bir ASP.NET MVC uygulamasında kullanın.'
author: tdykstra
description: Bu öğreticide bağlantı dayanıklılığı ve komut durdurma kullanmayı öğreneceksiniz. Bunlar, Entity Framework 6 iki önemli özellikleri şunlardır.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4ef13e676340f1bfac1a25219e6455c89b69df5e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837630"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Öğretici: Bağlantı dayanıklılığı ve komut durdurma Entity Framework ile bir ASP.NET MVC uygulamasında kullanın.

Şu ana kadar uygulamayı yerel olarak IIS Express'te URL'i geliştirme bilgisayarınızda çalışıyor. Gerçek bir uygulamada Internet üzerinden diğer kullanıcılar için kullanılabilir hale getirmek için bir web barındırma sağlayıcısına dağıtmanız ve veritabanı bir veritabanı sunucusuna dağıtmak zorunda.

Bu öğreticide bağlantı dayanıklılığı ve komut durdurma kullanmayı öğreneceksiniz. Bulut ortamı dağıtıyorsanız, özellikle iki önemli özellikleri Entity Framework 6 oldukları: (geçici hatalar için otomatik yeniden denemeler) bağlantı dayanıklılığı ve komut durdurma (catch veritabanına gönderilen tüm SQL sorguları oturum veya bunları değiştirmek için).

Bu bağlantı dayanıklılığı ve komut durdurma Öğreticisi isteğe bağlıdır. Bu öğreticiyi atlayabilir, birkaç küçük ayarlamalar sonraki öğreticilerde yapılması gerekecektir.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Bağlantı dayanıklılığı etkinleştir
> * Komut durdurma etkinleştir
> * Yeni yapılandırmayı test etme

## <a name="prerequisites"></a>Önkoşullar

* [Sıralama, Filtreleme ve Sayfalama](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Bağlantı dayanıklılığı etkinleştir

Uygulamayı Windows azure'a dağıtırken, bir bulut veritabanı hizmeti Windows Azure SQL veritabanı için veritabanı dağıtacaksınız. Bir bulut veritabanı hizmeti, web sunucunuzun ve veritabanı sunucusuna doğrudan birlikte aynı veri merkezinde bağlı daha bağlandığınızda geçici bağlantı hataları genellikle daha sık. Bir bulut web sunucusu ve bir bulut veritabanı hizmeti, aynı veri merkezinde barındırılan olsa bile, yük Dengeleyiciler gibi sorunlar olabilir bunlar arasında daha fazla ağ bağlantıları vardır.

Ayrıca bir bulut hizmeti vermediğinin onlar tarafından etkilenebilir yani diğer kullanıcılar tarafından genellikle da paylaşılır. Ve ağ kapasitesi azaltmaya tabidir veritabanına erişiminiz olabilir. Daha sık, hizmet düzeyi sözleşmesi (SLA) izin verilenden erişmeyi denediğinizde anlamına gelir azaltma özel durumları veritabanı hizmeti oluşturur.

Bir bulut hizmetine erişirken pek çok ya da çoğu bağlantı sorunları geçici, diğer bir deyişle, bunlar kendilerini kısa sürede çözün. Bu nedenle bir veritabanı işlemi deneyin ve genellikle geçici hata türünü almak kısa bekleyin ve işlemi başarılı olabilir sonra işlemi yeniden çalışabilir. Otomatik olarak yeniden deneyerek geçici hataları işlemek, kullanıcılarınız için bir çok daha iyi bir deneyim sağlayabilirsiniz. bunların çoğu müşteri için görünmez yapma. Entity Framework 6 bağlantı dayanıklılığı özelliğini yeniden deneme işlemi başarısız SQL sorgularının otomatikleştirir.

Bağlantı dayanıklılığı özelliğini belirli bir veritabanı hizmeti için uygun şekilde yapılandırılması gerekir:

- Bu, hangi özel durumları geçici bilmesi gerekir. Ağ bağlantısı altında geçici bir kaybına neden hataları göre program hatalar, örneğin neden hataları yeniden denemek istiyor.
- Uygun miktarda bir başarısız bir işlemin yeniden denemeler arasındaki süre beklemeniz gerekir. Bir kullanıcının yanıt burada bekleyen bir çevrimiçi web sayfası için daha uzun bir toplu işlem için yeniden denemeler arasındaki bekleyebilirsiniz.
- Uygun sayıda vazgeçmeden önce bir kez denemeye sahiptir. Çevrimiçi bir uygulamada yaptığınız bir toplu işlemde birden fazla kez yeniden denemek isteyebilirsiniz.

Bu ayarları el ile bir Entity Framework sağlayıcısı tarafından desteklenen her veritabanı ortam yapılandırabilirsiniz, ancak genellikle Windows Azure SQL veritabanı kullanan iyi bir çevrimiçi uygulama için çalışma varsayılan değerleri zaten yapılandırılmış olması, ve Bu, Contoso University uygulamayı uygulayacaksınız ayarlarıdır.

Tüm bağlantı dayanıklılığı etkinleştirmek için yapmanız gereken olan Oluştur bir sınıf türetildiği derlemenizi [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) sınıfı ve bu sınıfta SQL veritabanı kümesi *yürütme stratejisi*, EF olduğu bir sonraki dönem için *yeniden deneme ilkesi*.

1. DAL klasöründe adlı bir sınıf dosyası ekleyin *SchoolConfiguration.cs*.
2. Şablon kodunu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework otomatik olarak bulur, türetilen bir sınıf içinde kod çalıştırır `DbConfiguration`. Kullanabileceğiniz `DbConfiguration` Aksi takdirde yaptığınız kod yapılandırma görevlerini yapmak için sınıf *Web.config* dosya. Daha fazla bilgi için [EntityFramework kod tabanlı yapılandırma](https://msdn.microsoft.com/data/jj680699).
3. İçinde *StudentController.cs*, ekleme bir `using` bildirimi `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Tüm değişiklik `catch` engeller, catch `DataException` özel durumları böylece bunlar catch `RetryLimitExceededException` özel durumlar yerine. Örneğin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Kullanmakta olduğunuz `DataException` kolay bir "deneyin" iletisi vermek için geçici olabilecek hataları belirlemeye çalışır. Ancak bir yeniden deneme ilkesi etkinleştirdiyseniz, yalnızca hatalar geçici zaten alınan çalıştı ve birkaç kez başarısız oldu ve döndürülen özel durumu içinde sarmalanmış `RetryLimitExceededException` özel durum.

Daha fazla bilgi için [Entity Framework bağlantı dayanıklılığı / yeniden deneme mantığı](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Komut durdurma etkinleştir

Nasıl bir yeniden deneme ilkesi etkinleştirdiyseniz, beklendiği gibi çalışıp çalışmadığını doğrulamak için test? Geçici bir hata meydana gelmesine özellikle ne zaman yerel olarak çalıştırıyorsanız, ve gerçek geçici hatalar bir otomatik birim testine tümleştirmek özellikle zor zorlamak çok kolay değildir. Bağlantı dayanıklılığı özelliğini test etmek için SQL Server için Entity Framework gönderen sorguları kesecek ve SQL Sunucu yanıtı genellikle geçici bir özel durum türüyle değiştirin. bir yol gerekir.

Bulut uygulamaları için en iyi uygulama uygulamak için sorgu durdurma de kullanabilirsiniz: [gecikme süresi ve başarı veya başarısızlık dış hizmetlere yapılan tüm çağrıların oturum](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) veritabanı hizmetleri gibi. EF6 sağlayan bir [günlüğe kaydetme API'si ayrılmış](https://msdn.microsoft.com/data/dn469464) kolaylaştırmak günlük kaydı yapmak, ancak öğreticinin bu bölümünde Entity Framework'ün kullanmayı öğreneceksiniz [durdurma özelliği](https://msdn.microsoft.com/data/dn469464) doğrudan, hem oturum açma ve geçici hataları benzetimi.

### <a name="create-a-logging-interface-and-class"></a>Günlük arabirim ve sınıf oluşturma

A [açısından en iyisi günlüğe kaydetme için](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) bir arabirim kullanarak yapmak için System.Diagnostics.Trace veya günlük sınıfı çağrı sabit kodlama yerine. Bu günlük mekanizma daha sonra yapmanız gereken değiştirmeyi kolaylaştırır. Bu bölümde günlüğe kaydetme arabirimi ve onu/p uygulamak için bir sınıf oluşturacaksınız böylece >

1. Projeye bir klasör oluşturun ve adlandırın *günlüğü*.
2. İçinde *oturum* klasöründe adlı bir sınıf dosyası oluşturma *ILogger.cs*, şablonu kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Günlükleri göreceli önemini gösteren üç izleme düzeyi ve veritabanı sorguları gibi dış hizmet çağrıları için gecikme bilgileri sağlamak üzere tasarlanmış bir arabirim sağlar. Günlük yöntemlerini bir özel durum geçirmenize olanak tanıyan aşırı yüklemeleri vardır. Bu, böylelikle yığın izlemesi ve iç özel durumlar dahil olmak üzere özel durum bilgileri, her günlük yöntem çağrısının uygulama boyunca gerçekleştirilen güvenmek yerine arabirimini uygulayan bir sınıf tarafından güvenilir bir şekilde günlüğe kaydedilir.

    TraceApi yöntemleri, her SQL veritabanı gibi bir dış hizmete çağrı gecikme süresini izlemenize olanak sağlar.
3. İçinde *oturum* klasöründe adlı bir sınıf dosyası oluşturma *Logger.cs*, şablonu kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Uygulaması, System.Diagnostics izleme yapmak için kullanır. Bu izleme bilgilerini oluşturup kolaylaştıran .NET yerleşik bir özelliğidir. "Çok sayıda dinleyiciler, System.Diagnostics izleme ile dosyalara, örneğin, günlükleri yazmak için veya Azure blob depolamada yazmak için kullanabilirsiniz" vardır. Bazı seçenekler ve daha fazla bilgi için diğer kaynakların bağlantılarını görmek [Visual Studio'da Azure Web sitelerinin sorunlarını giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Bu öğreticide, yalnızca bakın Visual Studio'da günlüklerini için **çıkış** penceresi.

    Bir üretim uygulamasında dışında System.Diagnostics izleme paketleri düşünmek isteyebilirsiniz ve ILogger arabirimi oldukça farklı bir izleme mekanizması bunu karar verirseniz anahtar kolaylaştırır.

### <a name="create-interceptor-classes"></a>Dinleyiciyi sınıfları oluşturma

Ardından veritabanı, geçici hataların benzetimini yapmak için tek ve günlük kaydı yapmak için tek bir sorgu göndermek için gittiği her zaman Entity Framework içine çağıracak sınıfları oluşturacaksınız. Bu dinleyiciyi sınıfların türetilmesi gerekir `DbCommandInterceptor` sınıfı. Bunları sorgu çalıştırılmak üzere olduğunda otomatik olarak adlandırılır yöntemi geçersiz kılmalar yazın. Bu yöntemleri inceleyin veya veritabanına gönderilen sorgu günlük ve veritabanına gönderilmeden önce sorguyu değiştirin ya da bir şey için Entity Framework kendiniz sorgu dahi veritabanına geçmeden döndürür.

1. Veritabanına gönderilen her bir SQL sorgusu başlar.SSH dinleyiciyi sınıfı oluşturmak için adlı bir sınıf dosyası oluşturma *SchoolInterceptorLogging.cs* içinde *DAL* klasör ve şablon kod ile değiştirin Aşağıdaki kodu:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Başarılı sorguları veya komutlar için bu kod gecikme süresi bilgileriyle bir bilgi günlüğüne yazar. Özel durumlar için bir hata günlüğü oluşturur.
2. "Durum" girdiğinizde işlevsiz geçici hatalar oluşturur dinleyiciyi sınıf oluşturmak için **arama** kutusunda, adlı bir sınıf dosyası oluşturma *SchoolInterceptorTransientErrors.cs* içinde*DAL* klasör ve şablon kodunu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Bu kod yalnızca geçersiz kılma `ReaderExecuting` birden çok veri satırlarını döndüren sorgular için çağrılan yöntem. Diğer sorgu türleri için bağlantı dayanıklılığı denetlemek isterseniz, ayrıca geçersiz `NonQueryExecuting` ve `ScalarExecuting` yöntemleri günlük dinleyiciyi yapar.

    Öğrenci çalıştırırsanız ve "Durum" arama dizesini girin, bu kod hata sayısı 20'li genellikle geçici olarak bilinen bir tür için işlevsiz bir SQL veritabanı özel durumu oluşturur. Şu anda geçici kabul edilen diğer hata numaralarını 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 ve 40613, ancak bu SQL veritabanı'nın yeni sürümlerinde değiştirilebilir.

    Sorgu çalıştırmak ve arka sorgu sonuçları geçirme yerine Entity Framework özel durum kodu döndürür. Dört kez geçici özel durum döndürülür ve kodun sorgu veritabanına geçirme normal yordama geri döner.

    Her şeyi açmış olduğu için son başarılı önce dört kez sorguyu yürütmek Entity Framework çalışır ve uygulamadaki tek fark, sorgu sonuçlarını içeren bir sayfa işlemek için daha uzun sürer görmek mümkün olacaktır.

    Entity Framework yeniden denenme sayısı yapılandırılabilir; SQL veritabanı yürütme ilkesi için varsayılan değer olduğundan kod dört kez belirtir. Yürütme ilkesini değiştirirseniz, geçici hataları oluşturulan kaç kez belirten kodunu buraya de değişiklik gerekmektedir. Entity Framework atar, böylece daha fazla özel durum oluşturmak için kod da değiştirebilir `RetryLimitExceededException` özel durum.

    Arama kutusuna girdiğiniz değer olacaktır `command.Parameters[0]` ve `command.Parameters[1]` (bir kullanılan ad ve soyadı için). "% Throw % değerini" bulunduğunda, böylece bazı Öğrenciler bulunan ve döndürülen "Durum" Bu parametreleri "bir" değiştirilir.

    Bu uygulama kullanıcı Arabirimi için bir girdiyi değişen dayalı bağlantı dayanıklılığı test etmek için yalnızca bir yoludur. Tüm sorguları veya güncelleştirmeleri, geçici hatalar oluşturan kod hakkında yorumlar bölümünde açıklandığı gibi yazabilirsiniz *DbInterception.Add* yöntemi.
3. İçinde *Global.asax*, aşağıdaki `using` ifadeleri:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Vurgulanan satırları ekleyin `Application_Start` yöntemi:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Bu kod satırlarını ne Entity Framework veritabanı için sorgu gönderdiğinde çalıştırılacak dinleyiciyi kodunuzu neden olan. Geçici bir hata simülasyonu için ayrı dinleyiciyi sınıfları oluşturduğunuz ve günlüğe kaydetme, ayrı ayrı etkinleştirebilir ve devre dışı olduğundan dikkat edin.

    Kullanarak dinleyicileri ekleyebilirsiniz `DbInterception.Add` yöntemi kodunuzu; herhangi bir yerindeki olması gerekmez `Application_Start` yöntemi. Başka bir seçenek, bu kod yürütme ilkesini yapılandırmak için daha önce oluşturduğunuz DbConfiguration sınıfında eklemektir.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Her yerde olması dikkat edin yürütmek için bu kod put `DbInterception.Add` aynı dinleyiciyi için birden çok kez veya ek dinleyiciyi örnekleri elde edersiniz. Örneğin, günlük dinleyiciyi iki kez eklerseniz, her SQL sorgusu için iki günlüklerini görürsünüz.

    Kesiciler, kayıt sırasına göre yürütülür (sırayı `DbInterception.Add` yöntemi çağrılır). Sipariş dinleyiciyi içinde yaptığınız işlemlere bağlı olarak önemli. Örneğin, bir dinleyiciyi alır, SQL komut değişebilir `CommandText` özelliği. SQL komutu değiştirirseniz, sonraki dinleyiciyi değiştirilen SQL komutu, not özgün SQL komut alırsınız.

    Geçici hatalar kullanıcı Arabiriminde farklı bir değer girerek neden olanak tanır şekilde, geçici hata benzetimi kodu yazdığınız. Alternatif olarak, her zaman belirli bir parametre için denetlemeden geçici özel durumlar dizisi oluşturmak için dinleyiciyi kod yazabilirsiniz. Yalnızca geçici hataları oluşturmak istediğinizde dinleyiciyi sonra ekleyebilirsiniz. Ancak, veritabanı başlatma tamamlandıktan sonra bunu yaparsanız, dinleyiciyi kadar eklemeyin. Diğer bir deyişle, geçici hatalar üretme başlamadan önce varlık kümelerini birinde bir sorgu gibi en az bir veritabanı işlemi yapın. Entity Framework, veritabanı başlatma sırasında çeşitli sorguları yürütür ve başlatma sırasında hatalar tutarsız bir duruma geçmesine alınacak bağlam neden olabilir, bu işlem, yürütülen değildir.

## <a name="test-the-new-configuration"></a>Yeni yapılandırmayı test etme

1. Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırın ve ardından **Öğrenciler** sekmesi.
2. Konum Visual Studio **çıkış** izleme çıktısını görmek için penceresi. Yukarı doğru ilerleyin, Günlükçü tarafından yazılan günlükleri almak için bazı JavaScript hataları geçmiş gerekebilir.

    Veritabanına gönderilen gerçek SQL sorguları gördüğünüz dikkat edin. Bazı ilk sorgular ve veritabanı sürüm denetimi kullanmaya başlamak için Entity Framework mu komutları ve geçiş geçmiş tablosu (sonraki öğreticide geçişleri hakkında öğreneceksiniz) görürsünüz. Sayfalama vardır, kaç Öğrenciler bulmak için bkz ve son olarak, Öğrenci verilerinin alır sorgu görürsünüz.

    ![Normal bir sorgu için günlüğe kaydetme](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. İçinde **Öğrenciler** sayfasında, "Durum" arama dizesini girin ve tıklayın **arama**.

    ![Arama dizesi oluşturur](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Tarayıcısı Entity Framework, sorgu birkaç kez deniyor sırasında birkaç saniye kilitlenmesine gibi görünüyor fark edeceksiniz. Çok hızlı bir şekilde ilk yeniden deneme bekleme önce her bir ek yeniden denemeden önce arttıkça sonra gerçekleşir. Bu işlemi her yeniden deneme çağrılmadan önce artık bekleyen *üstel geri alma*.

    Sayfası görüntülendiğinde, sahip gösteren Öğrenciler "bir" adlarında, çıkış penceresine bakın ve aynı sorgu ilk dört kez geçici döndüren beş kez denendi görürsünüz özel durumlar. Geçici hata oluştururken yazma günlük görmek için her bir geçici hata `SchoolInterceptorTransientErrors` sınıfı ("döndürme geçici hata... komutu için") ve ne zaman yazılan günlük göreceksiniz `SchoolInterceptorLogging` özel durumu alır.

    ![Yeniden deneme gösteren günlük çıktısı](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Bir arama dizesi girdiğiniz olduğundan, Öğrenci verilerinin döndüren sorgu parametreli ise:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Parametre değerini oturum açtığınızdan değil, ancak bunu yapabilirsiniz. Parametre değerlerini görmek istiyorsanız, parametre değerlerinin almak için günlük kod yazabileceğiniz `Parameters` özelliği `DbCommand` nesnesini dinleyiciyi yöntemlere sahip olursunuz.

    Uygulamayı durdurun ve yeniden sürece bu test tekrarlanamıyor unutmayın. Bağlantı dayanıklılığı birden çok kez uygulamanın tek bir çalışma içinde test etmek istiyorsanız, hata sayaç sıfırlamak için kod yazabilirsiniz `SchoolInterceptorTransientErrors`.
4. Farkı görmek için yürütme stratejisi (yeniden deneme ilkesi) hale getirdiğini, yorum `SetExecutionStrategy` satırına *SchoolConfiguration.cs*Öğrenciler sayfanın hata ayıklama modunda yeniden çalıştırın ve yeniden "Throw" için arama yapın.

    Hemen bir sorguyu ilk kez çalıştığında, bu süre ilk oluşturulan özel durumla ilgili hata ayıklayıcıyı durdurur.

    ![İşlevsiz özel durumu](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Açıklamadan çıkarın *SetExecutionStrategy* satırına *SchoolConfiguration.cs*.

## <a name="get-the-code"></a>Kodu alma

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Ek kaynaklar

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Etkin bağlantı dayanıklılığı
> * Etkin komut durdurma
> * Yeni yapılandırmayı test

Code First geçişleri ve Azure dağıtım hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Kod First geçişleri ve Azure dağıtım](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
