---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: ASP.NET AJAX UpdatePanel tetikleyicilerini anlama | Microsoft Docs
author: scottcate
description: Visual Studio biçimlendirme düzenleyicide çalışırken (IntelliSense'de) iki alt öğelerinin bir UpdatePanel denetimine olduğunu fark edebilirsiniz. Wh birini...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 9501a2e855bdffe8c9d85c0dd0d836f50935b306
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755583"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>ASP.NET AJAX UpdatePanel tetikleyicilerini anlama
====================
tarafından [Scott Cate](https://github.com/scottcate)

[PDF'yi indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Visual Studio biçimlendirme düzenleyicide çalışırken (IntelliSense'de) iki alt öğelerinin bir UpdatePanel denetimine olduğunu fark edebilirsiniz. Biri olan (veya bir kullanıyorsanız kullanıcı denetimi) sayfasındaki denetimleri belirtir Tetikleyicileri öğesi, öğenin bulunduğu bir UpdatePanel denetimine, kısmi bir işleme tetikleme.


## <a name="introduction"></a>Giriş

Microsoft ASP.NET teknoloji, nesne yönelimli ve olay odaklı bir programlama modeli sunar ve derlenmiş kod avantajları sahip. Ancak, kendi sunucu tarafı işleme modelini birkaç engelleri çoğu Microsoft ASP.NET 3.5 AJAX uzantılarında bulunan yeni özellikler çözülebilir teknolojisindeki devralınan vardır. Bu uzantıları istemci komut dosyası (profil oluşturma API'si ASP.NET dahil olmak üzere) ve kapsamlı bir istemci tarafı API Web hizmetlerine erişme olanağını tam sayfa yenileme gerek kalmadan sayfaların kısmi işleme dahil olmak üzere, birçok yeni zengin istemci özelliklerini etkinleştirme ASP.NET sunucu tarafı denetim kümesinde görüldüğü denetim düzenleri birçoğu yansıtmak üzere tasarlanmıştır.

Bu teknik incelemede ASP.NET AJAX XML Tetikleyicileri işlevselliğini inceler `UpdatePanel` bileşeni. XML tetikleyicileri, belirli UpdatePanel denetimleri için kısmi işleme neden olabilecek bileşenleri üzerinde ayrıntılı denetim sağlar.

Bu teknik incelemede, Beta 2 sürümü .NET Framework 3.5 ve Visual Studio 2008 bağlıdır. ASP.NET AJAX uzantılarını, daha önce ASP.NET 2.0, hedeflenen bir eklenti derleme artık .NET Framework temel sınıf kitaplığı ile Tümleştirildi. Bu Teknik İnceleme de varsayar Visual Studio 2008 ile değil Visual Web Developer Express, çalışma ve (kod listeleri tamamen uyumlu açmamasından olsa da Visual Studio kullanıcı arabiriminin göre izlenecek yollar sağlar geliştirme ortamı).

## <a name="triggers"></a>*Tetikleyiciler*

Varsayılan olarak, belirli bir UpdatePanel tetikleyicilerini otomatik olarak (örneğin) olan TextBox denetimleri de dahil olmak üzere bir geri çağırma tüm alt denetimler içeren kendi `AutoPostBack` özelliğini **true**. Ancak, Tetikleyiciler ayrıca bildirimli olarak işaretleme kullanarak dahil olabilir; Bunun `<triggers>` bölümünü UpdatePanel denetim bildirimi. Tetikleyicileri aracılığıyla erişilebilir rağmen `Triggers` koleksiyon özelliği (örneğin, bir denetimi tasarım zamanında kullanılabilir değilse), çalışma zamanında hiçbir kısmi işleme tetikleyici kaydetmeniz önerilir kullanarak `RegisterAsyncPostBackControl(Control)` yöntemi ScriptManager sayfanız için içinde nesne `Page_Load` olay. Sayfaları durum bilgisiz olduğundan ve oluşturulan her zaman bu nedenle bu denetimleri yeniden kayıt işlemi olduğunu unutmayın.

Otomatik alt tetikleyici ekleme de devre dışı bırakılabilir (Geri göndermeler oluşturma alt denetimleri otomatik olarak kısmi işler tetiklemez böylece) ayarlayarak `ChildrenAsTriggers` özelliğini **false**. Hangi özel denetimlerin atanmasında en büyük esnekliği sayfa işleme çağırabilir ve önerilen böylece bir geliştirici bir olay yerine kaynaklanabilecek herhangi bir olayı işleme yanıt katılımı böylece.

UpdatePanel denetimleri iç içe geçmiş zaman zaman UpdateMode değerine ayarlandığını unutmayın **koşullu**, alt UpdatePanel tetiklenir, ancak üst sonra yalnızca updatepanel'i alt, değilse. UpdatePanel üst yenilenmiş olması durumunda, ancak ardından alt UpdatePanel ayrıca yenilenecek.

## <a name="the-lttriggersgt-element"></a>*&lt;Tetikleyicileri&gt; öğesi*

Visual Studio biçimlendirme düzenleyicide çalışırken, iki alt öğe (IntelliSense'de) görebilirsiniz, bir `UpdatePanel` denetimi. En sık görülen öğe `<ContentTemplate>` temelde güncelleme paneli tarafından tutulan içeriği yalıtan öğesi (kendisi için etkinleştirdik kısmi işleme içeriği). Diğer öğe `<Triggers>` (veya bir kullanıyorsanız kullanıcı denetimi) sayfasındaki denetimleri belirtir öğesi, kısmi bir işleme, UpdatePanel denetimin tetikleme &lt;Tetikleyicileri&gt; öğesi bulunur.

`<Triggers>` Öğesi herhangi bir sayıda her iki alt düğüm içerebilir: `<asp:AsyncPostBackTrigger>` ve `<asp:PostBackTrigger>`. Her ikisi de iki öznitelikleri kabul `ControlID` ve `EventName`ve geçerli birim kapsülleme içinde herhangi bir denetimi belirtebilirsiniz (UpdatePanel denetiminizi Web kullanıcı denetimi içinde yer alıyorsa, örneğin, üzerinde bir denetime başvurmak denememeniz gerekir Kullanıcı denetimi yer alacağı sayfası).

`<asp:AsyncPostBackTrigger>` Herhangi bir alt öğesi olarak var olan bir denetim olayı hedefleyebilir, öğe özellikle yararlı *herhangi* UpdatePanel denetimine kapsülleme, yalnızca bu tetikleyicinin altında bir alt olan UpdatePanel birimi . Bu nedenle, kısmi sayfa güncelleştirmesi tetiklemek için herhangi bir denetimi yapılabilir.

Benzer şekilde, `<asp:PostBackTrigger>` öğesi, bir kısmi sayfa işleme tetikleyicisi, ancak sunucu için tam bir gidiş dönüş gerektiren bir için kullanılabilir. Bu tetikleyici öğesi bir denetim Aksi halde normal bir kısmi sayfa işleme tetikleyecek bir tam sayfada işleme zorlamak için de kullanılabilir (olduğunda, örneğin, bir `Button` denetimi olduğunu `<ContentTemplate>` bir UpdatePanel denetimine öğesidir). Yeniden PostBackTrigger öğesi herhangi bir UpdatePanel denetimine geçerli birim kapsülleme içindeki alt herhangi bir denetime belirtebilirsiniz.

## <a name="lttriggersgt-element-reference"></a>*&lt;Tetikleyiciler&gt; öğe başvurusu*

*Biçimlendirme alt öğeleri:*

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Denetim ve kısmi sayfa güncelleştirmesi Bu tetikleyici başvuru içeren UpdatePanel için neden olacak bir olay belirtir. |
| &lt;asp:PostBackTrigger&gt; | Bir denetimi ve tam sayfada güncelleştirme (tam sayfa yenileme) neden olan olayı belirtir. Bu etiket, Denetim, aksi takdirde kısmi işleme tetikleyecek tam yenilemeye zorlamak için kullanılabilir. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*İzlenecek yol: Arası UpdatePanel tetikleyicilerini*

1. Kısmi işleme etkinleştirmek için ayarlanmış bir ScriptManager nesnesi ile yeni bir ASP.NET sayfası oluşturun. Bu sayfada - ilk iki UpdatePanels eklemek, bir etiket denetimi (Label1) ve iki düğme denetimi (Button1 ve Button2) içerir. Button1 hem güncelleştirmeye tıklayın yazması gerekir ve bu veya bu satırlar boyunca bir şey güncelleştirmek için tıklayın Button2 yazması gerekir. İkinci UpdatePanel bir etiket denetimi etiket (2) yalnızca içerir, ancak bunu ayırt etmek için varsayılan dışında bir şey kendi ForeColor özelliği ayarlayın.
2. Her iki UpdatePanel etiketleri UpdateMode özelliğini **koşullu**.

**Kod 1: Default.aspx için biçimlendirme:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Button1 için tıklama olay işleyicisine bir şey (DateTime.Now.ToLongTimeString()). gibi zamana bağımlı Label1.Text ve Label2.Text ayarlayın Tıklama olay işleyicisine Button2 için yalnızca Label1.Text zamana bağımlı değerine ayarlayın.

**Kod 2: Codebehind: (default.aspx.cs içinde kırpılmış)** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Derleme ve projeyi çalıştırmak için F5 tuşuna basın. Güncelleştirme hem panelleri tıkladığınızda, iki etiket metni değiştirmek, dikkat edin. Bu güncelleme paneli tıkladığınızda, ancak yalnızca Label1 güncelleştirir.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Başlık altında*

Yeni oluşturulan örneği kullanarak, ASP.NET AJAX ne yaptığını ve bizim UpdatePanel arası panel Tetikleyicileri nasıl göz alabilir. Oluşturulan sayfa kaynağı HTML yanı sıra FireBug - onunla adlı Mozilla Firefox uzantısı ile çalışacağız Bunu yapmak için biz AJAX Geri göndermeler kolayca inceleyebilirsiniz. Ayrıca .NET Reflector araç tarafından Lutz Roeder kullanacağız. Bu araçların her ikisi de çevrimiçi ücretsiz olarak kullanılabilir ve bir internet arama ile bulunabilir.

Sayfa kaynak kod incelemesini neredeyse sıra dışı bir şey gösterir. UpdatePanel denetimleri olarak işlenen `<div>` kapsayıcılar ve biz görebilirsiniz komut dosyası kaynak, sağlanan içerir tarafından `<asp:ScriptManager>`. AJAX istemci betiği kitaplığa iç bazı yeni özel AJAX çağrıları PageRequestManager vardır. Son olarak, iki UpdatePanel kapsayıcı - bir işlenen görüyoruz `<input>` iki düğmeleriyle `<asp:Label>` denetimleri işlenen olarak `<span>` kapsayıcıları. (FireBug DOM ağacında inceleyin, bunlar görünür içeriğe oluşturduğunu değil olduğunu belirtmek için etiketler soluklaştırılır görürsünüz).

Güncelleştirme bu paneli düğmesine tıklayın ve geçerli sunucu saati ile üst UpdatePanel güncelleştirilecek dikkat edin. FireBug içinde böylece istek inceleyebilir Konsolu sekmesini seçin. İlk gönderme isteği parametreleri inceleyin:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


UpdatePanel için sunucu tarafı AJAX kodu tam olarak hangi denetim ağacını ScriptManager1 parametresi ile harekete geçirildi belirtti Not: `Button1` , `UpdatePanel1` denetimi. Şimdi, güncelleştirme iki panel düğmesine tıklayın. Ardından, yanıt inceleme, bir dize ayarlanan değişkenler kanal ayrılmış bir dizi görüyoruz; Özellikle, üst UpdatePanel görüyoruz `UpdatePanel1`, tarayıcıya gönderilen, HTML tamamen sahiptir. AJAX istemci komut dosyası kitaplığı UpdatePanel'ın özgün HTML yeni içerikle içeriğini değiştirir `.innerHTML` özellik ve sunucu, HTML olarak sunucudan ve değiştirilen içerik gönderir.

Şimdi, güncelleştirme iki panel düğmesine tıklayın ve sunucudan sonuçları inceleyin. Sonuçları çok benzer - hem UpdatePanels yeni HTML sunucudan alma. Önceki geri araması ile gibi ek bir sayfa durumu gönderilir.

Özel kod için bir AJAX geri gönderme gerçekleştirmek için kullanılır çünkü biz, görebileceğiniz gibi AJAX istemci komut dosyası kitaplığı herhangi bir ek kod olmadan form Geri göndermeler ıntercept kuramıyor. Sunucu denetimleri otomatik olarak JavaScript kullanan böylece otomatik olarak formun göndermeyen - ASP.NET form doğrulaması ve zaten durumu için öncelikle otomatik komut dosyası kaynak ekleme PostBackOptions sınıfı elde edilen kod otomatik olarak ekler. ve ClientScriptManager sınıfı.

Örneğin, bir onay kutusu denetimi göz önünde bulundurun; .NET Reflector içinde sınıf ayrıştırılmış kodu inceleyin. Bunu yapmak için System.Web derlemesine açık olduğundan emin olun ve gidin `System.Web.UI.WebControls.CheckBox` açma sınıfı `RenderInputTag` yöntemi. Denetleyen bir koşul için konum `AutoPostBack` özelliği:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Üzerinde otomatik geri gönderme etkinleştirildiğinde bir `CheckBox` (true olan AutoPostBack), sonuç denetim özelliği `<input>` etiketi betikte işleme ASP.NET olaylı işlenen bu nedenle kendi `onclick` özniteliği. Form gönderme, daha sonra ele geçirilmesini ASP.NET AJAX'ın sayfalarına nonintrusively, eklenmiş olası herhangi bir büyük olasılıkla kesin olmayan bir dize değiştirme yararlanarak oluşabilecek değişiklikler önlemeye yardımcı sağlar. Ayrıca, böylece *herhangi* UpdatePanel kapsayıcı içindeki kullanımını desteklemek için ASP.NET AJAX gücünü herhangi bir ek kod olmadan kullanmak için özel ASP.NET denetimi.

`<triggers>` İşlevselliği karşılık gelen değerlere PageRequestManager çağrısında başlatıldı \_updateControls (Not ASP.NET AJAX istemci komut dosyası kitaplığı bu yöntem, olayları ve alan adları başlayan kuralı kullanır bir alt çizgiyle iç olarak işaretlenmiş ve kitaplığın kendisi dışında kullanım için değildir). Bununla, biz hangi denetimlerin AJAX Geri göndermeler neden amaçlayan gözlemleyebilirsiniz.

Örneğin, iki ek denetimler UpdatePanels dışında bir denetimin tamamen bırakarak ve biri UpdatePanel içinde bırakarak sayfasına ekleyelim. Biz üst UpdatePanel içinde bir CheckBox denetimi ekleyin ve bir liste içinde tanımlanan renk sayısı ile bir DropDownList bırakın. Yeni biçimlendirme şöyledir:

**Kod 3: Yeni biçimlendirme**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Ve yeni arka plan kod şu şekildedir:

**4 listeleme: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Bu sayfa arkasında fikirdir açılır listede ikinci etiketi göstermek için üç renk birini seçer, onay kutusunu kalın hem olup saatin yanı sıra tarih etiketlerini belirler. Onay kutusunun seçilmiş bir AJAX güncelleştirme neden olmamalıdır, ancak içinde UpdatePanel bünyesinde değil olsa da aşağı açılan liste gerekir.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Yukarıdaki ekran görüntüsünde görünür olduğundan, tıklanan için Son düğmesini sağ düğme alt süre üst zaman bağımsız güncelleştirilen güncelleştirme bu paneli, oldu. Tarih alt etiket içinde görünür olduğundan tarih ayrıca tıklama arasında değiştirildi devre dışı. Son olarak alt etiketin rengini ilgi çekecektir: denetim durumu önemli olduğunu gösterir, etiketin metin değerinden daha yakın zamanda güncelleştirildiği ve kullanıcılar AJAX Geri göndermeler korunması için bekler. *Ancak*, zaman güncelleştirilmedi. Saat, otomatik olarak kalıcılığı yeniden \_ \_sunucu üzerinde yeniden işlenmiş denetimi değiştirilirken, ASP.NET çalışma zamanı tarafından yorumlanmasını sayfanın görünüm durumu alanı. ASP.NET AJAX Sunucu kodu yöntemleri denetimleri bir durumu değiştiriliyor tanımaz; yalnızca görünüm durumundan yeniden doldurur ve ardından uygun olan olayların çalıştırır.

Bu, ancak miyim sayfa içindeki zamanında başlatılan vardı yönlendirilmesi\_yükleme olayı zaman doğru artmış. Sonuç olarak, geliştiriciler uygun kodu sırasında uygun olay işleyicileri çalıştırıldığı dikkatli olun ve sayfa kullanmaktan kaçının\_bir denetim olayı işleyicisi uygun olduğunda yükleyin.

## <a name="summary"></a>Özet

ASP.NET AJAX uzantılarını UpdatePanel denetimine kullanışlıdır ve bir dizi yöntem güncelleştirilmesi için neden denetim olayları tanımlamak için kullanabilir. Alt denetimlerine göre otomatik olarak güncelleştirilmesini destekler, ancak denetim olayları sayfasında başka bir yerde de yanıt verebilirsiniz.

Sunucu işleme yükü olasılığını azaltmak için önerilir `ChildrenAsTriggers` UpdatePanel özelliğinin ayarlanması `false`, ve olayları kabul yerine varsayılan olarak dahil olarak. Bu ayrıca gereksiz olaylara doğrulama ve giriş alanlarına değişiklikler dahil olmak üzere, olası istenmeyen etkileri neden olmasını engeller. Bu tür hataları, sayfa kullanıcıya şeffaf bir şekilde güncelleştirir ve neden bu nedenle hemen belirgin olmayabilir çünkü yalıtmak zor olabilir.

ASP.NET AJAX formun iç işleyişini inceleyerek durdurma modeli gönderin, biz zaten ASP.NET tarafından sağlanan çerçevesi kullanan belirlemek mümkün. Bunun yapılması, bu en büyük uyumluluk denetimlerle aynı framework kullanılarak tasarlanmış korur ve sayfa için yazılan herhangi bir ek JavaScript üzerinde en düşük düzeyde intrudes.

## <a name="bio"></a>Biyografi

Rob Paveza Terralever en üst düzey bir .NET uygulama geliştiricisi olan ([www.terralever.com](http://www.terralever.com)), önde gelen etkileşimli pazarlama firması Tempe, AZ. içinde He adresinden ulaşılabilir [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), ve blog şu konumdadır [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate 1997'den beri Microsoft Web teknolojileri ile çalışmakta olduğu ve myKB.com Yardımcısı ([www.myKB.com](http://www.myKB.com)) tabanlı Bilgi Bankası yazılım çözümlerinizi odaklı uygulamaları burada kendisinin ASP.NET yazma konusunda uzmanlaşmış. Scott temas kurulabileceğini e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog'da [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-partial-page-updates-with-asp-net-ajax.md)
> [İleri](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
