---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: ASP.NET Web sayfaları - HTML formuyla ilgili temel kavramlar ile tanışın | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Giriş bir formun nasıl oluşturulacağını ve ASP.NET Web sayfaları (Razor) kullandığınızda, kullanıcının giriş işlemek nasıl temelleri gösterilir. Ve şimdi...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 28385ac2244ab0bfb38ee5fcbc64e6e11804612b
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021683"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>ASP.NET Web sayfaları - HTML formuyla ilgili temel kavramlar ile tanışın
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğreticide, Giriş bir formun nasıl oluşturulacağını ve ASP.NET Web sayfaları (Razor) kullandığınızda, kullanıcının giriş işlemek nasıl temelleri gösterilir. Ve bir veritabanı kendinizi, veritabanında belirli filmler bulmalarına olanak form becerilerinizi kullanacaksınız. Bu seriyi aracılığıyla bitirdiğinizi [görüntüleme veri kullanarak ASP.NET Web sayfalarına giriş](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Öğrenecekleriniz:
> 
> - Standart HTML öğeleri kullanılarak, bir form oluşturma
> - Ndaki bir forma kullanıcı okuma giriş.
> - Seçerek bir arama'yı kullanarak verileri alır, kullanıcı terimi, bir SQL sorgusu oluşturma sağlar.
> - Sayfasında "ne bir kullanıcı girilen unutmayın" alanların nasıl.
>   
> 
> Ele alınan özelliklerin/teknolojiler:
> 
> - `Request` Nesne.
> - SQL `Where` yan tümcesi.


## <a name="what-youll-build"></a>Ne oluşturacaksınız

Önceki öğreticide, bir veritabanı oluşturdunuz, veri eklendiğinde ve ardından kullanılan `WebGrid` verileri görüntülemek için yardımcı. Bu öğreticide, ayarlanmış başlık girdiğiniz herhangi bir sözcük içeriyor ya da belirli bir türe, filmler bulmanıza olanak tanıyan bir arama kutusu ekleyeceksiniz. (Örneğin, sizin tarzı "Action" veya "Harry" veya "Adventure" ayarlanmış başlık içerdiği tüm filmlere bulmak mümkün olacaktır)

Bu öğretici ile işiniz bittiğinde bunun gibi bir sayfa gerekir:

![Türe ve başlık araması filmler sayfası](form-basics/_static/image1.png)

Sayfa listesi parçası son öğretici ile aynı olan &mdash; bir kılavuz. Fark, kılavuz için Aranan yalnızca filmler göstereceğini olacaktır.

## <a name="about-html-forms"></a>HTML formları hakkında

(Deneyim arasındaki farkı ve HTML form oluşturma ile kendinizi varsa `GET` ve `POST`, bu bölümü atlayabilirsiniz.)

Kullanıcı girişi öğelerinin bir biçimde &mdash; metin kutularını, düğmeleri, radyo düğmeleri, onay kutuları, açılan listeler ve benzeri. Kullanıcılar bu denetimleri doldurun veya seçimleri yapın ve ardından bir düğmeye tıklayarak form gönderilemiyor.

Bir form temel HTML sözdizimini Bu örnekle gösterilmiştir:

[!code-html[Main](form-basics/samples/sample1.html)]

Bu işaretleme, bir sayfa çalıştığında, bu çizim gibi görünüyor, basit bir form oluşturur:

![Tarayıcıda işlenmiş olarak temel HTML formu](form-basics/_static/image2.png)

`<form>` Öğesi gönderilecek HTML öğeleri alır. (Öğeleri sayfaya ekleyin, ancak bunları içine yerleştirebilirsiniz unuttunuz hale getirmek için kolay bir hata olduğu bir `<form>` öğesi. Bu durumda hiçbir şey gönderilir.) `method` Özniteliği, kullanıcı girişini göndermek nasıl tarayıcı söyler. Bu ayar `post` bir güncelleştirme sunucusunda veya çok gerçekleştiriyorsanız `get` yalnızca sunucudan verilerin getirilmesi durumunda.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST ve HTTP fiili güvenliği**
> 
> HTTP, tarayıcılar ve sunucuları hakkında bilgi alışverişi yapmak için kullandığı protokole temel işlemleri çok kolaydır. Tarayıcılar, sunucuya isteklerinde bulunmak için yalnızca birkaç fiilleri kullanın. Web için kod yazdığınızda, bu fiilleri ve bunları nasıl tarayıcı ve sunucu kullanılacağını anlamak yararlıdır. En sık kullanılan fiilleri bunlar ve uzakta:
> 
> - `GET`. Tarayıcı, bir sunucudan almak için bu fiil kullanır. Örneğin, bir URL'sini tarayıcınıza yazın, tarayıcı gerçekleştirir bir `GET` işlemi istediğiniz sayfa istemek için. Grafik sayfası içeriyorsa, tarayıcı ek gerçekleştirir `GET` görüntüleri almak için operations. Varsa `GET` işlemine sahip bilgilerini sunucuya geçirmek, bilgileri sorgu dizesini URL'ye bir parçası olarak geçirilir.
> - `POST`. Tarayıcı gönderen bir `POST` eklenebilir veya sunucu üzerinde değiştirilmiş verileri göndermek için isteği. Örneğin, `POST` fiil, veritabanı kaydı oluşturma veya var olanları değiştirmek için kullanılır. Çoğu zaman, ne zaman bir formu doldurun ve Gönder düğmesini tıklatın, tarayıcı gerçekleştiren bir `POST` işlemi. İçinde bir `POST` işlemi, sunucuya geçirilen veriler, sayfanın gövdesi içinde.
> 
> Olan bu eylemler arasında önemli bir ayrımdır bir `GET` işlemi sunucu üzerindeki herhangi bir ayarı değiştirmek için beklenen değildir — veya biraz daha soyut bir şekilde koymak için bir `GET` işlemi sunucunun durumunda bir değişiklik neden olmaz. Gerçekleştirebileceğiniz bir `GET` istediğiniz ve bu kaynakları değiştirme gibi birden çok kez aynı kaynaklara işlemi. (A `GET` işlemi genellikle söylediğiniz "güvenli" ya da teknik bir terim kullanılacak olan *ıdempotent*.) Buna karşılık, doğal olarak, bir `POST` istek sunucusundaki bir şey işlemi gerçekleştirdiğiniz her zaman değiştirir.
> 
> İki örnek Bu ayrım göstermeye yardımcı olur. Bing veya Google gibi bir altyapı kullanarak bir arama yaparken oluşan bir metin kutusundan bir formu doldurun ve ardından, arama düğmesine tıklayın. Tarayıcı gerçekleştiren bir `GET` işlemle URL'nin bir bölümü olarak geçirilen kutusuna girdiğiniz değer. Kullanarak bir `GET` işlemi sunucudaki tüm kaynakların bir arama işlemi değişmediğinden bu tür bir form uygundur için bu yalnızca bilgi getirir.
> 
> Şimdi, çevrimiçi bir şey sıralama işlemini göz önünde bulundurun. Sipariş ayrıntıları girin ve ardından Gönder düğmesine tıklayın. Bu işlem bir `POST` istek işlemi sunucusunda yeni bir sipariş kaydı, bir değişiklik hesap bilgilerinizi ve belki de diğer birçok değişikliği gibi değişiklikleri neden. Farklı `GET` işlemi yinelenemez, `POST` isteği — oluşturmadıysanız, isteği yeniden gönderildi her zaman yeni bir sipariş sunucuda karşılaşırsınız. (Bu gibi durumlarda, Web siteleri genellikle bir gönderme düğmesi birden çok kez'i sizi uyarır ve böylece form yanlışlıkla yeniden yoksa Gönder düğmesini devre dışı bırakır.)
> 
> Bu öğretici sırasında her ikisi de kullanacağınız bir `GET` işlemi ve `POST` HTML formlarla çalışmak için işlemi. Her durum kullandığınız fiili uygun olanı İşte açıklayacağız.
> 
> (HTTP fiilleri hakkında daha fazla bilgi için bkz: [yöntemi tanımları](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) W3C sitesinde makale.)


Çoğu kullanıcı girişi öğelerinin HTML'dir `<input>` öğeleri. Gibi görünürler `<input type="type" name="name">,` burada *türü* istediğiniz kullanıcı giriş denetim türünü belirtir. Sık Kullanılanlardan bu öğeler şunlardır:

- Metin kutusu: `<input type="text">`
- Onay kutusu: `<input type="check">`
- Radyo düğmesi: `<input type="radio">`
- Düğme: `<input type="button">`
- Gönder düğmesi: `<input type="submit">`

Ayrıca `<textarea>` öğesi bir çok satırlı metin kutusu oluşturma ve `<select>` öğesi bir açılan liste veya kaydırılabilir bir liste oluşturmak için. (HTML hakkında daha fazla öğeleri oluşturmak için bkz: [HTML formları ve giriş](http://www.w3schools.com/html/html_forms.asp) W3Schools sitesinde.)

`name` Kısa süre içinde anlatıldığı gibi daha sonra öğenin değeri nasıl elde edecekleriniz adı olduğu için öznitelik çok önemlidir.

Kullanıcının girişi ile neler sayfasında geliştirici olarak size ilginç parçasıdır. Bu öğelerle ilişkili hiçbir yerleşik davranış yoktur. Bunun yerine, kullanıcı girilebilen veya değerlerini alın ve bunlarla bir şey gerekir. Bu öğreticide öğrenecekleriniz olmasıdır.

> [!TIP] 
> 
> **HTML5 ve girdi biçimleri**
> 
> Biliyor olabilirsiniz gibi HTML geçiş durumunda ve en son sürümünü (HTML5) bilgi girmek kullanıcılar için daha sezgisel yolu için destek içerir. Örneğin, HTML5'te istediğiniz kullanıcının bir tarih girin (sayfa Geliştirici), sayfa söyleyebilirsiniz. Tarayıcı daha sonra otomatik olarak bir tarih el ile girmek kullanıcının gerektirmek yerine bir takvim görüntüleyebilirsiniz. Ancak, HTML5, yenilikler ve tüm tarayıcılarda henüz desteklenmiyor.
> 
> ASP.NET Web sayfaları, kullanıcının tarayıcı mu için toplasa bile, giriş HTML5 destekler. Yeni öznitelikler için hakkında bir fikir için `<input>` HTML5, öğesinin [HTML &lt;giriş&gt; öznitelik türü](http://www.w3schools.com/html/html_form_input_types.asp) W3Schools sitesinde.


## <a name="creating-the-form"></a>Form Oluşturma

Webmatrix'te, içinde **dosyaları** çalışma alanında, açık *Movies.cshtml* sayfası.

Kapatma sonrasında `</h1>` etiketi ve açmadan önce `<div>` etiketi `grid.GetHtml` çağrı, aşağıdaki işaretlemeyi ekleyin:

[!code-html[Main](form-basics/samples/sample2.html)]

Bu işaretleme adlı bir metin kutusu olan bir form oluşturur `searchGenre` ve bir Gönder düğmesi. Metin kutusu ve düğme içine alınmıştır göndermek bir `<form>` öğesi olan `method` özniteliği `get`. (Metin kutusu yoksa ve Gönder düğmesi içinde unutmayın bir `<form>` öğesi, düğmeye tıkladığınızda bir şey gönderilir.) Kullandığınız `GET` fiili form oluşturduğumuzdan Burada, sunucu üzerinde değişiklik değil; yalnızca bir arama sonuçları. (Önceki öğreticide kullandığınız bir `post` değişiklikleri sunucuya nasıl gönderdiğiniz olan yöntem. Sonraki öğreticide yeniden görürsünüz.)

Sayfayı çalıştırın. Form için herhangi bir davranış tanımladıysanız olsa da, nasıl göründüğünü görebilirsiniz:

![Arama kutusunu Tarz filmler sayfası](form-basics/_static/image3.png)

"Komedi." gibi bir metin kutusuna bir değer girin Ardından **arama Tarz**.

Sayfanın URL'sini not alın. Ayarladığınız çünkü `<form>` öğenin `method` özniteliğini `get`, girdiğiniz değer artık böyle bir URL'ye sorgu dizesi parçasıdır:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Okuma Form değerleri

Veritabanı verilerini alır ve sonuçları bir kılavuzda görüntüleyen bazı kod sayfası zaten var. Artık arama terimini içeren bir SQL sorgusu çalıştırabilmeniz için metin kutusunun değerini okur bazı kod eklemeniz gerekir.

Formun yöntemi kümesine çünkü `get`, aşağıdaki gibi kod kullanarak metin kutusuna girilen değerin edinebilirsiniz:

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString` Nesne ( `QueryString` özelliği `Request` nesnesi) bir parçası olarak gönderilen öğe değerlerini içeren `GET` işlemi. `Request.QueryString` Özelliği içeren bir *koleksiyon* (liste) biçiminde gönderilen değerler. Herhangi bir değer almak için istediğiniz öğe adını belirtin. İşte bu nedenle sahip olmanız bir `name` özniteliği `<input>` öğesi (`searchTerm`), metin kutusu oluşturur. (Hakkında daha fazla bilgi için `Request` nesne, bkz: [kenar](#BKMK_TheRequestObject) daha sonra.)

Metin kutusunun değerini okumak basit bir işlemdir. Ancak kullanıcı hiçbir şey hiç metin kutusuna girmediniz ancak tıklanan **arama** aramak için bir şey olmadığı yine de bu öğesini yoksayabilirsiniz.

Aşağıdaki kod bu koşullar uygulamak nasıl oluşturulduğunu gösteren bir örnektir. (Bu birazdan gerçekleştirirsiniz; Bu kod henüz eklemeniz gerekmez.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Bu yolla test ayırır:

- Değerini almak `Request.QueryString["searchGenre"]`, yani içine girilen değerin `<input>` adlı bir öğe `searchGenre`.
- Bunu kullanarak boş olup olmadığını öğrenmek `IsEmpty` yöntemi. Bu yöntem, bir şey (örneğin, bir form öğesi) bir değeri içerip içermediğini belirlemek için standart bir yoludur. Ancak yalnızca varsa gerçekten, ilgilendiğiniz *değil* boş, bu nedenle...
- Ekleme `!` işleci önüne `IsEmpty` test edin. ( `!` İşleci mantıksal değil anlamına gelir).

Düz İngilizce olarak tüm `if` aşağıdaki koşul çevirir: *formun searchGenre öğe sonra boş değilse...*

Bu bloğu aşama arama terimi kullanan bir sorgu oluşturmak için ayarlar. Bu sonraki bölümde gerçekleştirirsiniz.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **İstek nesnesi**
> 
> `Request` Nesnesi bir sayfa istenen veya gönderildiğinde, uygulamanız için tarayıcı gönderdiği tüm bilgileri içerir. Bu nesne kullanıcının sağladığı gibi metin kutusu değerleri veya karşıya yüklenecek dosyayı herhangi bir bilgi içerir. Tanımlama bilgileri, URL sorgu dizesi (varsa), değerleri gibi ek bilgi için her türlü ayrıca kullanarak kullanıcı, tarayıcıda ayarlanmış olan dillerin listesini tarayıcı türü, çalışan sayfanın dosya yolu ve dahası.
> 
> `Request` Nesnesi bir *koleksiyon* (listesi) değerleri. Tek bir değer koleksiyonu dışında adını belirterek alın:
> 
> `var someValue = Request["name"];`
> 
> `Request` Nesne aslında birkaç alt kümeleri sunar. Örneğin:
> 
> - `Request.Form` gönderilen içindeki öğelerden değerleri size `<form>` istek öğesi bir `POST` istek.
> - `Request.QueryString` size, URL'nin sorgu dizesinde yalnızca değerleri sağlar. (Gibi bir URL içinde `http://mysite/myapp/page?searchGenre=action&page=2`, `?searchGenre=action&page=2` bölümdür URL sorgu dizesi.)
> - `Request.Cookies` Koleksiyon tarayıcı gönderildiğini tanımlama bilgilerine erişim sağlar.
> 
> Bildiğiniz bir değer almak için gönderilen biçimindedir, kullanabileceğiniz `Request["name"]`. Alternatif olarak, daha belirli sürümlerini kullanabilir `Request.Form["name"]` (için `POST` istekleri) veya `Request.QueryString["name"]` (için `GET` istekleri). Elbette, *adı* almak için öğe adıdır.
> 
> Almak istediğiniz öğe adı koleksiyon içinde benzersiz olması gerekir. İşte bu `Request` nesnesi sağlar kümeleri ister `Request.Form` ve `Request.QueryString`. Sayfanız adlı bir form öğesi içerdiğini varsayın `userName` ve *ayrıca* adlı bir tanımlama bilgisi içeren `userName`. Alırsanız `Request["userName"]`, belirsiz olduğu form değeri veya tanımlama bilgisinin isteyip istemediğiniz. Ancak, alırsanız `Request.Form["userName"]` veya `Request.Cookie["userName"]`, hangi değerin alma hakkında açık.
> 
> Belirli ve alt kümesini kullanmak için iyi bir uygulamadır `Request` , gibi ilginizi çeken `Request.Form` veya `Request.QueryString`. Bu öğreticide oluşturmakta olduğunuz basit sayfaları için bu büyük olasılıkla gerçekten fark yapmaz. Ancak, daha karmaşık sayfaları oluşturmak gibi Sürüm'ni kullanarak `Request.Form` veya `Request.QueryString` sayfanın bir form (veya birden çok form) içerdiğinde ortaya çıkabilecek sorunları önlemenize yardımcı olabilir tanımlama bilgileri, sorgu dizesi değerlerini ve benzeri.


## <a name="creating-a-query-by-using-a-search-term"></a>Bir arama terimi kullanarak bir sorgu oluşturma

Kullanıcı girilen arama terimini alma artık bildiğinize göre bunu kullanan bir sorgu oluşturabilirsiniz. Veritabanı dışında tüm film öğeleri almak için bu bildirimi gibi görünen bir SQL sorgusu kullanmakta olduğunuz unutmayın:

`SELECT * FROM Movies`

İçeren bir sorgu kullanmak zorunda yalnızca belirli filmler almak için bir `Where` yan tümcesi. Bu yan tümce üzerinde satırları sorgu tarafından döndürülen bir koşul ayarlamaya olanak tanır. Örnek buradadır:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Temel biçimi `WHERE column = value`. Bunun yanında, yalnızca farklı işleçlerini kullanabilirsiniz `=`gibi `>` (büyüktür), `<` (küçüktür), `<>` (eşit değildir), `<=` (küçüktür veya eşittir), vb. ne aradığınız bağlı olarak.

SQL deyimleri, merak durumunda, büyük küçük harfe duyarlı değildir &mdash; `SELECT` aynı `Select` (ve hatta `select`). Ancak, kişilerin genellikle bir SQL deyimi anahtar sözcükler gibi büyük harfle `SELECT` ve `WHERE`okunmasını kolaylaştırmak için.

### <a name="passing-the-search-term-as-a-parameter"></a>Arama terimi, bir parametre olarak geçirerek

Belirli bir türe oldukça kolaydır için arama (`WHERE Genre = 'Action'`), ancak kullanıcının girdiği herhangi Tarz için arama yapmak istiyorsanız. Bunu yapmak için arama yapmak için bir yer tutucu değeri içeren SQL sorgusu oluşturun. Bunun, bu komutu gibi görünür:

`SELECT * FROM Movies WHERE Genre = @0`

Yer tutucusu `@` karakterini izleyen sıfır. Tahmin gibi bir sorgu birden fazla yer tutucuları içerebilir ve bunlar adlı `@0`, `@1`, `@2`vb.

Sorgu ve gerçekten değer geçirin aşağıdaki gibi bir kod kullanın:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Bu kod, ne daha önce veri kılavuzunda görüntülemek için yaptığınız için benzer. Tek fark vardır:

- Sorgu bir yer tutucu içeriyor (`WHERE Genre = @0"`).
- Sorgu bir değişken içine konur (`selectCommand`); sorgu doğrudan geçirilen önce `db.Query` yöntemi.
- Çağırdığınızda `db.Query` yöntemi, geçirdiğiniz sorgu hem yer tutucusu için kullanılacak değer. (Sorgu birden fazla yer tutucuları varsa bunları geçecekse yöntemi için ayrı değerler olarak tüm.)

Bu öğelerin hepsini araya, aşağıdaki kodu alın:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Önemli!** Yer tutucuları kullanma (gibi `@0`) bir SQL komutu için değerleri geçirmek için *son derece önemli* güvenlik. Bu, değişken veri yer tutucuları olan gördüğünüz şekilde SQL komutlarını oluşturmak tek yoludur.
> 
> Hiçbir zaman bir SQL deyimi (birleştirme) hazır metin ve kullanıcıdan alma değerleri koyarak birlikte oluşturun. Bir SQL deyimi içinde kullanıcı girişi birleştirme açılır sitenize bir *SQL ekleme saldırısına* burada kötü niyetli bir kullanıcı veritabanınıza hack değerleri sayfanıza gönderir. (Daha fazla makalede [SQL ekleme](https://msdn.microsoft.com/library/ms161953.aspx) MSDN Web sitesinde.)


## <a name="updating-the-movies-page-with-search-code"></a>Arama koduyla filmler sayfanın güncelleştiriliyor

Kodda güncelleştirebilirsiniz artık *Movies.cshtml* dosya. Başlamak için sayfanın üstündeki kod bloğu içindeki kod şu kodla değiştirin:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Burada sorgu içine yerleştirdiğiniz fark `selectCommand` için ileteceksiniz değişkeni `db.Query` daha sonra. SQL deyimine bir değişkene koyarak arama yapmak için yaparsınız deyimi değiştirmenize olanak tanır.

Geri daha sonra gireceğiniz, şu iki satırı da kaldırdıktan:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Sorgu henüz çalıştırmak istemediğiniz (diğer bir deyişle, çağırma `db.Query`) ve başlatmak istemiyorsanız `WebGrid` Yardımcısı henüz ya da. Hangi SQL deyimini çalıştırmak sahip verdi sonra bunları yaparsınız.

Bu yeniden bloğundan sonra arama'yı işlemek için yeni mantığı ekleyebilirsiniz. Tamamlanan kodu aşağıdakine benzer olacaktır. Sayfanızın kod bu örnek eşleşecek şekilde güncelleştirin:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Sayfa artık şu şekilde çalışır. Sayfa her çalıştırıldığında kod veritabanı açılır ve `selectCommand` değişkenini ayarlamak için tüm kayıtları alır SQL deyimini `Movies` tablo. Kod ayrıca başlatır `searchTerm` değişkeni.

Ancak, geçerli istek için bir değer içeriyorsa `searchGenre` öğesi, kod kümeleri `selectCommand` farklı bir sorgu için — yani birine içeren `Where` bir türe için aranacak yan tümcesi. Ayrıca ayarlar `searchTerm` için her şeyi (hiçbir şey olabilir) arama kutusu için geçirildi.

Bağımsız olarak SQL deyimi yer `selectCommand`, ardından kod çağırır `db.Query` sorguyu çalıştırmak için hangi iletmeden bulunduğu `searchTerm`. Yoksa hiçbir şey `searchTerm`, bu durumda olmadığı için parametre değeri geçirmek için bir önemi `selectCommand` yine de.

Son olarak, kod başlatır `WebGrid` önceden olduğu gibi sorgu sonuçlarını kullanarak Yardımcısı.

SQL deyimi koyarak görebilirsiniz ve değişkenler, arama terimini esneklik için kod ekledik. Bu öğreticinin ilerleyen bölümlerinde anlatıldığı gibi temel bu çerçeveyi kullanabilirsiniz ve aramalar farklı türleri için mantık ekleme tutun.

## <a name="testing-the-search-by-genre-feature"></a>Türe göre arama özelliğini test etme

Webmatrix'te, çalıştırma *Movies.cshtml* sayfası. Metin kutusu Tarz için sayfayı görürsünüz.

Test kayıtlarınızı birine girdikten sonra tıklayın Tarz girin **arama**. Bu süre, o Tarz eşleşen filmler listesi görürsünüz:

![Tarzı 'Comedies' aradıktan sonra listeleyen filmler sayfası](form-basics/_static/image4.png)

Farklı bir türe girin ve tekrar arama yapın. Arama büyük/küçük harfe duyarlı değildir görebilmeniz için tüm tümü büyük harf veya harf kullanarak Tarz girmeyi deneyin.

## <a name="remembering-what-the-user-entered"></a>"Hangi kullanıcının girdiği hatırlamak"

Girilen bir türe ve tıklanan sonra fark etmiş **arama Tarz**, bir listesi için bu tarz gördünüz. Ancak, arama metin kutusu boş &mdash; girilen diğer bir deyişle, unutmayın kaydetmedi.

Bu davranış neden meydana geldiğini anlamak önemlidir. Bir sayfa gönderdiğinizde, tarayıcı web sunucusuna bir istek gönderir. ASP.NET isteği aldığında, sayfa yeni bir örneğini oluşturur, kod içinde çalışır ve sayfa tarayıcıya'ı yeniden işler. Aslında, ancak sayfa kendisinin önceki bir sürümüyle yalnızca çalışmakta olduğunuz bilmez. BT'nin, bazı olan isteği alındı bilir tüm veri içinde oluşturur.

Her zaman bir sayfa istemek &mdash; ilk kez mi, göndererek &mdash; yeni bir sayfa karşılaşacağınız. Web sunucusu son isteğinizin belleğe sahip değil. ASP.NET diğerinden yapar ve tarayıcı diğerinden yapar. Yalnızca sayfanın ayrı Bu örnekler arasında aralarında aktaran herhangi bir veri bağlantısıdır. Örneğin, bir sayfa gönderirseniz, yeni sayfa örnek önceki örneği tarafından gönderildiği form verileri alabilirsiniz. (Sayfalar arasında veri aktarmak için başka bir yolu, tanımlama bilgileri kullanmaktır.)

Bu durumu tanımlamak için bir biçimsel web sayfaları olduğunuzu yöntemdir *durum bilgisiz*. Web sunucuları ve sayfaların ve sayfa öğeleri sayfasının önceki durumu hakkındaki tüm bilgileri bulundurmaz. Web, tek tek istekler için durum koruma hızla genellikle belki de binlerce işleyen web sunucularının kaynakları bile yüz binlerce, saniye başına istek harcadıklarını çünkü bu şekilde tasarlanmıştır.

İşte bu nedenle metin kutusu boş. Sayfa gönderdikten sonra ASP.NET sayfası yeni bir örneğini oluşturulur ve işaretleme ve kod çalıştı. Olan herhangi bir öğe, metin kutusuna bir değer koymak için ASP.NET söyledi koddaki. Bu nedenle ASP.NET herhangi bir şey olmadı ve metin kutusunda bir değer olmadan işlenir.

Bu sorunu aşmak gerçekten kolay bir yolu yoktur. Metin kutusuna girdiğiniz Tarz *olduğu* kodda kullanabileceğiniz &mdash; içinde `Request.QueryString["searchGenre"]`.

Metin kutusu için biçimlendirme güncelleştirme böylece `value` öznitelik değerini alır `searchTerm`, bu örnekte ister:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Bu sayfada, ayrıca ayarladığınız `value` özniteliğini `searchTerm` girdiğiniz değişken türü içerdiğinden değişken. Ancak kullanarak `Request` nesne ayarlayın `value` özniteliği bu görevi gerçekleştirmek için standart bir yol gösterildiği gibi. (Hatta istediğiniz Bunu yapmak varsayılarak &mdash; bazı durumlarda, sayfayı oluşturmak isteyebilirsiniz *olmadan* alanlarındaki değerleri. Tüm uygulamanızla neler olduğunu bağımlı.)

> [!NOTE]
> "Parola için kullanılan bir metin kutusunun değerini hatırlayamıyorsunuz". Bu kod kullanarak bir parola alanını doldurmak ulaşmasını sağlamak için bir güvenlik açığı olacaktır.


Sayfayı yeniden çalıştırın, bir türe girin ve tıklayın **arama Tarz**. Bu süre yalnızca, arama sonuçlarını görmek, ancak son girilen metin kutusunu hatırlar:

![Sayfa gösteren metin kutusuna 'önceki giriş anımsanacak '](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Başlık herhangi bir sözcük arama

Artık tüm Tarz için arama yapabilirsiniz ancak arama için bir başlık isteyebilirsiniz. Bunun yerine, herhangi bir yeri içinde bir başlık görünür bir sözcük arayabilirsiniz arama yaparken bir başlık tam doğru elde etmek zordur. SQL'de Bunu yapmak için kullandığınız `LIKE` işleci ve söz dizimi aşağıdaki gibidir:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Bu komut, "adventure" başlıklarını içeren tüm filmlere alır. Kullanırken `LIKE` işleci, joker karakter içeren `%` arama teriminin bir parçası olarak. Arama `LIKE 'adventure%'` "'adventure' ile başlayan" anlamına gelir. (Teknik olarak, "Dize 'herhangi bir şey tarafından izlenen adventure'." anlamına gelir) Benzer şekilde, arama terimi `LIKE '%adventure'` "hiçbir şey 'adventure' dize tarafından izlenen", "'adventure' görmektesiniz" söylemek için başka bir yol olduğu anlamına gelir.

Arama terimi `LIKE '%adventure%'` bu nedenle "ile"adventure' başlığı herhangi bir yerindeki."anlamına gelir (Teknik olarak, "her şey 'adventure' herhangi bir şey tarafından izlenen, ardından başlık,.")

İçinde `<form>` öğesi kapanış hemen altında aşağıdaki işaretlemeyi ekleyin `</div>` etiketi Tarz arama (yalnızca kapatmadan önce `</form>` öğesi):

[!code-html[Main](form-basics/samples/sample10.html)]

Birleştirmeniz gerekir. Bu arama işlemek üzere kod Tarz arama koda benzer `LIKE` arama. Bu sayfanın üst kısmındaki kod bloğunun içine ekleyin `if` hemen sonrasına block `if` bloğu Tarz arama için:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Arama kullanması hariç, bu kod, daha önce gördüğünüz aynı mantığı kullanır. bir `LIKE` işleci ve kod koyar "`%`" önce ve sonra arama terimi.

Başka bir arama sayfasına eklemek kolay nasıl dikkat edin. Tüm yapmak zorunda şöyleydi:

- Oluşturma bir `if` ilgili arama kutusuna bir değere sahip olup olmadığını görmek için test edilen blok.
- Ayarlama `selectCommand` değişken için yeni bir SQL deyimi.
- Ayarlama `searchTerm` değişken için değeri sorguya iletin.

Başlık araması için yeni mantığı içeren tam kod bloğunu şu şekildedir:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Bu kodun yaptığı bir özeti aşağıda verilmiştir:

- Değişkenleri `searchTerm` ve `selectCommand` üstünde başlatılır. Uygun SQL komut kullanıcı sayfasında yaptığı şirket tabanlı ve (varsa) uygun bir arama terimi bu değişkenleri ayarlamak için oluşturacağız. Varsayılan arama, tüm filmlere veritabanından almaya ilişkin basit bir durumdur.
- Testler için `searchGenre` ve `searchTitle`, kod kümeleri `searchTerm` aramak istediğiniz değeri. Bu kod blokları de ayarlanmış `selectCommand` , arama için uygun bir SQL komutu.
- `db.Query` Yöntemi çağrıldığında yalnızca bir kez, içinde hangi komutu çalıştırırsam SQL kullanarak `selectedCommand` ve değer `searchTerm`. (Hiçbir tarz ve başlık sözcük yok), arama terimi yok ise değerini `searchTerm` boş bir dizedir. Bu durumda, sorgu parametresi gerektirmediği ancak, fark etmez.

## <a name="testing-the-title-search-feature"></a>Başlık arama özelliğini test etme

Artık tamamlanmış arama sayfası test edebilirsiniz. Çalıştırma *Movies.cshtml*.

Bir türe girip __iade **arama Tarz**. Kılavuz önce gibi bu tarz filmler görüntüler.

Bir başlık sözcük girin ve tıklayın **arama başlık**. Kılavuzun bu sözcüğü başlığında filmler görüntüler.

![Başlığında '' için arama yaptıktan sonra listeleyen filmler sayfası](form-basics/_static/image6.png)

Her iki metin kutusunu boş bırakın ve her iki düğmesine tıklayın. Tüm filmlere kılavuz görüntüler.

## <a name="combining-the-queries"></a>Sorguları birleştirme

Gerçekleştirebileceğiniz arama Dışlayıcı olduğunu fark edebilirsiniz. Her iki arama kutusu değerleri içeren bile aynı zamanda, başlık ve Tarz arama yapamazsınız. Örneğin, "Adventure" ayarlanmış başlık içeren tüm eylem filmler için arama yapamazsınız. (Tarz ve başlık için bir değer girerseniz sayfası artık, kodlanmış gibi başlığı aramasını öncelik alır.) Koşullar birleştiren bir arama oluşturmak için aşağıdaki gibi bir söz dizimi olan bir SQL sorgusu oluşturma gerekir:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Ve aşağıdaki gibi bir deyim kullanarak sorguyu çalıştırmak gerekir (yaklaşık konuşulur):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Gördüğünüz gibi çok sayıda permütasyon arama ölçütü izin vermek için mantığı oluşturma biraz karmaşık alabilirsiniz. Bu nedenle, biz burada durdurulacak.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide, bir form kullanıcıların filmler veritabanına ekleme kullanan bir sayfa oluşturacaksınız.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Tam listesi için (arama ile güncelleştirilmiş) film sayfası

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web programlama Razor söz dizimini kullanarak giriş](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL WHERE yan tümcesi](http://www.w3schools.com/sql/sql_where.asp) W3Schools sitesinde
- [Yöntem tanımlarını](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) W3C sitesinde makale

> [!div class="step-by-step"]
> [Önceki](displaying-data.md)
> [İleri](entering-data.md)
