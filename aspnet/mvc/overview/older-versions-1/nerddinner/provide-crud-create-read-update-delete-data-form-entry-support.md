---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Sağlamak CRUD (oluşturma, okuma, güncelleştirme ve silme) veri formu giriş desteği | Microsoft Docs
author: microsoft
description: 5. adım, düzenleme, oluşturma ve azalma ile de silme için etkinleştirme desteği tarafından daha fazla bizim DinnersController sınıfı nasıl gösterir.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bfb8446ec8b39ad6fc88a0d5b747f0cec33bbd25
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817642"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Sağlamak CRUD (oluşturma, okuma, güncelleştirme ve silme) veri formu giriş desteği
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 5 ücretsiz / budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 5. adım, düzenleme, oluşturma ve azalma ile de silme için etkinleştirme desteği tarafından daha fazla bizim DinnersController sınıfı nasıl gösterir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner adım 5: Oluşturma, güncelleştirme ve silme Form senaryoları

Biz denetleyicileri ve görünümleri kullanıma sunulmuştur ve bunları bir azalma listeleme/Ayrıntılar deneyimi sitesinde uygulamak için kullanma dahil. Sonraki adımımız, bizim DinnersController sınıfı ileriye ve düzenleme, oluşturma ve azalma ile de silme desteğini etkinleştirmek için olacaktır.

### <a name="urls-handled-by-dinnerscontroller"></a>DinnersController tarafından işlenen URL'leri

Daha önce eylem yöntemleri iki URL'ler için destek uygulanmadı DinnersController ekledik: */Dinners* ve */Dinners/Ayrıntılar / [ID]*.

| **URL** | **FİİLİ** | **Amaç** |
| --- | --- | --- |
| */Dinners/* | AL | Yaklaşan azalma bir HTML listesini görüntüler. |
| */Dinners/Ayrıntılar / [ID]* | AL | Belirli bir Akşam Yemeği hakkında ayrıntıları görüntüler. |

Üç ek URL'ler uygulamak için eylem yöntemleri artık ekleyeceğiz: <em>/Dinners/düzenleme / [id], / azalma/oluşturun,</em>ve<em>/Dinners/Delete / [ID]</em>. Bu URL'ler, yeni azalma oluşturma ve silme azalma düzenleme mevcut azalma için desteği etkinleştirir.

Bu yeni URL'leri HTTP GET ve HTTP POST edimi etkileşim destekleyeceğiz. Bu URL'ler için HTTP GET isteklerini ("Düzenle" durumunda Dinner verilerle doldurulmuş bir form, "oluşturma" söz konusu olduğunda boş bir form ve bir delete onay ekranında "Sil" söz konusu olduğunda) verilerin ilk HTML görünümünü görüntüler. Bu URL'ler için HTTP POST isteklerini kaydetme/güncelleştirme/silme Dinner veri bizim DinnerRepository (ve buradan veritabanı) olur.

| **URL** | **FİİLİ** | **Amaç** |
| --- | --- | --- |
| */Dinners/düzenleme / [ID]* | AL | Akşam Yemeği doldurulmuş düzenlenebilir bir HTML form görüntüler. |
| YAYINLA | Belirli bir Akşam Yemeği veritabanına için form değişiklikleri kaydedin. |
| */ Azalma/oluşturma* | AL | Kullanıcıların yeni azalma tanımlamasına olanak tanıyan boş bir HTML form görüntüler. |
| YAYINLA | Yeni bir Akşam Yemeği oluşturun ve veritabanında kaydedin. |
| */Dinners/delete / [ID]* | AL | Görünen onay ekranında silin. |
| YAYINLA | Belirtilen Akşam Yemeği veritabanından siler. |

### <a name="edit-support"></a>Destek Düzenle

"Düzenle" senaryo uygulayarak başlayalım.

#### <a name="the-http-get-edit-action-method"></a>HTTP GET düzenleme eylem yöntemi

Bizim düzenleme eylem yöntemini HTTP "GET" davranışı uygulayarak başlayacağız. Bu yöntem olacaktır ne zaman çağrılır */Dinners/düzenleme / [ID]* istenen URL. Kararlılığımızın şöyle görünecektir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Yukarıdaki kod DinnerRepository Dinner nesneyi almak için kullanır. Ardından, Şimdi Akşam nesnesini kullanarak bir şablonu görüntüleme işler. Biz için bir şablon adı açıkça geçirmediğinden henüz *View()* yardımcı yöntemi, tabanlı varsayılan yol şablonu görüntüle çözmek için kullanacağı: /Views/Dinners/Edit.aspx.

Artık bu görünüm şablonu oluşturalım. Bu düzen yöntemi içinde sağ tıklayıp "Görünüm Ekle" bağlam menüsü komutu tarafından yapacağız:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

"Görünüm Ekle" iletişim kutusunun içinden biz biz Dinner nesnesi, model olarak bizim görünümü şablona geçirme ve otomatik-iskele "Düzenle" bir şablon seçin belirtmek:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Biz "Ekle" düğmesine tıkladığınızda, Visual Studio yeni "Edit.aspx" görünümü şablon dosyası ABD için "\Views\Dinners" dizininde ekler. Ayrıca, kod-ilk "Düzenle" iskele uygulaması gibi doldurulmuş Düzenleyici içindeki – yeni "Edit.aspx" Görünüm şablonu açar:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Şimdi oluşturulan varsayılan "Düzenle" iskele için birkaç değişiklik yapmak ve güncelleştirme (birkaç kullanıma sunmak için istemediğiniz özellikleri kaldıran) aşağıdaki içeriği için şablonu Düzen görüntüle:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Biz çalıştırıldığında uygulama ve istek *"/ azalma/düzenleme/1"* URL'si şu sayfayı görürsünüz:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Bizim görünüm tarafından oluşturulan HTML biçimlendirmesi aşağıdaki gibi görünür. Standart HTML – sahip olduğu bir &lt;form&gt; HTTP POST işlemi gerçekleştiren öğesi */Dinners/Edit/1* URL, "Kaydet" &lt;giriş türü = "submit" /&gt; düğmesi gönderildi. Bir HTML &lt;giriş türü = "text" /&gt; öğesi her düzenlenebilir bir özellik için çıkış oluştu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() ve Html.TextBox() Html yardımcı yöntemler

Bizim "Edit.aspx" Görünüm şablonu birkaç "Html Yardımcısı" yöntem kullanıyor: Html.ValidationSummary(), Html.BeginForm() Html.TextBox() ve Html.ValidationMessage(). Yerleşik hata işleme ve doğrulama bizim için oluşturma HTML biçimlendirmeyi yanı sıra bu yardımcı yöntemler sağlayan destekler.

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() yardımcı yöntemi

Hangi HTML çıktı Html.BeginForm() yardımcı yöntem olduğunu &lt;form&gt; bizim biçimlendirme öğesi. Bizim Edit.aspx görünümü şablonunda size C# "deyimi için bu yöntemi kullanırken kullanma" uyguladığınız olduğunu fark edeceksiniz. Açık küme ayracı başlangıcını gösterir &lt;form&gt; içerik ve kapanış küme ayracını olduğu ne sonuna belirten &lt;/form&gt; öğesi:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Alternatif olarak, "kullanma" bildirimi görürseniz bunun gibi bir senaryoya yönelik istemeyiz yaklaşımını, (Bu aynı şeyi yapar) Html.BeginForm() ve Html.EndForm() bir birleşimini kullanabilirsiniz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Hiçbir parametre olmadan Html.BeginForm() çağırma geçerli isteğin URL'sine HTTP POST yapan bir form öğesi çıktı neden olur. Diğer bir deyişle neden bizim düzenleme görünümünü oluşturur bir *&lt;form eylem = "/ azalma/düzenleme/1" yöntemi "post" =&gt;* öğesi. Farklı bir URL'ye gönderim yapması istediyseniz biz bunun yerine açık parametrelerini Html.BeginForm() için başarılı.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() yardımcı yöntemi

Bizim Edit.aspx görünümü, çıkış Html.TextBox() yardımcı yöntemini kullanır. &lt;giriş türü = "text" /&gt; öğeleri:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Her iki kimliği/ad özniteliklerini belirtmek için kullanılan tek bir parametre – yukarıdaki Html.TextBox() yöntemi alır &lt;giriş türü = "text" /&gt; metin değerini doldurmak için model özelliğinin yanı sıra çıkış öğesi. Örneğin, ".NET vadeli" öğesinin "Title" özellik değeri biz geçirilen düzenleme görünümüne Dinner nesnesi vardı ve bu nedenle bizim Html.TextBox("Title") yöntemini çağırın çıkış: *&lt;giriş kimliği "Title" name = "Title" type = "text" value = ".NET vadeli" = /&gt;*.

Alternatif olarak, öğenin kimliği/adını belirtin ve ikinci parametre olarak kullanılacak değeri açıkça geçirebilirsiniz ilk Html.TextBox() parametre kullanabiliriz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Genellikle şu çıkış değeri üzerinde özel biçimlendirme gerçekleştirmek isteyebilirsiniz. Bu senaryolar için .NET yerleşiktir String.Format() statik yöntem kullanışlıdır. Böylece süreyi saniye göstermez bizim Edit.aspx şablonu görüntüle (DateTime türünde) EventDate değerini biçimlendirmek için bu kullanıyor:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Html.TextBox() üçüncü parametresi, isteğe bağlı olarak, diğer HTML öznitelikleri çıktısını almak için de kullanılabilir. Aşağıdaki kod parçacığında bir ek boyut nasıl oluşturulacağını gösterir. "30" özniteliğini ve bir sınıf = "mycssclass" özniteliği = on &lt;giriş türü = "text" /&gt; öğesi. Biz sınıfı kullanmanın öznitelik adı nasıl kaçış Not bir "@" character because "sınıfı" C# ' de ayrılmış bir anahtar sözcük:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>HTTP POST düzenleme eylem yöntemi uygulama

Artık uygulanan bizim düzenleme eylem yöntemini HTTP GET sürümü var. Bir kullanıcı istediğinde */Dinners/Edit/1* URL bir HTML sayfasına aşağıdaki gibi alın:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

"Kaydet" düğmesine basarak neden olan bir form post */Dinners/Edit/1* URL, HTML sorgulayan ve &lt;giriş&gt; form değerleri HTTP POST edimi kullanılarak. Şimdi Akşam Yemeği kaydetme işleyecek bizim düzenleme eylem yöntemine – HTTP POST davranışını hemen uygulayın.

HTTP POST senaryoları işleme gösteren bir "AcceptVerbs" özniteliği içeren bizim DinnersController için aşırı yüklenmiş bir "Düzenleme" eylem yöntemine ekleyerek başlayın:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

[AcceptVerbs] özniteliği için aşırı yüklenmiş bir eylem yöntemleri uygulandığında, ASP.NET MVC gelen HTTP fiiline bağlı olarak uygun bir eylem yönteminin dispatching isteklerine otomatik olarak işler. HTTP POST isteklerini <em>/Dinners/düzenleme / [ID]</em> URL'leri diğer tüm HTTP fiili istekleri sırasında yukarıdaki düzenleme yönteme gider <em>/Dinners/düzenleme / [ID]</em>URL'leri yazılacak ilk düzenleme yönteme (hangi yaptığınız uyguladık bir [AcceptVerbs] özniteliğine sahip).

| **Yan konu: Neden HTTP fiilleri ayırt?** |
| --- |
| İsteyebilir – tek bir URL kullanarak ve davranışını HTTP fiili üzerinden farklılaştırılması duyuyoruz neden? Neden yalnızca yükleme ve düzenleme değişiklikleri kaydetme işlemek için iki ayrı URL vardır? Örneğin: /Dinners/düzenleme ilk formu ve /Dinners/kaydetme kaydetmek için form post işlemek için / [ID] / [ID]? Burada /Dinners/Save/2 için gönderin ve HTML formundaki Giriş bir hata nedeniyle yeniden gereken durumlarda, son kullanıcı (bu yana azalma/kaydetme/2 URL, tarayıcının adres çubuğunda sonunda, yayımlama iki ayrı URL ile dezavantajı olduğu URL için gönderilen bir formu). Son kullanıcı kendi tarayıcı Sık Kullanılanlar listesi redisplayed bu sayfaya yer işaretlerini veya kopyalama/URL yapıştırır ve bir arkadaşınıza e-postaları (Bu URL'yi post değerlerine bağlı olduğundan) gelecekte çalışmaz bir URL kaydetme sonlandırır. Tek bir URL gösterme tarafından (gibi: /Dinners/Edit/[id]) ve işlenmesini HTTP fiili ile ayrım yapma, bunu düzenleme sayfası yer işareti ve/veya URL başkalarına göndermek üzere son kullanıcılar için güvenli. |

#### <a name="retrieving-form-post-values"></a>Form Gönderme değerlerini alma

Gönderilen form parametreleri bizim HTTP POST yöntemi "Düzenle" içinde erişebilmeniz için yol çeşitli vardır. Yalnızca bir form koleksiyonu erişmek ve gönderilen değerlerden doğrudan almak için denetleyici taban sınıfa istek özelliği kullanmak için bir basit yaklaşım şöyledir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Hata işleme mantığı özellikle eklediğimiz sonra yukarıdaki yine de biraz ayrıntılı bir yaklaşımdır.

Bir yerleşik bu senaryo için daha iyi yaklaşımını *UpdateModel()* denetleyicisi taban sınıfta yardımcı yöntemi. Bu güncelleştirme biz gelen form parametreleri kullanarak başarılı bir nesnenin özelliklerini destekler. Özellik adlarının nesne üzerinde belirlemek için yansıtma kullanır ve ardından otomatik olarak dönüştürür ve bunları istemci tarafından gönderilen giriş değerlere göre değerleri atar.

HTTP POST Düzenle bu kodu kullanan Eylemimiz basitleştirmek için UpdateModel() yöntemi kullanabiliriz:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Şu anda ziyaret edebilirsiniz */Dinners/Edit/1* URL ve değişiklik bizim Dinner başlığı:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Biz "Kaydet" düğmesine tıkladığınızda, form postası düzenleme eylemimiz gerçekleştirirsiniz ve güncelleştirilmiş değerleri veritabanında kalıcı hale. Biz sonra ayrıntıları URL'sine (Bu yeni kaydedilen değerleri gösterir) Akşam yönlendirilir:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Düzenleme hataları işleme

Dışında geçerli HTTP POST uygulama works – ince hatalar.

Bir kullanıcı bir form düzenleniyor hata yaptığında, biz form bunları düzeltmek için yol gösteren bir açıklayıcı hata iletisi ile yeniden görüntülenir emin olmanız gerekir. Bu durumda olduğu bir son kullanıcı yazılarını hatalı giriş içerir (örneğin: hatalı biçimlendirilmiş tarih dizesi) giriş biçimi nerede durumlarda da geçerlidir, ancak bir iş kuralı ihlali gibi. Form giriş veri değişikliklerini el ile yenileme zorunda kalmazsınız başlangıçta girilen kullanıcı koruması gerektiğini hataları olduğunda. Form başarıyla tamamlanana kadar bu işlemi gerektiği kadar yineleyin.

ASP.NET MVC, hata işleme ve formu yeniden kolaylaştıran bazı kullanışlı yerleşik özellikler içerir. Şimdi bu özelliklerini iş başında görmek için bizim düzenleme eylem yöntemini aşağıdaki kodla güncelleştirin:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Yukarıdaki kod, biz artık bir try/catch hata işleme bloğu bizim geçici sarmalamış olursunuz dışında önceki kararlılığımızın için – benzerdir. Bir özel durum oluşursa UpdateModel() çağrılırken veya deneyin ve (kaydetmeye çalıştığınız Dinner nesne modelimizi içinde bir kural ihlali nedeniyle geçersizse, bir özel durum oluşturacak) DinnerRepository Kaydet bizim catch hata işleme bloğu olur yürütün. İçindeki Şimdi Akşam nesnesinde mevcut herhangi bir kural ihlali üzerinden döngü ve (hangi kısa bir süre içinde açıklayacağız) ModelState nesnesine ekleyin. Biz sonra görünümü yeniden görüntüleyin.

Bu uygulamayı yeniden çalıştıralım çalışma görmek için bir Akşam Yemeği düzenleyin ve boş bir başlık, "BOGUS", bir EventDate varsa ve UK telefon numarası ABD ülke değeriyle değiştirin. Biz "Kaydet" düğmesine bastığınızda yöntemi bizim HTTP POST Düzenle (hatalar olduğundan) Akşam Yemeği kaydetmek mümkün olmayacaktır ve formu görüntülemek:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Uygulamamız bir makul hata deneyimi vardır. Geçersiz giriş metin öğeleriyle kırmızıyla vurgulanır ve bunlara ilişkin son kullanıcı doğrulama hata iletisi görüntülenir. Herhangi bir şey puanı almak zorunda kalmazsınız formun Ayrıca kullanıcı ilk olarak girilen – giriş verilerini saklayan.

Nasıl isteyebilir, bu gerçekleşti? Nasıl başlık ve EventDate ContactPhone metin kutuları kendilerini kırmızı renkte vurgulayın ve ilk olarak girilen kullanıcı değer çıktısı için bilmeniz? Ve nasıl hata iletileri listesinde en üstünde gösterilen? Güzel bir haberimiz var olan Sihirli tarafından ortaya yaramadı - bunun yerine, giriş doğrulama ve hata işleme senaryoları kolaylaştıran yerleşik ASP.NET MVC özelliklerinden bazıları kullandık olduğundan oluştu.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>ModelState anlama ve doğrulama HTML yardımcı yöntemler

Denetleyici sınıflarına hataları görünümüne geçirilen bir model nesnesi ile mevcut olduğunu belirtmek için bir yol sağlayan bir "ModelState" özellik koleksiyonu vardır. ModelState koleksiyonu içinde hata girişlerini belirlemek model özelliğinin adını sorun (örneğin: "Title", "EventDate" veya "ContactPhone") ve bir insan kullanımı kolay hata iletisi belirtilmesine izin verir (örneğin: "Başlık gerekiyor").

*UpdateModel()* yardımcı yöntemi form değerleri, model nesnesi özelliklere atanmaya çalışılırken hatalarla karşılaştığında laravel'den ModelState koleksiyonu otomatik olarak doldurur. Örneğin, bizim Dinner nesnenin EventDate DateTime türünde özelliğidir. UpdateModel() yöntemi "BOGUS" dize değeri için yukarıdaki senaryoda atayamadı UpdateModel() yöntemi bu özellik ile bir atama hatası gösteren ModelState koleksiyona bir giriş oluşmadı eklenmesi.

Geliştiriciler Ayrıca, etkin kuralı ihlallerini göre girişlerle ModelState koleksiyon doldurma bizim "catch" hata işleme bloğu içinde biz aşağıda yapıyor gibi açıkça hata girişleri ModelState koleksiyona eklemek için kod yazma Akşam Yemeği nesnesi:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>ModelState tümleştirmesiyle HTML Yardımcısı

HTML yardımcı yöntemlerinden - Html.TextBox() gibi-çıkış işlenirken ModelState koleksiyon denetleyin. Öğe için bir hata varsa, kullanıcı tarafından girilen değer ve bir CSS hata sınıfı işleyin.

Örneğin, "Düzenle" bizim görünümünde Html.TextBox() yardımcı yöntem bizim Dinner nesnesinin EventDate işlemek için kullanıyoruz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Görünüm hata senaryosunda işlendiğinde Html.TextBox() yöntemi bizim Dinner nesnenin "EventDate" özellikle ilişkili herhangi bir hata olup olmadığını görmek için ModelState koleksiyon teslim. Bir hata oluştu belirlenen zaman değeri olarak gönderilen kullanıcı girişini ("BOGUS") oluşturulmasını ve css hata sınıfa eklediğiniz &lt;giriş türü "metin" = /&gt; biçimlendirmesi ürettiğini:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Ancak istediğiniz aramak için css hata sınıf görünümünü özelleştirebilirsiniz. Varsayılan CSS hata sınıfının – "giriş-doğrulama hata" – tanımlanan *\content\site.css* aşağıdaki gibi stil sayfası ve görünümler:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

CSS kuralın ne gibi aşağıda vurgulanmasını bizim geçersiz giriş öğelerini kaynaklanır:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() yardımcı yöntemi

Belirli bir model özelliğine ile ilişkili ModelState hata iletisi çıkışı Html.ValidationMessage() yardımcı yöntemi kullanılabilir:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Yukarıdaki kod çıkarır:  *&lt;span class = "alan doğrulama hata"&gt; 'BOGUS' değeri geçersiz &lt; /span&gt;*

Html.ValidationMessage() yardımcı yöntemi, geliştiricilerin görüntülenen hata SMS mesajı geçersiz kılmasına izin veren ikinci bir parametre de destekler:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Yukarıdaki kod çıkarır:  <em>&lt;span class = "alan doğrulama hata"&gt;\*&lt;/span&gt;</em>yerine bir hata için mevcut olduğunda varsayılan hata metni EventDate özelliği.

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() yardımcı yöntemi

Html.ValidationSummary() yardımcı yöntem eşlik bir Özet hata iletisini işlemek için kullanılan bir &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; listesi tüm ayrıntılı hata iletilerini ModelState koleksiyonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Ayrıntılı hataları listesini görüntülemek için bir Özet hata iletisi tanımlar ve isteğe bağlı dize parametresi – Html.ValidationSummary() yardımcı yöntemi alır:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

CSS hata listesinin nasıl göründüğünü geçersiz kılmak için isteğe bağlı olarak kullanabilirsiniz.

#### <a name="using-a-addruleviolations-helper-method"></a>AddRuleViolations yardımcı yöntemini kullanma

İlk HTTP POST Düzenle kararlılığımızın bir foreach deyimi, catch bloğu içinde Dinner nesnenin kural ihlalleri döngü ve denetleyicinin ModelState koleksiyona eklemek için kullanılır:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Bu kod bir "ControllerHelpers" ekleyerek az temizleyici NerdDinner projeye sınıf ve ASP.NET MVC ModelStateDictionary sınıfa yardımcı yöntemini ekleyen "AddRuleViolations" genişletme yöntemi içindeki uygulama yapabiliyoruz. Bu genişletme yöntemi ModelStateDictionary RuleViolation hataları listesini doldurmak için gerekli mantığı kapsülleyen:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Biz, bizim HTTP POST Düzenle eylem yöntemi, Şimdi Akşam kural ihlalleri ModelState koleksiyonuyla doldurmak için bu genişletme yöntemi kullanmak için daha sonra güncelleştirebilirsiniz.

#### <a name="complete-edit-action-method-implementations"></a>Düzenleme eylem yöntem uygulamaları tamamlayın

Aşağıdaki kod tüm gerekli düzenleme senaryomuz için denetleyici mantığı uygular:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Güzel bir şey düzenleme kararlılığımızın hakkında ne bizim denetleyici sınıfı, hem de bizim görünüm şablonu herhangi bir şey belirli doğrulama veya Akşam Yemeği modelimizi tarafından zorunlu kılınmayı iş kuralları hakkında bilgi edinmek sahip olur. Biz ek kurallar modelimiz için gelecekte ekleyebilirsiniz ve bizim denetleyicisi veya desteklenecek edebilmeleri görünümünde herhangi bir kod değişikliği yapmak gerekmez. Bu bize kolayca bizim uygulama gereksinimlerini gelecekte en az kod değişikliği geliştiren esnekliği sağlar.

### <a name="create-support"></a>Oluşturma desteği

Bizim DinnersController sınıfı "Düzenle" davranışını uygulama bitirdikten sonra. Artık "Oluştur" Destek üzerinde – kullanıcıların yeni azalma eklemesine olanak sağlayan uygulamak geçelim.

#### <a name="the-http-get-create-action-method"></a>HTTP GET oluşturma eylem yöntemi

Biz "GET" HTTP davranışını uygulayarak başlarsınız bizim eylem yöntemi oluşturun. Birisi ziyaret ettiğinde bu yöntem çağrılacak */azalma/Create* URL'si. Kararlılığımızın şuna benzer:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Yukarıdaki kod, Akşam Yemeği yeni bir nesne oluşturur ve bir hafta gelecekte olacak şekilde kendi EventDate özelliğine atar. Ardından, yeni Dinner nesnesine bağlı bir görünümü işler. Biz bir adla açıkça geçirmediğinden henüz *View()* yardımcı yöntemi, tabanlı varsayılan yol şablonu görüntüle çözmek için kullanacağı: /Views/Dinners/Create.aspx.

Artık bu görünüm şablonu oluşturalım. Oluştur eylemi yöntemi içinde sağ tıklatıp "Görünüm Ekle" bağlam menüsü komutu'i seçerek bunu yapabilirsiniz. "Görünüm Ekle" iletişim kutusunun içinden biz size Dinner nesne görünümü şablona geçirme ve otomatik yapı iskelesi için "Oluştur" şablonu seçin belirtmek:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Biz "Ekle" düğmesine tıkladığınızda, Visual Studio yeni iskele tabanlı "Create.aspx" Görünüm "\Views\Dinners" dizine kaydedin ve IDE içinde açın:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Şimdi oluşturulan varsayılan "Oluştur" iskele dosyasına bizim için birkaç değişiklik yapmak ve altında görünmesi için yukarı değiştirin:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Ve şimdi biz çalıştırıldığında, uygulama ve erişim *"/ azalma/oluştur"* Oluştur eylemi kararlılığımızın aşağıdaki gibi kullanıcı Arabirimi oluşturmak ve tarayıcıdaki URL'si:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Oluşturma eylem yöntemini HTTP POST uygulama

Uygulanan bizim oluşturma eylem yöntemi GET HTTP sürümüne sahibiz. Kullanıcı "Kaydet" düğmesine tıkladığında bir form post gerçekleştirdiği */azalma/Create* URL, HTML sorgulayan ve &lt;giriş&gt; form değerleri HTTP POST edimi kullanılarak.

Şimdi artık HTTP POST davranışını uygulayan bizim eylem yöntemi oluşturun. HTTP POST senaryoları işleme gösteren bir "AcceptVerbs" özniteliği içeren bizim DinnersController için aşırı yüklenmiş bir "Oluştur" eylem yöntemine ekleyerek başlayın:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Çeşitli şekillerde gönderilen form parametreleri "Oluştur" bizim etkin HTTP POST yöntemi içinde erişebilirsiniz vardır.

Bir yaklaşım ise yeni bir Akşam Yemeği nesnesi oluşturun ve ardından *UpdateModel()* (düzenleme işlemini ile yaptığımız gibi) yardımcı yöntem gönderilen formu değerlerle doldurulacak. Biz sonra için sunduğumuz DinnerRepository ekleyin, veritabanına kalıcı olması ve aşağıdaki kodu kullanarak yeni oluşturulan Akşam Yemeği gösterilecek ayrıntı eylemimiz kullanıcıyı yönlendirir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternatif olarak, Şimdi Akşam nesneye yöntemi parametre olarak geçirmesine bizim Create() eylem yöntemine sahip olduğumuz biz bir yaklaşımı kullanabilirsiniz. ASP.NET MVC sonra otomatik olarak bizim için yeni bir Akşam Yemeği nesne örneği, form girişlerini kullanarak özelliklerini doldurmak ve bizim eylem yönteme geçirin:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Yukarıdaki bizim eylem yöntemi, Şimdi Akşam nesne başarıyla ile form gönderme değerlerini ModelState.IsValid özelliğini kontrol ederek doldurulmadı olduğunu doğrular. Bu giriş dönüştürme sorunları varsa false döndürür (örneğin: "BOGUS" dizesi EventDate özelliği için), ve herhangi bir sorun varsa, eylem yöntemi form görüntüler.

Giriş değeri geçerliyse, eylem yöntemi ekleyin ve yeni Akşam Yemeği için DinnerRepository kaydetmek çalışır. Bir try/catch bloğu içinde bu iş sarmalar ve (hangi dinnerRepository.Save() yöntemi bir özel durum neden olur) herhangi bir iş kuralı ihlali varsa form görüntüler.

Bu hata işleme davranışını iş başında görmek için biz isteyebilir */azalma/Create* URL ve yeni bir Akşam Yemeği ayrıntılarını doldurun. Hatalı giriş veya değerleri hatalarla aşağıda belirtildiği gibi görünürler oluştur formunu neden olur:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Oluşturma formumuzu tam aynı doğrulama ve iş kuralları bizim düzenleme formunda nasıl uygularken dikkat edin. Bizim doğrulama ve iş kuralları modelde tanımlanan ve kullanıcı Arabirimi veya uygulama denetleyici içinde eklenmediği olmasıdır. Bu, size daha sonra Değiştir/bizim doğrulama geliştirebilirsiniz veya tek bir iş kuralları yerleştirin ve bunların uygulama genelinde uygulamak anlamına gelir. Size sunduğumuz düzenleme ya da içinde herhangi bir kod değişikliği veya herhangi bir yeni bir kural veya var olanları değişiklikler otomatik olarak uymanız eylem yöntemleri oluşturmak yoktur.

Giriş değerleri düzeltin ve "Kaydet" düğmesine tıklayın, yeniden DinnerRepository bizim ekleme başarılı olur ve yeni Dinner veritabanına eklenir. Biz ardından yönlendirilecek */Dinners/Ayrıntılar / [ID]* burada biz gösterilir yeni oluşturulan Akşam Yemeği ayrıntılarını içeren URL –:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Destek Sil

Artık "Sil" destek için sunduğumuz DinnersController ekleyelim.

#### <a name="the-http-get-delete-action-method"></a>HTTP GET silme eylem yöntemi

Biz, bizim delete eylem yöntemini HTTP GET davranışını uygulayarak başlarsınız. Birisi ziyaret ettiğinde bu yöntem çağrılmadığı */Dinners/Delete / [ID]* URL'si. Uygulama aşağıdadır:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Eylem yöntemi, silinecek Akşam Yemeği almayı dener. Akşam Yemeği işler varsa bir görünüm Dinner nesnesine bağlı. Nesne yok (veya zaten silinmiş olan), "Bulunamadı" işleyen bir görünüm verir bizim "Details" eylem yöntemi için daha önce oluşturduğumuz şablonu görüntüleyin.

"Sil" Görünüm şablonu silme eylem yöntemi içinde sağ tıklayıp "Görünüm Ekle" bağlam menüsü komutu tarafından oluşturabilirsiniz. "Görünüm Ekle" iletişim kutusunun içinden biz biz Dinner nesnesi, model olarak bizim görünümü şablona geçirme ve boş bir şablon oluşturmayı tercih belirtmek:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Biz "Ekle" düğmesine tıkladığınızda, Visual Studio yeni "Delete.aspx" görünümü şablon dosyası bizim için sunduğumuz "\Views\Dinners" dizininde ekler. Bazı HTML ve kod silme onay ekranında uygulamak için şablona ekleyeceğiz aşağıdaki gibi:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Yukarıdaki kod başlığı silinecek Akşam Yemeği ve çıktıları görüntüler bir &lt;form&gt; son içindeki "Sil" düğmesini tıklatırsa /Dinners/Delete / [ID] URL'sine bir GÖNDERİ yapan öğesi.

Biz çalıştırıldığında, uygulama ve erişim *"/ azalma/Delete / [ID]"* URL'si geçerli bir Akşam Yemeği nesnesi UI gibi aşağıda oluşturur:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Yan konu: Neden bir GÖNDERİ düşünüyorsunuz?** |
| --- |
| İsteyebilir – neden oldu oluşturma çaba bir &lt;form&gt; bizim silme onayı ekranındaki? Neden gerçek silme işlemini gerçekleştiren bir eylem yöntemine bağlamak için yalnızca standart köprü kullanırsınız? Web gezginleri karşı korumak ve arama motorları Url'lerimizi bulma ve yanlışlıkla veri bağlantıları takip ettiler silinmesine neden dikkat etmek istediğimizden nedenidir. Temel HTTP GET URL'leri "erişim/gezinme için güvenli" olarak değerlendirilir ve HTTP-POST olanları izlemeyecek şekilde gerekiyor. Her zaman zararlı koymadan veya veri değiştirme işlemleri HTTP POST istekleri arkasında emin olmak iyi bir kuraldır. |

#### <a name="implementing-the-http-post-delete-action-method"></a>HTTP POST silme eylem yöntemi uygulama

Artık bir silme onay ekranı görüntüleyen HTTP GET sürümüne uygulanan bizim silme eylem yönteminin sahibiz. Son kullanıcı "Sil" düğmesini tıkladığında bir form post gerçekleştireceği */Dinners/Dinner / [ID]* URL'si.

Şimdi aşağıdaki kodu kullanarak silme eylem yöntemini HTTP "POST" davranışı hemen uygulayın:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Bizim Delete eylem yöntemini HTTP POST sürümü, Şimdi Akşam nesne silme almayı dener. (Zaten silinmiş olduğundan), bulamazsa, bizim "Bulunamadı" şablonu oluşturur. Akşam Yemeği bulursa, onu DinnerRepository siler. Ardından, "Silinmiş" şablonu oluşturur.

"Silinmiş" şablonu uygulamak için biz eylem yönteminde sağ tıklayın ve "Görünüm Ekle" bağlam menüsünü seçin. Biz "Silindi" müşterilerimize görünüm adını ve boş bir şablon olması (ve kesin olarak belirlenmiş model nesnesi katılmak değil) sahip. Ardından bazı HTML içeriği için ekleyeceğiz:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Ve şimdi biz çalıştırıldığında, uygulama ve erişim *"/ azalma/Delete / [ID]"* aşağıdaki gibi bizim Dinner oluşturmak nesnesini silme onayı geçerli bir Akşam Yemeği URL'sini ekranında:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Biz "Sil" düğmesini tıkladığınızda bir HTTP POST gerçekleştireceği */Dinners/Delete / [ID]* Akşam Yemeği bizim veritabanından kalıcı olarak silmek ve bizim "Silinmiş" görünümü şablon görüntü URL'si:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Model bağlama güvenliği

Yerleşik ASP.NET MVC model bağlama özelliklerini kullanmak için iki farklı şekilde ele almıştık. İlk UpdateModel() yöntemi var olan bir model nesnesi üzerinde özelliklerini güncelleştirmek için ve ikinci kullanarak ASP.NET MVC'nin desteği eylem metodu parametreleriyle model nesneleri geçirme. Bu teknikler her ikisini birden çok güçlü ve son derece kullanışlıdır.

Bu da onunla kazandırır sorumluluk. Herhangi bir kullanıcı girişi kabul ederken her zaman güvenlik hakkında paranoid olmanız önemlidir ve bu da form girişi nesneleri bağlama true olur. İçin dikkatli olmanız gerekir her zaman HTML kodlama HTML ve JavaScript ekleme saldırılarını önlemek ve SQL ekleme saldırıları dikkatli bir kullanıcı tarafından girilen değerleri (Not: Bu önlemek için parametreleri otomatik olarak kodlar uygulamamız için LINQ to SQL kullanıyoruz tür saldırılar). Hiçbir zaman kullanan istemci tarafı doğrulama başına ve bilgisayar korsanlarının sahte değerleri gönderilmeye çalışılırken karşı koruma sağlamak için sunucu tarafı doğrulama her zaman eşitlemeli gerekir.

ASP.NET MVC bağlama özelliklerini kullanırken düşündüğünüzden emin olmak için bir ek güvenlik kapsamı bağlama nesnelerin öğesidir. Özellikle bağımlı olması ve yalnızca gerçekten güncelleştirilmesi için bir son kullanıcı tarafından güncelleştirilebilir olmalıdır özelliklere izin ver emin olmak için izin verme özellikler güvenlik etkilerini anladığınızdan emin olmak istersiniz.

Varsayılan olarak, UpdateModel() yöntemi model nesnesi üzerinde gelen form parametre değerlerini eşleşen tüm özelliklerini güncelleştirme dener. Benzer şekilde, nesneleri eylem metodu parametreleriyle de varsayılan olarak geçirilen tüm özellikleri form parametreleri aracılığıyla ayarlanan olabilir.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Kullanım başına temelinde bağlama kilitlemek

Kullanım başına temelinde bağlama ilkesi aşağı sağlayan bir açık "ekleme" güncelleştirilebilir özelliklerini tarafından listesi kilitleyebilirsiniz. Bu, aşağıdaki gibi UpdateModel() yöntemine bir ek dize dizisi parametre geçirerek yapılabilir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Bir "ekleme listesi" gibi aşağıda belirtilmesi özelliklerine izin sağlayan bir [bağlama] özniteliği eylem metodu parametreleriyle ayrıca geçirilen nesneleri destekler:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bağlama türü temelinde kilitlemek

Tür başına temelinde bağlama kurallarını aşağı kilitleyebilirsiniz. Bu, bağlama kurallarını belirlemek ve bunları tüm denetleyiciler ve eylem yöntemlerine (UpdateModel hem eylem yöntemi parametresini senaryoları dahil) tüm senaryolarda geçerli olması sağlar.

Tür başına bağlama kurallarını bir [bağlama] özniteliği bir tür üzerine ekleyerek veya (burada türüne ait olmayan senaryolar için yararlıdır) uygulamasının Global.asax dosyası içinde kaydederek özelleştirebilirsiniz. Sonra bağlama özniteliğin Ekle kullanabilirsiniz ve hangi özellikleri denetlemek için dışlama özellikler belirli bir sınıf veya arabirim için bağlanabilir.

Biz bu tekniği NerdDinner uygulamamız için Dinner sınıfı ve bağlanabilir özellikler listesinde, aşağıdaki kısıtlayan ona [bağlama] özniteliği ekleyin:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Biz bağlama aracılığıyla yönetilebilmesini LCV'ler koleksiyon sağlanmıyor ya da biz bağlama aracılığıyla ayarlanacak DinnerID veya HostedBy özellikler sağlayan dikkat edin. Güvenlik nedenleriyle biz bunun yerine yalnızca bizim eylem yöntemleri içinde açık kod kullanarak bu belirli özellikleri yöneten.

### <a name="crud-wrap-up"></a>CRUD Wrap-Up

ASP.NET MVC form gönderme senaryoları uygulanmasına yardımcı olan yerleşik özellikler içerir. Bizim DinnerRepository üzerine CRUD UI desteği sağlamak için bu özellikleri çeşitli kullandık.

Uygulamamızı uygulamak için model odaklı bir yaklaşım kullanılmıştır. Bu, tüm doğrulama ve iş kuralı mantığı bizim modeli katmanı içinde– ve bizim denetleyicileri veya görünümleri içinde değil tanımlanır anlamına gelir. Bizim denetleyici sınıfı ya da Görünüm şablonlarımız, Şimdi Akşam model sınıfı tarafından zorunlu kılınmayı belirli iş kuralları hakkında bir şey bilmek.

Bu uygulama mimarimiz temiz tutmaya ve test kolaylaştırır. Ek iş kuralları için sunduğumuz modeli katmanı gelecekte ekleyebiliriz ve *herhangi bir kod değişikliği yapmanız gerekmez* bizim denetleyicisine veya edebilmeleri görünümü desteklenmektedir. Bu, bize çeviklik evrim geçiren ve gelecekte uygulamamız değiştirmek için büyük ölçüde ile sağlamak için geçiyor.

Bizim DinnersController Şimdi Akşam Yemeği listeleme/Ayrıntılar, sağlar yanı sıra oluşturun, düzenleyin ve Sil desteği. Sınıf için tam kod altında bulunabilir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Sonraki adım

Artık bizim DinnersController sınıf içinde uygulama temel CRUD (oluşturma, okuma, güncelleştirme ve silme) desteği sunuyoruz.

Şimdi nasıl ViewData ve ViewModel sınıfları bile daha zengin kullanıcı Arabirimi bizim formlarında etkinleştirmek için kullanabilir miyiz göz atalım.

> [!div class="step-by-step"]
> [Önceki](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [İleri](use-viewdata-and-implement-viewmodel-classes.md)
