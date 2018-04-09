---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Yeni bir alan film modeli ve tablo ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Yeni bir alan film modeli ve tablo ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.


Bu bölümde değişiklik veritabanına uyguladınız bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First geçişleri kullanın.

Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler. Entity Framework eşitlenmiş durumda değilse, bir hata oluşturur. Bu, aksi halde yalnızca (tarafından belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zaman sorunları izlemek kolaylaştırır.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Model değişiklikleri için Code First Migrations kurulması

Visual Studio 2012 kullanıyorsanız, çift tıklayarak *Movies.mdf* veritabanı Aracı'nı açmak için Çözüm Gezgini'nden dosya. Web için Visual Studio Express veritabanı Gezgini, Visual Studio 2012 Sunucu Gezgini gösterecektir gösterir. Visual Studio 2010 kullanıyorsanız, SQL Server nesne Gezgini'ni kullanın.

Veritabanı Aracı'nda (veritabanı Gezgini, Sunucu Gezgini veya SQL Server Nesne Gezgini) sağ tıklayın `MovieDBContext` seçip **silmek** filmler veritabanı bırakılamadı.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Çözüm Gezgini'ne gidin. Sağ tıklayın *Movies.mdf* dosya ve seçin **silmek** filmler veritabanını kaldırmak için.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Hiçbir hata bulunmadığından emin olmak için uygulamayı oluşturun.

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

![Paketi adam ekleme](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istemi "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext" girin.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-Migrations** (yukarıda gösterilen) komut oluşturur bir *Configuration.cs* yeni bir dosyada *geçişler* klasör.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio açılır *Configuration.cs* dosya. Değiştir `Seed` yönteminde *Configuration.cs* aşağıdaki kod ile dosya:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Altında kırmızı kırık çizgi sağ tıklayın `Movie` seçip **gidermek** sonra **kullanarak** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Bunun yapılması ekler aşağıdaki using deyimi:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **update-database** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya if ekler satır güncelleştirir. Bunlar henüz mevcut değil.


**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Aşağıdaki adımları başarısız olur, bu noktada yapı yoktur.)

Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf. Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* bir önceki adımda dosya.

İçinde **Paket Yöneticisi Konsolu** penceresinde "add-migration ilk" komutu girin ilk geçiş oluşturmak için. "Başlangıç" adı isteğe bağlıdır ve oluşturulan geçiş dosya adı için kullanılır.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First geçişleri oluşturur başka bir sınıf dosyasında *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodunu içerir. Geçiş filename damgasıyla sıralama ile yardımcı olmak için önceden düzeltilmiştir. İncelemek *{tarih damgası}\_Initial.cs* dosyası için film DB filmler tablo oluşturmak için yönergeleri içerir. Aşağıda, bu yönergeler veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şeması oluşturun. Ardından **çekirdek** yöntemi DB test verileri ile doldurmak için çalışacak.

İçinde **Paket Yöneticisi Konsolu**, komut "Güncelleştirme-veritabanı oluşturmak ve çalıştırmak için veritabanı" girin **çekirdek** yöntemi.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, uygulama veritabanı silinmiş sonra ve, yürütülmeden önce çalışan, büyük olasılıkla çünkü `update-database`. Bu durumda, silme *Movies.mdf* dosyasını yeniden ve yeniden deneme `update-database` komutu. Hala bir hata alırsanız, geçişler klasörünü ve içeriğini silin sonra bu sayfanın üst kısmındaki yönergeleri ile başlayın (delete olan *Movies.mdf* dosya sonra Enable-Migrations devam).

Uygulamayı çalıştırın ve gidin */Movies* URL. Çekirdek verileri görüntülenir.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeline derecelendirme özellik ekleme

Yeni bir eklemeye başlayın `Rating` varolan özelliği `Movie` sınıfı. Açık *Models\Movie.cs* dosya ve ekleme `Rating` özelliği aşağıdakine benzer:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Tam `Movie` sınıfında aşağıdaki kodu şimdi görülüyor:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Uygulamayı kullanarak yapı **yapı** &gt; **yapı film** menü komutunu veya CTRL-SHIFT-b tuşuna basarak

Güncelleştirdiğinizden `Model` sınıfı, ayrıca gerekir güncelleştirmek *\Views\Movies\Index.cshtml* ve *\Views\Movies\Create.cshtml* yeni görüntülemekiçingörüntülemeşablonları`Rating`tarayıcı görünümünde özelliği.

Açık<em>\Views\Movies\Index.cshtml</em> dosya ve ekleme bir `<th>Rating</th>` hemen sonra sütun başlığı <strong>fiyat</strong> sütun. Ardından ekleyin bir `<td>` sütun oluşturmak için şablon sona `@item.Rating` değeri. Hangi güncelleştirilmiş aşağıdadır <em>Index.cshtml</em> şablonu görüntüleme gibi görünüyor:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Ardından, açık *\Views\Movies\Create.cshtml* dosyasını ve form sona aşağıdaki biçimlendirmeyi ekleyin. Yeni bir filmi oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Şimdi yeni desteklemek için uygulama kodu güncelleştirdik `Rating` özelliği.

Uygulamayı çalıştırın ve gidin artık */Movies* URL. Ancak, bunu yaptığınızda, aşağıdaki hatalardan biri görürsünüz:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Bu hata nedeniyle görüyorsunuz güncelleştirilmiş `Movie` uygulama modeli sınıfında şeması farklı şimdi `Movie` var olan veritabanının tablo. (Var. hiçbir `Rating` veritabanı tablosundaki sütun.)


Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır. Bu yaklaşım, bir test veritabanı üzerindeki etkin geliştirme yaparken çok kullanışlı olur; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar. Dezavantajı rağmen veritabanında var olan veri kaybı olan — böylece, *yok* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz! Otomatik olarak test verileri olan bir veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir yol görülür. Entity Framework veritabanı başlatıcıları hakkında daha fazla bilgi için bkz: zel Dykstra'nın [ASP.NET MVC/Entity Framework Öğreticisi](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin. Bu yaklaşımın avantajı, verilerinizi tutmak ' dir. Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.
3. Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.


Bu öğretici için Code First Migrations kullanacağız.

Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar. Migrations\Configuration.cs dosyasını açın ve her film nesnesine bir derecelendirme alanı ekleyin.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:

`add-migration AddRatingMig`

`add-migration` Komutu geçerli film DB şeması geçerli film modeliyle inceleyin ve yeni modele DB geçirmek için gerekli kodu oluşturmak için geçiş framework bildirir. AddRatingMig rastgeledir ve geçiş dosya adı için kullanılır. Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.

Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Çözümü derlemek ve "update-database" komutta enter **Paket Yöneticisi Konsolu** penceresi.

Aşağıdaki resimde çıktıda gösterir **Paket Yöneticisi Konsolu** penceresi (AddRatingMig eklenmesini tarih damgası farklı olacaktır.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin. Yeni Derecelendirme alanı görebilirsiniz.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı. Bir derecelendirme ekleyebileceğinizi unutmayın.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

**Oluştur**'u tıklatın. Derecelendirme dahil olmak üzere yeni film şimdi listeleme filmler içinde görüntülenir:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Ayrıca eklemelisiniz `Rating` görünüm şablonları düzenleme, Ayrıntılar ve SearchIndex alan.

"Update-database" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve hiçbir değişiklik yapılan, şema modeli eşleştiğinden.

Bu bölümde nasıl model nesneleri değiştirmek ve veritabanı değişiklikleri eşit tutmak gördünüz. Ayrıca, senaryolarını deneyebilirsiniz şekilde yeni oluşturulan bir veritabanını örnek verilerle doldurmak için bir yöntem de öğrendiniz. Ardından, nasıl daha zengin doğrulama mantığını model sınıfları ekleyebilir ve zorlanacak bazı iş kurallarını etkinleştirme sırasında bakalım.

> [!div class="step-by-step"]
> [Önceki](examining-the-edit-methods-and-edit-view.md)
> [sonraki](adding-validation-to-the-model.md)
