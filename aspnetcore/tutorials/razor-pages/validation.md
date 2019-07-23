---
title: ASP.NET Core Razor sayfasına doğrulama ekleme
author: rick-anderson
description: ASP.NET Core bir Razor sayfasına nasıl doğrulama ekleneceğini öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 8495849c89ca3d6fd2b2006b61ce2ec75ff504a5
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "67815647"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>ASP.NET Core Razor sayfasına doğrulama ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, doğrulama mantığı `Movie` modele eklenir. Doğrulama kuralları, bir Kullanıcı bir filmi oluşturduğunda veya düzenleişinizde zorlanır.

## <a name="validation"></a>Doğrulama

Yazılım geliştirmeye yönelik temel bir temel [kuru](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeon **Y**ourself") olarak adlandırılır. Razor Pages, işlevselliği bir kez belirtildiğinde geliştirme ve uygulama genelinde yansıtılmıştır. Kuru şu şekilde yardımcı olabilir:

* Uygulamadaki kod miktarını azaltın.
* Kodu daha az hata haline getirin ve test ve bakım yapmayı kolaylaştırın.

Razor Pages ve Entity Framework tarafından sunulan doğrulama desteği, Kuru ilkesine iyi bir örnektir. Doğrulama kuralları tek bir yerde (model sınıfında) bildirimli olarak belirtilir ve kurallar uygulamada her yerde zorlanır.

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

### <a name="validation-error-ui-in-razor-pages"></a>Razor Pages 'de doğrulama hatası Kullanıcı arabirimi

Uygulamayı çalıştırın ve sayfalar/Filmler ' e gidin.

**Yeni oluştur** bağlantısını seçin. Formu, bazı geçersiz değerlerle doldurun. JQuery istemci tarafı doğrulaması hatayı algıladığında, bir hata iletisi görüntüler.

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

Formun geçersiz bir değer içeren her alanda otomatik olarak bir doğrulama hata iletisi nasıl oluşturulduğuna dikkat edin. Hatalar hem istemci tarafında (JavaScript ve jQuery kullanılarak) hem de sunucu tarafında (bir Kullanıcı JavaScript devre dışı bırakıldığında) zorlanır.

Önemli bir avantaj, oluşturma veya düzenleme sayfalarında **hiçbir** kod değişikliği gerekli değildir. Veri ek açıklamaları modele uygulandıktan sonra, doğrulama kullanıcı arabirimi etkinleştirilmiştir. Bu öğreticide oluşturulan Razor Pages, otomatik olarak doğrulama kurallarını ( `Movie` model sınıfının özelliklerinde doğrulama özniteliklerini kullanarak) otomatik olarak çekti. Düzenleme sayfasını kullanarak doğrulama testi, aynı doğrulama uygulanır.

Form verileri, istemci tarafı doğrulama hatası kalmayana kadar sunucuya nakledilmez. Form verilerinin aşağıdaki yaklaşımlardan bir veya daha fazlası tarafından nakledilmediğinden emin olun:

* `OnPostAsync` Yöntemine bir kesme noktası koyun. Formu gönder ( **Oluştur** veya **Kaydet**' i seçin). Kesme noktası hiçbir şekilde isabet ettirilmez.
* [Fiddler aracını](https://www.telerik.com/fiddler)kullanın.
* Ağ trafiğini izlemek için tarayıcı Geliştirici Araçları ' nı kullanın.

### <a name="server-side-validation"></a>Sunucu tarafı doğrulaması

Tarayıcıda JavaScript devre dışı bırakıldığında, formun hatalarla gönderilmesi sunucuya gönderilir.

İsteğe bağlı, test sunucusu-tarafı doğrulaması:

* Tarayıcıda JavaScript 'ı devre dışı bırakın. Bunu tarayıcınızın geliştirici araçlarını kullanarak yapabilirsiniz. Tarayıcıda JavaScript 'ı devre dışı bırakadıysanız başka bir tarayıcı deneyin.
* Oluşturma veya düzenleme sayfasının `OnPostAsync` yönteminde bir kesme noktası ayarlayın.
* Doğrulama hatalarıyla bir form gönderebilirsiniz.
* Model durumunun geçersiz olduğunu doğrulayın:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Aşağıdaki kod, öğreticide daha önce iskele aldığınız *Create. cshtml* sayfasının bir bölümünü gösterir. İlk formu görüntülemek ve bir hata durumunda formu yeniden görüntülemek için sayfa oluşturma ve düzenleme sayfaları tarafından kullanılır.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[Giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms) , [dataaçıklamaların](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) özniteliklerini kullanır ve istemci tarafında jQuery doğrulaması için gerekli HTML özniteliklerini üretir. [Doğrulama etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-tag-helpers) doğrulama hatalarını görüntüler. Daha fazla bilgi için bkz. [doğrulama](xref:mvc/models/validation) .

Oluşturma ve düzenleme sayfalarında hiçbir doğrulama kuralı yoktur. Doğrulama kuralları ve hata dizeleri yalnızca `Movie` sınıfında belirtilmiştir. Bu doğrulama kuralları, `Movie` modeli düzenlediğiniz Razor Pages otomatik olarak uygulanır.

Doğrulama mantığının değişmesi gerektiğinde, yalnızca modelde yapılır. Doğrulama, uygulamanın tamamında tutarlı bir şekilde uygulanır (doğrulama mantığı tek bir yerde tanımlanır). Tek bir yerde doğrulama, kodun temiz kalmasına yardımcı olur ve bakım ve güncelleştirme işlemlerini kolaylaştırır.

## <a name="using-datatype-attributes"></a>DataType özniteliklerini kullanma

`Movie` Sınıfını inceleyin. Ad `System.ComponentModel.DataAnnotations` alanı, yerleşik doğrulama öznitelikleri kümesine ek olarak biçimlendirme öznitelikleri sağlar. `DataType` `ReleaseDate` Özniteliği ve`Price` özelliklerine uygulanır.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Öznitelikler yalnızca görünüm altyapısının verileri biçimlendirmek için ipuçları sağlar (ve URL 'ler ve `<a href="mailto:EmailAddress.com">` e-posta için gibi `<a>` öznitelikleri sağlar). `DataType` Veri biçimini doğrulamak için özniteliğinikullanın.`RegularExpression` `DataType` Özniteliği, veritabanı iç türünden daha belirgin bir veri türü belirtmek için kullanılır. `DataType`Öznitelikler, doğrulama öznitelikleri değildir. Örnek uygulamada, yalnızca tarih ve saat olmadan görüntülenir.

`DataType` Sabit listesi, tarih, saat, PhoneNumber, para birimi, emaadresi gibi birçok veri türünü sağlar. `DataType` Özniteliği Ayrıca uygulamanın türe özgü özellikleri otomatik olarak sağlamasını da sağlayabilir. Örneğin, için `DataType.EmailAddress`bir `mailto:` bağlantı oluşturulabilir. HTML5 'i destekleyen tarayıcılarda için `DataType.Date` bir tarih seçici sağlanmış olabilir. Öznitelikler HTML 5 tarayıcılarının kullandığı `data-` HTML 5 (bir veri Dash) özniteliklerini yayar. `DataType` Öznitelikler herhangi bir **doğrulama sağlamaz.** `DataType`

`DataType.Date`görüntülenen tarihin biçimini belirtmez. Varsayılan olarak, veri alanı sunucu `CultureInfo`' a göre varsayılan biçimlere göre görüntülenir.

Entity Framework Core veri ek açıklaması gerekir, bu nedenle veritabanında para birimiyle doğru şekilde eşleşebilirler `Price`. `[Column(TypeName = "decimal(18, 2)")]` Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).

`DisplayFormat` Öznitelik, tarih biçimini açıkça belirtmek için kullanılır:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ayar, değer düzenlenmek üzere görüntülendiğinde biçimlendirmenin uygulanacağını belirtir. Bazı alanlar için bu davranışı istemiyor olabilirsiniz. Örneğin, para birimi değerlerinde, büyük olasılıkla düzenleme kullanıcı arabirimindeki para birimi sembolünü istemezsiniz.

Özniteliği kendisi tarafından kullanılabilir, ancak genellikle `DataType` özniteliği kullanmak iyi bir fikir olabilir. `DisplayFormat` `DataType` Özniteliği, bir ekranda nasıl işlenirim aksine, verilerin semantiğini alır ve DisplayFormat ile elde olmadığınız avantajları sağlar:

* Tarayıcı HTML5 özelliklerini etkinleştirebilir (örneğin, bir Takvim denetimini, yerel ayara uygun para birimi sembolünü, e-posta bağlantılarını vb. göstermek için)
* Varsayılan olarak tarayıcı, verileri yerel ayarınızı temel alarak doğru biçimi kullanarak işleyebilir.
* `DataType` Öznitelik, ASP.NET Core çerçevesinin verileri işlemek için doğru alan şablonunu seçmesini sağlayabilir. Kendisi `DisplayFormat` tarafından kullanılıyorsa, dize şablonunu kullanır.

Note: jQuery doğrulaması, `Range` ve `DateTime`özniteliğiyle çalışmaz. Örneğin, aşağıdaki kod, tarih belirtilen aralıkta olduğunda bile her zaman bir istemci tarafı doğrulama hatası görüntüler:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Genellikle, modellerinizde sabit tarihleri derlemek iyi bir uygulamadır, bu nedenle `Range` özniteliğini kullanarak ve `DateTime` önerilmez.

Aşağıdaki kod, öznitelikleri tek bir satırda birleştirmeyi gösterir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

[Razor Pages kullanmaya başlayın ve EF Core](xref:data/ef-rp/intro) gelişmiş EF Core işlemlerini Razor Pages gösterir.

### <a name="publish-to-azure"></a>Azure'a Yayımlama

Azure 'a dağıtma hakkında bilgi için bkz [. Öğretici: SQL veritabanı](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase)ile Azure 'da bir ASP.NET uygulaması oluşturun. Bu yönergeler bir ASP.NET Core uygulaması değil, bir ASP.NET uygulaması içindir, ancak adımlar aynıdır.

Razor Pages için bu giriş tamamlanırken teşekkürler. [Razor Pages kullanmaya başlayın ve](xref:data/ef-rp/intro) Bu öğreticiye en uygun harika bir izleme EF Core.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [Bu öğreticinin YouTube sürümü](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [Öncekini Yeni alan ekleme](xref:tutorials/razor-pages/new-field)
