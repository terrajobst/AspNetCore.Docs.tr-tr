---
title: Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin
author: rick-anderson
description: Bir model için yeni bir alan ekleyin ve bu değişiklik veritabanına geçirmek için Entity Framework Code First Migrations'ı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: e58d5af90b997c66cb749ab8f1b2f8049b8f7303
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089692"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde kullanacaksınız [Entity Framework](/ef/core/get-started/aspnetcore/new-db) veritabanına Code First Migrations'modeli için yeni bir alan ekleme ve bu geçiş için değiştirin.

EF Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler. EF, bunlar eşit değilse bir özel durum oluşturur. Bu, tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeli derecelendirme özellik ekleme

Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

(Ctrl + Shift + B) uygulaması oluşturun.

Yeni bir alan eklediğiniz çünkü `Movie` sınıfı da gereken bağlama beyaz liste bu yeni özellik dahil olacak şekilde güncelleştirin. İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği hem de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Ayrıca görüntülemek, oluşturmak ve bunları yeni düzenleme görünümü şablonları güncelleştirmeye gerek duyduğunuz `Rating` Tarayıcı Görünümü özelliği.

Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan. Önceki "form grubu" kopyala/yapıştır ve alanları güncelleştirin IntelliSense Yardım sağlar. IntelliSense ile birlikte çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro). Not: Visual Studio 2017'in RTM sürümü yüklemeniz gerekir [Razor dil Hizmetleri](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) Razor IntelliSense. Bu, sonraki sürümde düzeltilecektir.

![Öznitelik değeri ASP R harfiyle Geliştirici yazdığını-için görünümün ikinci etiket öğesinde. IntelliSense bağlam menüsü listesinde otomatik olarak vurgulanır derecelendirme dahil olmak üzere kullanılabilir alanları gösteren görüntülendi. Geliştirici alana tıkladığında veya klavyedeki Enter tuşuna bastığında için derecelendirme değeri ayarlanır.](new-field/_static/cr.png)

Uygulama, yeni alan eklemek için bir veritabanı güncelleştiriyoruz kadar çalışmaz. Şimdi çalıştırırsanız, aşağıdaki elde edecekleriniz `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Güncelleştirilmiş film modeli sınıfı var olan veritabanının film tablonun şeması farklı olduğu için bu hatayı görüyorsunuz. (Veritabanı tablosunda hiçbir derecesi sütun var.)

Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır. Bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken zaman Geliştirme döngüsünün başlarında çok kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır. Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bir üretim veritabanında bu yaklaşımı kullanmak istemediğiniz şekilde! Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır.

2. Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin. Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır. Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.

3. Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.

Bu öğreticide, Code First Migrations kullanacağız.

Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını. Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Çözümü oluşturun.

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.

  ![PMC menüsü](adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Komut Framework'e geçerli incelemek için geçiş `Movie` geçerli model `Movie` DB şema ve DB yeni modeline geçirme için gereken kodu oluşturun. Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır. Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.

DB tüm kayıtların silerseniz, başlatma DB çekirdeğini ve dahil `Rating` alan. Tarayıcıda veya SSOX silme bağlantıları kullanarak bunu yapabilirsiniz.

Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan. De eklemeniz gerekir `Rating` alanı `Edit`, `Details`, ve `Delete` şablonlarını görüntüleyin.

> [!div class="step-by-step"]
> [Önceki](search.md)
> [İleri](validation.md)  
