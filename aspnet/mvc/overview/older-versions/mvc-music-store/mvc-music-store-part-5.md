---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: '5. Kısım: Formları düzenle ve şablon | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 5 formları düzenle ve şablon kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874918"
---
<a name="part-5-edit-forms-and-templating"></a>5. Kısım: Formları düzenle ve şablon oluşturma
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.
> 
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 5 formları düzenle ve şablon kapsar.


Önceki bölümde, biz bizim veritabanından veri yükleme ve onu görüntüleme. Bu bölümde, biz de verileri düzenleme etkinleştirin.

## <a name="creating-the-storemanagercontroller"></a>StoreManagerController oluşturma

Biz adlı yeni bir denetleyici oluşturarak başlarsınız **StoreManagerController**. Bu denetleyici için biz ASP.NET MVC 3 araçları güncelleştirme kullanılabilir yapı İskelesi özelliklerinden yararlanma. Aşağıda gösterildiği gibi denetleyici Ekle iletişim kutusu için seçenekleri ayarlayın.

![](mvc-music-store-part-5/_static/image1.png)

Ekle düğmesine tıkladığınızda, ASP.NET MVC 3 yapı iskelesi mekanizması iyi tutar iş sizin için yapar olduğunu görürsünüz:

- Yeni StoreManagerController sahip yerel bir Entity Framework değişken oluşturur
- Projenin görünümleri klasörüne StoreManager klasör ekler
- Albüm sınıfına kesin türü belirtilmiş Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml ve Index.cshtml görünümü ekler

![](mvc-music-store-part-5/_static/image2.png)

CRUD yeni StoreManager denetleyicisi sınıfı içerir (oluşturma, okuma, güncelleştirme, silme) albümü ile çalışmaya nasıl bilmeniz denetleyici eylemleri model sınıfı ve veritabanı erişimi için bizim Entity Framework bağlamına kullanın.

## <a name="modifying-a-scaffolded-view"></a>Kurulmuş bir görünümü değiştirme

Bu kod bize oluşturuldu, ancak yalnızca biz Bu öğreticide yazma gibi standart ASP.NET MVC kod olduğundan, unutmamak önemlidir. Demirbaş denetleyicisi kod yazma ve el ile güçlü şekilde yazılan görünümler oluşturma harcadığı zamanı kaydetmek hedeflenen, ancak bu tür nasıl değiştiğini dpm'nin ilgili açıklama doğrudan uyarılarla başında oluşturulan kodu görülen değil kod. Bu, kodunuzu ve değiştirmek için bekleniyor.

Bu nedenle, StoreManager dizin görüntülemek için hızlı bir düzenleme ile başlayalım (/ Views/StoreManager/Index.cshtml). Bu görünüm bizim deposundaki albümleri düzenleyin listeleyen bir tablo görüntüler / Ayrıntılar / silme bağlantılar ve albüm genel özellikleri içerir. Bu görünümde çok yararlı olmadığından biz AlbumArtUrl alan kaldırırsınız. İçinde &lt;tablo&gt; bölüm görünümü kodunu, kaldırma &lt;th&gt; ve &lt;td&gt; AlbumArtUrl başvuruları aşağıda vurgulanan satırlar tarafından belirtildiği şekilde çevreleyen öğeleri:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Değiştirilen görünümü kod şu şekilde görünür:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Mağaza Yöneticisi ilk bakma

Şimdi uygulamayı çalıştırın ve/StoreManager/göz atın. Bu, Depolama Yöneticisi biz yalnızca, bağlantılar, ayrıntı, düzenleme ve silme ile deposundaki albümleri listesini gösteren değiştiren dizin görüntüler.

![](mvc-music-store-part-5/_static/image3.png)

Düzenleme bağlantısını tıklatarak alanları düzenleme formla tarz ve sanatçı bırakmalar de dahil olmak üzere, albüm görüntüler.

![](mvc-music-store-part-5/_static/image4.png)

Altındaki "Geri listeye" bağlantısını tıklatın ve bir Albüm Ayrıntıları bağlantıyı tıklatın. Bu, tek tek albüm için ayrıntılı bilgileri görüntüler.

![](mvc-music-store-part-5/_static/image5.png)

Yeniden listesi bağlamak için Geri'yi tıklatın, sonra bir Delete bağlantısına tıklayın. Bu, albüm ayrıntılarını gösteren ve biz biz silmek istediğinizden emin olup olmadığınızı soran bir onay iletişim kutusunda, görüntüler.

![](mvc-music-store-part-5/_static/image6.png)

Altındaki Sil düğmesini tıklatarak albümü silin ve silinmiş albümü gösterir dizin sayfasına dönün.

Mağaza Yöneticisi ile bitmek değil, ancak denetleyici ve görünüm kod başlangıcı CRUD işlemleri için çalışma vardır.

## <a name="looking-at-the-store-manager-controller-code"></a>Mağaza Yöneticisi denetleyicisi koda bakmak

Mağaza Yöneticisi denetleyicisi iyi miktarda kod içerir. Bu işlemlerde üstten alta edelim. Bazı standart ad alanları bizim modelleri ad alanı referansı yanı sıra, MVC denetleyicisi için denetleyici içerir. Denetleyici MusicStoreEntities, her denetleyici eylemleri tarafından kullanılan veri erişim için özel bir örneğine sahip.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Yöneticisi dizin ve ayrıntıları eylemlerini depolama

Dizin görünümünün albümleri, daha önce deposu Gözat yöntemi çalışırken gördüğümüz gibi her albüm başvurulan tarz ve sanatçı bilgileri de dahil olmak üzere bir listesini alır. Denetleyici verimli ve özgün istekteki bu bilgiler için sorgulama için her albüm Tarz adı ve sanatçı adı görüntüleyebilmesi dizin görünümünün bağlı nesnelere başvurular takip ediyor.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

StoreManager denetleyicinin ayrıntıları denetleyici eylemi tam olarak aynı yazdığımız daha önce - albüm için sorgular deposu Denetleyicisi ayrıntıları eylem olarak çalışır Find() yöntemini kullanarak Kimliğine göre sonra görünümüne döndürür.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Eylem yöntemleri oluşturma

Form girişi işlediğinden Create eylem yöntemlerini o ana kadarki gördük olanları biraz farklıdır. Bir kullanıcı ilk /StoreManager/Oluştur/ziyaret ettiğinde boş bir form gösterilir. Bu HTML sayfası içerecek bir &lt;form&gt; açılır ve metin kutusu içeren öğe girişi burada bunlar girebilirsiniz Albüm Ayrıntıları öğeleri.

Kullanıcı albüm form değerlerine doldurur sonra bunlar uygulamamız veritabanı içinde kaydetmek için bu değişiklikleri geri göndermek için "Kaydet" düğmesine basabilirsiniz. Kullanıcı "Kaydet" düğmesine bastığında &lt;form&gt; geri /StoreManager/Oluştur/URL için bir HTTP POST gerçekleştirmek ve gönderme &lt;form&gt; değerleri HTTP POST bir parçası olarak.

ASP.NET MVC bize bize bizim StoreManagerController sınıfı içinde – /StoreManager/Create ilk HTTP GET Gözat işlemek için iki ayrı "Oluştur" eylem yöntemleri uygulamak etkinleştirerek bu iki URL çağırma senaryolar mantığı oluşturan kolayca bölüneceği sağlar / URL ve diğer gönderilen değişiklikleri HTTP POST işlemek için.

### <a name="passing-information-to-a-view-using-viewbag"></a>Görünüm paketi kullanarak bir görünüme bilgi geçirme

Biz ViewBag Bu öğreticide daha önce kullandıysanız, ancak çok ilgili açıklandı henüz. Görünüm Paketi bize bilgi kesin türü belirtilmiş model nesnesi kullanmadan görünüme iletmek sağlar. Bu durumda, bizim Düzenle HTTP GET denetleyici eylemi forma bırakmalar doldurmak için her iki tür ve listesini Sanatçılar geçirmek gerekir ve bunu yapmanın en kolay yolu ViewBag öğeleri olarak döndürülecek.

Görünüm paketini bir dinamik Nesne ViewBag.Foo veya ViewBag.YourNameHere bu özellikleri tanımlamak için kod yazmadan yazabilirsiniz olduğunu anlamına gelir. Bu durumda, form ile gönderilen açılır değerler bunlar ayarlamak albümü özellikleri GenreId ve ArtistId, böylece denetleyicisi kodu ViewBag.GenreId ve ViewBag.ArtistId kullanır.

Bu açılır liste değerleri için yalnızca bu amaçla oluşturulmuş SelectList nesnesini kullanarak form döndürülür. Bu gerçekleştirilir gibi bu kodu kullanarak:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Eylem yöntemi koddan gördüğünüz gibi bu nesnenin oluşturulacağı üç parametre kullanılıyor:

- Aşağı açılır görüntüleme öğeleri listesi. Bu yalnızca bir dize değil - türler listesini geçirme unutmayın.
- SelectList geçirilen sonraki parametresi seçili bir değerdir. Bu önceden listesindeki bir öğeyi seçmek nasıl SelectList nasıl bilir. Bu, biz oldukça benzer düzenleme formu baktığınızda anlamak daha kolay olacaktır.
- Son parametre görüntülenecek özelliğidir. Bu durumda, bu Genre.Name özelliği kullanıcıya gösterilecek olduğunu belirten.

Aklınızda ile daha sonra HTTP GET oluşturma eylem oldukça basittir - iki SelectLists ViewBag eklenir ve (bunu henüz oluşturulmamıştır bu yana) hiçbir model nesnesi forma geçirilir.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>HTML Yardımcıları Bırakma listeleri oluşturma görüntülemek için

Biz nasıl değerleri aşağı açılan geçirilir görünümüne hakkında açıklandı olduğundan, bu değerlerin nasıl görüntüleneceğini görmek için görünümü hızlı bir göz atalım. Görünüm kodda (/ Views/StoreManager/Create.cshtml), aşağıdaki çağrıyı Tarz açılır görüntülenecek yapıldığında görürsünüz aşağı.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Bu, bir HTML Yardımcısı - ortak bir görünüm görevi gerçekleştiren bir yardımcı program yöntemi bilinir. HTML Yardımcıları kısa ve okunabilir bizim görünümü kodu tutma çok yararlı olur. Html.DropDownList yardımcı ASP.NET MVC tarafından sağlanır, ancak daha sonra göreceğiz bizim uygulamada yeniden görünümü kodu için kendi Yardımcıları oluşturmak mümkün olur.

Html.DropDownList çağrısı, yalnızca iki şey - get görüntülemek için listede ve (varsa) hangi değer önceden seçilmiş olmalıdır burada söylediyse gerekir. İlk parametresi, GenreId, model veya ViewBag GenreId adlı bir değeri aramak için DropDownList bildirir. İkinci parametre olarak başlangıçta aşağı açılan listeden seçilen göstermek için değeri belirtmek için kullanılır. Bu form bir form oluştur olduğundan, hiçbir değer seçilmiş olması ve String.Empty geçirilir.

### <a name="handling-the-posted-form-values"></a>Gönderilen Form değerleri işleme

Biz önce anlatıldığı gibi her bir formla ilişkili iki eylem yöntemleri vardır. İlk HTTP GET isteği işler ve formu görüntüler. İkinci gönderilen form değerleri içeren HTTP POST isteği işler. Denetleyici eylemini ASP.NET MVC, yalnızca HTTP POST isteklerini yanıtlaması gerektiğini söyleyen bir [HttpPost] özniteliği olduğuna dikkat edin.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Bu eylem, dört sorumlulukları vardır:

- 1. Form değerleri okuma
- 2. Form değerleri tüm doğrulama kuralları geçirdiğiniz denetleyin
- 3. Form gönderme geçerliyse, verileri Kaydet ve güncelleştirilmiş listesini görüntüleme
- 4. Form gönderme geçerli değilse, doğrulama hataları olan formu yeniden görüntüleyin

#### <a name="reading-form-values-with-model-binding"></a>Model bağlama ile okuma Form değerleri

Denetleyici eylemini başlık, fiyat ve AlbumArtUrl değerlerinden GenreId ve ArtistId (aşağı açılan listeden) ve metin değerleri içeren bir form gönderme işliyor. Form değerleri doğrudan erişmesini mümkün olsa da, daha iyi bir yaklaşım ASP.NET MVC yerleşik Model bağlama özelliklerini kullanmaktır. Bir denetleyici eylemi bir model türünü bir parametre olarak aldığında, ASP.NET MVC form girişleri (yanı sıra yol ve sorgu dizesi değerleri) kullanarak bu türde bir nesne doldurmak dener. Bunu adları eşleşen model nesnesi özelliklerini örneğin değerlerini bakarak yapar yeni albüm nesnenin GenreId değeri ayarlanırken GenreId ada sahip bir giriş arar. ASP.NET MVC uygulamasında standart yöntemlerini kullanarak görünümler oluşturduğunuzda, formları Bu alan adları yalnızca eşleşmemesidir giriş alan adları olarak özellik adlarını kullanarak her zaman işlenir.

#### <a name="validating-the-model"></a>Model doğrulama

Model ModelState.IsValid basit çağrısıyla doğrulanır. Henüz hiçbir doğrulama kuralları bizim albüm sınıfına eklemediniz - bit - bu nedenle şu anda bu denetimi yapmak için çok yok bunu. Önemli olan bu ModelStat.IsValid onay doğrulama kuralları ileride yapılacak değişiklikler denetleyici eylem kodu herhangi bir güncelleştirme gerektiren olmaz böylece biz bizim model üzerinde put doğrulama kuralları Uyarlanır ' dir.

#### <a name="saving-the-submitted-values"></a>Gönderilen değerler kaydetme

Form gönderme doğrulama başarılı olursa, bu değerleri veritabanına kaydetmek için saattir. Entity Framework ile yalnızca model albümleri koleksiyona ekleme ve SaveChanges çağırma gerektirir.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework değeri kalıcı hale getirmek için uygun SQL komutlarını oluşturur. Bizim güncelleştirme görebiliriz böylece veriler kaydedildikten sonra biz albümleri listeye yeniden yönlendirin. Bu, görüntülenen istiyoruz denetleyici eylemi adıyla RedirectToAction döndürerek gerçekleştirilir. Bu durumda, dizin yöntemidir.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Geçersiz form gönderimlerini doğrulama hataları ile görüntüleme

Geçersiz form girişi durumunda açılır değerleri (HTTP-GET izlemesinde) ViewBag eklenir ve ilişkili model değerler görüntülenmek üzere geri görünümüne geçirilir. Doğrulama hataları, kullanarak otomatik olarak görüntülenir @Html.ValidationMessageFor HTML Yardımcısı.

#### <a name="testing-the-create-form"></a>Form Oluştur test etme

Bu, çalışma /StoreManager/Oluştur / - göz atın ve uygulama test etmek için bu, StoreController oluşturma HTTP GET yöntemi tarafından döndürülen boş bir form gösterir.

Bazı değerleri doldurun ve form gönderme için Oluştur düğmesine tıklayın.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Düzenlemelerini işleme

Düzen eylem çifti (HTTP-GET ve HTTP POST) yalnızca inceledik oluşturma eylem yöntemlerine çok benzer. İçindeki rota üzerinden geçen varolan albümü, düzenleme HTTP yöntemi "id" parametresi temelinde albüm yükler GET ile çalışma düzenleme senaryo içerir. Daha önce ayrıntıları denetleyici eylemi inceledik AlbumId tarafından albüm almak için bu kodu aynıdır. Gibi oluşturma / HTTP GET yöntemi değerleri aşağı açılan ViewBag döndürülür. Bu bize albüm bizim model nesnesi olarak (hangi albüm sınıfına kesin türü belirtilmiş) görünüme dönmek ek veriler (örneğin türler listesi) görünüm paketini geçirirken sağlar.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Düzen HTTP POST eylem oluşturma HTTP POST eyleme çok benzer. Tek fark yeni albümü db ekleme yerine olmasıdır. Albümleri koleksiyonu db kullanarak biz albümü geçerli örneği bulma. Entry(Album) ve durumuna Modified olarak ayarlama. Bu, yeni bir tane oluşturmak yerine var olan bir albümü değiştiriyorsunuz Entity Framework bildirir.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Biz bu çıkışı uygulaması çalıştıran ve tarama/StoreManger/sonra bir albümü düzenleme bağlantısını tıklatarak test edebilirsiniz.

![](mvc-music-store-part-5/_static/image9.png)

Bu düzen HTTP GET yöntemi tarafından gösterilen düzenleme formu görüntüler. Bazı değerleri doldurun ve Kaydet düğmesine tıklayın.

![](mvc-music-store-part-5/_static/image10.png)

Bu form yazılarını, değerleri kaydeder ve bize değerler güncelleştirildi gösteren albüm listeye döndürür.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>İşleme silme

Silme, düzenleme ve onay formu görüntülemek için bir denetleyici eylemi ve form gönderme işlemek için başka bir denetleyici eylemi kullanarak oluşturun, olarak aynı düzeni izler.

HTTP GET silme denetleyici eylemi tam olarak bizim önceki Deposu Yöneticisi ayrıntıları denetleyici eylemi ile aynıdır.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Biz kesin türü belirtilmiş bir form Delete Görünümü içerik şablonu kullanarak bir albüm türünü görüntüler.

![](mvc-music-store-part-5/_static/image12.png)

Modeli için tüm alanlar Delete şablon gösterir ancak Biz bu aşağı oldukça biraz basitleştirebilirsiniz. /Views/StoreManager/Delete.cshtml görünüm kodda şu şekilde değiştirin.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Bu, Basitleştirilmiş bir silme onayı görüntüler.

![](mvc-music-store-part-5/_static/image13.png)

Sil düğmesini tıklatarak DeleteConfirmed eylemi yürüten sunucuya geri, postalama forma neden olur.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Bizim HTTP POST silme denetleyici eylemi aşağıdaki eylemleri gerçekleştirir:

- 1. Kimliğe göre albüm yükler
- 2. Albüm siler ve değişiklikleri kaydedin
- 3. Albüm listeden kaldırıldığını gösteren dizin yönlendirir

Bu test etmek için uygulamayı çalıştırın ve /StoreManager için göz atın. Albüm listeden seçip Sil bağlantısını tıklayın.

![](mvc-music-store-part-5/_static/image14.png)

Bu bizim Sil onay ekranı görüntüler.

![](mvc-music-store-part-5/_static/image15.png)

Sil düğmesini tıklatarak albümü kaldırır ve bize albümü silinip silinmediğini gösterir Depolama Yöneticisi dizin sayfasına döndürür.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Metin kesmek için özel bir HTML Yardımcısını kullanma

Mağaza Yöneticisi dizin sayfamızı biz olası bir sorunu açıyor. Bizim albüm başlığı ve sanatçı adı özelliklerini hem de bunların bizim tablosunu biçimlendirme kapalı throw yeterince uzun olabilir. Bunlar ve diğer özellikleri bizim görünümlerinde kolayca kesmek için bize izin vermek için özel bir HTML Yardımcısı oluşturacağız.

![](mvc-music-store-part-5/_static/image17.png)

Razor'ın @helper sözdizimi yaptı bu oldukça kolay kendi yardımcı işlevleri kullanmak için görünümler oluşturun. /Views/StoreManager/Index.cshtml görünümünü açın ve hemen sonra aşağıdaki kodu ekleyin @model satır.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Bu yardımcı yöntemi, bir dize ve izin vermek için en fazla alır. Sağlanan metin belirtilen uzunluktan daha kısaysa, yardımcı olarak çıkarır-değil. Daha uzun olması durumunda metni keser ve işler geri kalanı "...".

Şimdi biz albüm başlığı ve sanatçı adı özelliklerini 25 karakterden az olduğundan emin olmak için bizim Truncate Yardımcısı kullanabilirsiniz. Aşağıda yeni bizim Truncate Yardımcısını kullanarak tam görünümü kodu görüntülenir.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Şimdi biz /StoreManager/ URL göz attığınızda, Albümler ve başlıkları bizim en çok uzunlukları tutulur.

![](mvc-music-store-part-5/_static/image18.png)

Not: Bu oluşturma ve tek bir görünümde bir Yardımcısını kullanarak basit bir durumda gösterir. Siteniz kullanabilirsiniz Yardımcıları oluşturma hakkında daha fazla bilgi edinmek için my blog gönderisi bakın: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-4.md)
> [sonraki](mvc-music-store-part-6.md)
