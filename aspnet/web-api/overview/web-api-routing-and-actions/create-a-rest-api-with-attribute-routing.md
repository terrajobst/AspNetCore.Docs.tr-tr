---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: ASP.NET Web API 2 özniteliği yönlendirme ile bir REST API'si oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 1f1e90544c9dd8439a522f2196d81d020ea2f4f2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30223268"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 yönlendirme özniteliği olan bir REST API'si oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Web API 2 destekleyen yeni bir tür yönlendirme biri olarak adlandırılan *özniteliği yönlendirme*. Öznitelik yönlendirme genel bir bakış için bkz: [özniteliği yönlendirme Web API 2'deki](attribute-routing-in-web-api-2.md). Bu öğreticide, öznitelik yönlendirme books koleksiyonu için bir REST API oluşturmak için kullanın. API aşağıdaki eylemleri destekler:

| Eylem | Örnek URI |
| --- | --- |
| Tüm books listesini alın. | / api/books |
| Kimliğe göre defteri alma | /api/Books/1 |
| Kitap ayrıntılarını alın. | /api/Books/1/details |
| Türe göre books listesini alın. | /api/Books/fantasy |
| Yayın tarihe göre books listesini alın. | /api/Books/Date/2013-02-16 /api/books/date/2013/02/16 (alternatif form) |
| Belirli bir yazar tarafından books listesini alın. | /api/Authors/1/Books |

Tüm yöntemleri salt okunur (HTTP GET isteği) adı verilir.

Entity Framework veri katmanı için kullanacağız. Kayıt defteri aşağıdaki alanları olacaktır:

- Kimlik
- Başlık
- Tarz
- Yayın tarihi
- Fiyat
- Açıklama
- AuthorID (yazarlar tablosuna yabancı anahtar)

Çoğu istekler için ancak API (başlık, yazar ve Tarz) bu verilerin bir alt kümesini döndürür. Tam kayıt, istemci isteklerini almak için `/api/books/{id}/details`.

## <a name="prerequisites"></a>Önkoşullar

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional veya Enterprise sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio çalıştırarak başlayın. Gelen **dosya** menüsünde, select **yeni** ve ardından **proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Proje adı &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu. "Klasörleri ekleyin ve çekirdek için başvurular" altında seçin **Web API** onay kutusu. Tıklatın **projesi oluşturma**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Bu Web API işlevselliği için yapılandırılmış bir çatı projesini oluşturur.

### <a name="domain-models"></a>Etki alanı modeli

Ardından, etki alanı modelleri için sınıfları ekleyin. Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Seçin **Ekle**seçeneğini belirleyip **sınıfı**. Sınıf adını `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Author.cs kodu aşağıdakilerle değiştirin:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Şimdi adlı başka bir sınıf ekleyin `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Bir Web API denetleyicisi ekleme

Bu adımda, veri katmanı olarak Entity Framework kullanan Web API denetleyicisi ekleyeceğiz.

Projeyi oluşturmak için CTRL+SHIFT+B tuşlarına basın. Entity Framework yansıma veritabanı şeması oluşturmak derlenmiş bir bütünleştirilmiş kod gerektirdiği şekilde modellerinin özelliklerini bulmak için kullanır.

Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın. Seçin **Ekle**seçeneğini belirleyip **denetleyicisi**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

İçinde **İskele Ekle** iletişim kutusunda "Web API 2 denetleyici Entity Framework kullanarak okuma/yazma eylemleri ile."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

İçinde **denetleyici Ekle** iletişim için **Denetleyici adı**, girin &quot;BooksController&quot;. Seçin &quot;zaman uyumsuz denetleyici eylemlerini kullanmak&quot; onay kutusu. İçin **Model sınıfı**seçin &quot;defteri&quot;. (Görmüyorsanız `Book` sınıfı listelenen açılır listede, proje yerleşik emin olun.) Ardından "+" düğmesini tıklatın.

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Tıklatın **Ekle** içinde **yeni veri bağlamı** iletişim.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Tıklatın **Ekle** içinde **denetleyici Ekle** iletişim. Yapı iskelesi adlı bir sınıf ekler `BooksController` API denetleyicisi tanımlar. Ayrıca adlı bir sınıf ekler `BooksAPIContext` için Entity Framework veri bağlamı tanımlar modeller klasörü.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Çekirdek veritabanı

Araçlar menüsünden seçin **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.

Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Bu komut Migrations klasörünü oluşturur ve Configuration.cs adlı yeni bir kod dosyası ekler. Bu dosyayı açın ve aşağıdaki kodu ekleyin `Configuration.Seed` yöntemi.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Paket Yöneticisi konsolu penceresinde aşağıdaki komutları yazın.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Bu komutlar yerel bir veritabanı oluşturun ve veritabanını doldurmak için Seed yöntemi çağırır.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>DTO sınıfları ekleme

Uygulamayı şimdi çalıştırmak ve /api/books/1 için bir GET isteği göndermek, yanıt aşağıdakine benzer durur. (Okunabilirlik için girinti ekledim.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Bunun yerine, bir alt kümesini alanları döndürmek için bu isteği istiyorum. Ayrıca, Yazar Kimliği yerine yazarın adını döndürmek için istediğim Bunu başarmak için biz döndürmek için denetleyici yöntemi değiştireceksiniz bir *veri aktarım nesnesini* (DTO) yerine EF modeli. Bir DTO yalnızca veri taşımak için tasarlanmış bir nesnedir.

Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** | **yeni klasör**. Klasör adı &quot;DTOs&quot;. Adlı bir sınıf ekleyin `BookDto` aşağıdaki tanımını içeren DTOs klasörü:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Adlı başka bir sınıf ekleyin `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Ardından, güncelleştirme `BooksController` dönmek için sınıf `BookDto` örnekleri. Kullanacağız [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) yöntemi projesine `Book` için örnekleri `BookDto` örnekleri. Denetleyici sınıfı için güncelleştirilmiş kod aşağıdaki gibidir.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> I silinmiş `PutBook`, `PostBook`, ve `DeleteBook` yöntemleri, çünkü bu öğretici için gerekli değildir.


Şimdi uygulamayı çalıştırın ve /api/books/1 istek, yanıt gövdesi aşağıdaki gibi görünmelidir:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Rota öznitelikleri ekleme

Ardından, biz özniteliği yönlendirmeyi kullanmak için denetleyici dönüştürmeniz. İlk olarak, ekleyin bir **routeprefix öğesi** özniteliği denetleyiciye. Bu öznitelik bu denetleyicisinde tüm yöntemleri için ilk URI kesimleri tanımlar.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Ardından ekleyin **[yol]** denetleyici eylemleri için aşağıdaki gibi öznitelikleri:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Her denetleyici yöntemi için rota şablonu için önekidir artı dizeyi belirtilen **rota** özniteliği. İçin `GetBook` yöntemi, rota şablonu içerir tabanlı dizeye &quot;{kimliği: int}&quot;, URI segmenti bir tamsayı değeri içeriyorsa eşleşir.

| Yöntem | Rota şablonu | Örnek URI |
| --- | --- | --- |
| `GetBooks` | "API/books" | `http://localhost/api/books` |
| `GetBook` | "API/books / {kimliği: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Defteri ayrıntıları alma

Defteri ayrıntıları almak için istemci bir GET isteği gönderir `/api/books/{id}/details`, burada *{id}* defteri kimliğidir.

Aşağıdaki yöntemi ekleyin `BooksController` sınıfı.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Eğer istenmişse `/api/books/1/details`, yanıtı şuna benzer:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Türe göre Books Al

İçinde belirli bir tarzını books listesini almak için istemci bir GET isteği gönderir `/api/books/genre`, burada *Tarz* Tarz adıdır. (Örneğin, `/api/books/fantasy`.)

Aşağıdaki yöntemi ekleyin `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Burada size URI şablonunu {Tarz} parametre içeren bir yol tanımlıyorsanız. Web API bu iki URI'ler ayırt etmek ve için farklı yöntemler rota mümkün olduğuna dikkat edin:

`/api/books/1`

`/api/books/fantasy`

Çünkü `GetBook` yöntemi "id" segmenti bir tamsayı değeri olmalıdır kısıtlama içerir:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

/Api/books/fantasy istek, yanıt şöyle görünür:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Yazar tarafından Books Al

İçin belirli bir yazarın bir books listesini almak için istemci bir GET isteği gönderir `/api/authors/id/books`, burada *kimliği* Yazar kimliğidir.

Aşağıdaki yöntemi ekleyin `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

Bu örnekte ilginç, çünkü &quot;books&quot; olan bir alt kaynağın kabul &quot;yazarlar&quot;. Bu desen RESTful API'leri oldukça yaygındır.

Rota şablonu içinde tilde (~) rota öneki geçersiz kılmaları **routeprefix öğesi** özniteliği.

## <a name="get-books-by-publication-date"></a>Yayın tarihe göre Books Al

Yayın tarihe göre books listesini almak için istemci bir GET isteği gönderir `/api/books/date/yyyy-mm-dd`, burada *yyyy-aa-gg* tarihidir.

Bunu yapmanın bir yolu şöyledir:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}` Eşleştirmek için parametre kısıtlı bir **DateTime** değeri. Bu çalışır, ancak isteriz daha gerçekten daha fazla izin veren. Örneğin, bu URI rota eşleşir:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Bu URI izin vermekle yanlış bir şey yoktur. Ancak, rota şablonu için bir normal ifade kısıtlaması ekleyerek belirli bir biçimde rota kısıtlayabilirsiniz:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Artık yalnızca tarih biçiminde &quot;yyyy-aa-gg&quot; ile eşleşir. Biz regex gerçek tarih elde ettiğinizi doğrulamak için kullanmayın dikkat edin. Web API uygulamasına URI segmenti dönüştürmek çalıştığında işlenmiş bir **DateTime** örneği. Geçersiz bir tarih gibi ' 2012-47-99' başarısız olur dönüştürülecek ve istemci 404 hatası alırsınız.

Ayrıca bir eğik çizgi ayırıcı destekleyebilir (`/api/books/date/yyyy/mm/dd`) başka bir ekleyerek **[yol]** farklı bir regex özniteliğiyle.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Bir ince ama önemli burada ayrıntılı yoktur. İkinci yol şablonu olan bir joker karakter (\*) {pubdate} parametresi başlangıcında:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Bu, Yönlendirme altyapısı bu {pubdate} rest URI'sinin eşleşmesi gereken bildirir. Varsayılan olarak, bir şablon parametresini tek bir URI segmenti eşleşir. Bu durumda, {pubdate} birkaç URI kesimleri span istiyoruz:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Denetleyici kodu

BooksController sınıfı için tam kod aşağıda verilmiştir.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Özet

Öznitelik yönlendirme, daha fazla denetim ve esneklik API'nizi URI'ler tasarlarken sunar.
