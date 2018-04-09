---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: ASP.NET Web sayfaları sunarak - veritabanı verileri güncelleştirme | Microsoft Docs
author: tfitzmac
description: Bu öğreticide, ASP.NET Web sayfaları (Razor) kullandığınızda (değiştirin) var olan bir veritabanını girişini güncelleştirmek nasıl gösterir. Seri tamamladığınızdan varsayar th...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: e889cd27e2267a08f7b6ea708c92e35edbdd7a1a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>ASP.NET Web sayfaları sunarak - veritabanı verileri güncelleştirme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğreticide, ASP.NET Web sayfaları (Razor) kullandığınızda (değiştirin) var olan bir veritabanını girişini güncelleştirmek nasıl gösterir. Seri aracılığıyla tamamladığınızdan varsayar [veri girişi tarafından Forms kullanarak ASP.NET Web sayfalarını kullanma](entering-data.md).
> 
> Öğrenecekleriniz:
> 
> - Tek bir kayıtta seçmek nasıl `WebGrid` Yardımcısı.
> - Tek kayıtlı bir veritabanından okunmaya yapma.
> - Veritabanı kaydı değerlerinden bir formla dağıtılacak yapma.
> - Bir veritabanında varolan bir kaydı güncelleştirmeye nasıl.
> - Görüntülemeden sayfasındaki bilgileri depolamak nasıl.
> - Gizli bir alan bilgilerini depolamak için nasıl kullanılacağını.
>   
> 
> Özellikler/teknolojilerini ele alınan:
> 
> - `WebGrid` Yardımcısı.
> - SQL `Update` komutu.
> - `Database.Execute` Yöntemi.
> - Gizli alanlar (`<input type="hidden">`).


## <a name="what-youll-build"></a>Ne oluşturacağınız

Önceki öğreticide, bir veritabanına bir kayıt eklemek öğrendiniz. Burada, düzenlemek için bir kayıt görüntülemek nasıl öğreneceksiniz. İçinde *filmler* update sayfasında, `WebGrid` onun görüntüler için yardımcı bir **Düzenle** her film yanındaki bağlantı:

![WebGrid görüntülemek her film için 'Düzenle' bağlantı](updating-data/_static/image1.png)

Tıkladığınızda **Düzenle** bağlantı sürdüğünü, farklı bir sayfaya film bilgileri zaten bir formda olduğu:

![Düzenlenecek film gösteren film sayfasını Düzenle](updating-data/_static/image2.png)

Herhangi bir değeri değiştirebilirsiniz. Değişiklikleri gönderdiğinizde, sayfa kodunda veritabanını güncelleştirir ve film listenin geri alır.

Bu işlemin parçası çalışır neredeyse tam olarak gibi *AddMovie.cshtml* oluşturduğunuz önceki öğreticide, bu öğreticinin daha tanıdık gelecektir şekilde sayfası.

Tek bir filmi düzenlemek için bir yol uygulamak birkaç yolu vardır. Uygulama kolay ve kolay anlaşılır olduğundan gösterilen yaklaşım seçilmiştir.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Film listenin bir düzenleme bağlantısı ekleme

Başlamak için güncelleştireceğim *filmler* de listeleme her film içeren sayfasında bir **Düzenle** bağlantı.

Açık *Movies.cshtml* dosya.

Sayfasının gövdesinde değiştirmek `WebGrid` bir sütunu ekleyerek biçimlendirme. Değiştirilen biçimlendirme aşağıdaki gibidir:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Yeni bir sütun bu bilgisayardır:

[!code-html[Main](updating-data/samples/sample2.html)]

Bu sütun bir bağlantıyı göstermek için noktasıdır (`<a>` öğesi) "Düzenle" metnini söyler. Sonra ki olduğundan sayfa çalıştığında, aşağıdakine benzer bir bağlantı oluşturmak için ile `id` her film farklı değeri:

[!code-css[Main](updating-data/samples/sample3.css)]

Bu bağlantıyı adlı bir sayfaya çağıracağı *EditMovie*, ve sorgu dizesi geçer `?id=7` bu sayfaya.

Yeni bir sütun için söz dizimi biraz karmaşık görünebilir, ancak yalnızca birkaç öğelerini birlikte koyar olmasıdır. Her öğesi basittir. Üzerinde yoğunlaşabilirsiniz varsa yalnızca `<a>` öğesi, bu biçimlendirme bakın:

[!code-html[Main](updating-data/samples/sample4.html)]

Kılavuz nasıl çalıştığı hakkında bazı arka plan: kılavuz satırları, her veritabanı kaydı için bir tane görüntüler ve veritabanı kaydında her bir alan sütunlarını görüntüler. Her kılavuz satır oluşturulmuş olsa da, `item` nesne satır veritabanı kaydını (öğe) içerir. Bu düzenleme kod ilgili satır veri almak için bir yol sağlar. Burada gördüğünüz olmasıdır: ifade `item.ID` kimliği değeri geçerli veritabanı öğesinin almaktır. Kullanarak veritabanı değerleri (başlık, tarz veya yıl) aynı şekilde alabilir `item.Title`, `item.Genre`, veya `item.Year`.

İfade `"~/EditMovie?id=@item.ID` hedef URL kodlanmış parçası birleştirir (`~/EditMovie?id=`) dinamik olarak türetilen bu kimliğiyle (Gördüğünüz `~` önceki öğreticideki; işleci, geçerli Web sitesi kök temsil eden bir ASP.NET operatördür.)

Bu bölümü biçimlendirme sütununda yalnızca aşağıdaki biçimlendirme gibi çalışma zamanında oluşturduğundan emin sonucudur:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Doğal olarak, gerçek değerini `id` her satır için farklı olacaktır.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Kılavuz sütunu için özel bir görüntü oluşturma

Şimdi kılavuz sütunu yedekleyin. Üç sütun başlangıçta görüntülenen kılavuz yalnızca veri değerlerini (başlık, tarzını ve yıl) içeriyor. Bu görüntü veritabanı sütununun adını geçirerek belirtilen &mdash; Örneğin, `grid.Column("Title")`.

Bu yeni **Düzenle** bağlantı sütun farklıdır. Bir sütun adı belirtmek yerine, geçirme bir `format` parametresi. Bu parametre, biçimlendirme tanımlamanıza olanak sağlar, `WebGrid` Yardımcısı ile birlikte sokacak `item` kalın veya yeşil olarak sütun verileri görüntüleyebilir veya ne olursa olsun, biçiminde istediğiniz değer. Örneğin, kalın görünmesi başlık istediyseniz, bu örnek gibi bir sütun oluşturabilirsiniz:

[!code-html[Main](updating-data/samples/sample6.html)]

(Çeşitli `@` içinde gördüğünüz karakterleri `format` özelliği biçimlendirme ve bir kod değeri arasında geçiş işaretlemek.)

İlgili öğrendikten sonra `format` özelliği, onu anlamak daha kolay nasıl yeni **Düzenle** bağlantı sütununu birlikte yerleştir:

[!code-html[Main](updating-data/samples/sample7.html)]

Sütun oluşur *yalnızca* bağlantıyı işleyen biçimlerini, bazı bilgiler (ID), ayıklanır satır için veritabanı kaydından.

> [!TIP]
> 
> **Adlandırılmış parametreler ve bir yöntem için konumsal Parametreler**
> 
> Birçok kez yöntemi çağrılır ve kendisine geçirilen parametreleri, virgülle ayrılmış parametre değerleri yalnızca listelediğiniz. Aşağıda birkaç örnek verilmiştir:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Biz öncelikle bu kodu gördüğünüz, ancak her durumda, belirli bir sırada yöntemlerine parametreleri geçirme sorunu Bahsediyor kaydetmedi &mdash; yani, bu yöntem parametreleri tanımlanır sırası. İçin `db.Execute` ve `Validation.RequireFields`, geçirdiğiniz değerlerin sırasını karma, sayfa çalıştığında bir hata iletisi veya en azından bazı garip sonuçları elde etmesi. Açıkçası, parametrelerde geçirmek için sipariş bilmeniz gerekir. (Webmatrix'te, IntelliSense, Şekil adı, türü ve parametreler sırasını öğrenmenize yardımcı olabilir.)
> 
> Geçirme değerleri sırada alternatif olarak, kullandığınız *adlandırılmış parametreleri*. (Parametreleri sırada geçirme bilinen kullanarak olarak *konumsal parametreler*.) Adlandırılmış parametreler için açıkça parametresinin adı değerini geçirilirken içerir. Bu öğreticiler adlandırılmış parametreleri birkaç kez zaten kullandığınız. Örneğin:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> and
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Özellikle bir yöntem birçok parametreleri yönlendirdiğinde adlandırılmış parametreleri durumlarda birkaç için kullanışlıdır. Yalnızca bir veya iki parametreleri geçirmek istiyorsanız ancak geçirmek istediğiniz değerleri parametre listesindeki ilk konumlar arasında olmayan biridir. Çoğu sizin için anlamlı sırada parametreleri ileterek kodunuzu daha okunabilir hale getirmek istediğinizde başka bir durumdur.
> 
> Belli ki, adlandırılmış parametreleri kullanmak için parametreleri adlarını bilmeniz gerekir. WebMatrix IntelliSense yapabilirsiniz *Göster* adları, ancak şu anda bunları sizin için doldurduğunuz olamaz.


## <a name="creating-the-edit-page"></a>Düzen sayfası oluşturma

Oluşturabileceğiniz artık *EditMovie* sayfası. Kullanıcılar'ı tıklattığınızda **Düzenle** bağlantı, bunlar şunun bu sayfada.

Adlı bir sayfa oluşturma *EditMovie.cshtml* ve dosyasında, aşağıdaki biçimlendirme nedir değiştirin:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Bu biçimlendirme ve kodun, sahip benzer *AddMovie* sayfası. Gönder düğmesine metninde küçük bir fark yoktur. İle *AddMovie* sayfasında, var olan bir `Html.ValidationSummary` varsa, doğrulama hataları görüntüler çağrısı. Biz bırakarak çağrıları çıkışı bu kez `Validation.Message`, bu yana hataları doğrulama özeti görüntülenir. Önceki öğreticide belirtildiği gibi çeşitli bileşimlerde doğrulama özeti ve tek tek hata iletileri kullanabilirsiniz.

Yeniden dikkat `method` özniteliği `<form>` ayarlanır `post`. İle *AddMovie.cshtml* sayfası, bu sayfayı yapar değişiklikleri veritabanına. Bu nedenle, bu formu gerçekleştirmesi gereken bir `POST` işlemi. (Arasındaki fark hakkında daha fazla bilgi için `GET` ve `POST` işlemleri görmek [GET, POST ve HTTP fiili güvenliği](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) kenar HTML formları öğreticideki.)

Bir önceki öğreticide gördüğünüz gibi `value` metin kutuları özniteliklerini ayarlanır Razor kodu ile bunları önceden yüklemek için. Bu süre, yine de değişkenler gibi kullandığınız `title` ve `genre` yerine bu görev için `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Olarak daha önce bu biçimlendirme metin kutusu değerleri film değerlerle dağıtılacak. Neden olduğunu kullanmak yerine bu kez değişkenleri kullanmak için kullanışlı bir dakika içinde görürsünüz `Request` nesnesi.

Ayrıca bir `<input type="hidden">` bu sayfada öğe. Bu öğe görünür sayfada yapmadan film Kimliğini depolar. Kimliği başlangıçta sayfa tarafından bir sorgu dizesi değeri kullanılarak geçirilir (`?id=7` veya URL'deki benzer). Gizli bir alana kimliği değeri koyarak kullanılabilir olduğundan emin yapabileceğiniz zaman formun gönderildi, sayfa ile çağrıldı özgün URL artık erişiminiz olsa bile.

Farklı *AddMovie* sayfası, kodunu *EditMovie* sayfası, iki farklı işlevler içerir. Sayfa ilk kez görüntülendiğinde olan ilk işlevi (ve *yalnızca* sonra), kod Sorgu dizesinden film Kimliğini alır. Kod sonra kullandığı kimliği veritabanı dışında karşılık gelen film okuyun ve görüntülemek için (Bu metin kutularına dağıtılacak).

Kullanıcı tıklattığında olan ikinci işlev **gönderme değişiklikleri** düğmesini koduna sahip metin kutuları değerini okumak ve onları doğrulamak. Kod yeni değerlerle veritabanı öğesini güncelleştirmek de vardır. İçinde anlatıldığı gibi bu teknik bir kayıt eklemeye benzer *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Tek bir filmi okumak için kod ekleme

İlk işlevi gerçekleştirmek için bu kodu sayfanın üst kısmına ekleyin:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Bu kod çoğunu başlayan bir blok içinde olduğu `if(!IsPost)`. `!` İşleci anlamına gelir "değil" ifade anlamına gelir şekilde *bu isteği bir posta gönderimi değilse*, bildiren dolaylı bir yolu olduğu *bu isteği bu sayfayı çalıştırmak ilk kez olup olmadığını*. Daha önce belirtildiği gibi bu kodun çalışması gerektiğini *yalnızca* sayfa ilk çalıştığında. Kodda içine alamadık, `if(!IsPost)`, sayfa, olup ilk kez çağrılması her zaman çalışması veya yanıt düğmesine tıklayın.

Kod içeren bildirim bir `else` engelleme bu süre. Ne zaman gösterdiğimizi denirse gibi `if` blokları, bazen test koşul doğru değilse alternatif kodu çalıştırmak istediğiniz. Burada bir durumdur. (Diğer bir deyişle, sayfaya geçirilen kimliği Tamam ise) koşul geçerse, bir satır veritabanından okuyamadı. Ancak, koşul geçmiyor, `else` bloğu çalıştırır ve bu kod hata iletisine ayarlar.

## <a name="validating-a-value-passed-to-the-page"></a>Sayfaya geçirilen bir değer doğrulanıyor

Kod kullanan `Request.QueryString["id"]` sayfasına geçirilen kimliği alınamadı. Kod bir değer kimliği için gerçekten geçildi emin olur Hiçbir değer geçirildi, kodu bir doğrulama hatası ayarlar.

Bu kod bilgileri doğrulamak için farklı bir şekilde gösterir. Önceki öğreticide çalıştığınız `Validation` Yardımcısı. Doğrulanacak alanları kayıtlı ve ASP.NET otomatik olarak doğrulama vermedi ve hataları kullanarak görüntülenen `Html.ValidationMessage` ve `Html.ValidationSummary`. Bu durumda, ancak siz değil gerçekten kullanıcı girişini doğrulama. Bunun yerine, başka bir yerden sayfaya geçirilmedi bir değer doğrulama. `Validation` Yardımcı değil yapın, sizin için.

Bu nedenle, değer kendiniz ile test ederek denetimi `if(!Request.QueryString["ID"].IsEmpty()`). Bir sorun varsa, kullanarak hata görüntüleyebilirsiniz `Html.ValidationSummary`ile yaptığınız gibi `Validation` Yardımcısı. Bunu yapmak için arama `Validation.AddFormError` ve görüntülenecek bir ileti geçirin. `Validation.AddFormError` zaten aşina doğrulama sistemi oturum tie özel iletileri tanımlamanıza izin verir yerleşik bir yöntemdir. (Daha sonra Bu öğreticide Biz bu doğrulama işlemi biraz daha sağlam hale hakkında konuşun.)

Film kimliği olduğundan emin olduktan sonra kodu için yalnızca bir tek veritabanı öğesi arayan veritabanı okur. (Büyük olasılıkla genel düzeni veritabanı işlemleri için fark etmiş: veritabanını açmak, bir SQL deyimi tanımlamak ve deyimini çalıştırın.) Bu süre, SQL `Select` deyimi içeren `WHERE ID = @0`. Yalnızca bir kayıt kimliği benzersiz olduğundan döndürülebilir.

Sorgu kullanılarak gerçekleştirilen `db.QuerySingle` (değil `db.Query`film listesi için kullanılan gibi), ve sonucu kodu yerleştirir `row` değişkeni. Adı `row` rastgeledir; değişkenleri herhangi bir şey istediğiniz gibi adlandırabilirsiniz. Bu değerleri metin kutularına görüntülenebilir böylece en üstünde başlatılmış değişkenleri sonra film ayrıntıları ile doldurulur.

## <a name="testing-the-edit-page-so-far"></a>Düzen Sayfası (kadarki) test etme

Sayfanızın test etmek isterseniz, çalıştırmak *filmler* şimdi sayfasında ve tıklayın bir **Düzenle** herhangi film yanındaki bağlantı. Göreceğiniz *EditMovie* Ayrıntıları sayfası doldurulmuş seçtiğiniz film için:

![Düzenlenecek film gösteren film sayfasını Düzenle](updating-data/_static/image3.png)

Sayfanın URL'sini şöyle içerdiğine dikkat edin `?id=10` (veya başka bir sayıyı). Şu ana kadar test ettiğiniz **Düzenle** içinde bağlantılar *film* çalışma sayfanız kimliği Sorgu dizesinden okuyor ve veritabanı tek film kaydı almak için sorgu çalışma sayfasında.

Film bilgileri değiştirebilirsiniz, ancak tıklattığınızda hiçbir şey olmaz **gönderme değişiklikleri**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Film kullanıcının değişikliklerle güncelleştirmek için kod ekleme

İçinde *EditMovie.cshtml* dosya, ikinci işlevi (değişiklikleri kaydetme) uygulamak için aşağıdaki kodu kapanış ayracı yalnızca eklemek `@` bloğu. (Kodu tam olarak nereye yerleştirileceğini emin değilseniz, bakabilir [Düzenle film sayfa için tam kod](#Complete_Page_Listing_for_EditMovie) Bu öğreticinin sonunda görüntülenir.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Yeniden, bu biçimlendirme ve kodun benzer kodunda *AddMovie*. Kod bir `if(IsPost)` yalnızca kullanıcı tıkladığında bu kod çalıştığından bloğunu **gönderme değişiklikleri** düğmesini &mdash; diğer bir deyişle, ne zaman (ve yalnızca) form gönderilen. Bu durumda, bir test gibi kullanmadığınızı `if(IsPost && Validation.IsValid())`— diğer bir deyişle, her iki testleri kullanarak birleştirme değil ve. Bu sayfada, öncelikle bir form gönderme olup olmadığını belirlemeniz (`if(IsPost)`) ve yalnızca doğrulama alanları kaydedin. Doğrulama sonuçlarını sınayabilirsiniz sonra (`if(Validation.IsValid()`). Akış biraz daha farklıdır *AddMovie.cshtml* sayfa ancak etkisi aynıdır.

Metin kutuları değerlerini kullanarak alma `Request.Form["title"]` ve diğer benzer bir kod `<input>` öğeleri. Bu süre, film kimliği kodu dışında gizli alan alır dikkat edin (`<input type="hidden">`). Sayfa ilk kez çalıştırdığınızda, kod sorgu dizesi dışında kimliği alındı. Sorgu dizesi şekilde o zamandan bu yana değiştirildiği durumunda ilk olarak görüntülenen, film Kimliğini aldığınızdan emin olmak için gizli alanından değeri alır.

Arasındaki en önemli fark *AddMovie* kodu ve bu kodu olduğundan bu kodda, SQL kullanmanızı `Update` deyimi yerine `Insert Into` deyimi. Aşağıdaki örnek SQL söz dizimi görülmektedir `Update` deyimi:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Herhangi bir sırada hiçbir sütun belirtebilirsiniz ve her sütun sırasında güncelleştirmek mutlaka yoksa bir `Update` işlemi. (Kimliği kendisini yürürlükte kaydettiğiniz kaydı yeni bir kayıt ve verilmeyen için olduğundan güncelleştirilemiyor bir `Update` işlemi.)

> [!NOTE] 
> 
> **Önemli** `Where` , veritabanı hangi veritabanı nasıl bilir olduğundan yan tümcesi Kimliğine sahip çok önemli güncelleştirmek istediğiniz kaydı. Devre dışı bırakılırsa `Where` yan tümcesi, veritabanı güncelleştirilmesi *her* veritabanındaki kayıt. Çoğu durumda, bir olağanüstü durumda olacaktır.


Kodda yer tutucuları kullanarak SQL deyimine güncelleştirmek için değerleri geçirilir. Ne biz önce belirttiniz yinelenecek: güvenlik nedenleriyle, *yalnızca* yer tutucuları bir SQL deyimi için değerleri geçirmek için kullanın.

Kod kullanan sonra `db.Execute` çalıştırmak için `Update` deyimi, burada görebilirsiniz değişiklikleri geri listeleme sayfasına yönlendirir.

> [!TIP] 
> 
> **Farklı SQL deyimlerini, farklı yöntemler**
> 
> Farklı SQL deyimlerini çalıştırmak için biraz farklı yöntemler kullanın fark etmiş olabilirsiniz. Çalıştırmak için bir `Select` sorgu bu büyük olasılıkla döndürür birden çok kayıt, kullandığınız `Query` yöntemi. Çalıştırmak için bir `Select` bildiğiniz sorgu yalnızca bir veritabanı öğe döndürür, kullandığınız `QuerySingle` yöntemi. Değişiklikler yapmak, ancak veritabanı öğeleri dönüş yok komutları çalıştırmak için kullandığınız `Execute` yöntemi.
> 
> Bunların her biri farklı sonuçlar döndürür çünkü zaten arasındaki farkı içinde anlatıldığı gibi farklı yöntemler sahip olmanız `Query` ve `QuerySingle`. ( `Execute` Yöntemi gerçekte değeri döndürür de &mdash; yani, komut tarafından etkilenen veritabanı satır sayısını &mdash; ancak, o ana kadarki yoksayılıyor.)
> 
> Elbette, `Query` yöntemi yalnızca bir veritabanı satır döndürebilir. Ancak, ASP.NET sonuçlarını her zaman davranır `Query` yöntemi bir koleksiyon olarak. Yöntem yalnızca tek bir satır döndürüyorsa bile koleksiyondan tek satır ayıklamanız gerekir. Bu nedenle, durumlarda Burada, *bilmeniz* yalnızca bir satır geri alırsınız, kullanmak daha kullanışlı bir bit `QuerySingle`.
> 
> Belirli türlerdeki veritabanı işlemleri gerçekleştirmek birkaç yöntem vardır. Veritabanı yöntemleri listesini bulabilirsiniz [ASP.NET Web sayfaları API hızlı başvuru](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Doğrulama Kimliği daha fazla bilgi için sağlam hale getirme

Bu film veritabanından alma çalışabilmeniz için sayfa çalıştığında, ilk kez, film kimliği Sorgu dizesinden alırsınız. Aslında, olduğunu bu kodu kullanarak yaptığınız arayın, gitmek için bir değer emin olun:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Bu kod bir kullanıcı için alırsa emin olmak için kullanılan *EditMovies* bir filmi ilk seçmeden sayfa *filmler* sayfası, sayfayı kolay hata iletisi görüntüler. (Aksi halde, kullanıcılar muhtemelen yalnızca bunları işlemlerini birbirine karıştırmaktadır bir hata görürsünüz.)

Ancak, bu doğrulama çok sağlam değil. Sayfa aynı zamanda bu hatalarla çağrılan:

- Kimliği bir sayı değil. Örneğin, sayfa gibi bir URL ile çağırılabilir `http://localhost:nnnnn/EditMovie?id=abc`.
- Kimlik numarasıdır, ancak mevcut olmayan bir filmi başvurduğu (örneğin, `http://localhost:nnnnn/EditMovie?id=100934`).

Çalıştırma bu URL'lerden neden hataları görmek merak ediyorsanız *filmler* sayfası. Düzenlemek için bir filmi seçin ve ardından URL'sini değiştirme *EditMovie* kimliği veya var olmayan film Kimliğini bir alfabetik içeren bir URL için sayfa.

Bu nedenle neler? İlk düzeltmeyi yalnızca bir kimlik bu sayfaya geçirilir, ancak kimliği tamsayı olduğunu emin olmaktır. Kodunu değiştirmek `!IsPost` test bu örneğe:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

İkinci bir koşul eklediğiniz `IsEmpty` ile bağlantılı bir test `&&` (mantıksal AND):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Unutmayın [ASP.NET Web sayfaları programlama giriş](../introducing-razor-syntax-c.md) yöntemleri gibi öğretici `AsBool` bir `AsInt` bir karakter dizesi başka bazı veri türüne dönüştürün. `IsInt` Yöntemi (ve diğer `IsBool` ve `IsDateTime`) benzer. Ancak, bunlar yalnızca sınama olup olmadığını, *için* dize dönüştürme işlemini gerçekleştirmeden Dönüştür. Bu nedenle Burada, aslında belirten *sorgu dizesi değeri tamsayıya dönüştürülebilir ise...* .

Olası bir sorunu mevcut olmayan bir film arıyor. Bir filmi alma kodu bu kod gibi görünür:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Geçirirseniz bir `movieId` değeri `QuerySingle` gerçek bir filmi karşılık gelmiyor yöntemi, hiçbir şey döndürülür ve deyimleri izleyin (örneğin, `title=row.Title`) neden hatalar.

Yeniden kolay bir düzeltme yoktur. Varsa `db.QuerySingle` yöntemi, hiçbir sonuç döndürür `row` değişkeni boş olacaktır. Böylece kontrol olup olmadığını `row` değerleri elde denemeden önce değişkeni null. Aşağıdaki kod ekler bir `if` değerlerini kullanıma alın deyimleri geçici blok `row` nesnesi:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Bu iki ek doğrulama testleri sayfası daha madde işareti kanıt haline gelir. İçin tam kod `!IsPost` şube şimdi aşağıdaki gibi görünür:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Biz kez daha bu görev için iyi bir kullanım olduğuna dikkat edin bir `else` bloğu. Testleri geçirmezseniz `else` blokları hata iletileri ayarlayın.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Film sayfasına geri dönmek için bağlantı ekleme

Bağlantı eklemek için son ve yararlı bir ayrıntısı şöyle geri *filmler* sayfası. Kullanıcıların olayları sıradan akışında başlangıç *filmler* sayfasında ve tıklayın bir **Düzenle** bağlantı. Getirir bunları *EditMovie* sayfasında, burada film düzenleme yapıp düğmesini tıklatın. Kod değişikliği işledikten sonra geri yönlendiren *filmler* sayfası.

Ancak:

- Kullanıcı değişikliği değil karar verebilirsiniz.
- Kullanıcı bu sayfaya ilk tıklamadan getirildiğini bir **Düzenle** bağlamak *filmler* sayfası.

Her iki durumda da, bunları ana listesine geri dönmek daha kolay hale getirmek istediğiniz. Kolay bir düzeltme durumdadır &mdash; kapattıktan sonra aşağıdaki biçimlendirme eklemek `</form>` işaretleme etiketinde:

[!code-html[Main](updating-data/samples/sample19.html)]

Bu biçimlendirme için aynı sözdizimini kullanan bir `<a>` başka bir yerde gördünüz öğesi. URL içeren `~` "Web sitesinin kök." anlamına gelir

## <a name="testing-the-movie-update-process"></a>Film güncelleştirme işlemini test etme

Artık test edebilirsiniz. Çalıştırma *filmler* sayfasında ve tıklayın **Düzenle** film yanındaki. Zaman *EditMovie* sayfası görüntülenirse, tıklatın ve film değişiklik **gönderme değişiklikleri**. Film listesi göründüğünde, değişikliklerinizi gösterildiğinden emin olun.

Doğrulama çalıştığından emin olmak için tıklatın **Düzenle** başka bir filmi için. Ulaştığınızda *EditMovie* sayfasında, Temizle **Tarz** alan (veya **yıl** alan veya her ikisi de) ve değişikliklerinizi göndermeyi deneyin. Beklediğiniz gibi bir hata görürsünüz:

![Doğrulama hataları gösteren film sayfasını Düzenle](updating-data/_static/image4.png)

Tıklatın **film dökümüne dönün** yaptığınız değişiklikleri iptal ve dönmek için bağlantı *filmler* sayfası.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide film kaydını silmek nasıl görürsünüz.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Tam listesi için (düzenleme bağlantıları ile güncelleştirilmiş) film sayfası

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Sayfa listesi düzenleme film sayfa için tamamlayın

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Razor sözdizimini kullanarak ASP.NET Web programlamaya giriş](../../getting-started/introducing-razor-syntax-c.md)
- [SQL güncelleştirme deyimini](http://www.w3schools.com/sql/sql_update.asp) W3Schools sitesinde

> [!div class="step-by-step"]
> [Önceki](entering-data.md)
> [sonraki](deleting-data.md)
