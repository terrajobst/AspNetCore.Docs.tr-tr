---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Yeni alan ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 87bb2c5f64e714268f5e2631b44fbb8a93a6a4b6
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578100"
---
<a name="adding-a-new-field"></a>Yeni alan ekleme
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde değişiklik veritabanına uygulanır. Bu nedenle, bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First Migrations'ı kullanacaksınız.

Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler. Entity Framework, bunlar eşit değilse bir hata oluşturur. Aksi durumda yalnızca (belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zamanında sorunlarını izleme kolaylaştırır.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Code First Migrations ayarlama Model değişiklikleri

Çözüm Gezgini'ne gidin. Sağ tıklayın *Movies.mdf* seçin ve dosya **Sil** filmler veritabanını kaldırmak için. Görmüyorsanız *Movies.mdf* dosya, tıklayarak **tüm dosyaları göster** aşağıda kırmızı anahat içinde gösterilen simge.

![](adding-a-new-field/_static/image1.png)

Hiçbir hata olmadığından emin olmak için uygulama oluşturun.

Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.

![Paketi Man Ekle](adding-a-new-field/_static/image2.png)

İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istem girin

-ContextTypeName MvcMovie.Models.MovieDBContext geçişleri etkinleştir

![](adding-a-new-field/_static/image3.png)

**Etkinleştir geçişleri** (yukarıda gösterilen) bir komut oluşturur bir *Configuration.cs* yeni dosya *geçişler* klasör.

![](adding-a-new-field/_static/image4.png)

Visual Studio açılır *Configuration.cs* dosya. Değiştirin `Seed` yönteminde *Configuration.cs* dosyasındaki kodu aşağıdaki kodla:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Altında kırmızı dalgalı çizgi üzerine `Movie` tıklatıp `Show Potential Fixes` ve ardından **kullanarak** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Bunun yapılması ekler aşağıdaki using deyimi:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya varsa ekler satırları güncelleştirir. Bunlar henüz yoktur.
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi aşağıdaki kodda bir "upsert" işlem gerçekleştirir:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Çünkü [çekirdek](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) yöntemi, her geçiş ile çalışır, eklemeye çalıştığınız satırların zaten var. veritabanı oluşturan ilk geçişten sonra olacağından, verileri yalnızca ekleyemezsiniz. "[Upsert](http://en.wikipedia.org/wiki/Upsert)" işlemi zaten var olan bir satır eklemeye çalışırsanız olacağını hataları engeller, ancak bu, uygulamayı test ederken yaptığınız değişiklikler geçersiz kılar. Bazı tablolar test verileri, bunun gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi veritabanı güncelleştirmelerinden sonra kalmasını istiyor. Bu durumda koşullu ekleme işlemi yapmak istediğiniz: yalnızca zaten mevcut değilse bir satır ekleyin.   
> 
> Geçirilen ilk parametre [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) özelliği bir satır zaten mevcut olup olmadığını denetlemek için kullanılacak yöntemi belirtir. Sağlama, test film verileri için `Title` özelliği listedeki her başlık benzersiz olduğundan bu amaç için kullanılabilir:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Bu kod, başlıklar benzersiz olduğunu varsayar. Yinelenen başlığa el ile eklerseniz, sonraki açışınızda bir geçiş gerçekleştirmek şu özel durum alırsınız.   
> 
>  *Birden fazla öğe dizisi içeriyor*  
> 
> Hakkında daha fazla bilgi için [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi bkz [EF 4.3 AddOrUpdate yöntemiyle ilgileniriz](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Bu noktada yapı yoksa aşağıdaki adımları başarısız olur.)

Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf. Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* dosya önceki bir adımda.

İçinde **Paket Yöneticisi Konsolu** penceresinde komutu girin `add-migration Initial` ilk geçiş oluşturmak için. ' % S'adı "Başlangıç" isteğe bağlıdır ve oluşturulan geçiş dosyasının adı için kullanılır.

![](adding-a-new-field/_static/image6.png)

Code First geçişleri başka bir sınıf dosyasında oluşturur *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodu içerir. Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabit. İnceleme *{tarih damgası}\_Initial.cs* dosyasını oluşturmak için yönergeleri içeren `Movies` film DB için tablo. Aşağıda, bu yönergeleri veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şema oluşturun. Ardından **çekirdek** yöntemi, bir veritabanı test verileri ile doldurmak için çalışır.

İçinde **Paket Yöneticisi Konsolu**, komutu girin `update-database` veritabanı oluşturmak ve çalıştırmak için `Seed` yöntemi.

![](adding-a-new-field/_static/image7.png)

Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, veritabanını ve yürüttüğünüz önce uygulamayı çalıştırdığınız için büyük olasılıkla olduğu `update-database`. Bu durumda, silme *Movies.mdf* yeniden dosya ve yeniden deneyin `update-database` komutu. Hata almaya devam ediyorsanız geçişleri klasörünü ve içeriğini silin daha sonra bu sayfanın üst kısmındaki yönergeleri ile başlatın (delete olan *Movies.mdf* dosya sonra Enable-geçişler için devam edin). Bir yayımlandığında almaya devam ediyorsanız SQL Server nesne Gezgini'ni açın ve veritabanı listeden kaldırın.

Uygulamayı çalıştırmak ve gidin */Movies* URL'si. Çekirdek veriler görüntülenir.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeli derecelendirme özellik ekleme

Yeni bir ekleyerek başlangıç `Rating` varolan özellik `Movie` sınıfı. Açık *Models\Movie.cs* dosya ve ekleme `Rating` bunun gibi özelliği:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Tam `Movie` sınıfı şimdi aşağıdaki aşağıdaki kod gibi görünür:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

(Ctrl + Shift + B) uygulaması oluşturun.

Yeni bir alan eklediğiniz çünkü `Movie` sınıfı da ihtiyacınız bağlama güncelleştirilecek *beyaz liste* bu yeni özellik dahil edilecek şekilde. Güncelleştirme `bind` özniteliğini `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Ayrıca görüntülemek, oluşturmak ve bunları yeni düzenleme görünümü şablonları güncelleştirmeye gerek duyduğunuz `Rating` Tarayıcı Görünümü özelliği.

Açık *\Views\Movies\Index.cshtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonrasına sütun başlığı **fiyat** sütun. Ardından Ekle bir `<td>` sütun oluşturmak için şablon sonlarında `@item.Rating` değeri. Hangi güncelleştirilmiş aşağıdadır *Index.cshtml* görünüm şablonu şöyle:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Ardından, açık *\Views\Movies\Create.cshtml* dosya ve ekleme `Rating` aşağıdaki highlighed biçimlendirmeye sahip alan. Yeni bir film oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Artık uygulama kodu yeni destekleyecek şekilde güncelleştirdik `Rating` özelliği.

Uygulamayı çalıştırmak ve gidin */Movies* URL'si. Ancak, bunu yaptığınızda, aşağıdaki hatalardan birini görürsünüz:

![](adding-a-new-field/_static/image9.png)  
  
Veritabanı oluşturulduktan sonra 'MovieDBContext' bağlam yedekleme modeli değişti. Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Çünkü bu hatayı görüyorsunuz güncelleştirilmiş `Movie` model sınıfı uygulama şemasını farklı artık `Movie` mevcut veritabanı tablosu. (Yok hiçbir `Rating` veritabanı tablosundaki sütun.)


Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır. Bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken zaman Geliştirme döngüsünün başlarında çok kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır. Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bu nedenle, *yoksa* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz! Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır. Entity Framework veritabanı başlatıcılar hakkında daha fazla bilgi için bkz. [ASP.NET MVC/Entity Framework öğretici](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin. Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır. Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.
3. Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.


Bu öğreticide, Code First Migrations kullanacağız.

Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar. Migrations\Configuration.cs dosyasını açın ve her bir nesnenin film Derecelendirme alanı ekleyin.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:

`add-migration Rating`

`add-migration` Komutu geçerli bir film veritabanı şeması ile geçerli film modeli inceleyin ve DB yeni modeline geçirme için gereken kodu oluşturmak için geçiş framework bildirir. Adı *derecelendirme* isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır. Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.

Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMigration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Çözümü derleyin ve enter `update-database` komutunu **Paket Yöneticisi Konsolu** penceresi.

Çıktıda aşağıdaki resimde gösterilmektedir **Paket Yöneticisi Konsolu** penceresi (tarih damgası eklenmesini *derecelendirme* farklı olacaktır.)

![](adding-a-new-field/_static/image11.png)

Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin. Yeni derecesi alanını görebilirsiniz.

![](adding-a-new-field/_static/image12.png)

Tıklayın **Yeni Oluştur** yeni bir film eklenecek bağlantı. Derecelendirme ekleyebilirsiniz unutmayın.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

**Oluştur**'u tıklatın. Yeni film derecelendirmesi dahil olmak üzere artık listeleme filmleri gösterilir:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Proje geçişleri kullanıyorsa, veritabanını yeni bir alan eklediğinizde veya yoksa şemayı güncelleştirmenin bırakma gerekmez. Sonraki bölümde daha fazla şema değişiklikleri yapın ve geçişleri veritabanını güncellemek için kullanırız.

De eklemeniz gerekir `Rating` Düzenle, Ayrıntılar ve Sil görünüm şablonları alanı.

"Update-veritabanı" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve hiçbir geçiş kodu çalıştırın, şema modeli ile eşleştiği için. Bununla birlikte, "veritabanını güncelleştir" çalıştıran çalışır `Seed` yeniden yöntemi ve çekirdek veri birini değiştirdiyseniz, çünkü kaybedilir `Seed` yöntemi upsert eder veri. Daha fazla bilgi edinebilirsiniz `Seed` yönteminde Tom Dykstra'nın popüler [ASP.NET MVC/Entity Framework öğretici](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Bu bölümde nasıl model nesneleri değiştirebilir ve veritabanı değişiklikleri ile eşitlenmiş halde tutun gördünüz. Ayrıca senaryolarını deneyebilirsiniz yeni oluşturulan bir veritabanı örnek verilerle doldurmak için bir yol öğrendiniz. Bu yalnızca hızlı bir giriş Code First için bkz: [bir ASP.NET MVC uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) konu ile ilgili daha kapsamlı bir öğretici. Ardından, nasıl model sınıfları için daha zengin Doğrulama mantığı eklemenize ve uygulanacak bazı iş kurallarını etkinleştirme sırasında bakalım.

> [!div class="step-by-step"]
> [Önceki](adding-search.md)
> [İleri](adding-validation.md)
