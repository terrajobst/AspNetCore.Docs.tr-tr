---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: HTML5 ve jQuery UI Datepicker Popup Calendar ile ASP.NET MVC - bölüm 4 kullanarak | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar, ASP.NET MV ile çalışmaya ilişkin temel bilgileri sağlanır...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7ecd180b7608e82ea143575c6590574b92843dcf
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577502"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>HTML5 ve jQuery UI Datepicker Popup Calendar ile ASP.NET MVC - bölüm 4 kullanma
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Bu öğreticide Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar'içinde bir ASP.NET MVC Web uygulaması ile çalışmaya ilişkin temel bilgileri sağlanır.


### <a name="adding-a-template-for-editing-dates"></a>Tarihleri düzenleme için bir şablonu ekleme

Bu bölümde, ASP.NET MVC ile işaretlenmiş model özelliklerini düzenlemek için kullanıcı arabirimini görüntüler, uygulanacak tarihlerini düzenlemek için bir şablon oluşturursunuz **tarih** numaralandırması [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) rolüne. Şablon, yalnızca tarih işlenir; zaman görüntülenmez. Şablonda, kullanacağınız [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) açılan takvim tarihlerini düzenlemek için bir yol sağlar.

Başlamak için açın *Movie.cs* dosya ve ekleme [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) özniteliğini **tarih** sabit listesine `ReleaseDate` aşağıdaki kodda gösterildiği gibi özelliği:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Bu kod neden `ReleaseDate` hem de zaman şablonlarını görüntülemek ve şablonları düzenleme olmadan görüntülenecek alan. Uygulamanız içeriyorsa bir *date.cshtml* şablonunda *Views\Shared\EditorTemplates* klasör veya *Views\Movies\EditorTemplates* klasör, bu şablonu Tüm işlemek için kullanılan `DateTime` düzenlerken özelliği. Aksi takdirde yerleşik ASP.NET şablon oluşturma sistem özelliği olarak bir tarih görüntüler.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Yayın Tarihi Giriş alanı yalnızca tarih gösterildiğini doğrulamak için bir düzenleme bağlantısını seçin.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

İçinde **Çözüm Gezgini**, genişletme *görünümleri* klasörünü genişletin *paylaşılan* klasörünü açın ve ardından sağ *Views\Shared\EditorTemplates* klasör.

Tıklayın **Ekle**ve ardından **görünümü**. **Görünüm Ekle** iletişim kutusu görüntülenir.

İçinde **Görünüm adı** kutusuna &quot;tarih&quot;.

Seçin **kısmi görünüm olarak oluştur** onay kutusu. Emin olun **düzeni veya ana sayfayı kullan** ve **kesin türü belirtilmiş görünüm oluşturmak** onay kutuları seçili.

**Ekle**'yi tıklatın. *Views\Shared\EditorTemplates\Date.cshtml* şablonu oluşturulur.

Aşağıdaki kodu ekleyin *Views\Shared\EditorTemplates\Date.cshtml* şablonu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

İlk satır olacak şekilde modeli bildirir bir `DateTime` türü. Şablonları görüntüler ve düzenleme model türünde bildirmeniz gerekmez ancak derleme zamanı size en iyi uygulama, böylelikle görünüme geçirilen modelinin denetleniyor. (Başka bir avantajı, sonra IntelliSense için Visual Studio görünümünde model elde etmenizdir içindir.) ASP.NET MVC, model türü bildirilmedi varsa dikkate bir [dinamik](https://msdn.microsoft.com/library/dd264741.aspx) yazın ve derleme zamanı yoktur tür denetimi. Model olarak bildirirseniz bir `DateTime` türü, türü kesin belirlenmiş olur.

İkinci satır görüntüler yalnızca literal HTML biçimlendirmesi olan &quot;tarih şablonu kullanarak&quot; tarih alanı önce. Bu tarih şablon kullanılmakta olduğunu doğrulamak için bu satırı geçici olarak kullanacaksınız.

Sonraki satırı bir [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) işleyen yardımcı bir `input` alan bir metin kutusu. Üçüncü parametresi için yardımcı sınıfı için metin kutusu için anonim bir tür kullanır `datefield` ve tür `date`. (Çünkü `class` ayrılmış bir C# ' ta kullanmak gereken `@` kaçış karakteri `class` özniteliği C# ayrıştırıcısında.)

`date` HTML5 Takvim denetimi oluşturmak HTML5 kullanan tarayıcılar sağlayan bir HTML5 giriş türünü türüdür. Daha sonra bazı JavaScript'ler için jQuery datepicker denetime ekleyeceksiniz `Html.TextBox` öğesini kullanarak `datefield` sınıfı.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Doğrulayabilirsiniz `ReleaseDate` düzenleme Görünümü'nde özellik şablon görüntülediği için Düzen şablonunu kullanarak &quot;tarih şablonu kullanarak&quot; hemen önce `ReleaseDate` metin girişi kutusunu, bu görüntüde gösterildiği gibi:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Tarayıcınızda, sayfanın kaynağı görüntüleyin. (Örneğin, sayfanın sağ tıklayıp **kaynağı görüntüle**.) Aşağıdaki örnek, sayfa için biçimlendirme bazıları gösterilmektedir gösteren `class` ve `type` İşlenmiş HTML öznitelikleri.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Geri dönüp *Views\Shared\EditorTemplates\Date.cshtml* şablonu ekleme ve kaldırma &quot;tarih şablonu kullanarak&quot; biçimlendirme. Şimdi tamamlanan şablon aşağıdaki gibi görünür:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Bir jQuery UI Datepicker Popup Calendar'ı ekleme NuGet kullanma

Bu bölümde, ekleyeceksiniz [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) açılan takvimi tarih düzenleme şablon. [JQuery kullanıcı Arabirimi](http://jqueryui.com/) kitaplığı, Gelişmiş etkileri ve özelleştirilebilir pencere öğeleri animasyon için destek sağlar. Bu, jQuery JavaScript kitaplığı üzerinde oluşturulmuştur. Tarih Seçici açılan takvimi, kolay ve doğal bir dize girmek yerine bir takvimden tarih girmenizi sağlar. Açılan takvimi de yasal tarihleri kullanıcılara sınırlar — bir tarih için normal metin girişi gibi girmenize izin `2/33/1999` (Şubat 33rd, 1999), ancak [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) açılan takvimi, izin vermiyor.

İlk olarak, jQuery UI kitaplıkları'ı yüklemeniz gerekir. Bunu yapmak için NuGet, Visual Studio 2010 ve Visual Web Developer, SP1 sürümlerinde bulunan bir paket yöneticisidir kullanacaksınız.

Visual Web Developer gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi** seçip **NuGet paketlerini Yönet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Not: Varsa **Araçları** değil menüsünü görüntüleme **kitaplık Paket Yöneticisi** komutunu NuGet yönergeleri izleyerek yüklemeniz gerekir [NuGet yükleme](http://docs.nuget.org/docs/start-here/installing-nuget) sayfası NuGet Web sitesi.   
  
Visual Web Developer yerine Visual Studio'ya gelen kullanıyorsanız **Araçları** menüsünde **kitaplık Paket Yöneticisi** seçip **kitaplık paketi Başvurusu Ekle**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

İçinde **MVCMovie - NuGet paketlerini Yönet** iletişim kutusu, tıklayın **çevrimiçi** sekmesinde solda ve enter &quot;jQuery.UI&quot; arama kutusuna. J seçin **sorgu kullanıcı Arabirimi pencere öğeleri: Datepicker**, ardından **yükleme** düğmesi.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet, bu hata ayıklama ve jQuery UI çekirdek ve jQuery UI tarih seçici küçültülmüş sürümlerini projenize ekler:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Not: Hata ayıklama sürümleri (içermeyen dosyaları *. min.js* uzantısı) yararlı olan hata ayıklama, ancak bir üretim site içinde yalnızca küçültülmüş sürümleri içerir.

JQuery tarih seçici gerçekten kullandığınız için Takvim pencere öğesi için Düzen şablonunu denetime bir jQuery betiği oluşturmak gerekir. İçinde **Çözüm Gezgini**, sağ tıklayın *betikleri* klasörü ve select **Ekle**, ardından **yeni öğe**ve ardından **JScript Dosya**. Dosya adı *DatePickerReady.js*.

Aşağıdaki kodu ekleyin *DatePickerReady.js* dosyası:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

JQuery ile ilgili bilgi sahibi değilseniz, bu işlevin kısa bir açıklama şöyledir: ilk satırının &quot;jQuery hazır&quot; sayfasındaki tüm DOM öğeleri yüklendiğinde çağrılan işlev. İkinci satır sınıf adına sahip tüm DOM öğeleri seçer `datefield`, sonra çağıran `datepicker` her biri için işlevi. (Unutmayın, eklediğiniz `datefield` sınıfının *Views\Shared\EditorTemplates\Date.cshtml* şablon öğreticinin önceki bölümlerinde.)

Ardından, açık *görünümler/paylaşılan\\_Layout.cshtml* dosya. Tarih Seçici kullanabilmesi için tüm gerekli aşağıdaki dosyaları, başvurular eklemeniz gerekir:

- *Content/Themes/Base/JQuery.ui.Core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/Themes/Base/JQuery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

Aşağıdaki örnek, alt kısmında eklemelisiniz gerçek kod gösterir `head` öğesinde *görünümler/paylaşılan\\_Layout.cshtml* dosya.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Tam `head` bölüm burada gösterilmektedir:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL içerik Yardımcısı](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) yöntemi için mutlak yol kaynak yolu dönüştürür. Kullanmalısınız `@URL.Content` uygulama IIS üzerinde çalışırken bu kaynakları doğru şekilde başvurmak için.

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Düzenleme bağlantısını seçin ve ardından ekleme noktasını koymak **ReleaseDate** alan. JQuery UI popup calendar görüntülenir.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

JQuery denetimleri gibi datepicker kapsamlı bir şekilde özelleştirmenizi sağlar. Bilgi için [Visual özelleştirme: jQuery kullanıcı Arabirimi teması tasarlama](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) üzerinde [jQuery kullanıcı Arabirimi](http://learn.jquery.com/jquery-ui/getting-started/) site.

### <a name="supporting-the-html5-date-input-control"></a>HTML5 tarih giriş denetimini destekleme

Daha fazla tarayıcı HTML5 desteği gibi giriş, gibi yerel HTML5 kullanmak isteyeceksiniz `date` giriş öğesi ve jQuery kullanıcı Arabirimi takvimi kullanmayacak. Mantıksal uygulamanızı tarayıcı bunları destekliyorsa, HTML5 denetimleri otomatik olarak kullanmak üzere ekleyebilirsiniz. Bunu yapmak için içeriğini değiştirin *DatePickerReady.js* aşağıdaki dosya:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

Bu komut dosyası ilk satırını Modernizr HTML5 tarih girişi desteklendiğini doğrulamak için kullanır. Desteklenmiyorsa, jQuery UI tarih seçici yerine ölçekledikçe. ([Modernizr](http://www.modernizr.com/docs/) HTML5 ve CSS3 yerel uygulamaları kullanılabilirliğini algılayan bir açık kaynak JavaScript kitaplığı. Modernizr içinde oluşturduğunuz tüm yeni bir ASP.NET MVC projeleri dahil edilir.)

Bu değişikliği yaptıktan sonra Opera 11 gibi HTML5'i destekleyen bir tarayıcı kullanarak test edebilirsiniz. HTML5 ile uyumlu bir tarayıcı kullanarak uygulamayı çalıştırın ve bir film girişi düzenleyin. HTML5 tarih denetimi jQuery UI popup calendar yerine kullanılır:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Yeni sürüm tarayıcıların artımlı olarak HTML5 uygulama olduğundan, şimdi için iyi bir yaklaşım HTML5'i destekleyen çeşitli barındırır, Web sitesi kodu eklemektir. Örneğin, daha güçlü bir *DatePickerReady.js* betik yalnızca kısmen HTML5 tarih denetimini destekleyen site desteği tarayıcılar olanak tanıyan gösterilmiştir.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Bu betik, HTML5 seçer `input` türünde öğeler `date` , tam olarak desteklemez HTML5 tarih denetimi. Bu öğeler için jQuery UI popup calendar ' kancaları ve ardından değişiklikleri `type` özniteliğini `date` için `text`. Değiştirerek `type` özniteliğini `date` için `text`, kısmi HTML5 tarih desteği ortadan kalkar. Daha güçlü *DatePickerReady.js* betik bulunabilir [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Null olabilen tarihler için şablonları ekleme

Var olan tarih şablonlardan birini kullanın ve bir null tarih geçirmek, bir çalışma zamanı hatası alırsınız. Tarih şablonlarını daha sağlam hale getirmek için null değerler işlemek için bunları değiştireceksiniz. Boş değer atanabilir tarihleri desteklemek için kodda değişiklik *Views\Shared\DisplayTemplates\DateTime.cshtml* aşağıdaki:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Model olduğunda kodu boş bir dize döndürür. **null**.

Kodda değişiklik *Views\Shared\EditorTemplates\Date.cshtml* aşağıdaki dosya:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Bu kod çalıştırıldığında, model null değilse, modelin `DateTime` değeri kullanılır. Model null ise, geçerli tarihi yerine kullanılır.

### <a name="wrapup"></a>Wrapup

Bu öğreticide, ASP.NET şablonlu Yardımcılar temelleri ele ve jQuery UI datepicker popup calendar'ı bir ASP.NET MVC uygulamasındaki kullanma işlemini gösterir. Daha fazla bilgi için bu kaynakları deneyin:

- Yerelleştirme hakkında daha fazla bilgi için bkz: Rajeesh'ın blog [ASP.NET mvc'de JQueryUI Datepicker](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- JQuery kullanıcı Arabirimi hakkında daha fazla bilgi için bkz: [jQuery kullanıcı Arabirimi](http://docs.jquery.com/UI).
- Datepicker denetimi yerelleştirmek hakkında daha fazla bilgi için bkz. [Datepicker/kullanıcı Arabirimi/yerelleştirme](http://docs.jquery.com/UI/Datepicker/Localization).
- ASP.NET MVC şablonları hakkında daha fazla bilgi için Brad Wilson'ın blog dizisini bakın [ASP.NET MVC 2 şablonları](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Serinin ASP.NET MVC 2 için yazılmış olsa, malzeme ASP.NET MVC geçerli sürümü için geçerli olacaktır.

> [!div class="step-by-step"]
> [Önceki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
