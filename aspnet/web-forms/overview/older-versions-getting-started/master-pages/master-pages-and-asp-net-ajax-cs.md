---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Ana sayfalar ve ASP.NET AJAX (C#) | Microsoft Docs
author: rick-anderson
description: ASP.NET AJAX ve ana sayfalar kullanmaya yönelik seçenekleriniz ele alınmaktadır. ScriptManagerProxy sınıfı kullanarak arar; çeşitli JS dosyaları dependi nasıl yüklendiğini açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fceed046ead6c8129d55aa4e4272cfef0ebe184
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390914"
---
<a name="master-pages-and-aspnet-ajax-c"></a>Ana sayfalar ve ASP.NET AJAX (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> ASP.NET AJAX ve ana sayfalar kullanmaya yönelik seçenekleriniz ele alınmaktadır. ScriptManagerProxy sınıfı kullanarak arar; nasıl çeşitli JS dosyaları ScriptManager Master kullanılıp kullanılmadığını bağlı olarak veya içerik sayfası yüklendiğini açıklar.


## <a name="introduction"></a>Giriş

Son birkaç yıl içinde giderek daha fazla geliştiriciler oluşturma [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-içerebilen web uygulamalarında. Bir AJAX içerebilen Web sitesi daha duyarlı bir kullanıcı deneyimi sunmak için bir dizi ilgili web teknolojilerini kullanır. AJAX etkinleştirilmiş ASP.NET uygulamaları oluşturmaya olan Microsoft teşekkür şaşırtıcı derecede kolaydır [ASP.NET AJAX framework](../../../../ajax/index.md). ASP.NET AJAX ASP.NET 3.5 ve Visual Studio 2008 içinde yerleşik olarak bulunur; Bu, aynı zamanda ASP.NET 2.0 uygulamaları için ayrı bir indirme olarak da kullanılabilir.

AJAX içerebilen web sayfaları ASP.NET AJAX framework ile oluştururken, tam olarak bir eklemelisiniz [ScriptManager denetimi](https://msdn.microsoft.com/library/bb398863.aspx) framework kullandığı her sayfa için. Adından da anlaşılacağı gibi ScriptManager AJAX etkinleştirilmiş web sayfalarında kullanılan istemci tarafı komut dosyası yönetir. En azından, tarayıcı, ASP.NET AJAX istemci kütüphanesi düzenini JavaScript dosyalarını indirme talimatı verir HTML'yi ScriptManager gösterir. Ayrıca, özel JavaScript dosyaları, komut dosyası etkin web hizmetleri ve özel uygulama hizmeti işlevselliği kaydetmek için de kullanılabilir.

(Olması gerektiği gibi), site kullandığı ana sayfa mutlaka bir ScriptManager denetimi her tek içerik sayfasına eklemek gerekirse değil; Bunun yerine, ana sayfaya bir ScriptManager denetimi ekleyebilirsiniz. Bu öğreticide, ScriptManager denetimini ana sayfasına eklemek gösterilir. Ayrıca özel komut dosyaları ve komut dosyası Hizmetleri belirli bir içerik sayfasındaki kaydedilecek ScriptManagerProxy denetimi kullanmayı bakar.

> [!NOTE]
> Bu öğreticide, tasarlama ya da ASP.NET AJAX framework ile AJAX etkinleştirilmiş web uygulamaları keşfedin değil. AJAX kullanarak hakkında daha fazla bilgi için danışın [ASP.NET AJAX videoları](../../../videos/aspnet-ajax/index.md) ve [öğreticiler](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)yanı Bu öğreticinin sonunda daha fazla bilgi bölümünde listelenen bu kaynakları olarak.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>ScriptManager denetimi tarafından yayılan biçimlendirme İnceleme

ScriptManager denetimini JavaScript dosyalarını indirmek için tarayıcı, ASP.NET AJAX istemci kütüphanesi düzenini yönlendiren biçimlendirme yayar. Ayrıca bir bit satır içi JavaScript bu kitaplığı başlatır sayfasına ekler. Bir ScriptManager denetimi içeren bir sayfa işlenen çıkışı için eklenen içeriği aşağıdaki biçimlendirme gösterilmektedir:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

`<script src="url"></script>` Etiketleri indirmek ve JavaScript dosyasını çalıştırmak için tarayıcı isteyin *url*. ScriptManager üç etiket yayar; bir dosya başvuruları `WebResource.axd`, buna karşın diğer iki dosya başvurusu `ScriptResource.axd`. Bu dosyalar, Web sitenizi dosyaları olarak gerçekten yok. Bunun yerine, web sunucusunda bu dosyaların herhangi birine yönelik bir istek geldiğinde, ASP.NET altyapısı sorgu dizesini inceler ve uygun JavaScript içeriği döndürür. Bu üç dış JavaScript dosyaları tarafından sağlanan betik, ASP.NET AJAX framework'ün istemci kitaplığı oluşturur. Diğer `<script>` ScriptManager tarafından yayılan etiketleri bu kitaplığı başlatır satır içi betik içerir.

Dış komut dosyası başvuruları ve satır içi betiği şu ScriptManager tarafından yayılan ASP.NET AJAX framework kullanan, ancak çerçevesi kullanmayan sayfaları için gerekli değildir bir sayfa için gereklidir. Bu nedenle, yalnızca ASP.NET AJAX framework kullanan bu sayfalara bir ScriptManager eklemek idealdir neden. Ve bu yeterlidir ancak framework kullanan birçok sayfaları varsa tüm sayfalara - en az söylemek için yinelenen bir görev ScriptManager denetimini ekleme elde edersiniz. Alternatif olarak, ardından bu gerekli betik tüm içerik sayfalarına eklediği ana sayfaya bir ScriptManager ekleyebilirsiniz. Bu yaklaşımda, ana sayfa tarafından zaten bulunduğundan, ASP.NET AJAX framework kullanan bir sayfada bir ScriptManager eklemek hatırlamanız gerekmez. Bir ScriptManager ana sayfasına ekleyerek aracılığıyla Yürüyüşü 1. adım.

> [!NOTE]
> Kullanıcı arabiriminde, ana sayfanın AJAX işlevselliği dahil olmak üzere planlıyorsanız, ilgili olarak hiçbir seçeneğiniz - ana sayfada bir ScriptManager eklemeniz gerekir.


Ana sayfaya ScriptManager ekleme bulunacağından olan yukarıdaki betik yayıldığını *her* sayfasında, bağımsız olarak, gerekli. Bu açıkça (ana sayfası) dahil ScriptManager sahip henüz ASP.NET AJAX framework'ün tüm özellikleri yalnızca sayfalar için harcanan bant genişliği neden olur. Ancak, ne kadar bant genişliği boşa harcanmış olur?

- ScriptManager (yukarıda gösterilmiştir) tarafından yayılan gerçek içeriği, 1 KB'lık biraz üstüne toplar.
- Üç dış komut dosyaları tarafından başvurulan `<script>` öğesi, ancak kabaca 450 KB'lık sıkıştırılmamış verinin oluşturan; gzip sıkıştırmasını kullanan bir Web sitesine, bu toplam bant genişliği 100 KB azaltılabilir. Ancak, bu komut dosyaları, bir yıl boyunca bunlar yalnızca bir kez indirilmesi gerektiğini anlamına gelir tarayıcı tarafından önbelleğe alınır ve sonra sitedeki diğer sayfalarda yeniden kullanılabilir.

Betik dosyaları önbelleğe alınır, en iyi durumda, daha sonra toplam 1 KB göz ardı edilebilir olduğu maliyetidir. En kötü durumda, ancak - ne zaman komut dosyalarını henüz yüklenmedi ve web sunucusu olduğu herhangi bir biçimde sıkıştırma kullanmıyor - bant genişliği isabet yaklaşık 450 ikinci bir veya iki için bir dakika kadar geniş bant bağlantı üzerinden her yerde ekleyebileceğiniz KB ' tır  çevirmeli modem üzerinden kullanıcılar. Dış komut dosyalarını tarayıcı tarafından önbelleğe alındığından, bu en kötü Durum senaryosu seyrek oluştuğunu güzel bir haberimiz var olur.

> [!NOTE]
> Yine de ana sayfada bir ScriptManager denetimi yerleştirme rahatsız düşünüyorsanız, Web formu göz önünde bulundurun ( `<form runat="server">` ana sayfaya biçimlendirmede). Geri gönderme modeli kullanan her ASP.NET sayfası, tam olarak bir Web formu içermesi gerekir. Web formu ekleme, ek içeriği ekler: bir gizli form alanları, bir dizi `<form>` kendisini etiketleyin ve gerekirse, bir JavaScript işlevi bir geri gönderme betikten başlatmak için. Bu işaretleme, geri gönderme yok sayfaları için gerekli değildir. Ana sayfadan Web formu kaldırarak ve bunu gerektiren her içerik sayfası için el ile ekleyerek bu fazlalık biçimlendirme ortadan. Ancak, ana sayfada Web formu sahip avantajları, gereksiz yere belirli içerik sayfalarına eklenmiş olan gelen dezavantajların üstünde.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>1. adım: bir ScriptManager denetimi için ana sayfa ekleme

ASP.NET AJAX framework kullanan her web sayfasını, tam olarak bir ScriptManager denetimi içermesi gerekir. Bu gereksinimden dolayı genellikle böylece tüm içerik sayfalarının otomatik olarak dahil ScriptManager denetimini ana sayfasında tek bir ScriptManager denetimi yerleştirmek için mantıklıdır. Ayrıca, ScriptManager UpdatePanel ve UpdateProgress denetimleri gibi ASP.NET AJAX sunucu denetimleri önce gelmelidir. Bu nedenle, Web Form içinde herhangi bir ContentPlaceHolder denetim önce ScriptManager koymak idealdir.

Açık `Site.master` ana sayfa ve bir ScriptManager denetimi Web formunda sayfasına önce ekleyin `<div id="topContent">` öğesi (bkz. Şekil 1). Visual Web Developer 2008 veya Visual Studio 2008 kullanıyorsanız, ScriptManager denetimini araç AJAX uzantılar sekmesinde bulunur. Visual Studio 2005 kullanıyorsanız, ilk ASP.NET AJAX Framework'ü yüklemek ve denetimler araç kutusuna ekleme gerekecektir. Ziyaret [ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) ASP.NET 2.0 framework alınamıyor.

ScriptManager sayfaya ekledikten sonra değiştirme, `ID` gelen `ScriptManager1` için `MyManager`.


[![ScriptManager ana sayfaya ekleyin.](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Şekil 01**: ScriptManager ana sayfaya ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>2. adım: ASP.NET AJAX Framework içerik sayfasından kullanma

Ana sayfaya eklenen ScriptManager denetimi ile artık ASP.NET AJAX framework işlevselliği için herhangi bir içerik sayfası ekleyebiliriz. Northwind veritabanındaki rastgele seçilmiş bir ürün görüntüleyen yeni bir ASP.NET sayfası oluşturalım. Bu görüntü 15 gösteren yeni bir ürün saniyede, güncelleştirilecek ASP.NET AJAX framework'ün Zamanlayıcı denetimi kullanacağız.

Adlı kök dizininde yeni bir sayfa oluşturarak başlayın `ShowRandomProduct.aspx`. Bu yeni sayfa için bağlanacağını unutmayın `Site.master` ana sayfa.


[![Web sitesine yeni bir ASP.NET sayfası Ekle](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Şekil 02**: Web sitesine yeni bir ASP.NET sayfası ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Geri çağırma [ *ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) adlı bir özel taban sayfası sınıfı oluşturduk öğretici `BasePage` , oluşturulan başlığı yazılmışsa açıkça ayarlayın. Git `ShowRandomProduct.aspx` sayfa arka plan kod sınıfı ve varsa, türetilen `BasePage` (yerine gelen `System.Web.UI.Page`).

Son olarak, güncelleştirme `Web.sitemap` bu ders için bir giriş eklemek için dosya. Altında aşağıdaki işaretlemeyi ekleyin `<siteMapNode>` asıl içerik sayfası etkileşim ders için:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

Bu ek `<siteMapNode>` öğesi derslerde yansıtılır (bkz: Şekil 5) listesi.

### <a name="displaying-a-randomly-selected-product"></a>Bir rastgele seçilmiş ürün görüntüleme

Geri dönüp `ShowRandomProduct.aspx`. Araç kutusundan bir UpdatePanel denetimine Designer'dan sürükleyin `MainContent` içerik denetimi ve ayarlayın, `ID` özelliğini `ProductPanel`. UpdatePanel ile kısmi sayfa geri gönderme zaman uyumsuz olarak güncelleştirilebilir ekran üzerindeki bir bölgeyi temsil eder.

Bizim ilk UpdatePanel içinde rastgele seçilmiş bir ürün hakkındaki bilgileri görüntülemek için bir görevdir. UpdatePanel ile bir DetailsView denetimi sürükleyerek başlatın. DetailsView denetimin ayarlamak `ID` özelliğini `ProductInfo` ve temizleyin, `Height` ve `Width` özellikleri. DetailsView'ın akıllı etiket genişletin ve DetailsView adlı yeni bir SqlDataSource denetimi bağlamak veri kaynağı Seç açılan listeden seçin `RandomProductDataSource`.


[![DetailsView yeni SqlDataSource denetime bağlama](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Şekil 03**: yeni bir SqlDataSource denetimi DetailsView bağlamak ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image9.png))


SqlDataSource denetimi ile Northwind veritabanına bağlanmak için yapılandırma `NorthwindConnectionString` (biz de oluşturulan [ *içerik sayfasından ana sayfa ile etkileşim* ](interacting-with-the-content-page-from-the-master-page-cs.md) öğretici). Select deyimi yapılandırma seçtiğinizde özel bir SQL deyimi belirtin ve ardından aşağıdaki sorguyu girin:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

`TOP 1` Anahtar sözcüğünü `SELECT` yan tümcesi yalnızca sorgu tarafından döndürülen ilk kaydı döndürür. [ `NEWID()` İşlevi](https://msdn.microsoft.com/library/ms190348.aspx) yeni bir dizi oluşturur [genel benzersiz tanımlayıcı değeri (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) ve kullanılabilir bir `ORDER BY` tablonun kayıtlarını rastgele sırayla döndürülecek yan tümcesi.


[![SqlDataSource rastgele seçilen, tek bir kaydı döndürmek için yapılandırma](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Şekil 04**: tek bir, rastgele seçilen kayıt döndürülecek SqlDataSource yapılandırın ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Sihirbazı tamamladıktan sonra Visual Studio bir BoundField Yukarıdaki sorgu tarafından döndürülen iki sütun oluşturur. Bu noktada bildirim temelli işaretleme, sayfanın aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

Şekil 5 gösterir `ShowRandomProduct.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Sayfayı yeniden yüklemek için tarayıcınızın yenile düğmesine tıklayın; görmelisiniz `ProductName` ve `UnitPrice` yeni bir rastgele seçilen kaydı için değerler.


[![Rastgele bir ürün adı ve fiyat görüntülenir](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Şekil 05**: A rastgele ürün adı ve fiyat görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Otomatik olarak yeni bir ürün görüntüleme her 15 saniyede

ASP.NET AJAX framework belirli bir zamanda geri gönderme gerçekleştiren bir zamanlayıcı denetimi içerir. üzerinde Zamanlayıcının geri gönderme `Tick` olayı oluşturulur. Zamanlayıcı denetimi içinde UpdatePanel yerleştirdiyseniz kısmi sayfa geri gönderme sırasında biz verileri yeni bir rastgele seçilmiş ürün görüntülenecek DetailsView rebind bir tetikler.

Bunu gerçekleştirmek için bir zamanlayıcı araç kutusundan sürükleyip UpdatePanel bırakın. Zamanlayıcının değiştirme `ID` gelen `Timer1` için `ProductTimer` ve kendi `Interval` 60000 özelliğine 15000. `Interval` Özelliği, Geri göndermeler arasında geçen milisaniye sayısını gösterir; için 15000 ayarı her 15 saniyede bir kısmi sayfa geri gönderme tetiklemek Zamanlayıcıyı neden olur. Bu noktada, Zamanlayıcının bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Zamanlayıcının için bir olay işleyicisi oluşturun `Tick` olay. DetailsView'ın çağırarak DetailsView verileri yeniden bağlamaya ihtiyacımız bu olay işleyicisinde `DataBind` yöntemi. Bunun yapılması verileri, verilerin kaynak denetiminden yeniden almak üzere DetailsView bildirir, seçin ve yeni bir rastgele görüntüleme (olduğu gibi sayfa tarayıcınızın yenile düğmesine tıklayarak zaman yeniden) kayıt seçili.


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Tüm İşte bu kadar kolay! Bir tarayıcı aracılığıyla sayfada yeniden ziyaret edin. Başlangıçta, rastgele bir ürünün bilgileri görüntülenir. Ekran alabilir izleyin, 15 saniye sonra yeni bir ürün hakkında bilgi Sihirli mevcut görüntüyü değiştirir, fark edeceksiniz.

Burada neler olduğunu daha iyi görmek için bir etiket denetimini ekranın en son güncelleştirildiği zaman görüntüleyen UpdatePanel ekleyelim. UpdatePanel içinde bir etiket Web denetimi ekleyin, kendi `ID` için `LastUpdateTime`, temizleyin, `Text` özelliği. Ardından, bir olay işleyicisi UpdatePanel için 's oluşturun `Load` olay ve görüntü etiketi geçerli saati. (UpdatePanel'ın `Load` her tam veya kısmi sayfa geri göndermede olay harekete geçirilir.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Bu değişiklik tam, sayfa şu anda görüntülenen ürünü yüklendi süreyi de içerir. Şekil 6 Sayfa ilk ziyaret edildiğinde gösterir. Şekil 7 sayfası 15 saniye sonra "Zamanlayıcı denetimi ticked" sonra yeni bir ürün hakkındaki bilgileri görüntülemek için UpdatePanel yenilendiğini gösterir.


[![Rastgele seçilmiş ürün üzerinde sayfa yükleme görüntülenir](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Şekil 06**: A rastgele seçilmiş ürün sayfa yüklendiğinde görüntülenen ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Her 15 saniyede bir yeni rastgele seçilmiş ürün görüntülenir](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Şekil 07**: her 15 saniyede bir yeni rastgele seçilmiş ürün görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>3. adım: ScriptManagerProxy denetimi kullanma

ScriptManager gerekli betik çerçevesi için sunulan ASP.NET AJAX istemci kitaplığı dahil olmak üzere birlikte özel JavaScript dosyaları, komut dosyası etkin Web Hizmetleri ve özel kimlik doğrulama, yetkilendirme ve profil hizmetler başvuruları da kaydedebilirsiniz. Genellikle bu tür özelleştirmeleri, belirli bir sayfaya özgüdür. Ancak, dosyalar, Web hizmeti başvuruları veya kimlik doğrulama özel betik, yetkilendirme veya profil hizmetler ana sayfada bir ScriptManager içinde başvurulan sonra dahil edilir *tüm* Web sayfaları.

Eklemek için bir sayfa tarafından temelinde ScriptManager ilgili özelleştirmeler ScriptManagerProxy denetimi kullanın. Bir ScriptManagerProxy içerik bir sayfaya ekleyin ve ardından özel JavaScript dosyası, Web hizmeti başvurusu veya kimlik doğrulaması, yetkilendirme veya ScriptManagerProxy profili hizmetinden kaydetme; Bu, bu hizmetler için belirli içerik sayfası kaydetme etkisi vardır.

> [!NOTE]
> ASP.NET sayfası, yalnızca birden fazla ScriptManager denetimi olabilir. Bu nedenle, ScriptManager denetimini ana sayfada zaten tanımlanmışsa bir ScriptManager denetimi için bir içerik sayfası eklenemiyor. Tek amacı ScriptManagerProxy, ana sayfada bir ScriptManager tanımlayın, ancak yine de bir sayfa tarafından temelinde ScriptManager özelleştirmeleri ekleme olanağı sahip için geliştiricilere bir yol sağlamaktır.


Şimdi de UpdatePanel ScriptManagerProxy denetimi iş başında görmek için büyütmek `ShowRandomProduct.aspx` duraklatma veya sürdürme Zamanlayıcı denetimi için istemci tarafı komut dosyası kullanan bir düğme eklemek için. Zamanlayıcı denetimi bu istenen işlevselliği elde etmek için kullanabileceğiniz üç istemci-tarafı yöntemi vardır:

- `_startTimer()` -Zamanlayıcı denetimini başlatır
- `_raiseTick()` -"değer çizgisi," Zamanlayıcı denetimi böylece geri gönderme ve yükseltme neden olur, `Tick` sunucusunda olayı
- `_stopTimer()` -Zamanlayıcı denetimi durdurur

Bir JavaScript dosyası adlı bir değişkenle oluşturalım `timerEnabled` ve adlı bir işlev `ToggleTimer`. `timerEnabled` Değişkeni Zamanlayıcı denetimi şu anda etkin olup olmadığını gösterir; bu true'dur. `ToggleTimer` İşlevi, iki giriş parametreleri kabul eder: duraklatın/sürdürün düğmesi ve istemci tarafı başvuru `id` Zamanlayıcı denetimi değeri. Bu işlev değerini değiştirir `timerEnabled`, Zamanlayıcı denetimi bir başvuru edinir, başlatıldığında veya durdurulduğunda Zamanlayıcı (değerine göre `timerEnabled`) ve "Duraklatma" veya "Devam" düğmenin görüntü metni güncelleştirir. Duraklatın/sürdürün düğmeye tıkladı olduğunda bu işlev çağrılır.

Yeni bir klasör adlı Web sitesi oluşturarak başlayın `Scripts`. Ardından, yeni bir dosya adlı Scripts klasörü olarak ekleme `TimerScript.js` JScript dosyası türü.


[![Scripts klasörü olarak yeni bir JavaScript dosyası ekleyin](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Şekil 08**: yeni bir JavaScript dosyasını eklemek `Scripts` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Web sitesine eklenmiş olan yeni bir JavaScript dosyası](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Şekil 09**: yeni JavaScript dosyası Web sitesine eklendi ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Ardından, aşağıdaki betik TimerScript.js dosyaya ekleyin:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Artık bu özel bir JavaScript dosyasında kayıt için ihtiyacımız `ShowRandomProduct.aspx`. Geri dönüp `ShowRandomProduct.aspx` ve bir ScriptManagerProxy denetimi sayfaya ekleyin; olarak kendi `ID` için `MyManagerProxy`. Özel bir JavaScript kaydetmek için dosya tasarımcıda ScriptManagerProxy denetimi seçin ve sonra Özellikler penceresine gidin. Özelliklerinden birini betikleri olarak adlandırılmıştır. Bu özellik seçildiğinde, Şekil 10'da gösterilen ScriptReference Koleksiyonu Düzenleyicisi görüntülenir. Ardından Path özelliği içindeki betik dosyasının yolunu girin ve yeni bir komut dosyası başvuru eklemek için Ekle düğmesine tıklayın: `~/Scripts/TimerScript.js`.


[![Bir ScriptManagerProxy denetimi için betik Başvurusu Ekle](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Şekil 10**: bir komut dosyası başvuru ScriptManagerProxy denetimi ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Betik başvurusu ScriptManagerProxy denetimi ekleme, bildirim temelli sonra biçimlendirme içerecek şekilde güncelleştirilmiştir bir `<Scripts>` tek bir koleksiyon `ScriptReference` girişi, biçimlendirme, aşağıdaki kod parçacığı gösterilmektedir:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

`ScriptReference` Giriş biçimlendirmenin, JavaScript dosyasına bir başvuru eklemek için ScriptManagerProxy bildirir. Diğer bir deyişle, özel kaydederek ScriptManagerProxy betik `ShowRandomProduct.aspx` sayfanın işlenmiş çıktı artık içerir başka `<script src="url"></script>` etiketi: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Biz çağırabilirsiniz `ToggleTimer` tanımlanan işlevi `TimerScript.js` istemci komut dosyası içinde `ShowRandomProduct.aspx` sayfası. Aşağıdaki HTML'yi UpdatePanel içinde ekleyin:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Bu, "Duraklatma" metin bir düğme görüntüler. Herhangi bir zamanda bu tıklatıldığında JavaScript işlevinin `ToggleTimer` , yapılan bir başvuru düğmesi ve Zamanlayıcı denetimi kimliği değerini geçirme çağrılır (`ProductTimer`). Alma için söz dizimi unutmayın `id` Zamanlayıcı denetimi değeri. `<%=ProductTimer.ClientID%>` değerini yayan `ProductTimer` Zamanlayıcı denetimin `ClientID` özelliği. İçinde [ *denetim kimliği adlandırma içerik sayfalarındaki* ](control-id-naming-in-content-pages-cs.md) sunucu tarafı arasındaki farkları ele aldığımız öğretici `ID` değer ve elde edilen istemci tarafı `id` değerini ve nasıl `ClientID` istemci-tarafı döndürür `id`.

Şekil 11, bir tarayıcıdan ilk ziyaret edildiğinde bu sayfada görüntülenir. Zamanlayıcı, şu anda çalışıyor ve her 15 saniyede görüntülenen ürün bilgileri güncelleştirir. Duraklat düğmesine tıkladıktan sonra çıkan Şekil 12 ekranı gösterilir. Duraklat düğmesine tıklayarak Zamanlayıcıyı durdurur ve "Devam" düğmenin metni güncelleştirir. Ürün bilgileri Yenile (ve her 15 saniyede yenilemeye devam etmek) sonra devam et kullanıcı tıklar.


[![Zamanlayıcı denetimi durdurmak için Duraklat düğmesini tıklatın](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Şekil 11**: Zamanlayıcı denetimi durdurmak için Duraklat düğmesini tıklatın ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Zamanlayıcıyı yeniden başlatmak için devam düğmesine tıklayın](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Şekil 12**: Zamanlayıcıyı yeniden başlatmak için devam düğmesine tıklayın ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Özet

ASP.NET AJAX framework kullanarak AJAX etkinleştirilmiş web uygulamaları oluştururken her AJAX etkinleştirilmiş web sayfası bir ScriptManager denetimi içerdiğini zorunludur. Bu işlemi kolaylaştırmak için bir ScriptManager her içerik bir ScriptManager eklemek anımsamak yerine ana sayfaya ekleyebiliriz. Adım 1 ana sayfanın AJAX işlevselliği bir içerik sayfasındaki uygulama sırasında aranan adım 2 sırasında ScriptManager eklemek nasıl oluşturulacağını gösterir.

Özel betikler, Web Hizmetleri, etkin komut dosyası başvuruları eklemeniz veya özelleştirilmiş kimlik doğrulaması, yetkilendirme veya belirli bir içerik sayfasının profil hizmetler için içerik sayfası bir ScriptManagerProxy denetimi ekleyin ve yapılandırın Özelleştirmeleri vardır. Adım 3, belirli bir içerik sayfasındaki özel bir JavaScript dosyası kaydetmek için ScriptManagerProxy kullanma incelenir.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET AJAX çerçevesi](../../../../ajax/index.md)
- [ASP.NET AJAX öğreticiler](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX videoları](../../../videos/aspnet-ajax/index.md)
- [Microsoft ASP.NET AJAX ile etkileşimli kullanıcı arabirimi oluşturma](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Rastgele sıralama kayıtlara NEWID kullanma](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Zamanlayıcı denetimi kullanma](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](interacting-with-the-content-page-from-the-master-page-cs.md)
> [İleri](specifying-the-master-page-programmatically-cs.md)
