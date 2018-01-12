---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Yeni bir alan ekleyerek | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 7427b4f7c6b7a00fe795053aac0f612471a163cd
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
<a name="adding-a-new-field"></a>Yeni bir alan ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

Bu bölümde değişiklik veritabanına uyguladınız bazı değişiklikler model sınıflarına geçirmek için Entity Framework Code First geçişleri kullanın.

Bu öğreticide daha önce yaptığınız gibi Entity Framework Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda varsayılan olarak, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler. Entity Framework eşitlenmiş durumda değilse, bir hata oluşturur. Bu, aksi halde yalnızca (tarafından belirsiz hatalar) çalışma zamanında bulabileceğiniz geliştirme zaman sorunları izlemek kolaylaştırır.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Model değişiklikleri için Code First Migrations kurulması

Çözüm Gezgini'ne gidin. Sağ tıklayın *Movies.mdf* dosya ve seçin **silmek** filmler veritabanını kaldırmak için. Görmüyorsanız, *Movies.mdf* dosya, tıklayın **tüm dosyaları göster** kırmızı hattaki aşağıda simgesi.

![](adding-a-new-field/_static/image1.png)

Hiçbir hata bulunmadığından emin olmak için uygulamayı oluşturun.

Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi** ve ardından **Paket Yöneticisi Konsolu**.

![Paketi adam ekleme](adding-a-new-field/_static/image2.png)

İçinde **Paket Yöneticisi Konsolu** penceresine `PM>` istem girin

Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-Migrations** (yukarıda gösterilen) komut oluşturur bir *Configuration.cs* yeni bir dosyada *geçişler* klasör.

![](adding-a-new-field/_static/image4.png)

Visual Studio açılır *Configuration.cs* dosya. Değiştir `Seed` yönteminde *Configuration.cs* aşağıdaki kod ile dosya:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Altında kırmızı kırık çizgi üzerine gelerek `Movie` tıklatıp `Show Potential Fixes` ve ardından **kullanarak** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Bunun yapılması ekler aşağıdaki using deyimi:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations çağrıları `Seed` yöntemi her geçişten sonra (diğer bir deyişle, çağırma **update-database** Paket Yöneticisi konsolunda), ve bu yöntem zaten eklenmiş veya if ekler satır güncelleştirir. Bunlar henüz mevcut değil.
> 
> [Örnek](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi aşağıdaki kodda bir "upsert" işlemi gerçekleştirir:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Çünkü [çekirdek](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx) yöntemi her geçiş ile çalışır, eklemeye çalıştığınız satır zaten var. bir veritabanı oluşturur ilk geçişten sonra olacağından, verileri, yalnızca ekleyemezsiniz. "[Upsert](http://en.wikipedia.org/wiki/Upsert)" işlemi zaten bir satır eklemeye çalışırsanız, olacağını hataları engeller, ancak uygulamayı test ederken yapmış olabileceğiniz veri değişiklikleri geçersiz kılar. Bazı tablolardaki test verilerle, gerçekleşmesi için istemeyebilirsiniz: Bazı durumlarda test ederken verileri değiştirdiğinizde değişikliklerinizi sonra veritabanı güncelleştirmeleri kalmasını istiyor. Bu durumda bir koşullu ekleme işlemi yapmak istiyor: yalnızca zaten yoksa bir satır ekleyin.   
>   
> Geçirilen ilk parametre [örnek](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi bir satır zaten var olup olmadığını denetlemek için kullanılacak özellik belirtir. Sağlama, test film veriler için `Title` özellik listesinde her başlık benzersiz olduğundan bu amaç için kullanılabilir:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Bu kod titiles benzersiz olduğunu varsayar. Yinelenen bir başlık el ile eklerseniz, şu özel bir geçiş gerçekleştirmek sonraki açışınızda elde edersiniz.   
>   
>  *Sıra birden fazla öğe içeriyor*  
>   
> Hakkında daha fazla bilgi için [örnek](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) yöntemi, bkz: [ilgilenebilmek EF 4.3 örnek yöntemiyle](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Projeyi derlemek için CTRL-SHIFT-B tuşuna basın.** (Bu noktada yapı yok aşağıdaki adımları başarısız olur.)

Sonraki adım oluşturmaktır bir `DbMigration` ilk geçiş için sınıf. Bu geçiş neden olan yeni bir veritabanı oluşturur, silinen *movie.mdf* bir önceki adımda dosya.

İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu girin `add-migration Initial` ilk geçiş oluşturmak için. "Başlangıç" adı isteğe bağlıdır ve oluşturulan geçiş dosya adı için kullanılır.

![](adding-a-new-field/_static/image6.png)

Code First geçişleri oluşturur başka bir sınıf dosyasında *geçişler* klasörü (adıyla *{tarih damgası}\_Initial.cs* ), ve bu sınıf, veritabanı şemasını oluşturan kodunu içerir. Geçiş filename damgasıyla sıralama ile yardımcı olmak için önceden düzeltilmiştir. İncelemek *{tarih damgası}\_Initial.cs* dosyasını oluşturmaya ilişkin yönergeleri içeren `Movies` film DB tablo. Aşağıda, bu yönergeler veritabanında güncelleştirdiğinizde *{tarih damgası}\_Initial.cs* dosyasını çalıştırın ve DB şeması oluşturun. Ardından **çekirdek** yöntemi DB test verileri ile doldurmak için çalışacak.

İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin `update-database` veritabanını oluşturmak ve çalıştırmak için `Seed` yöntemi.

![](adding-a-new-field/_static/image7.png)

Bir tablo zaten var ve oluşturulamaz belirten bir hata alırsanız, uygulama veritabanı silinmiş sonra ve, yürütülmeden önce çalışan, büyük olasılıkla çünkü `update-database`. Bu durumda, silme *Movies.mdf* dosyasını yeniden ve yeniden deneme `update-database` komutu. Hala bir hata alırsanız, geçişler klasörünü ve içeriğini silin sonra bu sayfanın üst kısmındaki yönergeleri ile başlayın (delete olan *Movies.mdf* dosya sonra Enable-Migrations devam). Bir eror hala alırsanız, SQL Server nesne Gezgini'ni açın ve veritabanını listeden kaldırın.

Uygulamayı çalıştırın ve gidin */Movies* URL. Çekirdek verileri görüntülenir.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeline derecelendirme özellik ekleme

Yeni bir eklemeye başlayın `Rating` varolan özelliği `Movie` sınıfı. Açık *Models\Movie.cs* dosya ve ekleme `Rating` özelliği aşağıdakine benzer:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Tam `Movie` sınıfında aşağıdaki kodu şimdi görülüyor:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Uygulama (Ctrl + Shift + B) oluşturun.

Yeni bir alan eklediğiniz çünkü `Movie` sınıfı, ayrıca gerekir bağlama güncelleştirmek *beyaz liste* bu yeni özelliği eklenecek şekilde. Güncelleştirme `bind` için öznitelik `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Ayrıca görüntülemek, oluşturmak ve yeni düzenlemek için görünümü şablonlarını güncelleştirmek gereken `Rating` tarayıcı görünümünde özelliği.

Açık *\Views\Movies\Index.cshtml* dosya ve ekleme bir `<th>Rating</th>` hemen sonra sütun başlığı **fiyat** sütun. Ardından ekleyin bir `<td>` sütun oluşturmak için şablon sona `@item.Rating` değeri. Hangi güncelleştirilmiş aşağıdadır *Index.cshtml* şablonu görüntüleme gibi görünüyor:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Ardından, açık *\Views\Movies\Create.cshtml* dosya ve ekleme `Rating` aşağıdaki highlighed biçimlendirme içeren alan. Yeni bir filmi oluşturulduğunda bir derecelendirme belirtmek için bu bir metin kutusu oluşturur.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Şimdi yeni desteklemek için uygulama kodu güncelleştirdik `Rating` özelliği.

Uygulamayı çalıştırın ve gidin */Movies* URL. Ancak, bunu yaptığınızda, aşağıdaki hatalardan biri görürsünüz:

![](adding-a-new-field/_static/image9.png)  
  
Veritabanının oluşturulmasından 'MovieDBContext' bağlamını destekleyen model değişti. Veritabanı (https://go.microsoft.com/fwlink/?LinkId=238269) güncelleştirmek için Code First Migrations kullanmayı düşünün.

![](adding-a-new-field/_static/image10.png)

Bu hata nedeniyle görüyorsunuz güncelleştirilmiş `Movie` uygulama modeli sınıfında şeması farklı şimdi `Movie` var olan veritabanının tablo. (Var. hiçbir `Rating` veritabanı tablosundaki sütun.)


Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır. Bu yaklaşım erken zaman, bir test veritabanı üzerindeki etkin geliştirme yapmakta olduğunuz geliştirme döngüsü çok kolaydır; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar. Dezavantajı rağmen veritabanında var olan veri kaybı olan — böylece, *yok* bir üretim veritabanında bu yaklaşımı kullanmak istediğiniz! Otomatik olarak test verileri olan bir veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir yol görülür. Entity Framework veritabanı başlatıcıları hakkında daha fazla bilgi için bkz: [ASP.NET MVC/Entity Framework Öğreticisi](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin. Bu yaklaşımın avantajı, verilerinizi tutmak ' dir. Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.
3. Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.


Bu öğretici için Code First Migrations kullanacağız.

Seed yöntemi güncelleştirin, böylece yeni bir sütun için bir değer sağlar. Migrations\Configuration.cs dosyasını açın ve her film nesnesine bir derecelendirme alanı ekleyin.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi ve aşağıdaki komutu girin:

`add-migration Rating`

`add-migration` Komutu geçerli film DB şeması geçerli film modeliyle inceleyin ve yeni modele DB geçirmek için gerekli kodu oluşturmak için geçiş framework bildirir. Adı *derecelendirme* rastgeledir ve geçiş dosya adı için kullanılır. Geçiş adımı için anlamlı bir ad kullanmak yararlıdır.

Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMigration` türetilmiş sınıf hem de `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Çözümü derlemek ve enter `update-database` komutunu **Paket Yöneticisi Konsolu** penceresi.

Aşağıdaki resimde çıktıda gösterir **Paket Yöneticisi Konsolu** penceresi (tarih damgası eklenmesini *derecelendirme* farklı olacaktır.)

![](adding-a-new-field/_static/image11.png)

Uygulamayı yeniden çalıştırın ve /Movies URL'ye gidin. Yeni Derecelendirme alanı görebilirsiniz.

![](adding-a-new-field/_static/image12.png)

Tıklatın **Yeni Oluştur** yeni film eklemek için bağlantı. Bir derecelendirme ekleyebileceğinizi unutmayın.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

**Oluştur**'u tıklatın. Derecelendirme dahil olmak üzere yeni film şimdi listeleme filmler içinde görüntülenir:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Proje geçişler kullanarak, yeni bir alan eklediğinizde veya aksi halde güncelleştirme şeması veritabanını bırakın gerekmez. Sonraki bölümde, biz daha fazla şema değişiklikleri yapın ve veritabanını güncelleştirmek için geçişler kullanın.

Ayrıca eklemelisiniz `Rating` düzenleme, Ayrıntılar ve Delete görünüm şablonları alanı.

"Update-database" komutta girebilirsiniz **Paket Yöneticisi Konsolu** penceresini tekrar ve hiçbir geçiş kodu çalışıyor, şema modeli eşleştiğinden. Bununla birlikte, "update-database" çalıştıran çalışır `Seed` yeniden, yöntemi ve çekirdek veri birini değiştirdiyseniz, çünkü kaybedilir `Seed` yöntemi upserts veri. Daha fazla bilgi edinebilirsiniz `Seed` yönteminde zel Dykstra'nın popüler [ASP.NET MVC/Entity Framework Öğreticisi](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Bu bölümde nasıl model nesneleri değiştirmek ve veritabanı değişiklikleri eşit tutmak gördünüz. Ayrıca, senaryolarını deneyebilirsiniz şekilde yeni oluşturulan bir veritabanını örnek verilerle doldurmak için bir yöntem de öğrendiniz. Bu yalnızca bir giriş Code First, bkz: [bir ASP.NET MVC uygulaması için Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) konu hakkında daha kapsamlı bir öğretici. Ardından, nasıl daha zengin doğrulama mantığını model sınıfları ekleyebilir ve zorlanacak bazı iş kurallarını etkinleştirme sırasında bakalım.

>[!div class="step-by-step"]
[Önceki](adding-search.md)
[sonraki](adding-validation.md)
