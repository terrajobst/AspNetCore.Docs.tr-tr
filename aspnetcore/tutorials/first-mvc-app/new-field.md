---
title: Yeni bir alan ekleme
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 7b92d1b50311cedbae2bdf30a2dc19204c5b81d1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-new-field"></a>Yeni bir alan ekleme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) yeni bir alan modele eklemek ve bu geçirmek için Code First geçişleri veritabanına değiştirin.

EF Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler. EF eşitlenmiş durumda değilse, bir özel durum oluşturur. Bu, tutarsız veritabanı/kod sorunları bulmak kolaylaştırır.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeline derecelendirme özellik ekleme

Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

(Ctrl + Shift + B) uygulaması oluşturma.

Yeni bir alan eklediğiniz çünkü `Movie` sınıfı, siz de bu yeni özelliği eklenecek şekilde bağlama beyaz liste güncellemeniz gerekir. İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği her ikisi için de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Ayrıca görüntülemek, oluşturmak ve yeni düzenlemek için görünümü şablonlarını güncelleştirmek gereken `Rating` tarayıcı görünümünde özelliği.

Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan. Önceki "form grubu" kopyalayıp yapıştırın ve alanları güncelleştirmeye IntelliSense Yardım sağlar. IntelliSense çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro). Not: Visual Studio 2017'ın RTM sürümü yüklemeniz gerekir [Razor dili Hizmetleri](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) Razor IntelliSense için. Bu bir sonraki sürümde düzeltilecektir.

![Geliştirici asp öznitelik değerini Harf R yazdı-için görünümün ikinci etiket öğesindeki. IntelliSense bağlam menüsü listesinde otomatik olarak vurgulanmış derecelendirme dahil olmak üzere kullanılabilir alanlar gösteren görüntülendi. Geliştirici alanını tıklattığında veya klavyedeki Enter tuşuna bastığında, değer derecelendirme için ayarlanır.](new-field/_static/cr.png)

Uygulama, yeni alanı içerecek şekilde DB güncelleştiriyoruz kadar çalışmaz. Şimdi çalıştırırsanız, aşağıdaki elde edersiniz `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Güncelleştirilmiş film model sınıfı var olan veritabanının film tablonun şeması farklı olduğundan bu hata görüyorsunuz. (Veritabanı tablosunda hiçbir Derecelendirme sütunu var.)

Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır. Bu yaklaşım erken zaman, bir test veritabanı üzerindeki etkin geliştirme yapmakta olduğunuz geliştirme döngüsü çok kolaydır; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar. Dezavantajı rağmen veritabanında var olan veri kaybı olan — bir üretim veritabanında bu yaklaşımı kullanmak istemediğiniz için! Otomatik olarak test verileri olan bir veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir yol görülür.

2. Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin. Bu yaklaşımın avantajı, verilerinizi tutmak ' dir. Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.

3. Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.

Bu öğretici için Code First Migrations kullanacağız.

Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlayan sınıf. Bir örnek değişiklik aşağıda gösterilen, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Çözümü oluşturun.

Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.

  ![PMC menüsü](adding-model/_static/pmc.png)

PMC aşağıdaki komutları girin:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Komutu geçerli incelemek için geçiş framework bildirir `Movie` geçerli model `Movie` DB şeması ve yeni modele DB geçirmek için gerekli kodu oluşturun. Adı "Derecelendirme" rastgeledir ve geçiş dosya adı için kullanılır. Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.

Veritabanındaki tüm kayıtları silerseniz, başlatma DB Çekirdek ve dahil `Rating` alan. Tarayıcıda veya SSOX delete bağlantılar yapın.

Uygulamayı çalıştırın ve oluşturma/düzenleme/görüntüleme filmler ile yapabilecekleriniz doğrulayın bir `Rating` alan. Ayrıca eklemelisiniz `Rating` alanı `Edit`, `Details`, ve `Delete` görüntüleme şablonları.

>[!div class="step-by-step"]
[Önceki](search.md)
[sonraki](validation.md)  
