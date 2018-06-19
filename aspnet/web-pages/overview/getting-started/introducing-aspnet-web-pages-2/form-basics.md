---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: HTML formu temelleri ASP.NET Web sayfaları - giriş | Microsoft Docs
author: tfitzmac
description: Bu öğreticide, bir giriş formun nasıl oluşturulacağını ve ASP.NET Web sayfaları (Razor) kullandığınızda kullanıcının giriş nasıl ele alınacağını temelleri gösterilir. Ve şimdi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 6f44f74774c2fa6338524987779e15f3940d1830
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898927"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>ASP.NET Web sayfalarını - HTML formu temelleri tanıtma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğreticide, bir giriş formun nasıl oluşturulacağını ve ASP.NET Web sayfaları (Razor) kullandığınızda kullanıcının giriş nasıl ele alınacağını temelleri gösterilir. Ve bir veritabanı olduğuna göre veritabanında belirli filmler bulmalarına olanak form yeteneklerinizi kullanacaksınız. Seri aracılığıyla tamamladığınızdan varsayar [giriş için görüntüleme verileri kullanarak ASP.NET Web sayfaları](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Öğrenecekleriniz:
> 
> - Standart HTML öğeleri kullanılarak bir form oluşturma
> - Kullanıcı okumak nasıl bir formda giriş anlamına gelir.
> - Seçmeli olarak bir arama kullanarak verileri, kullanıcı terimi, bir SQL sorgusu oluşturmak nasıl sağlar.
> - Sayfasında "ne kullanıcının girdiği unutmayın" alanları nasıl.
>   
> 
> Özellikler/teknolojilerini ele alınan:
> 
> - `Request` Nesnesi.
> - SQL `Where` yan tümcesi.


## <a name="what-youll-build"></a>Ne oluşturacağınız

Önceki öğreticide, bir veritabanı oluşturuldu, veri kendisine eklenmiş ve ardından kullanılan `WebGrid` verileri görüntülemek için yardımcı. Bu öğreticide, belirli bir tarzını filmler bulmanıza sağlayan veya girdiğiniz ne olursa olsun word, başlık içeren bir arama kutusu ekleyeceksiniz. (Örneğin, size tarzı "Eylem" veya "Harry" veya "Adventure.", başlık içerdiği tüm filmler bulamıyor olması)

Bu öğreticiyi tamamladığınızda, bunun gibi bir sayfa sahip olursunuz:

![Tarz ve başlık arama filmler sayfası](form-basics/_static/image1.png)

Son öğretici ile aynı sayfa listeleme parçası olan &mdash; bir kılavuz. Fark için arama kılavuz yalnızca filmler gösterecektir olacaktır.

## <a name="about-html-forms"></a>HTML formları hakkında

(Deneyimi HTML formları oluşturma ile arasındaki fark var olmadığını `GET` ve `POST`, bu bölümü atlayabilirsiniz.)

Kullanıcı girişi öğelerinin formun sahip &mdash; metin kutuları, düğmeleri, radyo düğmeleri, onay kutularını, açılan listeleri ve benzeri. Kullanıcılar bu denetimlerin doldurun veya seçim yapın ve ardından bir düğmeyi tıklatarak formu göndermeden.

Bu örnek tarafından formun temel HTML sözdizimi gösterilmiştir:

[!code-html[Main](form-basics/samples/sample1.html)]

Bu biçimlendirme bir sayfa çalıştığında, bu çizim gibi görünen basit bir form oluşturur:

![Tarayıcıda işlenmiş olarak temel HTML formu](form-basics/_static/image2.png)

`<form>` Öğesi gönderilecek HTML öğeleri alır. (Öğeleri sayfasına ekleyin, ancak bunları içine yerleştirin unuttunuz yapmak için kolay bir hata olduğundan bir `<form>` öğesi. Bu durumda, hiçbir şey gönderilir.) `method` Öznitelik kullanıcı girişini gönderme tarayıcı söyler. Bu ayar `post` bir güncelleştirme sunucusu veya çok gerçekleştiriyorsanız `get` yalnızca sunucudan veri getirme durumunda.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST ve HTTP fiili güvenliği**
> 
> HTTP, tarayıcılar ve sunucuları bilgi, exchange kullanmasını protokolü temel işlemlerde çok kolaydır. Tarayıcılar istekleri sunucularına yapmak için yalnızca birkaç fiilleri kullanın. Web için kodu yazarken bu fiilleri ve bunları nasıl tarayıcı ve sunucu kullanılacağını anlamak faydalıdır. En yaygın olarak kullanılan fiillerden bunlar ve uzakta:
> 
> - `GET`. Tarayıcı, bir sunucudan getirmek için bu fiil kullanır. Örneğin, bir URL tarayıcınıza yazın, tarayıcı gerçekleştirdiğinde bir `GET` istediğiniz sayfasını istemek için işlemi. Sayfa grafik içeriyorsa, tarayıcı ek gerçekleştirir `GET` görüntüleri alma işlemleri. Varsa `GET` işlemi sahip bilgi sunucuya geçirmek, bilgileri sorgu dizesinde URL'SİNİN bir parçası olarak geçirilir.
> - `POST`. Tarayıcı gönderir bir `POST` eklenemez veya sunucu üzerinde değiştirilmiş için veri göndermek için istek. Örneğin, `POST` fiili bir veritabanında kayıtları oluşturmak veya var olanları değiştirmek için kullanılır. Çoğu zaman, ne zaman bir formu doldurun ve Gönder düğmesine tıklayın, tarayıcı gerçekleştiren bir `POST` işlemi. İçinde bir `POST` sunucuya aktarılan veriler işlemdir sayfasının gövdesinde.
> 
> Bu eylemler arasında önemli bir fark olan bir `GET` işlemi sunucu üzerindeki herhangi bir şey değiştirmek için beklenen değildir — veya biraz daha soyut bir şekilde koymak için bir `GET` işlem sunucusunda durumda bir değişiklik neden olmaz. Gerçekleştirebileceğiniz bir `GET` aynı kaynakları sayıda istediğiniz ve bu kaynakları değiştirme işlemi. (A `GET` işlemi genellikle denirse "safe" ya da teknik bir terim kullanılacak olan *ıdempotent*.) Buna karşılık, doğal olarak, bir `POST` isteği sunucuda bir şey işlemi gerçekleştirdiğiniz her zaman değiştirir.
> 
> İki örnek Bu ayrım göstermeye yardımcı olur. Bing veya Google, gibi bir altyapısını kullanarak bir arama yaparken bir metin kutusunun oluşan bir formu doldurun ve ardından arama düğmesini tıklatın. Tarayıcı gerçekleştiren bir `GET` işlemiyle URL'SİNİN bir parçası geçirilen kutusuna girdiğiniz değer. Kullanarak bir `GET` işlemi sunucudaki tüm kaynakların bir arama işlemi değişmediğinden bu tür bir form uygundur için bu yalnızca bilgi getirir.
> 
> Şimdi, çevrimiçi bir şey sıralama işlemi göz önünde bulundurun. Sipariş ayrıntılarını doldurun ve ardından Gönder düğmesine tıklayın. Bu işlem bir `POST` isteği işlemi sunucuda yeni bir sipariş kaydı, bir değişiklik hesap bilgilerinizi ve belki de diğer birçok değişikliği gibi değişiklikler neden. Farklı `GET` işlemi, yinelenemez, `POST` isteği — olsaydı, istek yeniden gönderildi her zaman, sunucuda yeni bir sıra oluşturur. (Bu gibi durumlarda, Web siteleri genellikle bir gönderme düğmesi birden çok kez'i sizi uyarır ve böylece form yanlışlıkla yeniden yok Gönder düğmesi devre dışı bırakır.)
> 
> Bu öğretici esnasında her ikisi de kullanacağınız bir `GET` işlemi ve `POST` HTML formlarla çalışmak için işlemi. Uygun olanı kullandığınız fiil her servis talebi neden olan açıklayacağız.
> 
> (HTTP fiilleri hakkında daha fazla bilgi için bkz: [yöntemi tanımları](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) W3C sitesinde makale.)


Çoğu kullanıcı girişi öğelerinin HTML'dir `<input>` öğeleri. Neye benzediğini gösteren `<input type="type" name="name">,` nerede *türü* istediğiniz kullanıcı giriş denetiminin türünü gösterir. Bu öğeleri ortak olanlardır:

- Metin kutusu: `<input type="text">`
- Onay kutusu: `<input type="check">`
- Radyo düğmesi: `<input type="radio">`
- Button: `<input type="button">`
- Gönder düğmesi: `<input type="submit">`

De kullanabilirsiniz `<textarea>` öğenin çok satırlı metin kutusu oluşturma ve `<select>` aşağı açılan listesi veya kaydırılabilir listesi oluşturmak için öğesi. (HTML hakkında daha fazla öğeleri oluşturmak için bkz: [HTML formları ve giriş](http://www.w3schools.com/html/html_forms.asp) W3Schools sitesinde.)

`name` Özniteliği çok önemlidir, kısa süre içinde anlatıldığı gibi daha sonra öğenin değeri nasıl elde edersiniz çünkü adı.

Kullanıcının girişle ne sayfasında geliştirici olarak size ilginç parçasıdır. Bu öğelerle ilişkili hiçbir yerleşik davranış yoktur. Bunun yerine, kullanıcı girilen veya seçilen değerleri alır ve bunlarla bir şeyler gerekir. Bu öğreticide öğrenecekleriniz olmasıdır.

> [!TIP] 
> 
> **HTML5 ve girdi biçimleri**
> 
> Biliyor olabilirsiniz HTML geçiş durumunda ve en son sürümünü (HTML5) bilgi girmek kullanıcılar için daha sezgisel yolları için destek içerir. Örneğin, HTML5 ile bir tarih girmesini istediğiniz, (sayfa Geliştirici) sayfa anlayabilirsiniz. Tarayıcı daha sonra otomatik olarak bir tarih el ile girmek kullanıcının gerektiren yerine bir takvim görüntüleyebilirsiniz. Ancak, HTML5 yenidir ve tüm tarayıcılarda henüz desteklenmiyor.
> 
> ASP.NET Web sayfaları, kullanıcının tarayıcısının mu toplasa bile giriş HTML5 destekler. Yeni öznitelikler için hakkında bir fikir için `<input>` HTML5, öğesinde bkz [HTML &lt;giriş&gt; öznitelik türü](http://www.w3schools.com/html/html_form_input_types.asp) W3Schools sitesinde.


## <a name="creating-the-form"></a>Form Oluşturma

WebMatrix içinde içinde **dosyaları** çalışma alanında, açık *Movies.cshtml* sayfası.

Kapatma sonrasında `</h1>` etiketi ve açmadan önce `<div>` etiket `grid.GetHtml` çağrısı, aşağıdaki biçimlendirmeyi ekleyin:

[!code-html[Main](form-basics/samples/sample2.html)]

Bu biçimlendirme adlı bir metin kutusu olan bir form oluşturur `searchGenre` ve bir gönderme düğmesi. Metin kutusu ve düğmesi içine alınmıştır göndermek bir `<form>` öğesi, `method` özniteliği `get`. (Metin kutusu put yoksa ve Gönder düğmesi içinde unutmayın bir `<form>` öğesi, düğmeye tıkladığınızda bir şey gönderilecek.) Kullandığınız `GET` fiil form oluşturmakta olduğunuz çünkü Burada, herhangi bir değişiklik sunucuda yapmaz — yalnızca bir arama sonuçları. (Önceki öğreticide kullandığınız bir `post` değişiklikleri sunucuya gönderme nasıl olduğu yöntemi. Sonraki öğreticide yeniden göreceksiniz.)

Sayfayı çalıştırın. Form için herhangi bir davranış tanımlanan henüz rağmen nasıl göründüğünü görebilirsiniz:

![Arama kutusuna Tarz için filmler sayfası](form-basics/_static/image3.png)

"Komedi." gibi bir metin kutusuna bir değer girin Ardından **arama Tarz**.

Sayfanın URL'sini not edin. Ayarladığınız çünkü `<form>` öğenin `method` özniteliğini `get`, girdiğiniz değer artık böyle URL sorgu dizesinde parçasıdır:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Okuma Form değerleri

Veritabanı verileri alır ve sonuçları kılavuzda görüntüleyen biraz kod sayfası zaten var. Şimdi arama terimi içeren bir SQL sorgusu çalıştırabilmeniz için metin kutusunun değerini okuma biraz kod eklemeniz gerekir.

Formun yöntemi kümesine çünkü `get`, aşağıdaki gibi kod kullanarak metin kutusuna girilen değer okuyabilirsiniz:

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString` Nesne ( `QueryString` özelliği `Request` nesnesi) bir parçası olarak gönderildikleri öğelerinin değerlerini içerir `GET` işlemi. `Request.QueryString` Özelliği içeren bir *koleksiyonu* (listesi) biçiminde gönderilen değerler. Herhangi bir tek değer almak için istediğiniz öğenin adını belirtin. İşte bu nedenle sahip olmanız bir `name` özniteliği `<input>` öğesi (`searchTerm`), metin kutusu oluşturur. (Hakkında daha fazla bilgi için `Request` nesne için bkz: [kenar](#BKMK_TheRequestObject) daha sonra.)

Metin kutusunun değerini okumaya basit bir işlemdir. Ancak kullanıcı herhangi bir şey hiç metin kutusunda girmediniz ancak tıklattınız **arama** yine de olduğundan aramak için hiçbir şey o tıklatma göz ardı edebilirsiniz.

Aşağıdaki kod bu koşullar uygulamak nasıl oluşturulduğunu gösteren bir örnektir. (Bu kodu henüz eklemeniz gerekmez; bu birazdan gerçekleştirirsiniz.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Bu şekilde test parçalara ayırır:

- Değerini alın `Request.QueryString["searchGenre"]`, yani içine girilen değer `<input>` adlı öğe `searchGenre`.
- Bunu kullanarak boş olup olmadığını öğrenmek `IsEmpty` yöntemi. Bu yöntem, bir şey (örneğin, bir form öğesi) bir değer içerip içermediğini belirlemek için standart bir yoludur. Ancak yalnızca varsa, gerçekten, verdiğiniz *değil* boş, bu nedenle...
- Ekleme `!` önüne işleci `IsEmpty` sınayın. ( `!` İşleci mantıksal değil anlamına gelir).

Düz İngilizce, tüm `if` koşul aşağıdaki çevirir: *formun searchGenre öğesi sonra boş değilse...*

Bu bloğu aşama arama terimi kullanan bir sorgu oluşturmak için ayarlar. Bu sonraki bölümde gerçekleştirirsiniz.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **İstek nesnesi**
> 
> `Request` Nesnesi bir sayfa istenen veya gönderildiğinde, uygulamanız için tarayıcı gönderen tüm bilgileri içerir. Bu nesne kullanıcı sağlar metin kutusu değerleri veya karşıya yüklemek için bir dosya gibi herhangi bir bilgi içerir. Ayrıca, her türlü tanımlama bilgileri, URL sorgu dizesi (varsa), değerleri gibi ek bilgileri içeren, kullanıcının kullanarak tarayıcıda ayarlanmış olan dillerin listesini tarayıcı türü çalıştıran sayfanın dosya yolu ve çok daha fazlası.
> 
> `Request` Nesne bir *koleksiyonu* (listesi) değerleri. Tek bir değer koleksiyonu dışında adını belirterek alın:
> 
> `var someValue = Request["name"];`
> 
> `Request` Nesne gerçekten birkaç alt kümelerini kullanıma sunar. Örneğin:
> 
> - `Request.Form` gönderilen içindeki öğelerden değerleri verir `<form>` istek ise öğesi bir `POST` isteği.
> - `Request.QueryString` URL'nin sorgu dizesinde yalnızca değerleri verir. (Bir URL'de gibi `http://mysite/myapp/page?searchGenre=action&page=2`, `?searchGenre=action&page=2` bölümdür URL sorgu dizesi.)
> - `Request.Cookies` Koleksiyon tarayıcı gönderdi tanımlama bilgilerini erişmenizi sağlar.
> 
> Bildiğiniz bir değer almak için gönderilen biçimindedir, kullanabileceğiniz `Request["name"]`. Alternatif olarak, daha belirli sürümlerini kullanabilirsiniz `Request.Form["name"]` (için `POST` istekleri) veya `Request.QueryString["name"]` (için `GET` istekleri). Elbette, *adı* alınacak öğenin adıdır.
> 
> Almak istediğiniz öğenin adını kullandığınız koleksiyonu içinde benzersiz olması gerekir. İşte bu nedenle `Request` nesnesi sağlar kümeleri ister `Request.Form` ve `Request.QueryString`. Sayfanız adlı bir form öğesi içeriyor varsayalım `userName` ve *de* adlı bir tanımlama bilgisi içeren `userName`. Alırsanız `Request["userName"]`, form değer veya tanımlama bilgisinin isteyip istemediğiniz belirsiz. Ancak, alırsanız `Request.Form["userName"]` veya `Request.Cookie["userName"]`, hangi değerini almak için açık.
> 
> Belirli ve alt kümesini kullanmak için iyi bir uygulamadır `Request` , gibi ilginizi `Request.Form` veya `Request.QueryString`. Bu öğreticide oluşturduğunuz basit sayfaları için bu büyük olasılıkla gerçekten farklılığı yapmaz. Ancak, daha karmaşık sayfaları oluştururken, açık sürüm kullanarak `Request.Form` veya `Request.QueryString` sayfa bir form (veya birden çok form) varsa ortaya çıkabilecek sorunları önlemenize yardımcı olabilir tanımlama bilgileri, sorgu dizesi değerlerini ve benzeri.


## <a name="creating-a-query-by-using-a-search-term"></a>Bir arama terimi kullanarak bir sorgu oluşturma

Kullanıcının girdiği arama terimi alma bildiğinize göre bunu kullanan bir sorgu oluşturabilirsiniz. Veritabanı dışında tüm film öğeleri almak için bu bildirimi gibi görünen bir SQL sorgusu kullanmakta olduğunuz unutmayın:

`SELECT * FROM Movies`

İçeren bir sorgu kullanmak zorunda yalnızca belirli filmler almak için bir `Where` yan tümcesi. Bu yan tümcesi satırları sorgu tarafından döndürülen bir koşul ayarlamanıza olanak tanır. Örnek buradadır:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Temel biçim `WHERE column = value`. Bunun yanında, yalnızca farklı işleçleri kullanabilirsiniz `=`gibi `>` (büyük), `<` (küçüktür), `<>` (eşit değildir), `<=` (küçük veya eşittir), vs. aradığınızı bağlı olarak.

SQL deyimleri, merak durumunda büyük küçük harfe duyarlı olmayan &mdash; `SELECT` aynı `Select` (veya hatta `select`). Ancak, kişiler genellikle SQL deyimindeki anahtar sözcükler gibi büyük harfe çevirme `SELECT` ve `WHERE`okunmasını kolaylaştırmak için.

### <a name="passing-the-search-term-as-a-parameter"></a>Arama terimi parametre olarak geçirme

Belirli bir tarzını yeterince kolay için arama (`WHERE Genre = 'Action'`), ancak kullanıcının girdiği herhangi bir tarzını için arama yapmak istiyorsanız. Bunu yapmak için arama yapmak için bir yer tutucu değerini içeren SQL sorgusu oluşturun. Bu komutu gibi görünür:

`SELECT * FROM Movies WHERE Genre = @0`

Yer tutucusu `@` karakter sıfıra izlemelidir. Tahmin gibi bir sorgu birden fazla yer tutucuları içerebilir ve bunların adlı `@0`, `@1`, `@2`vb.

Sorgusu ayarlama ve gerçekte değeri geçirmek için aşağıdaki gibi bir kod kullanın:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Bu kod, ne zaten veri ızgarada gösterilecek yaptığınızı için benzer. Yalnızca farklılıklar şunlardır:

- Sorgu bir yer tutucu içerir (`WHERE Genre = @0"`).
- Sorgu bir değişkene konur (`selectCommand`); sorgu doğrudan geçirilen önce `db.Query` yöntemi.
- Çağırdığınızda `db.Query` yöntemi, geçirdiğiniz sorgu ve yer tutucusu için kullanılacak bir değer. (Birden çok yer tutucu sorgu sahipse, bunları geçip geçmeyeceğini yöntemi için ayrı değerleri olarak tüm.)

Tüm bu öğeler bir araya getirme, aşağıdaki kodu alın:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Önemli!** Yer tutucuları kullanma (gibi `@0`) bir SQL komutu değerleri geçirmektir *son derece önemli* güvenlik için. Burada, değişken veri yer tutucuları olan karşınıza SQL komutlarını oluşturmalısınız tek yolu yoludur.
> 
> Metin (birleştirme) ve kullanıcıdan Al değerleri birlikte koyarak hiçbir zaman bir SQL deyimi oluşturmak. Kullanıcı bir SQL deyimi girişine birleştirme açar, sitenize bir *SQL ekleme saldırısında* burada kötü niyetli bir kullanıcının veritabanınızı korsan saldırılarına değerleri sayfanıza gönderir. (Daha fazla bilgiyi makalesinde [SQL ekleme](https://msdn.microsoft.com/library/ms161953.aspx) MSDN Web.)


## <a name="updating-the-movies-page-with-search-code"></a>Arama koduyla filmler sayfa güncelleştiriliyor

Kodda güncelleştirebilirsiniz artık *Movies.cshtml* dosya. Başlamak için sayfanın üst kısmındaki kod bloğundaki kod bu kod ile değiştirin:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Burada sorguyu yerleştirdiğiniz farktır `selectCommand` için geçirdiğiniz Değişken `db.Query` daha sonra. Bir değişken SQL deyimine koyma arama yapmak için gerçekleştirirsiniz olduğundan deyim değiştirmenize olanak verir.

Ayrıca geri daha sonra içine bu iki satır kaldırdınız:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Sorgu henüz çalıştırmak istemediğiniz (diğer bir deyişle, çağrı `db.Query`) ve başlatmak istemiyorsanız `WebGrid` Yardımcısı henüz ya da. Çalıştırmak hangi SQL deyimini olduğunu dahil edilir sonra bunlar gerçekleştirirsiniz.

Bu yeniden bloğundan sonra arama işlemek için yeni mantık ekleyebilirsiniz. Tamamlanan kodu aşağıdaki gibi görünür. Bu örnek eşleşecek şekilde sayfanıza kodunu güncelleştirin:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Sayfa şimdi gibi çalışır. Sayfayı her çalıştığında veritabanını kodu açar ve `selectCommand` değişken olarak ayarlanmış tüm kayıtları alır SQL deyimine `Movies` tablo. Kod ayrıca başlatır `searchTerm` değişkeni.

Ancak, geçerli istek için bir değer içeriyorsa, `searchGenre` öğesi, kod kümeleri `selectCommand` farklı bir sorgu için — yani, birine içeren `Where` bir tarzını için aranacak yan tümcesi. Ayrıca ayarlar `searchTerm` için ne olursa olsun (hiçbir şey olabilir) arama kutusu için geçirildi.

Bağımsız olarak SQL deyimi yer `selectCommand`, kod sonra çağırır `db.Query` sorguyu çalıştırmak için hangi geçirme yer `searchTerm`. Yoksa hiçbir şey `searchTerm`, bu durumda olduğu için hiçbir parametre değerine geçirmek için önemli değildir `selectCommand` yine de.

Son olarak, kod başlatır `WebGrid` önceden olduğu gibi sorgu sonuçları kullanarak Yardımcısı.

SQL deyimi koyarak görebilirsiniz ve değişkenleri, arama terimini koda esneklik ekledik. Daha sonra Bu öğreticide gördüğünüz gibi bu temel çerçevesini kullanabilirsiniz ve aramaları farklı türleri için mantığı ekleme tutun.

## <a name="testing-the-search-by-genre-feature"></a>Türe göre arama özelliğini test etme

Webmatrix'te Çalıştır *Movies.cshtml* sayfası. Tarz metin kutusu sayfasına bakın.

Bir test kayıtlarınız için girdikten sonra'ı tıklatın bir tarzını girin **arama**. Bu süre, o Tarz eşleşen filmler listesini görürsünüz:

![Tarz 'Comedies' aradıktan sonra listeleyen filmler sayfası](form-basics/_static/image4.png)

Farklı bir tarzını girin ve yeniden arayın. Arama büyük küçük harfe duyarlı değildir görebilmeniz için tüm küçük harfler veya tüm büyük harfleri kullanarak Tarz girmeyi deneyin.

## <a name="remembering-what-the-user-entered"></a>"Ne kullanıcının girdiği hatırlamak"

Bir tarzını girilen ve tıklandığında sonra fark etmiş olabileceğiniz **arama Tarz**, bir listesi için bu tarz gördünüz. Ancak, arama metin kutusuna boş &mdash; girilen başka bir deyişle, unutmayın alamadık.

Bu davranış neden meydana geldiğini anlamak önemlidir. Bir sayfa gönderdiğinizde, tarayıcı web sunucusuna bir istek gönderir. ASP.NET istek aldığında, sayfa yeni bir örneğini oluşturur, kod içinde çalışır ve tarayıcı sayfasına yeniden oluşturur. Etkin, ancak sayfa kendisinin önceki bir sürümüyle yalnızca çalıştığınız bilmiyor. Tüm olan bazı olan isteği aldı bilir veri içinde oluşturur.

Bir sayfa isteği her zaman &mdash; ilk kez mi göndermeden tarafından &mdash; yeni bir sayfa alıyorsunuz. Web sunucusu son isteğinizin belleğe sahip değil. ASP.NET, yapar ve tarayıcı, yapar. Yalnızca bu sayfayı ayrı örnekleri arasında aralarında iletme herhangi bir veri bağlantısıdır. Örneğin, bir sayfa gönderirseniz, yeni sayfa örneği önceki örneği tarafından gönderildiği form verileri alabilirsiniz. (Sayfaları arasında veri aktarmak için başka bir yolu, tanımlama bilgileri kullanmaktır.)

Bu durumu tanımlamak için bir resmi web sayfaları olduğunu söylemek için yoludur *durum bilgisiz*. Web sunucuları ve sayfalarını ve sayfa öğeleri sayfa önceki durumuyla ilgili herhangi bir bilgi korumak değil. Web istekleri ayrı ayrı durumu koruma hızla genellikle binlerce, belki de işlemek web sunucuları, kaynakları bile yüz binlerce, saniye başına istek tüketebilir çünkü bu şekilde tasarlanmıştır.

Neden olacak şekilde metin kutusu boş. Sayfa gönderilen sonra ASP.NET sayfa yeni bir örneğini oluşturulan ve kod ve biçimlendirme çalıştırılmıştır. Bir şey yoktu metin kutusuna bir değer almaya ASP.NET söylediyse bu kodu. ASP.NET herhangi bir nedenle tepki vermedi ve metin kutusuna bir değer olmadan işlenir.

Bu sorunu çözmek almak için gerçekten kolay bir yol yoktur. Metin kutusuna girdiğiniz Tarz *olan* kodda kullanabileceğiniz &mdash; içinde `Request.QueryString["searchGenre"]`.

Metin kutusu için biçimlendirme güncelleştirme böylece `value` öznitelik değerini alır `searchTerm`, bu örnek ister:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Bu sayfada ayrıca ayarladığınız `value` özniteliğini `searchTerm` girdiğiniz değişkeni, bu değişkeni ayrıca Tarz içerdiğinden. Ancak kullanarak `Request` ayarlamak için nesne `value` özniteliği bu görevi gerçekleştirmek için standart yol gösterildiği gibi. (Hatta istediğiniz Bunu yapmak varsayılarak &mdash; bazı durumlarda, sayfayı oluşturmak isteyebilirsiniz *olmadan* alanlarında değer. Hiç uygulamanız ile neler olup bittiğini bağlıdır.)

> [!NOTE]
> "Parola için kullanılan bir metin kutusunun değerini unutmayın olamaz". Kod kullanarak bir parola alanına doldurulacak izin vermek için bir güvenlik açığı olacaktır.


Sayfayı yeniden çalıştırın, bir tarzını girin ve tıklatın **arama Tarz**. Bu zaman kalmaz, arama sonuçlarını görebilirsiniz, ancak son zamanı girilen metin kutusunda hatırlıyor:

![Metin kutusu 'önceki girdi hatırlanan olduğunu' gösteren sayfası](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Başlıkta herhangi bir sözcük arama

Artık herhangi bir tarzını için arama yapabilirsiniz, ancak aynı zamanda için bir başlık arama yapmak isteyebilirsiniz. Bunun yerine bir başlık herhangi bir yerde görünür bir sözcük arayabilirsiniz arama yaparken bir başlık tam doğru elde etmek zordur. SQL'de yapmak için kullandığınız `LIKE` işleci ve sözdizimi aşağıdaki gibi:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Bu komut, "adventure" başlıkları içeren tüm filmler alır. Kullandığınızda `LIKE` işleci, joker karakteri içeren `%` arama terimi bir parçası olarak. Arama `LIKE 'adventure%'` "'adventure' ile başlayan" anlamına gelir. (Teknik olarak, bu "Dize 'tarafından herhangi bir şey ardından adventure'." anlamına gelir) Benzer şekilde, arama terimi `LIKE '%adventure'` "hiçbir şey 'adventure' dizesi tarafından izlenen", "Bitiş 'adventure' ile" söylemek için başka bir yol olduğu anlamına gelir.

Arama terimi `LIKE '%adventure%'` bu nedenle "ile"adventure' başlığı başka bir yerindeki."anlamına gelir (Teknik olarak, "herhangi bir şey tarafından herhangi bir şey ardından adventure', arkasından başlığında.")

İçinde `<form>` öğesi, kapatma sağ altında aşağıdaki biçimlendirmeleri eklemek `</div>` etiketi Tarz arama için (yalnızca kapatmadan önce `</form>` öğesi):

[!code-html[Main](form-basics/samples/sample10.html)]

Bir araya getirmek zorunda bu arama işlemek üzere kod Tarz arama koda benzerdir `LIKE` arama. Bu sayfanın en üstündeki kod bloğunun içine ekleyin `if` hemen sonra engelleme `if` bloğu Tarz arama için:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Bu kodu, daha önce gördüğünüz aynı mantığı kullanır, arama kullanır ancak bu bir `LIKE` işleci ve kod geçirir "`%`" önce ve sonra arama terimi.

Başka bir arama eklemek kolay nasıl dikkat edin. Tüm yapmak zorunda oluştu:

- Oluşturma bir `if` ilgili arama kutusunda bir değere sahip olup olmadığını görmek için test bloğu.
- Ayarlama `selectCommand` yeni bir SQL deyimi için değişken.
- Ayarlama `searchTerm` sorgusuna geçirilecek değerine değişken.

Bir başlık araması için yeni mantığı içeren tam kod bloğu şöyledir:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Bu kodun yaptığı bir özeti aşağıda verilmiştir:

- Değişkenleri `searchTerm` ve `selectCommand` en üstünde başlatılır. Bu değişkenler (varsa) ilgili arama terim kümesi olacak ve kullanıcı sayfasında yaptığı üzerinde uygun SQL komutu dayanır. Varsayılan arama tüm filmler veritabanından alma basit bir durumdur.
- İçin testlerinde `searchGenre` ve `searchTitle`, kod kümeleri `searchTerm` aramak istediğiniz değeri. Bu kod blokları da ayarlamanız `selectCommand` bu arama için uygun bir SQL komutu.
- `db.Query` Yöntemi çağrıldığında yalnızca bir kez içinde ne olursa olsun SQL komutunu kullanarak `selectedCommand` ve ne olursa olsun değer `searchTerm`. (Bir tarzını ve başlık sözcük yok), arama terimi yok ise değerini `searchTerm` boş bir dize. Bu durumda sorgu parametresi gerektirmediğinden ancak, bu, önemli değildir.

## <a name="testing-the-title-search-feature"></a>Başlık arama özelliğini test etme

Artık tamamlanmış arama sayfası test edebilirsiniz. Çalıştırma *Movies.cshtml*.

Bir tarzını girin ve tıklayın **arama Tarz**. Kılavuz önce gibi bu tarz filmler görüntüler.

Bir başlık sözcüğü girin ve tıklayın **arama başlık**. Kılavuzun bu word başlığında filmler görüntüler.

![Başlık '' için arama yaptıktan sonra listeleyen filmler sayfası](form-basics/_static/image6.png)

Her iki metin kutusunu boş bırakın ve her iki düğmesine tıklayın. Kılavuz tüm filmler görüntüler.

## <a name="combining-the-queries"></a>Sorguları birleştirme

Gerçekleştirebileceğiniz aramaları dışlar fark edebilirsiniz. Her iki arama kutusu değerleri bunları olsa bile aynı anda başlığı ve bir tarzını arama yapamazsınız. Örneğin, "Adventure", başlık içeren tüm eylem filmler için arama yapamazsınız. (Başlık araması tarz ve başlığı için değerleri girerseniz, sayfa şimdi kodlanmış olarak öncelik alır.) Koşullar birleştiren bir arama oluşturmak için aşağıdaki gibi söz dizimi olan bir SQL sorgusu oluşturmak gerekir:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Ve aşağıdaki gibi bir deyimi kullanarak sorguyu çalıştırmanız gerekir (kabaca konuşulur):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Gördüğünüz gibi çok sayıda permütasyon arama ölçütü izin vermek için mantık oluşturma biraz karmaşık alabilirsiniz. Bu nedenle, biz burada durdurmanız.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide filmler veritabanına eklemek kullanıcıların izin vermek için bir form kullanan bir sayfa oluşturacaksınız.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Tam listesi için (arama güncelleştirilmiş) film sayfası

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Razor sözdizimini kullanan ASP.NET Web programlamaya giriş](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL WHERE yan tümcesi](http://www.w3schools.com/sql/sql_where.asp) W3Schools sitesinde
- [Yöntem tanımlarını](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) W3C sitesinde makale

> [!div class="step-by-step"]
> [Önceki](displaying-data.md)
> [sonraki](entering-data.md)
