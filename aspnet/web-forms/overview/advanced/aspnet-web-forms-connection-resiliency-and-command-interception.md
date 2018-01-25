---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: "ASP.NET Web Forms bağlantı dayanıklılığı ve komut kişiler tarafından ele | Microsoft Docs"
author: Erikre
description: "Bu öğretici, bağlantı dayanıklılığı ve komut kişiler tarafından ele desteklemek için örnek bir uygulamayı değiştirmeyi açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: e3347657fb5c7bf8c7bb4e51a2e810a1edde826a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Forms bağlantı dayanıklılığı ve komut kişiler tarafından ele
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

Bu öğreticide, bağlantı dayanıklılığı ve komut kişiler tarafından ele desteklemek için Wingtip Toys örnek uygulama değiştirecektir. Bir bulut ortamı tipik geçici hataları oluştuğunda bağlantı dayanıklılığı etkinleştirerek, Wingtip Toys örnek uygulama otomatik olarak veri aramaları yeniden deneyecek. Ayrıca, komut kişiler tarafından ele uygulayarak Wingtip Toys örnek uygulama tüm yakalar oturum veya bunları değiştirmek için veritabanına gönderilen SQL sorguları.

> [!NOTE] 
> 
> Bu Web Forms öğretici zel Dykstra'nın aşağıdaki MVC öğretici dayanır:  
> [Bağlantı dayanıklılığı ve ASP.NET MVC uygulamasındaki Entity Framework komut kişiler tarafından ele](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Bağlantı dayanıklılığı sağlamak nasıl.
- Komut kişiler tarafından ele gerçekleştirme.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki yazılım bilgisayarınızda yüklü olduğundan emin olun:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework otomatik olarak yüklenir.
- Böylece, bu öğreticide Wingtip Toys projede belirtilen işlevselliği uygulayabileceğiniz Wingtip Toys proje, örnek. Aşağıdaki bağlantıda indirme Ayrıntılar verilmiştir:

    - [ASP.NET 4.5.1 ile çalışmaya başlama Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Bu öğreticiyi tamamlamak önce ilgili öğretici serisi, gözden geçirme düşünün [ASP.NET 4.5 Web formları ve Visual Studio 2013 ile çalışmaya başlama](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Eğitmen serisi öğrenmeniz yardımcı olacak **WingtipToys** proje ve kod.

## <a name="connection-resiliency"></a>Bağlantı dayanıklılığı

Windows Azure için bir uygulama dağıtımının düşünürken, dikkate alınması gereken bir seçenek veritabanına dağıtma **Windows** **Azure SQL veritabanı**, bir bulut veritabanı hizmeti. Ne zaman, web sunucusu ve veritabanı sunucunuz doğrudan birlikte aynı veri merkezinde bağlı olan dan bir bulut veritabanı hizmeti bağlandığınızda geçici bağlantı hatalarını genellikle daha sık. Bir bulut web sunucusu ve bir bulut veritabanı hizmeti aynı veri merkezinde barındırılır olsa bile, yük dengeleyici gibi sorunlar ortaya çıkabilir aralarında daha fazla ağ bağlantıları vardır.

Ayrıca bir bulut hizmeti vermediğinin onlar tarafından etkilenebilir başka bir deyişle, diğer kullanıcılar tarafından genellikle da paylaşılır. Ve veritabanı erişiminizi azaltma tabi olabilir. İzin daha sık erişmeyi denediğinde anlamına gelir azaltma veritabanı hizmeti özel durumları oluşturur, *hizmet düzeyi sözleşmesi* (SLA).

Bir bulut hizmeti erişirken oluşan çok sayıda veya çoğu bağlantı sorunları geçici, diğer bir deyişle, bunlar kendilerini kısa bir süre içinde çözümleyin. Bu nedenle veritabanı işlemi deneyin ve genellikle geçici hata türünü alın, kısa bekleyin ve işlemi başarılı olabilir sonra işlemi yeniden deneyin. Otomatik olarak yeniden deneyerek geçici hataları işlemek, kullanıcılarınız için bir çok daha iyi bir deneyim sağlayabilir bunların çoğu müşteri için görünmez yapma. Entity Framework 6 bağlantı dayanıklılığı özelliği, yeniden deneme işlemi SQL sorguları başarısız otomatikleştirir.

Bağlantı dayanıklılığı özelliği için belirli veritabanı hizmeti uygun şekilde yapılandırılması gerekir:

1. Bu, hangi özel durum büyük olasılıkla geçici bilmesi gerekir. Ağ Bağlantısı'nda kaybı tarafından kaynaklanan hataları değil göre program hatalar, örneğin neden olduğu hata yeniden denemek istiyor.
2. Uygun miktarda bir yeniden deneme başarısız işlemi arasındaki süre beklemeniz gerekir. Bir kullanıcı için bir yanıt burada bekleyen çevrimiçi bir web sayfası için daha uzun bir toplu işlem için yeniden denemeler arasında bekleyebilirsiniz.
3. Uygun bir sayı vazgeçmeden önce kaç kez yeniden denemek vardır. Çevrimiçi bir uygulamada yaptığınız bir toplu işlemde birden fazla kez yeniden denemek isteyebilirsiniz.

Bu ayarları el ile bir Entity Framework sağlayıcısı tarafından desteklenen tüm veritabanı ortamı için yapılandırabilirsiniz.

Bağlantı dayanıklılığı etkinleştirmek için yapmanız gereken tek şey derlemenizi türeyen bir sınıf oluşturun `DbConfiguration` sınıfı ve bu sınıfta hangi Entity Framework yeniden deneme ilkesi için başka bir terimdir SQL veritabanı yürütme stratejisi ayarlayın.

### <a name="implementing-connection-resiliency"></a>Bağlantı dayanıklılığı uygulama

1. Karşıdan yükleyip açmak [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) örnek Visual Studio'da Web Forms uygulama.
2. İçinde *mantığı* klasöründe **WingtipToys** uygulama adlı bir sınıf dosyası ekleyin *WingtipToysConfiguration.cs*.
3. Var olan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework otomatik olarak bulduğu türeyen bir sınıf kodu çalıştırır `DbConfiguration`. Kullanabileceğiniz `DbConfiguration` Aksi takdirde yaptığınız kodda yapılandırma görevlerini yapmak için sınıf *Web.config* dosya. Daha fazla bilgi için bkz: [EntityFramework kod tabanlı yapılandırma](https://msdn.microsoft.com/data/jj680699).

1. İçinde *mantığı* klasörü, açık *AddProducts.cs* dosya.
2. Ekleme bir `using` bildirimi `System.Data.Entity.Infrastructure` sarı ile vurgulanan gösterildiği gibi:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Ekleme bir `catch` için engelleyin `AddProduct` yöntemi böylece `RetryLimitExceededException` vurgulanmış sarı günlüğe kaydedilir:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Ekleyerek `RetryLimitExceededException` özel durum, daha iyi günlüğü sağlayın veya burada bunlar seçebilirsiniz işlemi yeniden deneyin kullanıcıya bir hata iletisi görüntülenir. Yakalama tarafından `RetryLimitExceededException` özel durum, büyük olasılıkla geçici yalnızca hataların zaten alınan denedi ve birkaç kez başarısız oldu. Döndürülen özel durumu içinde kaydırılan `RetryLimitExceededException` özel durum. Ayrıca, ayrıca bir genel catch bloğu eklendi. Hakkında daha fazla bilgi için `RetryLimitExceededException` özel durum, bkz: [Entity Framework bağlantı dayanıklılığı / yeniden deneme mantığı](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Komut kişiler tarafından ele

Nasıl bir yeniden deneme ilkesi açtığınız, beklendiği gibi çalıştığını doğrulamak için test? Gerçekleşecek şekilde, özellikle ne zaman, yerel olarak çalıştırıyorsanız ve gerçek geçici hataları otomatik birim testine tümleştirmek özellikle zor olurdu geçici bir hata zorlamak kolay değil. Bağlantı dayanıklılığı özelliğini sınamak için Entity Framework SQL Server'a gönderen sorguları müdahale ve SQL Sunucu yanıtı, genellikle geçici bir özel durum türü ile değiştirmek için bir yönteme ihtiyacınız vardır.

Sorgu kişiler tarafından ele bulut uygulamaları için en iyi uygulama uygulamak için de kullanabilirsiniz: gecikme süresi ve başarı veya başarısızlık tüm çağrıları için dış günlük veritabanı hizmetleri gibi hizmetleri.

Öğreticinin bu bölümünde Entity Framework'ün kullanacağınız [ *kişiler tarafından ele özelliği* ](https://msdn.microsoft.com/data/dn469464) günlüğe kaydetme ve geçici hataları benzetimi için.

### <a name="create-a-logging-interface-and-class"></a>Bir günlük arabirim oluşturup sınıfı

Kullanarak yapmak için en iyi uygulama günlüğü için olan bir [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) çağrıları sabit kodlama yerine `System.Diagnostics.Trace` veya günlük sınıfı. Bunun ihtiyacınız varsa daha sonra günlük mekanizmasını değiştirin kolaylaştırır. Bu nedenle bu bölümde, günlük arabirim ve uygulamak için bir sınıf oluşturacaksınız.

Yukarıdaki yordam bağlı olarak, yüklediğiniz açılır ve **WingtipToys** örnek Visual Studio'da uygulama.

1. Bir klasör oluşturun **WingtipToys** proje ve adlandırın *günlüğü*.
2. İçinde *günlüğü* klasörünü adlı bir sınıf dosyası oluşturma *ILogger.cs* ve varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

 Günlükleri göreceli önemini belirtmek için üç izleme düzeyleri ve veritabanı sorguları gibi dış hizmeti çağrıları için gecikme bilgileri sağlamak üzere tasarlanmış bir arabirim sağlar. Günlüğe kaydetme yöntemi bir özel durum geçirmenize olanak tanıyan aşırı vardır. Bu, böylelikle yığın izleme ve iç özel durumlar dahil olmak üzere özel durum bilgileri güvenilir bir şekilde, her günlük yöntem çağrısı uygulama boyunca gerçekleştirilen güvenmek yerine arabirimini uygulayan sınıf tarafından kaydedilir.  
  
 `TraceApi` Yöntemleri, SQL veritabanı gibi dış bir hizmetine yapılan her çağrı gecikme izlemenize olanak sağlar.
3. İçinde *günlüğü* klasörünü adlı bir sınıf dosyası oluşturma *Logger.cs* ve varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Uygulama kullanan `System.Diagnostics` izleme yapmak için. Bu, oluşturulması ve izleme bilgilerinin kullanılması daha kolay hale getirir .NET, yerleşik bir özelliğidir. Vardır birçok &quot;dinleyicileri&quot; ile birlikte kullanabileceğiniz `System.Diagnostics` izleme günlüklerini dosyalara, örneğin, yazma ya da Windows Azure blob storage'da yazmak için. Bazı seçenekler ve daha fazla bilgi için diğer kaynakların bağlantılarını görmek [Visual Studio'da Windows Azure Web siteleri sorun giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Bu öğretici için yalnızca Visual Studio günlüklerinde bakmak **çıkış** penceresi.

Bir üretim uygulamasında izleme çerçeveleri dışında kullanarak düşünmek isteyebilirsiniz `System.Diagnostics`ve `ILogger` arabirimi görece Bunu yapmak karar verirseniz için farklı izleme mekanizması geçiş yapmayı kolaylaştırır.

### <a name="create-interceptor-classes"></a>Dinleyiciyi sınıfları oluşturma

Ardından, veritabanı, geçici hataları benzetimini yapmak için bir ve günlük kaydı yapmak için tek bir sorgu göndermeye geçiyor her zaman Entity Framework uygulamasına çağıracak sınıfları oluşturacaksınız. Bu dinleyiciyi sınıfları öğesinden türetilmelidir `DbCommandInterceptor` sınıfı. Bunları, sorgu çalıştırılmak üzere olduğunda otomatik olarak adlandırılır yöntemi geçersiz kılmalar yazma. Bu yöntemleri inceleyin veya veritabanına gönderilen sorgu oturum ve veritabanına gönderilmeden önce sorguyu değiştirin ya da bir şey için Entity Framework kendiniz sorgu veritabanına bile geçmeden döndürür.

1. Veritabanına gönderilmeden önce her SQL sorgusu oturum dinleyiciyi sınıf oluşturmak için adlı bir sınıf dosyası oluşturmanız *InterceptorLogging.cs* içinde *mantığı* kod klasörü ve varsayılan Değiştir Aşağıdaki kod:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

 Başarılı sorgular veya komutlar için bu kodu bir bilgi günlüğü gecikme bilgilerle yazar. Özel durumlar için bir hata günlüğü oluşturur.
2. Girdiğiniz yükleyen kukla geçici hataları oluşturacak dinleyiciyi sınıfı oluşturmak için &quot;Throw&quot; içinde **adı** adlı sayfasında textbox *AdminPage.aspx*, bir sınıf oluşturun adlı dosya *InterceptorTransientErrors.cs* içinde *mantığı* klasörü ve varsayılan Değiştir kodu aşağıdaki kodla:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Bu kod yalnızca geçersiz kılma `ReaderExecuting` verilerin birden çok satır döndürebilir sorgular için çağrılan yöntemi. Diğer sorgu türleri için bağlantı dayanıklılığı denetlemek istiyorsanız, ayrıca geçersiz kılmanız `NonQueryExecuting` ve `ScalarExecuting` yöntemleri günlük dinleyiciyi yapar.  
  
 Daha sonra siz "Yönetici" oturum açın ve seçin **yönetici** üst gezinti çubuğundaki bağlantı. Ardından *AdminPage.aspx* adlı bir ürün ekleyecek sayfa &quot;Throw&quot;. Bu kod hata numarası 20, genellikle geçici olarak bilinen bir türü için sahte SQL veritabanı özel durumu oluşturur. Şu anda geçici kabul edilen diğer hata numaraları 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 ve 40613, ancak bu SQL veritabanının yeni sürümlerde değiştirilebilir. Ürün kodunda takip edebilir "TransientErrorExample" yeniden adlandırılacak *InterceptorTransientErrors.cs* dosya.  
  
 Sorguyu çalıştırmak ve geri sonuçları geçirme yerine Entity Framework için özel durum kodu döndürür. Geçici özel durum döndürdü *dört* kez ve sorgu veritabanına geçirme normal yordama kod döner.

    Her şeyi açmış olduğu için son başarılı önce dört kez sorguyu yürütmek Entity Framework çalışır ve yalnızca uygulamadaki sorgu sonuçlarını içeren bir sayfa işlemek için daha uzun sürer farktır görmeye devam.  
  
 Entity Framework deneyecek sayısı yapılandırılabilir; SQL veritabanı yürütme ilkesi için varsayılan değeri olduğu için kod dört kez belirtir. Yürütme İlkesi değiştirirseniz, geçici hataları oluşturulan kaç kez belirten geçen kod de değişiklik gerekmektedir. Entity Framework atar böylece daha fazla özel durum oluşturmak için kodu aynı zamanda değişebilir `RetryLimitExceededException` özel durum.
3. İçinde *Global.asax*, aşağıdaki using deyimlerini:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Ardından, vurgulanan satırlar ekleyin `Application_Start` yöntemi:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Bu kod satırları ne Entity Framework veritabanı için sorgu gönderdiğinde çalıştırılacak dinleyiciyi kodunuzu neden olan. Geçici hatanın benzetimi için ayrı dinleyiciyi sınıfları oluşturulur ve günlüğe kaydetme, bağımsız olarak etkinleştirebilir ve devre dışı olduğundan dikkat edin.   
  
 Kullanarak dinleyiciler ekleyebilirsiniz `DbInterception.Add` kodunuzu; başka bir yerindeki yöntemi olması gerekmeyen `Application_Start` yöntemi. Başka bir seçenek de dinleyiciler eklemezseniz `Application_Start` yöntemi, güncelleştirmek veya adlı sınıf eklemek için olacaktır *WingtipToysConfiguration.cs* ve yukarıdaki kod oluşturucusunun sonunda put `WingtipToysbConfiguration` sınıfı.

Yerde dikkat yürütmek için bu kodu put `DbInterception.Add` aynı dinleyiciyi için birden çok kez veya ek dinleyiciyi örnekleri elde edersiniz. Örneğin, günlük dinleyiciyi iki kez eklerseniz, her SQL sorgusu için iki günlüklerini görürsünüz.

Dinleyiciler kayıt sırasına göre çalıştırılır (hangi sırayla `DbInterception.Add` yöntemi çağrılır). Sırada ne dinleyiciyi yaptığınız bağlı olarak önemli. Örneğin, bir dinleyiciden alır, SQL komutu değiştirebilirsiniz `CommandText` özelliği. SQL komutunun değiştirirseniz, sonraki dinleyiciyi değil özgün SQL komutu değiştirilen SQL komutu alır.

Kullanıcı Arabiriminde farklı bir değer girerek geçici hataları neden olanak sağlayan bir şekilde geçici hata benzetimi kodu yazdığınız. Alternatif olarak, her zaman belirli bir parametre için denetlemeden geçici özel durumlar dizisi oluşturmak için dinleyiciyi kod yazabilirsiniz. Yalnızca geçici hataları oluşturmak istediğinizde dinleyiciyi sonra ekleyebilirsiniz. Bunu yaparsanız, veritabanı başlatma tamamlandıktan sonra ancak kadar dinleyiciyi eklemeyin. Geçici hatalar üretme başlamadan önce diğer bir deyişle, bir varlık kümelerinin bir sorgu gibi en az bir veritabanı işlemi yapın. Entity Framework veritabanı başlatma sırasında birkaç sorguları yürütür ve başlatma sırasında hatalar tutarsız bir duruma getirmek içerik neden olabilir, bu işlemde, yürütülen değil.

## <a name="test-logging-and-connection-resiliency"></a>Test günlüğe kaydetme ve bağlantı dayanıklılığı

1. Visual Studio'da basın **F5** uygulama hata ayıklama modu ve oturum açma "Yönetici"Pa$ $word"parola olarak kullanarak" olarak çalıştırılacak.
2. Seçin **yönetici** üst gezinti çubuğunda.
3. "Durum" adlı yeni bir ürün ile uygun açıklaması, fiyat ve görüntü dosyası girin.
4. Tuşuna **Ürün Ekle** düğmesi.  
 Tarayıcı Entity Framework birkaç kez sorgu deniyor sırasında birkaç saniye askıda görünüyor fark edeceksiniz. İlk yeniden deneme çok hızlı olur ve ardından önce ek her yeniden deneme bekleme artırır. Bu işlemi artık bekleyen denemeler çağrılmadan önce *üstel geri alma* .
5. Sayfayı yüklemek için atttempting artık olana kadar bekleyin.
6. Proje durdurun ve Visual Studio'ya Ara **çıkış** İzleme çıktısı penceresini. Bulabileceğiniz **çıkış** seçerek penceresi **hata ayıklama**  - &gt; **Windows**  - &gt;  **Çıktı**. Günlükçü tarafından yazılan birkaç diğer günlükler kaydırarak gerekebilir.  
  
 Veritabanına gönderilen gerçek SQL sorguları görebilirsiniz dikkat edin. Bazı ilk sorgular ve başlamak için Entity Framework mu komutlar veritabanı sürümü ve geçiş geçmiş tablosunu denetimi görürsünüz.   
    ![Çıktı Penceresi](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
 Uygulamayı durdurun ve yeniden sürece bu test yinelenemez unutmayın. Bağlantı dayanıklılığı birden çok kez uygulamanın bir tek çalıştırmada test edebilmek istediyseniz, hata sayacı sıfırlamak için kod yazabilirsiniz `InterceptorTransientErrors` .
7. Farkı görmek için yürütme stratejisi (yeniden deneme ilkesi) yapar, yorum `SetExecutionStrategy` satırından *WingtipToysConfiguration.cs* dosyasını *mantığı* klasöründe şunu çalıştırın **yönetici**  sayfasında hata ayıklama modunda yeniden ve adlı ürün ekleme &quot;Throw&quot; yeniden.  
  
 Hemen ilk kez sorguyu yürütmek çalıştığında, bu süre ilk oluşturulan özel durum hata ayıklayıcı durdurur.  
    ![Hata ayıklama - ayrıntısı](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Açıklamadan çıkarın `SetExecutionStrategy` satırından *WingtipToysConfiguration.cs* dosya.

## <a name="summary"></a>Özet

Bu öğreticide bağlantı dayanıklılığı ve komut kişiler tarafından ele desteklemek için bir Web Forms örnek uygulamayı değiştirmek öğrendiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bağlantı dayanıklılığı ve ASP.NET Web Forms içerisinde komutu kişiler tarafından ele gözden geçirdikten sonra ASP.NET Web Forms konusunu gözden geçirmeniz [ASP.NET 4.5 içinde zaman uyumsuz yöntemleri](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). Konu Visual Studio kullanarak zaman uyumsuz bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek.
