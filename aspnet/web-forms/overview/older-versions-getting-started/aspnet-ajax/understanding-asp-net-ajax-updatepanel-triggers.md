---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: ASP.NET AJAX UpdatePanel Tetikleyicileri anlama | Microsoft Docs
author: scottcate
description: Visual Studio'da biçimlendirme düzenleyicisinde çalışırken, bir UpdatePanel denetiminin iki alt öğe olan (IntelliSense) fark edebilirsiniz. Uyu birini...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: f30f2ead402d2f49a89b2caf47cc30b6445d4cfb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890755"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>ASP.NET AJAX UpdatePanel Tetikleyicileri anlama
====================
tarafından [Scott göstermek](https://github.com/scottcate)

[PDF indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Visual Studio'da biçimlendirme düzenleyicisinde çalışırken, bir UpdatePanel denetiminin iki alt öğe olan (IntelliSense) fark edebilirsiniz. Biri olan sayfasında (veya birini kullanıyorsanız, kullanıcı denetimi) denetimleri belirtir Tetikleyicileri öğesi kısmi işleme öğesi bulunduğu UpdatePanel denetiminin tetiklemek.


## <a name="introduction"></a>Giriş

Microsoft ASP.NET teknolojisi bir nesne yönelimli ve olay kaynaklı programlama modeli getirir ve derlenmiş kod yararları birleştirmiştir. Ancak, kendi sunucu tarafı işleme modelini birçok Microsoft ASP.NET 3.5 AJAX uzantılarında bulunan yeni özellikler çözülebilir teknolojisinde devralınan birkaç dezavantajları vardır. Bu uzantıları tam sayfa yenileme, Web Hizmetleri (profil oluşturma API'si ASP.NET dahil) istemci komut dosyası ve kapsamlı bir istemci-tarafı API aracılığıyla erişim olanağı gerek kalmadan sayfaların kısmi işleme dahil olmak üzere, birçok yeni zengin istemci özelliklerini etkinleştirme ASP.NET sunucu tarafı denetimi kümesinde görüldüğü denetim düzenleri çoğunu yansıtmak üzere tasarlanmıştır.

Bu teknik ASP.NET AJAX XML Tetikleyicileri işlevselliğini inceler `UpdatePanel` bileşeni. XML tetikleyicileri, belirli UpdatePanel denetimleri için kısmi işlemesine neden olan bileşenleri üzerinde ayrıntılı denetim sağlar.

Bu teknik Beta 2 sürümü, .NET Framework 3.5 ve Visual Studio 2008 dayanır. ASP.NET AJAX uzantılar, daha önce ASP.NET 2.0 hedeflenen bir eklenti derlemesi .NET Framework temel sınıfı Kitaplığı'na artık tümleşiktir. Bu Teknik İnceleme de varsayar Visual Studio 2008 ile değil Visual Web Developer Express, çalışma ve Visual Studio kullanıcı arabirimi göre izlenecek yollar (kod listeleri bakılmaksızın tamamen uyumlu olsa da sağlar geliştirme ortamı).

## <a name="triggers"></a>*Tetikleyiciler*

Varsayılan olarak, belirli bir UpdatePanel için Tetikleyicileri (örneğin) sahip TextBox denetimleri de dahil olmak üzere bir geri çağırma tüm alt denetimleri otomatik olarak dahil kendi `AutoPostBack` özelliğini **doğru**. Ancak, Tetikleyiciler ayrıca bildirimli olarak işaretleme kullanarak dahil olabilir; Bu içinde yapılır `<triggers>` UpdatePanel denetimi bildirim bölümü. Tetikleyicileri aracılığıyla erişilen rağmen `Triggers` koleksiyon özelliği (örneğin, bir denetim tasarım zamanında kullanılabilir değilse), çalışma zamanında hiçbir kısmi işleme tetikleyici kaydetmeniz önerilir kullanarak `RegisterAsyncPostBackControl(Control)` yöntemi ScriptManager sayfanız için içinde nesne `Page_Load` olay. Sayfaları durum bilgisiz ve oluşturulan her zaman bu nedenle bu denetimleri kaydetmenize unutmayın.

Otomatik alt tetikleyici ekleme de devre dışı bırakılabilir (Geri göndermeler oluşturma alt denetimleri otomatik olarak kısmi işler tetiklemez böylece) ayarlayarak `ChildrenAsTriggers` özelliğine **false**. Bir geliştirici olaya yerine ortaya çıkabilecek tüm olayları işleme yanıt katılımı böylece bu, hangi özel denetimlerin atanmasında en büyük esnekliği sayfa işleme çağırabilir ve önerilen sağlar.

UpdatePanel denetimleri iç içe geçmiş zaman zaman UpdateMode değerine ayarlandığını unutmayın **koşullu**, UpdatePanel alt tetiklenir, ancak üst sonra UpdatePanel yenileme alt, değilse. Üst UpdatePanel yenilendi ise, ancak ardından UpdatePanel alt de yenilenecek.

## <a name="the-lttriggersgt-element"></a>*&lt;Tetikleyicileri&gt; öğesi*

Visual Studio'da biçimlendirme düzenleyicisinde çalışırken, iki alt öğe olan (IntelliSense) karşılaşabilirsiniz, bir `UpdatePanel` denetim. En sık görülen öğesi `<ContentTemplate>` temelde güncelleştirme Masası tarafından tutulan içeriği yalıtır öğesi (için biz etkinleştirdiğiniz kısmi işleme içeriği). Diğer öğe `<Triggers>` sayfasında (veya birini kullanıyorsanız, kullanıcı denetimi) denetimleri belirtir öğesi kısmi işleme, UpdatePanel denetiminin tetiklemek &lt;Tetikleyicileri&gt; öğesi yer alıyor.

`<Triggers>` Öğesi, herhangi bir sayıda her iki alt düğümler içerebilir: `<asp:AsyncPostBackTrigger>` ve `<asp:PostBackTrigger>`. Her ikisi de iki öznitelik kabul `ControlID` ve `EventName`ve geçerli birim kapsüllemenin içinde herhangi bir denetimi belirtebilirsiniz (UpdatePanel denetimi içinde bir Web kullanıcı denetimi bulunuyorsa örneği için üzerinde denetime başvuruda denememeniz gerekir Kullanıcı denetiminin yer alacağı sayfası).

`<asp:AsyncPostBackTrigger>` Öğesidir özellikle yararlı içeren herhangi bir alt öğesi olarak mevcut bir denetim olayı hedefleyebilirsiniz *herhangi* kapsülleme, yalnızca bu tetikleyici altında bir alt öğesi olan UpdatePanel biriminde UpdatePanel denetimi . Bu nedenle, herhangi bir denetim bir kısmi sayfa güncelleştirmesini tetiklemek için yapılabilir.

Benzer şekilde, `<asp:PostBackTrigger>` öğesi, kısmi bir sayfayı işlemek tetikleyicisi, ancak sunucuya tam gidiş gerektiren bir için kullanılabilir. Bu tetikleyici öğesi denetim aksi normalde bir kısmi sayfa işleme tetikleyecek bir tam sayfa işleme zorlamak için de kullanılabilir (olduğunda, örneğin, bir `Button` denetim mevcut `<ContentTemplate>` UpdatePanel denetiminin öğesi). Yeniden PostBackTrigger öğesi geçerli birim kapsüllemenin içindeki herhangi bir UpdatePanel denetimi alt herhangi bir denetimi belirtebilirsiniz.

## <a name="lttriggersgt-element-reference"></a>*&lt;Tetikleyiciler&gt; öğe başvurusu*

*Biçimlendirme alt öğeleri:*

| **Etiket** | **Açıklama** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Denetim ve kısmi sayfa güncelleştirmesi Bu tetikleyici başvuru içeren UpdatePanel için neden olacak olay belirtir. |
| &lt;asp:PostBackTrigger&gt; | Denetim ve tam sayfa Güncelleştirmesi (tam sayfa yenileme) neden olacak olay belirtir. Bu etiket bir denetim, aksi takdirde kısmi işleme tetikleyecek tam yenilemeye zorlamak için kullanılabilir. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*İzlenecek yol: Arası UpdatePanel Tetikleyicileri*

1. Yeni bir ASP.NET sayfası kısmi işleme etkinleştirmek için ayarlanmış bir ScriptManager nesnesi oluşturun. Etiket denetimi (Label1) ve iki düğmenin denetimleri (Button1 ve Button2) içeriyor, bu sayfada - ilk iki UpdatePanels ekleyin. Button1 her ikisi de güncelleştirmek için tıklatın yazması gerekir ve bu veya bu satırlar boyunca bir şey güncelleştirmek için tıklatın Button2 yazması gerekir. İkinci UpdatePanel yalnızca etiket denetimi (Label2) dahil, ancak bunu ayırt etmek için default dışında başka bir şey için kendi ForeColor özelliği ayarlayın.
2. Her iki UpdatePanel etiketleri UpdateMode özelliği ayarlanmış **koşullu**.

**Kod 1: Biçimlendirme default.aspx için:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Label1.Text ve Label2.Text bir şey (DateTime.Now.ToLongTimeString()). gibi zamana bağımlı Button1 için Click olay işleyicisi ayarlayın Button2 için Click olay işleyicisi için yalnızca Label1.Text zamana bağımlı değerine ayarlayın.

**Kod 2: (default.aspx.cs içinde kırpılmış) arkasındaki koda:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Derleme ve projeyi çalıştırmak için F5 tuşuna basın. Güncelleştirme hem paneller tıkladığınızda, iki etiket metni değiştirmek, unutmayın; Ancak, güncelleştirme bu paneli tıkladığınızda, yalnızca Label1 güncelleştirir.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Başlık altında*

Yeni oluşturulan örnek kullanılarak, ASP.NET AJAX ne yaptığını ve bizim UpdatePanel arası paneli Tetikleyicileri nasıl göz alabilir. Oluşturulan sayfa kaynağı HTML olarak FireBug - onunla adlı Mozilla Firefox uzantısı ile çalışacak şekilde yapmak için şu AJAX Geri göndermeler kolayca inceleyebilirsiniz. Ayrıca .NET Reflector aracı Lutz Roeder tarafından kullanacağız. Bu araçların her ikisi de çevrimiçi ücretsiz olarak kullanılabilir ve bir internet arama bulunabilir.

Bir sayfa kaynak kodu incelenmesi neredeyse sıra dışı bir şey gösterir; UpdatePanel denetimleri olarak işlenir `<div>` kapsayıcıları ve biz görebilir komut dosyası kaynağını içeren sağlanan tarafından `<asp:ScriptManager>`. PageRequestManager için AJAX istemci komut dosyası Kitaplığı'na iç bazı yeni bir özel AJAX çağrıları vardır. Son olarak, size iki UpdatePanel kapsayıcıları - işlenen biriyle görmek `<input>` iki düğmeleriyle `<asp:Label>` denetimleri işlenen olarak `<span>` kapsayıcıları. (FireBug DOM ağacında inceleyin, bunlar görünür içeriğe oluşturduğunu değil olduğunu belirtmek için etiketleri soluklaştırılır görürsünüz).

Güncelleştirme bu Panel düğmesini tıklatın ve geçerli sunucu saati ile üst UpdatePanel güncelleştirilecek dikkat edin. Böylece istek inceleyebilirsiniz FireBug içinde konsol sekmesini seçin. Gönderme isteği parametreleri ilk inceleyin:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Hangi denetim ağacı ScriptManager1 parametresi ile tam olarak tetiklenen UpdatePanel için sunucu tarafı AJAX koduna belirtti Not: `Button1` , `UpdatePanel1` denetim. Şimdi, güncelleştirme hem paneller düğmesine tıklayın. Sonra yanıt inceleyerek, biz değişken bir dize Ayarla kanal ayrılmış bir dizi bakın; Özellikle, biz üst UpdatePanel görmek `UpdatePanel1`, tarayıcıya gönderilen kendi HTML tamamen sahiptir. AJAX istemci komut dosyası kitaplığı UpdatePanel'ın özgün HTML yeni içerikle içeriği değiştirir `.innerHTML` özelliği ve sunucu, HTML olarak sunucusundan değiştirilen içerik gönderir.

Şimdi, güncelleştirme hem paneller düğmesine tıklayın ve sunucudan sonuçlarını inceleyin. Sonuçları çok benzer - hem UpdatePanels yeni HTML sunucudan alma. Önceki geri araması ile gibi ek sayfa durumu gönderilir.

AJAX geri gönderme gerçekleştirmek için özel kod göstermesi için görebiliriz gibi AJAX istemci komut dosyası kitaplığı form Geri göndermeler herhangi bir ek kod olmadan müdahale mümkün olur. Sunucu denetimleri otomatik olarak JavaScript kullanan böylece otomatik olarak form göndermeyen - ASP.NET form doğrulama ve zaten durumu için öncelikle otomatik komut dosyası kaynak ekleme PostBackOptions sınıfı elde kodu otomatik olarak yerleştirir. ve ClientScriptManager sınıfı.

Örneğin, bir onay kutusu denetimi düşünün; .NET Reflector içinde sınıf ayrıştırılmış inceleyin. Bunu yapmak için System.Web derlemenizi açık olduğundan emin olun ve gidin `System.Web.UI.WebControls.CheckBox` açma sınıfı `RenderInputTag` yöntemi. Denetleyen koşullu için Ara `AutoPostBack` özelliği:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Üzerinde otomatik geri gönderme etkinleştirildiğinde bir `CheckBox` (true olan AutoPostBack), sonuç denetimi özelliğini `<input>` etiketi bir ASP.NET olay komut dosyasında işleme ile işlenen bu nedenle, `onclick` özniteliği. Form gönderme, daha sonra ele geçirilmesini, büyük olasılıkla kesin olmayan dize değiştirme yararlanarak oluşabilecek önemli değişiklikler olasılığını önlemek için yardımcı ASP.NET AJAX'ı sayfaya nonintrusively, eklenemeyebilir sağlar. Ayrıca, böylece *herhangi* UpdatePanel kapsayıcı içindeki kullanımını desteklemek için gücünü ASP.NET AJAX herhangi bir ek kod olmadan kullanmaya özel ASP.NET denetim.

`<triggers>` İşlevselliği karşılık gelen PageRequestManager çağrısında başlatılmış değerlere \_updateControls (Not ASP.NET AJAX istemci komut dosyası kitaplığı bu yöntemler, olaylar ve başlayan alan adları kuralı kullanır bir alt çizgi gibi iç işaretlenir ve kitaplık dışında kullanım için değildir). Bununla, biz hangi denetimlerin AJAX Geri göndermeler neden amaçlanır görebilirsiniz.

Örneğin, iki ek denetimler UpdatePanels dışında bir denetim tamamen bırakarak ve bir bir UpdatePanel içinde bırakarak sayfasına ekleyelim. Biz işlemi üst UpdatePanel içinde bir onay kutusu denetimi ekler ve listenin içinde tanımlı renklerin sayısı ile bir DropDownList bırakın. Yeni markup şöyledir:

**Kod 3: Yeni Markup**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Ve yeni arka plan kod şöyledir:

**4 listeleme: arkasındaki koda**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Bu sayfayı arkasında fikirdir açılan listesinden ikinci etiketi göstermek için üç renkten birini seçer, onay kutusu kalın olup hem olup saatin yanı sıra tarih etiketleri görüntülemesi belirler. Onay kutusu bir AJAX güncelleştirme neden, ancak bir UpdatePanel içinde yerleştirilebilir değildir olsa da aşağı açılan liste gerekir.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Yukarıdaki ekran görüntüsü açıktır, tıklattınız için en son düğmesine sağ alt sürenin üst zaman bağımsız güncelleştirilen güncelleştirme bu paneli, düğme kaydedildi. Tarih alt etiket görünür olduğundan tarihi de tıklama değiştirildi devre dışı. Son olarak alt etiketin renk ilginizi çekecektir: daha yakın zamanda denetim durumu önemli olduğunu gösteren, etiketin metin güncelleştirildi ve kullanıcılar AJAX Geri göndermeler korunması için bekler. *Ancak*, zaman güncelleştirilmedi. Saat, otomatik olarak kalıcılığı yeniden \_ \_denetimi sunucuda yeniden işlenmiş zaman ASP.NET çalışma zamanı tarafından yorumlanmasını sayfanın görünüm durumu alanı. ASP.NET AJAX Sunucu kodu yöntemleri denetimleri durumu değiştiriyorsunuz tanımaz; yalnızca görünüm durumundan yeniden doldurur ve ardından uygun olan olayları çalıştırır.

Bu, ancak ı sayfa sürede başlatılmış olan işaret edilmesi gereken\_yük olay zaman doğru artmış. Sonuç olarak, geliştiricilerin uygun kodu sırasında uygun olay işleyicileri çalıştırıldığı dikkatli olun ve sayfa kullanmaktan kaçının\_yük denetim olay işleyicisi uygun olduğunda.

## <a name="summary"></a>Özet

ASP.NET AJAX uzantıları UpdatePanel denetim yönlüdür ve çeşitli yöntemler güncelleştirilmesi neden denetim olaylarını tanımlamak için kullanabilir. Kendi alt denetimleri tarafından otomatik olarak güncelleştirilmesini destekler, ancak aynı zamanda başka bir yerde sayfasında denetim olaylara yanıt verebilir.

Sunucu işleme yükünü olasılığını azaltmak için önerilir `ChildrenAsTriggers` bir UpdatePanel özelliğinin ayarlanması `false`, ve olayları seçti-yerine varsayılan olarak dahil içine. Bu ayrıca gereksiz olaylara doğrulama ve giriş alanları değişiklik de dahil olmak üzere, olası istenmeyen etkileri neden engeller. Bu tür hataların, çünkü sayfa kullanıcıya şeffaf bir şekilde güncelleştirir ve neden bu nedenle hemen belirgin olmayabilir yalıtmak zor olabilir.

Çalışmalar ASP.NET AJAX formun inceleyerek kişiler tarafından ele modeli işlemleri sonrasında, ASP.NET tarafından zaten sağlanan çerçevesi kullanan belirlemek bulduk. Bunun yapılması, onu aynı framework kullanılarak tasarlanmış denetimleri ile en fazla uyumluluğu korur ve sayfa için yazılmış herhangi bir ek JavaScript üzerinde en düşük düzeyde intrudes.

## <a name="bio"></a>Biyografisi

Ramiz Paveza Terralever en üst düzey bir .NET uygulama geliştiricisi olan ([www.terralever.com](http://www.terralever.com)), Tempe, AZ. içinde başında etkileşimli pazarlama kesin Kendisi üzerinde erişilebilir [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), ve kendi blog konumundadır [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Tan göstermek Microsoft Web teknolojileri ile bu yana 1997 çalışma ve myKB.com Başkanı ise ([www.myKB.com](http://www.myKB.com)) kendisine ASP.NET yazılırken burada uzmanlaşmış tabanlı Bilgi Bankası yazılım çözümlerini odaklanmış uygulamaları. Tan temas kurulabileceğini doğrula e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog adresindeki [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-partial-page-updates-with-asp-net-ajax.md)
> [sonraki](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
