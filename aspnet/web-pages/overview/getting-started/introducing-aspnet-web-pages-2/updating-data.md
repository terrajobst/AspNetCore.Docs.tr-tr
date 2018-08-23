---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Veritabanı verilerini güncelleştirme - ASP.NET Web sayfaları ile tanışın | Microsoft Docs
author: tfitzmac
description: Bu öğreticide, ASP.NET Web sayfaları (Razor) kullandığınızda (değiştirin) var olan bir veritabanını girişini güncelleştirmek gösterilmektedir. Bu seriyi bitirdiğinizi th...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 9d0da2eed9964e56a01f3c811d6a14c9d1735e10
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757065"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>ASP.NET Web sayfalarına giriş - veritabanı verilerini güncelleştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğreticide, ASP.NET Web sayfaları (Razor) kullandığınızda (değiştirin) var olan bir veritabanını girişini güncelleştirmek gösterilmektedir. Bu seriyi aracılığıyla bitirdiğinizi [veri girişi tarafından Forms kullanarak ASP.NET Web sayfalarını kullanma](entering-data.md).
> 
> Öğrenecekleriniz:
> 
> - Tek bir kaydı seçmek nasıl `WebGrid` Yardımcısı.
> - Tek kayıtlı bir veritabanından okumak nasıl.
> - Veritabanı kaydı değerlerle form dağıtılacak yapma.
> - Bir veritabanında varolan bir kaydı güncelleştirme yapma.
> - Bilgi görüntülemeden sayfasında depolamayı öğrenin.
> - Gizli bir alan bilgilerini depolamak için nasıl kullanılacağını.
>   
> 
> Ele alınan özelliklerin/teknolojiler:
> 
> - `WebGrid` Yardımcısı.
> - SQL `Update` komutu.
> - `Database.Execute` Yöntemi.
> - Gizli alanları (`<input type="hidden">`).


## <a name="what-youll-build"></a>Ne oluşturacaksınız

Önceki öğreticide, bir veritabanı için bir kayıt eklemek öğrendiniz. Burada, bir kaydı düzenlemek için görüntülemek üzere nasıl öğreneceksiniz. İçinde *filmler* sayfasında, güncelleştirme `WebGrid` yardımcı olan görünmesi bir **Düzenle** her filmin yanındaki bağlantısını:

![WebGrid görüntülemek için her filmin 'Düzenle' bağlantısının dahil olmak üzere](updating-data/_static/image1.png)

Tıkladığınızda **Düzenle** bağlantı sürer, başka bir sayfaya film bilgileri zaten bir biçimde olduğu:

![Düzenlenecek film gösteren film sayfayı Düzenle](updating-data/_static/image2.png)

Değerlerden herhangi birini değiştirebilirsiniz. Değişiklikleri gönderme, kod sayfasında veritabanını güncelleştirir ve film listeye geri alır.

İşlemin bu kısmında çalışır gibi neredeyse *AddMovie.cshtml* Bu öğreticinin daha tanıdık gelecektir şekilde önceki öğreticide oluşturduğunuz sayfası.

Tek bir film düzenlemek için bir yol uygulayabileceğine birkaç yolu vardır. Uygulanması kolaydır ve kolay anlaşılır olduğundan gösterilen yaklaşım seçilmiştir.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Düzenle bağlantısının film listeye ekleme

Başlamak için güncelleştireceksiniz *filmler* de listeleme her filmin içeren sayfa bir **Düzenle** bağlantı.

Açık *Movies.cshtml* dosya.

Sayfanın gövdesi içinde değiştirmek `WebGrid` sütunu ekleyerek biçimlendirme. Değiştirilen biçimlendirmesi şöyledir:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Bu yeni bir sütun verilmiştir:

[!code-html[Main](updating-data/samples/sample2.html)]

Bir bağlantı noktası bu sütunun göstermektir (`<a>` öğesi) metni "Düzenle" diyor. Sonra duyuyoruz olduğundan sayfa çalıştığında, aşağıdaki gibi görünen bir bağlantı oluşturmak için ile `id` her filmin için farklı bir değer:

[!code-css[Main](updating-data/samples/sample3.css)]

Bu bağlantıyı adlı sayfanın çağıracağı *EditMovie*, ve sorgu dizesi geçer `?id=7` o sayfaya.

Yeni bir sütun için söz dizimi biraz karmaşık görünebilir, ancak yalnızca birkaç öğeleri birlikte koyar olmasıdır. Her bağımsız öğede oldukça basittir. Hakkında yoğunlaşabilirsiniz, yalnızca `<a>` öğesi, bu işaretleme görürsünüz:

[!code-html[Main](updating-data/samples/sample4.html)]

Kılavuz nasıl çalıştığı hakkında bazı arka plan: satırları, her veritabanı kaydı için bir kılavuz gösterir ve veritabanı kayıttaki her alan için sütunları gösterir. Her bir kılavuz satırı oluşturulmuş olsa da `item` nesne ilgili satır için veritabanı kaydını (öğe) içerir. Bu düzenleme, kod ilgili satır için veri almak için bir yol sunar. Burada gördüğünüz olmasıdır: ifade `item.ID` geçerli veritabanı öğesi kimliği değerinin alınması. Kullanarak herhangi bir veritabanı değeri (başlık, türe veya yıl) aynı şekilde alabilir `item.Title`, `item.Genre`, veya `item.Year`.

İfade `"~/EditMovie?id=@item.ID` hedef URL kodlanmış bölümü birleştirir (`~/EditMovie?id=`) dinamik olarak türetilmiş Bu kimliğe sahip (Gördüğünüz `~` önceki öğreticide; işleci, geçerli Web sitesinin kök temsil eden bir ASP.NET işlecidir.)

Bu sütunda biçimlendirme parçası yalnızca aşağıdaki biçimlendirme gibi çalışma zamanında ürettiğini sonucudur:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Doğal olarak, gerçek değerini `id` her satır için farklı olacaktır.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Kılavuz sütunu için bir özel görüntü oluşturma

Kılavuz Sütunu şimdi yedekleyin. Üç sütunu ilk olarak görüntülenen kılavuz yalnızca veri değerlerini (başlık, türe ve yıl) vardı. Veritabanı sütununun adını geçirerek bu görüntü belirtilen &mdash; örneğin `grid.Column("Title")`.

Bu yeni **Düzenle** bağlantı sütununa farklıdır. Bir sütun adı belirtmek yerine çağırıyorsanız bir `format` parametresi. Bu parametre, biçimlendirme tanımlamanıza olanak tanır, `WebGrid` yardımcı işleme ile birlikte `item` kalın veya yeşil olarak sütun verileri görüntüleyebilir veya ne olursa olsun, sizin biçiminde istediğiniz değer. Örneğin, başlığı kalın görünmesini isterseniz şu örnekteki gibi bir sütun oluşturabilirsiniz:

[!code-html[Main](updating-data/samples/sample6.html)]

(Çeşitli `@` içinde gördüğünüz karakterleri `format` özelliği işaretleme ve kod değeri arasında geçiş işaretlemek.)

Hakkında öğrendikten sonra `format` özelliği olduğu anlamak daha kolay bir şekilde nasıl yeni **Düzenle** bağlantı sütununa birlikte yerleştirin:

[!code-html[Main](updating-data/samples/sample7.html)]

Sütun oluşur *yalnızca* bağlantıyı işleyen biçimlerini, bazı bilgiler (ID) yanı sıra, ayıklanır satır için veritabanı kaydı.

> [!TIP]
> 
> **Adlandırılmış parametreler ve bir yöntem için konumsal Parametreler**
> 
> Birden çok kez bir yöntemi ve parametreleri geçirilen parametre değerleri virgülle ayırarak yalnızca listelediğiniz. Aşağıda birkaç örnek verilmiştir:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Biz bu kod ilk gördüğünüz, ancak her durumda, belirli bir sırayla yöntemleri için parametre geçirme sorun bahsetmek yaramadı &mdash; yani, bu yönteme parametreleri tanımlanmıştır sırası. İçin `db.Execute` ve `Validation.RequireFields`, geçirdiğiniz değerlerin sırasını karıştırıldığında sayfa çalıştığında bir hata iletisi veya en azından bazı ilginç sonuçlar elde edersiniz. Açıkça görülebileceği gibi parametreleri geçirmek için sipariş bilmeniz gerekir. (Webmatrix'te, IntelliSense, adı, türü ve parametre sırası şekil öğrenmenize yardımcı olabilir.)
> 
> Değerlerin sırayla alternatif, kullandığınız *adlandırılmış parametreleri*. (Sırayla parametreleri geçirme bilinen kullanılarak *konumsal parametreler*.) Adlandırılmış parametreler için açıkça parametrenin adı değerini iletirken içerir. Aşağıdaki öğreticilerde adlandırılmış parametreler birkaç kez zaten kullandınız. Örneğin:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> and
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Özellikle fazla sayıda parametre bir yöntem çektiğinde adlandırılmış parametreler durumlarda birkaç için kullanışlıdır. Yalnızca bir veya iki parametre geçirmek istediğiniz, ancak geçirmek istediğiniz değerleri parametre listesindeki ilk konumlar arasında değildir biridir. Sizin için anlamlı bir sırada parametreleri ileterek kodunuzu daha okunabilir hale getirmek istediğinizde başka bir durumdur.
> 
> Kuşkusuz, adlandırılmış parametreleri kullanmak için parametrelerin adlarını bilmeniz gerekir. WebMatrix IntelliSense için *Göster* adları, ancak şu anda bunları sizin için doldurduğunuz olamaz.


## <a name="creating-the-edit-page"></a>Düzenleme sayfası oluşturma

Oluşturabileceğiniz artık *EditMovie* sayfası. Kullanıcılar'ı tıklattığınızda **Düzenle** bağlantı, bunlar düştüğünden bu sayfada.

Adlı bir sayfa oluşturun *EditMovie.cshtml* dosyasında aşağıdaki işaretlemeyle nedir değiştirin:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Bu işaretleme ve kod, sahip benzer *AddMovie* sayfası. Gönder düğmesine metninde küçük bir fark yoktur. Olduğu gibi *AddMovie* sayfasında, var olan bir `Html.ValidationSummary` çağrı varsa, doğrulama hataları görüntüler. Biz bırakarak çağrıları çıkış şu `Validation.Message`, bu yana hataları doğrulama özeti görüntülenir. Önceki öğreticide belirtildiği gibi doğrulama özeti ve tek hata iletileri çeşitli birleşimler kullanabilirsiniz.

Yeniden dikkat `method` özniteliği `<form>` ayarlanır `post`. Olduğu gibi *AddMovie.cshtml* sayfası, bu sayfada yapar değişiklikleri veritabanına. Bu nedenle, bu formu gerçekleştirmesi gereken bir `POST` işlemi. (Arasındaki fark hakkında daha fazla bilgi için `GET` ve `POST` işlemleri görmek [GET, POST ve HTTP fiili güvenliği](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) HTML formları öğreticide kenar.)

Önceki bir öğreticide gördüğünüz gibi `value` metin kutularını özniteliklerini ayarlanır Razor koduyla bunları dağıtılacak için. Bu kez, yine de değişkenlere benzer kullanmakta olduğunuz `title` ve `genre` yerine bu görev `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Olarak daha önce bu işaretleme film değerlerin metin kutusu değerlerle dağıtılacak. Neden olduğu kullanmak yerine bu kez değişkenleri kullanmak için kullanışlı bir dakika içinde görürsünüz `Request` nesne.

Ayrıca bir `<input type="hidden">` bu sayfadaki öğesi. Bu öğe görünür sayfada yapmadan film Kimliğini depolar. Kimliği başlangıçta sayfa tarafından bir sorgu dizesi değerini kullanarak geçirilir (`?id=7` veya URL'ye benzer). Kimlik değerini gizli bir alana koyarak kullanılabilir olduğundan emin olursunuz zaman formun gönderildi, artık sayfayı çağrıldı özgün URL'ye erişiminiz olsa bile.

Farklı *AddMovie* sayfası, kodunu *EditMovie* sayfası, iki ayrı işlevler içerir. Sayfanın ilk kez görüntülendiğinde olan ilk işlev (ve *yalnızca* sonra), kod Sorgu dizesinden film kimliği alır. Kod ardından kullandığı kimliği karşılık gelen bir film veritabanı dışına okuyun ve görüntülemek için (Bu metin kutularına dağıtılacak).

Kullanıcı tıkladığında olan ikinci işlevi **değişiklikleri gönderme** düğmesini koduna sahip metin kutularını değerlerini okumak ve onları doğrulamak. Kod, yeni değerlerle veritabanı öğeyi güncelleştirmek de vardır. Gördüğünüz gibi bu teknik bir kayıt eklemeye benzer *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Tek bir filmi okumayı kod ekleme

İlk işlev gerçekleştirmek için bu kodu sayfanın üst kısmına ekleyin:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Bu kod çoğunu başlatan bir blok içinde olduğu `if(!IsPost)`. `!` İşleci anlamına gelir "not", ifade anlamına gelir. Bu nedenle *bu isteği bir posta gönderimi değilse*, bildiren dolaylı bir yolu olan *çalıştıran bu sayfa ilk kez bu isteği olup olmadığını*. Daha önce belirtildiği gibi bu kodun çalışması gereken *yalnızca* ilk kez çalıştırdığında sayfası. Kod içine yaramadı alırsanız `if(!IsPost)`, sayfa, olup olmadığını ilk kez her çağrıldığında çalıştırmak veya yanıt düğmesine tıklayın.

Kodu içeren bildirimi bir `else` şu engelleyin. Ne zaman kullanıma sunduk dediğin gibi `if` blokları, bazen test koşul true değilse alternatif kod çalıştırmak istediğiniz. Durum burada olmasıdır. (Diğer bir deyişle, sayfaya geçirilen kimliği Tamam ise), koşul başarılı olursa, veritabanından bir satırı okuyun. Ancak, koşul başarısız olursa `else` bloğu çalışır ve bir hata iletisi kodunu ayarlar.

## <a name="validating-a-value-passed-to-the-page"></a>Sayfaya geçirilen bir değeri doğrulanıyor

Kod `Request.QueryString["id"]` sayfasına geçirilir kimliği alınamıyor. Kod bir değeri kimliği için gerçekten iletildi emin olur Hiçbir değer geçirildi, kod bir doğrulama hatası ayarlar.

Bu kod, bilgileri doğrulamak için farklı bir yol gösterir. Önceki öğreticide çalıştığınız `Validation` Yardımcısı. Doğrulamak için alanları kayıtlı ve ASP.NET otomatik olarak doğrulama tamamladıysanız ve hataları kullanarak görüntülenen `Html.ValidationMessage` ve `Html.ValidationSummary`. Bu durumda, ancak siz değil gerçekten kullanıcı girişini doğrulama. Bunun yerine, sayfaya başka yerlerden geçirilen değeri doğrulama. `Validation` Yardımcı olmayan yapın, sizin için.

Bu nedenle, değeri kendiniz ile test ederek kontrol `if(!Request.QueryString["ID"].IsEmpty()`). Bir sorun varsa, hata kullanarak görüntüleyebilirsiniz `Html.ValidationSummary`ile yaptığınız gibi `Validation` Yardımcısı. Bunu yapmak için çağrı `Validation.AddFormError` ve görüntülenecek bir ileti geçirin. `Validation.AddFormError` oturum zaten alışık olduğunuz doğrulama sistemi tie özel iletiler tanımlamanızı sağlar yerleşik bir yöntemdir. (Bu öğreticinin ilerleyen bölümlerinde bu doğrulama işlemi biraz daha sağlam hale hakkında konuşacağız.)

Film kimliği olduğundan emin olduktan sonra kodu yalnızca bir tek veritabanı öğesi için arayan veritabanı okur. (Genel düzen veritabanı işlemlerinin büyük bir olasılıkla fark etmiş: veritabanı, SQL deyimini tanımlayın ve deyimini çalıştırın.) Bu kez, SQL `Select` deyimi `WHERE ID = @0`. Kimliği benzersiz olduğundan, yalnızca bir kayıtla döndürülebilir.

Sorgu kullanarak gerçekleştirilir `db.QuerySingle` (değil `db.Query`film listeleme için kullandığınız gibi), ve kod sonucu koyar `row` değişkeni. Adı `row` rastgeledir; değişkenleri, istediğiniz şekilde adlandırabilirsiniz. Böylece bu değerleri metin kutularına görüntülenebilen en üstünde başlatılmamış değişkenler ardından film ayrıntıları ile doldurulur.

## <a name="testing-the-edit-page-so-far"></a>Düzen Sayfası (şimdiye) test etme

Sayfanızın test etmek istiyorsanız, çalıştırma *filmler* artık sayfasında ve tıklayın bir **Düzenle** tüm film yanındaki bağlantı. Göreceğiniz *EditMovie* Ayrıntıları sayfası, seçilen film doldurulur:

![Düzenlenecek film gösteren film sayfayı Düzenle](updating-data/_static/image3.png)

Sayfanın URL'sini gibi içerdiğine dikkat edin `?id=10` (veya başka bir sayı). Şu ana kadar test ettiğiniz **Düzenle** Bağlantılar *film* çalışma sayfanızı kimliği Sorgu dizesinden okuyor ve veritabanı tek film kaydını almak için sorgu çalışma sayfasında.

Film bilgileri değiştirebilirsiniz, ancak tıkladığınızda hiçbir şey olmuyor **değişiklikleri gönderme**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Film kullanıcının değişikliklerle güncelleştirmek için kod ekleme

İçinde *EditMovie.cshtml* (değişiklikleri kaydetme) ikinci işlevi uygulamak için aşağıdaki kodu yalnızca kapanış ayracı ekleyin `@` blok. (Kod tam olarak nerede emin değilseniz bakabilirsiniz [film düzenleme sayfası için tam kod](#Complete_Page_Listing_for_EditMovie) Bu öğreticinin sonunda görünür.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Yeniden, bu işaretleme ve kod kodda benzer *AddMovie*. Kodu bir `if(IsPost)` bloğunda yalnızca kullanıcı tıkladığında bu kodu çalıştığından **değişiklikleri gönderme** düğmesi &mdash; diğer bir deyişle, ne zaman (ve yalnızca) form forumumuza gönderildi. Bu durumda, bir test gibi kullanmadığınız `if(IsPost && Validation.IsValid())`— diğer bir deyişle, her iki testleri kullanarak birleştirdiğiniz değil ve. Bu sayfada, öncelikle bir form gönderimi olup olmadığını belirlemeniz (`if(IsPost)`) ve ardından yalnızca doğrulama için alanları kaydedin. Doğrulama sonuçları test edebilirsiniz sonra (`if(Validation.IsValid()`). Akışı biraz daha farklıdır *AddMovie.cshtml* sayfa ancak etkisi aynıdır.

Kullanarak metin kutularını değerlerini alın `Request.Form["title"]` ve diğer benzer bir kod `<input>` öğeleri. Bu kez, film Kimliğini kod dışında gizli alan alır dikkat edin (`<input type="hidden">`). Sayfanın ilk kez çalıştırdığınızda, kodu sorgu dizesi dışında kimliği alındı. Sorgu dizesi şekilde o zamandan bu yana değiştirildiği durumda başlangıçta zobrazilo film kimliği aldığınızdan emin olmak için gizli alanındaki değer elde edin.

Arasındaki en önemli fark *AddMovie* kodu ve bu kodu olduğundan bu kodda, SQL kullanmanızı `Update` deyimi yerine `Insert Into` deyimi. Aşağıdaki örnek SQL söz dizimi görülmektedir `Update` deyimi:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Herhangi bir sırada hiçbir sütun belirtebilirsiniz ve mutlaka sırasında her bir sütunun yoksa bir `Update` işlemi. (Kimliği kendisini yürürlükte kaydettiğiniz kaydı yeni bir kayıt ve verilmeyen için olduğundan güncelleştirilemiyor bir `Update` işlem.)

> [!NOTE] 
> 
> **Önemli** `Where` veritabanı hangi veritabanı nasıl bilir olduğu için kimliği yan tümcesinde çok önemli güncelleştirmek istediğiniz kayıt. Devre dışı bırakılırsa `Where` yan tümcesi, veritabanı güncelleştirme *her* veritabanında kayıt. Çoğu durumda, olağanüstü bir durumda olacaktır.


Kodda yer tutucuları kullanarak SQL deyiminde güncelleştirilecek hiçbir değer geçirilir. Ne önce söz ettiğimiz yinelenecek: güvenlik nedeniyle, *yalnızca* bir SQL deyimi için değerleri geçirmek için kullanın.

Kod sonra `db.Execute` çalıştırılacak `Update` deyimi, değişiklikleri görebileceğiniz geri listesi sayfasına yönlendirir.

> [!TIP] 
> 
> **Farklı SQL deyimleri, farklı yöntemler**
> 
> Farklı SQL deyimleri çalıştırma için biraz farklı yöntemler kullanın fark etmiş olabilirsiniz. Çalıştırılacak bir `Select` sorgu, büyük olasılıkla döndürür birden çok kayıt, kullandığınız `Query` yöntemi. Çalıştırılacak bir `Select` bildiğiniz bir sorgu tek bir veritabanı öğesi döndürür, kullandığınız `QuerySingle` yöntemi. Değişiklikleri yapın, ancak veritabanı öğeleri iade olmayan komutları çalıştırmak için kullandığınız `Execute` yöntemi.
> 
> Çünkü bunların her biri farklı sonuçlar döndürür zaten arasındaki farkı içinde anlatıldığı gibi farklı yöntemlere sahip olmanız `Query` ve `QuerySingle`. ( `Execute` Aslında bir değer döndürür ayrıca &mdash; yani, komut tarafından etkilenen veritabanı satır sayısını &mdash; ancak sizin, şimdiye yoksayılıyor.)
> 
> Elbette, `Query` yöntemi yalnızca bir veritabanı satır döndürebilir. Ancak, ASP.NET sonuçları her zaman davranır `Query` yöntem bir koleksiyon. Yöntemi yalnızca bir satır döndürür olsa bile, tek bir satır koleksiyondan ayıklamak sahip. Bu nedenle, durumlarda Burada, *bilmeniz* kullanmak daha kullanışlı bir bit olduğundan, yalnızca bir satır döneceğiz `QuerySingle`.
> 
> Belirli türde bir veritabanı işlemleri gerçekleştirmek birkaç yöntem vardır. Veritabanı yöntemleri listesini bulabilirsiniz [ASP.NET Web sayfaları API hızlı başvurusu](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Doğrulama için kimliği daha sağlam hale

Bu film veritabanından Al çalışabilmeniz için sayfa çalıştığında, ilk kez, film Kimliğini Sorgu dizesinden alın. Aslında olduğunu bu kodu kullanarak yaptığınız arayın, gitmek için bir değer emin yaptınız:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Bu kod bir kullanıcı için alırsa emin olmak için kullanılan *EditMovies* filmi ilk seçmeden sayfası *filmler* sayfası, sayfayı kolay bir hata iletisi görüntüler. (Aksi takdirde, kullanıcılar büyük olasılıkla yalnızca bunları karıştırır bir hata görür.)

Ancak, bu doğrulama çok sağlam değil. Sayfa Ayrıca bu hatalarla çağrılan:

- Kimliği bir sayı değil. Örneğin, sayfa gibi bir URL ile çağırılabilir `http://localhost:nnnnn/EditMovie?id=abc`.
- Bir sayı Kimliğin şöyle olduğunu, ancak mevcut olmayan bir filmi başvurduğu (örneğin, `http://localhost:nnnnn/EditMovie?id=100934`).

Bu URL'lerden çalıştırma sonucu görüyorsunuz merak ediyorsanız *filmler* sayfası. Bir filmi düzenlemek için seçin ve sonra URL'sini değiştirme *EditMovie* kimliği veya var olmayan film Kimliğini içeren bir alfabetik bir URL'ye sayfa.

Bu nedenle ne yapmalısınız? İlk düzeltmeyi yalnızca bir kimlik sayfasına geçirilir, ancak kimliği bir tamsayı olduğu emin olmaktır. Kodu değiştirmek `!IsPost` test şu örnekteki gibi aramak için:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

İkinci bir koşul eklediğiniz `IsEmpty` ile bağlantılı test `&&` (mantıksal ve):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Hatırlıyor [ASP.NET Web sayfaları programlama giriş](../introducing-razor-syntax-c.md) yöntemleri gibi öğretici `AsBool` bir `AsInt` bir karakter dizesi bazı diğer veri türüne dönüştürün. `IsInt` Yöntemi (ve diğerleri gibi `IsBool` ve `IsDateTime`) benzer. Ancak, bunlar yalnızca sınama olup olmadığını, *olabilir* dize dönüştürme işlemini gerçekleştirmeden Dönüştür. Burada, temelde belirten *tamsayıya sorgu dize değeri dönüştürülebilir ise...* .

Başka bir olası sorun, mevcut olmayan bir film arıyor. Bir filmi alma kodu şu kod gibi görünür:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Geçirirseniz bir `movieId` değerini `QuerySingle` deyimleri, izleyin ve gerçek bir filmi gelmiyor yöntemi, hiçbir şey döndürülür (örneğin, `title=row.Title`) hatalara neden.

Tekrar bir kolayca düzeltme yoktur. Varsa `db.QuerySingle` yöntemi döndürür, sonuç `row` değişkeni null olacaktır. Denetleyebilirsiniz şekilde olmadığını `row` değerleri almak denemeden önce değişken NULL'dur. Aşağıdaki kodu ekler bir `if` bloğu içine tanesi değerlerini alma deyimleri `row` nesnesi:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Bu iki ek doğrulama testleri ile daha madde işareti kavram sayfa olur. İçin tam kod `!IsPost` dalı artık şu örnekteki gibi görünür:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Biz bir kez daha bu görev için iyi bir kullanımı olduğuna dikkat edin bir `else` blok. Testleri geçirmezseniz `else` blokları hata iletileri ayarlayın.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Filmler sayfasına geri dönmek için bağlantı ekleme

Son ve yararlı bir ayrıntı bağlantısını eklemektir geri *filmler* sayfası. Kullanıcıların olayları sıradan akışında başlangıç *filmler* sayfasında ve tıklayın bir **Düzenle** bağlantı. Getiren bunları *EditMovie* sayfasında, burada film düzenleme yapıp düğmesine tıklayın. Kod değişikliği işledikten sonra geri yönlendirir *filmler* sayfası.

Ancak:

- Kullanıcı herhangi bir şey değiştirmemeye karar verebilirsiniz.
- Kullanıcının bu sayfaya ilk tıklamadan edindiğiniz bir **Düzenle** bağlantısını *filmler* sayfası.

Her iki durumda da, bunları ana listesi için döndürülecek kolaylaştırmak istediğiniz. Çözümü kolay olan &mdash; kapattıktan sonra aşağıdaki işaretlemeyi ekleyin `</form>` biçimlendirmede etiketi:

[!code-html[Main](updating-data/samples/sample19.html)]

Bu işaretleme aynı sözdizimini kullanan bir `<a>` başka bir yerde gördüğünüz öğesi. URL içerir `~` auto'yu "Web sitesinin kök."

## <a name="testing-the-movie-update-process"></a>Film güncelleştirme işlemi test etme

Artık test edebilirsiniz. Çalıştırma *filmler* sayfasında ve tıklayın **Düzenle** film yanında. Zaman *EditMovie* sayfası görüntülenirse, film ve seçeneğini değişiklik **değişiklikleri gönderme**. Film listesi göründüğünde, değişikliklerinizi gösterildiğinden emin olun.

Doğrulama çalıştığından emin olmak için tıklayın **Düzenle** başka bir filmi için. Ulaştığınızda *EditMovie* sayfasında, NET **Tarz** alan (veya **yıl** alan veya her ikisi de) ve yaptığınız değişiklikleri göndermek deneyin. Beklediğiniz gibi bir hata görürsünüz:

![Doğrulama hatalarını gösteren film sayfayı Düzenle](updating-data/_static/image4.png)

Tıklayın **dönmek için film liste** değişikliklerinizden vazgeçmek ve dönmek için bağlantı *filmler* sayfası.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide, bir film kayıt silme görürsünüz.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Tam listesi için (düzenleme bağlantıları ile güncelleştirilmiş) film sayfası

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Tam düzen film sayfa için sayfa listesi

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web programlama Razor söz dizimini kullanarak giriş](../../getting-started/introducing-razor-syntax-c.md)
- [SQL güncelleştirme bildirimi](http://www.w3schools.com/sql/sql_update.asp) W3Schools sitesinde

> [!div class="step-by-step"]
> [Önceki](entering-data.md)
> [İleri](deleting-data.md)
