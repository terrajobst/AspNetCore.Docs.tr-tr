---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Formları kullanarak veritabanı verileri girme - ASP.NET Web sayfaları ile tanışın | Microsoft Docs
author: tfitzmac
description: Bu öğretici, bir giriş formunu oluşturma ve ardından ASP.NET Web sayfaları (...) kullandığınızda bir veritabanı tablosuna formdan alma verileri girin gösterir
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 41122b3bca5a3d3162a66be163642610b8349cc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386413"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>ASP.NET Web sayfaları ile tanışın - formları kullanarak veritabanı verileri girme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğreticide bir giriş formunu oluşturmak ve ardından ASP.NET Web sayfaları (Razor) kullandığınızda, bir veritabanı tablosuna formdan alma verileri girin gösterilmektedir. Bu seriyi aracılığıyla bitirdiğinizi [HTML formu, temel ASP.NET Web Pages'de](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Öğrenecekleriniz:
> 
> - Girişi formları işlem hakkında daha fazla.
> - Verileri (Ekle) bir veritabanında ekleme.
> - Nasıl kullanıcılar gerekli bir değer biçiminde (kullanıcı girişini doğrulama nasıl) girdiğinizden emin olun.
> - Doğrulama hataları görüntülemek nasıl.
> - Geçerli sayfadaki başka bir sayfaya gitmek nasıl.
>   
> 
> Ele alınan özelliklerin/teknolojiler:
> 
> - `Database.Execute` Yöntemi.
> - SQL `Insert Into` deyimi
> - `Validation` Yardımcısı.
> - `Response.Redirect` Yöntemi.


## <a name="what-youll-build"></a>Ne oluşturacaksınız

Bir veritabanını nasıl oluşturulacağını gösterir, öğreticide daha önce veritabanı verileri doğrudan çalışma WebMatrix veritabanında düzenleyerek girdiğiniz **veritabanı** çalışma. Çoğu uygulamada, verileri veritabanına, ancak yerleştirme için pratik bir yol değil. Bu nedenle bu öğreticide, siz veya biri veri girin ve veritabanına kaydetme sağlayan web tabanlı bir arabirimi oluşturacaksınız.

Yeni filmler girebileceğiniz bir sayfa oluşturacaksınız. Sayfanın bir filmi, türe ve yıl girebileceğiniz alanları (metin kutuları) olan bir giriş formunu içerir. Sayfa, bu sayfayı gibi görünür:

!['Add film' sayfasını tarayıcıda](entering-data/_static/image1.png)

HTML metin kutuları olur `<input>` gibi bu işaretleme görüneceğini öğeler:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Temel giriş formu oluşturma

Adlı bir sayfa oluşturun *AddMovie.cshtml*.

Dosyasına aşağıdaki işaretlemeyle nedir değiştirin. Her şeyi üzerine; kısa bir süre içinde en üstünde bir kod bloğu ekleyeceksiniz.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Bu örnek, tipik bir HTML form oluştururken gösterir. Kullandığı `<input>` öğe metin kutuları ve Gönder düğmesi. Standart kullanarak metin kutuları için resim yazıları oluşturulur `<label>` öğeleri. `<fieldset>` Ve `<legend>` öğeleri yerleştirme form çevresinde güzel bir kutu.

Bu sayfada, dikkat `<form>` öğesini kullanan `post` değeri olarak `method` özniteliği. Önceki öğreticide oluşturduğunuz kullanılan form `get` yöntemi. Form değerleri sunucuya gönderilen olsa da, herhangi bir değişiklik isteği yapmayan olduğundan, doğru. Tüm kişiselleştirmeden farklı yollarla verileri getirme oluştu. Ancak, bu konuda, sayfa *olacak* değişiklik — yeni veritabanı kayıtları eklemek için oluşturacağız. Bu nedenle, bu formu kullanmalısınız `post` yöntemi. (Arasındaki fark hakkında daha fazla bilgi için `GET` ve `POST` işlemleri görmek[GET, POST ve HTTP fiili güvenliği](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) kenar önceki öğreticide.)

Her metin kutusuna sahip Not bir `name` öğesi (`title`, `genre`, `year`). Önceki öğreticide gördüğünüz gibi daha sonra kullanıcının girişi sınıflandırıp adları olması gerektiğinden bu adlar önemlidir. Herhangi bir adı kullanabilirsiniz. İçin yararlıdır yardımcı anlamlı adlar kullanın hangi verilerin ile çalıştığınızı unutmayın.

`value` Her öznitelik `<input>` öğesi içeren bir boyutta bir Razor kod (örneğin, `Request.Form["title"]`). Form gönderildikten sonra bu yöntem bir sürümünü (varsa) metin kutusuna girilen değer korumak için önceki öğreticide öğrendiniz.

## <a name="getting-the-form-values"></a>Form değerlerini alma

Ardından, bu formu işleyen kodu ekleyin. Anahat içinde aşağıdakileri yapmanız:

1. Sayfa gönderilen olup olmadığını denetleyin (gönderildi). Kodunuzu ilk değil sayfa çalıştığında yalnızca kullanıcıların düğmeye tıklandığında çalıştırmak istediğiniz.
2. Metin kutularına kullanıcı girilen değerleri alır. Bu durumda, formu kullandığından `POST` fiil, form değerleri alma `Request.Form` koleksiyonu.
3. Yeni bir kayıt olarak değer eklemek *filmler* veritabanı tablosu.

Dosyasının en üstüne aşağıdaki kodu ekleyin:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

İlk birkaç satırı değişkenlerinin (`title`, `genre`, ve `year`) metin kutularında değerleri tutmak için. Satır `if(IsPost)` değişkenleri ayarlandığından emin olur *yalnızca* kullanıcılar'ı tıklattığınızda **ekleme film** düğmesi — diğer bir deyişle, ne zaman formun forumumuza gönderildi.

Önceki bir öğreticide gördüğünüz gibi değeri metin kutusu gibi bir ifade kullanarak almak `Request.Form["name"]`burada *adı* adıdır `<input>` öğesi.

Değişkenlerin adlarını (`title`, `genre`, ve `year`) rasgele. Atadığınız adlar gibi `<input>` öğeleri, bunları istediğiniz herhangi bir şey çağırabilirsiniz. (Değişkenlerin adlarını ad özniteliklerini eşleşmesi gerekmez `<input>` formu üzerindeki öğelerin.) Ancak olduğu gibi `<input>` öğeleri, içerdikleri veriler yansıtan değişken adları kullanmak iyi bir fikir olduğunu. Kod yazdığınızda, tutarlı adlar sizin çalıştığınız hangi verilerin kolaylaştırır.

## <a name="adding-data-to-the-database"></a>Veritabanına veri ekleme

Kodda, yeni eklenen, yalnızca engelleme *içinde* kapanış ayracı ( `}` ), `if` engelleyin (yalnızca kod bloğu içinde), aşağıdaki kodu ekleyin:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Bu örnek, önceki bir öğreticide veri getirme ve görüntüleme için kullanılan kod benzerdir. İle başlayan satırı `db =` açılır önce gibi veritabanı ve sonraki satırı tanımlayan bir SQL deyimi yeniden olarak daha önce gördüğünüz. Ancak, bu kez tanımladığı bir SQL `Insert Into` deyimi. Aşağıdaki örnek genel söz dizimi görülmektedir `Insert Into` deyimi:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Diğer bir deyişle, tablonun takın, ardından içine eklenecek sütun listesinde ve ardından liste değerleri eklemek için belirtin. (Bazı kişiler komutu okunmasını kolaylaştırmak için anahtar sözcükler büyük harf ancak önce belirtildiği gibi SQL büyük/küçük harfe duyarlı değildir.)

İçine ekleme sütunları zaten komut içinde listelenen — `(Title, Genre, Year)`. Metin kutularına gelen değerlerin nereden ilgi çekici bir parçası olan `VALUES` komutunun bir parçası. Gerçek değerleri yerine gördüğünüz `@0`, `@1`, ve `@2`, olan doğal yer tutucu. Komutu çalıştırdığınızda (üzerinde `db.Execute` satır), metin kutularında aldığınız değerlerini geçirirsiniz.

**Önemli!** Burada gördüğünüz gibi hiç olmadığı kadar içermelidir çevrimiçi bir SQL deyimi bir kullanıcı tarafından girilen veriler tek yolu yer tutucuları, kullanılacak olduğunu unutmayın (`VALUES(@0, @1, @2)`). Bir SQL deyimi kullanıcı girişlerini birleştirmek, kendiniz SQL ekleme saldırısına açıklandığı şekilde açmak [formuyla ilgili temel kavramlar ASP.NET Web Pages'de](https://go.microsoft.com/fwlink/?LinkId=251581) (önceki öğreticide).

İçinde hala `if` engelleme, sonra aşağıdaki satırı ekleyin `db.Execute` satırı:

[!code-css[Main](entering-data/samples/sample4.css)]

Yeni film veritabanına eklendikten sonra bu satır, (yeniden yönlendirmeleri) atlar *filmler* girdiğiniz film görebilmeniz için sayfa. `~` İşleci anlamına gelir "Web sitesinin kök." ( `~` İşleci yalnızca içinde çalıştığı ASP.NET sayfaları, HTML değil, genellikle.)

Tam kod bloğunu şu örnekteki gibi görünür:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>INSERT komutu (şimdiye) test etme

Henüz tamamlanmadı ancak şimdi test etmek için iyi bir zamandır.

WebMatrix dosyalarında ağaç görünümünde, sağ *AddMovie.cshtml* sayfasında ve ardından **tarayıcıda Başlat**.

!['Add film' sayfasını tarayıcıda](entering-data/_static/image2.png)

(Başka bir sayfaya tarayıcıda elde edersiniz, URL olduğundan emin olun `http://localhost:nnnnn/AddMovie`), burada *nnnnn* kullandığınız bağlantı noktası numarasıdır.)

Bir hata sayfası mı aldınız? Bu durumda, dikkatle okuyun ve ne daha önce listelenen kodu tam olarak göründüğünden emin olun.

Bir filmi girin &mdash; Örneğin, "Gamze Vatandaşlık", "DRAM" ve "1941" kullanın. (Veya.) Ardından **ekleme film**.

Tüm yapıldığında için yönlendirilirsiniz *filmler* sayfası. Yeni, film listelendiğinden emin olun.

![Film gösteren yeni filmler sayfası eklendi](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

Geri Git *AddMovie* sayfasında veya yeniden çalıştırın. Başka bir film, ancak bu kez girin, yalnızca bir başlık girin &mdash; "Açev'nda Yağmur" girin. Ardından **ekleme film**.

İçin yönlendirilirsiniz *filmler* yeniden sayfa. Yeni film bulabilirsiniz, ancak eksik.

![Bazı değerler eksik gösteren yeni film filmler sayfası](entering-data/_static/image4.png)

Oluştururken *filmler* tablo açıkça söylediğiniz alanların null olabileceğini. Burada bir giriş formunu yeni filmler için sahip ve alanları boş bırakılması. Bu bir hatadır.

Bu durumda, veritabanını gerçekten yükseltmek istemediğiniz (veya *throw*) bir hata oluştu. Bir türe veya yıl, bu nedenle belirtmediyseniz kodda *AddMovie* sayfası bu değerleri olarak anılan kabul *boş dizelere*. SQL `Insert Into` komutun çalıştığını, türe ve yıl alanları yararlı verileri bunları yoktu, ancak bunlar null olmayan.

Belli ki, yarı boş film bilgileri veritabanına girmek kullanıcılara bildirmek istemiyorum. Kullanıcı girişini onaylamak için çözümüdür. Başlangıçta, doğrulama yalnızca kullanıcı bir değer tüm alanlar için girdiğini emin olmanızı sağlar (diğer bir deyişle, bunların hiçbiri boş bir dize içeriyor).

> [!TIP]
> 
> **NULL ve boş dizeler**
> 
> Programlamada, "değer yok" farklı kavramları arasında bir ayrım yoktur Genel olarak, bir değerdir *null* ise, hiçbir zaman ayarlayın veya bırakıldığı herhangi bir şekilde başlatıldı. Karakter verileri (dize) bekliyor bir değişkeni buna karşılık, ayarlanabilir bir *boş dize*. Bu durumda, değer null değil; Bunu yalnızca açıkça uzunluğu olan sıfır karakter dizesi için ayarlanmış. Bu iki deyimden farkı gösterir:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Bunu biraz daha karmaşık, ancak en önemli nokta olan `null` belirlenmemiş bir durum kavramını temsil eder.
> 
> Artık ve sonra tam olarak ne zaman bir değer null ve boş bir dize olduğunda anlamak önemlidir. Kodunda *AddMovie* sayfası, metin kutuları değerlerini kullanarak elde `Request.Form["title"]` ve benzeri. (Düğmesine tıklamadan önce) sayfa ilk çalıştığında, değerini `Request.Form["title"]` null. Ancak, formun gönderdiğinizde `Request.Form["title"]` değerini alır `title` metin kutusu. Belirgin değildir, ancak boş bir metin kutusu boş değil; Bunu yalnızca boş bir dize var. Bu nedenle kod yanıt düğme olarak çalıştığında tıklayın, `Request.Form["title"]` boş bir dize içinde yok.
> 
> Bu ayrım neden önemlidir? Oluştururken *filmler* tablo açıkça söylediğiniz alanların null olabileceğini. Ancak burada bir giriş formunu yeni filmler için sahip ve alanları boş bırakılması. Türe veya yıl değerlerini gerekmedi yeni filmler kaydetmeye çalıştığında şikayet veritabanına makul beklenir. Ancak, noktasıdır &mdash; olsa bile bu metin kutularını boş bırakın, değerler null dahil değildir; boş dizeler oldukları. Sonuç olarak, yeni bir film veritabanı boş bu sütunlar ile kaydedebilir &mdash; ancak boş değil! &mdash; değerler. Bu nedenle, kullanıcıların kullanıcı girişini doğrulama tarafından yapabileceğiniz boş bir dize paylaşmayın emin olmalısınız.


### <a name="the-validation-helper"></a>Doğrulama Yardımcısı

ASP.NET Web sayfaları içeren bir yardımcı &mdash; `Validation` Yardımcısı &mdash; kullanıcılar gereksinimlerinizi karşılayan verileri girin emin olmak için kullanabilirsiniz. `Validation` Yardımcısı NuGet, önceki bir öğreticide Gravatar Yardımcısı yüklü biçimini kullanarak bir paket olarak yüklemeniz gerekmez, ASP.NET Web sayfaları için yerleşik olarak Yardımcıları biridir.

Kullanıcı girişini doğrulamak için aşağıdakileri:

- Kod sayfasında metin kutularında değerleri gerektirir istediğinizi belirtmek için kullanın.
- Bir test, böylece yalnızca her şeyin düzgün bir şekilde doğrulanırsa film bilgileri veritabanına eklenen kodun içine yerleştirin.
- Kod hata iletilerini görüntülemek için işaretleme ekleyin.

Kod bloğu içinde *AddMovie* sayfasında, sağa yukarı üst değişken bildirimlerini önce aşağıdaki kodu ekleyin:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Çağırmanızı `Validation.RequireField` her alan için bir kez (`<input>` öğesi) bir giriş gerektiren istediğiniz. Burada gördüğünüz gibi her çağrı bir özel hata iletisi de ekleyebilirsiniz. (Biz, istediğiniz herhangi bir şey buraya koyabilirsiniz olduğunu göstermek için iletilerini değiştirilen.)

Bir sorun varsa, veritabanına eklenen yeni film bilgilerinin önlemek isteyebilirsiniz. İçinde `if(IsPost)` engelleme, kullanın `&&` (testleri başka bir koşul eklemek için mantıksal ve) `Validation.IsValid()`. Bitirdiğinizde, bütün `if(IsPost)` blok şu kod gibi görünür:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Kullanarak kaydettiğiniz alanlarının herhangi bir doğrulama hatası varsa `Validation` yardımcıyı `Validation.IsValid` yöntem false döndürür. Ve geçersiz film giriş yok veritabanına eklenecek şekilde bu durumda, söz konusu bloğu içindeki kod hiçbiri çalıştırılır. Ve Elbette yönlendirilirsiniz değil *filmler* sayfası.

Doğrulama kodu içeren tam bir kod bloğu, artık şu örnekteki gibi görünür:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Doğrulama hataları görüntüleme

Son adım, herhangi bir hata iletisi görüntülemektir. Bir Özet veya her ikisi de görüntüleyebilir veya tek tek her doğrulama hatası iletilerini görüntüleyebilirsiniz. Nasıl çalıştığını görebilmeniz için Bu öğretici için her ikisi de yaparsınız.

Her yanındaki `<input>` doğrulamakta öğe, çağrı `Html.ValidationMessage` yöntemi ve adını geçirin `<input>` öğesi doğrulanıyor. Eklediğiniz `Html.ValidationMessage` yöntemi sağ istediğiniz görüntülenecek hata iletisi. Sayfa çalıştığında, `Html.ValidationMessage` yöntemi oluşturur bir `<span>` doğrulama hatası nereye öğesi. (Herhangi bir hata varsa `<span>` öğesi işlenir, yoktur ancak metin içinde.)

Sayfa biçimlendirmeyi değiştirin, böylece içerdiği bir `Html.ValidationMessage` her üç yöntemi `<input>` gibi öğeler sayfasında, bu örnekte:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Özet nasıl çalıştığını görmek için ayrıca aşağıdaki işaretlemeyi ekleyin ve hemen sonra kod `<h1>Add a Movie</h1>` öğeyi sayfada:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Varsayılan olarak, `Html.ValidationSummary` yöntemi, tüm doğrulama iletilerinin bir listede görüntüler (bir `<ul>` içinde öğesi bir `<div>` öğesi). Olduğu gibi `Html.ValidationMessage` yöntemi, biçimlendirme doğrulama özeti için her zaman işlenir; herhangi bir hata varsa, liste öğesi oluşturulur.

Özet kullanarak yerine doğrulama iletileri görüntülemek için alternatif bir yolu olabilir `Html.ValidationMessage` her alana özgü hata görüntülemek için yöntemi. Veya, hem Özet hem de ayrıntılarını kullanabilirsiniz. Ya da `Html.ValidationSummary` genel bir hata görüntüler ve ardından tek yöntemi `Html.ValidationMessage` ayrıntılarını görüntülemek için çağrıları.

Tam sayfa artık şu örnekteki gibi görünür:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

İşte bu kadar. Bir filmi ekleme ancak bir veya daha fazla alan bırakarak artık sayfayı test edebilirsiniz. Bunu yaptığınızda, görüntüler aşağıdaki hatayı görürsünüz:

![Doğrulama hatası iletilerini gösteren film Sayfası Ekle](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Doğrulama hatası iletilerini stil oluşturma

Hata iletileri vardır, ancak bunlar gerçekten çok iyi göze yoksa görebilirsiniz. Hata iletileri, ancak stilini belirlemek için kolay bir yolu yoktur.

Tarafından görüntülenen tek hata iletilerini biçimlendirmek için `Html.ValidationMessage`, adlandırılmış bir CSS stil sınıfı oluşturma `field-validation-error`. Doğrulama Özeti Görünümü tanımlamak için adlandırılmış bir CSS stil sınıfı oluşturma `validation-summary-errors`.

Bu teknik nasıl çalıştığını görmek için ekleme bir `<style>` öğe içinde `<head>` sayfasının bölümünde. Ardından adlı stil sınıflarını tanımlayın `field-validation-error` ve `validation-summary-errors` aşağıdaki kurallar içerir:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Ayrı bir stil bilgileri büyük olasılıkla normalde koyabilirsiniz *.css* dosya, ancak kolaylık olması için bunları sayfasında şimdilik koyabilirsiniz. (Daha sonra Bu öğretici kümesinde, ayrı bir CSS kurallarını taşırsınız *.css* dosyası.)

Bir doğrulama hatası varsa `Html.ValidationMessage` yöntemi oluşturur bir `<span>` içeren öğe `class="field-validation-error"`. O sınıf için bir stil tanımını ekleyerek, ileti benzer yapılandırabilirsiniz. Hatalar, varsa `ValidationSummary` yöntemi öznitelik benzer şekilde dinamik olarak işler `class="validation-summary-errors"`.

Sayfayı yeniden çalıştırın ve alanları birkaç kasıtlı olarak bırakın. Hatalar daha belirgin görüntüleniyor. (Aslında, overdone ancak yalnızca neler yapabileceğinizi gösterecek şekilde olmasıdır.)

![Stil uygulanmış doğrulama hatalarını gösteren film Sayfası Ekle](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Filmler sayfasına bağlantı ekleme

Bir son adımdır almak uygun hale getirmek için *AddMovie* özgün film listenin sayfasından.

Açık *filmler* yeniden sayfa. Kapatma sonrasında `</div>` izleyen etiketi `WebGrid` Yardımcısı, aşağıdaki işaretlemeyi ekleyin:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Daha önce gördüğünüz gibi ASP.NET yorumlar `~` Web sitesinin kök olarak işleci. Kullanmak zorunda değilsiniz `~` işleci; biçimlendirme kullanabileceğinizi `<a href="./AddMovie">Add a movie</a>` veya HTML anlayan yolunu tanımlamak için başka bir şekilde. Ancak `~` işleci, iyi bir genel yaklaşım Razor sayfaları için bağlantılar oluşturduğunuzda, sitenin daha esnek hale getirdiği için — geçerli sayfa bir alt klasöre taşırsanız, bağlantıyı yine de gidin *AddMovie* sayfası. (Unutmayın `~` işleci yalnızca çalışır *.cshtml* sayfaları. ASP.NET anlar, ancak standart HTML değildir.)

İşiniz bittiğinde çalıştırma *filmler* sayfası. Bunun gibi bu sayfası görünür:

!['Filmler Ekle' sayfasına filmler sayfası](entering-data/_static/image7.png)

Tıklayın **film ekleme** için giden emin olmak için bağlantı *AddMovie* sayfası.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide zaten veritabanında olan verileri düzenleme kullanıcıların öğreneceksiniz.

## <a name="complete-listing-for-addmovie-page"></a>Tam listesi için AddMovie sayfası

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web programlama Razor söz dizimini kullanarak giriş](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL INSERT INTO deyimi](http://www.w3schools.com/sql/sql_insert.asp) W3Schools sitesinde
- [ASP.NET Web uygulamasında kullanıcı girdisi doğrulama sayfaları sitelerini](https://go.microsoft.com/fwlink/?LinkId=253002). İle çalışma hakkında daha fazla bilgi `Validation` Yardımcısı.

> [!div class="step-by-step"]
> [Önceki](form-basics.md)
> [İleri](updating-data.md)
