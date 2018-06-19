---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: ASP.NET Web sayfaları sunarak - formları kullanarak veritabanı veri girme | Microsoft Docs
author: tfitzmac
description: Bu öğreticide, bir giriş formu oluşturma ve ASP.NET Web sayfaları (... kullandığınızda formundan bir veritabanı tablosuna alma verileri enter gösterilir
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: bbccf8134e90c19e29efaa5afe1e46e15320c189
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897975"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>ASP.NET Web sayfaları sunarak - formları kullanarak veritabanı veri girme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici bir giriş formu oluşturma ve ASP.NET Web sayfaları (Razor) kullandığınızda, bir veritabanı tablosuna formdan alma veri enter gösterilmektedir. Seri aracılığıyla tamamladığınızdan varsayar [HTML formu, temel ASP.NET Web Pages'de](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Öğrenecekleriniz:
> 
> - Girişi formları işlem hakkında daha fazla.
> - Verileri (Ekle) bir veritabanına ekleme.
> - Nasıl kullanıcılar gerekli değeri biçiminde (kullanıcı girişi doğrulamak nasıl) girdiğinizden emin olun.
> - Doğrulama hataları görüntülemek nasıl.
> - Geçerli sayfadaki başka bir sayfaya gitmek nasıl.
>   
> 
> Özellikler/teknolojilerini ele alınan:
> 
> - `Database.Execute` Yöntemi.
> - SQL `Insert Into` deyimi
> - `Validation` Yardımcısı.
> - `Response.Redirect` Yöntemi.


## <a name="what-youll-build"></a>Ne oluşturacağınız

Bir veritabanı oluşturmak nasıl oluşturulacağını gösterir öğreticide daha önce veritabanı verileri doğrudan çalışma WebMatrix veritabanında düzenleyerek girdiğiniz **veritabanı** çalışma. Çoğu uygulamada, verileri veritabanına ancak yerleştirmek için kolay bir yolu değil. Bu nedenle bu öğreticide, veya herkes veri girme ve veritabanına kaydetme imkan tanıyan web tabanlı bir arabirim oluşturacaksınız.

Yeni filmler girebilecekleri bir sayfa oluşturacaksınız. Sayfa bir filmi, tarzını ve yıl girebileceğiniz alanları (metin kutularında) olan bir giriş formu içerir. Sayfa bu sayfayı gibi görünür:

!['Film Ekle' sayfasını tarayıcıda](entering-data/_static/image1.png)

Metin kutuları HTML olacaktır `<input>` gibi bu biçimlendirme görüneceğini öğeler:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Temel giriş formu oluşturma

Adlı bir sayfa oluşturma *AddMovie.cshtml*.

Aşağıdaki biçimlendirmede dosyasıyla nedir değiştirin. Her şeyi üzerine; en üstte kısa süre içinde bir kod bloğu ekleyeceksiniz.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Bu örnek, bir form oluşturmak için tipik HTML gösterir. Kullandığı `<input>` öğeleri Gönder düğmesi ve metin kutuları için. Metin kutuları için resim yazıları standart kullanılarak oluşturulan `<label>` öğeleri. `<fieldset>` Ve `<legend>` öğeleri yerleştirmek formun çevresinde iyi bir kutu.

Bu sayfada, dikkat `<form>` öğesini kullanan `post` değeri olarak `method` özniteliği. Önceki öğreticide oluşturduğunuz kullanılan bir form `get` yöntemi. Form değerleri sunucuya gönderilen rağmen istek herhangi bir değişiklik yapmadınız olduğundan, doğru. Tümü olduğu farklı şekillerde fetch veri oluştu. Ancak, bu konuda, sayfa *olacak* değişiklik — yeni veritabanı kayıtları eklemek için oluşturacağız. Bu nedenle, bu formu kullanması gereken `post` yöntemi. (Arasındaki fark hakkında daha fazla bilgi için `GET` ve `POST` işlemleri görmek[GET, POST ve HTTP fiili güvenliği](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) önceki öğreticideki kenar.)

Her metin kutusuna sahip Not bir `name` öğesi (`title`, `genre`, `year`). Önceki öğreticide gördüğünüz gibi daha sonra kullanıcının giriş sınıflandırıp bu adları olması gerektiğinden bu adları önemlidir. Herhangi bir ad kullanabilirsiniz. İçin yararlıdır yardımcı anlamlı adları kullan ile çalışırken hangi verilerin unutmayın.

`value` Her özniteliği `<input>` öğesi içeren bir boyutta bir Razor kod (örneğin, `Request.Form["title"]`). Form gönderildikten sonra önceki öğreticide, (varsa) metin kutusuna girilen değer korumak için bu eli sürümü öğrendiniz.

## <a name="getting-the-form-values"></a>Form değerleri alma

Ardından, formu işleyen kodu ekleyin. Anahat içinde şunları yapmanız:

1. Sayfa gönderilen olup olmadığını denetleyin (gönderildi). Kodunuzu değil sayfa ilk kez çalıştırdığında yalnızca kullanıcıların düğmesine tıklandığında çalıştırmak istediğiniz.
2. Metin kutularına kullanıcının girdiği değerleri alır. Bu durumda, formun kullandığından `POST` fiili form değerleri alma `Request.Form` koleksiyonu.
3. Yeni bir kayıt olarak değerleri Ekle *filmler* veritabanı tablosu.

Dosyanın üst kısmında, aşağıdaki kodu ekleyin:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

İlk birkaç satırı değişkenlerinin (`title`, `genre`, ve `year`) değerleri metin kutularından tutmak için. Satır `if(IsPost)` değişkenleri ayarlandığından emin yapar *yalnızca* kullanıcılar'ı tıklattığınızda **eklemek film** düğmesi — diğer bir deyişle, ne zaman formun yayımlandı.

Bir önceki öğreticide gördüğünüz gibi bir metin kutusunun değerini gibi ifade kullanarak aldığınız `Request.Form["name"]`, burada *adı* adı `<input>` öğesi.

Değişken adları (`title`, `genre`, ve `year`) rastgeledir. Gibi atadığınız adlar `<input>` öğeleri, bunları istediğiniz herhangi bir şey çağırabilirsiniz. (Değişkenlerin adlarındaki ad özniteliklerini eşleşmesi gerekmez `<input>` formda öğeleri.) Ancak olduğu gibi `<input>` öğeleri, içerdikleri verileri yansıtacak değişken adları kullanmak iyi bir fikir değil. Kodu yazarken tutarlı adlar, üzerinde çalıştığınız hangi verilerin anımsaması kolaylaştırır.

## <a name="adding-data-to-the-database"></a>Veritabanına veri ekleme

Kodda, yeni eklenen, yalnızca engelleme *içinde* kapanış ayracı ( `}` ), `if` engelle (yalnızca kod bloğunun içine), aşağıdaki kodu ekleyin:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Bu örnek, önceki öğreticide fetch ve görüntü verileri için kullanılan kod benzerdir. İle başlayan satırı `db =` gibi önce veritabanı ve bir sonraki satıra tanımlayan bir SQL deyimi yeniden olarak açılır önce gördünüz. Ancak, bu süre, tanımlayan bir SQL `Insert Into` deyimi. Aşağıdaki örnek genel söz dizimi görülmektedir `Insert Into` deyimi:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Diğer bir deyişle, takın, tabloya eklemek için sütunları listesinde ve değerler eklemek için liste belirtin. (Önce belirtildiği gibi SQL büyük küçük harfe duyarlı değildir ancak bazı kişiler komutu okunmasını kolaylaştırmak için anahtar sözcükler büyük harfle.)

İçine ekleme sütunları zaten komut içinde listelenen — `(Title, Genre, Year)`. Metin kutularına gelen değerlerin nereden ilginç parçası olan `VALUES` komutu parçası. Gerçek değerler yerine gördüğünüz `@0`, `@1`, ve `@2`, olan Elbette yer tutucuları. Komutu çalıştırdığınızda (üzerinde `db.Execute` satırı), metin kutularından aldığınız değerlerini geçirin.

**Önemli!** Burada gördüğünüz gibi herhangi bir zamanda içermelidir çevrimiçi bir SQL deyimi bir kullanıcı tarafından girilen veriler tek yolu yer tutucuları, kullanılacak olduğunu unutmayın (`VALUES(@0, @1, @2)`). Kullanıcı bir SQL deyimi girişine birleştirmek, kendinize bir SQL ekleme saldırısı açıklandığı gibi açmak [Form temelleri ASP.NET Web Pages'de](https://go.microsoft.com/fwlink/?LinkId=251581) (önceki öğretici).

İçinde hala `if` engellemek, sonra aşağıdaki satırı ekleyin `db.Execute` satır:

[!code-css[Main](entering-data/samples/sample4.css)]

Yeni film veritabanına eklendikten sonra bu satır, (yeniden yönlendirmeleri) atlar *filmler* girdiğiniz film görebilmeniz için sayfa. `~` İşleci "Web sitesinin kök." anlamına gelir ( `~` İşleci yalnızca içinde çalıştığı HTML değil, ASP.NET sayfaları genellikle.)

Tam kod bloğunu aşağıdaki gibi görünür:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>INSERT komutu (kadarki) test etme

Henüz tamamlanmadı, ancak test etmek için iyi bir zamandır sunulmuştur.

WebMatrix dosyalarında ağaç görünümünde, sağ *AddMovie.cshtml* sayfasında ve ardından **başlatma tarayıcıda**.

!['Film Ekle' sayfasını tarayıcıda](entering-data/_static/image2.png)

(, Farklı sayfasını tarayıcıda şunun, URL olduğundan emin olun `http://localhost:nnnnn/AddMovie`), burada *nnnnn* kullandığınız bağlantı noktası numarasıdır.)

Bir hata sayfası mı aldınız? Öyleyse, dikkatlice okuyun ve ne daha önce listelenen kodu tam olarak göründüğünden emin olun.

Bir filmi girin &mdash; Örneğin, "Citizen Gamze", "Ekranda" ve "1941" kullanın. (Veya ne olursa olsun.) Ardından **eklemek film**.

Tüm giderse iyi için yönlendirilirsiniz *filmler* sayfası. Yeni film listelendiğinden emin olun.

![Film yeni gösteren filmler sayfası eklenmedi](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

Geri dönerek *AddMovie* sayfasında ya da yeniden çalıştırın. Başka bir filmi, ancak bu kez sadece başlığı girin, &mdash; Örneğin, "Açev'ın Yağmur" girin. Ardından **eklemek film**.

İçin yönlendirilirsiniz *filmler* yeniden sayfa. Yeni film bulabilirsiniz, ancak eksik.

![Bazı değerler eksik gösteren yeni film filmler sayfası](entering-data/_static/image4.png)

Oluşturduğunuzda *filmler* tablosu, açıkça denirse alanlarının hiçbiri boş olması. Burada bir giriş formu yeni filmler için sahip ve alanları boş bırakarak. Bir hata olmasıdır.

Bu durumda, veritabanını gerçekten yükseltmek siz (veya *throw*) bir hata oluştu. Bir tarzını veya yıl, bu nedenle sağlamadı kodda *AddMovie* sayfa bu değerleri olarak sözde kabul *boş dizeler*. Zaman SQL `Insert Into` komutun çalıştığını, tarz ve yıl alanları yararlı veri içeren oldu, ancak null doğru.

Belli ki, yarı boş film bilgileri veritabanına girme kullanıcılar izin vermek istemezsiniz. Kullanıcının giriş doğrulamak için kullanılan çözümüdür. Başlangıçta, doğrulama yalnızca kullanıcının bir değer tüm alanlar için girdiğini emin olur (yani, bunların hiçbiri boş bir dize içerir).

> [!TIP]
> 
> **NULL ve boş dizeler**
> 
> Programlamada "değeri yok." farklı kavramları arasında fark yoktur Genel olarak, bir değerdir *null* , hiçbir zaman ayarlayın veya bırakıldığı herhangi bir şekilde başlatıldı. Karakter veri (dize) bekliyor bir değişken tersine, ayarlanabilir bir *boş dize*. Bu durumda, değeri null değil; Bunu yalnızca açıkça uzunluğunu sıfırdır karakter dizesi için ayarlanmış. Bu iki ifade fark göster:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Bu biraz daha karmaşık, ancak en önemli nokta olan `null` belirlenmemiş duruma sıralama temsil eder.
> 
> Şimdi yapıp değeri null olduğunda ve yalnızca boş bir dize olduğunda tam olarak anlamak önemlidir. Kodunda *AddMovie* sayfası, metin kutuları değerlerini kullanarak elde `Request.Form["title"]` ve benzeri. (Düğmesini tıklatmadan önce) sayfa ilk kez çalıştığında, değeri `Request.Form["title"]` null. Ancak formu gönderdiğinde `Request.Form["title"]` değerini alır `title` metin kutusu. Açık değildir, ancak boş bir metin kutusu boş değil; Bunu yalnızca boş bir dize içeriyor. Bu nedenle kod düğmesine yanıtında çalıştığında tıklayın, `Request.Form["title"]` , içinde boş bir dize içeriyor.
> 
> Bu ayrım neden önemlidir? Oluşturduğunuzda *filmler* tablosu, açıkça denirse alanlarının hiçbiri boş olması. Ancak burada yeni filmler için giriş formu sahip ve alanları boş bırakarak. Tarz veya yıl değerlerini olmadığına yeni filmler kaydetmeye çalışırken şikayetçi veritabanına makul beklenir. Ancak, noktasıdır &mdash; bu metin kutularını boş bırakın olsa bile değerleri null değil; boş dizeler oldukları. Sonuç olarak, bu sütun boş olan veritabanına yeni filmler kaydedemediğini &mdash; ancak null! &mdash; değerler. Bu nedenle, kullanıcıların kullanıcı girişini doğrulama yapmak için boş bir dize gönderme yok emin olmak zorunda.


### <a name="the-validation-helper"></a>Doğrulama Yardımcısı

ASP.NET Web sayfaları içeren bir yardımcı &mdash; `Validation` yardımcı &mdash; kullanıcılar gereksinimlerinizi karşılayan verileri girdiğinizden emin olmak için kullanabilirsiniz. `Validation` Yardımcı NuGet, bir önceki öğreticide Gravatar Yardımcısı yüklü yolunu kullanarak bir paket olarak yüklemek zorunda kalmamak için ASP.NET Web sayfaları için yerleşik olarak bulunan Yardımcıları biridir.

Kullanıcının giriş doğrulamak için şunları yapmanız:

- Kod sayfasında metin kutularındaki değerleri gerektiren istediğinizi belirtmek için kullanın.
- Böylece yalnızca her şeyin doğru şekilde doğrulanırsa film bilgileri veritabanına eklenen bir test koda yerleştirin.
- Kod hata iletilerini görüntülemek için işaretleme ekleyin.

Kod bloğundaki *AddMovie* sayfasında, sağa yukarı üst değişken bildirimleri önce aşağıdaki kodu ekleyin:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Çağırmanız `Validation.RequireField` her bir alan için bir kez (`<input>` öğesi) istediğiniz girdi gerekir. Burada gördüğünüz gibi her çağrı için bir özel hata iletisi de ekleyebilirsiniz. (Biz yalnızca, istediğiniz herhangi bir şey buraya koyabilirsiniz olduğunu göstermek için iletilerini değişmesi.)

Bir sorun varsa, veritabanına eklenen yeni film bilgilerinin önlemek istiyor. İçinde `if(IsPost)` engellemek, kullanın `&&` (testleri başka bir koşul eklemek için mantıksal ve) `Validation.IsValid()`. Bitirdiğinizde, bütün `if(IsPost)` blok Bu kod gibi görünür:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Bir doğrulama hatasıyla kullanarak kayıtlı alanlar ise `Validation` Yardımcısı, `Validation.IsValid` yöntemi false döndürür. Ve hiçbir geçersiz film girişleri veritabanına eklenecek şekilde bu durumda, bu bloğundaki kod hiçbiri, çalıştırılır. Ve tabi ki, için yönlendirilirsiniz değil *filmler* sayfası.

Doğrulama kodu içeren tam bir kod bloğu şimdi aşağıdaki gibi görünür:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Doğrulama hataları görüntüleme

Hata iletilerini görüntülemek için son adım olacaktır. Her bir doğrulama hata için tek bir ileti görüntüleyebilir veya bir Özet ya da her ikisini de görüntüleyebilirsiniz. Nasıl çalıştığını görebilmeniz için Bu öğretici için her ikisi de gerçekleştirirsiniz.

Her yanındaki `<input>` doğrulama öğesinde, çağrı `Html.ValidationMessage` yöntemi ve adını geçirin `<input>` doğrulama öğesi. Yerleştirdiğiniz `Html.ValidationMessage` yöntemi sağ istediğiniz yere görüntülenecek hata iletisi. Sayfa çalıştığında, `Html.ValidationMessage` yöntemi işler bir `<span>` doğrulama hatası gider burada öğesi. (Herhangi bir hata varsa `<span>` öğesi işlenir ancak metin yok içinde.)

Sayfa biçimlendirmede değiştirin, böylece içerdiği bir `Html.ValidationMessage` her üç yöntem `<input>` gibi öğeler sayfasında, bu örnek:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Özet nasıl çalıştığını görmek için de aşağıdaki biçimlendirmeyi ekleyin ve hemen sonra kod `<h1>Add a Movie</h1>` öğeyi sayfada:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Varsayılan olarak, `Html.ValidationSummary` yöntemi tüm doğrulama iletilerinin bir listesini görüntüler (bir `<ul>` içinde öğesi bir `<div>` öğesi). İle `Html.ValidationMessage` yöntemi, doğrulama özeti için biçimlendirme her zaman işlenir; herhangi bir hata varsa, liste öğelerini işlenir.

Özet kullanarak yerine doğrulama iletileri görüntülemek için alternatif bir yol olabilir `Html.ValidationMessage` her alana özgü hatayı görüntülemek için yöntem. Veya, özetini ve ayrıntılarını kullanabilirsiniz. Veya kullanabilirsiniz `Html.ValidationSummary` genel bir hata görüntülemek ve tek tek kullanmak için yöntem `Html.ValidationMessage` ayrıntılarını görüntülemek için çağrıları.

Tam sayfa şimdi aşağıdaki gibi görünür:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

İşte bu kadar. Film ekleme ancak bir veya daha fazla alan bırakmak artık sayfayı test edebilirsiniz. Bunu yaptığınızda, görüntüleme aşağıdaki hata görürsünüz:

![Doğrulama hata iletilerini gösteren film Sayfası Ekle](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Doğrulama hatası iletilerini stil oluşturma

Hata iletileri vardır, ancak bunlar gerçekten çok iyi göze yok görebilirsiniz. Hata iletileri, ancak stilini belirlemek için kolay bir yolu yoktur.

Tarafından görüntülenen tek tek hata iletilerini biçimlendirmek için `Html.ValidationMessage`, adlandırılmış bir CSS stil sınıfı oluşturmak `field-validation-error`. Doğrulama özetinin görünümünü tanımlamak için adlandırılmış bir CSS stil sınıfı oluşturmak `validation-summary-errors`.

Bu teknik nasıl çalıştığını görmek için Ekle bir `<style>` öğesi içinde `<head>` sayfasının bölümünde. Adlı stil sınıflarını tanımlayın `field-validation-error` ve `validation-summary-errors` aşağıdaki kuralları içerir:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalde büyük olasılıkla stil bilgilerini ayrı bir içine koyabilirsiniz *.css* dosya, ancak kolaylık sağlamak için bunları sayfasında şimdilik koyabilirsiniz. (Daha sonra Bu öğretici kümesinde, ayrı bir CSS kuralları taşırsınız *.css* dosyası.)

Bir doğrulama hatası varsa `Html.ValidationMessage` yöntemi işler bir `<span>` içeren öğe `class="field-validation-error"`. Bu sınıf için bir stil tanımını ekleyerek, ileti benzer yapılandırabilirsiniz. Hatalar, varsa `ValidationSummary` yöntemi dinamik olarak benzer şekilde işler öznitelik `class="validation-summary-errors"`.

Sayfa yeniden çalıştırın ve alanları birkaç kasıtlı olarak bırakın. Şimdi daha belirgin hatalardır. (Aslında, overdone, ancak yalnızca neler yapabileceğinizi göstermek için.)

![Biçimlendirilmiş doğrulama hataları gösteren film Sayfası Ekle](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Film sayfasına bağlantı ekleme

Bir son adımdır almak kullanışlı hale getirmek *AddMovie* özgün film listenin sayfasından.

Açık *filmler* yeniden sayfa. Kapatma sonrasında `</div>` izleyen etiketi `WebGrid` Yardımcısı, aşağıdaki biçimlendirmeyi ekleyin:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Daha önce gördüğünüz gibi ASP.NET yorumlar `~` Web sitesinin kök olarak işleci. Kullanmak zorunda değilsiniz `~` işleci; işaretleme kullanabileceğinizi `<a href="./AddMovie">Add a movie</a>` veya HTML anlar yolunu tanımlamak için başka bir şekilde. Ancak `~` işlecidir iyi bir genel yaklaşım Razor sayfalarının bağlantıları oluşturduğunuzda, sitenin daha esnek yaptığından — geçerli sayfa bir alt klasöre taşırsanız, bağlantıyı hala gideceği *AddMovie* sayfası. (Unutmayın `~` işleci yalnızca çalışır *.cshtml* sayfaları. ASP.NET anladığı, ancak standart HTML değildir.)

İşiniz bittiğinde çalıştırmak *filmler* sayfası. Bu sayfa gibi görünür:

!['Add filmler' sayfasına bağlantı filmler sayfası](entering-data/_static/image7.png)

Tıklatın **film ekleme** için gidiyor emin olmak için bağlantı *AddMovie* sayfası.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide zaten veritabanında mevcut verileri düzenleme kullanıcıların izin öğreneceksiniz.

## <a name="complete-listing-for-addmovie-page"></a>Tam listesi için AddMovie sayfası

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Razor sözdizimini kullanan ASP.NET Web programlamaya giriş](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL INSERT INTO deyimi](http://www.w3schools.com/sql/sql_insert.asp) W3Schools sitesinde
- [ASP.NET Web uygulamasında kullanıcı girdisi doğrulama sayfaları siteleri](https://go.microsoft.com/fwlink/?LinkId=253002). İle çalışma hakkında daha fazla bilgi `Validation` Yardımcısı.

> [!div class="step-by-step"]
> [Önceki](form-basics.md)
> [sonraki](updating-data.md)
