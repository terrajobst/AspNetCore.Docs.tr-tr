---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms bağlantı dayanıklılığı ve komut durdurma | Microsoft Docs
author: Erikre
description: Bu öğreticide, bağlantı dayanıklılığı ve komut durdurma desteklemek için örnek uygulamayı değiştirmek açıklar.
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 039923a91d957765fa8b2c0cfe11abc8790c1e88
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755669"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Forms bağlantı dayanıklılığı ve komut durdurma
====================
tarafından [Erik Reitan](https://github.com/Erikre)

Bu öğreticide, Wingtip Toys örnek uygulama, bağlantı dayanıklılığı ve komut durdurma destekleyecek şekilde değiştirir. Tipik bir bulut ortamında geçici hatalar oluştuğunda bağlantı dayanıklılığı etkinleştirerek Wingtip Toys örnek uygulamayı otomatik olarak veri aramaları yeniden deneyecek. Ayrıca, komut durdurma uygulayarak, Wingtip Toys örnek uygulamanın tüm yakalar SQL sorguları oturum veya bunları değiştirmek için veritabanına gönderilir.

> [!NOTE] 
> 
> Bu Web Forms öğretici temel Tom Dykstra'nın aşağıdaki MVC Öğreticisi:  
> [Bağlantı dayanıklılığı ve komut durdurma bir ASP.NET MVC uygulamasındaki Entity Framework ile](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Bağlantı dayanıklılığı sağlamak nasıl.
- Komut durdurma gerçekleştirme.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki yazılımlar bilgisayarınızda yüklü olduğundan emin olun:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework otomatik olarak yüklenir.
- Bu öğreticide Wingtip Toys proje içinde bahsedilen işlevselliğini uygulayabilmesi Wingtip Toys proje, örnek. Aşağıdaki bağlantıda indirme ayrıntıları sağlar:

    - [ASP.NET 4.5.1 kullanmaya başlama Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Bu öğreticiyi tamamlamadan önce ilgili öğretici serisinde, gözden geçirme düşünün [ASP.NET 4.5 Web Forms ve Visual Studio 2013 ile çalışmaya başlama](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Öğretici serisinin sahibi yardımcı olacak **WingtipToys** proje ve kod.

## <a name="connection-resiliency"></a>Bağlantı dayanıklılığı

Bir uygulamayı Windows azure'a dağıtmayı göz önünde bulundurun, dikkate alınması gereken bir seçenek veritabanına dağıtma **Windows** **Azure SQL veritabanı**, bir bulut veritabanı hizmetidir. Bir bulut veritabanı hizmeti, web sunucunuzun ve veritabanı sunucusuna doğrudan birlikte aynı veri merkezinde bağlı daha bağlandığınızda geçici bağlantı hataları genellikle daha sık. Bir bulut web sunucusu ve bir bulut veritabanı hizmeti, aynı veri merkezinde barındırılan olsa bile, yük Dengeleyiciler gibi sorunlar olabilir bunlar arasında daha fazla ağ bağlantıları vardır.

Ayrıca bir bulut hizmeti vermediğinin onlar tarafından etkilenebilir yani diğer kullanıcılar tarafından genellikle da paylaşılır. Ve ağ kapasitesi azaltmaya tabidir veritabanına erişiminiz olabilir. İzin daha sık erişmeyi denediğinizde anlamına gelir azaltma veritabanı hizmeti istisnalar fırlatıyorsa, *hizmet düzeyi sözleşmesi* (SLA).

Bir bulut hizmetine erişirken oluşabilecek birçok veya çoğu bağlantı sorunları geçici, diğer bir deyişle, bunlar kendilerini kısa sürede çözün. Bu nedenle bir veritabanı işlemi deneyin ve genellikle geçici hata türünü almak kısa bekleyin ve işlemi başarılı olabilir sonra işlemi yeniden çalışabilir. Otomatik olarak yeniden deneyerek geçici hataları işlemek, kullanıcılarınız için bir çok daha iyi bir deneyim sağlayabilirsiniz. bunların çoğu müşteri için görünmez yapma. Entity Framework 6 bağlantı dayanıklılığı özelliğini yeniden deneme işlemi başarısız SQL sorgularının otomatikleştirir.

Bağlantı dayanıklılığı özelliğini belirli bir veritabanı hizmeti için uygun şekilde yapılandırılması gerekir:

1. Bu, hangi özel durumları geçici bilmesi gerekir. Ağ bağlantısı altında geçici bir kaybına neden hataları göre program hatalar, örneğin neden hataları yeniden denemek istiyor.
2. Uygun miktarda bir başarısız bir işlemin yeniden denemeler arasındaki süre beklemeniz gerekir. Bir kullanıcının yanıt burada bekleyen bir çevrimiçi web sayfası için daha uzun bir toplu işlem için yeniden denemeler arasındaki bekleyebilirsiniz.
3. Uygun sayıda vazgeçmeden önce bir kez denemeye sahiptir. Çevrimiçi bir uygulamada yaptığınız bir toplu işlemde birden fazla kez yeniden denemek isteyebilirsiniz.

Bu ayarları el ile bir Entity Framework sağlayıcısı tarafından desteklenen her veritabanı ortam yapılandırabilirsiniz.

Bağlantı dayanıklılığı etkinleştirmek için yapmanız gereken tek şey bir sınıf türetildiği derlemenizi oluşturun `DbConfiguration` sınıfı ve bu sınıfta, bir sonraki dönem için yeniden deneme ilkesi olan Entity Framework, SQL veritabanı yürütme stratejisi ayarlayın.

### <a name="implementing-connection-resiliency"></a>Uygulama bağlantı dayanıklılığı

1. İndir ve Aç [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) örneği Visual Studio'da Web Forms uygulaması.
2. İçinde *mantıksal* klasörü **WingtipToys** uygulama adlı bir sınıf dosyası ekleyin *WingtipToysConfiguration.cs*.
3. Varolan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework otomatik olarak bulur, türetilen bir sınıf içinde kod çalıştırır `DbConfiguration`. Kullanabileceğiniz `DbConfiguration` Aksi takdirde yaptığınız kod yapılandırma görevlerini yapmak için sınıf *Web.config* dosya. Daha fazla bilgi için [EntityFramework kod tabanlı yapılandırma](https://msdn.microsoft.com/data/jj680699).

1. İçinde *mantıksal* açık klasör *AddProducts.cs* dosya.
2. Ekleme bir `using` bildirimi `System.Data.Entity.Infrastructure` sarı renkte vurgulanmış gösterildiği gibi:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Ekleme bir `catch` bloğunu `AddProduct` yöntemi böylece `RetryLimitExceededException` vurgulanmış sarı günlüğe kaydedilir:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Ekleyerek `RetryLimitExceededException` özel durum, daha iyi günlüğü sağlayın veya burada seçim yapabilecekleri işlemi yeniden denemek için kullanıcıya bir hata iletisi görüntüler. Yakalama tarafından `RetryLimitExceededException` özel durum, yalnızca hatalar geçici zaten çalıştı ve birkaç kez başarısız oldu. Döndürülen özel durumu içinde sarmalanmış `RetryLimitExceededException` özel durum. Ayrıca, ayrıca bir genel bir catch bloğu eklendi. Hakkında daha fazla bilgi için `RetryLimitExceededException` özel durumu görmek [Entity Framework bağlantı dayanıklılığı / yeniden deneme mantığı](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Komut durdurma

Nasıl bir yeniden deneme ilkesi etkinleştirdiyseniz, beklendiği gibi çalışıp çalışmadığını doğrulamak için test? Geçici bir hata meydana gelmesine özellikle ne zaman yerel olarak çalıştırıyorsanız, ve gerçek geçici hatalar bir otomatik birim testine tümleştirmek özellikle zor zorlamak çok kolay değildir. Bağlantı dayanıklılığı özelliğini test etmek için SQL Server için Entity Framework gönderen sorguları kesecek ve SQL Sunucu yanıtı genellikle geçici bir özel durum türüyle değiştirin. bir yol gerekir.

Bulut uygulamaları için en iyi uygulama uygulamak için sorgu durdurma de kullanabilirsiniz: günlük gecikme süresi ve başarı veya başarısızlık tüm çağrıları için dış veritabanı hizmetleri gibi hizmetleri.

Öğreticinin bu bölümünde Entity Framework'ün kullanacağınız [ *durdurma özelliği* ](https://msdn.microsoft.com/data/dn469464) hem oturum hem de geçici hatalar benzetimi için.

### <a name="create-a-logging-interface-and-class"></a>Günlük arabirim ve sınıf oluşturma

Kullanarak yapmak için günlüğü için en iyi uygulama olan bir [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) çağrıları sabit kodlama yerine `System.Diagnostics.Trace` veya günlük sınıfı. Bu günlük mekanizma daha sonra yapmanız gereken değiştirmeyi kolaylaştırır. Bu nedenle bu bölümde, günlüğe kaydetme arabirimi ve uygulamak için bir sınıf oluşturacaksınız.

Yukarıdaki yordama bağlı, indirdiğiniz açıldı ve **WingtipToys** örnek Visual Studio'da uygulama.

1. Bir klasörde oluşturmak **WingtipToys** adlandırın ve proje *günlüğü*.
2. İçinde *günlüğü* klasöründe adlı bir sınıf dosyası oluşturma *ILogger.cs* varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Günlükleri göreceli önemini gösteren üç izleme düzeyi ve veritabanı sorguları gibi dış hizmet çağrıları için gecikme bilgileri sağlamak üzere tasarlanmış bir arabirim sağlar. Günlük yöntemlerini bir özel durum geçirmenize olanak tanıyan aşırı yüklemeleri vardır. Bu, böylelikle yığın izlemesi ve iç özel durumlar dahil olmak üzere özel durum bilgileri, her günlük yöntem çağrısının uygulama boyunca gerçekleştirilen güvenmek yerine arabirimini uygulayan bir sınıf tarafından güvenilir bir şekilde günlüğe kaydedilir.  
  
   `TraceApi` Yöntemleri, her SQL veritabanı gibi bir dış hizmete çağrı gecikme süresini izlemenize olanak sağlar.
3. İçinde *günlüğü* klasöründe adlı bir sınıf dosyası oluşturma *Logger.cs* varsayılan kodu aşağıdaki kodla değiştirin:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Uygulama kullanan `System.Diagnostics` izleme yapmak için. Bu izleme bilgilerini oluşturup kolaylaştıran .NET yerleşik bir özelliğidir. Kullanabileceğiniz birçok &quot;dinleyicileri&quot; ile kullanabileceğiniz `System.Diagnostics` izleme dosyaları, örneğin, günlükleri yazmak veya bunları Windows azure'da blob depolama alanına yazılacak. Bazı seçenekler ve daha fazla bilgi için diğer kaynakların bağlantılarını görmek [Visual Studio'da Windows Azure Web sitelerinin sorunlarını giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Bu öğretici için yalnızca Visual Studio günlüklerine bakın **çıkış** penceresi.

Bir üretim uygulamasında dışında izleme çerçeveleri kullanarak düşünmek isteyebilirsiniz `System.Diagnostics`ve `ILogger` arabirimi görece yapmak isterseniz, farklı izleme mekanizması geçiş yapmayı kolaylaştırır.

### <a name="create-interceptor-classes"></a>Dinleyiciyi sınıfları oluşturma

Ardından, veritabanı, geçici hataların benzetimini yapmak için tek ve günlük kaydı yapmak için tek bir sorgu göndermek için gittiği her zaman Entity Framework içine çağıracak sınıfları oluşturacaksınız. Bu dinleyiciyi sınıfların türetilmesi gerekir `DbCommandInterceptor` sınıfı. Bunlar, sorgu çalıştırılmak üzere olduğunda otomatik olarak adlandırılır yöntemi geçersiz kılmalar yazın. Bu yöntemleri inceleyin veya veritabanına gönderilen sorgu günlük ve veritabanına gönderilmeden önce sorguyu değiştirin ya da bir şey için Entity Framework kendiniz sorgu dahi veritabanına geçmeden döndürür.

1. Veritabanına gönderilmeden önce her SQL sorgusu başlar.SSH dinleyiciyi sınıf oluşturmak için adlı bir sınıf dosyası oluşturma *InterceptorLogging.cs* içinde *mantığı* ile kod klasörü ve varsayılan Değiştir Aşağıdaki kodu:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Başarılı sorguları veya komutlar için bu kod gecikme süresi bilgileriyle bir bilgi günlüğüne yazar. Özel durumlar için bir hata günlüğü oluşturur.
2. Girdiğiniz zaman, işlevsiz geçici hatalar oluşturan dinleyiciyi sınıfı oluşturmak için &quot;Throw&quot; içinde **adı** adlı sayfasında metin *AdminPage.aspx*, bir sınıf oluşturun adlı dosyayı *InterceptorTransientErrors.cs* içinde *mantıksal* klasörü ve varsayılan Değiştir kodu aşağıdaki kod ile:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Bu kod yalnızca geçersiz kılma `ReaderExecuting` birden çok veri satırlarını döndüren sorgular için çağrılan yöntem. Diğer sorgu türleri için bağlantı dayanıklılığı denetlemek isterseniz, ayrıca geçersiz `NonQueryExecuting` ve `ScalarExecuting` yöntemleri günlük dinleyiciyi yapar.  
  
   Daha sonra "Yönetici" oturum açın ve seçin **yönetici** üst gezinti çubuğundaki bağlantı. Ardından *AdminPage.aspx* adlı bir ürün ekleyeceğiniz sayfa &quot;Throw&quot;. Bu kod, hata numarası 20, genellikle geçici olarak bilinen bir tür için işlevsiz bir SQL veritabanı özel durumu oluşturur. Şu anda geçici kabul edilen diğer hata numaralarını 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 ve 40613, ancak bu SQL veritabanı'nın yeni sürümlerinde değiştirilebilir. Ürün kodunda izleyerek "TransientErrorExample" yeniden adlandırılacak *InterceptorTransientErrors.cs* dosya.  
  
   Sorgu çalıştırmak ve arka sonuçları geçirme yerine Entity Framework özel durum kodu döndürür. Geçici özel durum döndürülür *dört* kez ve ardından sorgu veritabanına geçirme normal yordama kodunu döner.

    Her şeyi açmış olduğu için son başarılı önce dört kez sorguyu yürütmek Entity Framework çalışır ve uygulamadaki tek fark, sorgu sonuçlarını içeren bir sayfa işlemek için daha uzun sürer görmek mümkün olacaktır.  
  
   Entity Framework yeniden denenme sayısı yapılandırılabilir; SQL veritabanı yürütme ilkesi için varsayılan değer olduğundan kod dört kez belirtir. Yürütme ilkesini değiştirirseniz, geçici hataları oluşturulan kaç kez belirten kodunu buraya de değişiklik gerekmektedir. Entity Framework atar, böylece daha fazla özel durum oluşturmak için kod da değiştirebilir `RetryLimitExceededException` özel durum.
3. İçinde *Global.asax*, aşağıdaki using deyimlerini:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Ardından, vurgulanan satırları ekleyin `Application_Start` yöntemi:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Bu kod satırlarını ne Entity Framework veritabanı için sorgu gönderdiğinde çalıştırılacak dinleyiciyi kodunuzu neden olan. Geçici bir hata simülasyonu için ayrı dinleyiciyi sınıfları oluşturduğunuz ve günlüğe kaydetme, ayrı ayrı etkinleştirebilir ve devre dışı olduğundan dikkat edin.   
  
 Kullanarak dinleyicileri ekleyebilirsiniz `DbInterception.Add` yöntemi kodunuzu; herhangi bir yerindeki olması gerekmez `Application_Start` yöntemi. Başka bir seçenek de dinleyicileri eklemediyseniz `Application_Start` yöntemi, güncelleştirmek veya adlı bir sınıf eklemek için olacak *WingtipToysConfiguration.cs* ve yukarıdaki kod oluşturucusunun sonunda `WingtipToysbConfiguration` sınıfı.

Her yerde olması dikkat edin yürütmek için bu kod put `DbInterception.Add` aynı dinleyiciyi için birden çok kez veya ek dinleyiciyi örnekleri elde edersiniz. Örneğin, günlük dinleyiciyi iki kez eklerseniz, her SQL sorgusu için iki günlüklerini görürsünüz.

Kesiciler, kayıt sırasına göre yürütülür (sırayı `DbInterception.Add` yöntemi çağrılır). Sipariş dinleyiciyi içinde yaptığınız işlemlere bağlı olarak önemli. Örneğin, bir dinleyiciyi alır, SQL komut değişebilir `CommandText` özelliği. SQL komutu değiştirirseniz, sonraki dinleyiciyi değiştirilen SQL komutu, not özgün SQL komut alırsınız.

Geçici hatalar kullanıcı Arabiriminde farklı bir değer girerek neden olanak tanır şekilde, geçici hata benzetimi kodu yazdığınız. Alternatif olarak, her zaman belirli bir parametre için denetlemeden geçici özel durumlar dizisi oluşturmak için dinleyiciyi kod yazabilirsiniz. Yalnızca geçici hataları oluşturmak istediğinizde dinleyiciyi sonra ekleyebilirsiniz. Ancak, veritabanı başlatma tamamlandıktan sonra bunu yaparsanız, dinleyiciyi kadar eklemeyin. Diğer bir deyişle, geçici hatalar üretme başlamadan önce varlık kümelerini birinde bir sorgu gibi en az bir veritabanı işlemi yapın. Entity Framework, veritabanı başlatma sırasında çeşitli sorguları yürütür ve başlatma sırasında hatalar tutarsız bir duruma geçmesine alınacak bağlam neden olabilir, bu işlem, yürütülen değildir.

## <a name="test-logging-and-connection-resiliency"></a>Test günlüğü ve bağlantı dayanıklılığı

1. Visual Studio'da **F5** hata ayıklama modu ve oturum açma olarak "Admin" parola olarak "Pa$ $word" kullanarak uygulamayı çalıştırın.
2. Seçin **yönetici** üst gezinti çubuğundan.
3. "Durum" adlı yeni bir ürün ile ilgili açıklama, fiyat ve görüntü dosyası girin.
4. Tuşuna **Ürün Ekle** düğmesi.  
   Tarayıcısı Entity Framework, sorgu birkaç kez deniyor sırasında birkaç saniye kilitlenmesine gibi görünüyor fark edeceksiniz. İlk yeniden deneme çok hızlı bir şekilde gerçekleşir ve her bir ek yeniden denemeden önce beklemeyi artırır. Bu işlemi her yeniden deneme çağrılmadan önce artık bekleyen *üstel geri alma* .
5. Sayfa artık yüklenecek atttempting tamamlanana kadar bekleyin.
6. Proje durdurun ve Visual Studio'yu Ara **çıkış** izleme çıktısını görmek için penceresi. Bulabilirsiniz **çıkış** penceresini seçerek **hata ayıklama**  - &gt; **Windows**  - &gt;  **Çıkış**. Geçmiş, Günlükçü tarafından yazılmış birden fazla günlük kaydırmanız gerekebilir.  
  
   Veritabanına gönderilen gerçek SQL sorguları gördüğünüz dikkat edin. Bazı ilk sorgular ve kullanmaya başlamak için Entity Framework yapan komutları veritabanı sürümü ve geçiş geçmiş tablosu denetimini görürsünüz.   
    ![Çıktı Penceresi](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Uygulamayı durdurun ve yeniden sürece bu test tekrarlanamıyor unutmayın. Bağlantı dayanıklılığı birden çok kez uygulamanın tek bir çalışma içinde test etmek istiyorsanız, hata sayaç sıfırlamak için kod yazabilirsiniz `InterceptorTransientErrors` .
7. Farkı görmek için yürütme stratejisi (yeniden deneme ilkesi) hale getirdiğini, yorum `SetExecutionStrategy` satırına *WingtipToysConfiguration.cs* dosyası *mantıksal* klasöründe şunu çalıştırın **yönetici**  sayfasında hata ayıklama modunda yeniden ve adlı Ürün Ekle &quot;Throw&quot; yeniden.  
  
   Hemen bir sorguyu ilk kez çalıştığında, bu süre ilk oluşturulan özel durumla ilgili hata ayıklayıcıyı durdurur.  
    ![Hata ayıklama - ayrıntısı](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Açıklamadan çıkarın `SetExecutionStrategy` satırına *WingtipToysConfiguration.cs* dosya.

## <a name="summary"></a>Özet

Bu öğreticide bağlantı dayanıklılığı ve komut durdurma desteklemek için bir Web Forms örnek uygulaması değiştirileceğini öğrendiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bağlantı dayanıklılığı ve komut durdurma ASP.NET Web Forms'da gözden geçirdikten sonra ASP.NET Web Forms konusunu gözden geçirmeniz [ASP.NET 4.5 içinde zaman uyumsuz yöntemler](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). Konu Visual Studio kullanarak zaman uyumsuz bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır.
