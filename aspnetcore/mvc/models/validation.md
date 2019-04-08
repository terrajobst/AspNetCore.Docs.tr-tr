---
title: ASP.NET Core MVC model doğrulama
author: tdykstra
description: Razor sayfaları ile ASP.NET Core MVC, model doğrulama hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/01/2019
monikerRange: '>= aspnetcore-2.1'
uid: mvc/models/validation
ms.openlocfilehash: 8d3d19791861b09d87eb3c85e8da0a8db061d4e9
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068371"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a>Razor sayfaları ile ASP.NET Core MVC, model doğrulama

Bu makalede, bir ASP.NET Core MVC veya Razor sayfaları uygulamada kullanıcı girişini doğrulama açıklanmaktadır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).

## <a name="model-state"></a>Model durumu

Model durumu gösteren iki alt sistemlerin gelen hataları: model bağlama ve model doğrulama. Kaynaklanan hatalar [model bağlama](model-binding.md) genellikle veri dönüştürme hataları (örneğin, "x" girilen bir tamsayı bekliyor. bir alanda) olan. Model doğrulama gerçekleşir model bağlama ve raporları hataları sonra verileri nerede iş kuralları için uygun değil (örneğin, 0, 1 ile 5 arasında bir derecelendirme bekliyor alanındaki girilir).

Model bağlama hem doğrulamayı bir denetleyici eylemi ya da bir Razor sayfaları işleyicisi yöntem yürütmeden önce oluşur. Web apps için bunu denetlemek için uygulamanın sorumluluğudur `ModelState.IsValid` ve uygun şekilde tepki verin. Web uygulamaları, genellikle sayfanın bir hata iletisi ile yeniden:

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

Web API denetleyicisi, denetlenecek yok `ModelState.IsValid` oluşturulduysa `[ApiController]` özniteliği. Bu durumda, bir otomatik HTTP 400 yanıt içeren sorun ayrıntıları döndürülür model durumu geçersiz. Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).

## <a name="rerun-validation"></a>Doğrulama yeniden çalıştırın

Doğrulama işlemi otomatiktir ancak el ile yinelemek isteyebilirsiniz. Örneğin, bir özellik için bir değer hesaplamak ve hesaplanan değere özelliği ayarlandıktan sonra doğrulama yeniden çalıştırmak istiyorsanız. Doğrulama yeniden çalıştırmak için çağrı `TryValidateModel` burada gösterildiği gibi yöntemi:

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a>Doğrulama öznitelikleri

Doğrulama özniteliklerinin model özelliklerini doğrulama kurallarını belirtmenizi sağlar. Aşağıdaki örnekte [örnek uygulamasını](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) doğrulama özniteliklerle ek açıklama bir model sınıfı gösterir. `[ClassicMovie]` Diğer yerleşik ve özel doğrulama özniteliği bir özniteliktir. (Gösterilmemiştir olan `[ClassicMovie2]`, özel bir öznitelik uygulamak için alternatif bir yolu gösterir.)

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a>Yerleşik öznitelikleri

Yerleşik doğrulama özniteliklerinin bazıları şunlardır:

* `[CreditCard]`: Özelliği bir kredi kartı biçimine sahip olduğunu doğrular.
* `[Compare]`: Bu iki modeli eşleşme özelliklerinde doğrular.
* `[EmailAddress]`: Özelliği bir e-posta biçiminde olduğunu doğrular.
* `[Phone]`: Özelliği biçimlendirme bir telefon numarasını sahip olduğunu doğrular.
* `[Range]`: Özellik değeri belirtilen bir aralığa denk gelen olduğunu doğrular.
* `[RegularExpression]`: Özellik değeri, belirtilen normal ifadeyle eşleştiğini doğrular.
* `[Required]`: Alanın boş olmadığını doğrular. Bkz: [[gerekli] özniteliği](#required-attribute) bu özniteliğin davranış hakkında ayrıntılı bilgi için.
* `[StringLength]`: Bir dize özelliği değerinde belirtilen uzunluk sınırını aşmaması doğrular.
* `[Url]`: Özelliği bir URL biçimine sahip olduğunu doğrular.
* `[Remote]`: Sunucuda bir eylem yöntemini çağırarak giriş istemcide doğrular. Bkz: [[uzak] özniteliği](#remote-attribute) bu özniteliğin davranış hakkında ayrıntılı bilgi için.

Doğrulama özniteliklerinin tam listesini bulabilirsiniz [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) ad alanı.

### <a name="error-messages"></a>Hata iletileri

Doğrulama öznitelikleri için geçersiz giriş görüntülenecek hata iletisi belirtmenizi sağlar. Örneğin:

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

Dahili olarak, öznitelikleri çağrı `String.Format` bazen ek yer tutucular ve alan adı için bir yer tutucu. Örneğin:

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

Uygulandığında bir `Name` özelliği, önceki kod tarafından oluşturulan hata iletisi olacak "adı uzunluğu 6 ile 8 arasında olmalıdır.".

Hangi parametreler geçirilen bulmak için `String.Format` belirli bir özniteliğin hata iletisi için [DataAnnotations kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).

## <a name="required-attribute"></a>[Gerekli] özniteliği

Bunlar varmış gibi varsayılan olarak, doğrulama sistemi NULL olmayan parametreleri veya özellikleri değerlendirir bir `[Required]` özniteliği. [Değer türleri](/dotnet/csharp/language-reference/keywords/value-types) gibi `decimal` ve `int` NULL olmayan atanabilir.

### <a name="required-validation-on-the-server"></a>Sunucuda [gerekli] doğrulama

Özellik null ise sunucu üzerinde gerekli değer eksik olarak kabul edilir. Her zaman NULL olmayan bir alan geçerli ve [gerekli] özniteliğin hata iletisi hiçbir zaman görüntülenir.

Ancak, model bağlama için alamayan bir özellik gibi bir hata iletisi kaynaklanan başarısız olabilir `The value '' is invalid`. Sunucu tarafı doğrulama atanamayan tür için bir özel hata iletisi belirtmek için aşağıdaki seçenekleriniz vardır:

* Alanın boş değer atanabilir olun (örneğin, `decimal?` yerine `decimal`). [Boş değer atanabilir\<T >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri gibi standart boş değer atanabilir türler kabul edilir.
* Model bağlama tarafından kullanılan varsayılan hata iletisi, aşağıdaki örnekte gösterildiği gibi belirtin:

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  Model bağlama hataları hakkında daha fazla bilgi varsayılan ayarlayabilirsiniz için iletileri için bkz: <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.

### <a name="required-validation-on-the-client"></a>İstemcide [gerekli] doğrulama

Olmayan boş değer atanabilir türler ile dizeler sunucuya karşılaştırıldığında istemcide farklı şekilde ele alınır. İstemcide:

* Bir değer girdi için yalnızca girilen mevcut olarak kabul edilir. Bu nedenle, istemci tarafı doğrulama atanamayan türleri aynı boş değer atanabilir türler olarak işler.
* JQuery doğrulaması dize alanı boş alanlarda geçerli giriş değerlendirilir [gerekli](https://jqueryvalidation.org/required-method/) yöntemi. Sunucu tarafı doğrulama yalnızca boşluk girilirse gerekli dize alanı geçersiz olarak değerlendirir.

Daha önce belirtildiği gibi sahip oldukları gibi sorgulamanıza atanamaz türler değerlendirilir bir `[Required]` özniteliği. Size istemci tarafı doğrulama bile uygulamazsanız anlamına `[Required]` özniteliği. Ancak öznitelik kullanmayın, varsayılan bir hata iletisi alırsınız. Özel hata iletisi belirtmek için özniteliği kullanın.

## <a name="remote-attribute"></a>[Uzak] özniteliği

`[Remote]` Öznitelik alanı girişinin geçerli olup olmadığını belirlemek için sunucu üzerinde bir yöntemi çağırmak gerektiren bir istemci tarafı doğrulama gerçekleştirir. Örneğin, bir kullanıcı adı zaten kullanımda olup olmadığını doğrulamak uygulamayı gerekebilir.

Uzak doğrulama uygulamak için:

1. Çağrılacak JavaScript için bir eylem yöntemi oluşturun.  JQuery doğrulama [uzak](https://jqueryvalidation.org/remote-method/) yöntemi bir JSON yanıtı bekliyor:

   * `"true"` Giriş verilerinin geçerli olduğu anlamına gelir.
   * `"false"`, `undefined`, veya `null` giriş geçersiz olduğu anlamına gelir.  Varsayılan hata iletisi görüntülenir.
   * Herhangi bir dize girişi geçersiz anlamına gelir. Dize bir özel hata iletisi görüntüler.

   Özel hata iletisi döndüren bir eylem yöntemi bir örnek aşağıda verilmiştir:

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. Model sınıfında, ek açıklama özelliğiyle bir `[Remote]` doğrulama eylem yöntemine, aşağıdaki örnekte gösterildiği gibi işaret eden bir öznitelik:

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]

### <a name="additional-fields"></a>Ek alanlar

`AdditionalFields` Özelliği `[Remote]` özniteliği karşı sunucuda veri alanlarının kombinasyonları doğrulamanıza olanak sağlar. Örneğin, varsa `User` modeli olan `FirstName` ve `LastName` özellikleri, mevcut hiçbir kullanıcı adları bu çifti zaten sahip olduğunuzu doğrulayın isteyebilirsiniz. Aşağıdaki örnek nasıl kullanılacağını gösterir `AdditionalFields`:

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

`AdditionalFields` dizeler için açıkça ayarlanmış `"FirstName"` ve `"LastName"`, ancak kullanarak [ `nameof` ](/dotnet/csharp/language-reference/keywords/nameof) işleci, daha sonra yeniden düzenleme basitleştirir. Bu doğrulama için eylem yöntemi, hem ad hem de son adı bağımsız değişkenleri kabul etmeniz gerekir:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

Kullanıcı ilk veya son adı girdiğinde, JavaScript adları bu çift geçen görmek için bir uzak çağrı yapar.

İki veya daha fazla ek alanlar doğrulamak için virgülle ayrılmış bir liste olarak belirtin. Örneğin, eklemek için bir `MiddleName` model özelliğinin ayarlandığı `[Remote]` özniteliği aşağıdaki örnekte gösterildiği gibi:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, tüm öznitelik bağımsız değişkenleri gibi bir sabit ifade olmalıdır. Bu nedenle, kullanmadığınız bir [ilişkilendirilmiş bir dizedir](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı <xref:System.String.Join*> başlatmak için `AdditionalFields`.

## <a name="alternatives-to-built-in-attributes"></a>Yerleşik özniteliklerini alternatifleri

Yerleşik öznitelikleri tarafından sağlanmayan doğrulama gerekiyorsa, şunları yapabilirsiniz:

* [Özel öznitelikler oluşturma](#custom-attributes).
* [IValidatableObject uygulamak](#ivalidatableobject).

## <a name="custom-attributes"></a>Özel öznitelikler

Yerleşik doğrulama özniteliklerinin işlemez senaryoları için özel doğrulama öznitelikleri oluşturabilirsiniz. Devralınan bir sınıf oluşturmanız <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>ve geçersiz kılma <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemi.

`IsValid` Yöntemi kabul adlı bir nesne *değer*, doğrulanacak giriş olduğu. Bir aşırı yüklemeyi de kabul eden bir `ValidationContext` model bağlama tarafından oluşturulan model örneği gibi ek bilgileri sağlayan nesne.

Aşağıdaki örnek, yayın tarihi için içinde bir film doğrular *Klasik* Tarz belirtilen bir yıldan daha yeni değil. `[ClassicMovie2]` Özniteliği Tarz ilk denetler ve yalnızca olup olmadığını devam *Klasik*. Klasikler tanımlanan film için daha sonraki bir öznitelik oluşturucuya geçirilen sınırı olmadığından emin olmak için yayın tarihi denetler.)

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

`movie` Önceki örnekte değişken temsil eden bir `Movie` form gönderme verilerini içeren nesne. `IsValid` Yöntemi, tarih ve Tarz denetler. Doğrulama başarılı bağlı `IsValid` döndürür bir `ValidationResult.Success` kod. Doğrulama başarısız olduğunda bir `ValidationResult` ile ilgili bir hata iletisi döndürülür.

## <a name="ivalidatableobject"></a>IValidatableObject

Yukarıdaki örnekte, yalnızca çalışır `Movie` türleri. Sınıf düzeyindeki doğrulama için başka bir seçenek uygulamaktır `IValidatableObject` model sınıfında, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a>Üst düzey düğüm doğrulama

Üst düzey düğümleri içerir:

* Eylem parametreleri
* Denetleyicisi Özellikleri
* Sayfa işleyici parametreleri
* Sayfa modeli özellikleri

Model bağlama en üst düzey düğümlerin yanı sıra model özelliklerini doğrulama doğrulanır. Aşağıdaki örnekte, örnek uygulamadan `VerifyPhone` yöntemi kullanan <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> doğrulamak için `phone` eylem parametresi:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

En üst düzey düğümlerini kullanabileceğiniz <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> doğrulama özniteliklere sahip. Aşağıdaki örnekte, örnek uygulamadan `CheckAge` yöntemini belirtir `age` parametre gerekir bağlı Sorgu dizesinden bir form gönderildiğinde:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

Yaş kontrol edin sayfasında (*CheckAge.cshtml*), iki biçimi vardır. İlk formu gönderdiği bir `Age` değerini `99` sorgu dizesi olarak: `https://localhost:5001/Users/CheckAge?Age=99`.

Düzgün şekilde biçimlendirilmiş olduğunda `age` sorgu dizesi parametresi gönderildi, formun doğrular.

Yaş kontrol edin sayfasında ikinci formu gönderdiği `Age` doğrulama başarısız olur ve istek gövdesinde bir değer. Başarısız olduğundan bağlama `age` parametresi, bir sorgu dizesinden gelmelidir.

İle çalıştırırken `CompatibilityVersion.Version_2_1` ya da daha üst düzey düğümü doğrulaması varsayılan olarak etkindir. Aksi takdirde, üst düzey düğüm doğrulaması devre dışı bırakıldı. Varsayılan seçenek ayarlayarak geçersiz kılınabilir <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> özelliğinde (`Startup.ConfigureServices`), burada gösterildiği gibi:

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a>En yüksek hata sayısı

Doğrulama hataları sayısı (varsayılan olarak, 200) ulaşıldığında durur. Bu sayı aşağıdaki kod ile yapılandırabileceğiniz `Startup.ConfigureServices`:

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a>Özyineleme sayısı üst sınırı

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> Doğrulanmakta olan model Nesne grafiği erişir. Çok derin veya sonsuz yinelemelidir modelleri için doğrulama, yığın taşmasına neden olabilir. [MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) ziyaretçi özyineleme yapılandırılan derinlik aşarsa erken doğrulama durdurmak için bir yol sağlar. Varsayılan değer olan `MvcOptions.MaxValidationDepth` 200 ile çalışırken olduğu `CompatibilityVersion.Version_2_2` veya üzeri. Önceki sürümleri için derinliği kısıtlama yok anlamına gelir, değeri null.

## <a name="automatic-short-circuit"></a>Otomatik kısa devre oluşturur

Model graf doğrulama gerektirmiyorsa doğrulama otomatik olarak (atlandı) kısa devre yapılma. Çalışma zamanı için doğrulamayı atlar nesneler temel elemanlar koleksiyonları içerir (gibi `byte[]`, `string[]`, `Dictionary<string, string>`) ve tüm doğrulayıcıları yoksa karmaşık nesne grafikler.

## <a name="disable-validation"></a>Doğrulama devre dışı bırak

Doğrulama devre dışı bırakmak için:

1. Bir uygulamasını oluşturun `IObjectModelValidator` alanları geçersiz olarak işaretlemiyor.

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. Aşağıdaki kodu ekleyin `Startup.ConfigureServices` varsayılan değiştirilecek `IObjectModelValidator` bağımlılık ekleme kapsayıcısını uygulamasında.

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

Model bağlamanın dışında kaynaklanan model durumu hatalarının yine de görebilirsiniz.

## <a name="client-side-validation"></a>İstemci tarafı doğrulama

Formun geçerli olana kadar istemci tarafı doğrulama gönderimi engeller. Gönder düğmesine formu gönderdiği veya hata iletilerini görüntüler JavaScript çalışır.

Bir form üzerinde giriş hataları olduğunda istemci tarafı doğrulama sunucusuna gereksiz bir gidiş dönüş önler. Aşağıdaki betiği başvurularının *_Layout.cshtml* ve *_ValidationScriptsPartial.cshtml* istemci tarafı doğrulama desteği:

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

[JQuery örtük doğrulaması](https://github.com/aspnet/jquery-validation-unobtrusive) betiğidir popüler üzerinde oluşturan özel bir ön uç Microsoft Kitaplığı [jQuery doğrulama](https://jqueryvalidation.org/) eklentisi. JQuery örtük doğrulaması aynı doğrulama mantığı iki yerde kod gerekirdi: model özellikleri, sunucu tarafı doğrulama öznitelikleri içinde bir kez ve ardından tekrar istemci tarafı betikler. Bunun yerine, [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) doğrulama öznitelikleri kullanın ve türü meta verileri HTML 5 işlemek için model özelliklerinden `data-` doğrulama gerekli form öğeleri için öznitelikler. jQuery doğrulama örtük ayrıştırır `data-` öznitelikleri ve jQuery doğrulama, etkili bir şekilde "sunucu tarafı doğrulama mantığını istemciye kopyalama" mantığı geçirir. Etiket Yardımcıları burada gösterildiği gibi kullanarak istemci üzerinde doğrulama hataları görüntüleyebilirsiniz:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

Önceki etiket Yardımcıları aşağıdaki HTML'yi işleyin.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

Dikkat `data-` HTML özniteliklerinde çıkış için doğrulama öznitelikleri karşılık `ReleaseDate` özelliği. `data-val-required` Özniteliği, kullanıcı Yayın Tarihi alanını doldurmazsa görüntülenecek hata iletisi içerir. jQuery doğrulaması örtük jQuery doğrulama için bu değer geçirir [ `required()` ](https://jqueryvalidation.org/required-method/) sonra bu iletiyi eşlik yöntemini  **\<span >** öğesi.

Veri türü doğrulama, tarafından geçersiz kılınmadığı sürece bir özelliği .NET türüne dayalı bir `[DataType]` özniteliği. Kendi varsayılan hata iletileri tarayıcılar var ancak bu iletileri jQuery doğrulama örtük doğrulama paketi geçersiz kılabilirsiniz. `[DataType]` öznitelikleri ve sınıfları gibi `[EmailAddress]` hata iletisi belirtmenizi sağlar.

### <a name="add-validation-to-dynamic-forms"></a>Doğrulama için dinamik formlar ekleyin

Sayfa ilk yüklendiğinde jQuery örtük doğrulaması jQuery doğrulama Doğrulama mantığı ve parametrelerini geçirir. Bu nedenle, doğrulama otomatik olarak dinamik olarak üretilen formlar üzerinde çalışmaz. Doğrulamayı etkinleştirmek için jQuery oluşturduktan hemen sonra dinamik biçimi ayrıştırmak için örtük doğrulaması söyleyin. Örneğin, aşağıdaki kod AJAX eklenen bir form üzerinde istemci tarafı doğrulama ayarlar.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

`$.validator.unobtrusive.parse()` Yöntemi jQuery Seçicisi kendi bir bağımsız değişkeni kabul eder. Bu yöntem jQuery ayrıştırmak için örtük doğrulaması söyler `data-` formları içinde seçicinin öznitelikleri. Bu öznitelik değerleri, ardından için jQuery doğrulama eklentisi geçirilir.

### <a name="add-validation-to-dynamic-controls"></a>Dinamik denetimlere doğrulama ekleme

`$.validator.unobtrusive.parse()` Gibi çalışır tek dinamik olarak üretilen denetimleri değil, tüm bir form üzerinde yöntemi `<input/>` ve `<select/>`. Yeniden ayrıştırma form için form daha önce ayrıştırıldığında aşağıdaki örnekte gösterildiği gibi eklendikten sonra doğrulama verileri kaldırın:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a>Özel istemci tarafı doğrulama

Özel istemci tarafı doğrulama oluşturarak yapılır `data-` özel jQuery doğrulama bağdaştırıcısı birlikte çalışan HTML öznitelikleri. Aşağıdaki örnek bağdaştırıcısı kod için yazılmıştır `ClassicMovie` ve `ClassicMovie2` bu makalenin önceki bölümlerinde sunulan öznitelikleri:

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

Bağdaştırıcıları yazma hakkında daha fazla bilgi için bkz: [jQuery doğrulama belgeleri](http://jqueryvalidation.org/documentation/).

Belirli bir alan için bir bağdaştırıcı kullanımını tarafından tetiklenen `data-` öznitelikleri:

* Alan doğrulama işlemine tabi olarak bayrak (`data-val="true"`).
* Doğrulama kuralı adı ve hata ileti metnini belirleyin (örneğin, `data-val-rulename="Error message."`).
* Ek parametreler Doğrulayıcı sağlamak gerekir (örneğin, `data-val-rulename-parm1="value"`).

Aşağıdaki örnekte gösterildiği `data-` örnek uygulamanın özniteliklerini `ClassicMovie` özniteliği:

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Daha önce belirtildiği gibi [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) işlemek için doğrulama öznitelikleri bilgileri kullanmak `data-` öznitelikleri. Özel oluşturulmasında Sonuç kodu yazmak için iki seçenek `data-` HTML öznitelikleri:

* Türetilen bir sınıf oluşturmanız `AttributeAdapterBase<TAttribute>` ve uygulayan bir sınıf `IValidationAttributeAdapterProvider`ve, öznitelik ve onun bağdaştırıcısı DI kaydedin. Bu yöntemi izleyen [tek sorumluluk sorumlusu](https://wikipedia.org/wiki/Single_responsibility_principle) ayrı sınıflarda sunucu ve istemci ilgili doğrulama kodu olması. Bağdaştırıcıyı ayrıca DI kaydedildiğinden, diğer hizmetlerde DI gerekirse hazır olduğunu avantajına sahiptir.
* Uygulama `IClientModelValidator` içinde `ValidationAttribute` sınıfı. Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulama yapmaz ve her DI hizmetlerinden ihtiyaç duymadığı uygun olabilir.

### <a name="attributeadapter-for-client-side-validation"></a>İstemci tarafı doğrulama için AttributeAdapter

İşleme Bu yöntem `data-` HTML öznitelikleri tarafından kullanılan `ClassicMovie` örnek uygulamada özniteliği. Bu yöntemi kullanarak istemci doğrulama eklemek için:

1. Özel doğrulama özniteliği için öznitelik bağdaştırıcı sınıfı oluşturun. Türetin [AttributeAdapterBase\<T >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2). Oluşturma bir `AddValidation` ekler yöntemi `data-` işlenen çıkışı için bu örnekte gösterildiği gibi öznitelikleri:

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. Uygulayan bir bağdaştırıcı sağlayıcı sınıfı oluşturma <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>. İçinde `GetAttributeAdapter` yöntemi bu örnekte gösterildiği gibi bu özel öznitelik bağdaştırıcının yapıcısına geçirin:

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. DI içinde için bağdaştırıcı sağlayıcısını kaydetme `Startup.ConfigureServices`:

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a>İstemci tarafı doğrulama için IClientModelValidator

İşleme Bu yöntem `data-` HTML öznitelikleri tarafından kullanılan `ClassicMovie2` örnek uygulamada özniteliği. Bu yöntemi kullanarak istemci doğrulama eklemek için:

* Özel doğrulama özniteliği uygulamak `IClientModelValidator` arabirim ve oluşturma bir `AddValidation` yöntemi. İçinde `AddValidation` yöntemi ekleme `data-` doğrulama için aşağıdaki örnekte gösterildiği gibi öznitelikleri:

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a>İstemci tarafı doğrulama devre dışı bırak

Aşağıdaki kod, istemci doğrulama MVC görünümleri devre dışı bırakır:

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

Bu MVC görünümleri, Razor sayfaları içinde değil, yalnızca çalışır. İstemci doğrulamasını devre dışı bırakmak için başka bir başvuru açıklama seçenektir `_ValidationScriptsPartial` içinde *.cshtml* dosya.

## <a name="additional-resources"></a>Ek kaynaklar

* [System.ComponentModel.DataAnnotations ad alanı](xref:System.ComponentModel.DataAnnotations)
* [Model bağlama](model-binding.md)
