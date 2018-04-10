---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Bir özel hata sayfası (C#) görüntüleme | Microsoft Docs
author: rick-anderson
description: Bir ASP.NET web uygulamasında bir çalışma zamanı hatası meydana geldiğinde kullanıcı neleri? Yanıt bağlıdır Web sitesinin &lt;customErrors&gt; yapılandırma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: f01a0f3af3680d53639512d7a86ac1a8645d00e2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="displaying-a-custom-error-page-c"></a>Bir özel hata sayfası (C#) görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Bir ASP.NET web uygulamasında bir çalışma zamanı hatası meydana geldiğinde kullanıcı neleri? Yanıt bağlıdır Web sitesinin &lt;customErrors&gt; yapılandırma. Varsayılan olarak, bir çalışma zamanı hatası oluştu proclaiming bir sayfanın sarı ekran kullanıcılara gösterilir. Bu öğretici, sitenizin görünüm eşleşen bir aesthetically güzel görünen özel hata sayfası için bu ayarları özelleştirmek nasıl gösterir.


## <a name="introduction"></a>Giriş

İdeal şartlar altında hiç çalışma zamanı hataları olacaktır. Programcıları bir hata kodu ile n öğeli yazarsınız ve güçlü kullanıcı girdisi doğrulama ve dış veritabanı sunucuları ve e-posta sunucuları gibi kaynakları hiçbir zaman çevrimdışı. Elbette, gerçekte kaçınılmaz hatalardır. .NET Framework sınıfları, bir özel durum atma tarafından bir hata işaret eder. Örneğin, bir SqlConnection çağırma nesnesinin Open yöntemini bir bağlantı dizesi tarafından belirtilen veritabanına bir bağlantı kurar. Ancak, veritabanı çalışmıyorsa veya kimlik bilgileri bağlantı dizesinde geçersiz olduğunda sonra açık yöntemi atar bir `SqlException`. Özel durumlar tarafından kullanımını işlenebilir `try/catch/finally` engeller. Varsa içindeki kod bir `try` bloğu bir özel durum oluşturur, geliştirici hatadan kurtarmak burada deneyebilirsiniz denetim uygun catch bloğu ile aktarılır. Eşleşen hiçbir catch bloğu yok ya da bir özel durum belirtti kod bir try bloğu içinde değilse, özel durum çağrı yığını search, Yukarı percolates `try/catch/finally` engeller.

İşlenen olmadan, tüm ASP.NET çalışma zamanı en fazla özel durum köpürür varsa [ `HttpApplication` sınıfı](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)'s [ `Error` olay](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) tetiklenir ve yapılandırılmış *hata sayfası*  görüntülenir. Varsayılan olarak, ASP.NET affectionately olarak adlandırılır bir hata sayfası görüntüler [Ölüm sarı bir ekran](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). YSOD iki sürümü vardır: bir uygulamada hata ayıklama geliştiricilere yararlı gösterir özel durum ayrıntıları, yığın izleme ve diğer bilgileri (bkz **Şekil 1**); diğer yalnızca bir çalışma zamanı hatası (bkz: olduğunubildiren **Şekil 2**).

Özel durum ayrıntıları YSOD uygulamada hata ayıklama geliştiriciler için oldukça faydalıdır, ancak son kullanıcılara bir YSOD gösteren tacky ve profesyonel. Bunun yerine, son kullanıcıların sitenin görünüm durumu açıklayan daha kullanıcı dostu bir yazı olabilir ile tutan bir hata sayfası dikkat edilmelidir. İyi haber böyle bir özel hata sayfası oluşturma oldukça kolay olmasıdır. Bu öğretici ASP göz başlar. NET'in farklı hata sayfaları. Ardından, kullanıcıların bir özel hata sayfası bir hata karşısında göstermek için web uygulamasının nasıl yapılandırılacağını gösterir.

### <a name="examining-the-three-types-of-error-pages"></a>Hata sayfaları üç tür İnceleme

Bir ASP.NET uygulaması hata sayfaları üç tür birini işlenmeyen bir özel durum olduğunda ortaya görüntülenir:

- Özel durum ayrıntıları sarı ekran, ölüm hata sayfası
- Çalışma zamanı hata sarı ekran, ölüm hata sayfası veya
- Özel hata sayfası

Hata sayfası geliştiricileri ile özel durum ayrıntıları YSOD en bilinen'dır. Varsayılan olarak, bu sayfa yerel olarak ziyaret eden kullanıcılara gösterilir ve bu nedenle site geliştirme ortamında test edilirken bir hata oluştuğunda, gördüğünüz sayfasıdır. Adından da anlaşılacağı gibi özel durum ayrıntıları YSOD ayrıntılarını özel durum oluştu - türü, iletisi ve yığın izleme sağlar. Daha ASP.NET sayfanızın arka plandaki kod sınıfı kodda tarafından özel durumu oluştu ve uygulamanın hata ayıklama için yapılandırılmışsa, ardından özel durum ayrıntıları YSOD de bu satırı kodunu (ve birkaç satır kod yukarıdaki ve altındaki) gösterilir.

**Şekil 1** özel durum ayrıntıları YSOD sayfası gösterir. Tarayıcının adres penceresindeki URL'ye dikkat edin: `http://localhost:62275/Genre.aspx?ID=foo`. Sözcüğünün `Genre.aspx` sayfa içinde belirli bir tarzını Kitap incelemeleri listeler. Olmasını gerektirir `GenreId` değeri (bir `uniqueidentifier`) geçirilen sorgu dizesi; Örneğin, kurgu incelemeleri görüntülemek için uygun URL'dir `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Olmayan bir varsa`uniqueidentifier` değeri geçirilir, sorgu dizesi (örneğin, "foo") aracılığıyla bir özel durum oluşur.

> [!NOTE]
> Tanıtım web uygulamasındaki bu hatayı yeniden oluşturmaya yüklenebilir ya da ziyaret yapabilecekleriniz `Genre.aspx?ID=foo` doğrudan ya da "Çalışma zamanı hatası oluştur" bağlantıya tıklayın `Default.aspx`.


İçinde sunulan özel durum bilgilerini Not **Şekil 1**. Özel durum iletisi "dönüştürme bir karakter dizesinden benzersiz tanımlayıcıya dönüştürme yapılırken başarısız oldu" sayfanın en üstünde bulunur. Özel durumun türünü `System.Data.SqlClient.SqlException`, de listelenir. Yığın izleme yoktur.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Şekil 1**: özel durum ayrıntıları YSOD özel durumla ilgili bilgiler içerir  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-custom-error-page-cs/_static/image3.png))

YSOD başka türden çalışma zamanı hatası YSOD olduğu ve gösterilen **Şekil 2**. Çalışma zamanı hatası YSOD bir çalışma zamanı hatası oluştu ziyaretçi bilgilendirir, ancak oluşturulan özel durum hakkında hiçbir bilgi içermez. (Bunu ancak değiştirerek hata ayrıntılarını görünür yapmak nasıl yönergelerinizi `Web.config` profesyonel Ara YSOD kılan bir parçası olan dosya.)

Varsayılan olarak, çalışma zamanı hatası YSOD uzaktan ziyaret eden kullanıcılara gösterilir (aracılığıyla http://www.yoursite.com)tarayıcının adres çubuğunda URL tarafından yi gibi **Şekil 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Geliştiriciler hata ayrıntılarını bilmek ilgilenen ancak olası güvenlik açıkları ve diğer hassas bilgiler ziyaret herkes gösterebilir gibi bilgileri canlı sitede gösterilmiyor çünkü iki farklı YSOD ekranda var, Site.

> [!NOTE]
> Aşağıdaki ve DiscountASP.NET, web ana bilgisayarı olarak kullanıyorsanız, çalışma zamanı hatası YSOD Canlı sitesini ziyaret ederken görüntülemez fark edebilirsiniz. Özel durum ayrıntıları YSOD göstermek için varsayılan olarak yapılandırılan sunucularını DiscountASP.NET sahip olmasıdır. İyi haber ekleyerek bu varsayılan davranışı geçersiz kılabilirsiniz olan bir `<customErrors>` için bölüm, `Web.config` dosya. "Yapılandırma hata sayfası görüntülendiği" bölümü inceler `<customErrors>` ayrıntı bölümünde.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Şekil 2**: çalışma zamanı hatası YSOD tüm hata ayrıntılarını içermez  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-custom-error-page-cs/_static/image6.png))

Hata sayfasının üçüncü oluşturduğunuz bir web sayfası özel hata sayfası türüdür. Özel hata sayfasının sayfanın görünüm birlikte kullanıcıya görüntülenen bilgileri tam denetime sahip olursunuz avantajdır; özel hata sayfası, aynı ana sayfa ve stiller diğer sayfalar halinde kullanabilirsiniz. "Özel hata sayfası kullanma" bölümüne bir özel hata sayfası oluşturma ve işlenmeyen bir özel durum durumunda görüntülemek için yapılandırma anlatılmaktadır. **Şekil 3** bu özel hata sayfasının fişini yoğun sunar. Gördüğünüz gibi daha fazla profesyonel görünümlü sarı ekranlar, Şekil 1 ve 2'de gösterilen renkli kilitlenme ya da daha hata sayfasının Görünüm ve yapısını gereklidir.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Şekil 3**: bir özel hata sayfası daha özel bir görünüm sunar  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-custom-error-page-cs/_static/image9.png))

Tarayıcının adres çubuğunda incelemek için bir dakikanızı ayırın **Şekil 3**. Adres çubuğunda özel hata sayfasının URL'sini gösterdiğine dikkat edin (`/ErrorPages/Oops.aspx`). Şekil 1 ve 2'de, sarı ekranlar ölüm hata kaynaklandığı aynı sayfa gösterilir (`Genre.aspx`). Özel hata sayfası hatanın oluştuğu aracılığıyla sayfasının URL'sini geçirilen `aspxerrorpath` sorgu dizesi parametresi.

## <a name="configuring-which-error-page-is-displayed"></a>Hata sayfası yapılandırma görüntülenir

Üç olası hata sayfalarının görüntülendiği iki değişkenine dayanır:

- Yapılandırma bilgilerini `<customErrors>` bölümünde ve
- Olup kullanıcı yerel olarak veya uzaktan sitesinden değil.

[ `<customErrors>` Bölüm](https://msdn.microsoft.com/library/h0hfz6fc.aspx) içinde `Web.config` hangi hata sayfası gösterilir etkileyen iki özniteliklere sahiptir: `defaultRedirect` ve `mode`. `defaultRedirect` Özniteliği isteğe bağlıdır. Sağlanırsa, özel hata sayfasının URL'sini belirtir ve özel hata sayfası yerine çalışma zamanı hatası YSOD gösterilen olduğunu gösterir. `mode` Özniteliği gereklidir ve üç değerden birini kabul eder: `On`, `Off`, veya `RemoteOnly`. Bu değerler, aşağıdaki davranış vardır:

- `On` -özel hata sayfası veya çalışma zamanı hatası YSOD yerel veya uzak olup olmadıkları bağımsız olarak tüm ziyaretçiler için gösterildiğini belirtir.
- `Off` -Yerel veya uzak olup olmadıkları bağımsız olarak tüm ziyaretçiler için özel durum ayrıntıları YSOD görüntüleneceğini belirtir.
- `RemoteOnly` -özel durum ayrıntıları YSOD yerel ziyaretçileri gösterilmese özel hata sayfası veya çalışma zamanı hatası YSOD uzak ziyaretçileri göründüğünü gösterir.

Aksi belirtilmedikçe, mod özniteliği kümesine gibi ASP.NET davranır `RemoteOnly` ve belirtilmemiş bir `defaultRedirect` değeri. Diğer bir deyişle, varsayılan çalışma zamanı hatası YSOD uzak ziyaretçileri gösterilmese özel durum ayrıntıları YSOD yerel ziyaretçilerine görüntülenir davranıştır. Ekleyerek bu varsayılan davranışı geçersiz kılabilirsiniz bir `<customErrors>` web uygulamanızın bölümüne `Web.config file.`

## <a name="using-a-custom-error-page"></a>Özel hata sayfası kullanma

Her web uygulaması bir özel hata sayfasının olması gerekir. Çalışma zamanı hatası YSOD daha profesyonel görünümlü bir alternatif sunar, oluşturmak kolaydır ve özel hata sayfası kullanmak için uygulamayı yapılandırma yalnızca birkaç dakika sürer. İlk adım, özel hata sayfası oluşturuyor. Adlı kitap incelemeleri uygulamaya yeni bir klasör eklediğiniz `ErrorPages` ve yeni bir ASP.NET sayfası adlı eklenen `Oops.aspx`. Otomatik olarak aynı görünüme ve hisse devralması sitenizdeki sayfaları geri kalanı olarak aynı ana sayfayı kullanan sayfa sahip olabilir.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Şekil 4**: bir özel hata sayfası oluşturun

Ardından, hata sayfası için içerik oluşturmak birkaç dakikanızı ayırın. Bunun yerine basit özel hata sayfası belirten bir ileti beklenmeyen bir hata oluştu ve bir bağlantı sitenin giriş sayfasına geri ile oluşturduğunuzu düşünün.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Şekil 5**: özel hata sayfası tasarlama  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-custom-error-page-cs/_static/image14.png))

Hata sayfası tamamlandı web uygulama çalışma zamanı hatası YSOD yerine özel hata sayfasını kullanmak üzere yapılandırın. Bu hata sayfasının URL'sini belirterek gerçekleştirilir `<customErrors>` bölümün `defaultRedirect` özniteliği. Aşağıdaki biçimlendirmede, uygulamanızın eklemek `Web.config` dosyası:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Yukarıdaki biçimlendirme yerel olarak uzaktan ziyaret bu kullanıcılar için özel hata sayfası Oops.aspx kullanırken ziyaret eden kullanıcılar için özel durum ayrıntıları YSOD göstermek üzere uygulamayı yapılandırır. Bu eylem görmek için Web sitenizi üretim ortamına dağıtma ve canlı sitede bir geçersiz sorgu dizesi değeri olan, daha sonra Genre.aspx sayfasını ziyaret edin. Özel hata sayfasını görmeniz gerekir (geri başvurmak **Şekil 3**).

Özel hata sayfası yalnızca uzak kullanıcılara gösterilen doğrulamak için ziyaret `Genre.aspx` geliştirme ortamından geçersiz bir sorgu dizesi sayfası. Özel durum ayrıntıları YSOD hala görmeniz gerekir (geri başvurmak **Şekil 1**). `RemoteOnly` Ayar sağlar özel durum ayrıntılarını görmek yerel olarak çalışan geliştiricilere devam ederken üretim ortamına siteyi ziyaret eden kullanıcıların özel hata sayfası konusuna bakın.

## <a name="notifying-developers-and-logging-error-details"></a>Geliştiriciler bildirme ve hata ayrıntıları günlüğe kaydetme

Geliştirme ortamında oluşan hataları kendi bilgisayarında durduğunu Geliştirici nedeni. Aynen özel durumun bilgiler özel durum ayrıntıları YSOD gösterilir ve aynen hata oluştuğu sırada aynen çalıştığını hangi adımların bilir. Ancak üretimde bir hata ortaya çıkarsa, geliştirici hiçbir bilgi sitesini ziyaret son kullanıcı hatası rapor Süren sürece bir hata oluştu. Ve kullanıcı bir hata oluştu, özel durum türü, ileti ve hatanın nedenini tanılayın, let alone Düzelt zor olabilir yığın izlemesi bilmeden geliştirme ekibi uyarı şekilde kendi dışında aşsa bile.

Bu nedenlerle, üretim ortamında herhangi bir hata bazı kalıcı depoya (örneğin, bir veritabanı) ve, oturum en önemli geliştiriciler bu hatadan uyarılırsınız. Özel hata sayfası, bu günlüğü ve bildirim yapmak için iyi bir yerdir gibi görünebilir. Ne yazık ki, özel hata sayfası hata ayrıntılarını erişiminiz yok ve bu nedenle bu bilgileri günlüğe kaydetmek için kullanılamaz. İyi haber çeşitli yollarla hata ayrıntılarını müdahale ve bunları açmak için vardır ve bu konuda daha ayrıntılı sonraki üç öğreticileri keşfedin ' dir.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Farklı bir özel hata sayfaları için farklı bir HTTP hata durumları kullanma

Bir özel durum ASP.NET sayfası tarafından oluşturulan ve değil gerçekleştirilir, özel durum yapılandırılmış hata sayfasını görüntüler ASP.NET çalışma kadar percolates. Bir isteği ASP.NET motoruna gelir ancak herhangi bir nedenle işlenemiyor - belki istenen dosya bulunamadı veya okunduğunda değil, izinler için dosyayı - devre dışı sonra ASP.NET altyapısı başlatır bir `HttpException`. ASP.NET sayfaları, oluşturulan özel durumlar gibi bu özel durumun görüntülenmesi uygun hata sayfasına neden çalışma kadar köpürür.

Bir kullanıcı özel hata sayfasını görürsünüz bulunamadı bir sayfa isterse üretim web uygulaması için yani olmasıdır. **Şekil 6** böyle bir örneği gösterir. İstek için bir mevcut olmayan sayfa olduğundan (`NoSuchPage.aspx`), bir `HttpException` oluşturulur ve özel hata sayfası görüntülenir (referansı Not `NoSuchPage.aspx` içinde `aspxerrorpath` sorgu dizesi parametresi).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Şekil 6**: ASP.NET çalışma zamanı yapılandırılmış hata sayfası içinde yanıt geçersiz bir istek için görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-custom-error-page-cs/_static/image17.png))

Varsayılan olarak, tüm hata türleri görüntülenecek aynı özel hata sayfası neden. Bununla birlikte, belirli HTTP durum kodu kullanarak bir için farklı bir özel hata sayfası belirtebilirsiniz `<error>` içinde alt öğelerin `<customErrors>` bölümü. Örneğin, bir sayfa bulunamadı sahip bir HTTP durum kodu 404 hatası durumunda görüntülenen farklı hata sayfası için güncelleştirme `<customErrors>` bölümü aşağıdaki biçimlendirme eklenerek:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Uzaktan ziyaret eden bir kullanıcının var olmayan bir ASP.NET kaynağı istediğinde yerinde bu değişiklikle, bunlar yönlendirilecek `404.aspx` özel hata sayfası yerine `Oops.aspx`. Olarak **Şekil 7** gösterilmektedir, `404.aspx` sayfası, genel özel hata sayfası daha fazla belirli bir ileti içerebilir.

> [!NOTE]
> Kullanıma [404 hata sayfaları, bir fazla kez](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) etkili 404 hata sayfaları oluşturma konusunda yönergeler için.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Şekil 7**: Özel 404 hata sayfası daha fazla hedeflenen bir ileti görüntüler `Oops.aspx`  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-custom-error-page-cs/_static/image20.png)) 

Bildiğiniz için `404.aspx` sayfası kullanıcı bulunamadı bir sayfa için bir istek yaptığında, yalnızca ulaşıldığında, bu belirli tür hatalara yönelik kullanıcı yardımcı olmak için işlevsellik eklemek için bu özel hata sayfası geliştirebilirsiniz. Örneğin, hatalı URL'leri iyi URL'lere bilinen eşleşen bir veritabanı tablosu oluşturur ve ardından sahip `404.aspx` tablo ve kullanıcı çalışıyor olabilir ulaşması sayfaları önermek özel hata sayfası bir sorgu çalıştırın.

> [!NOTE]
> Özel hata sayfası, yalnızca ASP.NET altyapısı tarafından işlenen bir kaynak için bir istek yapıldığında görüntülenir. Biz anlatıldığı gibi [arasındaki temel farklılıklar IIS ve ASP.NET Geliştirme Sunucusu](core-differences-between-iis-and-the-asp-net-development-server-cs.md) Öğreticisi, web sunucusu belirli isteklerini işlemek kendisi. Varsayılan olarak, IIS, ASP.NET altyapısı çağırmadan sunucu işlemleri istekleri görüntüler ve HTML dosyaları gibi statik içerik web. Mevcut olmayan görüntü dosyası kullanıcı isterse, sonuç olarak, bunlar geri ASP yerine IIS varsayılan 404 hata iletisi alırsınız. NET'in yapılandırılmış hata sayfası.


## <a name="summary"></a>Özet

Bir ASP.NET uygulamasındaki işlenmeyen bir özel durum meydana geldiğinde kullanıcı üç hata sayfaları birini gösterilir: özel durum ayrıntıları sarı ekran, ölüm; çalışma zamanı hatası sarı renkli kilitlenme; ekranı veya bir özel hata sayfası. Hangi hata sayfası görüntülenir uygulamaya ait bağlıdır `<customErrors>` yapılandırması ve yerel olarak veya uzaktan kullanıcı ziyaret olup olmadığı. Özel durum ayrıntıları YSOD yerel ziyaretçileri ve çalışma zamanı hatası YSOD uzak ziyaretçileri göstermek için varsayılan davranıştır.

Çalışma zamanı hatası YSOD hassas olabilecek hata bilgilerini sitesini ziyaret kullanıcıdan gizler olsa da, sitenizin görünüm keser ve buggy Ara uygulamanızı sağlar. Daha iyi bir yaklaşım kapsar oluşturma ve özel hata sayfası tasarlama ve onun URL belirterek bir özel hata sayfası kullanmaktır `<customErrors>` bölümün `defaultRedirect` özniteliği. Birden çok özel hata sayfaları farklı HTTP hata durumları için bile olabilir.

Özel hata sayfasını bir Web sitesi üretim stratejisi işleme kapsamlı bir hata ilk adımdır. Geliştiricisi hata, uyarı ve ayrıntılarını oturum de önemli adımlardır. Sonraki üç öğreticileri teknikler hata bildirimi ve günlüğe kaydetme için keşfedin.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Hata sayfaları, bir kez daha](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Özel Durumlar için Tasarım Yönergeleri](https://msdn.microsoft.com/library/ms229014.aspx)
- [Kullanıcı dostu hata sayfaları](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [İşleme ve özel durumları atma](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Özel hata sayfaları ASP.NET kullanılarak uygun şekilde](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Önceki](strategies-for-database-development-and-deployment-cs.md)
> [sonraki](processing-unhandled-exceptions-cs.md)
