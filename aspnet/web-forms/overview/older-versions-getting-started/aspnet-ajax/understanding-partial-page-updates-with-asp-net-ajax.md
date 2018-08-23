---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: ASP.NET AJAX ile kısmi Sayfa güncelleştirmelerini anlama | Microsoft Docs
author: scottcate
description: Belki de en çok görünen ASP.NET AJAX uzantılarını artımlı ya da kısmi Sayfa güncelleştirmelerini t tam bir geri gönderme yapmadan yapabilmenizi özelliğidir...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 2e0b1e1d4cbb282e7fd4b27e0a93ba1b9702edea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757286"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>ASP.NET AJAX ile kısmi sayfa anlama güncelleştirir
====================
tarafından [Scott Cate](https://github.com/scottcate)

[PDF'yi indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Belki de en çok görünen ASP.NET AJAX uzantılarını artımlı ya da kısmi Sayfa güncelleştirmelerini tam bir geri gönderme sunucuya olmayan kod değişiklikleri ve en az biçimlendirme değişiklikleri yapmadan yapabilmenizi özelliğidir. Avantajları kapsamlı – multimedya (örneğin, Adobe Flash veya Windows Media) durumu değiştirilmez, bant genişliği maliyetleri azalır ve istemci genellikle bir geri gönderme ile ilişkilendirilmiş titreşimini karşılaşmaz.


## <a name="introduction"></a>Giriş

Microsoft ASP.NET teknoloji, nesne yönelimli ve olay odaklı bir programlama modeli sunar ve derlenmiş kod avantajları sahip. Ancak, kendi sunucu tarafı işleme modelini birkaç engelleri teknolojisinde devralınan vardır:

- Sayfa güncelleştirmeleri bir gidiş dönüş gerektiren bir sayfa yenileme sunucuya gerektirir.
- Gidiş dönüş, Javascript veya başka bir istemci-tarafı teknolojisi (örneğin, Adobe Flash) tarafından oluşturulan tüm etkilerinin etmez
- Geri gönderme sırasında Microsoft Internet Explorer dışındaki tarayıcıları otomatik olarak kaydırma konumunu geri desteklemez. Ve bile Internet Explorer'da yine de bir titreşimini gibi sayfa yenilenir.
- Geri göndermeler, yüksek miktarda bant genişliği gerektirebilir \_ \_VIEWSTATE form alanı büyütün, özellikle yineleyiciler ve GridView denetimi gibi denetimler ile ilgilenirken.
- JavaScript veya başka bir istemci-tarafı teknolojisi Web hizmetlerine erişme hiçbir birleşik modeli yoktur.

Microsoft ASP.NET AJAX uzantılarını girin. Anlamına gelir, AJAX **A** zaman uyumlu **J** avaScript **A** nd **X** ML, artımlı sayfa sağlamak için tümleşik bir çerçeve olan platformlar arası JavaScript aracılığıyla güncelleştirmeleri Microsoft AJAX çerçevesi ve Microsoft AJAX komut dosyası kitaplığı adlı bir betik bileşen kapsayan sunucu tarafı kodu oluşur. ASP.NET AJAX uzantılarını ASP.NET Web Hizmetleri JavaScript aracılığıyla erişmek için platformlar arası desteği de sağlar.

Bu teknik incelemede ScriptManager bileşeni ve bir UpdatePanel denetimine UpdateProgress denetimi içerir ve bunlar gerekir veya olan olmamalıdır senaryoları göz önünde bulundurur ASP.NET AJAX uzantılarını, kısmi Sayfa güncelleştirmelerini işlevselliğini inceler. gerçekleştirdiğiniz.

Bu teknik Beta 2 sürümünü Visual Studio 2008 ve temel sınıf kitaplığı (bunu daha önce bir eklenti bileşeni ASP.NET 2.0 için kullanılabilir olduğu) ASP.NET AJAX uzantılarını tümleştirir .NET Framework 3.5 temel alır. Bu teknik incelemede, ayrıca, Visual Studio 2008 ve Visual Web Developer Express sürüm değil kullandığınızı varsayar; Başvurulan bazı proje şablonları, kullanıcıların Visual Web Developer Express kullanılamayabilir.

## <a name="partial-page-updates"></a>Kısmi Sayfa güncelleştirmelerini

Belki de en çok görünen ASP.NET AJAX uzantılarını artımlı ya da kısmi Sayfa güncelleştirmelerini tam bir geri gönderme sunucuya olmayan kod değişiklikleri ve en az biçimlendirme değişiklikleri yapmadan yapabilmenizi özelliğidir. Avantajları kapsamlı - multimedya (örneğin, Adobe Flash veya Windows Media) durumu değiştirilmez, bant genişliği maliyetleri azalır ve istemci genellikle bir geri gönderme ile ilişkilendirilmiş titreşimini karşılaşmaz.

Kısmi sayfa işleme tümleştirme olanağı, projenize çok az değişiklikle ASP.NET ile tümleşiktir.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>İzlenecek yol: Kısmi işleme varolan bir projeye tümleştirme


1. Microsoft Visual Studio 2008'de giderek yeni bir ASP.NET Web sitesi projesi oluşturma <em>dosya</em>  <em>- &gt; yeni</em>  <em>- &gt; Websitesi</em> ve ASP.NET Web sitesi iletişim kutusundan seçme. İstediğiniz gibi adlandırabilirsiniz ve dosya sistemi veya Internet Information Services (IIS) içinde yükleyebilir.
2. Boş bir varsayılan sayfanın temel ASP.NET biçimlendirme ile birlikte sunulur (bir sunucu tarafı formu ve `@Page` yönergesi). Adlı bir etiket bırak `Label1` ve bir düğmeyi adlı `Button1` sayfaya form öğesi içinde. Metin özellikleri dilediğiniz şekilde ayarlayabilir.
3. Tasarım görünümünde, çift `Button1` arka plan kod olay işleyicisi oluşturmak için. Bu olay işleyicisini içinde `Label1.Text` için düğmeye tıkladı! biçimindeki telefon numarasıdır.

**Kod 1: Kısmi işleme etkinleştirmeden önce default.aspx için biçimlendirme**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Kod 2: (default.aspx.cs içinde kırpılmış) Codebehind**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Web sitenizi başlatmak için F5 tuşuna basın. Visual Studio hata ayıklamayı etkinleştirmek için web.config dosyasına eklemek isteyecektir; Bunu yapın. Düğmeye tıkladığınızda, etiket metni değiştirmek için sayfa yenilendikten ve sayfayı yeniden gibi kısa bir titreşimini olan olduğuna dikkat edin.
2. Tarayıcı penceresini kapattıktan sonra Visual Studio ve biçimlendirme sayfasına dönün. Visual Studio araç kutusunda aşağı kaydırın ve AJAX uzantıları etiketli sekmeyi bulun. (AJAX veya Atlas Uzantıları'nın eski bir sürümünü kullandığınızdan Bu sekme yoksa bu teknik incelemede AJAX uzantıları araç kutusu öğelerini kaydetmek için izlenecek bakın veya Windows Installer indirilebilir güncel sürümü yükleme Web sitesinden).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Bilinen sorun:</em>Visual Studio 2008 zaten Visual Studio 2005 ile ASP.NET 2.0 AJAX uzantıları yüklü olduğu bir bilgisayara yüklerseniz, Visual Studio 2008 AJAX uzantıları araç kutusu öğeleri içeri aktaracak. Araç İpucu bileşenlerin inceleyerek bu durumun bu olup olmadığını belirlemek; Bunlar, sürüm 3.5.0.0 yazması gerekir. Sürüm 2.0.0.0 diyor, eski, araç kutusu öğeleri içe aktardıktan sonra bunları Visual Studio araç kutusu öğelerini Seç iletişim kutusunu kullanarak el ile içeri aktarmanız gerekir. Sürüm 2 Denetim Tasarımcısı aracılığıyla eklemek mümkün olmayacaktır.

2. Önce `<asp:Label>` etiketi başlar, çizgi boşluk, oluşturma ve araç UpdatePanel denetimde çift tıklayın. Unutmayın yeni `@Register` yönergesiyse System.Web.UI ad alanı içindeki denetimleri kullanarak içe aktarılması gerekip gerekmediğini belirten sayfanın en üstünde bulunan `asp:` önek.
3. Kapanış sürükleyin `</asp:UpdatePanel>` öğe sarmalanmış etiket ve düğme denetimleri ile iyi biçimlendirilmemiş böylece Button öğesi sonundan etiketi.
4. Açılış `<asp:UpdatePanel>` etiketi, yeni bir etiket açma başlayın. IntelliSense, iki seçenek ister unutmayın. Bu durumda, oluşturun bir `<ContentTemplate>` etiketi. Böylece biçimlendirme iyi biçimlendirilmemiş etiket ve düğmeyi etrafında Bu etiket sarmalama emin olun.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. İçinde herhangi bir yere `<form>` öğe, çift tıklayarak bir ScriptManager denetimi içeren `ScriptManager` araç kutusu öğesi.
2. Düzen `<asp:ScriptManager>` öznitelik içeren etiket `EnablePartialRendering= true`.

**Kod 3: Etkin kısmi işleme ile default.aspx için biçimlendirme**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Web.config dosyanızı açın. Visual Studio System.Web.Extensions.dll derleme başvurusu otomatik olarak eklemiştir dikkat edin.

1. Visual Studio 2008'deki yenilikler: ASP.NET Web sitesi proje şablonlarıyla otomatik olarak gelen web.config ASP.NET AJAX uzantıları için gereken tüm başvurular içerir ve açıklamalı bölümlerini olabilir yapılandırma bilgilerini içerir Ek işlevselliğini etkinleştirmek için açıklamalı kaydetmeyin. ASP.NET 2.0 AJAX uzantıları yüklediğinizde visual Studio 2005 benzer şablonları vardı. Ancak, Visual Studio 2008'de çevirme AJAX uzantıları varsayılan olarak (diğer bir deyişle, bunlar varsayılan olarak başvurulur, ancak başvuru olarak kaldırılabilir).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Web sitenizi başlatmak için F5 tuşuna basın. Nasıl kaynak kod değişikliği yapmadan kısmi işleme desteklemek için gerekli - yalnızca işaretleme değiştirildi unutmayın.

Web sitenizi başlattığında, kısmi işleme artık etkin, tıkladığınızda düğmesi var. hiçbir titreşimini olacaktır ya da sayfa kaydırma konumunu (Bu örnekte, gösterilmemiştir) herhangi bir değişiklik olacak çünkü olduğunu görmeniz gerekir. Düğmesine tıklandıktan sonra sayfanın işlenmiş kaynağında aramak için olsaydı, aslında bir sonrası yedekleme değil oluştu - özgün etiket metni hala kaynak işaretlemesiyle parçasıdır ve etiket JavaScript değişti onaylar.

Visual Studio 2008 ile bir ASP.NET AJAX etkinleştirilmiş web sitesi için önceden tanımlanmış bir şablon gelir görünmüyor. Ancak, Visual Studio 2005 ve ASP.NET 2.0 AJAX uzantıları yüklenmişse içinde Visual Studio 2005 böyle bir şablon yoktu. Şablon (tüm ASP.NET AJAX Web Hizmetleri dahil olmak üzere uzantıları destekleyen tamamen yapılandırılmış web.config dosyası içermelidir. sonuç olarak, web sitesi yapılandırma ve bir Web sitesi AJAX-Enabled şablonla başlayarak büyük olasılıkla daha da kolay olacaktır ve JSON serileştirme - JavaScript nesne gösterimi) ve bir UpdatePanel ve ContentTemplate ana Web Forms sayfası içinde varsayılan olarak içerir. Bu varsayılan sayfası ile kısmi işleme etkinleştirme bu kılavuzun adım 10'da ve denetimleri sayfaya bırakma kadar basittir.

## <a name="the-scriptmanager-control"></a>ScriptManager denetimi

## <a name="scriptmanager-control-reference"></a>ScriptManager denetimi başvurusu

Biçimlendirme etkin özellikler:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| AllowCustomErrors-yeniden yönlendirme | bool | Özel hata bölümü web.config dosyasının hataları işlemek için kullanılıp kullanılmayacağını belirtir. |
| AsyncPostBackError iletisi | Dize | Alır veya ayarlar bir hata oluşturulursa istemciye gönderilen hata iletisi. |
| AsyncPostBack zaman aşımı | Int32 | Alır veya bir istemci tamamlamak zaman uyumsuz istek için beklemesi gereken süreyi varsayılan süreyi ayarlar. |
| ENABLESCRIPT Genelleştirme | bool | Alır veya komut dosyası Genelleştirme etkinleştirilip etkinleştirilmediğini ayarlar. |
| ENABLESCRIPT yerelleştirme | bool | Alır veya betik yerelleştirme etkinleştirilip etkinleştirilmediğini ayarlar. |
| ScriptLoadTimeout | Int32 | Komut istemcisini yüklemek için izin verilen saniye sayısını belirler. |
| ScriptMode | Enum (Auto, hata ayıklama, yayın, devralır) | Alır veya yayın sürümleri komut dosyası oluşturulup oluşturulmayacağını belirler |
| ScriptPath | Dize | Alır veya kök yolu istemciye gönderilecek komut dosyalarının konumunu ayarlar. |

Yalnızca kod özellikleri:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService Yöneticisi | İstemciye gönderilecek ASP.NET kimlik doğrulama hizmeti proxy ayrıntılarını alır. |
| IsDebuggingEnabled | bool | Olup olmadığını alır betik ve kod hata ayıklaması etkinleştirildi. |
| IsInAsyncPostback | bool | Sayfa şu anda içinde zaman uyumsuz geri gönderme isteği olup olmadığını alır. |
| ProfileService | ProfileService Yöneticisi | İstemciye gönderilecek ASP.NET Profil Hizmeti proxy ayrıntılarını alır. |
| Komut dosyaları | Koleksiyon&lt;betik başvurusu&gt; | Bir istemciye gönderilecek bir komut dosyası başvuruları koleksiyonunu alır. |
| Hizmetler | Koleksiyon&lt;hizmet başvurusu&gt; | Bir istemciye gönderilecek Web hizmeti proxy başvuruları koleksiyonunu alır. |
| Pokud je | bool | Geçerli istemci kısmi işleme destekleyip desteklemediğini alır. Bu özellik döndürürse **false**, tüm sayfa istekleri standart Geri göndermeler olmayacaktır. |

Ortak kod yöntemleri:

| **Yöntem adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| SetFocus(string) | Geçersiz kılma | İstek tamamlandıktan sonra istemcinin odağı özel bir denetime ayarlar. |

Biçimlendirme alt öğeleri:

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;AuthenticationService&gt; | ASP.NET kimlik doğrulama hizmeti için proxy hakkında ayrıntılar sağlar. |
| &lt;ProfileService&gt; | Proxy ayrıntılarını service profil ASP.NET'i sağlar. |
| &lt;Komut dosyaları&gt; | Ek komut dosyası başvuruları sağlar. |
| &lt;asp:ScriptReference&gt; | Bir özel betik başvurusu gösterir. |
| &lt;Hizmet&gt; | Oluşturulan proxy sınıflar olan ek Web hizmeti başvuruları sağlar. |
| &lt;ASP: ServiceReference&gt; | Belirli bir Web hizmeti başvurusu gösterir. |

ScriptManager, ASP.NET AJAX uzantıları için temel çekirdek denetimidir. Kod kitaplığı (kapsamlı istemci tarafı komut dosyası tür sistemine dahil) erişim sağlar, kısmi işleme destekler ve ek ASP.NET Hizmetleri (örneğin, kimlik doğrulaması ve profil oluşturma, aynı zamanda diğer Web hizmetlerini) için kapsamlı destek sağlar. ScriptManager denetimini Genelleştirme ve yerelleştirme için istemci betiklerini desteği de sağlar.

## <a name="providing-alterative-and-supplemental-scripts"></a>Diğer ve ek betikleri sağlama

Microsoft ASP.NET 2.0 AJAX uzantıları hem hata ayıklama tüm betik kodu içerebilir ve başvurulan bütünleştirilmiş kod içinde gömülü kaynaklar. sürümleri yayın olsa da geliştiriciler yanı sıra ScriptManager yeniden yönlendirmek için özelleştirilmiş komut dosyaları kaydetmek ücretsiz Ek gerekli betikler.

Tipik olarak bulunan komut dosyaları (örneğin Sys.WebForms ad alanı ve özel yazı sistemi destekleyen) için varsayılan bağlama geçersiz kılmak için kaydolabilirsiniz `ResolveScriptReference` olay ScriptManager sınıf. Bu yöntem çağrıldığında, olay işleyicisi, söz konusu komut dosyası yolunu değiştirmek için fırsatına sahip; komut Yöneticisi'ni, sonra komut dosyalarını farklı veya özelleştirilmiş bir kopyasını istemciye gönderir.

Ayrıca, komut dosyası başvuruları (tarafından temsil edilen `ScriptReference` sınıfı) programlama yoluyla veya biçimlendirme dahil edilebilir. Bunu yapmak için ya da program aracılığıyla değiştirebilme `ScriptManager.Scripts` koleksiyonu veya dahil `<asp:ScriptReference>` altında etiketleri `<Scripts>` ScriptManager denetimini birinci düzey alt etiketi.

## <a name="custom-error-handling-for-updatepanels"></a>Özel hata için UpdatePanels işleme

Hata işleme ve özel hata iletileri için destek, güncelleştirmeleri UpdatePanel denetimleri tarafından belirtilen tetikleyiciler tarafından işlenmiş olsa da, sayfanın ScriptManager denetimi örneği tarafından işlenir. Bir olay göstererek yapıldığını `AsyncPostBackError`, alabilen sayfasına sonra özel bir özel durum işleme mantığı sağlar.

AsyncPostBackError olay tüketen tarafından belirttiğiniz `AsyncPostBackErrorMessage` özelliği daha sonra geri çağırma tamamlanmasından sonra oluşturulacak bir uyarı kutusu neden olur.

İstemci tarafı özelleştirmesi, varsayılan uyarı kutusunu kullanmak yerine mümkündür; Örneğin, özelleştirilmiş görüntülemek isteyebilirsiniz `<div>` varsayılan tarayıcı kalıcı iletişim kutusu yerine öğesi. Bu durumda, istemci komut dosyası hata işleyebilir:

**5 listeleniyor: özel hataları görüntülemek için istemci tarafı betiği**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Basitçe açıklamak, zaman uyumsuz istek tamamlandıktan sonra yukarıdaki betik için istemci tarafı AJAX çalışma zamanı ile bir geri çağırma kaydeder. Ardından bir hata rapor edildi olup olmadığını denetler ve bu durumda, son olarak çalışma zamanının özel betikte hata işlendiğini belirten ayrıntılarını işler.

## <a name="globalization-and-localization-support"></a>Genelleştirme ve yerelleştirme desteği

ScriptManager denetimini yerelleştirme betik dizeleri ve kullanıcı arabirimi bileşenleri için kapsamlı destek sağlar; Ancak, bu konuda, bu teknik incelemede kapsamı dışında olur. Teknik İncelemesi, Genelleştirme destek ASP.NET AJAX Uzantıları'nda daha fazla bilgi için bkz.

## <a name="the-updatepanel-control"></a>Bir UpdatePanel denetimine

## <a name="updatepanel-control-reference"></a>UpdatePanel denetim başvurusu

Biçimlendirme etkin özellikler:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| Na | bool | Alt denetimler otomatik geri gönderme yenileme çağırma belirtir. |
| RenderMode | Enum (blok, satır içi) | İçerik yolu görsel olarak sunulur belirtir. |
| UpdateMode | Enum (her zaman, koşullu) | UpdatePanel kısmi işleme sırasında her zaman yenilenip yenilenmeyeceğini ya da bir tetikleyici isabet edildiğinde yalnızca yenileniyor, belirtir. |

Yalnızca kod özellikleri:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| IsInPartialRendering | bool | UpdatePanel geçerli istek için kısmi işleme destekleyen olup olmadığını alır. |
| ContentTemplate | ITemplate | Biçimlendirme şablonu güncelleştirme isteği alır. |
| ContentTemplateContainer | Denetim | Programlı şablonu güncelleştirme isteği alır. |
| Tetikleyiciler | UpdatePanel - TriggerCollection | Geçerli UpdatePanel ile ilişkili Tetikleyicileri listesini alır. |

Ortak kod yöntemleri:

| **Yöntem adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| Update() | Geçersiz kılma | Belirtilen UpdatePanel programlı olarak güncelleştirir. Kısmi bir işleme, aksi takdirde untriggered UpdatePanel tetiklemek sunucu isteği sağlar. |

Biçimlendirme alt öğeleri:

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;ContentTemplate&gt; | Kısmi işleme sonucu işlemek için kullanılacak biçimlendirmeyi belirtir. Alt &lt;asp: UpdatePanel&gt;. |
| &lt;Tetikleyiciler&gt; | Bir koleksiyonu belirtir *n* bu UpdatePanel güncelleştirme ile ilişkili denetimler. Alt &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Kısmi sayfa işleme için verilen UpdatePanel çağıran bir tetikleyici belirtir. Bu olabilir veya bir denetim UpdatePanel söz konusu alt öğesi olmayabilir. Ayrıntılı olay adı. Alt &lt;Tetikleyicileri&gt;. |
| &lt;asp:PostBackTrigger&gt; | Tüm sayfayı yenilemek neden denetimi belirtir. Bu olabilir veya bir denetim UpdatePanel söz konusu alt öğesi olmayabilir. Ayrıntılı nesne. Alt &lt;Tetikleyicileri&gt;. |

`UpdatePanel` Bölümü AJAX uzantıları kısmi işleme işlevselliğini alacağı sunucu tarafı içeriği sınırlandıran denetimi bir denetimdir. Bir sayfada olabilir UpdatePanel denetimleri sayısına bir sınır yoktur ve bunlar yuvalanabilir. Her bağımsız olarak çalışabilir, böylece her UpdatePanel yalıtılır (aynı anda çalışan iki UpdatePanels sayfasının farklı bölümlerini bağımsız olarak sayfa geri gönderme işleme sahip olabilir).

UpdatePanel öncelikle anlaşmalar denetim tetikleyicilerle - varsayılan olarak, UpdatePanel içinde 's yer alan herhangi bir denetimi kontrol `ContentTemplate` bir geri gönderme oluşturan UpdatePanel için tetikleyici olarak kaydedilir. UpdatePanel varsayılan verilere bağlı denetimler (örneğin, GridView), kullanıcı denetimleri ile birlikte çalışabilir ve betikte programlanabilir anlamına gelir.

Varsayılan olarak, kısmi sayfa işleme tetiklendiğinde, sayfadaki tüm UpdatePanel denetimleri yenilenir, bu tür bir eylemin Tetikleyicileri tanımlı UpdatePanel olup olmadığını denetler. Örneğin, bir düğme denetimi bir UpdatePanel tanımlar ve bu düğme denetimi tıklandığında, varsayılan olarak bu sayfadaki tüm UpdatePanel denetimleri yenilenir. Bunun nedeni, varsayılan olarak, `UpdateMode` özelliği prvku UpdatePanel `Always`. Alternatif olarak, UpdateMode özelliği ayarlayın `Conditional`, belirli bir tetikleyici ulaşılırsa UpdatePanel yalnızca yenilenir, anlamına gelir.

## <a name="custom-control-notes"></a>Özel denetim notları

UpdatePanel herhangi bir kullanıcı denetimi veya özel denetim eklenebilir; Ancak üzerinde bu denetimleri dahil edilir sayfada bir ScriptManager denetimi Supportspartialrendering ayarlanan özelliği ile de bulunmalıdır **true**.

Web özel denetimleri kullanarak korumalı geçersiz kılmak için olduğunda, tek yönlü, bu hesabı `CreateChildControls()` yöntemi `CompositeControl` sınıfı. Bunu yaptığınızda, denetimin alt öğeleri ve dış dünya arasındaki UpdatePanel sayfanın kısmi işleme desteklediği belirlerseniz ekleyebilir; Aksi takdirde, yalnızca alt denetimlerin bir kapsayıcıya katmanlayabilirsiniz `Control` örneği.

## <a name="updatepanel-considerations"></a>UpdatePanel konuları

UpdatePanel şey bir kara kutu, sarmalama ASP.NET Geri göndermeler bir JavaScript XMLHttpRequest bağlamında çalışır. Ancak, unutmayın, hem davranış ve hızı açısından işlenmesi için önemli performans konuları mevcuttur. Kullanımını uygun olduğunda en iyi karar verebilir böylece UpdatePanel nasıl çalıştığını anlamak için AJAX exchange incelemeniz gerekir. Aşağıdaki örnek bir var olan site ve, Mozilla Firefox (Firebug XMLHttpRequest verileri yakalar) Firebug uzantısıyla kullanır.

Başka şeylerin yanında, bir form veya denetim city ve state alanı doldurmak için beklenen bir posta kodu metin kutusu olan bir form göz önünde bulundurun. Bu formu Sonuçta bir kullanıcı adı, adresi ve kişi bilgilerini dahil olmak üzere, üyelik bilgilerini toplar. Belirli bir proje gereksinimlerine göre dikkate almak için birçok tasarım konuları mevcuttur.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


Bu uygulamanın orijinal yinelemede bir denetim, posta kodu, şehir ve durumu da dahil olmak üzere kullanıcı kayıt verilerini tamamen dahil oluşturulmuştur. Tüm denetim içinde UpdatePanel sarmalandı ve Web formunun üzerine bırakıldı. Posta kodu, kullanıcı tarafından girilen UpdatePanel olay (arka uç, Tetikleyiciler belirterek ya da true olarak ayarlanan na özelliği kullanarak, karşılık gelen TextChanged olayını) algılar. AJAX gönderir, tüm alanları UpdatePanel içinde FireBug tarafından yakalanan gibi (diyagrama sağ tarafta bakın).

Ekran yakalamayı da anlaşılacağı gibi her bir UpdatePanel denetimine değerlerinden teslim edilir (Bu durumda, tüm boş oldukları), Görünüm durumu alanı yanı sıra. Aslında yalnızca beş bayt veri bu belirli bir istekte bulunmak için gerekli olan tüm told üzerinde 9 kb veri, gönderilir. Yanıt daha da bloated: Toplam, yalnızca bir metin alanı ve bir açılan alan güncelleştirmek için istemciye 57kb gönderilir.

ASP.NET AJAX sunu nasıl yansıdığını göreceğiz ilgi de olabilir. UpdatePanel'ın güncelleştirme isteğine yanıt bölümü soldaki Firebug konsol ekran gösterilir; istemci komut dosyası tarafından bölünür ve sayfada daha sonra yeniden bir özel şeklide kanal ayrılmış dizedir. Özellikle, ASP.NET AJAX ayarlar *innerHTML* özelliği, UpdatePanel temsil eden istemcideki HTML öğesi. Tarayıcı DOM yeniden oluştururken, işlenmesi gereken bilgi miktarına bağlı olarak biraz gecikme yoktur.

DOM anahtarınızın yeniden oluşturulması birkaç ek sorun tetikleyen:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Odaklanmış HTML öğesi içinde UpdatePanel ise, odağı kaybeder. Bu nedenle, posta kodu metin kutusundan çıkmak için SEKME tuşunu basılı olan kullanıcılar için sonraki hedeflerine Şehir metin kutusu olacaktı. Ancak, UpdatePanel ekranı yenilendi sonra form artık odak zorunda kalacaktır ve SEKME tuşuna basarak odağı öğelerini (örneğin, bağlantıları) vurgulama başlamış olması.
- Özel istemci tarafı komut dosyası herhangi bir türde erişimleri DOM öğeleri, başvuruları işlevleri tarafından kalıcı kullanılıyorsa kısmi bir geri gönderme sonra geçersiz hale gelebilir.

UpdatePanels catch tüm çözümler için tasarlanmamıştır. Bunun yerine, hızlı bir çözüm, belirli durumlarda, prototip oluşturma, Denetim küçük güncelleştirmeler dahil olmak üzere sağlar ve .NET nesne modeline alışkın ancak için daha az DOM ile ASP.NET geliştiricileri için tanıdık bir arabirim sağlayın Uygulama senaryosuna bağlı olarak daha iyi performans sonuçlanabilir alternatifleri vardır:

- PageMethods kullanmayı göz önünde bulundurun ve bir web hizmeti çağrısı çağrıldı gibi bir sayfa üzerinde statik yöntemlerini çağırmak Geliştirici JSON (JavaScript nesne gösterimi) sağlar. Yöntemleri statik olduğundan, hiçbir durum gereklidir. komut dosyasını çağırıcı parametreleri sağlar ve sonuç zaman uyumsuz olarak döndürülür.
- Bir uygulamanın tamamında çeşitli yerlerde kullanılacak tek bir denetim gerekiyorsa, bir Web hizmeti ve JSON kullanmayı düşünün. Bu yeniden özel çok az çalışma gerektirir ve zaman uyumsuz olarak çalışır.

Web Hizmetleri veya sayfa yöntemleri aracılığıyla işlevsellik ekleme de dezavantajları vardır. İlk ve, ASP.NET geliştiricilerine işlevselliği küçük bileşenlerden kullanıcı denetimi (.ascx dosyaları) oluşturmak için genellikle eğilimi gösterir. Sayfa yöntemleri bu dosyalarda barındırılamaz; Bunlar gerçek .aspx sayfası sınıfı içinde barındırılması gerekir. Web Hizmetleri .asmx sınıfla benzer şekilde, barındırılması gerekir. Tek bir bileşen için işlevsellik şimdi çok az kayıpla veya hiç cohesive TIES sahip olabilecek iki veya daha fazla fiziksel bileşenler arasında yayılır, uygulamaya bağlı olarak, tek sorumluluk ilkesini bu mimari neden olabilir.

Son olarak, bir uygulama UpdatePanels kullanılmasını gerektiriyorsa, aşağıdaki yönergeler sorun giderme ve Bakım ile yardımcı olmalıdır.

- **UpdatePanels mümkün olduğunca az yalnızca içinde-birimleri, aynı zamanda kod birimleri arasında iç içe.** Örneğin, bir denetimin, denetimin UpdatePanel içeren başka bir denetimi içeren bir UpdatePanel içerirken sarmalayan bir sayfada UpdatePanel sahip, çapraz birim iç içe istenir. Bu açık tutmak için hangi öğelerin yenileme olmalıdır ve alt UpdatePanels beklenmeyen yenilemeleri engeller yardımcı olur.
- **Tutun *na* özelliği false olarak ayarlayın ve olayları tetikleme açıkça ayarlayın.** Yararlanarak `<Triggers>` toplama olayları işlemek için çok daha net bir yoldur ve bakım görevleri ile yardımcı ve kabul etmek için bir olay için bir geliştirici zorlama beklenmeyen davranışı engelliyor olabilir.
- **Olası en küçük birim işlevselliği elde etmek için kullanın.** Yalnızca en az sunucu, toplam işleme ve istemci-sunucu exchange ayak izine süresini azaltır sarmalama posta kodu hizmetinin içindeki tartışmalara belirtildiği gibi performans geliştirme.

## <a name="the-updateprogress-control"></a>UpdateProgress denetimi

## <a name="updateprogress-control-reference"></a>UpdateProgress denetimi başvurusu

Biçimlendirme etkin özellikler:

| **Özellik adı** | **Türü** | **Açıklama** |
| --- | --- | --- |
| AssociatedUpdate PanelID | Dize | Bu UpdateProgress üzerinde bildirmelisiniz UpdatePanel Kimliğini belirtir. |
| DisplayAfter | int | Bu denetim, zaman uyumsuz istek başladıktan sonra görüntülenmeden önce zaman aşımını milisaniye cinsinden belirtir. |
| DynamicLayout | bool | İlerleme dinamik olarak oluşturulup oluşturulmayacağını belirtir. |

Biçimlendirme alt öğeleri:

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Bu denetimle görüntülenecek içeriği için denetim şablonu içerir. |

UpdateProgress denetimi, bir ölçü bir sunucuya taşıma için gerekli iş yaparken, kullanıcılarınızın ilgi tutmak için geri bildirim sağlar. Bu, özellikle çoğu kullanıcının yenileme ve vurgulama durum çubuğu görmeye sayfasına kullanıldığından, belirgin, olmayabilir rağmen bir şey yaptığınızı olduğunu bilmek, kullanıcılarınızın yardımcı olabilir.

Not olarak UpdateProgress denetimleri herhangi bir sayfa hiyerarşisini görünebilir. Ancak, kısmi bir geri gönderme (burada UpdatePanel başka bir UpdatePanel içinde yer alan) UpdatePanel alt başlatıldığı durumlarda, bu tetikleyici UpdatePanel için alt öğe görüntülenecek şablonları UpdateProgress açacak alt postbacks UpdatePanel yanı sıra UpdatePanel üst. Ancak, tetikleyici UpdatePanel üst öğesinin doğrudan alt öğesi ise, yalnızca üst öğesi ile ilişkili UpdateProgress şablonlar görüntülenecektir.

## <a name="summary"></a>Özet

Microsoft ASP.NET AJAX web içeriği daha erişilebilir hale yardımcı olmak ve web uygulamalarınız için daha zengin bir kullanıcı deneyimi sağlamak için tasarlanan gelişmiş ürünleri uzantılarıdır. Kısmi sayfa işleme denetimleri, ASP.NET AJAX uzantılarını bir parçası olarak ScriptManager, UpdatePanel ve UpdateProgress denetimleri de dahil olmak üzere en çok görünen araç seti bileşenlerinin bazılarıdır.

ScriptManager bileşeni istemci JavaScript hazırlama uzantıları için tümleşir yanı en az geliştirme yatırım ile birlikte çalışmak çeşitli sunucu ve istemci tarafı bileşenleri sağlar.

UpdatePanel denetimine görünen Sihirli kutusudur - biçimlendirme UpdatePanel içinde sunucu tarafı Codebehind olması ve sayfa yenileme tetiklemek değil. UpdatePanel denetimleri, iç içe olabilir ve diğer UpdatePanels denetimlerinde bağımlı olabilir. Bu işlev ince, bildirimli olarak veya programlama yoluyla ayarlanabilecek olsa da varsayılan olarak, kendi alt denetimler tarafından çağrılan bir Geri göndermeler UpdatePanels işleyin.

Bir UpdatePanel denetimine kullanırken, geliştiricilerin meydana gelebilecek olası performans etkisini bilmeniz gerekir. Uygulama tasarımını düşünülmesi gereken ancak olası alternatifler web hizmetleri ve sayfa yöntemleri içerir.

UpdateProgress denetimi yaptığı veya yok sayılıyor ve gelen istek sırasında sayfa bittiğini aksi değil bilmesini sağlar, kullanıcı girişi için yanıt vermek için herhangi bir şey yapmak. Ayrıca, kısmi sonuçları iptal özelliği içerir.

Birlikte, bu araçlar server çalışma kullanıcıya daha az belirgin hale getirme ve daha az iş akışı kesintiye bir zengin ve sorunsuz bir kullanıcı deneyimi oluşturmaya yardımcı olur.

## <a name="bio"></a>Biyografi

Scott Cate 1997'den beri Microsoft Web teknolojileri ile çalışmakta olduğu ve myKB.com Yardımcısı ([www.myKB.com](http://www.myKB.com)) tabanlı Bilgi Bankası yazılım çözümlerinizi odaklı uygulamaları burada kendisinin ASP.NET yazma konusunda uzmanlaşmış. Scott temas kurulabileceğini e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog'da [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Next](understanding-asp-net-ajax-updatepanel-triggers.md)
