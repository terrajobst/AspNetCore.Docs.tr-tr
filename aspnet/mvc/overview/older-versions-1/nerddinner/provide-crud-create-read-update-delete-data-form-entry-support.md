---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: CRUD sağlayın (oluşturma, okuma, güncelleştirme, silme) veri Form girişi destek | Microsoft Docs
author: microsoft
description: 5. adım, düzenleme, oluşturma ve azalma ile de silmek için etkinleştirme desteği tarafından daha fazla bizim DinnersController sınıfı yapılacak gösterilmiştir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876754"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>CRUD sağlayın (oluşturma, okuma, güncelleştirme, silme) veri Form girişi desteği
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 5. adımını ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> 5. adım, düzenleme, oluşturma ve azalma ile de silmek için etkinleştirme desteği tarafından daha fazla bizim DinnersController sınıfı yapılacak gösterilmiştir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner adım 5: Oluştur, Güncelleştir, Form senaryolarını silin

Artık denetleyicileri ve görünümler sunulan ve bunların azalma dökümü/ayrıntıları deneyimindeki sitesinde uygulamak için nasıl kullanılacağını kapsamdaki. Bizim DinnersController sınıfı başka yapmanıza ve düzenleme, oluşturma ve azalma ile de silmeden desteğini etkinleştirmek için sonraki adım olacaktır.

### <a name="urls-handled-by-dinnerscontroller"></a>DinnersController tarafından işlenen URL'leri

Daha önce eylem yöntemleri için iki URL'ler için destek uygulanmadı DinnersController eklediğimiz: */Dinners* ve */Dinners/Ayrıntılar / [kimlik]*.

| **URL** | **FİİL** | **Amaç** |
| --- | --- | --- |
| */Dinners/* | AL | Yaklaşan azalma HTML listesini görüntüler. |
| */Dinners/Ayrıntılar / [kimlik]* | AL | Belirli bir Yemeği hakkındaki ayrıntıları görüntüler. |

Üç ek URL'ler uygulamak için eylem yöntemleri şimdi ekleyeceğiz: <em>/Dinners/düzenleme / [kimlik], / azalma/oluşturun,</em>ve<em>/Dinners/silme / [kimlik]</em>. Bu URL'leri yeni azalma oluşturma ve azalma silme düzenleme varolan azalma desteğini etkinleştirir.

Bu yeni URL'ler ile HTTP GET ve HTTP POST fiil etkileşimleri destekliyoruz. Bu URL'leri HTTP GET isteklerine ("Düzenle" durumunda Yemeği verilerle doldurulur form, "oluşturma" durumunda boş bir form ve bir delete onay ekranı "Sil" söz konusu olduğunda) verilerinin ilk HTML görünümü görüntüler. Bu URL'leri HTTP POST isteklerini kaydetme/güncelleştirme/silme Yemeği verileri bizim DinnerRepository (ve veritabanına buradan) olur.

| **URL** | **FİİL** | **Amaç** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | AL | Yemeği verilerle doldurmuş düzenlenebilir bir HTML formuna görüntüler. |
| YAYINLA | Belirli bir Yemeği veritabanı için form değişiklikleri kaydedin. |
| */Dinners/Create* | AL | Yeni azalma tanımlamak kullanıcılara boş bir HTML formuna görüntüler. |
| YAYINLA | Yeni Yemeği oluşturma ve veritabanındaki kaydedin. |
| */Dinners/silme / [kimlik]* | AL | Görüntü onay ekranı silin. |
| YAYINLA | Belirtilen Yemeği veritabanından siler. |

### <a name="edit-support"></a>Destek Düzenle

"Düzenle" senaryo uygulayarak başlayalım.

#### <a name="the-http-get-edit-action-method"></a>HTTP GET düzenleme eylem yöntemi

HTTP "GET" davranışını bizim düzenleme eylem yöntemi uygulayarak başlayacağız. Bu yöntem olacaktır olduğunda çağrılan */Dinners/düzenleme / [kimlik]* URL istendi. Bizim uygulaması gibi görünür:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Yukarıdaki kod DinnerRepository Yemeği nesnesini almak için kullanır. Ardından, Yemeği nesnesi kullanılarak bir görünüm şablonu oluşturur. Biz açıkça bir şablon adı geçirilen henüz çünkü *View()* yardımcı yöntemi, onu kullanır dayalı varsayılan yol şablonu görüntüleme gidermek için: /Views/Dinners/Edit.aspx.

Şimdi bu görünüm şablonu oluşturalım. Bu düzen yöntemi içinde sağ tıklayıp "Görünümü Ekle" bağlam menü komutu seçme gerçekleştiririz:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

"Görünümü Ekle" iletişim biz biz Yemeği nesne bizim görünüm şablonuna kendi modelini olarak geçirme ve otomatik iskele için bir "Düzenle" şablonunu seçin belirtmek:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir "Edit.aspx" görünümü şablon dosyası bize "\Views\Dinners" dizininde ekler. Ayrıca, kod-ilk "Düzenle" iskele uygulaması gibi doldurulan Düzenleyicisi içinde– yeni "Edit.aspx" Görünüm şablonu açar:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Şimdi oluşturulan varsayılan "Düzenle" iskele için birkaç değişiklik ve (kullanıma sunmak için istemediğiniz özellikleri bazılarını kaldıran) aşağıdaki içeriği sağlamak için düzenleme görünümü şablonu güncelleştirin:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Biz çalıştırdığınızda uygulama ve istek *"/ düzenleme/azalma/1"* URL biz aşağıdaki sayfayı görürsünüz:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Bizim görünüm tarafından oluşturulan HTML biçimlendirmesi aşağıdaki gibi görünüyor. Standart HTML – olduğu bir &lt;form&gt; bir HTTP POST yapar öğesi */Dinners/Edit/1* URL zaman "Kaydet" &lt;giriş türü "gönderme" = /&gt; düğmesi gönderilir. Bir HTML &lt;giriş türü = "text" /&gt; öğesi düzenlenebilir her özellik için çıktı açıldı:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() ve Html.TextBox() Html yardımcı yöntemler

Bizim "Edit.aspx" Görünüm şablonu birkaç "Html Yardımcısı" yöntem kullanıyor: Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() ve Html.ValidationMessage(). Yerleşik hata işleme ve doğrulama için bize oluşturma HTML biçimlendirmesini yanı sıra bu yardımcı yöntemler sağlar destekler.

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() yardımcı yöntemi

Ne HTML çıktı Html.BeginForm() yardımcı yöntemi olan &lt;form&gt; bizim biçimlendirmede öğesi. Bizim Edit.aspx görünüm şablonunda bir "deyimi bu yöntemi kullanırken kullanarak C" # uyguladığımız olduğunu fark edeceksiniz. Açık kaşlı ayraç başlangıcını gösterir &lt;form&gt; içerik ve kapanış kuşak olduğu ne sonunu gösterir &lt;/form&gt; öğe:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Alternatif olarak, "kullanarak" deyimi bulursanız, böyle bir senaryo için doğal olmayan yaklaşımını, (hangi aynı şeyi yapar) Html.BeginForm() ve Html.EndForm() bir bileşimini kullanabilirsiniz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Hiçbir parametre olmadan Html.BeginForm() çağırma geçerli isteğin URL'sine bir HTTP POST yapar bir form öğesi çıktısını neden olur. Diğer bir deyişle neden bizim düzenleme görünümü oluşturan bir *&lt;form eylemi = "/ düzenleme/azalma/1" yöntemi "post" =&gt;* öğesi. Farklı bir URL'ye gönderme istediyseniz biz alternatif olarak açık parametreleri Html.BeginForm() için geçmiş.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() yardımcı yöntemi

Bizim Edit.aspx görünümü Html.TextBox() yardımcı yöntem çıktısını almak için kullanır. &lt;giriş türü = "text" /&gt; öğeleri:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Her iki kimliği/ad özniteliklerini belirtmek için kullanılan tek bir parametre – yukarıdaki Html.TextBox() yöntemi alır &lt;giriş türü = "text" /&gt; metin değerinden doldurmak için model özelliğinin yanı sıra, çıktı öğesi. Örneğin, "Title" özelliğinin değeri ".NET vadeli" biz geçirilen düzenleme görünümü Yemeği nesne sahipse ve bu nedenle bizim Html.TextBox("Title") yöntemini çağırın çıktı: *&lt;girdi kimliği = "Title" name = "Title" type = "metin" value = ".NET vadeli" = /&gt;*.

Alternatif olarak, biz öğesinin kimliği/adı belirtin ve ikinci parametre olarak kullanılacak değer içinde açıkça geçirmek için ilk Html.TextBox() parametresini kullanabilirsiniz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Genellikle biz özel çıkış değerine biçimlendirme gerçekleştirmek istersiniz. Yerleşik-String.Format() statik yöntemi .NET bu senaryolar için kullanışlıdır. Böylece zamanı saniye göstermiyor bizim Edit.aspx şablonu görüntüleme (DateTime türünde olan) EventDate değeri biçimlendirmek için bu kullanıyor:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Html.TextBox() üçüncü parametresi, isteğe bağlı olarak, ek HTML özniteliklerini çıktısını almak için de kullanılabilir. Aşağıdaki kod parçacığında, ek bir boyut işlemek gösterilmiştir = "30" özniteliğini ve bir sınıf üzerinde = "mycssclass" özniteliği &lt;giriş türü = "text" /&gt; öğesi. Biz sınıfı özniteliği kullanılarak adı nasıl kaçış Not bir "@" character because "sınıfı" ayrılmış bir anahtar C#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>HTTP POST düzenleme eylem yöntemi uygulama

Biz şimdi uygulanan bizim Edit eylem yöntemini HTTP GET sürümüne sahip. Bir kullanıcı isteğinde bulunduğunda */Dinners/Edit/1* bir HTML sayfasında aşağıdaki gibi aldıkları URL'si:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Form post neden "Kaydet" düğmesine basarak */Dinners/Edit/1* URL, HTML sorgulayan ve &lt;giriş&gt; form değerleri HTTP POST edimi kullanılarak. Şimdi Yemeği kaydetme işleyecek bizim düzenleme eylem yöntemi – HTTP POST davranışını şimdi uygulayın.

Aşırı yüklenmiş bir "Düzenle" eylem yöntemini HTTP POST senaryoları işleme gösteren bir "AcceptVerbs" özniteliği olan bizim DinnersController için ekleyerek başlamak:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

[AcceptVerbs] özniteliği için aşırı yüklenmiş eylem yöntemleri uygulandığında, ASP.NET MVC gelen HTTP fiiline bağlı olarak uygun bir eylem yönteminin dağıtırken istekleri otomatik olarak yönetir. HTTP POST isteklerini <em>/Dinners/düzenleme / [kimlik]</em> URL'leri yukarıdaki düzenleme yöntemine diğer tüm HTTP fiili isteklerine sırasında yazılacak <em>/Dinners/düzenleme / [kimlik]</em>URL'leri yazılacak ilk düzenleme için biz (hangi vermedi uygulanan yöntemi [AcceptVerbs] özniteliğine sahip değil).

| **Yan konu: Neden HTTP fiilleri ayırt?** |
| --- |
| İsteyebilir – tek bir URL'yi kullanarak ve davranışını HTTP fiili aracılığıyla ayrım neden gerçekleştiriyoruz? Neden yükleme ve düzenleme değişiklikleri kaydetme işlemek için iki ayrı URL'ler yalnızca var? Örneğin: /Dinners/düzenleme için ilk form ve /Dinners/Kaydet kaydetmeyi form post işlemek için / [kimlik] / [kimlik]? Burada biz /Dinners/Save/2 için post ve Giriş bir hata nedeniyle HTML formu yeniden görüntüleyin gereken durumlarda, son kullanıcı (, başlatıldığından beri azalma/kaydetme/2 URL, tarayıcının adres çubuğunda sahip sona ereceği yayımlama iki ayrı URL'ler ile dezavantajı olduğu URL postalama form). Son kullanıcı kendi tarayıcı sık kullanılanları listesini redisplayed bu sayfaya yer işaret veya kopya/URL yapıştırır ve arkadaşınıza e-postalar, bunlar bu yana (Bu URL'yi post değerlerine bağlıdır), gelecekte çalışmaz bir URL kaydetme sona erer. Tek bir URL gösterme tarafından (gibi: /Dinners/Edit/[id]) ve HTTP fiili bunu işlemesi ayrım yapma, bu düzenleme sayfasını yer işareti ve/veya URL başkalarına göndermek son kullanıcılar için güvenli. |

#### <a name="retrieving-form-post-values"></a>Form Post değerlerini alma

Çeşitli şekillerde erişmek için form parametrelerini "Düzenle" bizim HTTP POST yöntemi içinde gönderilen vardır. Basit bir yaklaşım yalnızca istek özelliği denetleyici temel sınıfını form koleksiyonu erişmek ve gönderilen değerler doğrudan almak için kullanmaktır:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Hata işleme mantığı özellikle eklediğimiz sonra yukarıdaki yine de biraz ayrıntılı bir yaklaşımdır.

Bir daha iyi yaklaşımını bu senaryo için yerleşik kullanmak için *UpdateModel()* denetleyicisi temel sınıf yardımcı yöntemi. Bu, biz gelen form parametrelerini kullanarak geçirmek bir nesnenin özelliklerini güncelleştirme destekler. Yansıma nesnesindeki özellik adları belirlemek için kullanır ve ardından otomatik olarak dönüştürür ve bunları istemci tarafından gönderilen giriş değerleri temel alarak değerleri atar.

Bizim HTTP POST Düzenle bu kodu kullanarak eylem basitleştirmek için biz UpdateModel() yöntemi kullanabilirsiniz:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Biz şimdi ziyaret edebilirsiniz */Dinners/Edit/1* URL ve değişiklik bizim Yemeği Başlık:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Biz "Kaydet" düğmesini tıklatın, bir form post bizim düzenleme eylemi gerçekleştirirsiniz ve güncelleştirilmiş değerleri veritabanında kalıcı. Biz sonra ayrıntıları URL'si (hangi yeni kaydedilen değerler görüntülenir) yemeği için yönlendirilir:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>İşleme düzenleme hataları

Dışında geçerli HTTP POST uygulama works – ince hataları vardır.

Bir kullanıcı bir form düzenleme hata yaptığında, form, bunları düzeltmek için rehberlik edecek bir bilgilendirici hata iletisi ile yeniden görüntülenir emin olmanız gerekir. Bu durumda olduğu bir son kullanıcı yazılarını hatalı giriş içerir (örneğin: hatalı biçimlendirilmiş tarih dizesi), giriş biçimi where durumlarda da geçerlidir, ancak bir iş kuralı ihlali gibi. Ne zaman formun yaptıkları değişiklikleri el ile yenileme gerekmez böylece başlangıçta girilen kullanıcı giriş verilerini korumak hataları oluşur. Form başarıyla tamamlanana kadar bu işlemi gerektiği kadar yineleyin.

ASP.NET MVC hata işleme ve formu yeniden kolaylaştırmak iyi bazı yerleşik özellikler içerir. Bu özellikler eylem görelim için bizim düzenleme eylem yöntemine aşağıdaki kod ile güncelleştirin:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Yukarıdaki kod, biz şimdi try/catch hata işleme bloğu bizim geçici kaydırma dışında bizim önceki uygulamasına – benzerdir. Bir özel durum oluşursa UpdateModel() çağrılırken veya deneyin ve (kaydetmeye çalıştığınız Yemeği nesne modelimizi içinde bir kuralı ihlali nedeniyle geçersiz ise, bir özel durum oluşturacak) DinnerRepository Kaydet bizim catch hata işleme bloğu olur yürütün. İçindeki Yemeği nesnesinde mevcut herhangi bir kural ihlal üzerinden döngü ve bunları (biz kısaca ele alacağız) bir ModelState nesnesi için ekleyin. Biz ardından görünümü yeniden görüntüleyin.

Şimdi uygulamayı yeniden çalıştırın çalışma bu görmek için bir Yemeği düzenlemek ve bir EventDate "BOGUS", boş bir başlık varsa ve ABD ülke değeri olan bir BK telefon numarası kullanmak şekilde değiştirin. Biz "Kaydet" düğmesine bastığınızda bizim HTTP POST Edit yöntemi (hatalar olduğundan) Yemeği kaydetmek mümkün olmaz ve formu yeniden görüntüleyin:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Uygulamamız bir makul hata deneyimi olur. Geçersiz giriş metin öğeleriyle kırmızı ile vurgulanan ve bunlarla ilgili son kullanıcının bir doğrulama hata iletisi görüntülenir. Böylece her şeyi Dolum gerekmez formun Ayrıca kullanıcı ilk olarak girilen – giriş verilerini saklayan.

Nasıl sorabilirsiniz. Bu durum ortaya? Nasıl başlık, EventDate ve ContactPhone kutularındaki kendilerini kırmızıyla vurgulayın ve başlangıçta girilen kullanıcı değerlerinin çıktısını almak için bilmeniz? Ve nasıl hata iletileri listenin en üstünde görüntülenir? İyi haber bu magic ile ortaya alamadık - yerine Giriş doğrulama ve hata işleme senaryolarını kolaylaştırmak yerleşik ASP.NET MVC özelliklerinden bazıları kullandık nedeni, olur.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Anlama ModelState ve doğrulama HTML yardımcı yöntemler

Denetleyici sınıflarının hataları bir görünüme geçirilen bir model nesnesi ile mevcut olduğunu belirtmek için bir yol sağlayan bir "ModelState" özellik koleksiyonu vardır. Hata girişleri ModelState koleksiyonundaki sorun model özelliğinin adı belirleyin (örneğin: "Title", "EventDate" veya "ContactPhone") ve bir insan kolay hata iletisi belirtilmesine izin (örneğin: "Başlık gerekli").

*UpdateModel()* yardımcı yöntemi, model nesne üzerindeki özellikleri form değerleri atamak çalışırken hatalarla karşılaştığında ModelState koleksiyonu otomatik olarak doldurur. Örneğin, bizim Yemeği nesnenin EventDate özellik DateTime türünde. UpdateModel() yöntemi "BOGUS" dize değeri için yukarıdaki senaryoda atayamadı UpdateModel() yöntemi bir giriş olduğunu belirten bir atama hata ModelState koleksiyonuna bu özellik ile oluştu eklenmesi.

Geliştiriciler Ayrıca hangi ModelState koleksiyonu içinde active kural ihlallerinin göre girişlerle doldurma bizim "catch" hata işleme bloğu içinde biz aşağıda yaptıklarını gibi açık hata girişleri ModelState koleksiyona eklemek için kod yazma Yemeği nesnesi:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>ModelState ile HTML Yardımcısı tümleştirme

HTML yardımcı yöntemlerinden - Html.TextBox() gibi-çıkış işleme biçiminde ModelState koleksiyon kontrol edin. Öğesi için bir hata varsa, kullanıcı tarafından girilen değer ve bir CSS hata sınıfı işleyebilir.

Örneğin, "Düzenle" bizim görünümünde Html.TextBox() yardımcı yöntem bizim Yemeği nesnesinin EventDate işlemek için kullanıyoruz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Görünüm hata senaryosunda işlendiğinde Html.TextBox() yöntemi bizim Yemeği nesnesinin "EventDate" özelliği ile ilişkili herhangi bir hata olup olmadığını görmek için ModelState koleksiyonu denetimi. Bir hata oluştu belirlenen zaman değeri olarak gönderilen kullanıcı girişini ("BOGUS") oluşturulmasını ve bir css hata sınıfa eklenen &lt;giriş türü "metin" = /&gt; biçimlendirmesi ürettiğini:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Ancak, istediğiniz aramak için css hata sınıfı görünümünü özelleştirebilirsiniz. Varsayılan CSS hata sınıfı – "giriş-doğrulama hata" – tanımlanan *\content\site.css* stil ve görünümler gibi altında:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

CSS kuralın hangi bizim geçersiz giriş öğeleri gibi aşağıda vurgulanmasını kaynaklanır:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() yardımcı yöntemi

Html.ValidationMessage() yardımcı yöntemi, belirli model özelliği ile ilişkili ModelState hata iletisi çıktısını almak için kullanılabilir:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Yukarıdaki kod çıkarır:  *&lt;span class = "alan doğrulama hata"&gt; 'BOGUS' değeri geçersiz &lt; /span&gt;*

Html.ValidationMessage() yardımcı yöntemi, geliştiricilerin görüntülenen hata SMS mesajı geçersiz kılmasına izin veren ikinci bir parametresi de destekler:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Yukarıdaki kod çıkarır:  <em>&lt;span class = "alan doğrulama hata"&gt;\*&lt;/span&gt;</em>yerine bir hata için mevcut olduğunda varsayılan hata metni EventDate özelliği.

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() yardımcı yöntemi

Html.ValidationSummary() yardımcı yöntemi tarafından eşlik bir Özet hata iletisi işlemek için kullanılan bir &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; listesi tüm ayrıntılı hata iletileri ModelState koleksiyonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Ayrıntılı hataları listesini görüntülemek için bir Özet hata iletisi tanımlar ve isteğe bağlı dize parametresi – Html.ValidationSummary() yardımcı yöntemi alır:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Hata listesi benzer geçersiz kılmak için CSS isteğe bağlı olarak kullanabilirsiniz.

#### <a name="using-a-addruleviolations-helper-method"></a>AddRuleViolations yardımcı yöntemini kullanma

Bizim ilk HTTP POST Düzenle uygulama catch bloğu içinde bir foreach Yemeği nesnenin kural ihlallerinin döngü ve bunları denetleyicinin ModelState koleksiyona eklemek için kullanılır:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Biz "ControllerHelpers" ekleyerek küçük bir temizleyici NerdDinner projeye sınıf ve ASP.NET MVC ModelStateDictionary sınıfına yardımcı yöntemini ekleyen bir "AddRuleViolations" genişletme yöntemi içindeki uygulama bu kodu yapabilirsiniz. Bu uzantı metodu ModelStateDictionary RuleViolation hataların listesini ile doldurmak için gerekli mantığı sarmalayabilen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Biz bu genişletme yöntemi bizim Yemeği kural ihlallerinin ModelState koleksiyonuyla doldurmak için kullanılacak bizim HTTP POST Düzenle eylem yöntemi sonra güncelleştirebilirsiniz.

#### <a name="complete-edit-action-method-implementations"></a>Düzen eylem yöntemi uygulamaları tamamlayın

Aşağıdaki kodu tüm düzenleme senaryomuz için gerekli denetleyicisi mantığı uygular:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Bizim düzenleme uygulaması hakkında iyi tarafı bizim denetleyici sınıfı ne bizim şablonu görüntüleme herhangi bir şey belirli doğrulama veya bizim Yemeği modeli tarafından zorlanan iş kuralları hakkında bilmeniz sahip olur. Biz ek kurallar bizim modeline gelecekte ekleyebilirsiniz ve bizim denetleyicisi ya da sunumların desteklenmesi görünümünde kod değişikliklerini yapmak zorunda değilsiniz. Bu bize kolayca bizim uygulama gereksinimlerini gelecekte en az ile kod değişikliklerini gelişmesi esnekliği sağlar.

### <a name="create-support"></a>Destek oluşturma

Bizim DinnersController sınıfı "Düzenle" davranışını uygulama bitirdikten sonra. Şimdi şimdi "Oluştur" Destek üzerinde – kullanıcıların yeni azalma eklemesine olanak sağlayan uygulamak geçin.

#### <a name="the-http-get-create-action-method"></a>HTTP GET oluşturma eylem yöntemi

Biz "GET" HTTP davranışını uygulayarak başlarsınız bizim eylem yöntemi oluşturun. Birisi ziyaret ettiğinde bu yöntem çağrılır */azalma/oluşturma* URL. Bizim uygulama şuna benzer:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Yukarıdaki kod yeni bir Yemeği nesnesi oluşturur ve onun EventDate özelliğini gelecekte bir hafta atar. Ardından, yeni Yemeği nesnesini temel alan bir görünümü işler. Biz bir adına açıkça geçirilen henüz çünkü *View()* yardımcı yöntemi, onu kullanır dayalı varsayılan yol şablonu görüntüleme gidermek için: /Views/Dinners/Create.aspx.

Şimdi bu görünüm şablonu oluşturalım. Oluşturma eylem yöntemi içinde sağ tıklayıp "Görünümü Ekle" context menü komutunu seçerek bunu yapabilirsiniz. "Görünümü Ekle" iletişim biz biz Yemeği nesne şablonu görüntüleme geçtiğiniz ve otomatik-iskele "Oluştur" şablonunu seçin belirtmek:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir katman kalıbı iskeleti tabanlı "Create.aspx" Görünüm "\Views\Dinners" dizinine kaydedin ve IDE içinde açın:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Şimdi oluşturulan varsayılan "Oluştur" iskele dosyasına bize birkaç değişiklikleri yapın ve aşağıdaki gibi görünecek şekilde yukarı değiştirin:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Ve şimdi biz çalıştırdığınızda, uygulama ve erişim *"/ azalma/oluştur"* onu sokacak aşağıdaki gibi UI bizim Oluştur eylemi uygulamasından tarayıcısından URL'si:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>HTTP POST uygulama oluşturma eylemini yöntemi

Biz uygulanan bizim oluşturma eylem yöntemini HTTP GET sürümüne sahip. Bir kullanıcı "Kaydet" düğmesine tıkladığında, form post gerçekleştirir */azalma/oluşturma* URL, HTML sorgulayan ve &lt;giriş&gt; form değerleri HTTP POST edimi kullanılarak.

Şimdi HTTP POST davranışını şimdi uygulamak bizim eylem yöntemi oluşturun. HTTP POST senaryoları işleme gösteren bir "AcceptVerbs" özniteliği olan bizim DinnersController için aşırı yüklenmiş "Oluştur" eylem yöntemine ekleyerek başlamak:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Çeşitli şekillerde gönderilen formu parametreleri "Oluştur" bizim etkin HTTP POST yöntemi içinde erişebilirsiniz vardır.

Bir yaklaşım ise yeni bir Yemeği nesnesi oluşturun ve ardından *UpdateModel()* (Düzenle eylemiyle yaptığımız gibi) yardımcı yöntem gönderilen formu değerlerle doldurmak için. Biz sonra bizim DinnerRepository eklemek, veritabanına kalıcı ve aşağıdaki kodu kullanarak yeni oluşturulan Yemeği göstermek için bizim Ayrıntılar eylemi kullanıcıyı yönlendirir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternatif olarak, size bir yaklaşım Yemeği nesnesi bir yöntem parametresi olarak ele bizim Create() eylem yöntemine sahip olduğumuz kullanabilirsiniz. ASP.NET MVC sonra otomatik olarak bize için yeni bir Yemeği nesne örneği, form girişleri kullanarak özelliklerini doldurmak ve bizim eylem yöntemine geçirin:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Yukarıdaki bizim eylem yöntemi Yemeği nesne başarıyla form post değerlerle ModelState.IsValid özelliğini kontrol ederek doldurulmuş olduğunu doğrular. Bu giriş dönüştürme sorunları varsa false döndürür (örneğin: "BOGUS" dizesi EventDate özelliği için), ve herhangi bir sorun varsa, eylem yöntemi form görüntüler.

Giriş değeri geçerliyse, eylem yöntemine ekleyin ve yeni Yemeği DinnerRepository kaydetmek çalışır. Try/catch bloğu içinde bu iş sarmalar ve (hangi bir özel durum yükseltmek dinnerRepository.Save() yöntemi neden olur) herhangi bir iş kuralı ihlal varsa biçiminde görüntüler.

Eylem işleme bu hatayı görmek için biz isteyebilir */azalma/oluşturma* URL ve yeni Yemeği ayrıntılarını doldururken. Yanlış giriş veya değerleri aşağıdaki gibi vurgulanmış hatalarla görünürler form oluştur neden olur:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Create formumuzun tam aynı doğrulama hem de iş kurallarını Düzenle formumuzun olarak nasıl uygularken dikkat edin. Bizim doğrulama hem de iş kurallarını modelde tanımlanan ve kullanıcı Arabirimi veya uygulamanın denetleyici içinde ekli değil olmasıdır. Bu size daha sonra değişiklik/bizim doğrulama gelişmesi veya tek bir iş kuralları yerleştirin ve bunları uygulama genelinde geçerli anlamına gelir. Biz bizim düzenleme ya da içinde herhangi bir kod değiştirebilir veya herhangi bir yeni kurallar veya var olanları değişiklikler otomatik olarak kabul etmeniz eylem yöntemleri oluşturabilirsiniz gerekmez.

Giriş değerleri düzeltin ve "Kaydet" düğmesini tıklatın, yeniden DinnerRepository bizim ekleme başarılı olur ve yeni Yemeği veritabanına eklenir. Biz sonra yönlendirilecek */Dinners/Ayrıntılar / [kimlik]* burada biz sunulabilir yeni oluşturulan Yemeği hakkında ayrıntılarla URL –:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Sil desteği

Şimdi "Sil" Destek bizim DinnersController ekleyelim.

#### <a name="the-http-get-delete-action-method"></a>HTTP GET silme eylem yöntemi

Biz bizim delete eylem yöntemini HTTP GET davranışını uygulayarak başlarsınız. Birisi ziyaret ettiğinde bu yöntem çağrılmadığı */Dinners/silme / [kimlik]* URL. Uygulama aşağıdadır:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Eylem yönteminin silinecek Yemeği almaya çalışır. Yemeği işler varsa bir görünüm Yemeği nesnesine bağlı. Nesne yok (veya zaten silinmiş varsa), "Bulunamadı" işleyen görünüm döndürür bizim "Ayrıntılar" eylem yöntemi için daha önce oluşturduğumuz şablon görüntüleyin.

"Sil" görünümü şablon silme eylem yöntemi içinde sağ tıklayıp "Görünümü Ekle" context menü komutunu seçerek oluşturabilirsiniz. "Görünümü Ekle" iletişim biz biz Yemeği nesne bizim görünüm şablonuna kendi modelini olarak geçirme ve boş bir şablon oluşturmak için seçtiğiniz belirtmek:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir "Delete.aspx" görünümü şablon dosyası bize bizim "\Views\Dinners" dizininde ekler. Bazı HTML ve kod silme onayı ekranı uygulamak için şablon ekleyeceğiz aşağıdaki gibi:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Yukarıdaki kod başlığı silinecek Yemeği ve çıkışları görüntüler bir &lt;form&gt; son kullanıcı içindeki "Sil" düğmesini tıklarsa /Dinners/silme / [kimlik] URL bir POST yapan öğesi.

Biz çalıştırdığınızda, uygulama ve erişim *"/ azalma/silme / [kimlik]"* geçerli Yemeği URL'sini nesne UI gibi aşağıda işler:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Yan konu: Neden biz bir POST yaptıklarını?** |
| --- |
| İsteyebilir – neden biz Git oluşturma çaba bir &lt;form&gt; bizim silme onayı ekranındaki? Neden gerçek silme işlemi gerçekleştiren bir eylem yöntemine bağlamak için yalnızca standart bir köprü kullanabilir? Web gezginleri karşı koruma ve arama motorları Url'lerimizi bulmak ve yanlışlıkla veri bağlantıları izlediğinizde silinecek neden dikkat etmek istiyoruz çünkü nedenidir. HTTP GET göre URL'leri "erişim/gezinme için güvenli" değerlendirilir ve HTTP POST olanları izlemeyecek şekilde olması. Her zaman bozucu koymadan ya da veri değiştirme işlemleri HTTP POST isteklerini arkasında emin iyi bir kuraldır. |

#### <a name="implementing-the-http-post-delete-action-method"></a>HTTP POST silme eylem yöntemi uygulama

Biz şimdi bir delete onay ekranı görüntüler ın uygulanan bizim Delete eylem yöntemini HTTP GET sürümüne sahip. Bir son kullanıcı "Sil" düğmesini tıklattığında form post gerçekleştirecek */Dinners/Yemeği / [kimlik]* URL.

Şimdi artık HTTP "POST" davranışını aşağıdaki kodu kullanarak silme eylem yöntemini uygulayın:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Bizim Delete eylem yöntemini HTTP POST sürümü silinecek Yemeği nesne almaya çalışır. (Öğesi zaten silindiğinden onun için), bulamazsa, bizim "Bulunamadı" şablonu oluşturur. Yemeği bulursa, onu DinnerRepository siler. Ardından, bir "Silinmiş" şablonu oluşturur.

"Silinmiş" şablonu uygulamak için biz eylem yöntemine sağ tıklatın ve "Görünümü Ekle" bağlam menüsünü seçin. Biz bizim görünümü "Silinmiş" olarak adlandırın ve boş bir şablonu (ve kesin türü belirtilmiş model nesnesi geçmeyecek) sahip. Ardından bazı HTML içeriği için ekleyeceğiz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Ve şimdi biz çalıştırdığınızda, uygulama ve erişim *"/ azalma/silme / [kimlik]"* aşağıdaki gibi bizim Yemeği sokacak nesnesini silme onayı geçerli bir Yemeği URL'sini ekranında:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Biz "Sil" düğmesini tıklatın, bir HTTP POST işlemini gerçekleştirecek */Dinners/silme / [kimlik]* hangi Yemeği bizim veritabanından silmek ve bizim "Silinmiş" görünümü şablon görünen URL'si:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Model bağlama güvenliği

Biz, yerleşik ASP.NET MVC model bağlama özelliklerini kullanmak için iki farklı şekilde ele. Önce var olan bir model nesne üzerindeki özellikleri güncelleştirmek için UpdateModel() yöntemini kullanarak ve ikinci kullanarak ASP.NET MVC'ın desteği eylem yöntemi parametrelerine model nesneleri geçirme. Bu teknikler her ikisini birden çok güçlü ve son derece kullanışlıdır.

Bu güç de onunla getirir sorumluluğu. Bu ayrıca nesneler için form girişi bağlamayı true olduğunda ve herhangi bir kullanıcı giriş kabul ederken her zaman güvenlik hakkında paranoid olması önemlidir. İçin dikkatli olmalıdır her zaman HTML kodlama HTML ve JavaScript ekleme saldırıları önlemek ve SQL ekleme saldırıları dikkatli olun hiçbir kullanıcı tarafından girilen değerleri (Not: Bu önlemek için parametreleri otomatik olarak kodlar uygulamamız için LINQ-SQL kullanıyoruz tür saldırılar). Hiçbir zaman tek başına istemci tarafı doğrulama üzerinde kullanır ve her zaman bilgisayar korsanlarının sahte değerleri gönderilmeye çalışılırken karşı koruma sağlamak için sunucu tarafında doğrulama uygulamadığınız gerekir.

ASP.NET MVC bağlama özelliklerini kullanırken düşündüğünüzden emin olmak için bir ek güvenlik bağlama nesnelerinin kapsamını öğesidir. Özellikle, bağlı olabilir ve yalnızca gerçekten güncelleştirilmesi için bir son kullanıcı tarafından güncelleştirilebilir olması gereken özelliklere izin ver emin olmak için izin verme özellikler güvenlik etkilerini anladığınızdan emin olmak istersiniz.

Varsayılan olarak, UpdateModel() yöntemi gelen form parametre değerleriyle eşleşen tüm model nesnesinin özellikleri güncelleştirme dener. Benzer şekilde, eylem yöntemi parametrelerine da varsayılan olarak geçirilen nesneleri özellikleri form parametreleri aracılığıyla ayarlanan tümüne sahip olabilir.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Kullanım başına temelinde bağlama kilitleme

Kullanım başına temelinde bağlama ilkesi aşağı sağlayan bir açık "Ekle" güncelleştirilebilir özelliklerini tarafından listesi kilitleyebilirsiniz. Bu, aşağıdaki gibi UpdateModel() yöntemine bir ek dize dizi parametresini geçirerek yapılabilir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Eylem yöntemi parametrelerine da geçirilen nesneleri bir "dahil etme listesi" özelliklere gibi aşağıda belirtilmesine izin verir [Bind] özniteliği destekler:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bağlama türü temelinde kilitleme

Tür başına temelinde bağlama kuralları aşağı kilitleyebilirsiniz. Bu, bağlama kuralları kez belirtmek ve bunları tüm denetleyiciler ve eylem yöntemlerine (UpdateModel ve eylem yöntemi parametresini senaryoları dahil) tüm senaryolarda geçerli olması sağlar.

Tür başına bağlama kuralları [Bind] özniteliği bir tür üzerine ekleyerek veya (burada türü ait olmayan senaryolar için yararlıdır) uygulamasının Global.asax dosyasındaki kaydetme özelleştirebilirsiniz. Dışlama özelliklerini hangi özellikleri denetlemek için belirli bir sınıf veya arabirim için bağlanabilir ve ardından BIND özniteliğin INCLUDE kullanabilirsiniz.

Biz NerdDinner uygulamamız Yemeği sınıfı için bu tekniği kullanın ve bağlanabilirse özelliklerin listesi için aşağıdaki kısıtlayan ona [Bind] özniteliği ekleyin:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Biz bağlama aracılığıyla yönetilebilmesini LCV'ler koleksiyonu sağlanmıyor dikkat edin veya biz bağlama aracılığıyla ayarlanacak DinnerID veya HostedBy özellikler sağlamaktadır. Güvenlik nedenleriyle biz bunun yerine yalnızca bizim eylem yöntemleri içinde açık kod kullanarak bu belirli özelliklerini yönetme.

### <a name="crud-wrap-up"></a>CRUD Wrap-Up

ASP.NET MVC birkaç senaryo nakil form uygulama ile yardımcı olan yerleşik özellikler içerir. Bizim DinnerRepository üstünde CRUD UI desteği sağlamak için bu özellikleri çeşitli kullandık.

Uygulamamız uygulamak için model odaklı bir yaklaşım kullanıyoruz. Bu bizim doğrulama ve iş kuralı mantığı bizim modeli katmanı içinde– ve bizim denetleyicileri veya görünümler içinde değil tanımlanır anlamına gelir. Bizim denetleyici sınıfı ne görünüm şablonlarımız bizim Yemeği model sınıfı tarafından zorlanan belirli iş kuralları hakkında hiçbir şey öğrenin.

Bu uygulama mimarimizin temiz tutmak ve test kolaylaştırır. Ek iş kuralları bizim modeli katmanı gelecekte ekleyebiliriz ve *kod değişikliklerini yapmak zorunda değil* bizim denetleyicisi ya da sunumların görünümünde desteklenmesini sağlar. Bu bize gelişmesi ve uygulamamızı gelecekte değiştirmek için çevikliğinin büyük bir bölümünü ile sağlamak için geçiyor.

Bizim DinnersController şimdi Yemeği listeleri/ayrıntılar sağlar yanı sıra oluşturma, düzenleme ve Sil desteği. Sınıfı için tam kod aşağıda bulunabilir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Sonraki adım

Bizim DinnersController sınıf içinde uygulama temel CRUD (Oluştur, okuma, güncelleştirme ve silme) desteği artık sunuyoruz.

Şimdi nasıl biz ViewData ve ViewModel sınıfları daha zengin UI bizim formlarında etkinleştirmek için kullanabilirsiniz bakalım.

> [!div class="step-by-step"]
> [Önceki](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [sonraki](use-viewdata-and-implement-viewmodel-classes.md)
