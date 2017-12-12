---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 sayfa modeli | Microsoft Docs
author: microsoft
description: "ASP.NET 1.x, geliştiricilerin sahip bir satır içi kod modeli ve arka plan kodu kod modeli arasında bir seçim. Arka plan kodu Src attr kullanarak uygulanabilir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: e008f197cf08bec81c560018f2d42306598f9e6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="the-aspnet-20-page-model"></a>ASP.NET 2.0 sayfa modeli
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET 1.x, geliştiricilerin sahip bir satır içi kod modeli ve arka plan kodu kod modeli arasında bir seçim. Arka plan kodu Src özniteliği veya arkasındaki koda özniteliği kullanılarak gerçekleştirilen @Page yönergesi. ASP.NET 2. 0'da, geliştiricilerin hala satır içi kod ve arka plan kodu arasında bir seçim var, ancak arka plan kod modeli önemli geliştirmeler olmuştur.


ASP.NET 1.x, geliştiricilerin sahip bir satır içi kod modeli ve arka plan kodu kod modeli arasında bir seçim. Arka plan kodu Src özniteliği veya arkasındaki koda özniteliği kullanılarak gerçekleştirilen @Page yönergesi. ASP.NET 2. 0'da, geliştiricilerin hala satır içi kod ve arka plan kodu arasında bir seçim var, ancak arka plan kod modeli önemli geliştirmeler olmuştur.

## <a name="improvements-in-the-code-behind-model"></a>Arka plan kod modeli yenilikleri

Hızlı şekilde modeli gözden geçirmek için en iyi, ASP.NET 2.0 arka plan kodu modelindeki değişikliklere tam olarak anlamak için ASP.NET var 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>ASP.NET arka plan kodu modelinde 1.x

ASP.NET 1.x, arka plan kod modeli oluşmuştur ASPX dosyası (Webform) ve programlama kodu içeren bir arka plan kod dosyası. İki dosya kullanarak bağlanmış @Page ASPX dosyasındaki yönergesi. Her denetim ASPX sayfasında karşılık gelen bir bildirimi arka plan kod dosyasına bir örnek değişkeni vardı. Arka plan kod dosyası ayrıca olay bağlama kodunu bulunan ve oluşturulan kodda Visual Studio tasarımcısı için gerekli. Bu model oldukça iyi çalıştı, ancak her ASP.NET öğesi ASPX sayfasında arka plan kod dosyasına karşılık gelen kod gerektirdiğinden, kod ve içerik true hiçbir ayrılması vardı. Bir tasarımcı yeni bir sunucu denetimi Visual Studio IDE dışında ASPX dosyası eklediyseniz, örneğin, uygulama bu denetim için bir bildirim olmaması nedeniyle arka plan kod dosyasına çalışmamasına neden.

## <a name="the-code-behind-model-in-aspnet-20"></a>ASP.NET 2.0 arka plan kod modeli

ASP.NET 2.0 Bu model önemli ölçüde artırır. ASP.NET 2. 0'da, arka plan kodu kullanarak yeni uygulanan *kısmi sınıflar* ASP.NET 2.0 ile sağlanan. Arka plandaki kod sınıfı, ASP.NET 2.0 definied sınıf tanımını yalnızca bir parçasının içerdiği anlamına bir kısmi sınıftır. Sınıf tanımı kalan kısmını çalışma zamanında veya Web sitesi önceden derlenmiş ASPX sayfasını kullanarak ASP.NET 2.0 tarafından dinamik olarak oluşturulur. Arka plan kod dosyası ve ASPX Sayfası arasındaki bağlantı hala @page yönergesi kullanılarak oluşturulur. Ancak, bir arkasındaki koda veya Src özniteliği yerine ASP.NET 2.0 CodeFile özniteliği şimdi kullanır. Inherits özniteliği de sayfa için sınıf adını belirtmek için kullanılır.

Tipik bir @page yönergesi şuna benzeyebilir:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Bir ASP.NET 2.0 arka plan kod dosyasına tipik sınıf tanımında şuna benzeyebilir:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# ve Visual Basic şu anda kısmi sınıflar destekleyen yönetilen diller yalnızca var. Bu nedenle, geliştiricilerin J# kullanarak ASP.NET 2.0 ile arka plan kod modeli kullanmanız mümkün olmaz.


Geliştiriciler artık, oluşturmuş olduğunuz kod içeren kod dosyaları olacağı için yeni model arka plan kodu modelini geliştirir. Hiçbir örnek değişken bildirimleri arka plan kod dosyasına olduğundan true ayrılması kodunuz ve içeriğinizle de sağlar.

> [!NOTE]
> Olay bağlama gerçekleştiği parçalı sınıf ASPX Sayfası olduğu için Visual Basic geliştiricileri olayları bağlanacak kod arkasında tanıtıcıları anahtar sözcüğünü kullanarak hafif performans artışı sağlarsınız. C# eşdeğer anahtar sözcük vardır.


## <a name="new--page-directive-attributes"></a>Yeni @ sayfa yönergesi öznitelikler

ASP.NET 2.0 @page yönergesi için birçok yeni öznitelikler ekler. Aşağıdaki öznitelikler, ASP.NET 2.0 ile yenidir.

## <a name="async"></a>Zaman Uyumsuz

Async özniteliği, zaman uyumsuz olarak yürütülecek sayfa yapılandırmanıza olanak sağlar. Zaman uyumsuz sayfalarında daha sonra bu modül de kapsar.

## <a name="asynctimeout"></a>AsyncTimeout

Zaman uyumsuz sayfalar için zaman aşımı belirtildi. Varsayılan değer 45 saniyedir.

## <a name="codefile"></a>CodeFile

CodeFile özniteliği arkasındaki koda özniteliği, Visual Studio 2002/2003 yerini alır.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

CodeFileBaseClass özniteliği tek bir taban sınıftan türetilen birden çok sayfa istediğiniz durumlarda kullanılır. ASP.NET, kısmi sınıflar bu özniteliği olmadan uygulanması nedeniyle paylaşılan ortak alanları bir ASPX sayfasında bildirilen denetimleri başvurmak için kullandığı bir temel sınıf düzgün çünkü çalışmayan ASP. Ağ derleme altyapısı sayfasındaki denetimleri dayalı yeni üyeler otomatik olarak oluşturur. Bu nedenle, ASP.NET iki veya daha fazla sayfalar için ortak bir taban sınıf istiyorsanız tanımlamak gerekecektir CodeFileBaseClass özniteliğinde, taban sınıfı belirtin ve ardından her sayfa sınıfı, temel sınıfından türetilir. Bu öznitelik kullanıldığında CodeFile özniteliği de gereklidir.

## <a name="compilationmode"></a>compilationMode

Bu öznitelik ASPX sayfasının CompilationMode özelliği ayarlamanıza olanak sağlar. Değerleri içeren bir numaralandırma CompilationMode özelliktir **her zaman**, **otomatik**, ve **hiçbir zaman**. Varsayılan değer **her zaman**. **Otomatik** ayarı, ASP.NET dinamik olarak sayfa mümkünse derlemesini engeller. Dinamik derlemeden sayfaları hariç performansı artırır. Ancak, hariç tutulan bir sayfa derlenmelidir bu kodu içeriyorsa, bir hata sayfası tarandığında oluşturulacaktır.

## <a name="enableeventvalidation"></a>EnableEventValidation

Bu öznitelik geri gönderme ve geri çağırma olayları doğrulanmış olup olmadığını belirtir. Bu etkin olduğunda, değişkenler geri gönderme veya geri çağırma olayları ilk olarak işlenen sunucu denetiminden kaynaklanan denetlenir.

## <a name="enabletheming"></a>EnableTheming

Bu öznitelik, bir sayfa üzerinde kullanılan ASP.NET Temalar olup olmadığını belirtir. Varsayılan değer **false**. ASP.NET temaları ele alınmıştır [modülü 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Bu öznitelik, derleme sırasında satır pragmaları eklenmesi gerekip gerekmediğini belirtir. Satır pragmaları kodun belirli bölümlerine işaretlemek için hata ayıklayıcıları tarafından kullanılan seçeneklerdir.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Bu öznitelik Geri göndermeler arasında kaydırma konumunu korumak için sayfaya JavaScript eklenmiş olup olmadığını belirtir. Bu öznitelik **false** varsayılan olarak.

Bu öznitelik olduğunda **true**, ASP.NET ekleyecek bir &lt;betik&gt; şöyle geri gönderme blok:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Bu betik bloğu src WebResource.axd olduğuna dikkat edin. Bu kaynak fiziksel bir yol değil. Bu komut dosyası istendiğinde, ASP.NET betik dinamik olarak oluşturur.

### <a name="masterpagefile"></a>MasterPageFile

Bu öznitelik geçerli sayfa için ana sayfa dosyası belirtir. Yol, göreli veya mutlak olabilir. Ana sayfalar ele alınmıştır [modülü 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Bu öznitelik, bir ASP.NET 2.0 tema tarafından tanımlanan kullanıcı arabirimi görünüm özellikleri geçersiz kılmanıza olanak sağlar. Temalar ele alınmıştır [modülü 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Sayfa temasının belirtir. StyleSheetTheme özniteliği için bir değer belirtilmezse, öznitelikten sayfadaki denetimleri uygulanan tüm stillerini geçersiz kılar.

## <a name="title"></a>Başlık

Sayfa başlığını ayarlar. Burada belirtilen değer görüntülenir &lt;başlık&gt; işlenen sayfanın öğesi.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

ViewStateEncryptionMode numaralandırması için değeri ayarlar. Kullanılabilir değerler **her zaman**, **otomatik**, ve **hiçbir zaman**. Varsayılan değer **otomatik**. Bu öznitelik için bir değer ayarlandığında **otomatik**, viewstate şifrelenir denetim istekleri, çağırarak olan **RegisterRequiresViewStateEncryption** yöntemi.

## <a name="setting-public-property-values-via-the--page-directive"></a>Ortak özellik değerlerinin aracılığıyla @ sayfa yönergesi

Başka bir yeni özelliği ASP.NET 2.0 @page yönergesinde, ortak bir taban sınıf özelliklerini ilk değerini ayarlamak için yeteneğidir. Örneğin, bir genel özelliğe sahip adlı varsayalım **SomeText** temel sınıfınız ve buna benzer d için başlatılması için **Hello** ne zaman bir sayfa yüklenir. Bu değer yalnızca @page yönergesinde ayarlayarak gerçekleştirmek sözlüğüdür:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** @ sayfa yönergesi özniteliği için temel sınıf SomeText özelliğinin ilk değeri ayarlar *Hello!*. Video @page yönergesi kullanarak bir taban sınıf içinde bir genel özelliğinin ilk değeri ayarlamanın bir kılavuz ' dir.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Açık Tam Ekran Video](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Sayfa sınıfının yeni ortak özellikleri

Aşağıdaki genel özellikleri ASP.NET 2. 0 ' yenidir.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Uygulama göreli yolu sayfasının veya denetiminin döndürür. Örneğin, http://app/folder/page.aspx bulunan bir sayfa için özelliğin döndürdüğü ~ / klasör /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Göreli sanal dizin yolu sayfasının veya denetiminin döndürür. Örneğin özellik http://app/folder/page.aspx yer alan için bir sayfa döndürür ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Alır veya zaman uyumsuz sayfa işleme için kullanılan zaman aşımı ayarlar. (Zaman uyumsuz sayfaları Bu modülün daha sonra ele alınacaktır.)

## <a name="clientquerystring"></a>ClientQueryString

İstenen URL sorgu dizesi bölümünü döndürür salt okunur özellik. URL kodlanmış değerdir. Bunu çözmek için HttpServerUtility sınıfının UrlDecode yöntemini kullanabilirsiniz.

## <a name="clientscript"></a>ClientScript

Bu özellik, istemci tarafı komut dosyası ASP.NETs yayımlanmasını yönetmek için kullanılan bir ClientScriptManager nesnesi döndürür. (ClientScriptManager sınıfı daha sonra bu modülde ele alınmıştır.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Bu özellik, olay doğrulama geri gönderme ve geri çağırma olaylar için etkin denetler. Etkin olduğunda, değişkenler geri gönderme veya geri çağırma olayları ilk olarak işlenen sunucu denetiminden kaynaklanan emin olmak için doğrulanır.

## <a name="enabletheming"></a>EnableTheming

Bu özellik alır veya ASP.NET 2.0 tema sayfasına uygulanıp uygulanmayacağını belirten bir Boole değeri ayarlar.

## <a name="form"></a>Form

Bu özellik HTML formu ASPX sayfasında HtmlForm nesne olarak döndürür.

## <a name="header"></a>Üstbilgi

Bu özellik, bir nesneye başvuru sayfa üstbilgisi içeren HtmlHead döndürür. Döndürülen HtmlHead nesne get/set için stil sayfaları, Meta etiketleri kullanabilirsiniz.

## <a name="idseparator"></a>IdSeparator

Bu özelliği salt okunur bir sayfadaki denetimleri için benzersiz bir kimlik ASP.NET oluştururken denetim tanımlayıcıları ayırmak için kullanılan karakteri alır. Doğrudan kodunuzdan kullanılmaya yönelik değildir.

## <a name="isasync"></a>Isasync

Bu özellik için zaman uyumsuz sayfaları sağlar. Zaman uyumsuz sayfaları daha sonra bu modülde ele alınmıştır.

## <a name="iscallback"></a>IsCallback

Bu salt okunur özellik döndürür **true** sayfa geri arama sonucu ise. Geri daha sonra bu modülde ele alınmıştır.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Bu salt okunur özellik döndürür **true** sayfa Sayfalar arası geri gönderimin parçası olduğunda. Çapraz sayfa Geri göndermeler daha sonra bu modülde ele alınmıştır.

## <a name="items"></a>Öğeler

IDictionary örneğine sayfaları bağlamda depolanan tüm nesnelerini içeren bir başvuru döndürür. Bunlar için bağlam ömrü kullanılabilir ve bu IDictionary nesnesine öğeleri ekleyebilirsiniz.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Bu özellik, ASP.NET geri gönderimin gerçekleştikten sonra sayfaları tarayıcının konumda kaydırma tutar JavaScript destekleyip desteklemediğini yayar denetler. (Bu özellik ayrıntılarını Bu modülün daha önce bahsedilen.)

## <a name="master"></a>Ana

Bu salt okunur özellik AnaSayfa örneğine bir ana sayfa uygulanmış bir sayfa için bir başvuru döndürür.

## <a name="masterpagefile"></a>MasterPageFile

Alır veya ayarlar sayfası için ana sayfa dosya adı. Bu özellik yalnızca PreInit yönteminde ayarlayabilirsiniz.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Bu özellik alır veya en fazla sayfa durumu için bayt cinsinden ayarlar. Özelliği pozitif bir sayı olduğunda, böylece belirtilen bayt sayısını aşmadığından sayfaları görünüm durumu birden çok gizli alanlarına ayrılmış olarak gösterilir. Özellik negatif bir sayı ise, Görünüm durumu parçalara bozuk değil.

## <a name="pageadapter"></a>PageAdapter

Sayfa için istekte bulunan tarayıcıyı değiştirir PageAdapter nesnesine bir başvuru döndürür.

## <a name="previouspage"></a>PreviousPage

Önceki sayfaya başvuru bir Server.Transfer veya çapraz sayfa geri gönderimin durumlarda döndürür.

## <a name="skinid"></a>SkinID değerine

Sayfaya uygulamak için ASP.NET 2.0 kaplama belirtir.

## <a name="stylesheettheme"></a>StyleSheetTheme

Bu özellik alır veya bir sayfaya uygulanan stil sayfasının ayarlar.

## <a name="templatecontrol"></a>TemplateControl

Sayfa için içeren denetlemek için bir başvuru döndürür.

## <a name="theme"></a>Tema

Alır veya sayfaya uygulanan ASP.NET 2.0 tema adını ayarlar. Bu değer PreInit yöntemi önce ayarlanmalıdır.

## <a name="title"></a>Başlık

Bu özellik alır veya sayfanın başlığı sayfaları başlığından edinildiği şekilde ayarlar.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Alır veya sayfanın ViewStateEncryptionMode ayarlar. Bu özellik bu modüldeki hakkında ayrıntılı bilgi bakın.

## <a name="new-protected-properties-of-the-page-class"></a>Sayfa sınıfının yeni korumalı Özellikler

ASP.NET 2.0 sayfa sınıfının yeni korumalı özellikleri şunlardır:

## <a name="adapter"></a>Bağdaştırıcı

Cihaz sayfasında işleyen ControlAdapter başvuru istendiğinde döndürür.

## <a name="asyncmode"></a>AsyncMode

Bu özellik sayfası zaman uyumsuz olarak işlenir olup olmadığını gösterir. Bu kodu doğrudan de, çalışma zamanı tarafından kullanılmaya yöneliktir.

## <a name="clientidseparator"></a>ClientIDSeparator

Bu özellik benzersiz istemci denetimleri için kimlikleri oluştururken ayırıcı olarak kullanılan karakter döndürür. Bu kodu doğrudan de, çalışma zamanı tarafından kullanılmaya yöneliktir.

## <a name="pagestatepersister"></a>PageStatePersister

Bu özellik sayfası için PageStatePersister nesnesini döndürür. Bu özellik, öncelikli olarak ASP.NET denetim geliştiriciler tarafından kullanılır.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Bu özellik, tarayıcılar önbelleğe alma için dosya yolu eklenen benzersiz bir suffic döndürür. Varsayılan değer \_ \_ufps = ve 6 basamaklı bir sayı.

## <a name="new-public-methods-for-the-page-class"></a>Sayfa sınıfı için yeni genel yöntemler

Aşağıdaki genel yöntemler, ASP.NET 2.0 sayfa sınıfında yenidir.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Bu yöntem, olay işleyici temsilcileri zaman uyumsuz sayfa yürütme için kaydeder. Zaman uyumsuz sayfaları daha sonra bu modülde ele alınmıştır.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Stili yaprak özelliklerinde sayfasına uygulanır.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Bu yöntem sorularınızı zaman uyumsuz bir görevi.

### <a name="getvalidators"></a>GetValidators

Hiçbiri belirtilmezse belirtilen doğrulama grubunun veya varsayılan doğrulama grubunun doğrulayıcıları koleksiyonunu döndürür.

## <a name="registerasynctask"></a>RegisterAsyncTask

Bu yöntem yeni bir zaman uyumsuz görev kaydeder. Zaman uyumsuz sayfaları daha sonra bu modülde ele alınmıştır.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Bu yöntem ASP.NET sayfaları denetim durumu kalıcı olduğunu söyler.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Bu yöntem ASP.NET sayfaları viewstate şifreleme gerektiren söyler.

## <a name="resolveclienturl"></a>ResolveClientUrl

Görüntüler, vb. için istemci istekleri için kullanılan göreli bir URL döndürür.

## <a name="setfocus"></a>SetFocus

Bu yöntem odağı sayfa ilk kez yüklendiğinde, belirttiğiniz denetimi için ayarlar.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Bu yöntem artık denetim durumu kalıcılığını gerektiren olarak geçirilen denetim kaldırır.

## <a name="changes-to-the-page-lifecycle"></a>Sayfa yaşam döngüsü yapılan değişiklikler

ASP.NET 2.0 sayfa çevriminin önemli ölçüde değişmediğinden, ancak bilincinde olmanız gereken bazı yeni yöntemler vardır. ASP.NET 2.0 sayfa yaşam döngüsü, aşağıda gösterilmiştir.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (yeni, ASP.NET 2.0)

Bir geliştirici erişebilirsiniz yaşam döngüsü erken aşamada PreInit olayıdır. Bu olay eklenmesi programlı olarak ASP.NET 2.0 temalar, ana sayfalar değiştirmek, bir ASP.NET 2.0 profili, vb. için özelliklerine erişmek mümkün kılar. En önemli nokta Viewstate henüz yaşam döngüsü bu noktada denetimlerine uygulandığını değil hayata geçirmek bir geri gönderme durumda varsa. Bir geliştirici bu aşamada bir denetimin bir özelliğini değişirse, bu nedenle, büyük olasılıkla daha sonra sayfa çevriminin üzerine yazılacak.

## <a name="init"></a>Init

Init olayı ASP.NET tarafından değiştirilmediği 1.x. Okuma veya sayfanızda denetimlerin özelliklerini başlatmak istediğiniz budur. Bu aşama, ana sayfalar, temalar vb. zaten sayfasına uygulanır.

## <a name="initcomplete-new-in-20"></a>InitComplete (yeni, 2.0)

InitComplete olay sayfaları başlatma aşaması sonunda çağrılır. Bu noktada çevriminin sayfadaki denetimleri erişebilirsiniz, ancak durumlarına değil henüz doldurulan.

## <a name="preload-new-in-20"></a>Önyükleme (2. 0'yeni)

Bu olay, tüm geri gönderme verileri uygulandıktan sonra ve sayfa önceki çağrılır\_yük.

## <a name="load"></a>Yükleme

Load olayı ASP.NET tarafından değiştirilmediği 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (yeni, 2.0)

LoadComplete son olay sayfaları yük aşamasında olaydır. Bu aşamada, tüm geri gönderme ve viewstate veri uygulandı sayfasına.

## <a name="prerender"></a>PreRender

Sayfaya dinamik olarak eklenen denetimler için düzgün bir şekilde korunmasını viewstate isterseniz PreRender olayını bunları eklemek için son şansınızdır.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (yeni, 2.0)

PreRenderComplete aşamada tüm denetimler sayfaya eklendiğinden ve sayfa işlenmek üzere hazırdır. Son olay sayfaları viewstate kaydedilmeden önce gerçekleşti PreRenderComplete etkinliğidir.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (yeni, 2.0)

Tüm Sayfa görünüm durumu ve denetim durumu hemen kaydedildikten sonra SaveStateComplete olay çağrılır. Bu sayfa gerçekten tarayıcıya işlenmeden önce son olaydır.

## <a name="render"></a>İşleme

İşleme yöntemi ASP.NET bu yana değişmemiştir 1.x. Burada HtmlTextWriter başlatılır ve sayfa tarayıcıda görüntülenen budur.

## <a name="cross-page-postback-in-aspnet-20"></a>ASP.NET 2.0 Sayfalar arası geri gönderme

ASP.NET 1.x, Geri göndermeler aynı sayfaya göndermek için gerekli. Çapraz sayfa Geri göndermeler izin verilmiyor. ASP.NET 2.0 IButtonControl arabirimi aracılığıyla başka bir sayfaya geri gönderilecek yeteneği ekler. (Düğme, LinkButton ve üçüncü taraf özel denetimler yanı sıra ImageButton) yeni IButtonControl arabirimini uygulayan herhangi bir denetimi bu yeni işlevsellik PostBackUrl özniteliğinin kullanımı aracılığıyla yararlanabilir. Aşağıdaki kod, ikinci bir sayfaya yazılarını bir düğme denetimi gösterir.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Sayfa geri gönderildiğinde, geri gönderme başlatır sayfa ikinci sayfasında PreviousPage özelliği aracılığıyla erişilebilir. Bu işlev yeni WebForm uygulanır\_denetim başka bir sayfaya gönderdiğinde sayfasına ASP.NET 2.0 işleyen DoPostBackWithOptions istemci-tarafı işlevi. Bu JavaScript işlevi, istemci betiği yayar yeni WebResource.axd işleyici tarafından sağlanır.

Video Sayfalar arası geri gönderimin bir kılavuz ' dir.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Açık Tam Ekran Video](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Çapraz sayfa Geri göndermeler hakkında daha fazla bilgi

### <a name="viewstate"></a>Görünüm durumu

Kendiniz zaten viewstate Sayfalar arası geri gönderme senaryosunda ilk sayfadan olabilecekler sorulan. Tüm IPostBackDataHandler uygulamıyor herhangi bir denetimi viewstate, aracılığıyla durumu nedenle Sihirbazın ikinci sayfasında bir çapraz sayfa geri gönderme denetleyen özelliklerini erişimi için korunur, sayfanın görünüm durumu için erişimi olması gerekir. ASP.NET 2.0 adlı ikinci sayfasında yeni bir gizli alan kullanarak bu senaryoyu mvc'deki \_ \_PREVIOUSPAGE. \_ \_PREVIOUSPAGE form alanı, tüm denetimler özelliklerine erişimi ikinci sayfasında böylece ilk sayfa için Görünüm durumu içerir.

### <a name="circumventing-findcontrol"></a>FindControl atlamak

Çapraz sayfa geri gönderimin video kılavuzda ı FindControl TextBox denetimi ilk sayfasında bir başvuru almak için kullanılan yöntem. Bu yöntem, bu amaçla iyi çalışır, ancak FindControl pahalıdır ve ek kod yazma gerektirir. Neyse ki, ASP.NET 2.0, birçok senaryoda çalışır, bu amaç için alternatif FindControl sağlar. PreviousPageType yönergesi TypeName veya VirtualPath özniteliği kullanılarak kesin türü belirtilmiş bir başvuru önceki sayfaya sahip olmanızı sağlar. TypeName özniteliği VirtualPath özniteliği sanal yolu kullanarak önceki sayfaya bakın sağlar, ancak önceki sayfaya türünü belirtmenize olanak tanır. PreviousPageType yönergesi ayarladıktan sonra daha sonra ortaya gerekir denetimleri vb. ortak özellikleri kullanılarak erişimine izin vermek istiyor.

## <a name="lab-1-cross-page-postback"></a>Laboratuvar 1 Sayfalar arası geri gönderme

Bu laboratuvarda, ASP.NET 2.0 yeni sayfalar arası geri gönderme işlevlerini kullanan bir uygulama oluşturacaksınız.

1. Visual Studio 2005 açın ve yeni bir ASP.NET Web sitesi oluşturun.
2. Page2.aspx adlı yeni bir Webform ekleyin.
3. Default.aspx Tasarım görünümünde açın ve bir düğmeyi ve TextBox denetimi ekleyin. 

    1. Düğme denetimi kimliği vermek **SubmitButton** ve metin kutusu denetim kimliği **kullanıcıadı**.
    2. Düğmenin PostBackUrl özelliği için page2.aspx ayarlayın.
4. Page2.aspx kaynağı görünümünü açın.
5. @ PreviousPageType yönergesi aşağıda gösterildiği gibi ekleyin:
6. Aşağıdaki kod sayfasına ekleme\_page2.aspx'ın arka plan kod yükünü: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Build menüsünden üzerinde yapı tıklayarak projeyi oluşturun.
8. Arka plan kod Default.aspx için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Sayfa değiştirme\_page2.aspx aşağıdaki yükleme: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Projeyi oluşturun.
11. Projeyi çalıştırın.
12. Metin kutusuna adınızı girin ve düğmesini tıklatın.
13. Sonuç nedir?

## <a name="asynchronous-pages-in-aspnet-20"></a>ASP.NET 2.0 zaman uyumsuz sayfaları

ASP.NET birçok Çekişme sorunları (örneğin, Web hizmeti veya veritabanı çağrıları) dış çağrıları, dosya g/ç gecikmesi gecikmesine neden olur. Bir ASP.NET uygulaması karşı bir istek yapıldığında, ASP.NET birini kendi iş parçacıklarını Bu isteğe hizmet vermek için kullanır. Bu istek, istek tamamlandıktan ve gönderilen yanıtı kadar o iş parçacığı sahip olur. Özellik sayfaları zaman uyumsuz olarak yürütülecek ekleyerek bu tür sorunları gecikmesi sorunları çözümlemek ASP.NET 2.0 arar. Bir çalışan iş parçacığı isteği başlatmak ve böylece kullanılabilir iş parçacığı havuzuna hızla döndürme başka bir iş parçacığı için ek yürütme kapalı el anlamına gelir. Dosya g/ç, veritabanı çağrısı, vb. tamamlandı, yeni bir iş parçacığı isteği tamamlamak için iş parçacığı havuzundan elde edilir.

Zaman uyumsuz yürütme bir sayfa yapmadan ilk adımı ayarlamaktır **zaman uyumsuz** sayfa yönergesi özniteliği şu şekilde:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Bu öznitelik sayfa IHttpAsyncHandler uygulamak için ASP.NET söyler.

Sonraki adım PreRender önce sayfasının ömrü içindeki bir noktada AddOnPreRenderCompleteAsync yöntemi çağırmaktır. (Bu yöntem genellikle sayfasında çağrılır\_yük.) AddOnPreRenderCompleteAsync yöntemi iki parametre alır; bir BeginEventHandler ve bir EndEventHandler. BeginEventHandler parametre olarak EndEventHandler sonra geçen bir IAsyncResult döndürür.

Aşağıdaki video bir zaman uyumsuz sayfa isteği bir kılavuz ' dir.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Açık Tam Ekran Video](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> EndEventHandler tamamlanana kadar bir zaman uyumsuz sayfası tarayıcıya işlemez. Hiçbir şüpheli ancak bazı geliştiriciler için zaman uyumsuz geri aramalar benzer olarak zaman uyumsuz isteği düşünün. Olmadıklarını hayata geçirmek önemlidir. Zaman uyumsuz isteklerine hizmet yeni istekler, böylece azaltma GÇ bağlı olması nedeniyle Çekişme, vb. için iş parçacığı havuzuna ilk çalışan iş parçacığı döndürülebilecek avantajdır.


## <a name="script-callbacks-in-aspnet-20"></a>ASP.NET 2.0 betik geri aramalar

Web geliştiricileri yolları geri arama ile ilişkili titremeyi önlemek için her zaman attıktan. ASP.NET 1.x, SmartNavigation titremeyi önlemenin en yaygın yöntem olsa da, istemci uygulaması karmaşıklığı nedeniyle bazı geliştiriciler için SmartNavigation sorunlarına neden. ASP.NET 2.0 betik geri aramalar ile bu sorunu giderir. Betik geri aramalar JavaScript aracılığıyla Web sunucusuna karşı istekler yapmasını XMLHttp kullanın. XMLHttp isteği sonra yönetilebilir XML verileri tarayıcının DOM döndürür. XMLHttp kod kullanıcıdan yeni WebResource.axd işleyici tarafından gizlenir.

ASP.NET 2.0 ile bir betik geri yapılandırmak için gerekli olan birkaç adım vardır.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>1. adım: ICallbackEventHandler arabirimini uygulama

Bir komut dosyası geri katılan olarak sayfanızı tanımak ASP.NET sırayla ICallbackEventHandler arabirimini uygulamalıdır. Arka plan kodu dosyanızda bunu şu şekilde:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Bu, @ Implements yönergesi benzer bunu kullanarak da yapabilirsiniz:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Satır içi ASP.NET kodunun kullanırken genellikle @ Implements yönergesi kullanırsınız.

## <a name="step-2--call-getcallbackeventreference"></a>2. adım: Çağrı GetCallbackEventReference

Daha önce belirtildiği gibi XMLHttp çağrısı WebResource.axd işleyicisinde kapsüllenir. Sayfanız işlendiğinde, ASP.NET WebForm yapılan bir çağrı ekleyin\_DoCallback, WebResource.axd tarafından sağlanan bir istemci komut dosyası. WebForm\_DoCallback işlevi değiştirir \_ \_doPostBack işlevi için bir geri çağırma. Unutmayın \_ \_doPostBack sayfasındaki formu programlı olarak gönderir. Bir geri çağırma senaryosunda, geri gönderimin, bu nedenle engellemek istediğiniz \_ \_doPostBack değil yeterli.

> [!NOTE]
> \_\_doPostBack hala bir istemci komut dosyası geri çağırma senaryosunda sayfasına işlenir. Ancak, geri çağırma için kullanılmaz.


WebForm bağımsız değişkenleri\_DoCallback istemci-tarafı işlevi, sunucu tarafı işlevi normalde sayfasında denilen GetCallbackEventReference aracılığıyla sağlanan\_yük. GetCallbackEventReference tipik bir çağrı şuna benzeyebilir:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> Bu durumda, cm ClientScriptManager örneğidir. ClientScriptManager sınıfı Bu modülün daha sonra ele alınacaktır.


GetCallbackEventReference birkaç aşırı yüklü sürümü vardır. Bu durumda, bağımsız değişkenleri şunlardır:

`this`

Burada GetCallbackEventReference çağrılan denetim referansı. Bu durumda, sayfa olur.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

İstemci tarafı kodu sunucu tarafı olaya geçirilen dize bağımsız değişkeni. Bu durumda, anlık ileti bir açılır liste değeri geçirme ddlCompany çağrılır.

`ShowCompanyName`

Dönüş değeri (dize) sunucu tarafı geri çağırma etkinlikten kabul eder istemci-tarafı işlevinin adı. Sunucu tarafı geri arama başarılı olduğunda bu işlev yalnızca çağrılmaz. Bu nedenle, sağlamlık amacıyla, genellikle bir hata durumunda yürütmek için bir istemci-tarafı işlevin adını belirterek bir ek dize bağımsız değişken GetCallbackEventReference aşırı yüklü sürümünü kullanmak için önerilir.

`null`

Sunucuya geri çağırma önce başlatılan bir istemci-tarafı işlevi temsil eden dize. Bu durumda olmadığından bu tür bir betik yok, bağımsız değişkeni null.

`true`

Geri çağırma zaman uyumsuz olarak gerçekleştir gerekip gerekmediğini belirten bir Boole değeri.

WebForm çağrısı\_DoCallback istemcide, bu bağımsız değişkenler geçecek. Bu nedenle, bu sayfayı istemcide işlendiğinde, bu kodu görüneceğini sözlüğüdür:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

İstemci üzerinde işlevinin imzası biraz farklı olduğuna dikkat edin. İstemci tarafı işlevi 5 dizeler ve bir Boole değeri geçirir. Sunucu tarafı geri aramadan hataları işleyecek istemci-tarafı işlevi (yukarıdaki örnekte null ise) ek dizesini içerir.

## <a name="step-3--hook-the-client-side-control-event"></a>3. adım: istemci-tarafı denetim olayı bağlayın

Yukarıdaki GetCallbackEventReference dönüş değeri bir dize değişkenine atanan dikkat edin. Bu dize, geri çağırma başlatır denetimi için istemci tarafı bir olay kanca için kullanılır. Kanca istediğiniz şekilde bu örnekte, geri çağırma sayfasında açılır başlatılır *değiştiğinde* olay.

İstemci tarafında olay kanca için yalnızca bir işleyicinin istemci-tarafı biçimlendirme aşağıdaki şekilde ekleyin:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Sözcüğünün *cbRef* GetCallbackEventReference çağrısı dönüş değeri. WebForm çağrısı içerdiği\_yukarıda gösterilen DoCallback.

## <a name="step-4--register-the-client-side-script"></a>4. adım: istemci tarafı komut dosyası kaydetme

İstemci tarafı komut dosyası denen GetCallbackEventReference çağrısı belirtilen geri çağırma **ShowCompanyName** sunucu tarafı geri arama başarılı olduğunda yürütülmesi. Bu komut dosyası ClientScriptManager örneği kullanarak sayfaya eklenmesi gerekir. (ClientScriptManager sınıfı daha sonra bu modüldeki dicussed olacaktır.) Bu benzer şekilde yapın:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>5. adım: ICallbackEventHandler arabiriminin yöntemlerini çağırın

ICallbackEventHandler kodunuzda uygulamanız gereken iki yöntem içerir. Bunlar **RaiseCallbackEvent** ve **GetCallbackEvent**.

**RaiseCallbackEvent** bağımsız değişken olarak bir dize alır ve hiçbir şey döndürür. Dize bağımsız değişkeni WebForm için istemci tarafı çağrısından geçirilen\_DoCallback. Bu durumda, bu değeri olan *değeri* ddlCompany adlı açılır özniteliğidir. Sunucu tarafı kodunuzu RaiseCallbackEvent yönteminde yerleştirilmelidir. Örneğin, geri WebRequest dış kaynak karşı değiştirirken, bu kod içinde RaiseCallbackEvent yerleştirilmelidir.

**GetCallbackEvent** geri dönüş istemciye işlemekten sorumludur. Bağımsız değişken almayan ve bir dize döndürür. Döndürdüğü dize bağımsız değişken olarak istemci tarafı işlevi için bu durumda geçirilir *ShowCompanyName*.

Yukarıdaki adımları tamamladıktan sonra ASP.NET 2.0 ile bir betik geri gerçekleştirmek hazır olursunuz.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Açık Tam Ekran Video](the-asp-net-2-0-page-model/_static/callback1.wmv)


ASP komut dosyasını geri aramalar yapmayı XMLHttp çağrıları destekleyen herhangi bir tarayıcısında desteklenir. Tüm modern tarayıcılar kullanımda bugün içerir. Modern tarayıcılar (yaklaşan IE 7 dahil) bir iç XMLHttp nesne kullanırken Internet Explorer XMLHttp ActiveX nesnesini kullanır. Program aracılığıyla bir tarayıcı geri aramalar destekliyorsa, kullanabileceğiniz belirlemek için **Request.Browser.SupportCallback** özelliği. Bu özellik döndürülecek **true** isteyen istemci komut dosyası geri aramalar destekliyorsa.

## <a name="working-with-client-script-in-aspnet-20"></a>ASP.NET 2.0 istemci komut dosyası ile çalışma

İstemci komut dosyalarını, ASP.NET 2.0 ClientScriptManager sınıfı kullanımı aracılığıyla yönetilir. ClientScriptManager sınıfı, bir türü ve adı'nı kullanarak istemci betikleri izler. Bu, aynı komut dosyasını program aracılığıyla bir sayfa üzerinde birden çok kez eklenmekte gelen önler.

> [!NOTE]
> Bir komut dosyası bir sayfa üzerinde başarıyla kaydedildikten sonra aynı komut dosyasını kaydetme sonraki girişimleri yalnızca ikinci kez kaydedilmemiş komut dosyasında neden olur. Yinelenen komut dosyası eklenir ve hiçbir özel durum oluşur. Gereksiz hesaplama önlemek için böylece birden çok kez kaydettirmeye çalışırsanız olmayan bir komut dosyası zaten kayıtlı olup olmadığını belirlemek için kullanabileceğiniz yöntemler vardır.


ClientScriptManager yöntemlerinin tüm geçerli ASP.NET geliştiricilerinin aşina olmanız gerekir:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Bu yöntem, bir komut dosyası işlenen sayfanın üst kısmına ekler. Bu, istemci üzerinde açıkça çağrılacağı işlevler eklemek için kullanışlıdır.

Bu yöntem aşırı yüklenmiş iki sürümü vardır. Üç dört bağımsız bunlar arasında ortak olan. Bunlar:

`type (string)`

***Türü*** bağımsız değişken, komut dosyası için bir tür tanımlar. Bu genellikle sayfa türü (Bu. kullanmak için iyi bir fikirdir GetType()) türü için.

`key (string)`

***Anahtar*** bağımsız değişkeni olan komut dosyası için bir kullanıcı tanımlı anahtar. Bu, her komut dosyası için benzersiz olmalıdır. Bir komut dosyası aynı anahtar ve zaten eklenmiş bir komut dosyası türünü eklemeye çalışırsanız, eklenmeyecek.

`script (string)`

***Betik*** bağımsız değişkeni eklemek için gerçek komut dosyasını içeren bir dize değil. Komut dosyası oluşturabilir ve ardından atamak için StringBuilder üzerinde ToString() yöntemini kullanmak için bir StringBuilder kullanmanız önerilir ***betik*** bağımsız değişkeni.

Yalnızca üç bağımsız değişken almayan aşırı yüklenmiş RegisterClientScriptBlock kullanırsanız, komut dosyası öğeleri içermelidir (&lt;betik&gt; ve &lt;/script&gt;) komut.

Dördüncü bir bağımsız değişken RegisterClientScriptBlock yüklemesini kullanmayı seçebilirsiniz. Dördüncü değişken ASP.NET komut dosyası öğeleri için eklemelisiniz olup olmadığını belirten bir Boole değeri. Bu bağımsız değişken ise **doğru**, kodunuzu komut dosyası öğeleri açıkça içermemelidir.

Bir komut dosyası zaten kayıtlı olup olmadığını belirlemek için IsClientScriptBlockRegistered yöntemini kullanın. Bu, zaten kayıtlı bir komut dosyası yeniden kaydetmek için girişiminde önlemek sağlar.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (yeni, 2.0)

RegisterClientScriptInclude etiket bir dış komut dosyası bağlanan bir betik bloğu oluşturur. İki aşırı yüklemeye sahip. Bir anahtarı ve bir URL alır. İkinci, üçüncü bağımsız değişken türünü belirleme ekler.

Örneğin, aşağıdaki kod betikler klasörüne uygulamanın kök dizininde jsfunctions.js bağlanan bir betik bloğu oluşturur:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Bu kod işlenen sayfa aşağıdaki kodda üretir:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Betik bloğu sayfasının en altında oluşturulur.


Bir komut dosyası zaten kayıtlı olup olmadığını belirlemek için IsClientScriptIncludeRegistered yöntemini kullanın. Bu, bir komut dosyası yeniden kaydetmek için girişiminde önlemek sağlar.

## <a name="registerstartupscript"></a>RegisterStartupScript

RegisterStartupScript yöntemi aynı bağımsız değişkenlere RegisterClientScriptBlock yöntemi alır. RegisterStartupScript ile kayıtlı bir komut dosyası, Sayfa yüklendikten sonra ancak yüklendiğinde istemci-tarafı olayı önce yürütür. 1.X içinde RegisterStartupScript ile kayıtlı komut dosyaları yalnızca kapatmadan önce yerleştirildi &lt;/form&gt; RegisterClientScriptBlock ile kayıtlı betikleri açtıktan hemen sonra yerleştirildi sırada etiketi &lt;form&gt; etiketi. ASP.NET 2. 0'da, her ikisi de hemen kapatmadan önce yerleştirilir &lt;/form&gt; etiketi.

> [!NOTE]
> Bir işlev ile RegisterStartupScript kaydolursanız, açıkça istemci-tarafı kodda çağrısı tamamlanana kadar bu işlev çalıştırmaz.


Bir komut dosyası zaten kayıtlı olup olmadığını belirlemek ve komut dosyası yeniden kaydetmek için girişiminde önlemek için IsStartupScriptRegistered yöntemini kullanın.

## <a name="other-clientscriptmanager-methods"></a>Diğer ClientScriptManager yöntemleri

Diğer kullanışlı yöntemler ClientScriptManager sınıfının bazıları aşağıda verilmiştir.

| **GetCallbackEventReference** | Bu modüldeki betik geri aramalar bakın. |
| --- | --- |
| **GetPostBackClientHyperlink** | JavaScript başvurusu alır (javascript:&lt;çağrısı&gt;) bir istemci-tarafı olayından göndermek için kullanılabilir. |
| **GetPostBackEventReference** | Bir posta istemcisinden geri başlatmak için kullanılan bir dize alır. |
| **GetWebResourceUrl** | Bir derlemede katıştırılmış bir kaynağı bir URL döndürür. İle birlikte kullanılmalıdır **RegisterClientScriptResource**. |
| **RegisterClientScriptResource** | Bir Web kaynağını sayfasıyla kaydeder. Bu derlemede katıştırılmış ve yeni WebResource.axd işleyicisi tarafından işlenen kaynaklardır. |
| **RegisterHiddenField** | Gizli bir form alanı sayfasıyla kaydeder. |
| **RegisterOnSubmitStatement** | HTML form gönderildiğinde yürütülen istemci-tarafı kodunun kaydeder. |
