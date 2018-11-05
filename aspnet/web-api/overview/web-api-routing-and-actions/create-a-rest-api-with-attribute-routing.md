---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: ASP.NET Web API 2'de öznitelik yönlendirme ile REST API oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 18a44c280e6df1603837938d24d7d639d8c87cc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021423"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 yönlendirme özniteliğine sahip bir REST API'si oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Web API 2 destekleyen yeni bir tür adı yönlendirme *öznitelik yönlendirme*. Öznitelik yönlendirme genel bakış için bkz: [Web API 2'de öznitelik yönlendirme](attribute-routing-in-web-api-2.md). Bu öğreticide, öznitelik yönlendirme books koleksiyonu için bir REST API oluşturmak için kullanın. API aşağıdaki eylemleri destekler:

| Eylem | Örnek URI |
| --- | --- |
| Tüm Kitaplar bir listesini alın. | / api/kitaplar |
| Bir kitap kimliğe göre Al | /api/Books/1 |
| Bir kitap ayrıntılarını alın. | /api/Books/1/details |
| Kitap listesi türe göre alın. | /api/Books/fantasy |
| Kitap listesi, yayın tarihe göre alın. | /api/Books/Date/2013-02-16 /api/books/date/2013/02/16 (alternatif formu) |
| Belirli bir yazar tarafından books bir listesini alın. | /api/Authors/1/Books |

Salt okunur (HTTP GET isteklerini) tüm yöntemlerdir.

Entity Framework veri katmanı için kullanacağız. Kitap kayıtlarını, aşağıdaki alanları olacaktır:

- Kimlik
- Başlık
- Tarzı
- Yayın tarihi
- Fiyat
- Açıklama
- AuthorID (yazarlar tabloya yabancı anahtar)

İsteklerin çoğuna ancak API (başlık, yazar ve Tarz) bu verilerin bir alt kümesini döndürür. Tam kayıt, istemci isteklerini almak için `/api/books/{id}/details`.

## <a name="prerequisites"></a>Önkoşullar

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional veya Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio çalıştırarak başlayın. Gelen **dosya** menüsünde **yeni** seçip **proje**.

Genişletin **yüklü** > **Visual C#** kategorisi. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET Web uygulaması (.NET Framework)**. Projeyi adlandırın &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

İçinde **yeni ASP.NET Web uygulaması** iletişim kutusunda **boş** şablonu. "Klasör eklemek ve çekirdek başvuruları için" altında seçin **Web API** onay kutusu. **Tamam**'ı tıklatın.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Bu, Web API işlevleri için yapılandırılmış bir çatı projesini oluşturur.

### <a name="domain-models"></a>Etki alanı modelleri

Ardından, etki alanı modelleri sınıfları ekleyin. Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Seçin **Ekle**, ardından **sınıfı**. Sınıf adını `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Author.cs kodu aşağıdakiyle değiştirin:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Şimdi adlı başka bir sınıf ekleyin `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Web API denetleyici ekleme

Bu adımda, veri katmanı olarak Entity Framework kullanan bir Web API denetleyicisi ekleyeceğiz.

Projeyi oluşturmak için CTRL+SHIFT+B tuşlarına basın. Entity Framework veritabanı şeması oluşturmak derlenmiş bir bütünleştirilmiş kod gerektirir böylece modellerinin özelliklerini bulmak için yansıtma kullanır.

Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın. Seçin **Ekle**, ardından **denetleyicisi**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

İçinde **İskele Ekle** iletişim kutusunda **Web API 2 denetleyici Entity Framework kullanarak Eylemler ile**.

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

İçinde **denetleyici Ekle** iletişim için **Denetleyici adı**, girin &quot;BooksController&quot;. Seçin &quot;zaman uyumsuz denetleyici eylemlerini kullanmak&quot; onay kutusu. İçin **Model sınıfı**seçin &quot;kitap&quot;. (Görmüyorsanız `Book` sınıfı listelenen açılır menüde, proje oluşturulan emin olun.) Ardından, "+" düğmesine tıklayın.

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Tıklayın **Ekle** içinde **yeni veri bağlamı** iletişim.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Tıklayın **Ekle** içinde **denetleyici Ekle** iletişim. Yapı iskelesi adlı bir sınıf ekler `BooksController` tanımlayan bir API denetleyicisi. Ayrıca adlı bir sınıf ekler `BooksAPIContext` için Entity Framework veri bağlamı tanımlar modelleri klasöründe.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Veritabanının Çekirdeğini Oluşturma

Araçlar menüsü'nden seçin **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.

Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Bu komut, geçişler klasörü oluşturur ve Configuration.cs adlı yeni bir kod dosyası ekler. Bu dosyayı açın ve aşağıdaki kodu ekleyin `Configuration.Seed` yöntemi.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Paket Yöneticisi konsolu penceresinde, aşağıdaki komutları yazın.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Bu komutlar, yerel bir veritabanı oluşturun ve veritabanını doldurmak için Seed yöntemi çağırır.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>DTO sınıfları ekleme

Şimdi uygulamayı çalıştırın ve bir GET isteği göndermek için /api/books/1, yanıtı aşağıdaki gibi görünür. (Okunabilirlik için Girintileme ekledim.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Bunun yerine, alanların bir alt kümesini döndürmek için bu istek için istiyorum. Ayrıca, Yazar Kimliği yerine, yazarın adı döndürülecek istediğim Bunu yapmak için biz döndürülecek denetleyicisi yöntemi değiştireceksiniz bir *veri aktarımı nesnesi* (DTO) yerine EF modeli. Bir DTO, yalnızca veri taşımak üzere tasarlanmış bir nesnedir.

Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle** | **yeni klasör**. Klasör adı &quot;Dto'lar&quot;. Adlı bir sınıf ekleyin `BookDto` Dto'lar klasörüne aşağıdaki tanımıyla:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Adlı başka bir sınıf ekleyin `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Ardından, güncelleştirme `BooksController` döndürülecek sınıfı `BookDto` örnekleri. Kullanacağız [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) projesine yöntemi `Book` için örnekler `BookDto` örnekleri. Denetleyici sınıfı için güncelleştirilmiş kod aşağıdaki gibidir.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Sildim `PutBook`, `PostBook`, ve `DeleteBook` yöntemleri, çünkü bu öğretici için gerekli değildir.


Şimdi uygulamayı çalıştırabilir ve /api/books/1 istek, yanıt gövdesi şöyle görünmelidir:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Rota özniteliklerini ekleme

Ardından, biz özniteliği yönlendirmeyi kullanmak için denetleyici dönüştürmeniz. İlk olarak, ekleme bir **routeprefix öğesi** özniteliği denetleyiciye. Bu öznitelik, bu denetleyici üzerinde tüm yöntemleri için başlangıç URI kesimleri tanımlar.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Ardından Ekle **[yol]** denetleyici eylemleri için aşağıdaki gibi öznitelikleri:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Her denetleyici yöntemi için rota şablonu önekidir ve dizeyi belirtilen **rota** özniteliği. İçin `GetBook` parametreli dizeyi includes yöntemi, rota şablonu &quot;{kimliği: int}&quot;, URI segmenti bir tamsayı değeri içeriyorsa eşleşir.

| Yöntem | Rota şablonu | Örnek URI |
| --- | --- | --- |
| `GetBooks` | "API/books" | `http://localhost/api/books` |
| `GetBook` | "API/books / {kimliği: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Kitap ayrıntılarını Al

Kitap ayrıntıları almak için istemci, bir GET isteği gönderir `/api/books/{id}/details`burada *{id}* kitabın kimliğidir.

Aşağıdaki yöntemi ekleyin `BooksController` sınıfı.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Eğer istenmişse `/api/books/1/details`, yanıt şöyle görünür:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Türe göre kitap alma

Belirli bir tarzında books listesini almak için istemci, bir GET isteği gönderir `/api/books/genre`burada *Tarz* Tarz adıdır. (Örneğin, `/api/books/fantasy`.)

Aşağıdaki yöntemi ekleyin `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Biz bir URI şablonu {Tarz} parametresi içeren bir yol burada tanımlarsınız. Web API'si bu iki bir URI'leri ayırmak ve bunları farklı yöntemleri için yol olduğuna dikkat edin:

`/api/books/1`

`/api/books/fantasy`

Çünkü `GetBook` yöntemi içeren bir kısıtlama "id" segment bir tamsayı değeri olmalıdır:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

/Api/books/fantasy istek, yanıt şöyle görünür:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Yazara göre kitapları alın

Belirli bir yazar için bir kitap listesi almak için istemci, bir GET isteği gönderir `/api/authors/id/books`burada *kimliği* Yazar kimliğidir.

Aşağıdaki yöntemi ekleyin `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

Bu örnekte ilginç, çünkü &quot;books&quot; olan bir alt kaynağın kabul &quot;yazarlar&quot;. Bu düzen, RESTful API'leri oldukça yaygındır.

Yol şablonunda tilde (~) yol ön eki geçersiz kılmalar **routeprefix öğesi** özniteliği.

## <a name="get-books-by-publication-date"></a>Kitap Yayını tarihe göre Al

Yayın tarihe göre kitap listesi almak için istemci, bir GET isteği gönderir `/api/books/date/yyyy-mm-dd`burada *yyyy-aa-gg* tarihtir.

Bunu yapmanın bir yolu aşağıda verilmiştir:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}` Eşleştirilecek parametresi kısıtlı bir **DateTime** değeri. Bu çalışır, ancak çok isteriz gerçekten daha fazla izin veremez. Örneğin, bu bir URI'leri rota eşleşir:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Bu bir URI'leri vermekle yanlış bir şey yoktur. Ancak bir normal ifade kısıtlaması için rota şablonu ekleyerek belirli bir biçime rota kısıtlayabilirsiniz:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Artık yalnızca tarih biçiminde &quot;yyyy-aa-gg&quot; eşleşir. Biz regex gerçek tarih yapılandırdığımıza doğrulamak için kullanmayın dikkat edin. Web API uygulamasına URI segmenti Dönüştür çalıştığında işlenen bir **DateTime** örneği. Geçersiz bir tarih gibi ' 2012-47-99' başarısız olur dönüştürülecek ve istemci bir 404 hatası alırsınız.

Bir eğik çizgi ayırıcı de destekleyebilir (`/api/books/date/yyyy/mm/dd`) diğerine ekleyerek **[yol]** farklı bir regex ile özniteliği.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Burada küçük ancak önemli ayrıntı yok. İkinci rota şablonu olan bir joker karakter (\*) {pubdate} parametresi başlangıcı:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Bu, yönlendirme altyapısını {pubdate} URI'ın geri kalan eşleşmelidir bildirir. Varsayılan olarak, bir şablon parametresi, tek bir URI segmenti eşleşir. Bu durumda, {pubdate} birkaç URI segmentleri span istiyoruz:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Denetleyici kodlarının

BooksController sınıf için tam kod aşağıda verilmiştir.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Özet

Öznitelik yönlendirme, daha fazla denetim ve esneklik, API'niz için bir URI'leri tasarlarken sunar.
