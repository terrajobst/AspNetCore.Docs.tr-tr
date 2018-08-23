---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 sayfa modeli | Microsoft Docs
author: microsoft
description: ASP.NET'te 1.x, geliştiricilerin sahip bir satır içi kod modeli ve gerideki kod modeli arasında seçim yapma. Arka plan kod Src attr kullanarak uygulanabileceğine...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 4452169a01276cbc60f2a2057e6b560022ccd7c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755544"
---
<a name="the-aspnet-20-page-model"></a>ASP.NET 2.0 sayfa modeli
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET'te 1.x, geliştiricilerin sahip bir satır içi kod modeli ve gerideki kod modeli arasında seçim yapma. Arka plan kod Src özniteliğine veya CodeBehind özniteliğinin kullanarak uygulanmasını @Page yönergesi. ASP.NET 2. 0'da, geliştiricilerin yine de satır içi kod ve arka plan kod arasında bir seçim var, ancak arka plan kod modeline önemli geliştirmeler yapıldı.


ASP.NET'te 1.x, geliştiricilerin sahip bir satır içi kod modeli ve gerideki kod modeli arasında seçim yapma. Arka plan kod Src özniteliğine veya CodeBehind özniteliğinin kullanarak uygulanmasını @Page yönergesi. ASP.NET 2. 0'da, geliştiricilerin yine de satır içi kod ve arka plan kod arasında bir seçim var, ancak arka plan kod modeline önemli geliştirmeler yapıldı.

## <a name="improvements-in-the-code-behind-model"></a>Arka plan kod modelinde geliştirmeleri

Hızla model olarak onu gözden geçirmek için en iyi şekilde varolan ASP.NET'te ASP.NET 2.0 arka plan kod modelde değişiklikler tam olarak anlamak için 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>ASP.NET arka plan kod modelde 1.x

ASP.NET'te 1.x, arka plan kod modeli oluşturan bir ASPX dosyası (Webform) ve programlama kodu içeren bir arka plan kod dosyası. İki dosyayı kullanarak bağlanmış @Page ASPX dosyasında yönergesi. Her denetimi ASPX sayfasına karşılık gelen bir bildirim arka plan kod dosyasında bir örnek değişkeni vardı. Arka plan kod dosyasında olay bağlama kodunu yer alan ve oluşturulan kod Visual Studio tasarımcısı için gerekli. Bu modeli oldukça iyi çalışan, ancak her ASPX sayfasına ASP.NET öğesinde karşılık gelen kodu arka plan kod dosyasında gerektirdiğinden, kod ve içeriğinin doğru hiçbir ayrım vardı. Bir tasarımcı, Visual Studio IDE dışında ASPX dosyasına yeni bir sunucu denetimi eklediyseniz, örneğin, uygulama bu denetim için bir bildirim olmaması nedeniyle arka plan kod dosyasında çalışmamasına neden.

## <a name="the-code-behind-model-in-aspnet-20"></a>ASP.NET 2.0 arka plan kod modeli

ASP.NET 2.0 üzerinde bu model önemli ölçüde artırır. ASP.NET 2. 0'da, arka plan kod kullanarak yeni uygulanan *kısmi sınıflar* ASP.NET 2.0 sürümünde sağlanan. Arka plan kod ASP.NET 2.0 el sınıf tanımı yalnızca bir kısmını içerdiği anlamına gelen bir kısmi sınıf olarak sınıftır. Sınıf tanımının kalan bölümü, çalışma zamanında veya Web sitesi Ön derlenmiş ASPX sayfa kullanarak ASP.NET 2.0 tarafından dinamik olarak oluşturulur. Arka plan kod dosyası ve ASPX Sayfası arasındaki bağlantıyı yine de @ sayfa yönergesi kullanarak oluşturulur. Ancak, bir CodeBehind veya Src özniteliği yerine ASP.NET 2.0 CodeFile öznitelik şimdi kullanır. Inherits özniteliği de sayfa için sınıf adını belirtmek için kullanılır.

Tipik bir @ sayfa yönergesi şuna benzeyebilir:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Bir ASP.NET 2.0 arka plan kod dosyasında bir genel sınıf tanımının şuna benzeyebilir:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# ve Visual Basic şu anda kısmi sınıflar desteği yalnızca yönetilen diller şunlardır. Bu nedenle, geliştiriciler J# kullanarak ASP.NET 2.0 sürümünde arka plan kod modelini kullanmanız mümkün olmayacaktır.


Geliştiriciler artık, kullanıcının oluşturduğu kodu içeren kod dosyaları olduğundan yeni modeli arka plan kod modelini geliştirir. Arka plan kod dosyasında hiçbir örneği değişken bildirimlerini olduğundan doğru ayrımı kod ve içerik de sağlar.

> [!NOTE]
> ASPX sayfa için kısmi sınıf olay bağlama gerçekleştiği olduğundan, Visual Basic geliştiricileri olaylar bağlamak için gerideki kod tanıtıcıları anahtar sözcüğü kullanarak küçük bir performans artışı sağlarsınız. C# anahtar sözcük eşdeğer vardır.


## <a name="new--page-directive-attributes"></a>Yeni @ sayfa yönergesi öznitelikler

ASP.NET 2.0 @ sayfa yönergesi için birçok yeni öznitelikler ekler. Aşağıdaki öznitelikler ASP.NET 2.0 sürümünde yenidir.

## <a name="async"></a>Zaman Uyumsuz

Zaman uyumsuz özniteliği zaman uyumsuz olarak yürütülecek sayfa yapılandırmanıza olanak sağlar. Daha sonra bu modüldeki zaman uyumsuz sayfaları da kapsar.

## <a name="asynctimeout"></a>AsyncTimeout

Zaman uyumsuz sayfaları için zaman aşımı belirtildi. Varsayılan değer 45 saniyedir.

## <a name="codefile"></a>CodeFile

CodeFile özniteliği CodeBehind özniteliğinin, Visual Studio 2002/2003 yerini alır.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

CodeFileBaseClass öznitelik, tek bir temel sınıftan türetmek için birden çok sayfada istediğiniz durumlarda kullanılır. Bu öznitelik olmadan, ASP.NET'te kısmi sınıflar uygulanması nedeniyle bir ASPX sayfa bildirilen denetimleri başvurmak için paylaşılan ortak alanları kullanan bir temel sınıf düzgün olduğundan çalışmak istemiyor ASP. Ağ derleme altyapısı sayfasındaki denetimleri temel alarak yeni üyeleri otomatik olarak oluşturur. Bu nedenle, iki veya daha fazla ASP.NET sayfaları için genel bir temel sınıf istiyorsanız, tanımlama gerekecektir CodeFileBaseClass özniteliğinde, taban sınıf belirtin ve ardından o temel sınıftan her sayfaları sınıf türetin. Bu öznitelik kullanıldığında CodeFile özniteliği de gereklidir.

## <a name="compilationmode"></a>CompilationMode

Bu öznitelik ASPX sayfa CompilationMode özelliği ayarlamanıza olanak tanır. CompilationMode özellik değerleri içeren bir numaralandırmadır **her zaman**, **otomatik**, ve **hiçbir zaman**. Varsayılan değer **her zaman**. **Otomatik** ayarı, ASP.NET dinamik olarak sayfa mümkünse derleme öğesinden engeller. Sayfaları dinamik derlemeden hariç performansı artırır. Ancak, hariç bir sayfa derlenmelidir bu kodu içeriyorsa, sayfasına göz atıldığında bir hata oluşturulur.

## <a name="enableeventvalidation"></a>EnableEventValidation

Bu öznitelik, geri gönderme ve geri çağırma olayları doğrulanır olup olmadığını belirtir. Bu etkinleştirildiğinde, geri gönderme bağımsız değişkenler veya geri çağırma olayları, ilk işlenen sunucu denetiminden kaynaklı olduğunu emin olmak için denetlenir.

## <a name="enabletheming"></a>EnableTheming

Bu öznitelik, bir sayfa üzerinde ASP.NET temaları kullanılan olup olmadığını belirtir. Varsayılan değer **false**. ASP.NET temaları kapsamdaki [modülü 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Bu öznitelik, derleme sırasında satır pragmaları eklenmesi gerekip gerekmediğini belirtir. Satırı pragmaları kodun belirli bölümlerine işaretlemek için hata ayıklayıcıları tarafından kullanılan seçeneklerdir.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Bu öznitelik, Geri göndermeler arasında kaydırma konumunu korumak için JavaScript sayfalarına eklenmiş olup olmadığını belirtir. Bu öznitelik **false** varsayılan olarak.

Bu öznitelik olduğunda **true**, ASP.NET ekleyecek bir &lt;betik&gt; şuna benzer bir geri gönderme blok:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Bu betik bloğu için src WebResource.axd olduğunu unutmayın. Bu kaynak, fiziksel bir yol değil. Bu betik istendiğinde, ASP.NET dinamik olarak betik oluşturur.

### <a name="masterpagefile"></a>MasterPageFile

Bu öznitelik geçerli sayfa için ana sayfa dosyası belirtir. Göreli veya mutlak yol olabilir. Ana sayfalar kapsamdaki [modülü 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Bu öznitelik, bir ASP.NET 2.0 tema tarafından tanımlanan kullanıcı arabirimi görünüm özellikleri geçersiz kılmanıza olanak sağlar. Temalar kapsamdaki [modülü 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Sayfa teması belirtir. Tema özniteliği StyleSheetTheme özniteliği için bir değer belirtilmezse, sayfadaki denetimleri için uygulanan tüm stillerini geçersiz kılar.

## <a name="title"></a>Başlık

Sayfanın başlığını ayarlar. Burada belirtilen değeri görünür &lt;başlık&gt; işlenen sayfanın öğesi.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

ViewStateEncryptionMode numaralandırma değerini ayarlar. Kullanılabilir değerler **her zaman**, **otomatik**, ve **hiçbir zaman**. Varsayılan değer **otomatik**. Bu öznitelik değerine ayarlandığında **otomatik**, viewstate şifrelenir denetim istekleri, çağırarak olan **RegisterRequiresViewStateEncryption** yöntemi.

## <a name="setting-public-property-values-via-the--page-directive"></a>Genel özellik değerleri aracılığıyla @ sayfa yönergesi ayarlama

ASP.NET 2.0 @ sayfa yönergesi başka bir yeni özellik genel bir temel sınıf özelliklerinin başlangıç değeri ayarlamak da yeteneğidir. Örneğin, ortak bir özellik olan adlı varsayalım **SomeText** temel sınıfınız ve buna benzer d için başlatılması için **Hello** ne zaman bir sayfa yüklenir. @ Sayfa yönergesinde yalnızca bir değere ayarlayarak bunu gerçekleştirmenin şu şekilde:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** @ sayfa yönergesi özniteliği için bir temel sınıfta SomeText özelliğinin ilk değeri ayarlar *Merhaba!*. Aşağıdaki video @ sayfa yönergesi kullanarak temel bir sınıfta bir genel özelliğinin ilk değeri ayarı bir gözden geçirme ' dir.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Açık tam ekran görüntü](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Yeni genel özellik sayfası sınıfının

Aşağıdaki genel özellikleri ASP.NET 2.0 sürümünde yenidir.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Sayfa veya denetim için uygulamaya göreli yolunu döndürür. Örneğin, konumunda bulunan bir sayfa için http://app/folder/page.aspx, özellik döndürür ~ / folder /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Sayfa veya denetim için göreli sanal dizin yolu döndürür. Örneğin konumunda bulunan bir sayfa için http://app/folder/page.aspx, özellik döndürür ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Alır veya ayarlar sayfası zaman uyumsuz işleme için kullanılan zaman aşımı. (Zaman uyumsuz sayfaları Bu modülün daha sonra ele alınacaktır.)

## <a name="clientquerystring"></a>ClientQueryString

İstenen URL sorgu dizesi bölümünü döndürür salt okunur özellik. Bu değer, URL kodlamalı olur. Bu kodu çözülecek HttpServerUtility sınıfının UrlDecode yöntemi kullanabilirsiniz.

## <a name="clientscript"></a>ClientScript

Bu özellik, istemci tarafı komut dosyası ASP.NETs arabellek yönetmek için kullanılan bir ClientScriptManager nesnesi döndürür. (ClientScriptManager sınıfı bu modül içinde ele alınmıştır.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Bu özellik, olay doğrulama geri gönderme ve geri çağırma olayları için etkin olup olmadığını denetler. Etkin olduğunda, bağımsız geri gönderme veya geri çağırma olayları, ilk işlenen sunucu denetiminden kaynaklı olduğunu emin olmak için doğrulanır.

## <a name="enabletheming"></a>EnableTheming

Bu özelliği alır veya ASP.NET 2.0 tema sayfasına uygulanıp uygulanmayacağını belirten Boolean bir değer ayarlar.

## <a name="form"></a>Form

Bu özellik, HTML form ASPX sayfa üzerinde bir HtmlForm nesne olarak döndürür.

## <a name="header"></a>Üstbilgi

Bu özellik, sayfa üstbilgisindeki içeren bir HtmlHead nesnesine bir başvuru döndürür. Döndürülen HtmlHead nesne get/set için stil sayfaları, Meta etiketler kullanabilirsiniz.

## <a name="idseparator"></a>IdSeparator

Bu salt okunur özelliği, ASP.NET bir sayfadaki denetimleri için benzersiz bir kimlik oluştururken denetim tanımlayıcıları ayırmak için kullanılan karakteri alır. Doğrudan kodunuzdan kullanılmaya yönelik değildir.

## <a name="isasync"></a>Isasync

Bu özellik için zaman uyumsuz sayfalar sağlar. Zaman uyumsuz sayfalar, bu modül içinde ele alınmıştır.

## <a name="iscallback"></a>IsCallback

Bu salt okunur özellik döndürür **true** sayfa geri arama sonucunu ise. Geri aramalar, daha sonra bu modülde ele alınmıştır.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Bu salt okunur özellik döndürür **true** sayfa çapraz sayfa geri gönderme parçası olduğunda. Çapraz sayfa Geri göndermeler, daha sonra bu modülde ele alınmaktadır.

## <a name="items"></a>Öğeler

Sayfaları bağlam içinde depolanmış tüm nesneler içeren bir IDictionary örneğe bir başvuru döndürür. Bu IDictionary nesneye öğeleri ekleyebilir ve kullanabileceğiniz bağlamı kullanım ömrü boyunca olacaktır.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Bu özellik, ASP.NET bir geri gönderme tamamlandıktan sonra konumu tarayıcıda sayfaların kaydırma tutar JavaScript olup olmadığını yayan denetler. (Bu özellik ayrıntılarını Bu modülün daha önce bahsedilen.)

## <a name="master"></a>Ana

Bu salt okunur bir özellik için bir ana sayfa uygulanmış bir sayfa AnaSayfa örneğe bir başvuru döndürür.

## <a name="masterpagefile"></a>MasterPageFile

Alır veya ayarlar sayfası için ana sayfanın dosya adı. Bu özellik yalnızca PreInit yöntemi ayarlayabilirsiniz.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Bu özelliği alır veya en fazla uzunluk sayfaları durumu için bayt cinsinden ayarlar. Özelliği için pozitif bir sayı ayarlanırsa, belirtilen bayt sayısını aşmadığından emin sayfaları görünüm durumu birden çok gizli alanlarına bölünmesi. Özelliği negatif bir sayı ise, Görünüm durumu parçalara bozuk değil.

## <a name="pageadapter"></a>PageAdapter

Sayfa için isteyen tarayıcıya değiştirir PageAdapter nesnesine bir başvuru döndürür.

## <a name="previouspage"></a>PreviousPage

Bir Server.Transfer veya çapraz sayfa geri gönderme durumlarda önceki sayfaya bir başvuru döndürür.

## <a name="skinid"></a>SkinID

Sayfaya uygulamak için ASP.NET 2.0 dış belirtir.

## <a name="stylesheettheme"></a>StyleSheetTheme

Bu özelliği alır veya ayarlar bir sayfaya uygulanan stil sayfası.

## <a name="templatecontrol"></a>TemplateControl

İçeren denetim sayfası için bir başvuru döndürür.

## <a name="theme"></a>Tema

Alır veya ayarlar sayfasına ASP.NET 2.0 tema adı. Bu değer PreInit yöntemi önce ayarlanmalıdır.

## <a name="title"></a>Başlık

Bu özelliği alır veya sayfaları üst bilgisinden alınan sayfanın başlığını ayarlar.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Alır veya ayarlar sayfasının ViewStateEncryptionMode. Bu özelliğin bu modüldeki ayrıntılı bir tartışma bakın.

## <a name="new-protected-properties-of-the-page-class"></a>Sayfa sınıfının yeni bir korumalı Özellikler

ASP.NET 2.0 sayfa sınıfının yeni bir korumalı özellikleri aşağıda verilmiştir.

## <a name="adapter"></a>Bağdaştırıcı

İstendiğinde, cihazdaki sayfaya işler ControlAdapter başvuru döndürür.

## <a name="asyncmode"></a>AsyncMode

Bu özellik sayfası zaman uyumsuz olarak işlenen olup olmadığını gösterir. Çalışma zamanı tarafından ve doğrudan kod, kullanım içindir.

## <a name="clientidseparator"></a>ClientIDSeparator

Bu özellik, benzersiz istemci denetimleri için kimlikleri oluştururken bir ayırıcı olarak kullanılan karakter döndürür. Çalışma zamanı tarafından ve doğrudan kod, kullanım içindir.

## <a name="pagestatepersister"></a>PageStatePersister

Bu özellik sayfası için PageStatePersister nesnesi döndürür. Bu özellik, öncelikli olarak ASP.NET denetimi geliştiriciler tarafından yaygın olarak kullanılır.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Bu özellik, tarayıcılar önbelleğe alma için dosya yolu eklenen benzersiz bir suffic döndürür. Varsayılan değer \_ \_ufps = ve 6 basamaklı bir sayı.

## <a name="new-public-methods-for-the-page-class"></a>Sayfası sınıfı için yeni genel yöntemler

Aşağıdaki genel yöntemleri, ASP.NET 2.0 sayfa sınıfında yenidir.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Bu yöntem, zaman uyumsuz sayfa yürütme için olay işleyici temsilcilerini kaydeder. Zaman uyumsuz sayfalar, bu modül içinde ele alınmıştır.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Sayfaları stil sayfası özellikler sayfasına geçerlidir.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Bu yöntem canlı bir zaman uyumsuz görev.

### <a name="getvalidators"></a>GetValidators

Belirtilmezse, belirtilen doğrulama veya varsayılan doğrulama grubunun yönelik Doğrulayıcıların bir koleksiyonunu döndürür.

## <a name="registerasynctask"></a>RegisterAsyncTask

Bu yöntem, yeni bir zaman uyumsuz görev kaydeder. Bu modül zaman uyumsuz sayfalar ele alınmaktadır.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Bu yöntem ASP.NET sayfaları denetim durumu kalıcı olduğunu bildirir.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Bu yöntem, ASP.NET sayfaları viewstate şifreleme gerektiren söyler.

## <a name="resolveclienturl"></a>ResolveClientUrl

İstemci istekleri görüntüler vb. için kullanılabilecek bir göreli URL döndürür.

## <a name="setfocus"></a>SetFocus

Bu yöntem odağı sayfa ilk yüklendiğinde belirtilen denetimi için ayarlar.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Bu yöntem artık denetim durumu kalıcılığını gerektiren olarak geçirilen denetim kaldırır.

## <a name="changes-to-the-page-lifecycle"></a>Sayfa yaşam döngüsü değişiklikleri

ASP.NET 2.0 sayfa yaşam döngüsü önemli ölçüde değişmediğinden, ancak farkında olmanız gereken bazı yeni yöntemler vardır. ASP.NET 2.0 sayfa yaşam döngüsü aşağıda ana hatlarıyla açıklanmıştır.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (yeni, ASP.NET 2.0)

En erken bir geliştirici erişip yaşam döngüsü aşamasında PreInit olayıdır. Bu olay eklenmesi, programlı olarak ASP.NET 2.0 temalar, ana sayfalar değiştirmek için bir ASP.NET 2.0 profili, vb. için özelliklere erişmek mümkün kılar. Önemli Viewstate henüz yaşam döngüsü bu noktada denetimlere uygulandığını değil hayata geçirmek bir geri gönderme durumda olduğunda. Bir geliştirici bu aşamada bir denetimin bir özelliğine değişirse, bu nedenle, büyük olasılıkla daha sonra sayfa yaşam döngüsünde üzerine yazılacak.

## <a name="init"></a>Init

ASP.NET tarafından Init olay değişmedi 1.x. Okuma veya sayfanızda denetimlerin özelliklerini açma başlatmak istediğiniz budur. Bu aşama, ana sayfalar, temalar vb. zaten sayfasına uygulanır.

## <a name="initcomplete-new-in-20"></a>InitComplete (yeni, 2.0)

InitComplete olayı sayfaları başlatma aşamasının sonunda çağrılır. Bu noktada yaşam döngüsünde sayfadaki denetimleri erişebilirsiniz, ancak durumlarını değil henüz doldurulmadı.

## <a name="preload-new-in-20"></a>PreLoad (2.0 sürümünde yeni)

Bu olay, tüm geri gönderme veri uyguladıktan sonra ve sayfa hemen önce çağrılır\_yük.

## <a name="load"></a>Yükleme

Yükleme olayı ASP.NET tarafından değiştirilmemiştir 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (yeni, 2.0)

LoadComplete olay son sayfaları yük aşamasında etkinliğidir. Bu aşamada, tüm geri gönderme ve viewstate verilerini uygulanmış sayfasına.

## <a name="prerender"></a>PreRender

Görünüm durumu sayfaya eklenen dinamik olarak denetimleri için düzgün şekilde saklanması için isterseniz PreRender olayını bunları eklemek için son şansınızdır.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (yeni, 2.0)

PreRenderComplete aşamada, tüm denetimler sayfasına eklenen ve sayfa işlenmek üzere hazırdır. Sayfaları görünüm durumu kaydedilmeden önce tetiklenen son olayı PreRenderComplete olayıdır.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (yeni, 2.0)

SaveStateComplete olayı, hemen tüm sayfa viewstate ve denetim durumu kaydedildikten sonra çağrılır. Sayfa tarayıcıya gerçekten işlenmeden önce son olay budur.

## <a name="render"></a>İşleme

İşleme yöntemi ASP.NET bu yana değişmemiştir 1.x. Burada HtmlTextWriter başlatılır ve tarayıcıda görüntülenen sayfa budur.

## <a name="cross-page-postback-in-aspnet-20"></a>ASP.NET 2.0 çapraz sayfa geri gönderme

ASP.NET'te 1.x, Geri göndermeler aynı sayfaya göndermek için gerekli. Çapraz sayfa Geri göndermeler izin verilmiyor. ASP.NET 2.0 IButtonControl arabirimi üzerinden başka bir sayfaya geri gönderilecek özelliği ekler. Bu yeni işlevselliği aracılığıyla PostBackUrl öznitelik kullanımı (düğme, LinkButton ve üçüncü taraf özel denetimler yanı sıra ImageButton) yeni IButtonControl arabirimi uygulayan herhangi bir denetimi yararlanabilirsiniz. Aşağıdaki kod, ikinci bir sayfaya gönderir düğme denetimini gösterir.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Sayfanın geri gönderildiğinde geri göndermeyi başlatan sayfanın PreviousPage özelliğinin ikinci sayfasında aracılığıyla erişilebilir. Bu işlev yeni WebForm uygulanan\_DoPostBackWithOptions istemci-tarafı bir denetimi başka bir sayfaya gönderileri sayfasına ASP.NET 2.0 işleyen bir işlev. Bu JavaScript işlevi, istemci betiği yayan yeni WebResource.axd işleyici tarafından sağlanır.

Aşağıdaki video, çapraz sayfa geri gönderme kılavuz olduğunda.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Açık tam ekran görüntü](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Çapraz sayfa geri gönderme hakkında daha fazla bilgi

### <a name="viewstate"></a>Görünüm durumu

Kendiniz zaten viewstate için çapraz sayfa geri gönderme senaryosunda ilk sayfadan olabilecekler sorulan. Sonuçta IPostBackDataHandler uygulamıyor herhangi bir denetimi durumuna aracılığıyla görünüm durumu, bu nedenle çapraz sayfa geri gönderme ikinci sayfasında bu denetimin özelliklerini erişimi için açık kalır, sayfanın görünüm durumu için erişimi olmalıdır. ASP.NET 2.0 adlı ikinci sayfasında yeni bir gizli alan kullanarak bu senaryonun üstlenir \_ \_PREVIOUSPAGE. \_ \_PREVIOUSPAGE form alanı, böylece tüm denetimlerin özelliklerine erişim ikinci sayfada olabilir ilk sayfanın görünüm durumu içerir.

### <a name="circumventing-findcontrol"></a>FindControl atlanarak

Çapraz sayfa geri gönderme video kılavuzda miyim FindControl TextBox denetimi ilk sayfasında bir başvuru almak için kullanılan yöntem. Bu yöntem, bu amaç için iyi çalışır, ancak FindControl pahalıdır ve bu ek kod yazma gerekir. Neyse ki, ASP.NET 2.0 pek çok senaryoda çalışmaz, bu amaçla FindControl bir alternatif sunar. PreviousPageType yönergesi TypeName veya VirtualPath özniteliğini kullanarak türü kesin belirlenmiş bir başvuru önceki sayfaya sahip olmanızı sağlar. TypeName öznitelik sanal yolu kullanarak önceki sayfasına başvurmak VirtualPath öznitelik sağlar, ancak önceki sayfaya türünü belirtmenizi sağlar. PreviousPageType yönergesi ayarladıktan sonra ardından açığa çıkarmalıdır denetimler, genel özelliklerini kullanarak erişim vermek istediğiniz vb.

## <a name="lab-1-cross-page-postback"></a>Laboratuvar 1 arası sayfa geri gönderme

Bu laboratuvarda, ASP.NET 2.0 yeni çapraz sayfa geri gönderme işlevlerini kullanan bir uygulama oluşturacaksınız.

1. Visual Studio 2005'i açın ve yeni bir ASP.NET Web sitesi oluşturun.
2. Page2.aspx adlı yeni bir Web formu ekleyin.
3. Default.aspx Tasarım Görünümü'nde açın ve bir düğme denetimi ve bir metin kutusu denetimi ekleyebilirsiniz. 

    1. Düğme denetimini kimliği vermek **SubmitButton** ve metin kutusu denetim Kimliğini **UserName**.
    2. Düğmenin PostBackUrl özelliği için page2.aspx ayarlayın.
4. Page2.aspx Kaynak Görünümü'nde açın.
5. @ PreviousPageType yönergesi, aşağıda gösterildiği gibi ekleyin:
6. Aşağıdaki kod sayfasına ekleme\_page2.aspx'ın arka plan kod yükü: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Yapı menüsünde derlemeyi tıklayarak projeyi derleyin.
8. Arka plan kod Default.aspx için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Sayfayı Değiştir\_page2.aspx aşağıdaki yükleme: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Projeyi oluşturun.
11. Projeyi çalıştırın.
12. Metin kutusunda adınızı girin ve düğmeye tıklayın.
13. Sonuç nedir?

## <a name="asynchronous-pages-in-aspnet-20"></a>ASP.NET 2.0 sürümünde zaman uyumsuz sayfaları

ASP.NET'te birçok Çekişme sorunları (örneğin, Web hizmeti veya veritabanı çağrıları) dış aramalar, dosya g/ç gecikme gecikmesine neden olur. Bir ASP.NET uygulaması karşı bir istek yapıldığında, ASP.NET birini kendi çalışan iş parçacıkları Bu isteğe hizmet vermek için kullanır. Bu istek, istek tamamlandıktan ve yanıtı gönderildi kadar iş parçacığı sahip olur. ASP.NET 2.0 sayfalarını zaman uyumsuz olarak yürütmek için özellik ekleyerek bu tür sorunları gecikme sorunlarını gidermek çalışmaktadır. Bu iş parçacığı isteği başlatın ve ardından başka bir iş parçacığına, böylece kullanılabilir iş parçacığı havuzuna hızla döndüren ek yürütme devre dışı el anlamına gelir. Yeni bir iş parçacığı, dosya GÇ, veritabanı çağrısı, vb. tamamlandı, istek tamamlamak için iş parçacığı havuzundan elde edilir.

Zaman uyumsuz yürütülen bir sayfa yapmadan ilk adımı ayarlamaktır **zaman uyumsuz** sayfa yönergesi özniteliği şu şekilde:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

ASP.NET sayfası için IHttpAsyncHandler uygulamak için bu öznitelik bildirir.

Sonraki adımda, sayfanın PreRender önce yaşam döngüsünde bir noktada AddOnPreRenderCompleteAsync yöntemi çağırmaktır. (Bu yöntem genellikle sayfasında çağrılır\_yük.) AddOnPreRenderCompleteAsync yöntem iki parametre alır; bir BeginEventHandler ve bir EndEventHandler. Ardından EndEventHandler için parametre olarak geçirilen bir IAsyncResult BeginEventHandler döndürür.

Aşağıdaki videoyu bir kılavuz bir zaman uyumsuz sayfa isteğinin ' dir.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Açık tam ekran görüntü](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> EndEventHandler tamamlanana kadar zaman uyumsuz bir sayfa tarayıcıya işlemez. Hiçbir şüpheli ancak bazı geliştiriciler zaman uyumsuz isteği zaman uyumsuz geri çağırmalar için benzer olarak düşünebilirsiniz. Olmadıklarını bilmeniz önemlidir. Zaman uyumsuz istekler için ilk çalışan iş parçacığının iş parçacığı havuzu hizmet yeni isteklerini, böylece azaltma Çekişme nedeniyle GÇ bağlı olan, vb. için döndürülebilir avantajdır.


## <a name="script-callbacks-in-aspnet-20"></a>ASP.NET 2.0 betik geri çağırmaları

Web geliştiricileri, her zaman bir geri çağırma ile ilişkili titremeyi önlemek yollarını attıktan. ASP.NET'te 1.x, SmartNavigation titremeyi kaçınmak için en yaygın yöntem olan, ancak istemci uygulaması karmaşıklığı nedeniyle bazı geliştiriciler için SmartNavigation sorunlarına neden. ASP.NET 2.0 betik geri çağırmaları ile bu sorunu giderir. Betiği geri çağırmaları JavaScript aracılığıyla Web sunucuya yönelik isteklerde XMLHttp kullanır. XMLHttp isteği tarayıcının yerli sonra işlenebilir XML verileri döndürür XMLHttp kod kullanıcıdan yeni WebResource.axd işleyicisi tarafından gizlenir.

ASP.NET 2.0 ile bir betik geri çağırma yapılandırmak için gereken birkaç adım vardır.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>1. adım: ICallbackEventHandler arabirim uygular.

Sayfanız bir betik geri katılan olarak tanınması ASP.NET için sırada ICallbackEventHandler arabirimini uygulaması gerekir. Bu, arka plan kod dosyasında yapabileceğiniz şu şekilde:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Ayrıca @ Implements yönerge benzeri kullanarak bunu yapabilirsiniz:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Genellikle, satır içi ASP.NET kodu kullanırken @ Implements yönergesinin kullanmanız gerekir.

## <a name="step-2--call-getcallbackeventreference"></a>2. adım: Çağrı GetCallbackEventReference

Daha önce belirtildiği gibi XMLHttp çağrı WebResource.axd işleyicisinin içinde kapsüllenir. ASP.NET WebForm bir çağrı, sayfa işlendiğinde ekleyeceksiniz\_DoCallback, WebResource.axd tarafından sağlanan bir istemci komut dosyası. WebForm\_DoCallback işlevi değiştirir \_ \_doPostBack işlevi için bir geri çağırma. Unutmayın \_ \_doPostBack program aracılığıyla sayfasındaki formu gönderir. Bu nedenle, bir geri gönderme önlemek istediğiniz bir geri çağırma senaryosunda \_ \_doPostBack değil yeterli.

> [!NOTE]
> \_\_doPostBack hala sayfasına istemci betiği geri çağırma senaryosunda işlenir. Bununla birlikte, geri çağırma için kullanılmaz.


Bağımsız değişkenleri WebForm\_DoCallback istemci-tarafı işlevi, normalde sayfa içinde denilen GetCallbackEventReference sunucu tarafı işlev aracılığıyla sağlanan\_yük. Tipik bir GetCallbackEventReference çağrı şuna benzeyebilir:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> Bu durumda, cm ClientScriptManager örneğidir. ClientScriptManager sınıfı Bu modülün daha sonra ele alınacaktır.


GetCallbackEventReference birden fazla aşırı yüklenmiş sürümleri vardır. Bu durumda, bağımsız değişkenleri şunlardır:

`this`

Burada GetCallbackEventReference Aranan denetim başvuru. Bu durumda, sayfa olur.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

İstemci-tarafı koddan sunucu tarafı olaya geçirilen bir dize bağımsız değişkeni. Bu durumda, anlık ileti bir açılan değerini geçirme ddlCompany çağrılır.

`ShowCompanyName`

Dönüş değeri (dize) sunucu tarafı geri çağırma etkinlikten kabul eder istemci tarafı işlevinin adı. Bu işlev, yalnızca sunucu tarafı geri çağırma başarılı olduğunda çağrılır. Bu nedenle, sağlamlık açısından, genellikle bir hatanın olayı içerisinde yürütülmek üzere bir istemci-tarafı işlevin adını belirterek ek dize bağımsız değişken GetCallbackEventReference Aşırı yüklenen sürümünü kullanmanız önerilir.

`null`

Sunucuya geri çağırma önce başlatılan bir istemci-tarafı işlevi temsil eden bir dize. Bu durumda, bağımsız değişken null, bu nedenle böyle herhangi bir komut yoktur.

`true`

Zaman uyumsuz geri çağırma kuralları gerekip gerekmediğini belirten bir Boole değeri.

WebForm çağrısı\_DoCallback istemcide, bu bağımsız değişkenler geçecek. Bu nedenle, bu sayfayı istemcide işlendiğinde bu kodu görüneceğini şu şekilde:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

İstemci üzerinde işlev imzası biraz farklı olduğuna dikkat edin. İstemci tarafı işlevi 5 dize ve Boole geçirir. Sunucu tarafı geri çağırma tüm hataları işleyecek istemci-tarafı işlevi (Bu, yukarıdaki örnekte null) olan ek dizeyi içerir.

## <a name="step-3--hook-the-client-side-control-event"></a>3. adım: istemci tarafı denetim olayının bağlama

Yukarıdaki GetCallbackEventReference dönüş değeri dize değişkenine atandığını dikkat edin. Bu dize, geri çağırmayı başlatan denetimi için istemci tarafı bir olay bağlama için kullanılır. Bağlamak istediğiniz şekilde bu örnekte geri çağırma sayfasında, açılan bir menüde başlatılır *OnChange* olay.

İstemci tarafında olay bağlama için bir işleyici için istemci tarafı biçimlendirme gibi eklemeniz yeterlidir:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Bu geri çağırma *cbRef* GetCallbackEventReference çağrıdan dönüş değeridir. WebForm çağrısı içerdiği\_yukarıda gösterilen DoCallback.

## <a name="step-4--register-the-client-side-script"></a>4. adım: istemci tarafı komut dosyası kaydetme

İstemci tarafı komut dosyası adında GetCallbackEventReference çağrısı belirtilen geri çağırma **ShowCompanyName** sunucu tarafı geri çağırma işlemi başarılı olduğunda yürütülmesi. Bu betik bir ClientScriptManager örneği kullanarak sayfaya eklenmesi gerekir. (Daha sonra bu modüldeki dicussed ClientScriptManager sınıfı olacaktır.) Bu benzer şekilde yapın:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>5. adım: ICallbackEventHandler arabirimin yöntemlerini çağırın

ICallbackEventHandler kodunuzda uygulamak için gereken iki yöntemi içerir. Bunlar **RaiseCallbackEvent** ve **GetCallbackEvent**.

**RaiseCallbackEvent** bağımsız değişken olarak bir dize alır ve nothing döndürür. İstemci tarafı çağrısından WebForm geçirilen dize bağımsız değişken\_DoCallback. Bu durumda, bu değeri olduğu *değer* ddlCompany adlı açılan özniteliği. Sunucu tarafı kodunuzu RaiseCallbackEvent yöntemi yerleştirilmelidir. Örneğin, bir dış kaynağa karşı WebRequest çağırmanıza yapıyor, kod içinde RaiseCallbackEvent yerleştirilmelidir.

**GetCallbackEvent** istemciye geri dönüşü işlemekten sorumludur. Hiçbir bağımsız değişkeni alır ve bir dize döndürür. Döndürdüğü dize bağımsız değişken olarak istemci tarafı işleve, bu durumda geçirilir *ShowCompanyName*.

Yukarıdaki adımları tamamladıktan sonra ASP.NET 2.0 ile bir betik geri çağırma işlemini gerçekleştirmek hazır olursunuz.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Açık tam ekran görüntü](the-asp-net-2-0-page-model/_static/callback1.wmv)


ASP.NET'te betik geri aramaları yapmayı XMLHttp çağrıları destekleyen herhangi bir tarayıcıda desteklenir. Tüm modern tarayıcılarda kullanımda bugün içerir. Modern tarayıcılar (gelecek IE 7 dahil) bir iç XMLHttp nesnesine kullanırken Internet Explorer XMLHttp ActiveX nesnesini kullanır. Program aracılığıyla bir tarayıcı geri çağırmaları destekliyorsa, kullanabileceğiniz belirlemek için **Request.Browser.SupportCallback** özelliği. Bu özellik döndüreceği **true** istekte bulunan istemciye betik geri çağırmaları destekliyorsa.

## <a name="working-with-client-script-in-aspnet-20"></a>ASP.NET 2.0 istemci komut dosyası ile çalışma

İstemci betiklerini ASP.NET 2.0 ClientScriptManager sınıfının kullanımı yönetilir. ClientScriptManager sınıfı, bir türü ve adı'nı kullanarak istemci betikleri izler. Bu, aynı komut dosyasını program aracılığıyla bir sayfada birden çok kez eklenen öğesinden engeller.

> [!NOTE]
> Bir komut dosyası başarıyla bir sayfada kaydedildikten sonra aynı komut dosyasını kaydetmek için sonraki denemelere yalnızca ikinci kez kaydedilmemiş betikte neden olur. Yinelenen betik eklenir ve hiçbir özel durum oluşur. Gereksiz hesaplama önlemek için böylece birden çok kez kaydetmek çalışmayın bir betik zaten kayıtlı olup olmadığını belirlemek için kullanabileceğiniz yöntemler vardır.


ClientScriptManager yöntemlerini tüm geçerli ASP.NET geliştiricilerinin aşina olmanız gerekir:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Bu yöntem, işlenen sayfanın üst için bir komut dosyası ekler. Bu, istemci üzerinde çağrılır açıkça işlevleri eklemek için kullanışlıdır.

Bu yöntemin aşırı yüklenmiş iki sürümü vardır. Üç dört bağımsız değişken, bunlar arasında ortak olan. Bunlar:

`type (string)`

***Türü*** bağımsız değişken, komut dosyası için bir tür tanımlar. Genel sayfa türü (Bu. kullanmak iyi bir fikir olduğunu GetType()) türü.

`key (string)`

***Anahtar*** komut dosyası için bir kullanıcı tanımlı anahtar olmayan bağımsız değişken. Bu, her komut dosyası için benzersiz olmalıdır. Aynı anahtara ve zaten eklenmiş bir komut dosyası türü ile bir komut dosyası eklemeyi denediğinizde, eklenmeyecek.

`script (string)`

***Betik*** eklemek için gerçek betik içeren bir dizedir. Komut dosyası oluşturabilir ve sonra atamak için StringBuilder üzerinde ToString() yöntemini kullanmanız için StringBuilder kullanın önerilir ***betik*** bağımsız değişken.

Yalnızca üç bağımsız değişkeni alan aşırı yüklenmiş RegisterClientScriptBlock kullanırsanız, kod öğeleri içermelidir (&lt;betik&gt; ve &lt;/SCRIPT&gt;) betiğinizde.

Dördüncü bir bağımsız değişken RegisterClientScriptBlock aşırı yüklemesini kullanmayı tercih edebilirsiniz. Dördüncü bağımsız değişken ASP.NET kod öğeleri, eklemelisiniz olup olmadığını belirten Boolean bir değer var. Bu bağımsız değişken ise **true**, kodunuzu betik öğeleri açıkça içermemelidir.

Bir betik zaten kayıtlı olup olmadığını belirlemek için IsClientScriptBlockRegistered yöntemi kullanın. Bu, zaten kayıtlı bir betiği yeniden kaydetme girişiminde önlemek sağlar.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (yeni, 2.0)

RegisterClientScriptInclude etiketi, harici bir komut dosyasına bağlanan bir betik bloğu oluşturur. Bu iki aşırı yüklemesi vardır. Bir anahtarı ve bir URL alır. İkinci türünü belirterek üçüncü bir bağımsız değişkeni ekler.

Örneğin, aşağıdaki kod, bir komut dosyası bloğu jsfunctions.js bağlanan scripts klasörü uygulamasının kök dizininde oluşturur:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Bu kod aşağıdaki kodda işlenen sayfa üretir:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Betik bloğundaki sayfasının altında oluşturulur.


Bir betik zaten kayıtlı olup olmadığını belirlemek için IsClientScriptIncludeRegistered yöntemi kullanın. Bu, bir betiği yeniden kaydetme girişiminde önlemek sağlar.

## <a name="registerstartupscript"></a>RegisterStartupScript

RegisterStartupScript yöntem RegisterClientScriptBlock yöntemi olarak aynı bağımsız değişkenleri alır. Sayfa yüklendikten sonra ancak önce yüklendiğinde istemci-tarafı olayı RegisterStartupScript ile kayıtlı bir betik yürütür. 1.X kapatmadan önce RegisterStartupScript ile kayıtlı bir betikleri yerleştirildi &lt;/form&gt; RegisterClientScriptBlock ile kayıtlı bir betikleri açıldıktan hemen sonra yerleştirildi sırada etiketi &lt;formu&gt; etiketi. ASP.NET 2. 0'da, her ikisi de kapatılmadan hemen önce yerleştirilir &lt;/form&gt; etiketi.

> [!NOTE]
> Bir işlev ile RegisterStartupScript kaydederseniz, bu işlevi açıkça istemci-tarafı kodunda çağırana kadar yürütülmez.


Bir betik zaten kayıtlı olup olmadığını belirlemek ve bir betik yeniden kaydetme girişiminde önlemek için IsStartupScriptRegistered yöntemi kullanın.

## <a name="other-clientscriptmanager-methods"></a>Diğer ClientScriptManager yöntemleri

Diğer kullanışlı yöntem ClientScriptManager sınıfın bazıları aşağıda verilmiştir.


|  <strong>GetCallbackEventReference</strong>   |                                                 Bu modüldeki betik geri çağırmaları bakın.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                JavaScript başvurusu alır (javascript:&lt;çağrı&gt;) bir istemci-tarafı olayından göndermek için kullanılabilir.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Bir posta istemcisinden geri başlatmak için kullanılan bir dize alır.                                    |
|      <strong>GetWebResourceUrl</strong>       | Bir derlemede gömülü bir kaynak için bir URL döndürür. İle birlikte kullanılmalıdır <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Bir Web kaynağı ile sayfaya kaydeder. Bu yeni WebResource.axd işleyici tarafından işlenir ve bir derlemede gömülü kaynaklardır.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Gizli bir form alanı sayfasıyla kaydeder.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  HTML form gönderildiğinde yürüten istemci-tarafı kodunu kaydeder.                                   |

