---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Film modeli ve tablosuna (C#) yeni bir alan ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 3767ca5f0f32c2a93217d902e200fa2dd3dd262a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578295"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Film modeli ve tablosuna (C#) yeni bir alan ekleme
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir. [C# sürümü indirme](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


Bu bölümde model sınıflarına bazı değişiklikler yapmanız ve veritabanı şeması modeli değişikliklerle eşleştirmek için nasıl güncelleştirebilirsiniz öğrenin.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeli derecelendirme özellik ekleme

Yeni bir ekleyerek başlangıç `Rating` varolan özellik `Movie` sınıfı. Açık *Movie.cs* dosya ve ekleme `Rating` bunun gibi özelliği:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Tam `Movie` sınıfı şimdi aşağıdaki aşağıdaki kod gibi görünür:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Kullanarak uygulamayı derleyin **hata ayıklama** &gt; **derleme film** menü komutu.

Güncelleştirdiğinize göre `Model` sınıfı da ihtiyacınız güncelleştirilecek *\Views\Movies\Index.cshtml* ve *\Views\Movies\Create.cshtml* yeni desteklemekiçingörüntülemeşablonları`Rating`özelliği.

Açık *\Views\Movies\Index.cshtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonrasına sütun başlığı **fiyat** sütun. Ardından Ekle bir `<td>` sütun oluşturmak için şablon sonlarında `@item.Rating` değeri. Hangi güncelleştirilmiş aşağıdadır *Index.cshtml* görünüm şablonu şöyle:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Ardından, açık *\Views\Movies\Create.cshtml* dosya ve formun sonlarında aşağıdaki işaretlemeyi ekleyin. Yeni bir film oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Model ve veritabanı şema farklılıkları yönetme

Artık uygulama kodu yeni destekleyecek şekilde güncelleştirdik `Rating` özelliği.

Şimdi uygulamayı çalıştırabilir ve gidin */Movies* URL'si. Ancak, bunu yaptığınızda, aşağıdaki hatayı görürsünüz:

![](adding-a-new-field/_static/image1.png)

Çünkü bu hatayı görüyorsunuz güncelleştirilmiş `Movie` model sınıfı uygulama şemasını farklı artık `Movie` mevcut veritabanı tablosu. (Yok hiçbir `Rating` veritabanı tablosundaki sütun.)

Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler. Entity Framework, bunlar eşit değilse bir hata oluşturur. Aksi durumda yalnızca (belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zamanında sorunlarını izleme kolaylaştırır. Eşitleme denetimi özelliği, görüntülenecek hata iletisi yalnızca gördüğünüz ne neden olur.

Hatayı çözümlemek için iki yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır. Model ve veritabanı şeması birlikte hızla geliştirilebilen izin verdiğinden bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken çok yararlı olur. Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bu nedenle, *yoksa* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz!
2. Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin. Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır. Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.

Bu öğreticide, ilk yaklaşımı kullanacağız — Entity Framework Code model değişiklikleri her zaman otomatik olarak veritabanını yeniden oluşturma First sahip olacaksınız.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Otomatik olarak Model değişiklikleri veritabanını yeniden oluşturma

Code First otomatik olarak bırakır ve uygulama için model değiştirme herhangi bir zamanda veritabanını yeniden oluşturur, uygulamayı güncelleştirelim.

> [!NOTE] 
> 
> **Uyarı** otomatik olarak bırakarak ve yalnızca geliştirme veya test veritabanını kullanırken veritabanı yeniden oluşturarak bu yaklaşım etkinleştirmeniz gerekir ve *hiçbir zaman* gerçek verileri içeren bir üretim veritabanında. Bir üretim sunucusunda kullanarak veri kaybına neden olabilir.


İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.

![](adding-a-new-field/_static/image2.png)

' % S'sınıfı "MovieInitializer" olarak adlandırın. Güncelleştirme `MovieInitializer` sınıfı aşağıdaki kodu içerir:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

`MovieInitializer` Sınıfı model tarafından kullanılan veritabanı bırakılacak ve model sınıfları hiç olmadığı kadar değiştirirseniz otomatik olarak yeniden oluşturulan olduğunu belirtir. Kod içeren bir `Seed` yöntemi otomatik olarak herhangi bir veritabanına eklemek için bazı varsayılan veri süresi belirtmek için oluşturduğu (veya yeniden oluşturulduğunda). Bu, bunu değiştirmek için bir model yaptığınız her zaman el ile doldurmak üzere gerek kalmadan bazı örnek verilerle bir veritabanını doldurmak için kullanışlı bir yol sağlar.

Tanımladığınız göre `MovieInitializer` sınıfı, uygulama her çalıştırıldığında, bu model sınıfları veritabanında şemasından farklı olup olmadığını denetler. böylece yedekleme wire olmak isteyeceksiniz. Böyle bir durumda, model eşleşen ve ardından örnek verileriyle veritabanını doldurmak için veritabanını yeniden oluşturmak için Başlatıcı çalıştırabilirsiniz.

Açık *Global.asax* kökünde dosya `MvcMovies` proje:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

*Global.asax* dosyasını içeren proje için uygulamanın tamamını tanımlar ve içeren sınıf bir `Application_Start` uygulama ilk kez başlatıldığında çalıştırılan olay işleyicisi.

İki ekleyelim using deyimlerini dosyanın üstüne. İlk Entity Framework ad alanı başvuruyor ve ikinci bir ad alanı başvuruları burada bizim `MovieInitializer` hayatını sınıfı:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Ardından bulun `Application_Start` yöntemi ve bir çağrı ekleyin `Database.SetInitializer` aşağıda gösterildiği gibi yöntemin başında:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

`Database.SetInitializer` Eklediğiniz bir ifadeyi gösterir veritabanı tarafından kullanılan `MovieDBContext` örneği otomatik olarak silinecek ve şema ve veritabanı eşleşmiyorsa yeniden oluşturulacak. Ve gördüğünüz gibi ayrıca veritabanını belirtilen örnek verilerle doldurursunuz `MovieInitializer` sınıfı.

Kapat *Global.asax* dosya.

Uygulamayı yeniden çalıştırın ve gidin */Movies* URL'si. Uygulama başladığında, model yapısını artık veritabanı şemasını eşleştiğini algılar. Otomatik olarak yeni model yapısı için veritabanını yeniden oluşturur ve örnek filmler veritabanıyla doldurur:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı. Derecelendirme ekleyebilirsiniz unutmayın.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

**Oluştur**'u tıklatın. Yeni film derecelendirmesi dahil olmak üzere artık listeleme filmleri gösterilir:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

Bu bölümde nasıl model nesneleri değiştirebilir ve veritabanı değişiklikleri ile eşitlenmiş halde tutun gördünüz. Ayrıca senaryolarını deneyebilirsiniz yeni oluşturulan bir veritabanı örnek verilerle doldurmak için bir yol öğrendiniz. Ardından, nasıl model sınıfları için daha zengin Doğrulama mantığı eklemenize ve uygulanacak bazı iş kurallarını etkinleştirme sırasında bakalım.

> [!div class="step-by-step"]
> [Önceki](examining-the-edit-methods-and-edit-view.md)
> [İleri](adding-validation-to-the-model.md)
