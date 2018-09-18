---
title: Doğrulama için bir ASP.NET Core Razor sayfası ekleme
author: rick-anderson
description: ASP.NET Core Razor sayfasına doğrulamanın nasıl keşfedin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/validation
ms.openlocfilehash: e27253e3f02fa808136494ed5373059ef6e0df0d
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011600"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>Doğrulama için bir ASP.NET Core Razor sayfası ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, doğrulama mantığı eklenen `Movie` modeli. Doğrulama kuralları, bir kullanıcı oluşturur veya düzenler film dilediğiniz zaman uygulanır.

## <a name="validation"></a>Doğrulama

Bir anahtar uyarlamanız yazılım geliştirme adlı [KURU](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**aşağıdakilerden **R**epeat **Y**ourself"). Razor sayfaları burada işlevselliği bir kez belirtildiğinden ve uygulama boyunca yansıtılır geliştirme teşvik eder. KURU, bir uygulamada kod miktarını azaltmaya yardımcı olabilir. KURU kod daha az hata yapmaya açık ve test etmek ve sürdürmek daha kolay sağlar.

Razor sayfaları ve Entity Framework tarafından sağlanan doğrulama desteği, KURU İlkesi iyi bir örnektir. Doğrulama kuralları (model sınıfında) tek bir yerde bildirimli olarak belirtilir ve kuralları uygulamada her yerde uygulanır.

### <a name="adding-validation-rules-to-the-movie-model"></a>Film modeli doğrulama kuralları ekleme

Açık *Movie.cs* dosya. [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) yerleşik bir sınıf ya da özellik bildirimli olarak uygulanan doğrulama öznitelikleri kümesi sağlar. DataAnnotations gibi biçimlendirme öznitelikleri de içeren `DataType` biçimlendirmesinde yardımcı olabilecek ve doğrulama sağlaması gerekmez.

Güncelleştirme `Movie` yararlanmak için sınıf `Required`, `StringLength`, `RegularExpression`, ve `Range` doğrulama öznitelikleri.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

Doğrulama özniteliklerinin zorlanan davranış modeli özellikleri belirtin:

* `Required` Ve `MinimumLength` öznitelikleri belirtmek bir özelliği bir değere sahip olmalıdır. Ancak, hiçbir şey kullanıcı boş değer atanabilir bir tür için doğrulama kısıtlamasını karşılamak için boşluk girmesini engeller. Atanamayan [değer türleri](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (gibi `decimal`, `int`, `float`, ve `DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği.
* `RegularExpression` Öznitelik, kullanıcının girebileceği karakter sınırlar. Önceki kodda, `Genre` bir veya daha fazla büyük harf ile başlamalı ve sıfır veya daha fazla harf, tek veya çift tırnak işareti, boşluk karakteri veya tire ile izleyin. `Rating` bir veya daha fazla büyük harf ile başlamalı ve ile sıfır veya daha fazla harf, sayı, tek veya çift tırnak, boşluk karakteri veya tire izleyin.
* `Range` Özniteliği için belirtilen bir aralıktaki bir değer kısıtlar.
* `StringLength` Öznitelik, bir dizenin maksimum uzunluğunu ve isteğe bağlı olarak en az uzunluk ayarlar. 

ASP.NET Core tarafından otomatik olarak uygulanan doğrulama kurallarına sahip bir uygulama daha sağlam hale getirmeye yardımcı olur. Otomatik doğrulama modeller üzerinde yeni bir kod eklendiğinde uygulanacaklarını hatırlamak zorunda olmadığınız için uygulama koruma yardımcı olur.

### <a name="validation-error-ui-in-razor-pages"></a>Razor sayfaları kullanıcı Arabiriminde doğrulama hatası

Uygulamayı çalıştırın ve sayfaları/filmler gidin.

Seçin **Yeni Oluştur** bağlantı. Bazı geçersiz değerlerle formu doldurun. JQuery istemci tarafı doğrulama hatası algıladığında, bir hata iletisi görüntüler.

![Birden çok jQuery istemci tarafı doğrulama hatalarını film görünüm formu](validation/_static/val.png)

> [!NOTE]
> Ondalık nokta veya virgül, girmeniz mümkün olmayabilir `Price` alan. Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") ondalık ve ABD İngilizce olmayan tarih biçimleri için uygulamanızı globalleştirmek için adımları izlemelisiniz. Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için. Şimdilik yalnızca 10 gibi tam sayı girin.

Form otomatik olarak işlenen geçersiz bir değer içeren her bir alan bir doğrulama hatası iletisini nasıl dikkat edin. (Bir kullanıcının JavaScript devre dışı olduğunda) hataları hem istemci-tarafı (JavaScript ve jQuery kullanarak) hem de sunucu tarafı uygulanır.

Önemli faydası ise **hiçbir** kod değişiklikleri oluşturma veya düzenleme sayfalarında gerekli. DataAnnotations modele uygulandı sonra kullanıcı Arabirimi doğrulama etkinleştirildi. Bu öğreticide otomatik olarak oluşturulan Razor sayfaları, doğrulama kurallarını çekilen (özelliklerini doğrulama öznitelikleri kullanarak `Movie` model sınıfı). Aynı doğrulama düzenleme sayfasını kullanarak test doğrulama uygulanır.

Form verileri, istemci tarafı doğrulama hata kalmayana kadar sunucusuna gönderilen değil. Form verileri bir veya daha fazla aşağıdaki yaklaşımlardan tarafından gönderilen değil doğrulayın:

* Bir kesme noktası koymak `OnPostAsync` yöntemi. Form gönderilemiyor (seçin **Oluştur** veya **Kaydet**). Kesme noktası hiçbir zaman ulaşılır.
* Kullanım [Fiddler aracı](http://www.telerik.com/fiddler).
* Ağ trafiğini izlemek için tarayıcı geliştirici araçlarını kullanın.

### <a name="server-side-validation"></a>Sunucu tarafı doğrulama

Tarayıcıda JavaScript devre dışı bırakıldığında hatalarla formu göndermeden sunucuya gönderin.

İsteğe bağlı, test sunucu tarafı doğrulama:

* Tarayıcıda JavaScript devre dışı bırakın. Tarayıcıda JavaScript devre dışı bırakamazsınız, başka bir tarayıcı deneyin.
* Bir kesme noktası kümesinde `OnPostAsync` yöntemi oluşturma veya düzenleme sayfası.
* Bir form doğrulama hatalarını gönderin.
* Model durumu geçersiz doğrulayın:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Aşağıdaki kod bir bölümü gösterilmektedir *Create.cshtml* öğreticinin önceki bölümlerinde iskele kurulmuş sayfası. İlk formu görüntülemek ve bir hata olması durumunda formu görüntülemek için oluşturma ve düzenleme sayfaları tarafından kullanılır.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) kullanan [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve jQuery doğrulaması istemci tarafında gereken HTML öznitelikleri oluşturur. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hataları görüntüler. Bkz: [doğrulama](xref:mvc/models/validation) daha fazla bilgi için.

Oluşturma ve düzenleme sayfaları hiçbir doğrulama kuralları olması. Doğrulama kuralları ve hata dizelerini yalnızca belirtilen `Movie` sınıfı. Bu doğrulama kuralları Düzenle Razor sayfaları için otomatik olarak uygulanacağını da `Movie` modeli.

Doğrulama mantığını değişmesi gerektiğinde, yalnızca model içinde yapılır. Doğrulama uygulama genelinde tutarlı olarak uygulandığından (tek bir yerde Doğrulama mantığı tanımlanır). Tek bir yerde doğrulama kodu temiz tutmaya yardımcı olur ve bakımını ve güncelleştirmesini yapmak kolaylaşır.

## <a name="using-datatype-attributes"></a>Veri türü öznitelikleri kullanma

İnceleme `Movie` sınıfı. `System.ComponentModel.DataAnnotations` Yerleşik doğrulama öznitelikleri kümesi yanı sıra biçimlendirme öznitelikleri ad alanı sağlar. `DataType` Özniteliği uygulandığı `ReleaseDate` ve `Price` özellikleri.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Öznitelikler yalnızca verilerin biçimlendirilmesi görünüm altyapısı için ipuçları sağlar (ve öznitelikleri gibi kaynakları `<a>` URL'leri için ve `<a href="mailto:EmailAddress.com">` e-posta için). Kullanım `RegularExpression` verilerin biçimi doğrulamak için özniteliği. `DataType` Özniteliği veritabanı iç türünden daha belirli bir veri türü belirtmek için kullanılır. `DataType` öznitelikleri doğrulama özniteliklerinin değildir. Örnek uygulama, yalnızca tarih, saat görüntülenir.

`DataType` Tarih, saat, telefon numarası, para birimi, EmailAddress ve diğer birçok veri türlerini numaralandırma sağlar. `DataType` Özniteliğini de otomatik olarak türe özgü özellikler sağlamak için uygulamayı etkinleştirin. Örneğin, bir `mailto:` bağlantısını için oluşturulamaz `DataType.EmailAddress`. Bir tarih seçici için sağlanan `DataType.Date` HTML5 destekleyen tarayıcılarda. `DataType` Öznitelikleri yayan HTML 5 `data-` HTML 5 tarayıcılar kullanma (okunur veri dash) öznitelikleri. `DataType` Öznitelikleri yapmak **değil** tüm doğrulama sağlar.

`DataType.Date` Görüntülenen tarih biçimi belirtmiyor. Varsayılan olarak, sunucu üzerinde temel alan varsayılan biçimler göre veri alanı görüntülenir `CultureInfo`.

::: moniker range=">= aspnetcore-2.1"

`[Column(TypeName = "decimal(18, 2)")]` Veri ek açıklama, Entity Framework Core doğru şekilde eşleyebilirsiniz biçimde gereklidir `Price` veritabanında para birimi. Daha fazla bilgi için [veri türleri](/ef/core/modeling/relational/data-types).

::: moniker-end

`DisplayFormat` Açıkça tarih biçimini belirtmek için özniteliği kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ayar değeri düzenlemek için görüntülendiğinde biçimlendirme uygulanması gerektiğini belirtir. Bazı alanlar için bu davranışı istemeyebilirsiniz. Örneğin, para birimi değerleri düzenleme UI para birimi sembolü büyük olasılıkla istemezsiniz.

`DisplayFormat` Özniteliği tek başına kullanılabilir, ancak genel olarak kullanmak iyi bir fikir olduğunu `DataType` özniteliği. `DataType` Özniteliği bir ekranda işlenecek nasıl aksine veri semantiği iletmez ve DisplayFormat ile elde etmezsiniz aşağıdaki avantajları sağlar:

* Tarayıcı HTML5 özelliklerini (örneğin bir Takvim denetimi, yerel ayara uygun bir para birimi simgesi, e-posta bağlantılarını, vb. gösterilecek) etkinleştirebilirsiniz.
* Varsayılan olarak, tarayıcı, yerel ayarları temel alarak doğru biçimde kullanarak verileri işlemez.
* `DataType` Öznitelik verileri işlemek için sağ alanı şablon seçmek ASP.NET Core framework etkinleştirebilirsiniz. `DisplayFormat` Tarafından kullanılan, kendi dize şablonu kullanır.

Not: jQuery doğrulama ile işe yaramazsa `Range` özniteliği ve `DateTime`. Örneğin, tarih belirtilen aralıkta olduğunda bile aşağıdaki kod her zaman bir istemci tarafı doğrulama hatası görüntülenir:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Bu genellikle Sabit tarihler kullanarak Modellerinizi derlemek için iyi bir uygulama değildir `Range` özniteliği ve `DateTime` önerilmez.

Aşağıdaki kod, bir satır birleştirme öznitelikleri gösterir:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

[Razor sayfaları ve EF Core ile çalışmaya başlama](xref:data/ef-rp/intro) EF çekirdekli Razor sayfaları işlemleriyle Gelişmiş gösterir.

### <a name="publish-to-azure"></a>Azure'da yayımlama

Bkz: [Visual Studio kullanarak Azure App Service'e bir ASP.NET Core web uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulamayı Azure'a yayımlama konusunda yönergeler için.

Razor sayfaları giriş tamamlamak için teşekkür ederiz. Geri bildirim için teşekkür ederiz. [Razor sayfaları ve EF Core ile çalışmaya başlama](xref:data/ef-rp/intro) olan Bu öğreticide kadar mükemmel bir izleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Formlarla Çalışma](xref:mvc/views/working-with-forms)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)
* [Etiket Yardımcıları giriş](xref:mvc/views/tag-helpers/intro)
* [Yazma etiketi Yardımcıları](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [Önceki: yeni alan ekleme](xref:tutorials/razor-pages/new-field)
