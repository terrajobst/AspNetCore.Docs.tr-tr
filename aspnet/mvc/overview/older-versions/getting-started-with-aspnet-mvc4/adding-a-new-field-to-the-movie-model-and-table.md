---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Film modeli ve tablosuna yeni alan ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 2435bb50bb8124cce3150ba488ad76c012107b27
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840182"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Film modeli ve tablosuna yeni alan ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.


Bu bölümde değişiklik veritabanına uygulanır. Bu nedenle, bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First Migrations'ı kullanacaksınız.

Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler. Entity Framework, bunlar eşit değilse bir hata oluşturur. Aksi durumda yalnızca (belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zamanında sorunlarını izleme kolaylaştırır.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Code First Migrations ayarlama Model değişiklikleri

Visual Studio 2012 kullanıyorsanız, çift tıklayarak *Movies.mdf* veritabanı aracını açmak için Çözüm Gezgini'nden bir dosya. Web için Visual Studio Express veritabanı Gezgini, Visual Studio 2012 Sunucu Gezgini gösterecektir gösterilir. Visual Studio 2010 kullanıyorsanız, SQL Server nesne Gezgini'ni kullanın.

Veritabanı Aracı'nda (veritabanı Gezgini, Sunucu Gezgini veya SQL Server Nesne Gezgini) sağ tıklayın `MovieDBContext` seçip **Sil** filmler veritabanını bırakmak için.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Çözüm Gezgini'ne geri gidin. Sağ tıklayın *Movies.mdf* seçin ve dosya **Sil** filmler veritabanını kaldırmak için.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Hiçbir hata olmadığından emin olmak için uygulama oluşturun.

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.

![Paketi Man Ekle](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istemi "Enable-geçişleri - ContextTypeName MvcMovie.Models.MovieDBContext" girin.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Etkinleştir geçişleri** (yukarıda gösterilen) bir komut oluşturur bir *Configuration.cs* yeni dosya *geçişler* klasör.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio açılır *Configuration.cs* dosya. Değiştirin `Seed` yönteminde *Configuration.cs* dosyasındaki kodu aşağıdaki kodla:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Altında kırmızı dalgalı çizgi sağ tıklayın `Movie` seçip **çözmek** ardından **kullanarak** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Bunun yapılması ekler aşağıdaki using deyimi:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya varsa ekler satırları güncelleştirir. Bunlar henüz yoktur.


**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Aşağıdaki adımları başarısız olur, bu noktada oluşturmayın.)

Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf. Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* dosya önceki bir adımda.

İçinde **Paket Yöneticisi Konsolu** penceresinde "Ekle geçiş ilk" komutunu girin ilk geçiş oluşturmak için. ' % S'adı "Başlangıç" isteğe bağlıdır ve oluşturulan geçiş dosyasının adı için kullanılır.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First geçişleri başka bir sınıf dosyasında oluşturur *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodu içerir. Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabit. İnceleme *{tarih damgası}\_Initial.cs* dosyası, filmler tablo için bir film veritabanı oluşturmak için yönergeleri içerir. Aşağıda, bu yönergeleri veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şema oluşturun. Ardından **çekirdek** yöntemi, bir veritabanı test verileri ile doldurmak için çalışır.

İçinde **Paket Yöneticisi Konsolu**, komut "update-veritabanı oluşturmak ve çalıştırmak için veritabanı" girin **çekirdek** yöntemi.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, veritabanını ve yürüttüğünüz önce uygulamayı çalıştırdığınız için büyük olasılıkla olduğu `update-database`. Bu durumda, silme *Movies.mdf* yeniden dosya ve yeniden deneyin `update-database` komutu. Hata almaya devam ediyorsanız geçişleri klasörünü ve içeriğini silin daha sonra bu sayfanın üst kısmındaki yönergeleri ile başlatın (delete olan *Movies.mdf* dosya sonra Enable-geçişler için devam edin).

Uygulamayı çalıştırmak ve gidin */Movies* URL'si. Çekirdek veriler görüntülenir.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeli derecelendirme özellik ekleme

Yeni bir ekleyerek başlangıç `Rating` varolan özellik `Movie` sınıfı. Açık *Models\Movie.cs* dosya ve ekleme `Rating` bunun gibi özelliği:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Tam `Movie` sınıfı şimdi aşağıdaki aşağıdaki kod gibi görünür:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Kullanarak uygulama derleme **derleme** &gt; **derleme film** menü komutunu ya da CTRL-SHIFT-b tuşuna basarak

Güncelleştirdiğinize göre `Model` sınıfı da ihtiyacınız güncelleştirilecek *\Views\Movies\Index.cshtml* ve *\Views\Movies\Create.cshtml* yeni görüntülemekiçingörüntülemeşablonları`Rating`Tarayıcı Görünümü özelliği.

Açık<em>\Views\Movies\Index.cshtml</em> dosya ve ekleme bir `<th>Rating</th>` hemen sonrasına sütun başlığı <strong>fiyat</strong> sütun. Ardından Ekle bir `<td>` sütun oluşturmak için şablon sonlarında `@item.Rating` değeri. Hangi güncelleştirilmiş aşağıdadır <em>Index.cshtml</em> görünüm şablonu şöyle:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Ardından, açık *\Views\Movies\Create.cshtml* dosya ve formun sonlarında aşağıdaki işaretlemeyi ekleyin. Yeni bir film oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Artık uygulama kodu yeni destekleyecek şekilde güncelleştirdik `Rating` özelliği.

Şimdi uygulamayı çalıştırabilir ve gidin */Movies* URL'si. Ancak, bunu yaptığınızda, aşağıdaki hatalardan birini görürsünüz:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Çünkü bu hatayı görüyorsunuz güncelleştirilmiş `Movie` model sınıfı uygulama şemasını farklı artık `Movie` mevcut veritabanı tablosu. (Yok hiçbir `Rating` veritabanı tablosundaki sütun.)


Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır. Bu yaklaşım, bir test veritabanında etkin geliştirme işi yaparken, kullanışlı olur; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır. Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bu nedenle, *yoksa* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz! Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır. Entity Framework veritabanı başlatıcılar hakkında daha fazla bilgi için bkz: Tom Dykstra'nın [ASP.NET MVC/Entity Framework öğretici](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin. Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır. Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.
3. Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.


Bu öğreticide, Code First Migrations kullanacağız.

Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar. Migrations\Configuration.cs dosyasını açın ve her bir nesnenin film Derecelendirme alanı ekleyin.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:

`add-migration AddRatingMig`

`add-migration` Komutu geçerli bir film veritabanı şeması ile geçerli film modeli inceleyin ve DB yeni modeline geçirme için gereken kodu oluşturmak için geçiş framework bildirir. AddRatingMig isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır. Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.

Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMIgration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Çözümü derleyin ve ardından "veritabanını güncelleştir" komut girin **Paket Yöneticisi Konsolu** penceresi.

Çıktıda aşağıdaki resimde gösterilmektedir **Paket Yöneticisi Konsolu** penceresi (AddRatingMig eklenmesini tarih damgası farklı olacaktır.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin. Yeni derecesi alanını görebilirsiniz.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı. Derecelendirme ekleyebilirsiniz unutmayın.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

**Oluştur**'u tıklatın. Yeni film derecelendirmesi dahil olmak üzere artık listeleme filmleri gösterilir:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

De eklemeniz gerekir `Rating` alan düzenleme, ayrıntıları ve SearchIndex şablonları göster.

"Update-veritabanı" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve herhangi bir değişiklik yapılacak, şema modeli ile eşleştiği için.

Bu bölümde nasıl model nesneleri değiştirebilir ve veritabanı değişiklikleri ile eşitlenmiş halde tutun gördünüz. Ayrıca senaryolarını deneyebilirsiniz yeni oluşturulan bir veritabanı örnek verilerle doldurmak için bir yol öğrendiniz. Ardından, nasıl model sınıfları için daha zengin Doğrulama mantığı eklemenize ve uygulanacak bazı iş kurallarını etkinleştirme sırasında bakalım.

> [!div class="step-by-step"]
> [Önceki](examining-the-edit-methods-and-edit-view.md)
> [İleri](adding-validation-to-the-model.md)
