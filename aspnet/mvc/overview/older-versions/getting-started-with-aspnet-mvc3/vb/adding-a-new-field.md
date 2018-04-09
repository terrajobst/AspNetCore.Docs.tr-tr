---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Film modeli ve veritabanı tablosu (VB) için yeni bir alan ekleyerek | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Film modeli ve veritabanı tablosu (VB) yeni bir alan ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-new-field.md) Bu öğreticinin.


Bu bölümde model sınıflarına bazı değişiklikler yapın ve veritabanı şeması model değişiklikleri eşleşecek şekilde nasıl güncelleştirebileceğinizi öğrenin.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeline derecelendirme özellik ekleme

Yeni bir eklemeye başlayın `Rating` varolan özelliği `Movie` sınıfı. Açık *Movie.cs* dosya ve ekleme `Rating` özelliği aşağıdakine benzer:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

Tam `Movie` sınıfında aşağıdaki kodu şimdi görülüyor:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Kullanarak uygulamayı derleyin **hata ayıklama** &gt; **yapı film** menü komutu.

Güncelleştirdiğinizden `Model` sınıfı, ayrıca gerekir güncelleştirmek *\Views\Movies\Index.vbhtml* ve *\Views\Movies\Create.vbhtml* yeni desteklemekiçingörüntülemeşablonları`Rating`özelliği.

Açık<em>\Views\Movies\Index.vbhtml</em> dosya ve ekleme bir `<th>Rating</th>` hemen sonra sütun başlığı <strong>fiyat</strong> sütun. Ardından ekleyin bir `<td>` sütun oluşturmak için şablon sona `@item.Rating` değeri. Hangi güncelleştirilmiş aşağıdadır <em>Index.vbhtml</em> şablonu görüntüleme gibi görünüyor:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Ardından, açık *\Views\Movies\Create.vbhtml* dosyasını ve form sona aşağıdaki biçimlendirmeyi ekleyin. Yeni bir filmi oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Model ve veritabanı şeması farklar yönetme

Şimdi yeni desteklemek için uygulama kodu güncelleştirdik `Rating` özelliği.

Uygulamayı çalıştırın ve gidin artık */Movies* URL. Ancak, bunu yaptığınızda, aşağıdaki hatayı görürsünüz:

![](adding-a-new-field/_static/image1.png)

Bu hata nedeniyle görüyorsunuz güncelleştirilmiş `Movie` uygulama modeli sınıfında şeması farklı şimdi `Movie` var olan veritabanının tablo. (Var. hiçbir `Rating` veritabanı tablosundaki sütun.)

Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler. Entity Framework eşitlenmiş durumda değilse, bir hata oluşturur. Bu, aksi halde yalnızca (tarafından belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zaman sorunları izlemek kolaylaştırır. Eşitleme denetimi özelliği görüntülenecek hata iletisi yalnızca gördüğünüz neyin neden olur.

Hatayı çözümlemek için iki yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır. Hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi izin verdiği için bu yaklaşım bir test veritabanı üzerindeki etkin geliştirme yaparken çok kullanışlı bir yoldur. Dezavantajı rağmen veritabanında var olan veri kaybı olan — böylece, *yok* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!
2. Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin. Bu yaklaşımın avantajı, verilerinizi tutmak ' dir. Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.

Bu öğretici için ilk yaklaşım kullanacağız — Entity Framework kod model değişiklikleri zaman otomatik olarak veritabanını yeniden oluşturmak ilk sahip olacaksınız.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Otomatik olarak Model değişiklikleri veritabanını yeniden oluşturma

Code First otomatik olarak bırakır ve uygulama için model değiştirmek zaman veritabanını yeniden oluşturur şekilde şimdi uygulama güncelleştirin.

> [!NOTE] 
> 
> **Uyarı** otomatik olarak bırakarak ve yalnızca geliştirme veya test veritabanını kullanırken veritabanı yeniden oluşturarak bu yaklaşım etkinleştirmeniz gerekir ve *hiçbir zaman* gerçek verileri içeren bir üretim veritabanında. Bir üretim sunucusunda kullanarak veri kaybına yol açabilir.


İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.

![](adding-a-new-field/_static/image2.png)

Sınıf adını &quot;MovieInitializer&quot;. Güncelleştirme `MovieInitializer` sınıfı aşağıdaki kod içerir:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

`MovieInitializer` Sınıf modeli tarafından kullanılan veritabanı bırakılacak ve modeli sınıfları herhangi bir zamanda değiştirmeniz durumunda otomatik olarak yeniden oluşturulması gerektiğini belirtir. Kodu içeren bir `Seed` otomatik olarak herhangi bir veritabanına eklemek için bazı varsayılan veri süresi belirtmek için yöntemi oluşturduğu (veya yeniden oluşturulur). Bu, el ile değiştirmek model yaptığınızda doldurmak gerek kalmadan veritabanını bazı örnek verilerle doldurmak için kullanışlı bir yol sağlar.

Tanımladığınız göre `MovieInitializer` sınıfı, istemeniz yukarı, kablo böylece uygulama her çalıştırıldığında, modeli sınıfları veritabanındaki şemasından farklı olup olmadığını denetler. Bunlar Başlatıcı modelle eşleşecek ve veritabanı örnek verileri ile doldurmak için veritabanını yeniden oluşturmak için çalıştırabilirsiniz.

Açık *Global.asax* kökünde dosya `MvcMovies` proje:

*Global.asax* dosyayı içeren tüm uygulama projesi için tanımlar ve içeren sınıf bir `Application_Start` uygulamayı ilk kez başlatıldığında çalıştırılan olay işleyicisi.

Bul `Application_Start` yöntemi ve bir çağrı ekleyin `Database.SetInitializer` aşağıda gösterildiği gibi yöntem başında:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

`Database.SetInitializer` Eklediğiniz deyimi gösterir veritabanı tarafından kullanılan `MovieDBContext` örneği otomatik olarak silinecek ve şema ve veritabanı eşleşmiyorsa yeniden oluşturulacak. Ve gördüğünüz gibi aynı zamanda belirtilen örnek verileri veritabanıyla dolduracaktır `MovieInitializer` sınıfı.

Kapat *Global.asax* dosya.

Uygulamayı yeniden çalıştırın ve gidin */Movies* URL. Uygulama başladığında model yapısını artık veritabanı şeması eşleştiğini algılar. Otomatik olarak yeni model yapısını eşleştirmek üzere veritabanını yeniden oluşturur ve örnek filmler veritabanıyla doldurur:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı. Bir derecelendirme ekleyebileceğinizi unutmayın.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

**Oluştur**'u tıklatın. Derecelendirme dahil olmak üzere yeni film şimdi listeleme filmler içinde görüntülenir:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

Bu bölümde nasıl model nesneleri değiştirmek ve veritabanı değişiklikleri eşit tutmak gördünüz. Ayrıca, senaryolarını deneyebilirsiniz şekilde yeni oluşturulan bir veritabanını örnek verilerle doldurmak için bir yöntem de öğrendiniz. Ardından, nasıl daha zengin doğrulama mantığını model sınıfları ekleyebilir ve zorlanacak bazı iş kurallarını etkinleştirme sırasında bakalım.

> [!div class="step-by-step"]
> [Önceki](examining-the-edit-methods-and-edit-view.md)
> [sonraki](adding-validation-to-the-model.md)
