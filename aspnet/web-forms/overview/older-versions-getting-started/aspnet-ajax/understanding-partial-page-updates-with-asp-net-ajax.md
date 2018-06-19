---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Kısmi Sayfa güncelleştirmelerini ASP.NET AJAX ile anlama | Microsoft Docs
author: scottcate
description: Belki de en görünür, ASP.NET AJAX uzantıları kısmi veya artımlı sayfası güncelleştirmeleri t tam geri gönderimin yapmadan yeteneği özelliğidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 91a98bf1c9a71ae84c569f7ae40930422cb652e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891327"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>ASP.NET AJAX ile anlama kısmi sayfa güncelleştirir
====================
tarafından [Scott göstermek](https://github.com/scottcate)

[PDF indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Belki de en görünür, ASP.NET AJAX uzantıları kısmi veya artımlı sayfası güncelleştirmeleri sunucuya hiçbir kod değişikliklerini ve en az biçimlendirme değişiklikleri ile tam geri gönderimin yapmadan yeteneği özelliğidir. Avantajları kapsamlı – (örneğin, Adobe Flash veya Windows Media) çoklu değişmemiş bir durumda, bant genişliği maliyetlerini düşürür ve istemci genellikle geri gönderimin ile ilişkilendirilmiş titreşimi karşılaşmaz.


## <a name="introduction"></a>Giriş

Microsoft ASP.NET teknolojisi bir nesne yönelimli ve olay kaynaklı programlama modeli getirir ve derlenmiş kod yararları birleştirmiştir. Ancak, kendi sunucu tarafı işleme modelini teknolojisinde devralınan birkaç sakıncaları vardır:

- Sayfa güncelleştirmelerini sayfa yenileme gerektiriyor sunucuya gidiş gerektirir.
- Gidiş dönüş Javascript veya başka bir istemci-tarafı teknolojinin (örneğin, Adobe Flash) tarafından oluşturulan tüm etkilerinin devam etmez
- Geri gönderme sırasında tarayıcılar Microsoft Internet Explorer dışında otomatik olarak kaydırma konumunu geri yüklemeyi desteklemez. Ve hatta Internet Explorer'da hala bir titreşimi Sayfa yenilendiğinde gibi.
- Geri göndermeler yüksek miktarda bant genişliği gerektirebilir \_ \_VIEWSTATE form alanı büyüme, özellikle GridView denetimini veya yineleyiciler gibi denetimleri ile ilgilenirken.
- JavaScript veya diğer istemci-tarafı teknolojisi Web hizmetlerine erişme için birleşik hiçbir model yok.

Microsoft ASP.NET AJAX uzantılarını girin. Anlamına gelir AJAX **A** zaman uyumlu **J** avaScript **A** nd **X** ML, artımlı sayfa sağlamak için tümleşik bir çerçeve olan platformlar arası JavaScript aracılığıyla güncelleştirmeleri Microsoft AJAX Framework ve Microsoft AJAX komut dosyası kitaplığı adlı bir komut dosyası bileşen kapsayan sunucu tarafı kodu oluşur. ASP.NET AJAX uzantılar, ASP.NET Web Hizmetleri JavaScript aracılığıyla erişmek için platformlar arası desteği de sunar.

Bu teknik ScriptManager bileşeni, UpdatePanel denetim ve UpdateProgress denetim içerir ve bunlar gerekir veya olan olmamalıdır senaryoları göz önünde bulundurur ASP.NET AJAX uzantılar, kısmi Sayfa güncelleştirmelerini işlevselliğini inceler kullanılan.

Bu teknik Visual Studio 2008 Beta 2 sürümünü ve temel sınıf kitaplığı (burada ASP.NET 2.0 için kullanılabilir bir eklenti bileşeni öncekinden) ASP.NET AJAX uzantıları tümleştirir .NET Framework 3.5 temel alır. Bu Teknik İnceleme de Visual Studio 2008 ve değil Visual Web Developer Express Edition kullandığınızı varsayar; Visual Web Developer Express kullanıcılara başvurulan bazı proje şablonları kullanılamayabilir.

## <a name="partial-page-updates"></a>Kısmi Sayfa güncelleştirmelerini

Belki de en görünür, ASP.NET AJAX uzantıları kısmi veya artımlı sayfası güncelleştirmeleri sunucuya hiçbir kod değişikliklerini ve en az biçimlendirme değişiklikleri ile tam geri gönderimin yapmadan yeteneği özelliğidir. Avantajları kapsamlı - (örneğin, Adobe Flash veya Windows Media) çoklu değişmemiş bir durumda, bant genişliği maliyetlerini düşürür ve istemci genellikle geri gönderimin ile ilişkilendirilmiş titreşimi karşılaşmaz.

Kısmi sayfa işleme tümleştirme özelliği ASP projenize küçük değişiklikler tümleşiktir.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>İzlenecek yol: Kısmi işleme varolan bir projeye tümleştirme


1. Microsoft Visual Studio 2008'de giderek yeni bir ASP.NET Web sitesi projesi oluşturma <em>dosya</em>  <em>- &gt; yeni</em>  <em>- &gt; Web sitesi</em> ve ASP.NET Web sitesi iletişim kutusundan seçerek. İstediğiniz gibi adlandırabilirsiniz ve dosya sistemi veya Internet Information Services (IIS) içine yükleyebilir.
2. Boş varsayılan sayfanın temel ASP.NET biçimlendirme ile birlikte sunulur (sunucu tarafı form ve bir `@Page` yönergesi). Adlı bir etiket bırakma `Label1` ve bir düğme adlı `Button1` form öğesi içinde sayfaya. Metin özellikleri ne olursa olsun ister ayarlayabilir.
3. Tasarım görünümünde çift `Button1` bir arka plan kodu olay işleyicisi oluşturmak için. Bu olay işleyicisi içinde ayarlamak `Label1.Text` için düğmesine tıklanana! biçimindeki telefon numarasıdır.

**Kod 1: Kısmi işleme etkinleştirilmeden önce default.aspx biçimlendirme**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Kod 2: (default.aspx.cs içinde kırpılmış) arkasındaki koda**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Web sitenizi başlatmak için F5 tuşuna basın. Visual Studio hata ayıklamayı etkinleştirmek için bir web.config dosyası eklemek isteyip istemediğinizi sorar; Bunu yapın. Düğmeye tıkladığınızda, etiket metni değiştirmek için sayfa yenilendikten ve sayfa çizilme gibi kısa titreşimi olduğu dikkat edin.
2. Tarayıcı pencerenizi kapattıktan sonra Visual Studio ve biçimlendirme sayfasına dönün. Visual Studio araç kutusunda aşağı kaydırın ve AJAX uzantıları etiketli sekmesinde bulabilirsiniz. (AJAX veya Atlas uzantıları daha eski bir sürümü kullandığından bu sekme yoksa bu teknik yazıda AJAX uzantıları araç kutusu öğelerini kaydettirmek için izlenecek bakın veya Windows Installer indirilebilir geçerli sürümünü yükleyin Web sitesinden).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Bilinen bir sorun:</em>Visual Studio 2008 ASP.NET 2.0 AJAX uzantılarıyla yüklü Visual Studio 2005 zaten olan bir bilgisayara yüklerseniz, Visual Studio 2008 AJAX uzantıları araç kutusu öğelerini içeri aktaracak. Araç İpucu bileşenlerini inceleyerek bu durum geçerli olup olmadığını belirlemek; Bunlar, sürüm 3.5.0.0 yazması gerekir. Bunlar 2.0.0.0 veya söylediğinizde sonra eski araç kutusu öğelerini içe aktardıktan ve el ile Visual Studio içinde araç kutusu öğelerini Seç iletişim kutusunu kullanarak içe aktarmanız gerekir. Tasarımcı aracılığıyla sürüm 2 denetimleri ekleme yükleyemez.

2. Önce `<asp:Label>` etiketi başlar, boşluk, bir satır oluşturmak ve araç UpdatePanel denetiminde çift tıklayın. Unutmayın yeni `@Register` yönergesi System.Web.UI ad alanı içinde denetimleri kullanarak içeri gerektiğini belirten sayfanın en üstünde bulunur `asp:` öneki.
3. Kapatma sürükleyin `</asp:UpdatePanel>` öğesi Sarmalanan düğmeyi ve etiket denetimleri ile iyi biçimlendirilmiş böylece Button öğesi sonunun etiketi.
4. Açtıktan sonra `<asp:UpdatePanel>` etiketi, yeni bir etiket açarak başlar. IntelliSense, iki seçenek ister unutmayın. Bu durumda, oluşturma bir `<ContentTemplate>` etiketi. Bu etiketin etiket ve düğmesi çevresine işaretleme doğru biçimlendirildiğinden emin sarmalamak emin olun.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. İçinde herhangi bir yerde `<form>` öğesi, çift tıklayarak bir ScriptManager denetimi dahil `ScriptManager` araç kutusu öğesi.
2. Düzen `<asp:ScriptManager>` özniteliği içeren etiket `EnablePartialRendering= true`.

**Kod 3: Etkin kısmi işleme ile default.aspx biçimlendirme**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Web.config dosyanızı açın. Visual Studio derleme başvurusu System.Web.Extensions.dll için otomatik olarak ekledi dikkat edin.

1. Visual Studio 2008'deki yenilikler: ASP.NET Web sitesi proje şablonlarıyla otomatik olarak gelen web.config ASP.NET AJAX uzantıları için gereken tüm başvurular içerir ve açıklamalı bölümleri olabilir yapılandırma bilgilerini içerir Ek işlevlerini etkinleştirmek için açıklamalı kaydetmeyin. ASP.NET 2.0 AJAX uzantıları yüklendiğinde visual Studio 2005 benzer şablon vardı. Ancak, Visual Studio 2008'de AJAX çevirin uzantıları varsayılan olarak (diğer bir deyişle, bunlar varsayılan olarak başvurulan, ancak başvuru olarak kaldırılabilir).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Web sitenizi başlatmak için F5 tuşuna basın. Nasıl hiçbir kaynak kod değişiklikleri kısmi işleme desteklemek için gerekli - yalnızca biçimlendirme değiştirildi unutmayın.

Web sitenizi başlattığında, kısmi işleme etkin artık, tıkladığınızda düğmesi vardır hiçbir titreşimi olur ya da vardır (Bu örnekte, gösterilmemiştir) sayfasında kaydırma konumda herhangi bir değişiklik olacak çünkü olduğu görmeniz gerekir. Düğmesine tıkladıktan sonra sayfanın işlenmiş kaynağında aramak için olsaydı, aslında sonrası geri değil oluştu - özgün etiket metnini hala kaynak işaretleme parçasıdır ve etiket JavaScript değişti onaylar.

Visual Studio 2008 ASP.NET AJAX etkin bir web sitesi için önceden tanımlanmış bir şablonu ile birlikte gelen görünmüyor. Ancak, ASP.NET 2.0 AJAX uzantıları ve Visual Studio 2005 yüklenmişse gibi bir şablon Visual Studio 2005 içinde kullanılabilir. Şablon (tüm Web Hizmetleri erişim dahil olmak üzere ASP.NET AJAX uzantılarını destekleyen bir tam olarak yapılandırılmamış web.config dosyası içermelidir sonuç olarak, web sitesi yapılandırma ve AJAX-Enabled Web sitesi şablonu ile başlayarak büyük olasılıkla daha kolay olacaktır ve JSON serileştirmesi - JavaScript nesne gösterimi) ve bir UpdatePanel ve ContentTemplate ana Web Forms sayfası içinde varsayılan olarak içerir. Bu varsayılan sayfayla kısmi işleme etkinleştirme adımı 10 bu kılavuzun yeniden ziyaret etme ve denetimleri sayfaya bırakma kadar basittir.

## <a name="the-scriptmanager-control"></a>ScriptManager denetimi

## <a name="scriptmanager-control-reference"></a>ScriptManager denetimi başvurusu

Biçimlendirme etkin özellikleri:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | bool | Web.config dosyasının özel hata bölümü hataları işlemek için kullanılıp kullanılmayacağını belirtir. |
| AsyncPostBackError iletisi | Dize | Alır veya bir hata oluşursa istemciye gönderilen hata iletisini ayarlar. |
| AsyncPostBack zaman aşımı | Int32 | Alır veya varsayılan bir istemci zaman uyumsuz istek için beklemesi gereken süreyi ayarlar. |
| ENABLESCRIPT Genelleştirme | bool | Alır veya komut dosyası Genelleştirme etkinleştirilip etkinleştirilmediğini ayarlar. |
| EnableScript-Localization | bool | Alır veya komut dosyası yerelleştirme etkinleştirilip etkinleştirilmediğini ayarlar. |
| ScriptLoadTimeout | Int32 | Komut istemcisini yüklemek için izin verilen saniye sayısını belirler |
| ScriptMode | Enum (otomatik olarak, hata ayıklama, yayın, devralma) | Alır veya yayın sürümleri komut dosyaları oluşturmak ayarlar |
| ScriptPath | Dize | Alır veya kök yolu istemciye gönderilecek komut dosyalarının konumunu ayarlar. |

Yalnızca kod özellikleri:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService Yöneticisi | İstemciye gönderilen ASP.NET kimlik doğrulama hizmeti proxy ayrıntılarını alır. |
| IsDebuggingEnabled | bool | Alır mı komut dosyası ve kodda hata ayıklama etkin. |
| IsInAsyncPostback | bool | Sayfa içinde zaman uyumsuz bir post geri İstek şu anda olup olmadığını alır. |
| ProfileService | ProfileService-Manager | İstemciye gönderilen ASP.NET Profil Hizmeti proxy ayrıntılarını alır. |
| Komut dosyaları | Koleksiyon&lt;komut başvurusu&gt; | İstemciye gönderilen komut dosyası başvuruları koleksiyonunu alır. |
| Hizmetler | Koleksiyon&lt;hizmet başvurusu&gt; | İstemciye gönderilen Web hizmeti proxy başvuruları koleksiyonunu alır. |
| SupportsPartialRendering | bool | Geçerli istemci kısmi işleme destekleyip desteklemediğini alır. Bu özellik döndürürse **yanlış**, sonra da tüm sayfa istekleri standart Geri göndermeler olacaktır. |

Ortak kod yöntemleri:

| **Yöntem adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| SetFocus(string) | Geçersiz kılma | İstek tamamlandıktan sonra istemcinin odağı belirli bir denetim için ayarlar. |

Biçimlendirme alt öğeleri:

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;AuthenticationService&gt; | ASP.NET kimlik doğrulama hizmeti için proxy hakkında ayrıntılar sağlar. |
| &lt;ProfileService&gt; | Hizmet profili oluşturma ASP.NET proxy hakkında ayrıntılar sağlar. |
| &lt;Komut dosyaları&gt; | Ek komut dosyası başvuru sağlar. |
| &lt;asp:ScriptReference&gt; | Bir özel komut dosyası referansının gösterir. |
| &lt;Hizmet&gt; | Oluşturulan proxy sınıflar olan ek Web hizmeti başvuru sağlar. |
| &lt;asp:ServiceReference&gt; | Belirli bir Web Hizmeti başvurusunu gösterir. |

ASP.NET AJAX uzantıları için temel çekirdek ScriptManager denetimdir. Kod kitaplığı (kapsamlı istemci tarafı komut dosyası türü sistem dahil) erişim sağlayan, kısmi işleme destekler ve ek ASP.NET Hizmetleri (örneğin, kimlik doğrulama ve profil oluşturma, aynı zamanda diğer Web Hizmetleri) için kapsamlı destek sağlar. ScriptManager denetimi de Genelleştirme ve yerelleştirme için istemci komut dosyalarını desteği sağlar.

## <a name="providing-alterative-and-supplemental-scripts"></a>Diğer ve ek komut dosyaları sağlama

Microsoft ASP.NET 2.0 AJAX uzantıları hem hata ayıklama modunda tüm betik kodu içerir ve başvurulan derlemeleri gömülü kaynaklar olarak sürümleri yayın olsa da, geliştiriciler gibi özelleştirilmiş komut dosyalarının ScriptManager yönlendirmek kaydetmek boş Ek gerekli komut dosyaları.

Genellikle dahil komut dosyaları (örneğin Sys.WebForms ad alanı ve özel yazarak sistem destekleyen) için varsayılan bağlama geçersiz kılmak için için kaydedebilirsiniz `ResolveScriptReference` ScriptManager sınıfının olay. Bu yöntem çağrıldığında, olay işleyici komut dosyası söz konusu yolunu değiştirmek fırsatına sahip; betik Yöneticisi'ni, sonra komut dosyalarını farklı veya özelleştirilmiş bir kopyasını istemciye gönderir.

Ayrıca, komut dosyası başvuruları (tarafından temsil edilen `ScriptReference` sınıfı) programlı olarak veya işaretleme aracılığıyla eklenebilir. Bunu yapmak için ya da program aracılığıyla değiştirme `ScriptManager.Scripts` koleksiyonu veya dahil `<asp:ScriptReference>` altında etiketler `<Scripts>` ScriptManager denetimi birinci düzey alt etiketi.

## <a name="custom-error-handling-for-updatepanels"></a>Özel hata için UpdatePanels işleme

Güncelleştirmeleri UpdatePanel denetimleri tarafından belirtilen tetikleyiciler tarafından işlenir ancak hata işleme ve özel hata iletileri için destek bir sayfanın ScriptManager denetimi örneği tarafından işlenir. Bu bir olay göstererek yapılır `AsyncPostBackError`, yapabilirsiniz sayfaya sonra özel özel durum işleme mantığı sağlar.

AsyncPostBackError olay tüketen tarafından belirttiğiniz `AsyncPostBackErrorMessage` bir uyarı kutusu geri çağırma tamamlandıktan sonra çıkarılmasına neden olan özelliği.

İstemci tarafı özelleştirmesi, varsayılan uyarı kutusunu kullanmak yerine mümkündür; Örneğin, bir özelleştirilmiş görüntülemek isteyebilirsiniz `<div>` varsayılan tarayıcı modal iletişim kutusu yerine öğesi. Bu durumda, istemci komut dosyası hata işleyebilir:

**5 listeleme: özel hataları görüntülemek için istemci tarafı komut dosyası**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Eklemenin, zaman uyumsuz istek tamamlandığında yukarıdaki komut dosyasını istemci tarafı AJAX çalışma zamanı için bir geri çağırma kaydeder. Ardından bir hata bildirildi olup olmadığını denetler ve varsa, bu, son olarak çalışma özel komut dosyasında hata işlendiğini gösteren ayrıntılarını işler.

## <a name="globalization-and-localization-support"></a>Genelleştirme ve yerelleştirme desteği

ScriptManager denetimi betik dizelerini ve kullanıcı arabirimi bileşenlerini yerelleştirme için kapsamlı destek sağlar; Ancak, bu konuda bu teknik kapsamı dışında ' dir. Daha fazla bilgi için ASP.NET AJAX uzantıları Genelleştirme desteği teknik incelemesine bakın.

## <a name="the-updatepanel-control"></a>UpdatePanel denetimi

## <a name="updatepanel-control-reference"></a>UpdatePanel denetim başvurusu

Biçimlendirme etkin özellikleri:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Alt denetimler otomatik olarak geri gönderme üzerinde yenileme çağırma olup olmadığını belirtir. |
| RenderMode | Enum (blok, satır içi) | İçerik yolu görsel olarak sunulan belirtir. |
| UpdateMode | Enum (her zaman, koşullu) | UpdatePanel kısmi işleme sırasında her zaman yenilenip yenilenmeyeceğini veya tetikleyici gelindiğinde yalnızca yenileniyor olmadığını belirtir. |

Yalnızca kod özellikleri:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| IsInPartialRendering | bool | UpdatePanel geçerli istek için kısmi işleme destekleme olup olmadığını alır. |
| ContentTemplate | ITemplate | Biçimlendirme şablonu için güncelleştirme isteği alır. |
| ContentTemplateContainer | Denetim | Programlama şablonunu için güncelleştirme isteği alır. |
| Tetikleyiciler | UpdatePanel - TriggerCollection | Geçerli UpdatePanel ile ilişkilendirilmiş tetikleyiciler listesini alır. |

Ortak kod yöntemleri:

| **Yöntem adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| Update() | Geçersiz kılma | Belirtilen UpdatePanel programlı olarak güncelleştirir. Aksi takdirde untriggered UpdatePanel, kısmi bir işleme tetiklemek sunucu isteği sağlar. |

Biçimlendirme alt öğeleri:

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;ContentTemplate&gt; | Kısmi işleme sonuç işlemek için kullanılacak biçimlendirmeyi belirtir. Alt &lt;asp: UpdatePanel&gt;. |
| &lt;Tetikleyiciler&gt; | Bir koleksiyonunu belirtir *n* bu UpdatePanel güncelleştirme ile ilişkili denetimleri. Alt &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Kısmi sayfa işleme için belirli UpdatePanel çağıran bir tetikleyici belirtir. Bu olabilir veya bir denetim söz konusu UpdatePanel alt öğesi olmayabilir. Ayrıntılı olay adı. Alt &lt;Tetikleyicileri&gt;. |
| &lt;asp:PostBackTrigger&gt; | Tüm sayfayı yenilemek neden olan bir denetim belirtir. Bu olabilir veya bir denetim söz konusu UpdatePanel alt öğesi olmayabilir. Nesne için ayrıntılı. Alt &lt;Tetikleyicileri&gt;. |

`UpdatePanel` Denetimidir bölümü AJAX uzantıları kısmi işleme işlevselliğini sürer sunucu tarafı içerik sınırlandıran denetimi. Bir sayfada olabilir UpdatePanel denetimleri sayısına bir sınır yoktur ve bunlar iç içe. Her bağımsız olarak çalışabilmeniz için her UpdatePanel yalıtılır (sayfanın farklı bölümlerini sayfanın geri gönderme bağımsız işleme aynı anda çalışan iki UpdatePanels olabilir).

UpdatePanel denetim öncelikle anlaşmalar denetim tetikleyicileri ile - varsayılan olarak, bir UpdatePanel içinde 's bulunan herhangi bir denetim `ContentTemplate` geri gönderimin oluşturan bir tetikleyici olarak UpdatePanel için kayıtlı değil. Bu UpdatePanel kullanıcı denetimleri ile (örneğin, GridView), varsayılan veri bağlama denetimleri çalışabilir ve komut dosyasında programlanabilir anlamına gelir.

Varsayılan olarak, kısmi sayfa işleme tetiklendiğinde sayfadaki tüm UpdatePanel denetimleri yenilenir, desteklemediğini UpdatePanel denetimleri bu tür bir eylemin Tetikleyicileri tanımlı. Örneğin, bir UpdatePanel düğme denetimi tanımlar ve bu düğme denetimi tıklandığında, varsayılan olarak bu sayfadaki tüm UpdatePanel denetimleri yenilenecektir. Bunun nedeni, varsayılan olarak, `UpdateMode` UpdatePanel özelliği ayarlanmış `Always`. Alternatif olarak, size UpdateMode özellik kümesine `Conditional`, başka bir deyişle, belirli bir tetikleyicinin ulaşılırsa, UpdatePanel'in yalnızca yenilenir.

## <a name="custom-control-notes"></a>Özel denetim notları

Bir UpdatePanel herhangi bir kullanıcı denetimi veya özel denetim eklenebilir; Ancak, bu denetimleri olduğu dahil sayfada bir ScriptManager denetimi EnablePartialRendering ayarlanan özelliği ile de bulunmalıdır **doğru**.

Web özel denetimleri kullanarak korumalı geçersiz kılmak için olduğunda, tek yönlü bu hesabı `CreateChildControls()` yöntemi `CompositeControl` sınıfı. Bunu yaparak, kısmi işleme sayfanın desteklediği karar verirseniz denetimin alt ve dış dünya arasında bir UpdatePanel ekleyemezsiniz; Aksi takdirde, yalnızca alt denetimleri bir kapsayıcıya katmanı `Control` örneği.

## <a name="updatepanel-considerations"></a>UpdatePanel dikkat edilecek noktalar

UpdatePanel şey bir siyah kutusu, ASP.NET Geri göndermeler bir JavaScript XMLHttpRequest bağlamında kaydırma çalışır. Ancak, unutmayın, hem davranış ve hız açısından ayı için önemli performans noktalar vardır. UpdatePanel nasıl çalışır, böylece kullanımına uygun olduğunda en iyi karar verebilirsiniz anlamak için AJAX exchange incelemeniz gerekir. Aşağıdaki örnekte bir var olan site ve, Mozilla Firefox (Firebug XMLHttpRequest verileri yakalar) Firebug uzantısıyla kullanır.

Başka şeylerin bir form veya denetim bir şehir ve Bölge alanı doldurmak için beklenen bir posta kodu textbox olan bir form göz önünde bulundurun. Bu formu, sonuçta bir kullanıcının adını, adresini ve kişi bilgileri de dahil olmak üzere, üyelik bilgilerini toplar. Belirli bir proje gereksinimlerine bağlı olarak dikkate almak için çoğu tasarımları vardır.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


Bu uygulamanın özgün yinelemede bir denetim, posta kodu, şehir ve bölge dahil olmak üzere kullanıcı kayıt verilerini tamamen birleştirilmiş oluşturuldu. Tüm denetim içinde bir UpdatePanel Sarmalanan ve Web formunun üzerine bırakıldı. Posta kodu kullanıcı tarafından girilen UpdatePanel (arka uç, tetikleyicileri belirterek veya true ChildrenAsTriggers özellik kümesi kullanarak karşılık gelen TextChanged olayı) olay algılar. AJAX yazılarını, tüm alanları UpdatePanel içinde FireBug tarafından yakalanan gibi (sağ tarafta diyagramına bakın).

Ekran yakalamayı da anlaşılacağı gibi her denetim UpdatePanel içinde değerlerinden teslim edilir (Bu durumda, bunlar tüm boş olan), Görünüm durumu alanı yanı sıra. Aslında yalnızca beş veri baytı sayısı bu belirli isteği yapmak için gerekli olan tüm told üzerinde 9 kb veri, gönderilir. Yanıt daha da bloated: Toplam, yalnızca bir metin alanı ve açılan alanını güncelleştirmek için istemciye 57kb gönderilir.

Ayrıca, nasıl ASP.NET AJAX sunu güncelleştirmeleri görmek için ilgi çekici de olabilir. UpdatePanel'ın güncelleştirme isteği yanıt kısmı soldaki Firebug konsol görünümünde gösterilir; Bu istemci komut dosyası tarafından bölünür ve sayfada yeniden birleştirilen bir özel şeklide kanal ayrılmış dizesidir. Özellikle, ASP.NET AJAX ayarlar *InnerHtml* , UpdatePanel temsil eden istemcideki HTML öğesi özelliği. Tarayıcı DOM yeniden oluşturur olarak işlenmesi için gereken bilgiler miktarına bağlı olarak bir gecikme yoktur.

DOM yeniden oluşturma işlemi birkaç ek sorunlar tetikler:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Odaklanmış HTML öğesi içinde UpdatePanel ise, odak kaybedersiniz. Bu nedenle, posta kodu metin kutusu'ndan çıkmak için SEKME tuşunu basılı kullanıcılar için sonraki hedeflerine Şehir metin kutusu olacaktı. Ancak, görüntü UpdatePanel yenilenir sonra formun artık odağa sahip ve Tab tuşuna basarak odağı öğelerinin (örneğin, bağlantıları) vurgulama başlamış olması.
- Özel istemci tarafı komut dosyası herhangi bir türde erişimleri DOM öğeleri, başvuruları işlevleri tarafından kalıcı kullanılmıyorsa, kısmi bir geri gönderme sonra geçersiz hale gelebilir.

UpdatePanels catch tüm çözümleri olması amaçlanmamıştır. Bunun yerine, bunlar hızlı bir çözüm belirli durumlarda, prototipi oluşturulurken, küçük denetim güncelleştirmeleri de dahil olmak üzere sağlar ve .NET nesne modeli aşina ancak şekilde daha az DOM ile olabilir ASP.NET geliştiriciler için bilinen bir arayüz sağlar Uygulama senaryosuna bağlı olarak daha iyi performans sonuçlanabilir alternatifleri vardır:

- PageMethods kullanmayı düşünün ve bir web hizmeti çağrısı çağrıldı gibi bir sayfa üzerinde statik yöntemlerini çağırmak Geliştirici JSON (JavaScript nesne gösterimi) sağlar. Yöntem statik olduğundan, hiçbir durumu gereklidir; komut dosyasını çağıran parametreleri sağlar ve sonuç zaman uyumsuz olarak döndürülür.
- Tek bir denetim çeşitli yerlerde bir uygulama arasında kullanılması gerekiyorsa, bir Web hizmeti ve JSON kullanmayı düşünün. Bu yeniden çok az özel iş gerektirir ve zaman uyumsuz olarak çalışır.

Web Hizmetleri veya sayfa yöntemleri aracılığıyla işlevselliği ekleme de sakıncaları vardır. Öncelikle, ASP.NET geliştiricilerinin genellikle kullanıcı denetimi (.ascx dosyaları) derleme işlevselliğin küçük bileşenlerini eğilimindedir. Bu dosyalarda sayfa yöntemleri barındırılamaz; Bunlar içindeki gerçek .aspx sayfası sınıf barındırılması gerekir. Web Hizmetleri içinde .asmx sınıfı benzer şekilde, barındırılması gerekir. Tek bir bileşen için işlevsellik artık çok az kayıpla veya hiç bağlı TIES olabilir iki veya daha fazla fiziksel bileşenler arasında yayılır, uygulamaya bağlı olarak, bu mimarinin tek sorumluluk ilkesini ihlal edebilecek.

Son olarak, bir uygulama UpdatePanels kullanılmasını gerektiriyorsa, aşağıdaki yönergeleri sorun giderme ve Bakım yardımcı olmalıdır.

- **UpdatePanels mümkün olduğunca az yalnızca içinde-birimleri, aynı zamanda kod birimleri iç içe.** Örneğin, bir denetim denetleyen bir UpdatePanel içeren başka bir denetimi içeren bir UpdatePanel içerirken kaydırılan bir sayfada bir UpdatePanel sahip arası birim iç içe geçme istenir. Bu açık tutmak için hangi öğelerin yenilemeye olmalıdır ve alt UpdatePanels beklenmeyen yenilemeleri engeller yardımcı olur.
- **Tutmak *ChildrenAsTriggers* özelliği false olarak ayarlayın ve açıkça olaylarını tetikleme ayarlayın.** Kullanılarak `<Triggers>` koleksiyonu olayları işlemek için çok daha net bir yoldur ve bakım görevleri ile yardımcı olur ve bir geliştirici katılımı için bir olay için zorlama beklenmeyen davranışları engelleyebilir.
- **En küçük olası birimi işlevselliği elde etmek için kullanın.** Sunucu, toplam işleme ve istemci-sunucu exchange ayak yalnızca tam minimum saat azaltır kaydırma posta kodu hizmet tartışmada belirtildiği gibi performansı artırır.

## <a name="the-updateprogress-control"></a>UpdateProgress denetimi

## <a name="updateprogress-control-reference"></a>UpdateProgress denetim başvurusu

Biçimlendirme etkin özellikleri:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Dize | Bu UpdateProgress üzerinde bildirmelisiniz UpdatePanel Kimliğini belirtir. |
| DisplayAfter | int | Zaman uyumsuz istek başladıktan sonra bu denetim görüntülenmeden önce zaman aşımı milisaniye cinsinden belirtir. |
| DynamicLayout | bool | İlerleme dinamik olarak işlenip işlenmeyeceğini belirtir. |

Biçimlendirme alt öğeleri:

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Bu denetimi ile görüntülenen içerik için ayarlanmış denetim şablonu içerir. |

UpdateProgress denetimi sunucuya taşıma için gerekli olan iş yaparken, kullanıcılarınızın ilgi tutmak için geri bildirim ölçüsü sağlar. Bu, özellikle kullanıcıların çoğunun yenileme ve vurgulayın durum çubuğu görmesini sayfasına kullanıldığından onu görünen, olmayabilir olsa bile bir şey yaptığınız olduğunu biliyorsanız, kullanıcılarınızın yardımcı olabilir.

Bir not olarak UpdateProgress denetimleri bir sayfa hiyerarşi herhangi bir yerde görünebilir. Ancak, kısmi bir geri gönderme alt (burada bir UpdatePanel başka bir UpdatePanel içinde yer alan) UpdatePanel başlatıldığına durumlarda, bu tetikleyici UpdatePanel görüntülenecek UpdateProgress şablonları için alt neden olacak alt postbacks UpdatePanel yanı sıra UpdatePanel üst. Ancak, tetikleyici doğrudan alt öğesi üst UpdatePanel ise, yalnızca üst öğesi ile ilişkili UpdateProgress şablonları sonra görüntülenir.

## <a name="summary"></a>Özet

Microsoft ASP.NET AJAX, web içeriği daha erişilebilir hale getirilmesi konusunda yardımcı olmak ve web uygulamaları için daha zengin bir kullanıcı deneyimi sağlamak için tasarlanmış gelişmiş ürünleri uzantılarıdır. ASP.NET AJAX uzantıları olan kısmi sayfası işleme denetimlerinin bir parçası olarak ScriptManager, UpdatePanel ve UpdateProgress denetimler de dahil olmak üzere araç setini en görünür bileşenlerinin bazılarıdır.

ScriptManager bileşeni istemci JavaScript sağlama uzantıları için tümleştirir, aynı zamanda en az geliştirme yatırım ile birlikte çalışmak çeşitli sunucu ve istemci tarafı bileşenleri sağlar.

UpdatePanel denetim görünen Sihirli kutusudur - işaretleme UpdatePanel içinde sunucu tarafı arkasındaki koda sahip ve bir sayfa yenileme tetiklemek değil. UpdatePanel denetimleri iç içe olabilir ve diğer UpdatePanels denetimlerinde bağımlı olabilir. Bu işlev ince, bildirimli olarak veya program aracılığıyla ayarlanabilecek rağmen varsayılan olarak, kendi alt denetimleri tarafından çağrılan bir Geri göndermeler UpdatePanels işleyin.

UpdatePanel denetimini kullanırken, geliştiricilerin meydana gelebilecek olası performans etkisini bilmeniz gerekir. Uygulama tasarımını düşünülmesi gereken ancak olası alternatifler web hizmetleri ve sayfa yöntemleri içerir.

Denetim veya kendisine yok sayılıyor ve Perde Arkası istek sırasında sayfa olacağına, aksi takdirde değil bilmek verir UpdateProgress kullanıcı girişi yanıt herhangi bir şey yapılıyor. Ayrıca, kısmi işleme sonuçları abort yeteneğini içerir.

Birlikte, bu araçları zengin ve sorunsuz bir kullanıcı deneyimi server iş kullanıcıya görünen daha az yapma ve daha az iş akışını kesintiye oluşturma yardımcı.

## <a name="bio"></a>Biyografisi

Tan göstermek Microsoft Web teknolojileri ile bu yana 1997 çalışma ve myKB.com Başkanı ise ([www.myKB.com](http://www.myKB.com)) kendisine ASP.NET yazılırken burada uzmanlaşmış tabanlı Bilgi Bankası yazılım çözümlerini odaklanmış uygulamaları. Tan temas kurulabileceğini doğrula e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog adresindeki [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Next](understanding-asp-net-ajax-updatepanel-triggers.md)
