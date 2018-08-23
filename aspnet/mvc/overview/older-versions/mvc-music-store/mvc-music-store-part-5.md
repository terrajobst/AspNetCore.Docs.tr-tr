---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: '5. Bölüm: Formları düzenleme ve şablon oluşturma | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 5. Bölüm formları düzenleme ve şablon oluşturma kapsar.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: c1065dcb45b6d28672edba32b95c7fc476c8b944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756179"
---
<a name="part-5-edit-forms-and-templating"></a>5. Bölüm: Formları düzenleme ve şablon oluşturma
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.
> 
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 5. Bölüm formları düzenleme ve şablon oluşturma kapsar.


Son bölümde, biz bizim veritabanından veri yükleme ve görüntülemeden. Bu bölümde, biz de verileri düzenleme tıklatmalarını sağlarsınız.

## <a name="creating-the-storemanagercontroller"></a>StoreManagerController oluşturma

Biz adlı yeni bir denetleyici oluşturarak başlarsınız **StoreManagerController**. Bu denetleyici için biz ASP.NET MVC 3 araçları güncelleştirme kullanılabilir olan yapı İskelesi özelliklerinin avantajlarından yararlanmakla. Denetleyici Ekle iletişim kutusu seçenekleri aşağıda gösterildiği gibi ayarlayın.

![](mvc-music-store-part-5/_static/image1.png)

Ekle düğmesine tıkladığınızda, ASP.NET MVC 3 yapı iskelesi mekanizması iyi bir iş miktarı sizin için halleder olduğunu görürsünüz:

- Yerel bir Entity Framework değişkenle yeni StoreManagerController oluşturur
- Projenin görünümleri klasörüne StoreManager klasör ekler
- Albüm sınıfı için kesin Create.cshtml, Delete.cshtml Details.cshtml Edit.cshtml ve Index.cshtml görünümü ekler

![](mvc-music-store-part-5/_static/image2.png)

Yeni StoreManager denetleyici sınıfı içeren CRUD (oluşturma, okuma, güncelleştirme ve silme) nasıl ile albümü çalışılacağını bildiğinizi denetleyici eylemleri model sınıfı ve veritabanı erişimi için Entity Framework bağlamına bizim kullanın.

## <a name="modifying-a-scaffolded-view"></a>İskele kurulmuş bir görünümünü değiştirme

Bizim için bu kodu oluşturuldu, ancak sadece biz Bu öğretici boyunca yazma gibi standart ASP.NET MVC kodu olduğunu unutmamak önemlidir. Ortak denetleyicisi kod yazma ve kesin türü belirtilmiş görünümler el ile oluşturmak harcadığı zamanı kaydetmek hedeflenen, ancak bu oluşturulan kod içinde nasıl değiştiğini gerekmez hakkında yorumlar doğrudan uyarılarla başında görülen tür değil kodu. Bu, kodunuzu ve değiştirmek için beklenen.

Bu nedenle, hızlı düzenleme StoreManager dizin görünümüne başlayalım (/ Views/StoreManager/Index.cshtml). Bu görünümü, bizim deposundaki albümleri düzenleme ile listeleyen bir tablo görüntüler / Ayrıntılar / Sil bağlantılarını ve albüm genel özellikleri içerir. Bu görünümde çok yararlı olduğu AlbumArtUrl alanın kaldıracağız. İçinde &lt;tablo&gt; bölümü görünümü kod kaldırmak &lt;th&gt; ve &lt;td&gt; AlbumArtUrl başvuruları, aşağıda vurgulanan satırları tarafından belirtildiği gibi çevreleyen öğeleri:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Değiştirilen görünümü kod şu şekilde görünür:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>İlk göz Store Yöneticisi

Artık uygulamayı çalıştırabilir ve/StoreManager/dizinine göz atın. Bu, biz yalnızca, depolama, Ayrıntılar, düzenleme ve silme için bağlantılarla birlikte albümleri listesini gösteren değiştiren Store Yöneticisi dizini görüntüler.

![](mvc-music-store-part-5/_static/image3.png)

Düzenleme bağlantısını tıklatarak tarz ve sanatçının için açılır menüleri kullanarak da dahil olmak üzere albümü, bir düzenleme formunda alanları görüntüler.

![](mvc-music-store-part-5/_static/image4.png)

Altındaki "Geri listesine" bağlantısına tıklayın ve ardından Albüm Ayrıntıları bağlantısına tıklayın. Bu, tek tek albüm için ayrıntılı bilgileri görüntüler.

![](mvc-music-store-part-5/_static/image5.png)

Yeniden liste bağlantısını için Geri'yi tıklatın ve ardından bir Delete bağlantıya tıklayın. Bu, albüm ayrıntılarını gösteren ve biz silmek istediğinizden emin olup olmadığınızı onay iletişim kutusunda görüntüler.

![](mvc-music-store-part-5/_static/image6.png)

Alttaki Sil düğmesine tıklanarak albümü silin ve Silinen albümü gösteren dizin sayfasına dönün.

Store yöneticisiyle tamamlanmadı ancak denetleyici ve Görünüm Kodu başlatmak CRUD işlemleri için çalışma sahibiz.

## <a name="looking-at-the-store-manager-controller-code"></a>Store Yöneticisi denetleyicisi koda baktığınızda

Store Yöneticisi denetleyicisi iyi miktarda kod içerir. Bu üstten alta Bahsedelim. Bazı standart ad alanları bizim modelleri ad alanı başvuru yanı sıra, bir MVC denetleyicisi için denetleyici içerir. Denetleyici MusicStoreEntities her denetleyici eylemleri tarafından kullanılan veri erişimi için özel bir örneği vardır.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Dizin Yöneticisi ve ayrıntıları Eylemler Store

Dizin görünümünün albümleri, biz Store Gözat yöntem üzerinde çalışırken, daha önce bahsettiğim gibi her albüm başvurulan tarz ve sanatçı bilgiler dahil olmak üzere bir listesini alır. Denetleyici, verimli ve bu bilgileri özgün istek için sorgulama için her albüm Tarz hem de sanatçının adını görüntüleyebilmesi dizin görünümünün bağlı nesnelere başvuru kullanılmasıdır.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Tam olarak aynı yazdığımız daha önce - albümü için sorgular Store denetleyicisi Ayrıntılar eylemi StoreManager denetleyicinin ayrıntıları denetleyici eylemi çalışır Find() yöntemi kullanarak ve Kimliğe göre ardından görünümü verir.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Eylem yöntemleri oluşturun

Bunlar form girişi işlediğinden Oluştur eylemi şu ana kadar gördük olanları biraz farklı yöntemlerdir. Bir kullanıcı ilk /StoreManager/oluşturma/ziyaret ettiğinde boş bir form gösterilir. Bu HTML sayfası içerecek bir &lt;form&gt; açılan menüyü ve metin kutusu içeren öğeleri nerede bunlar girebilirsiniz albüm ayrıntıları girin.

Kullanıcı albüm form değerlerine dolgular, bunlar bu değişiklikleri uygulamamız veritabanı içinde kaydetmeyi geri göndermek için "Kaydet" düğmesine basabilirsiniz. Kullanıcı, "Kaydet" düğmesine bastığında &lt;form&gt; dön /StoreManager/oluşturma/URL bir HTTP POST gerçekleştirin ve gönderme &lt;form&gt; değerleri HTTP POST bir parçası olarak.

ASP.NET MVC kolayca bizim StoreManagerController sınıf içindeki – /StoreManager/Create ilk HTTP GET Gözat işlemek için iki ayrı "Oluştur" eylem yöntemleri uygulamak bize etkinleştirerek bu iki URL çağırma senaryolar mantığı oluşturma bölme sağlıyor / URL ve diğer gönderilen değişiklikler, HTTP POST işlemek için.

### <a name="passing-information-to-a-view-using-viewbag"></a>Görünüm paketi kullanarak bir görünüme bilgi geçirme

Biz ViewBag, bu öğreticide daha önce kullandınız, ancak bunu hakkında pek fazla açıklandı henüz. Görünüm paketi bilgileri kesin olarak belirlenmiş model nesnesi kullanmadan görünüme iletilecek sağlıyor. Bu durumda, HTTP GET Düzenle denetleyicisi eylemimiz açılır menüleri kullanarak doldurmak için forma bir listesi türleri ve sanatçıların geçirmek gereken ve bunu yapmanın en kolay yolu ViewBag öğeleri olarak döndürülecek olan.

Görünüm paketini ViewBag.Foo veya ViewBag.YourNameHere bu özelliklerini tanımlamak için kod yazmaya gerek kalmadan yazabilirsiniz, yani dinamik bir nesne olan. Bu durumda, bunlar ayarı Albüm özellikleri GenreId ve ArtistId, formun ile gönderilen açılır değerleri olması denetleyici kodlarının ViewBag.GenreId ve ViewBag.ArtistId kullanır.

Bu açılır değerleri yalnızca bu amaç için oluşturulan SelectList nesnesini kullanarak forma döndürülür. Bunu yapmanız bu kodu kullanarak:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Eylem yöntemi koddan görebileceğiniz gibi bu nesnenin oluşturulacağı üç parametreler kullanılıyor:

- Açılan listeyi görüntüleme öğeleri listesi. Bu yalnızca bir dize değil - biz türleri listesini geçirme unutmayın.
- SelectList için geçirilen sonraki parametre seçtiğiniz bir değerdir. Bu nasıl SelectList önceden listesindeki bir öğeyi seçmek nasıl bilir. Bu, oldukça benzer düzenleme formunda baktığımızda anlamak daha kolay olacaktır.
- Görüntülenecek özelliği son parametredir. Genre.Name özelliği kullanıcıya gösterilecek olduğunu gösteren bu durumda, bu.

Böylece aklınızda daha sonra HTTP GET Oluştur eylemi oldukça basittir - iki SelectLists ViewBag için eklenir ve model nesnesi yok (Bu henüz oluşturulmadıysa bu yana) biçimine geçirilir.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>HTML Yardımcıları oluşturma Görünümü'nde açılan listeleri görüntülemek için

Değerleri açılan görünümüne geçirilir nasıl hakkında konuştuk olduğundan, bu değerlerin gösterilme biçimini görmek için görüntüyü hızlı bir göz atalım. Görünüm kod (/ Views/StoreManager/Create.cshtml), türe açılan görüntülemek için aşağıdaki Çağrının yapıldığı görürsünüz aşağı.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Bu, bir HTML Yardımcısı - genel bir görünüm görev gerçekleştiren bir yardımcı yöntem bilinir. HTML Yardımcıları görünümü kodumuz kısa süren ve okunabilir sağlamadaki çok yararlı olur. Html.DropDownList Yardımcısı, ASP.NET MVC tarafından sağlanır, ancak daha sonra anlatıldığı gibi kodu görüntüle biz uygulamamız yeniden için kendi Yardımcıları oluşturmak mümkündür.

Html.DropDownList çağrı yalnızca iki şey - get görüntülemek için listede ve (varsa) hangi değeri önceden seçilmiş olmalıdır nerede olduğunu gerekiyor. İlk parametre, GenreId, modeli veya ViewBag GenreId adlı değeri aramak için DropDownList söyler. İkinci parametre olarak ilk açılan listeden seçilen gösterilecek değeri belirtmek için kullanılır. Bu form, form oluştur olduğundan, hiçbir değer seçilmiş olması ve String.Empty geçirilir.

### <a name="handling-the-posted-form-values"></a>Gönderilen Form değerleri işleme

Önce Bahsettiğimiz gibi her bir formla ilişkili iki eylem yöntemleri vardır. İlk HTTP GET isteği işler ve form görüntüler. İkinci gönderilen form değerleri içeren HTTP POST isteği işler. Denetleyici eylemini ASP.NET MVC, yalnızca HTTP POST isteklerini yanıtlaması gerektiğini söyleyen bir [HttpPost] özniteliği olduğuna dikkat edin.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Bu eylem, dört sorumluluklara sahiptir:

- 1. Form değerlerini okuma
- 2. Form değerlerini geçirdiğiniz tüm doğrulama kurallarını denetleyin
- 3. Form gönderme geçerliyse, verileri kaydetmek ve güncelleştirilmiş listesini görüntüleme
- 4. Form gönderme geçerli değilse, doğrulama hata formu görüntülemek

#### <a name="reading-form-values-with-model-binding"></a>Okuma Form değerleri Model bağlama

Denetleyici eylemini başlık, fiyat ve AlbumArtUrl değerlerinden GenreId ve ArtistId (aşağı açılan listeden) ve metin değerlerini içeren bir form gönderimi işliyor. Form değerlerini doğrudan erişmek mümkün olsa da, daha iyi bir yaklaşım ASP.NET MVC yerleşik bağlama modeli özelliklerini kullanmaktır. Bir denetleyici eylemi bir model türünü bir parametre olarak alan, ASP.NET MVC form girişleri (hem de Yönlendirme ve sorgu dizesi değerleri) kullanarak bu türde bir nesne doldurun dener. Bunu adları eşleşen özellikler model nesnesi, örn değerlerini bakarak yapar yeni albüm nesnenin GenreId değerini ayarlarken GenreId ada sahip bir girdi arar. Standart yöntemlerini kullanarak ASP.NET MVC görünümleri oluşturduğunuzda, formlar Bu alan adları yalnızca eşleşmemesidir girdi alanı adları, özellik adları kullanarak her zaman işlenir.

#### <a name="validating-the-model"></a>Model doğrulama

Modelin basit ModelState.IsValid çağrısı ile doğrulanır. Henüz herhangi bir doğrulama kuralları bizim albüm sınıfına eklemediniz - biz biraz - İşte şimdi bu denetimi yapmak için çok olmayan gerçekleştirirsiniz. Önemli olan bu ModelStat.IsValid denetimi, doğrulama kuralları ileride yapılacak değişiklikler denetleyici eylem kodu herhangi bir güncelleştirme yapılması gerekmez, böylece biz modelimizi üzerinde yerleştirin doğrulama kurallarına göre uyarlayacak olduğu.

#### <a name="saving-the-submitted-values"></a>Gönderilen değerler kaydediliyor

Form gönderme doğrulama testlerini geçerse, bu değerleri veritabanına kaydetme zamanı geldi. Entity Framework ile yalnızca model albümleri koleksiyona ekleme ve SaveChanges çağırma gerektirir.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework değeri kalıcı hale getirmek için uygun SQL komutlarını oluşturur. Güncelleştirmemiz görebiliriz verileri kaydettikten sonra size geri albümleri listesine yönlendirmelidir, böylece. Bu, görüntülenen istiyoruz denetleyici eylemi adıyla RedirectToAction döndürerek gerçekleştirilir. Bu durumda, dizin yöntemidir.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Geçersiz form gönderilerini görselleştirip doğrulama hataları ile görüntüleme

Geçersiz bir form girişi, söz konusu olduğunda açılır değerleri Görünüm (olduğu gibi HTTP GET) paketi eklenir ve bağlanmış modeli değerleri görünüme dönmek için görünen geçirilir. Doğrulama hataları, kullanarak otomatik olarak görüntülenir @Html.ValidationMessageFor HTML Yardımcısı.

#### <a name="testing-the-create-form"></a>Test oluşturma formu

Bu, çalışma /StoreManager/oluşturma / - göz atın ve uygulamayı test etmek için bu, StoreController oluşturma HTTP GET yöntemi tarafından döndürülen boş form gösterir.

Bazı değerleri doldurun ve formunun Oluştur düğmesine tıklayın.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Düzenlemelerini işleme

Düzenleme eylem çifti (HTTP GET ve HTTP POST) yalnızca inceledik oluşturma eylem yöntemlerine oldukça benzerdir. İçinde bir rota üzerinden geçen mevcut albümü, Düzen HTTP-albümü "id" parametresini temel alan metodunun GET ile çalışma düzenleme senaryo içerir. Daha önce ayrıntı denetleyici eylemi inceledik AlbumId tarafından albüm almak için bu kodu aynı olur. Gibi oluşturma / HTTP GET yöntemi, değerleri açılan ViewBag döndürülür. Bu bize albüm bizim model nesnesi (hangi albüm sınıfı için kesin) görüntülemek için döndürülecek ek veriler (örneğin türleri listesi) görünüm paketini geçirirken sağlar.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

HTTP POST Düzenle eylemi için HTTP POST oluşturma eylemini çok benzer. Tek fark DB'ye yeni albümü eklemek yerine olmasıdır. Albümleri koleksiyon db kullanarak biz albümü'nün geçerli örneğini bulma. Entry(Album) ve durumuna Modified olarak ayarlama. Bu, yeni bir tane oluşturmak yerine var olan bir albümü değiştiriyorsunuz Entity Framework bildirir.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Bu uygulamayı çalıştırma ve tarama/StoreManger/sonra albüm düzenleme bağlantısını tıklatarak sınayabiliriz.

![](mvc-music-store-part-5/_static/image9.png)

Bu düzen HTTP GET yöntemi tarafından gösterilen düzenleme formu görüntüler. Bazı değerleri doldurun ve Kaydet düğmesine tıklayın.

![](mvc-music-store-part-5/_static/image10.png)

Bu formu gönderir, değerleri kaydeder ve bize güncelleştirilmiş değerleri olduğunu gösteren albüm listesine döndürür.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>İşleme silme

Silme, düzenleme ve oluşturma, form gönderme işlemek için başka bir denetleyici eylemi ile onay formu görüntülemek için bir denetleyici eylemi olarak aynı deseni izler.

HTTP GET Sil denetleyici eylemi tam olarak önceki Store Manager ayrıntıları denetleyicisi eylemimiz ile aynıdır.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Delete Görünümü içerik şablonu kullanarak bir albüm türü kesin belirlenmiş bir form görüntüleriz.

![](mvc-music-store-part-5/_static/image12.png)

Delete şablon modeli için tüm alanları gösterir, ancak biz bir bit alt basitleştirin. /Views/StoreManager/Delete.cshtml görünümü kod şu şekilde değiştirin.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Bu basitleştirilmiş bir silme onayı görüntüler.

![](mvc-music-store-part-5/_static/image13.png)

Sil düğmesine tıklanarak DeleteConfirmed eylemi yürüten sunucuya geri, yayımlanacak formun neden olur.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

HTTP POST Sil denetleyicisi Eylemimiz aşağıdaki işlemleri yapar:

- 1. Albüm kimliği tarafından yükler
- 2. Albüm siler ve değişiklikleri kaydedin
- 3. Albüm listeden kaldırıldığını gösteren bir dizini yeniden yönlendirir

Bunu test etmek için uygulamayı çalıştırın ve /StoreManager için göz atın. Albüm listeden seçin ve Sil bağlantısını tıklayın.

![](mvc-music-store-part-5/_static/image14.png)

Bu, bizim silme onay ekranında görüntüler.

![](mvc-music-store-part-5/_static/image15.png)

Sil düğmesine tıklanarak albümü kaldırır ve bize albümü silindiğini gösterir Store Yöneticisi dizin sayfasına döndürür.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Metin kesmek için özel bir HTML Yardımcısını kullanma

Bir olası sorun Store Yöneticisi dizini sayfamızı yapılandırdığımıza göre. Bizim albüm başlığı ve sanatçı adı özelliklerinin her ikisi de bunlar bizim tablo biçimlendirmeyi devre dışı durum oluşturabilir yeterince uzun olabilir. Bize bu ve diğer özellikler bizim görünümlerde bir kolayca kesecek şekilde izin vermek için özel bir HTML Yardımcısı oluşturacağız.

![](mvc-music-store-part-5/_static/image17.png)

Razor'ın @helper söz dizimi vermiştir, oldukça kolay kendi kullanımı için yardımcı işlevleri, görünümler oluşturun. /Views/StoreManager/Index.cshtml görünümü açın ve hemen sonra aşağıdaki kodu ekleyin @model satır.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Bu yardımcı yöntemi, bir dize ve izin vermek için bir maksimum uzunluğunu alır. Sağlanan metnin belirtilen uzunluktan daha kısaysa yardımcı olarak çıkarır-olduğu. Uzunsa, metni keser ve geri kalanı için "..." işler.

Bizim Truncate Yardımcısı albüm başlığı ve sanatçı adı özellikleri 25 karakterden az olduğundan emin olmak için şimdi kullanabiliriz. Yeni sunduğumuz Truncate Yardımcısı kullanarak tam görünümü kodu altında görünür.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Şimdi biz /StoreManager/ URL göz attığınızda, Albümler ve başlıkları müşterilerimizin en çok uzunlukları tutulur.

![](mvc-music-store-part-5/_static/image18.png)

Not: Bu oluşturma ve tek bir görünümde bir Yardımcısını kullanarak basit bir durumda gösterir. Siteniz kullanabileceğiniz Yardımcıları oluşturma hakkında daha fazla bilgi için Bilgisayarım blog gönderisi bakın: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-4.md)
> [İleri](mvc-music-store-part-6.md)
