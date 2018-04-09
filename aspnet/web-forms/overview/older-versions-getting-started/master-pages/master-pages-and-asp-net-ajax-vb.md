---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: Ana sayfalar ve ASP.NET AJAX (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET AJAX ve ana sayfalar kullanmak için seçenekleri açıklar. ScriptManagerProxy sınıfı kullanarak arar; çeşitli JS dosyaları dependi nasıl yüklendiğini açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c7d8477d6d9d235749d88d0b657d60454298e53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="master-pages-and-aspnet-ajax-vb"></a>Ana sayfalar ve ASP.NET AJAX (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> ASP.NET AJAX ve ana sayfalar kullanmak için seçenekleri açıklar. ScriptManagerProxy sınıfı kullanarak arar; nasıl çeşitli JS dosyaları ScriptManager Master kullanılıp kullanılmadığını bağlı olarak veya içerik sayfasını yüklendiğini açıklar.


## <a name="introduction"></a>Giriş

Son birkaç yıl içinde daha da fazla geliştiriciler AJAX etkinleştirilmiş web uygulamaları oluşturmak. AJAX etkinleştirilmiş bir Web sitesi bir dizi ilgili web teknolojileri daha iyi yanıt bir kullanıcı deneyimi sunmak için kullanır. AJAX etkinleştirilmiş ASP.NET uygulamaları oluşturmak için Microsoft ASP.NET AJAX framework son derece kolay teşekkür olur. ASP.NET AJAX, ASP.NET 3.5 ve Visual Studio 2008 yerleşik; ASP.NET 2.0 uygulamalar için ayrı bir yükleme olarak da kullanılabilir.

Web sayfalarıyla AJAX etkinleştirilmiş ASP.NET AJAX framework oluştururken, tam olarak bir ScriptManager denetimi framework kullandığı her sayfa eklemeniz gerekir. Adından da anlaşılacağı gibi ScriptManager AJAX etkinleştirilmiş web sayfalarında kullanılan istemci tarafı komut dosyası yönetir. En azından bu oluşma şekli ASP.NET AJAX istemci kitaplığı JavaScript dosyalarını indirmek için tarayıcıyı yönlendirir HTML'yi ScriptManager gösterir. Ayrıca, özel JavaScript dosyaları, komut dosyası etkin web hizmetleri ve özel bir uygulama hizmeti işlevselliği kaydetmek için de kullanılabilir.

(Gerektiği gibi), site kullandığı ana sayfa varsa, mutlaka her içerik sayfada bir ScriptManager denetimi eklemek gerekmez; Bunun yerine, ana sayfaya bir ScriptManager denetimi ekleyebilirsiniz. Bu öğretici, ana sayfaya ScriptManager denetimi eklemek gösterilmiştir. Ayrıca özel komut dosyaları ve komut dosyası Hizmetleri belirli bir içerik sayfasındaki kaydetmek üzere ScriptManagerProxy denetimi kullanmak nasıl bakar.

> [!NOTE]
> Bu öğretici, tasarlarken veya ASP.NET AJAX framework ile AJAX etkinleştirilmiş web uygulamaları oluşturmak keşfedin değil. AJAX kullanma hakkında daha fazla bilgi için ASP.NET AJAX videolar ve öğreticiler yanı sıra, bu öğreticinin sonunda daha fazla bilgi bölümünde listelenen kaynaklarla başvurun.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>ScriptManager denetimi tarafından gösterilen biçimlendirme inceleniyor

ScriptManager denetimi o oluşma şekli ASP.NET AJAX istemci kitaplığı JavaScript Dosyaları indirmek için tarayıcıyı yönlendirir biçimlendirme yayar. Ayrıca satır içi JavaScript biraz bu kitaplığı başlatır sayfasına ekler. Aşağıdaki biçimlendirmede bir ScriptManager denetimi içeren bir sayfa işlenmiş çıkışına eklenen içerik gösterilir:


[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

`<script src="url"></script>` Etiketleri indirmek ve JavaScript dosyasını çalıştırmak için tarayıcı isteyin *url*. ScriptManager üç tür etiketleri yayar; başvuruda bulunan bir dosyanın `WebResource.axd`diğer iki dosya başvuru yaparken `ScriptResource.axd`. Bu dosyalar, Web sitenizin dosyaları olarak gerçekten yok. Bunun yerine, web sunucusunda bu dosyaları birini için bir istek geldiğinde, ASP.NET altyapısı querystring inceler ve uygun JavaScript içeriği döndürür. Bu üç dış JavaScript dosyaları tarafından sağlanan komut dosyası ASP.NET AJAX framework'ün istemci kitaplığı oluşturur. Diğer `<script>` ScriptManager tarafından gösterilen etiketleri bu kitaplığı başlatır satır içi betiği içerir.

Dış betik başvuruları ve satır içi betiği tarafından ScriptManager yayılan, ASP.NET AJAX çerçevesi kullanır, ancak framework kullanmayın sayfalar için gerekli değildir bir sayfa için gereklidir. Bu nedenle, yalnızca bir ScriptManager ASP.NET AJAX framework kullanan bu sayfalara eklemek idealdir neden. Ve bu yeterlidir, ancak framework kullanan çok sayıda sayfa varsa, tüm sayfalara - en az söylemek için yinelenen bir görev ScriptManager denetimi ekleme elde edersiniz. Alternatif olarak, daha sonra gerekli bu komut tüm içerik sayfalarına yerleştirir ana sayfaya bir ScriptManager ekleyebilirsiniz. Bu yaklaşımda, ASP.NET AJAX framework ana sayfa tarafından zaten bulunduğundan kullanan yeni bir sayfaya bir ScriptManager eklemeyi unutmayın gerekmez. Ana sayfaya bir ScriptManager ekleme aracılığıyla 1. adım yetenekte.

> [!NOTE]
> AJAX işlevselliği ana sayfanızın kullanıcı arabiriminden dahil planlıyorsanız, ilgili olarak hiçbir seçeneğiniz - ana sayfasında ScriptManager eklemeniz gerekir.


Ana sayfaya ScriptManager ekleme bir dezavantajı olduğundan yukarıdaki betik içinde yayınlanır *her* sayfasında, bağımsız olarak mı yoksa, gerekli. Bu açıkça yalnızca (ana sayfa) dahil ScriptManager sahip henüz herhangi bir özellik ASP.NET AJAX framework'ün kullanmayan sayfalar için harcanan bant genişliği neden olmaktadır. Ancak bant genişliği boşa harcanmış yalnızca ne olur?

- (Yukarıda gösterilen) ScriptManager tarafından gösterilen gerçek içeriği biraz üstüne 1 KB toplar.
- Tarafından başvurulan üç dış komut dosyalarını `<script>` öğesi, ancak, kabaca 450 KB sıkıştırılmamış veri oluşturan; gzip sıkıştırması kullanan bir Web sitesi bu toplam bant genişliği 100 KB azaltılabilir. Ancak, bu komut dosyaları bir yıl boyunca bunlar yalnızca bir defa indirilmesi gerektiğini anlamına gelir tarayıcı tarafından önbelleğe alınır ve daha sonra sitedeki diğer sayfalarda yeniden kullanılabilir.

Komut dosyaları önbelleğe alınır, en iyi durumda da, daha sonra toplam maliyeti 1 KB düşünülerek olduğu değildir. En kötü durumda, ancak - zaman komut dosyalarını henüz yüklenmedi ve web sunucusu olduğu herhangi bir biçimde sıkıştırma kullanmayan - bant genişliği isabet yaklaşık 450, ikinci bir veya iki bir dakika kadar geniş bant bağlantı üzerinden her yerden ekleyebilir, KB'dir  çevirmeli modem içindeki kullanıcı. İyi haber dış betik dosyalar tarayıcı tarafından önbelleğe alındığı için bu en kötü Durum senaryosu seyrek olduğunu gerçekleşir.

> [!NOTE]
> Hala ana sayfasında ScriptManager denetimi yerleştirme rahatsız düşünüyorsanız, Web formu göz önünde bulundurun ( `<form runat="server">` biçimlendirme ana sayfa içinde). Geri gönderme modeli kullanan her ASP.NET sayfası tam olarak bir Web Form içermelidir. Bir Web formu ekleme ek içerik ekler: bir gizli form alanları, bir dizi `<form>` kendisini etiketi ve gerekirse, JavaScript işlev komut dosyasından geri gönderimin başlatmaktan. Bu biçimlendirme geri gönderme yok sayfalar için gerekli değildir. Bu yabancı biçimlendirme ana sayfasından Web formu kaldırarak ve el ile bir gerektiren her içerik sayfasına ekleme ortadan. Ancak, bu gereksiz yere belirli içerik sayfalarına eklenen sahip gelen dezavantajları Web formu ana sayfasında sahip olmanın ağır basıyor.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>1. adım: bir ScriptManager denetimi için ana sayfa ekleme

ASP.NET AJAX framework kullanan her web sayfasını tam olarak bir ScriptManager denetimi içermelidir. Bu gereksinimden dolayı genellikle böylece tüm içerik sayfalarının otomatik olarak dahil ScriptManager denetimi ana sayfasında tek bir ScriptManager denetimi yerleştirmek için mantıklıdır. Ayrıca, ScriptManager UpdatePanel ve UpdateProgress denetimleri gibi ASP.NET AJAX sunucu denetimleri önce gelmelidir. Bu nedenle, Web Form içinde herhangi bir ContentPlaceHolder denetim önce ScriptManager koymak en iyisidir.

Açık `Site.master` ana sayfa ve bir ScriptManager denetimi Web formu sayfasında önce ekleyip `<div id="topContent">` öğesi (bkz: Şekil 1). Visual Web Developer 2008 veya Visual Studio 2008 kullanıyorsanız, ScriptManager denetimi araç AJAX uzantıları sekmesinde bulunur. Visual Studio 2005 kullanıyorsanız, ilk ASP.NET AJAX Framework'ü yüklemek ve denetimler için araç kutusu eklemek gerekir. ASP.NET 2.0 framework almak için ASP.NET AJAX indirme sayfasını ziyaret edin.

ScriptManager sayfasına eklendikten sonra değiştirme, `ID` gelen `ScriptManager1` için `MyManager`.


[![Ana sayfaya ScriptManager ekleyin](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**Şekil 01**: ScriptManager ana sayfasına ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>2. adım: bir içerik sayfasını ASP.NET AJAX çerçevesinden kullanma

Ana sayfaya eklenen ScriptManager denetimi ile biz artık ASP.NET AJAX framework işlevselliği herhangi bir içerik sayfasına ekleyebilirsiniz. Northwind veritabanı rastgele seçili üründen görüntüler yeni bir ASP.NET sayfası oluşturalım. Bu görüntü 15 yeni bir ürün gösteren dakikada, güncelleştirmek için ASP.NET AJAX framework'ün Zamanlayıcı denetim kullanacağız.

Başlangıç adlı kök dizininde yeni bir sayfa oluşturarak `ShowRandomProduct.aspx`. Bu yeni sayfa bağlamak unutmayın `Site.master` ana sayfa.


[![Web sitesine yeni bir ASP.NET sayfa ekleyin](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**Şekil 02**: yeni bir ASP.NET sayfa Web sitesine ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image6.png))


Belirtme sözcüğünün başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfa [SKM1] öğreticideki adlı bir özel ana sayfa sınıf oluşturduğumuz `BasePage` , oluşturulan sayfanın başlığı açıkça ayarlanmamış ise. Git `ShowRandomProduct.aspx` sayfanın arka plandaki kod sınıfı ve sahip öğesinden türetilen `BasePage` (yerine gelen `System.Web.UI.Page`).

Son olarak, güncelleştirme `Web.sitemap` dosya bu ders için bir giriş içerir. Altında aşağıdaki biçimlendirmeleri eklemek `<siteMapNode>` içerik sayfası etkileşim Ders ana için:


[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

Bu ek `<siteMapNode>` öğesi dersleri yansıtılır (bkz. Şekil 5) listesi.

### <a name="displaying-a-randomly-selected-product"></a>Rastgele seçilmiş ürün görüntüleme

Geri dönüp `ShowRandomProduct.aspx`. Tasarımcısı'ndan bir UpdatePanel denetimini araç sürükleyin `MainContent` içerik denetimi ve ayarlayın, `ID` özelliğine `ProductPanel`. UpdatePanel kısmi sayfa geri gönderimin zaman uyumsuz olarak güncelleştirilebilir ekranında bir bölgeyi temsil eder.

Bizim ilk UpdatePanel içinde rastgele seçilen ürün hakkındaki bilgileri görüntülemek için bir görevdir. UpdatePanel DetailsView denetimini sürükleyerek başlatın. DetailsView denetimin ayarlamak `ID` özelliğine `ProductInfo` ve temizleyin, `Height` ve `Width` özellikleri. DetailsView'un akıllı etiket genişletin ve DetailsView adlı yeni bir SqlDataSource denetimi bağlamak veri kaynağı Seç açılan listeden seçin `RandomProductDataSource`.


[![Yeni bir SqlDataSource denetimi DetailsView bağlama](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**Şekil 03**: yeni bir SqlDataSource denetimi DetailsView bağlamak ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image9.png))


Northwind veritabanına bağlanmak için SqlDataSource denetimini yapılandırmak `NorthwindConnectionString` (hangi içerik sayfasını [SKM2] öğreticiden ana sayfa ile etkileşim içinde oluşturduğumuz). Select deyimi yapılandırma seçtiğinizde özel bir SQL ifadesi belirtin ve ardından aşağıdaki sorguyu girin:


[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

`TOP 1` Anahtar sözcük `SELECT` yan tümcesi yalnızca sorgu tarafından döndürülen ilk kaydı döndürür. `NEWID()` İşlevi yeni bir genel benzersiz tanımlayıcı değeri (GUID) oluşturur ve kullanılabilir bir `ORDER BY` rastgele sırayla tablonun kayıtları döndürmek için yan tümcesi.


[![Bir tek, rasgele seçilen kaydı döndürülecek SqlDataSource yapılandırın](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**Şekil 04**: tek bir rastgele seçilen kayıt döndürülecek SqlDataSource yapılandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image12.png))


Sihirbazı tamamladıktan sonra Visual Studio Yukarıdaki sorgu tarafından döndürülen iki sütun için bir BoundField oluşturur. Bu noktada sayfanızın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

Şekil 5 gösterir `ShowRandomProduct.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Sayfayı yeniden yüklemek için tarayıcınızın Yenile düğmesini tıklatın; görmeniz gerekir `ProductName` ve `UnitPrice` yeni bir rastgele seçili kayıt için değer.


[![Rastgele bir ürün adı ve fiyat görüntülenir](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**Şekil 05**: A rastgele ürün adı ve fiyat görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Otomatik olarak yeni bir ürün görüntüleme 15 dakikada

ASP.NET AJAX framework belirli bir zamanda geri gönderimin gerçekleştiren bir zamanlayıcı denetimi içerir; üzerinde Zamanlayıcı 's geri gönderme `Tick` olayı oluşturulur. Zamanlayıcı denetimi içinde bir UpdatePanel girdiyseniz sırasında size yeni bir rastgele seçili ürün görüntülenecek DetailsView verileri rebind bir kısmi sayfa geri gönderme tetikler.

Bunu gerçekleştirmek için bir zamanlayıcı Araç Kutusu'ndan sürükleyin ve UpdatePanel bırakın. Zamanlayıcı 's değiştirme `ID` gelen `Timer1` için `ProductTimer` ve kendi `Interval` 60000 özelliğine 15000. `Interval` Özelliği Geri göndermeler arasındaki milisaniye olarak gösterir; 15000 için ayarlanması 15 dakikada bir kısmi sayfa geri gönderme tetiklemek Zamanlayıcı neden olur. Bu noktada, Zamanlayıcı'nın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

Olay işleyicisi için süreölçeri 's oluşturmak `Tick` olay. Bu olay işleyicisi DetailsView'un çağırarak DetailsView verileri rebind seçmeliyiz `DataBind` yöntemi. Bunun yapılması, veri kaynağı denetimi verileri yeniden almak için DetailsView bildirir, seçin ve yeni bir rastgele görüntülemek (gibi sayfa tarayıcınızın Yenile düğmesini tıklatarak yeniden yükleniyor olduğunda) kayıt seçili.


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

Tüm olan İşte bu kadar! Bir tarayıcı aracılığıyla sayfa yeniden ziyaret. Başlangıçta, rastgele bir ürünün bilgiler görüntülenir. Ekran patiently izleyebilir, 15 saniye sonra yeni bir ürün hakkında bilgi ASP mevcut görüntü yerini alır, fark edeceksiniz.

Burada neler olduğunu daha iyi görmek için bir etiket denetimi görünen son güncelleştirildiği zaman görüntüleyen UpdatePanel ekleyelim. Bir etiket Web denetimi UpdatePanel içinde ekleyin, ayarlamak kendi `ID` için `LastUpdateTime`ve temizleyin, `Text` özelliği. Ardından, olay işleyicisi UpdatePanel için 's oluşturmak `Load` olay ve görüntü etiketi geçerli saati. (UpdatePanel's `Load` olay her tam veya kısmi sayfa geri göndermede şu.)


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

Bu değişiklik tam, sayfa şu anda görüntülenen Ürün yüklendi saati içerir. Şekil 6 ilk sitesini ziyaret ettiğinizde sayfası gösterilir. Şekil 7 sayfa "Zamanlayıcı denetim ticked" ve yeni bir ürün hakkındaki bilgileri görüntülemek için UpdatePanel yenilendikten sonra 15 saniye sonra gösterir.


[![Rastgele seçilmiş ürün üzerinde sayfa yükleme görüntülenir](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**Şekil 06**: A rastgele seçilmiş ürün üzerinde sayfa yükleme görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image18.png))


[![Yeni bir rastgele seçilmiş ürün görüntülenen her 15 saniye](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**Şekil 07**: her 15 saniye yeni bir rastgele seçilmiş ürün görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>3. adım: ScriptManagerProxy denetimi kullanma

ASP.NET AJAX Framework istemci kitaplığı gerekli betik dahil olmak üzere birlikte ScriptManager özel JavaScript dosyaları, komut dosyası etkin Web Hizmetleri ve özel kimlik doğrulama, yetkilendirme ve profil Hizmetleri başvurular kaydedebilirsiniz. Genellikle bu özelleştirmeleri belirli bir sayfasına özgüdür. Özel komut dosyaları, Web hizmeti başvuruları veya kimlik doğrulama, yetkilendirme veya profili services ana sayfasında ScriptManager içinde başvuruluyorsa ancak ardından bunlar Web sitesindeki tüm sayfalar dahil edilmiştir.

Eklemek için ScriptManager ilgili özelleştirmeleri sayfa tarafından temelinde ScriptManagerProxy denetimi kullanın. Bir içerik sayfasının bir ScriptManagerProxy ekleyin ve özel JavaScript dosyası, Web hizmeti, başvuru veya kimlik doğrulama, yetkilendirme veya ScriptManagerProxy profili hizmetinden kaydedin; Bu, bu hizmetler için belirli içerik sayfasını kaydı etkisi yoktur.

> [!NOTE]
> Bir ASP.NET sayfası, yalnızca birden fazla ScriptManager denetimi mevcut olabilir. Bu nedenle, ana sayfasında ScriptManager denetimi zaten tanımlanmışsa, bir içerik sayfasının bir ScriptManager denetimi ekleyemezsiniz. Tek amacı, ScriptManagerProxy ana sayfasında ScriptManager tanımlamak, ancak bir sayfa tarafından temelinde ScriptManager özelleştirmeleri ekleme yeteneği çözümlenmedi için geliştiricilere yol sağlamaktır.


Eylem ScriptManagerProxy denetiminde görmek için şirketinizdeki UpdatePanel büyütmek `ShowRandomProduct.aspx` duraklatmak veya devam ettirmek Zamanlayıcı denetimi için istemci tarafı komut dosyası kullanan bir düğme eklenecek. Zamanlayıcı denetim Biz bu istenen işlevselliği elde etmek için kullanabileceğiniz üç istemci-tarafı yöntemi vardır:

- `_startTimer()` -Süreölçer denetim başlatır
- `_raiseTick()` -"değer," Zamanlayıcı denetimine böylece arka nakil ve sunucuda kendi onay olayı tetiklenmeden neden olur
- `_stopTimer()` -Süreölçer denetim durdurur

Bir JavaScript dosyası adlı bir değişkene oluşturalım `timerEnabled` ve adlı bir işlev `ToggleTimer`. `timerEnabled` Değişkeni Zamanlayıcı denetimi şu anda etkin devre dışı mı olduğunu gösterir; true olarak varsayılan olarak. `ToggleTimer` İşlevi, iki giriş parametreleri kabul eder: Duraklat/Sürdür düğmesi ve istemci tarafı başvuru `id` Zamanlayıcı denetiminin değeri. Bu işlev değerini değiştirir `timerEnabled`, Zamanlayıcı denetlemek için bir başvuru alır, başlatıldığında veya durdurulduğunda Zamanlayıcı (değerine bağlı olarak `timerEnabled`) ve "Duraklat" veya "Sürdür" düğmenin görüntüleme metni güncelleştirir. Duraklat/Resume düğmesine tıklandığında olduğunda bu işlev çağrılmaz.

Başlangıç adlı Web sitesini yeni bir klasör oluşturarak `Scripts`. Ardından, yeni bir dosya adındaki betikler klasörüne eklemek `TimerScript.js` türü JScript dosyası.


[![Yeni bir JavaScript dosyası betikler klasörüne ekleyin](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**Şekil 08**: yeni bir JavaScript dosyası ekleme `Scripts` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image24.png))


[![Yeni bir JavaScript dosyası Web sitesine eklendi](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**Şekil 09**: yeni JavaScript dosyası Web sitesine eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image27.png))


Ardından, aşağıdaki betik eklemek `TimerScript.js` dosyası:


[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

Şimdi bu özel JavaScript dosyası kaydetmek ihtiyacımız `ShowRandomProduct.aspx`. Geri dönüp `ShowRandomProduct.aspx` ve ScriptManagerProxy denetimi sayfasına ekleyin; ayarlamak kendi `ID` için `MyManagerProxy`. Özel bir JavaScript kaydetmek için dosya Tasarımcısı'nda ScriptManagerProxy denetimi seçin ve Özellikler penceresine gidin. Komut dosyaları özelliklerden birini başlıklı. Bu özellik seçme ScriptReference koleksiyon Düzenleyicisi'nin Şekil 10'da gösterilen görüntüler. Path özelliği içinde komut dosyasının yolunu girin ve yeni bir komut başvurusu dahil etmek için Ekle düğmesini tıklatın: `~/Scripts/TimerScript.js`.


[![Komut dosyası için bir başvuru ScriptManagerProxy denetim ekleyin](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**Şekil 10**: ScriptManagerProxy denetlemek için bir komut dosyası başvuru ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image30.png))


Komut dosyası başvurusunu ScriptManagerProxy denetim ekleme bildirim temelli sonra biçimlendirme içerecek şekilde güncelleştirilmiştir bir `<Scripts>` tek bir koleksiyon `ScriptReference` girişi, aşağıdaki kod parçacığında biçimlendirme gösterilmektedir:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

`ScriptReference` Giriş işlenmiş biçimlendirmede JavaScript dosyası için bir başvuru eklemek için ScriptManagerProxy bildirir. Diğer bir deyişle, ScriptManagerProxy özel kaydederek komut dosyası `ShowRandomProduct.aspx` sayfanın işlenmiş çıkış şimdi içeren başka bir `<script src="url"></script>` etiketi: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Biz şimdi çağırabilirsiniz `ToggleTimer` tanımlanan işlevi `TimerScript.js` istemci komut `ShowRandomProduct.aspx` sayfası. Aşağıdaki HTML UpdatePanel içinde ekleyin:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

Bu metin "Duraklat" düğmesiyle görüntüler. Zaman onu tıklandığında, JavaScript işlevinin `ToggleTimer` olarak adlandırılan, düğmesine başvurusu geçirme ve `id` Zamanlayıcı denetim değeri (`ProductTimer`). Alma sözdizimi Not `id` Zamanlayıcı denetiminin değeri. `<%=ProductTimer.ClientID%>` değerini yayar `ProductTimer` Zamanlayıcı denetimin `ClientID` özelliği. Denetim Kimliği adlandırma içerik sayfaları [SKM3] öğreticideki biz sunucu tarafı arasındaki farklar ele `ID` değeri ve sonuçta elde edilen istemci tarafı `id` değerini ve nasıl `ClientID` istemci-tarafı döndürür `id`.

Şekil 11 bir tarayıcı üzerinden ilk sitesini ziyaret ettiğinizde bu sayfada görüntülenir. Zamanlayıcı şu anda çalışıyor ve 15 dakikada görüntülenen ürün bilgilerini güncelleştirir. Şekil 12 Duraklat düğmesini tıklatıldıktan sonra ekran gösterir. Duraklat düğmesini tıklatarak Zamanlayıcı durdurur ve "Sürdür" düğmenin metni güncelleştirir. Ürün bilgilerini yenileyin (ve 15 dakikada yenilemeye devam etmek) Sürdür kullanıcı sonra.


[![Zamanlayıcı denetimi durdurmak için Duraklat düğmesini tıklatın](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**Şekil 11**: Zamanlayıcı denetimi durdurmak için Duraklat düğmesini tıklatın ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image33.png))


[![Zamanlayıcı yeniden başlatmak için devam düğmesine tıklayın](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**Şekil 12**: Zamanlayıcı yeniden başlatmak için devam düğmesine tıklayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-asp-net-ajax-vb/_static/image36.png))


## <a name="summary"></a>Özet

ASP.NET AJAX framework kullanarak AJAX etkinleştirilmiş web uygulamaları oluştururken her AJAX etkinleştirilmiş web sayfası bir ScriptManager denetimi içermesi zorunludur. Bu işlemi kolaylaştırmak için size bir ScriptManager her içerik bir ScriptManager eklemek anımsamak yerine ana sayfa ekleyebilirsiniz. Adım 1 adım 2'de içerik sayfasında AJAX işlevselliği uygulanmasına arama sırasında ana sayfa ScriptManager eklemek nasıl oluşturulacağını gösterir.

Özel komut dosyaları, komut dosyası etkin Web hizmetlerine başvurular eklemeniz veya özelleştirilmiş kimlik doğrulama, yetkilendirme veya belirli bir içerik sayfasının profili Hizmetleri ScriptManagerProxy denetimi içerik sayfasına ekleyin ve yapılandırın Özelleştirmeleri vardır. Adım 3 incelenmesi nasıl ScriptManagerProxy belirli bir içerik sayfasındaki özel bir JavaScript dosyası kaydetmek için kullanın.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET AJAX çerçevesi](../../../../ajax/index.md)
- [ASP.NET AJAX öğreticileri](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX videolar](../../../videos/aspnet-ajax/index.md)
- [Microsoft ASP.NET AJAX ile yapı etkileşimli kullanıcı arabirimi](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Rastgele sıralama kayıtlara NEWID kullanma](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Zamanlayıcı denetimi kullanma](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](interacting-with-the-content-page-from-the-master-page-vb.md)
> [sonraki](specifying-the-master-page-programmatically-vb.md)
