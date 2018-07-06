---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: Özel hata sayfası (VB) görüntüleme | Microsoft Docs
author: rick-anderson
description: Bir ASP.NET web uygulamasında bir çalışma zamanı hatası meydana geldiğinde kullanıcı gördükleri? Yanıt bağlıdır Web sitesinin &lt;customErrors&gt; yapılandırması...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 749b754903fb8e164b77edff4216fb2e2cf057b6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812931"
---
<a name="displaying-a-custom-error-page-vb"></a>Özel hata sayfası (VB) görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> Bir ASP.NET web uygulamasında bir çalışma zamanı hatası meydana geldiğinde kullanıcı gördükleri? Yanıt bağlıdır Web sitesinin &lt;customErrors&gt; yapılandırma. Varsayılan olarak, bir çalışma zamanı hatası oluştu proclaiming sayfanın bir sarı ekran kullanıcılara gösterilir. Bu öğreticide, sitenizin görünüme eşleşen görüntü aesthetically Hoş bir özel hata sayfası için bu ayarları özelleştirmek gösterilir.


## <a name="introduction"></a>Giriş

Mükemmel bir dünyada hiçbir çalışma zamanı hataları olacaktır. Programcıların bir hata koduyla n öğeli yazarsınız ve güçlü kullanıcı girdisi doğrulama ve dış veritabanı sunucuları ve e-posta sunucuları gibi kaynakları hiçbir zaman çevrimdışı. Elbette, gerçekte hatalar kaçınılmazdır. .NET Framework sınıfları, bir özel durum ile bir hata sinyali vermek. Örneğin, bir SqlConnection çağırma açık yöntemi nesnenin bir bağlantı dizesi tarafından belirtilen veritabanına bir bağlantı kurar. Ancak, veritabanı kapalı olduğunda ya da bağlantı dizesinde kimlik bilgileri geçersiz olduğunda ardından açık çağırılıyorsa yöntem bir `SqlException`. Özel durum kullanımı tarafından işlenebilir `Try/Catch/Finally` engeller. Kod içinde bir `Try` blok bir özel durum oluşturursa, Denetim, uygun bir catch bloğunun Geliştirici burada hatadan kurtarmayı deneyebilir aktarılır. Hiç eşleşen bir catch bloğu yok ya da özel durum oluşturdu kodu bir try bloğu içinde değilse, özel durum çağrı yığınına search, percolates `Try/Catch/Finally` engeller.

İşlenen olmadan, tüm ASP.NET çalışma zamanı en fazla özel durum Balonlar varsa [ `HttpApplication` sınıfı](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)'s [ `Error` olay](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) tetiklenir ve yapılandırılmış *hata sayfası*  görüntülenir. Varsayılan olarak, ASP.NET affectionately şeklinde adlandırılan bir hata sayfası görüntüler [Ölüm sarı bir ekran](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). YSOD iki sürümü vardır: bir uygulamada hata ayıklama geliştiricilere yararlı gösterir özel durum ayrıntıları, bir yığın izlemesi ve diğer bilgi (bkz **Şekil 1**); diğer yalnızca bir çalışma zamanı hatası (bkz: olduğunubildiren **Şekil 2**).

Özel durum ayrıntıları YSOD uygulamada hata ayıklama geliştiriciler için oldukça faydalıdır, ancak tacky ve okuyucunuz bir YSOD son kullanıcılara gösterme. Bunun yerine, son kullanıcıların sitenin görünüm durumu açıklayan daha kullanıcı dostu prose ile tutan bir hata sayfası için gerçekleştirilmelidir. Güzel bir haberimiz var böyle bir özel hata sayfası oluşturma oldukça kolay olmasıdır. Bu öğreticide ASP göz başlar. NET farklı hata sayfaları. Ardından, özel hata sayfası bir hata ile karşılaşıldığında kullanıcıları göstermek için web uygulamasının nasıl yapılandırılacağını gösterir.

### <a name="examining-the-three-types-of-error-pages"></a>Üç tür hata sayfaları İnceleme

Ne zaman bir ASP.NET uygulamasında hata sayfalarının üç türlerden biri işlenmeyen bir özel durum ortaya çıkar görüntülenir:

- Özel durum ayrıntıları sarı ekran, ölüm hata sayfası
- Çalışma zamanı hatası sarı ekran, ölüm hata sayfası veya
- Özel hata sayfası

Hata sayfası geliştiriciler ile özel durum ayrıntıları YSOD en tanıdık gelir. Varsayılan olarak, bu sayfada yerel olarak ziyaret eden kullanıcılara görüntülenir ve bu nedenle site geliştirme ortamında test edilirken bir hata meydana geldiğinde, gördüğünüz sayfasıdır. Adından da anlaşılacağı gibi özel durum ayrıntıları YSOD ayrıntılarını özel durum oluştu - türü, ileti ve yığın izlemesi sağlar. Daha özel durum, ASP.NET sayfa arka plan kod sınıfı içindeki kod tarafından tetiklendi ve uygulamayı hata ayıklama için yapılandırılmışsa, ardından özel durum ayrıntıları YSOD de bu satırı kodu (ve birkaç satır kod yukarıdaki ve altındaki) gösterilir.

**Şekil 1** özel durum ayrıntıları YSOD sayfada gösterilir. Tarayıcının adres penceresinde URL'yi not alın: `http://localhost:62275/Genre.aspx?ID=foo`. Bu geri çağırma `Genre.aspx` sayfası, belirli bir türe kitap incelemelerde listeler. Olmasını gerektirir `GenreId` değeri (bir `uniqueidentifier`) geçirilecek sorgu dizesi; Örneğin, kurgu incelemeleri görüntülemek için uygun URL'dir `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Olmayan bir varsa`uniqueidentifier` değeri geçirilir, sorgu dizesini (örneğin, "foo") aracılığıyla bir özel durum oluşturulur.

> [!NOTE]
> Demo web uygulamasında bu hatayı yeniden oluşturmaya indirilebilir ya da ziyaret yapabilecekleriniz `Genre.aspx?ID=foo` doğrudan ya da "Çalışma zamanı hatası oluştur" bağlantısını tıklatın `Default.aspx`.


İçinde sunulan özel durum bilgilerini Not **Şekil 1**. Özel durum iletisi "dönüştürme bir karakter dizesinden uniqueidentifier değerine dönüştürme başarısız oldu" sayfasının en üstünde bulunur. Özel durumun türünü `System.Data.SqlClient.SqlException`, de listelenir. Yığın izlemesi yok.

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**Şekil 1**: özel durum ayrıntıları YSOD özel durum hakkında bilgi içerir  
 ([Tam boyutlu görüntüyü görmek için tıklatın](displaying-a-custom-error-page-vb/_static/image3.png))

YSOD başka türde çalışma zamanı hatası YSOD olduğu ve gösterilen **Şekil 2**. Bir çalışma zamanı hatası oluştu ziyaretçi çalışma zamanı hatası YSOD bildirir, ancak oluşturulan özel durumla ilgili herhangi bir bilgi içermez. (Bu ancak değiştirerek hata ayrıntılarını görünür yapmak nasıl yönergelerinizi `Web.config` okuyucunuz Ara YSOD kılan bir parçası olan dosyası.)

Varsayılan olarak, çalışma zamanı hatası YSOD uzaktan ziyaret eden kullanıcılara gösterilir (aracılığıyla http://www.yoursite.com)tarayıcının adres çubuğuna URL'yi tarafından yi gibi **Şekil 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Hata ayrıntılarını bilmekle isteyen geliştiriciler, ancak olası güvenlik açıklarını ve diğer hassas bilgiler herkes tarafından ziyaret gösterebilir gibi tür bilgilerin canlı sitede gösterilmeyecek iki farklı YSOD ekran var, Site.

> [!NOTE]
> Aşağıdaki ve DiscountASP.NET uygulamanızın web ana bilgisayarı kullanıyorsanız, çalışma zamanı hatası YSOD Canlı siteyi ziyaret görüntülemez fark edebilirsiniz. Bu durum, özel durum ayrıntıları YSOD göstermek için varsayılan olarak yapılandırılmış sunucularından DiscountASP.NET sahip olmasıdır. Ekleyerek bu varsayılan davranışı geçersiz kılabilirsiniz güzel bir haberimiz var olan bir `<customErrors>` bölümünü, `Web.config` dosya. "Yapılandırma hatası sayfasında görüntülenen" bölümünde inceler `<customErrors>` ayrıntısı bölümünde.


[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**Şekil 2**: çalışma zamanı hatası YSOD herhangi bir hata ayrıntılarını içermez  
 ([Tam boyutlu görüntüyü görmek için tıklatın](displaying-a-custom-error-page-vb/_static/image6.png))

Üçüncü hata sayfası bir web sayfası özel hata sayfası türüdür. Özel hata sayfası avantajı, sayfanın görünüm yanı sıra kullanıcıya görüntülenen bilgileri üzerinde tam denetime sahip olur; özel hata sayfası, aynı ana sayfa ve stiller diğer sayfaları olarak kullanabilirsiniz. İşlenmeyen bir özel durum durumunda görüntülemek için yapılandırma ve özel hata sayfası oluşturma "Özel hata sayfasını kullanarak" bölümünde açıklanmaktadır. **Şekil 3** kısaca bir tepe bu özel hata sayfası sunar. Gördüğünüz gibi çok daha fazla profesyonel görünümlü Ölüm sarı ekranlar, rakamları 1 ve 2'de gösterilen ya da daha görünümünü hata sayfasının olur.

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**Şekil 3**: özel hata sayfası daha özel olarak uyarlanmış bir görünüm sunar.  
 ([Tam boyutlu görüntüyü görmek için tıklatın](displaying-a-custom-error-page-vb/_static/image9.png))

Tarayıcının adres çubuğunda incelemek için birkaç dakikanızı **Şekil 3**. Adres çubuğuna özel hata sayfasının URL'sini gösterdiğine dikkat edin (`/ErrorPages/Oops.aspx`). Şekil 1 ve 2'de, sarı ekranlar ölüm hata kaynaklandığı sayfanın gösterilir (`Genre.aspx`). Özel hata sayfası, hatanın oluştuğu aracılığıyla sayfasının URL'sini geçirilir `aspxerrorpath` sorgu dizesi parametresi.

## <a name="configuring-which-error-page-is-displayed"></a>Yapılandırma, hata sayfası görüntülenir

Üç olası hata sayfalarında görüntülenen iki değişkenlerde dayanır:

- Yapılandırma bilgileri `<customErrors>` bölümünde ve
- Olup kullanıcı yerel olarak veya uzaktan sitesinden olduğu.

[ `<customErrors>` Bölümü](https://msdn.microsoft.com/library/h0hfz6fc.aspx) içinde `Web.config` hangi hata sayfası gösterilir etkileyen iki öznitelikleri: `defaultRedirect` ve `mode`. `defaultRedirect` İsteğe bağlı öznitelik. Sağlanırsa, özel hata sayfasının URL'sini belirtir ve özel hata sayfası yerine çalışma zamanı hatası YSOD gösterileceğini belirtir. `mode` Özniteliği gereklidir ve üç değerden birini kabul eder: `On`, `Off`, veya `RemoteOnly`. Bu değerler aşağıdaki davranışa sahip:

- `On` -özel hata sayfası veya çalışma zamanı hatası YSOD yerel veya uzak olup olmadıkları bağımsız olarak tüm kullanıcılarına, gösterildiğini belirtir.
- `Off` -Yerel veya uzak olup olmadıkları bağımsız olarak tüm kullanıcılarına, özel durum ayrıntıları YSOD görüntüleneceğini belirtir.
- `RemoteOnly` -özel durum ayrıntıları YSOD yerel yayımlanacağını ve Ziyaretçiler gösterilmese özel hata sayfası veya çalışma zamanı hatası YSOD uzak ziyaretçileri gösterileceğini belirtir.

Aksi belirtilmedikçe, ASP.NET mod özniteliği kümesine yokmuş gibi davranan `RemoteOnly` ve belirtilmemiş bir `defaultRedirect` değeri. Diğer bir deyişle, varsayılan davranış, çalışma zamanı hatası YSOD uzak yayımlanacağını ve Ziyaretçiler gösterilmese özel durum ayrıntıları YSOD yerel ziyaretçilerine görüntülenir şeklindedir. Ekleyerek bu varsayılan davranışı geçersiz kılabilirsiniz bir `<customErrors>` , web uygulamanızın bölümüne `Web.config file.`

## <a name="using-a-custom-error-page"></a>Özel hata sayfasını kullanarak

Her web uygulaması, özel hata sayfası olması gerekir. Çalışma zamanı hatası YSOD daha profesyonel görünümlü bir alternatif sağlayan, oluşturmak kolaydır ve özel hata sayfası kullanmak için uygulamayı yapılandırma yalnızca birkaç dakika sürer. İlk adım, özel hata sayfası oluşturuyor. Yeni bir klasör adlı kitabı incelemeleri uygulamaya eklediğiniz `ErrorPages` ve adlı yeni bir ASP.NET sayfasına eklenen `Oops.aspx`. Otomatik olarak aynı görünüme devralması sitenizdeki sayfaları geri kalanı gibi aynı ana sayfayı kullanmak sayfa vardır.

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**Şekil 4**: özel hata sayfası oluşturma

Ardından, hata sayfası için içerik oluşturma birkaç dakikanızı ayırın. Bunun yerine basit özel hata sayfası belirten bir ileti beklenmeyen bir hata oluştu ve bağlantı sitenin giriş sayfasına geri ile oluşturmuş oldunuz.

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**Şekil 5**: tasarım, özel hata sayfası  
 ([Tam boyutlu görüntüyü görmek için tıklatın](displaying-a-custom-error-page-vb/_static/image14.png))

Tamamlandı, hata sayfası ile web uygulaması çalışma zamanı hatası YSOD yerine özel hata sayfasını kullanacak şekilde yapılandırın. Bu hata sayfasının URL'sini belirterek gerçekleştirilir `<customErrors>` bölümün `defaultRedirect` özniteliği. Aşağıdaki işaretlemeyi ekleyin, uygulamanızın `Web.config` dosyası:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

Yukarıdaki biçimlendirme yerel olarak uzaktan ziyaret söz konusu kullanıcılar için özel hata sayfası Oops.aspx kullanırken ziyaret eden kullanıcılara özel durum ayrıntıları YSOD göstermek üzere uygulamayı yapılandırır. Bu uygulamada görmek için Web sitenizi üretim ortamına dağıtın ve canlı sitede geçersiz querystring değerine sahip, ardından Genre.aspx sayfasını ziyaret edin. Özel hata sayfasını görmeniz gerekir (kiracıurl **Şekil 3**).

Özel hata sayfası yalnızca uzak kullanıcılara gösterilen doğrulamak için ziyaret `Genre.aspx` geliştirme ortamından geçersiz bir sorgu dizesi içeren sayfa. Özel durum ayrıntıları YSOD hala görmeniz gerekir (kiracıurl **Şekil 1**). `RemoteOnly` Ayar, özel durumun ayrıntılarını görmek yerel olarak çalışan geliştiriciler devam ederken siteyi ziyaret ettiği üretim ortamına özel hata sayfası gördüğünüzü sağlar.

## <a name="notifying-developers-and-logging-error-details"></a>Geliştiriciler bildiren ve hata ayrıntılarını günlüğe kaydetme

Geliştirme ortamında gerçekleşen hataları kendi bilgisayar başında oturan Geliştirici kaynaklanan. İçinde özel durum ayrıntıları YSOD Filiz özel durumun bilgiler gösterilir ve kendisi bir hata oluştuğunda Filiz gerçekleştirmekte olduğu hangi adımların bilir. Ancak üretimde hata oluştuğunda, geliştirici sitesinden son kullanıcı hatası bildirmeye zaman sürece bir hata oluştu bilgisi sahiptir. Ve kullanıcı bir hata oluştu, özel durum türü, ileti ve yığın izlemesi sürelerde hatanın nedenini tanılayın, düzeltin etmek zor olabilir bilmeden geliştirme ekibi uyar şekilde kendi dışında kalsa bile.

Bu nedenlerle, üretim ortamında herhangi bir hata bazı kalıcı depoya (örneğin, bir veritabanı) ve bu oturum üst düzey öneme geliştiriciler bu hata verilir. Özel hata sayfası, bu günlüğe kaydetme ve bildirim yapmak için iyi bir yer gibi görünebilir. Ne yazık ki, özel hata sayfası için hata ayrıntılarını erişimi yok ve bu bilgileri günlüğe kaydetmek için kullanılamaz. Güzel bir haberimiz var, çeşitli yollarla hata ayrıntılarını izlemesine ve bunları oturum vardır ve bu konuda daha fazla ayrıntı sonraki üç öğreticileri keşfedin ' dir.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Farklı bir özel hata sayfaları için farklı HTTP hata durumları kullanma

Bir özel durum ASP.NET sayfası tarafından oluşturulur ve işlenmezse, yapılandırılmış bir hata sayfası görüntüleyen ASP.NET çalışma zamanı kadar özel durum percolates. Bir isteği ASP.NET motoruna gelir ancak herhangi bir nedenden dolayı işlenemiyor - belki istenen dosya bulunamadı veya okunduğunda değil, izinler için dosyayı - devre dışı sonra ASP.NET altyapısı başlatır bir `HttpException`. Bu özel durumun ortaya ASP.NET sayfaları, özel durumlar gibi uygun hata sayfasında görüntülenecek neden çalışma zamanının en fazla Balonlar.

Bir kullanıcı özel hata sayfası görürsünüz, bulunmayan bir sayfa istediğinde üretim web uygulaması için bunun anlamı olmasıdır. **Şekil 6** bu tür bir örnek gösterilmektedir. İstek için bir mevcut olmayan sayfası olduğu için (`NoSuchPage.aspx`), bir `HttpException` oluşturulur ve özel hata sayfası görüntülenir (başvuru Not `NoSuchPage.aspx` içinde `aspxerrorpath` querystring parametresi).

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png)**Şekil 6**: ASP.NET çalışma zamanı, geçersiz bir isteğine yanıt olarak yapılandırılmış bir hata sayfası görüntüler.  
 ([Tam boyutlu görüntüyü görmek için tıklatın](displaying-a-custom-error-page-vb/_static/image17.png)) 

Varsayılan olarak, tüm hata türleri görüntülenecek aynı özel hata sayfası neden. Bununla birlikte, belirli HTTP durum kodu kullanarak farklı özel hata sayfası belirtebilirsiniz `<error>` alt öğelerin hizalamasını `<customErrors>` bölümü. Örneğin, bir sayfa bulunamadı sahip bir HTTP durum kodu 404 hatası olması durumunda görüntülenen bir farklı hata sayfası için güncelleştirme `<customErrors>` bölümü aşağıdaki biçimlendirme eklemek için:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

Uzaktan ziyaret kullanıcı mevcut değil, bir ASP.NET kaynak istediğinde yerinde bu değişiklik, bunlar yönlendirilecek `404.aspx` özel hata sayfası yerine `Oops.aspx`. Olarak **Şekil 7** gösterir, `404.aspx` sayfasına, genel özel hata sayfası daha ayrıntılı bir ileti içerebilir.

> [!NOTE]
> Kullanıma [404 hata sayfaları, bir fazla kez](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) etkili 404 hatası sayfaları oluşturma konusunda yönergeler için.


[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png)**Şekil 7**: Özel 404 hata sayfası değerinden daha hedefe bir ileti görüntüler. `Oops.aspx`  
 ([Tam boyutlu görüntüyü görmek için tıklatın](displaying-a-custom-error-page-vb/_static/image20.png)) 

Bildiğiniz `404.aspx` sayfası yalnızca ulaşıldığında kullanıcı bulunamadı bir sayfa için bir istekte bulunduğunda, bu belirli tür adres kullanıcının yardımcı olmak için işlevselliği eklemek için bu özel hata sayfası geliştirebilirsiniz. Örneğin, iyi URL'leri hatalı URL'ler bilinen eşleyen bir veritabanı tablosu oluşturun ve sonra `404.aspx` , tablo; bu sayfa kullanıcı çalışıyor olabilirsiniz ulaşmak için özel hata sayfası karşı bir sorgu çalıştırın.

> [!NOTE]
> Özel hata sayfası, yalnızca ASP.NET altyapısı tarafından işlenen bir kaynağa bir istekte bulunulduğunda görüntülenir. Açıkladığımız gibi [arasındaki temel farklar IIS ve ASP.NET Geliştirme Sunucusu](core-differences-between-iis-and-the-asp-net-development-server-vb.md) Öğreticisi, web sunucusu belirli isteklerini işlemek kendisi. Varsayılan olarak, IIS, ASP.NET altyapısı çağırmadan sunucu işlemleri istekleri görüntüleri ve HTML dosyaları gibi statik içerik web. Kullanıcı mevcut olmayan görüntü dosyası isterse, sonuç olarak, geri ASP yerine IIS varsayılan 404 hata iletisi alırlar. Hata sayfası NET'in yapılandırılmış.


## <a name="summary"></a>Özet

Bir ASP.NET uygulamasında işlenmeyen bir özel durum ortaya çıktığında, kullanıcı üç hata sayfalardan biri görüntülenir: özel durum ayrıntıları sarı ekran, ölüm; çalışma zamanı hatası; ölüm ekran sarı veya bir özel hata sayfası. Uygulamanın üzerinde hangi hata sayfası görüntülenir bağlıdır `<customErrors>` yapılandırması ve yerel olarak veya uzaktan kullanıcı ziyaret olup olmadığı. Özel durum ayrıntıları YSOD yerel ziyaretçileri ve çalışma zamanı hatası YSOD uzak yayımlanacağını ve Ziyaretçiler göstermek için varsayılan davranıştır.

Çalışma zamanı hatası YSOD hassas olabilecek hata bilgilerini sitesinden kullanıcıdan gizliyor ancak sitenizin görünüme ayırır ve buggy Ara uygulamanızı sağlar. Daha iyi bir yaklaşım kapsar oluşturma ve özel hata sayfası tasarlama ve kendi URL belirterek özel hata sayfası kullanmaktır `<customErrors>` bölümün `defaultRedirect` özniteliği. Birden çok özel hata sayfaları farklı HTTP hata durumları için bile olabilir.

Özel hata sayfası işleme stratejisi üretim ortamında bir Web sitesi için kapsamlı bir hata ilk adımdır. Geliştiricisi hata, uyarı ve ayrıntılarını günlüğe kaydetme Ayrıca önemli adımlardır. Teknikleri hata bildirimi ve günlüğe kaydetme için sonraki üç öğreticileri keşfedin.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Hata sayfaları, bir kez daha](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Özel Durumlar için Tasarım Yönergeleri](https://msdn.microsoft.com/library/ms229014.aspx)
- [Kullanımı kolay hata sayfaları](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [İşleme ve özel durumları atma](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Özel hata sayfaları ASP.NET'te düzgün bir şekilde kullanma](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Önceki](strategies-for-database-development-and-deployment-vb.md)
> [İleri](processing-unhandled-exceptions-vb.md)
