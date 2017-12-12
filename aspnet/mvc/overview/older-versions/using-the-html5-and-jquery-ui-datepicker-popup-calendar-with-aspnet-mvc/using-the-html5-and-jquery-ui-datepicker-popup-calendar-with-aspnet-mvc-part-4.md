---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: "HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölümü 4 ile kullanma | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar ASP.NET MV içinde çalışmak nasıl temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2b76d2e449d491fd8d808343065b22ba267f1152
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>HTML5 ve jQuery UI Datepicker Popup Calendar ASP.NET MVC - bölümü 4 ile kullanma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Düzenleyicisi şablonları, görüntüleme şablonları ve jQuery UI datepicker popup calendar bir ASP.NET MVC Web uygulamasında çalışmak nasıl temellerini öğretmek.


### <a name="adding-a-template-for-editing-dates"></a>Tarihleri düzenlemek için bir şablon ekleme

Bu bölümde ASP.NET MVC ile işaretlenen model özelliklerini düzenlemek için kullanıcı Arabirimi görüntülediğinde, uygulanacak tarihleri düzenlemek için bir şablon oluşturacaksınız **tarih** numaralandırması [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) relationshipend. Şablon olarak yalnızca tarih kabul eder; zaman görüntülenmez. Şablonda kullanacağınız [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) açılan Takvim tarihleri düzenlemek için bir yol sağlar.

Başlamak için açın *Movie.cs* dosya ve ekleme [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) ile öznitelik **tarih** numaralandırma `ReleaseDate` aşağıdaki kodda gösterildiği gibi özelliği:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Bu kod neden `ReleaseDate` hem de zaman şablonlarını görüntülemek ve Şablonları Düzenle olmadan görüntülenecek alan. Uygulamanız içeriyorsa bir *date.cshtml* şablonunda *Views\Shared\EditorTemplates* klasör veya *Views\Movies\EditorTemplates* klasörü, bu şablonu herhangi bir işlemek için kullanılan `DateTime` düzenlerken özelliği. Aksi takdirde yerleşik ASP.NET şablon sistem özelliği bir tarih olarak görüntüler.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. Yayın Tarihi Giriş alanını yalnızca tarihi gösteren olduğunu doğrulamak için bir düzenleme bağlantısını seçin.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

İçinde **Çözüm Gezgini**, genişletin *görünümleri* klasörünü genişletin *paylaşılan* klasörünü ve ardından sağ tıklayarak *Views\Shared\EditorTemplates* klasör.

Tıklatın **Ekle**ve ardından **Görünüm**. **Görünüm Ekle** iletişim kutusu görüntülenir.

İçinde **Görünüm adı** kutusuna &quot;tarih&quot;.

Seçin **kısmi görünüm olarak oluştur** onay kutusu. Olduğundan emin olun **düzen veya ana sayfa kullanmak** ve **kesin türü belirtilmiş görünüm oluşturmak** onay kutularının seçili değil.

**Ekle**'yi tıklatın. *Views\Shared\EditorTemplates\Date.cshtml* şablonu oluşturulur.

Aşağıdaki kodu ekleyin *Views\Shared\EditorTemplates\Date.cshtml* şablonu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

İlk satırı olacak şekilde model bildiren bir `DateTime` türü. Model türü düzenleme bildirme ve şablonları görüntülemek gerekmeyen rağmen derleme zamanı alma en iyi uygulama, böylelikle görünümüne geçirilen modelinin denetleniyor. (Başka bir avantajı, ardından IntelliSense Visual Studio görünümünde modelde aldığını olur.) Model türü ile bildirilmedi, ASP.NET MVC bunu göz önünde bulundurur bir [dinamik](https://msdn.microsoft.com/en-us/library/dd264741.aspx) yazın ve hiçbir derleme süresi türü denetleniyor. Modelin olmasını bildirirseniz bir `DateTime` türü kesin türü belirtilmiş haline gelir.

İkinci satır görüntüler yalnızca sabit HTML biçimlendirmesi olan &quot;kullanarak tarih şablonunu&quot; tarih alanı önce. Bu tarih şablon kullanılmakta olduğunu doğrulamak için bu satırı geçici olarak kullanacaksınız.

Bir sonraki satır bir [Html.TextBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.inputextensions.textbox.aspx) işler yardımcı bir `input` bir metin kutusu alan. Metin kutusu için sınıf ayarlamak için anonim bir tür Yardımcısı üçüncü parametresi kullanır `datefield` ve tür `date`. (Çünkü `class` ayrılmış bir C# ' ta kullanmanıza gerek `@` kaçış karakteri `class` C# ayrıştırıcı özniteliğinde.)

`date` HTML5 Takvim denetimi oluşturmak HTML5 algılayan tarayıcılar sağlayan HTML5 input type türüdür. Daha sonra bazı JavaScript jQuery datepicker bağlanacağını ekleyeceksiniz `Html.TextBox` öğesi kullanılarak `datefield` sınıfı.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. Olduğunu doğrulayabilirsiniz `ReleaseDate` düzenleme görünümü özelliğinde şablon görüntülediğinden Düzen şablonu kullanarak &quot;kullanarak tarih şablonunu&quot; hemen önce `ReleaseDate` metin kutusunda, bu görüntüde gösterildiği gibi giriş:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Tarayıcınızda sayfasının kaynağı görüntüleyin. (Örneğin, sayfanın sağ tıklatın ve seçin **kaynağı görüntüle**.) Aşağıdaki örnek, sayfa için biçimlendirme bazıları gösterir gösteren `class` ve `type` işlenen HTML öznitelikleri.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Geri dönüp *Views\Shared\EditorTemplates\Date.cshtml* şablonu ekleme ve kaldırma &quot;kullanarak tarih şablonunu&quot; biçimlendirme. Artık tamamlanmış şablon şöyle görünür:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>JQuery UI Datepicker Popup Calendar ekleme NuGet kullanma

Bu bölümde, ekleyeceksiniz [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) açılan Takvim tarih düzenleme şablon. [JQuery UI](http://jqueryui.com/) kitaplığı etkiler ve özelleştirilebilir pencere öğeleri Gelişmiş animasyon için destek sağlar. JQuery JavaScript kitaplığı üzerinde oluşturulmuştur. Datepicker popup calendar kolay ve tarihleri bir dize girmek yerine bir takvim kullanarak girmek için doğal kolaylaştırır. Açılan Takvim de yasal tarihleri kullanıcılara sınırlar — normal metin girişi bir tarih için şöyle girmenize izin `2/33/1999` (Şubat 33rd, 1999), ancak [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) popup calendar, izin vermiyor.

İlk olarak, jQuery UI kitaplıkları yüklemeniz gerekir. Bunu yapmak için Visual Studio 2010 ve Visual Web Developer, SP1 sürümlerinde bulunan bir paket Yöneticisi NuGet kullanacaksınız.

Visual Web Developer, gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi** ve ardından **NuGet paketlerini Yönet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Not: Varsa **Araçları** değil menüsünü görüntüleme **kitaplık Paket Yöneticisi** komutu, yönergeleri izleyerek NuGet yüklemeniz gereken [yükleme NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) sayfası NuGet Web sitesi.   
  
Gelen Visual Web Developer yerine Visual Studio kullanıyorsanız, **Araçları** menüsünde, select **kitaplık Paket Yöneticisi** ve ardından **kitaplık paketi Başvurusu Ekle**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

İçinde **MVCMovie - NuGet paketlerini Yönet** iletişim kutusu, tıklatın **çevrimiçi** sol tarafta sekmesini ve ardından girin &quot;jQuery.UI&quot; arama kutusuna. J seçin **sorgu UI pencere öğeleri: Datepicker**seçeneğini belirleyip **yükleme** düğmesi.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet bu hata ayıklama ve jQuery UI çekirdek ve jQuery UI tarih seçici küçültülmüş sürümlerini projenize ekler:

- *JQuery.ui.Core.js*
- *JQuery.ui.Core.Min.js*
- *JQuery.ui.DatePicker.js*
- *JQuery.ui.DatePicker.Min.js*

Not: Hata ayıklama sürümleri (dosyaları olmadan *. min.js* uzantısı) yararlıdır hata ayıklama için ancak bir üretim site içinde yalnızca küçültülmüş sürümlerini içerir.

Gerçekte jQuery tarih seçici kullanmak için Takvim pencere öğesi düzenleme şablon bağlanacağını jQuery komut dosyası oluşturmanız gerekir. İçinde **Çözüm Gezgini**, sağ *komut dosyaları* klasörü ve seçin **Ekle**, ardından **yeni öğe**ve ardından **JScript Dosya**. Dosya adı *DatePickerReady.js*.

Aşağıdaki kodu ekleyin *DatePickerReady.js* dosyası:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

JQuery ile bilmiyorsanız bu yaptığı kısa bir açıklama şöyledir: ilk satır &quot;jQuery hazır&quot; sayfasındaki tüm DOM öğeleri yüklendiğinde çağrılan işlev. İkinci satır sınıfı adına sahip tüm DOM öğeleri seçer `datefield`, ardından çağırır `datepicker` her biri için işlevi. (Unutmayın, eklediğiniz `datefield` sınıfının *Views\Shared\EditorTemplates\Date.cshtml* şablonu öğreticinin önceki bölümlerinde.)

Ardından, açık *görünümler/paylaşılan\\_Layout.cshtml* dosya. Tarih Seçici kullanabilmesi için tüm gerekli aşağıdaki dosyaları başvurular eklemeniz gerekir:

- *Content/Themes/Base/JQuery.ui.Core.css*
- *Content/Themes/Base/JQuery.ui.DatePicker.css*
- *Content/Themes/Base/JQuery.ui.theme.css*
- *JQuery.ui.Core.Min.js*
- *JQuery.ui.DatePicker.Min.js*
- *DatePickerReady.js*

Aşağıdaki örnek, alt kısmındaki eklemelisiniz gerçek kod gösterir `head` öğesinde *görünümler/paylaşılan\\_Layout.cshtml* dosya.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Tam `head` bölüm burada gösterilmiştir:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL içerik Yardımcısı](https://msdn.microsoft.com/en-us/library/system.web.mvc.urlhelper.content.aspx) yöntemi kaynak yolu mutlak bir yol dönüştürür. Kullanmalısınız `@URL.Content` uygulama IIS üzerinde çalışırken bu kaynakları doğru şekilde başvurmak için.

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. Düzenleme bağlantısını seçin ve ardından ekleme noktasını put **ReleaseDate** alan. JQuery UI popup calendar görüntülenir.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Çoğu jQuery denetimleri gibi datepicker yaygın özelleştirmenizi sağlar. Bilgi için bkz: [görsel özelleştirme: jQuery UI Tema tasarlama](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) üzerinde [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) site.

### <a name="supporting-the-html5-date-input-control"></a>HTML5 tarih giriş denetiminin destekleme

Daha fazla tarayıcılar HTML5 destek gibi giriş, gibi yerel HTML5 kullanmak isteyeceksiniz `date` giriş öğesi ve jQuery UI Takvim kullanmayın. Tarayıcı bunları destekliyorsa, HTML5 denetimleri otomatik olarak kullanmak için uygulamanıza mantığı ekleyebilirsiniz. Bunu yapmak için içeriğini değiştirmek *DatePickerReady.js* aşağıdaki dosyasıyla:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

Bu komut dosyasının ilk satırı Modernizr HTML5 tarih giriş desteklendiğini doğrulamak için kullanır. Desteklenmiyorsa, jQuery UI tarih seçici yerine kancalanmış. ([Modernizr](http://www.modernizr.com/docs/) HTML5 ve CSS3, yerel uygulamalarını kullanılabilirliğini algılar bir açık kaynak JavaScript kitaplığı. Modernizr içinde oluşturduğunuz tüm yeni bir ASP.NET MVC projeleri dahil edilir.)

Bu değişikliği yaptıktan sonra Opera 11 gibi HTML5 destekleyen bir tarayıcı kullanarak test edebilirsiniz. HTML5 ile uyumlu bir tarayıcı kullanarak uygulamayı çalıştırın ve bir filmi girişi düzenleyin. HTML5 tarih denetimi jQuery UI popup calendar yerine kullanılır:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Tarayıcıların yeni sürümleri artımlı olarak HTML5 uygulama olduğundan, şimdi için iyi bir yaklaşım, çok çeşitli HTML5 destek düzenler Web sitenize kod eklemektir. Örneğin, bir daha sağlam *DatePickerReady.js* betik HTML5 tarih denetimi yalnızca kısmen destekleyen site destek tarayıcılar olanak tanıyan gösterilmiştir.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Bu komut dosyası HTML5 seçer `input` türündeki öğeler `date` yok tam olarak destekleyen HTML5 tarih denetimi. Bu öğeler için jQuery UI popup calendar kanca oluşturur ve ardından değişiklikleri `type` özniteliğini `date` için `text`. Değiştirerek `type` özniteliğini `date` için `text`, kısmi HTML5 tarih desteği ortadan kalkar. Bir daha sağlam *DatePickerReady.js* betik bulunabilir [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Boş değer atanabilir tarihleri şablonlarını ekleme

Varolan tarih şablonlarından birini kullanırsanız ve bir null tarih geçirmek, bir çalışma zamanı hata iletisi alırsınız. Tarih şablonlarını daha sağlam hale getirmek için bunları null değerleri işlemek için değiştireceksiniz. Boş değer atanabilir tarihleri desteklemek için kodda değişiklik *Views\Shared\DisplayTemplates\DateTime.cshtml* şu:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Model olduğunda kodu boş bir dize döndürür. **null**.

Kodda değişiklik *Views\Shared\EditorTemplates\Date.cshtml* aşağıdaki dosyasına:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Bu kod çalıştığında, model null değilse modelinin `DateTime` değeri kullanılır. Geçerli tarih, model null ise, bunun yerine kullanılır.

### <a name="wrapup"></a>Wrapup

Bu öğretici ASP.NET şablonlu Yardımcılar temelleri ele ve jQuery UI datepicker popup calendar bir ASP.NET MVC uygulamasındaki kullanmayı gösterir. Daha fazla bilgi için bu kaynakları deneyin:

- Yerelleştirme hakkında daha fazla bilgi için bkz: Rajeesh'ın blog [JQueryUI Datepicker ASP.NET mvc'de](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- JQuery UI hakkında daha fazla bilgi için bkz: [jQuery UI](http://docs.jquery.com/UI).
- Datepicker denetimini yerelleştirme hakkında daha fazla bilgi için bkz: [Datepicker/UI/yerelleştirme](http://docs.jquery.com/UI/Datepicker/Localization).
- ASP.NET MVC şablonları hakkında daha fazla bilgi için Brad Wilson'ın blog dizisini bakın [ASP.NET MVC 2 şablonları](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Seri için ASP.NET MVC 2 yazılmış olsa, malzeme hala ASP.NET MVC geçerli sürümü için geçerlidir.

>[!div class="step-by-step"]
[Önceki](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
