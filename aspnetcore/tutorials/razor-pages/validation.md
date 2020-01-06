---
title: ASP.NET Core Razor sayfasına doğrulama ekleme
author: rick-anderson
description: ASP.NET Core bir Razor sayfasına nasıl doğrulama ekleneceğini öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 34157a63e43372876a02a858741dfd3a83a063b1
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354807"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>ASP.NET Core Razor sayfasına doğrulama ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, `Movie` modeline doğrulama mantığı eklenir. Doğrulama kuralları, bir Kullanıcı bir filmi oluşturduğunda veya düzenleişinizde zorlanır.

## <a name="validation"></a>Doğrulama

Yazılım geliştirmeye yönelik temel bir temel [kuru](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeon **Y**ourself") olarak adlandırılır. Razor Pages, işlevselliği bir kez belirtildiğinde geliştirme ve uygulama genelinde yansıtılmıştır. Kuru şu şekilde yardımcı olabilir:

* Uygulamadaki kod miktarını azaltın.
* Kodu daha az hata haline getirin ve test ve bakım yapmayı kolaylaştırın.

Razor Pages ve Entity Framework tarafından sunulan doğrulama desteği, Kuru ilkesine iyi bir örnektir. Doğrulama kuralları tek bir yerde (model sınıfında) bildirimli olarak belirtilir ve kurallar uygulamada her yerde zorlanır.

## <a name="add-validation-rules-to-the-movie-model"></a>Film modeline doğrulama kuralları ekleme

Dataaçıklamalarda ad alanı, bir sınıfa veya özelliğe bildirimli olarak uygulanan bir yerleşik doğrulama öznitelikleri kümesi sağlar. Veri açıklamaları, biçimlendirme ile yardım eden `DataType` gibi biçimlendirme özniteliklerini de içerir ve herhangi bir doğrulama sağlamaz.

Yerleşik `Required`, `StringLength`, `RegularExpression`ve `Range` doğrulama özniteliklerinden yararlanmak için `Movie` sınıfını güncelleştirin.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

Doğrulama öznitelikleri, uygulanan model özellikleri üzerinde zorlamak istediğiniz davranışı belirtir:

* `Required` ve `MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir; Ancak hiçbir şey, kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller.
* `RegularExpression` özniteliği, hangi karakterlerin giriş yapabileceğini sınırlamak için kullanılır. Yukarıdaki kodda, "tarz":

  * Yalnızca harfler kullanılmalıdır.
  * İlk harfin büyük harfle olması gerekir. Boşluk, sayı ve özel karakterlere izin verilmez.

* `RegularExpression` "derecelendirmesi":

  * İlk karakterin büyük harf olmasını gerektirir.
  * Sonraki boşlukların içindeki özel karakter ve sayılara izin verir. "PG-13" bir derecelendirme için geçerlidir, ancak bir "tarz" için başarısız olur.

* `Range` özniteliği, bir değeri belirtilen bir aralık içinde kısıtlar.
* `StringLength` özniteliği, bir dize özelliğinin en büyük uzunluğunu ve isteğe bağlı olarak en düşük uzunluğunu ayarlamanıza olanak sağlar.
* Değer türleri (örneğin `decimal`, `int`, `float`, `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğine gerek kalmaz.

Doğrulama kurallarının otomatik olarak uygulanmasını ASP.NET Core uygulamanızın daha sağlam olmasına yardımcı olur. Ayrıca, bir şeyi doğrulamayı unutmanızı ve veritabanına yanlışlıkla veri vermemesini de sağlar.

### <a name="validation-error-ui-in-razor-pages"></a>Razor Pages 'de doğrulama hatası Kullanıcı arabirimi

Uygulamayı çalıştırın ve sayfalar/Filmler ' e gidin.

**Yeni oluştur** bağlantısını seçin. Formu, bazı geçersiz değerlerle doldurun. JQuery istemci tarafı doğrulaması hatayı algıladığında, bir hata iletisi görüntüler.

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](validation/_static/val.png)

[!INCLUDE[](~/includes/localization/currency.md)]

Formun geçersiz bir değer içeren her alanda otomatik olarak bir doğrulama hata iletisi nasıl oluşturulduğuna dikkat edin. Hatalar hem istemci tarafında (JavaScript ve jQuery kullanılarak) hem de sunucu tarafında (bir Kullanıcı JavaScript devre dışı bırakıldığında) zorlanır.

Önemli bir avantaj, oluşturma veya düzenleme sayfalarında **hiçbir** kod değişikliği gerekli değildir. Veri ek açıklamaları modele uygulandıktan sonra, doğrulama kullanıcı arabirimi etkinleştirilmiştir. Bu öğreticide oluşturulan Razor Pages otomatik olarak doğrulama kurallarını (`Movie` model sınıfının özelliklerinde doğrulama özniteliklerini kullanarak) otomatik olarak alır. Düzenleme sayfasını kullanarak doğrulama testi, aynı doğrulama uygulanır.

Form verileri, istemci tarafı doğrulama hatası kalmayana kadar sunucuya nakledilmez. Form verilerinin aşağıdaki yaklaşımlardan bir veya daha fazlası tarafından nakledilmediğinden emin olun:

* `OnPostAsync` yöntemine bir kesme noktası koyun. Formu gönder ( **Oluştur** veya **Kaydet**' i seçin). Kesme noktası hiçbir şekilde isabet ettirilmez.
* [Fiddler aracını](https://www.telerik.com/fiddler)kullanın.
* Ağ trafiğini izlemek için tarayıcı Geliştirici Araçları ' nı kullanın.

### <a name="server-side-validation"></a>Sunucu tarafı doğrulaması

Tarayıcıda JavaScript devre dışı bırakıldığında, formun hatalarla gönderilmesi sunucuya gönderilir.

İsteğe bağlı, test sunucusu-tarafı doğrulaması:

* Tarayıcıda JavaScript 'ı devre dışı bırakın. Tarayıcının geliştirici araçlarını kullanarak JavaScript 'ı devre dışı bırakabilirsiniz. Tarayıcıda JavaScript 'ı devre dışı bırakadıysanız başka bir tarayıcı deneyin.
* Oluşturma veya düzenleme sayfasının `OnPostAsync` yönteminde bir kesme noktası ayarlayın.
* Geçersiz verilerle form gönderme.
* Model durumunun geçersiz olduğunu doğrulayın:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```
  
Alternatif olarak, [sunucuda istemci tarafı doğrulamayı devre dışı](xref:mvc/models/validation#disable-client-side-validation)bırakabilirsiniz.

Aşağıdaki kod, öğreticide daha önce *Create. cshtml* sayfa scafkatın bir bölümünü gösterir. İlk formu görüntülemek ve bir hata durumunda formu yeniden görüntülemek için sayfa oluşturma ve düzenleme sayfaları tarafından kullanılır.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) , [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hatalarını görüntüler. Daha fazla bilgi için bkz. [doğrulama](xref:mvc/models/validation) .

Oluşturma ve düzenleme sayfalarında hiçbir doğrulama kuralı yoktur. Doğrulama kuralları ve hata dizeleri yalnızca `Movie` sınıfında belirtilmiştir. Bu doğrulama kuralları, `Movie` modelini düzenleyebilen Razor Pages otomatik olarak uygulanır.

Doğrulama mantığının değişmesi gerektiğinde, yalnızca modelde yapılır. Doğrulama, uygulamanın tamamında tutarlı bir şekilde uygulanır (doğrulama mantığı tek bir yerde tanımlanır). Tek bir yerde doğrulama, kodun temiz kalmasına yardımcı olur ve bakım ve güncelleştirme işlemlerini kolaylaştırır.

## <a name="using-datatype-attributes"></a>DataType özniteliklerini kullanma

`Movie` sınıfını inceleyin. `System.ComponentModel.DataAnnotations` ad alanı, yerleşik doğrulama öznitelikleri kümesine ek olarak biçimlendirme öznitelikleri sağlar. `DataType` özniteliği `ReleaseDate` ve `Price` özelliklerine uygulanır.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` öznitelikleri yalnızca görünüm altyapısının verileri biçimlendirmek için ipuçları sağlar (ve URL için `<a>` gibi öznitelikleri ve e-posta için `<a href="mailto:EmailAddress.com">`) sağlar. Verilerin biçimini doğrulamak için `RegularExpression` özniteliğini kullanın. `DataType` özniteliği, veritabanı iç türünden daha özel bir veri türü belirtmek için kullanılır. `DataType` öznitelikleri, doğrulama öznitelikleri değildir. Örnek uygulamada, yalnızca tarih ve saat olmadan görüntülenir.

`DataType` numaralandırması, tarih, saat, PhoneNumber, para birimi, Emaadresi ve daha fazlası gibi birçok veri türü sağlar. `DataType` özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir. Örneğin, `DataType.EmailAddress`için bir `mailto:` bağlantısı oluşturulabilir. HTML5 'i destekleyen tarayıcılarda `DataType.Date` için bir tarih seçici sağlanmış olabilir. `DataType` öznitelikleri HTML 5 tarayıcıların kullandığı HTML 5 `data-` (bir veri Dash) özniteliklerini yayar. `DataType` öznitelikleri herhangi bir **doğrulama sağlamaz.**

`DataType.Date` görüntülenen tarihin biçimini belirtmiyor. Varsayılan olarak, veri alanı, sunucunun `CultureInfo`göre varsayılan biçimlere göre görüntülenir.

Entity Framework Core `[Column(TypeName = "decimal(18, 2)")]` veri ek açıklaması gerekir, bu nedenle `Price` veritabanındaki para birimine doğru şekilde eşleyebilir. Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).

`DisplayFormat` özniteliği, açıkça tarih biçimini belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` ayarı, değer düzenlenmek üzere görüntülendiğinde biçimlendirmenin uygulanacağını belirtir. Bazı alanlar için bu davranışı istemiyor olabilirsiniz. Örneğin, para birimi değerlerinde, büyük olasılıkla düzenleme kullanıcı arabirimindeki para birimi sembolünü istemezsiniz.

`DisplayFormat` özniteliği kendisi tarafından kullanılabilir, ancak genellikle `DataType` özniteliğini kullanmak iyi bir fikir olabilir. `DataType` özniteliği, bir ekranda nasıl işlenirim aksine verilerin semantiğini sunar ve DisplayFormat ile elde olmadığınız aşağıdaki avantajları sağlar:

* Tarayıcı HTML5 özelliklerini etkinleştirebilir (örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını vb. göstermek için)
* Varsayılan olarak tarayıcı, verileri yerel ayarınızı temel alarak doğru biçimi kullanarak işleyebilir.
* `DataType` özniteliği, ASP.NET Core çerçevesinin verileri işlemek için doğru alan şablonunu seçmesini sağlayabilir. Kendisi tarafından kullanılıyorsa `DisplayFormat` dize şablonunu kullanır.

Note: jQuery doğrulaması `Range` özniteliğiyle ve `DateTime`birlikte çalışmaz. Örneğin, aşağıdaki kod, tarih belirtilen aralıkta olduğunda bile her zaman bir istemci tarafı doğrulama hatası görüntüler:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Modellerinizde sabit tarihleri derlemek genellikle iyi bir uygulamadır, bu nedenle `Range` özniteliği ve `DateTime` kullanılması önerilmez.

Aşağıdaki kod, öznitelikleri tek bir satırda birleştirmeyi gösterir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDAmult.cs?name=snippet1)]

[Razor Pages kullanmaya başlayın ve EF Core](xref:data/ef-rp/intro) gelişmiş EF Core işlemlerini Razor Pages gösterir.

### <a name="apply-migrations"></a>Geçişleri Uygula

Sınıfa uygulanan Dataek açıklamaları şemayı değiştirir. Örneğin, `Title` alanına uygulanan veri ek açıklamaları:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet11)]

* Karakterleri 60 olarak sınırlandırır.
* `null` değere izin vermez.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

`Movie` tablosu şu anda aşağıdaki şemaya sahiptir:

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (MAX)  NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (MAX)  NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (MAX)  NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

Önceki şema değişiklikleri, EF 'in özel durum oluşturmasına neden olmaz. Ancak, şemanın modelle tutarlı olması için bir geçiş oluşturun.

**Araçlar** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi konsolu**' nu seçin.
PMC'de aşağıdaki komutları girin:

```powershell
Add-Migration New_DataAnnotations
Update-Database
```

`Update-Database` `New_DataAnnotations` sınıfının `Up` yöntemlerini çalıştırır. `Up` yöntemini inceleyin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Migrations/20190724163003_New_DataAnnotations.cs?name=snippet)]

Güncelleştirilmiş `Movie` tablosu aşağıdaki şemaya sahiptir:

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (60)   NOT NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (30)   NOT NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (5)    NOT NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

SQLite için geçişler gerekli değildir.

---

### <a name="publish-to-azure"></a>Azure'a Yayımlama

Azure 'a dağıtma hakkında bilgi için bkz. [öğretici: Azure 'DA SQL veritabanı ile ASP.NET Core uygulama oluşturma](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).

Razor Pages için bu giriş tamamlanırken teşekkürler. [Razor Pages kullanmaya başlayın ve](xref:data/ef-rp/intro) Bu öğreticiye en uygun harika bir izleme EF Core.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [Bu öğreticinin YouTube sürümü](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [Önceki: yeni bir alan ekleme](xref:tutorials/razor-pages/new-field)
