---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: "Film modeli ve tablosu (C#) için yeni bir alan ekleme | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: c10f3be30a92a605c34fa1c56fa3691389374beb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Film modeli ve tablosu (C#) için yeni bir alan ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [C# sürümü](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


Bu bölümde model sınıflarına bazı değişiklikler yapın ve veritabanı şeması model değişiklikleri eşleşecek şekilde nasıl güncelleştirebileceğinizi öğrenin.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeline derecelendirme özellik ekleme

Yeni bir eklemeye başlayın `Rating` varolan özelliği `Movie` sınıfı. Açık *Movie.cs* dosya ve ekleme `Rating` özelliği aşağıdakine benzer:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Tam `Movie` sınıfında aşağıdaki kodu şimdi görülüyor:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Kullanarak uygulamayı derleyin **hata ayıklama** &gt; **yapı film** menü komutu.

Güncelleştirdiğinizden `Model` sınıfı, ayrıca gerekir güncelleştirmek *\Views\Movies\Index.cshtml* ve *\Views\Movies\Create.cshtml* yeni desteklemekiçingörüntülemeşablonları`Rating`özelliği.

Açık *\Views\Movies\Index.cshtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonra sütun başlığı **fiyat** sütun. Ardından ekleyin bir `<td>` sütun oluşturmak için şablon sona `@item.Rating` değeri. Hangi güncelleştirilmiş aşağıdadır *Index.cshtml* şablonu görüntüleme gibi görünüyor:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Ardından, açık *\Views\Movies\Create.cshtml* dosyasını ve form sona aşağıdaki biçimlendirmeyi ekleyin. Yeni bir filmi oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.

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

"MovieInitializer" sınıf adı. Güncelleştirme `MovieInitializer` sınıfı aşağıdaki kod içerir:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

`MovieInitializer` Sınıf modeli tarafından kullanılan veritabanı bırakılacak ve modeli sınıfları herhangi bir zamanda değiştirmeniz durumunda otomatik olarak yeniden oluşturulması gerektiğini belirtir. Kodu içeren bir `Seed` otomatik olarak herhangi bir veritabanına eklemek için bazı varsayılan veri süresi belirtmek için yöntemi oluşturduğu (veya yeniden oluşturulur). Bu, el ile değiştirmek model yaptığınızda doldurmak gerek kalmadan veritabanını bazı örnek verilerle doldurmak için kullanışlı bir yol sağlar.

Tanımladığınız göre `MovieInitializer` sınıfı, istemeniz yukarı, kablo böylece uygulama her çalıştırıldığında, modeli sınıfları veritabanındaki şemasından farklı olup olmadığını denetler. Bunlar Başlatıcı modelle eşleşecek ve veritabanı örnek verileri ile doldurmak için veritabanını yeniden oluşturmak için çalıştırabilirsiniz.

Açık *Global.asax* kökünde dosya `MvcMovies` proje:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

*Global.asax* dosyayı içeren tüm uygulama projesi için tanımlar ve içeren sınıf bir `Application_Start` uygulamayı ilk kez başlatıldığında çalıştırılan olay işleyicisi.

İki ekleyelim using deyimlerini dosyanın en üstüne ekleyin. İlk Entity Framework ad alanına başvuruda bulunuyor ve ikinci ad alanı referanslarını burada bizim `MovieInitializer` sınıf yaşamlarını:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Ardından bulmak `Application_Start` yöntemi ve bir çağrı ekleyin `Database.SetInitializer` aşağıda gösterildiği gibi yöntem başında:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

`Database.SetInitializer` Eklediğiniz deyimi gösterir veritabanı tarafından kullanılan `MovieDBContext` örneği otomatik olarak silinecek ve şema ve veritabanı eşleşmiyorsa yeniden oluşturulacak. Ve gördüğünüz gibi aynı zamanda belirtilen örnek verileri veritabanıyla dolduracaktır `MovieInitializer` sınıfı.

Kapat *Global.asax* dosya.

Uygulamayı yeniden çalıştırın ve gidin */Movies* URL. Uygulama başladığında model yapısını artık veritabanı şeması eşleştiğini algılar. Otomatik olarak yeni model yapısını eşleştirmek üzere veritabanını yeniden oluşturur ve örnek filmler veritabanıyla doldurur:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı. Bir derecelendirme ekleyebileceğinizi unutmayın.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)


              **Oluştur**'u tıklatın. Derecelendirme dahil olmak üzere yeni film şimdi listeleme filmler içinde görüntülenir:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

Bu bölümde nasıl model nesneleri değiştirmek ve veritabanı değişiklikleri eşit tutmak gördünüz. Ayrıca, senaryolarını deneyebilirsiniz şekilde yeni oluşturulan bir veritabanını örnek verilerle doldurmak için bir yöntem de öğrendiniz. Ardından, nasıl daha zengin doğrulama mantığını model sınıfları ekleyebilir ve zorlanacak bazı iş kurallarını etkinleştirme sırasında bakalım.

>[!div class="step-by-step"]
[Önceki](examining-the-edit-methods-and-edit-view.md)
[sonraki](adding-validation-to-the-model.md)
