---
title: ASP.NET Core MVC 'de model doğrulaması
author: rick-anderson
description: ASP.NET Core MVC ve Razor Pages model doğrulaması hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
monikerRange: '>= aspnetcore-2.1'
uid: mvc/models/validation
ms.openlocfilehash: eb18d3a701a4d1937ac6eb9f61916f348b95882a
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975252"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a>ASP.NET Core MVC ve Razor Pages model doğrulaması

Bu makalede, ASP.NET Core MVC veya Razor Pages uygulamasında Kullanıcı girişinin nasıl doğrulanacağı açıklanır.

[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([indirme](xref:index#how-to-download-a-sample)).

## <a name="model-state"></a>Model durumu

Model durumu iki alt sistemden gelen hataları temsil eder: model bağlama ve model doğrulama. [Model bağlamasından](model-binding.md) kaynaklanan hatalar genellikle veri dönüştürme hatalardır (örneğin, bir tamsayı bekleyen bir alana bir "x" girilir). Model bağlama ve verilerin iş kurallarına uygun olmadığı rapor hataları (örneğin, 1 ile 5 arasında bir derecelendirme bekleyen bir alana bir 0 girildiğinde) oluşturulduktan sonra model doğrulaması oluşur.

Hem model bağlama hem de doğrulama, bir denetleyici eyleminin veya bir Razor Pages işleyicisi yönteminin yürütülmesinden önce oluşur. Web uygulamaları için uygulama, uygun şekilde İnceleme `ModelState.IsValid` ve tepki verme sorumluluğundadır. Web Apps genellikle sayfayı bir hata iletisiyle yeniden görüntülerdi:

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

Web API denetleyicilerinin `[ApiController]` özniteliğe sahip olup olmadığını `ModelState.IsValid` kontrol etmek zorunda değildir. Bu durumda, model durumu geçersiz olduğunda sorun ayrıntıları içeren bir otomatik HTTP 400 yanıtı döndürülür. Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).

## <a name="rerun-validation"></a>Doğrulamayı yeniden çalıştır

Doğrulama otomatiktir, ancak el ile yinelemek isteyebilirsiniz. Örneğin, bir özellik için bir değer hesaplamanıza ve özelliği hesaplanan değere ayarladıktan sonra doğrulamayı yeniden çalıştırmak isteyebilirsiniz. Doğrulamayı yeniden çalıştırmak için, `TryValidateModel` yöntemi aşağıda gösterildiği gibi çağırın:

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a>Doğrulama öznitelikleri

Doğrulama öznitelikleri, model özellikleri için doğrulama kuralları belirtmenize olanak tanır. [Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) aşağıdaki örnek, doğrulama öznitelikleriyle açıklama eklenmiş bir model sınıfı gösterir. `[ClassicMovie]` Özniteliği özel bir doğrulama özniteliğidir ve diğerleri yerleşik olarak bulunur. (Gösterilmez `[ClassicMovie2]`, bu, özel bir özniteliği uygulamak için alternatif bir yol gösterir.)

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a>Yerleşik öznitelikler

Yerleşik doğrulama özniteliklerinden bazıları şunlardır:

* `[CreditCard]`: Özelliğin kredi kartı biçimine sahip olduğunu doğrular.
* `[Compare]`: Bir modeldeki iki özelliği eşleştiğini doğrular.
* `[EmailAddress]`: Özelliğin bir e-posta biçimine sahip olduğunu doğrular.
* `[Phone]`: Özelliğin bir telefon numarası biçimine sahip olduğunu doğrular.
* `[Range]`: Özellik değerinin belirtilen bir aralık dahilinde olduğunu doğrular.
* `[RegularExpression]`: Özellik değerinin belirtilen bir normal ifadeyle eşleştiğini doğrular.
* `[Required]`: Alanın null olduğunu doğrular. Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [[Required] özniteliği](#required-attribute) .
* `[StringLength]`: Bir dize özellik değerinin belirtilen uzunluk sınırını aşmadığını doğrular.
* `[Url]`: Özelliğin bir URL biçimine sahip olduğunu doğrular.
* `[Remote]`: Sunucuda bir eylem yöntemi çağırarak istemcideki girişi doğrular. Bu özniteliğin davranışı hakkındaki ayrıntılar için bkz. [[Remote] Attribute](#remote-attribute) .

Doğrulama özniteliklerinin tüm listesi [System. ComponentModel. Dataaçıklamalarda](xref:System.ComponentModel.DataAnnotations) ad alanında bulunabilir.

### <a name="error-messages"></a>Hata iletileri

Doğrulama öznitelikleri, geçersiz giriş için görüntülenecek hata iletisini belirtmenize izin verir. Örneğin:

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

Dahili olarak, öznitelikler, `String.Format` alan adı için bir yer tutucu ve bazen ek yer tutucular ile çağrı yapılır. Örneğin:

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

Bir `Name` özelliğe uygulandığında, yukarıdaki kod tarafından oluşturulan hata iletisi "ad uzunluğu 6 ile 8 arasında olmalıdır." olacaktır.

Belirli bir özniteliğin hata iletisinde hangi parametrelerin geçtiğini `String.Format` öğrenmek için, bkz. [dataaçıklamalarda kaynak kodu](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).

## <a name="required-attribute"></a>[Zorunlu] özniteliği

Varsayılan olarak, doğrulama sistemi, null olamayan parametreleri veya özellikleri bir `[Required]` özniteliğe sahip gibi davranır. `decimal` [](/dotnet/csharp/language-reference/keywords/value-types) Ve`int` gibi değer türleri null atanamaz.

### <a name="required-validation-on-the-server"></a>[Zorunlu] sunucuda doğrulama

Sunucuda, özelliği null ise gerekli bir değer eksik olarak kabul edilir. Null yapılamayan bir alan her zaman geçerlidir ve [gerekli] özniteliğinin hata mesajı hiçbir zaman gösterilmez.

Ancak, null olamayan bir özellik için model bağlama başarısız olabilir ve gibi bir hata mesajı `The value '' is invalid`elde edilir. Null yapılamayan türlerin sunucu tarafı doğrulaması için özel bir hata iletisi belirtmek üzere aşağıdaki seçenekleriniz vardır:

* Alanı null yapılabilir yapın (örneğin, `decimal?` `decimal`yerine). [Null\<atanabilir T >](/dotnet/csharp/programming-guide/nullable-types/) değer türleri standart null yapılabilir türler gibi değerlendirilir.
* Aşağıdaki örnekte gösterildiği gibi model bağlama tarafından kullanılacak varsayılan hata iletisini belirtin:

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  İçin varsayılan iletileri ayarlayabileceğiniz model bağlama hataları hakkında daha fazla bilgi için bkz <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.

### <a name="required-validation-on-the-client"></a>[Zorunlu] istemcide doğrulama

Null yapılamayan türler ve dizeler, sunucu ile karşılaştırıldığında, istemci üzerinde farklı şekilde işlenir. İstemcide:

* Yalnızca girdi girildiğinde bir değer vardır. Bu nedenle, istemci tarafı doğrulaması null yapılamayan türler, null yapılabilir türler ile aynı şekilde işler.
* Bir dize alanındaki boşluk, jQuery doğrulaması [gerekli](https://jqueryvalidation.org/required-method/) yöntemi tarafından geçerli bir girdi olarak kabul edilir. Yalnızca boşluk girildiğinde, sunucu tarafı doğrulaması gerekli bir dize alanını geçersiz kabul eder.

Daha önce belirtildiği gibi, null olamayan türler bir `[Required]` özniteliğe sahip olsa da kabul edilir. Bu, `[Required]` özniteliğini uygulamasanız bile istemci tarafı doğrulamayı alacağınız anlamına gelir. Ancak özniteliğini kullanmazsanız varsayılan bir hata iletisi alırsınız. Özel bir hata iletisi belirtmek için özniteliğini kullanın.

## <a name="remote-attribute"></a>[Uzak] özniteliği

`[Remote]` Özniteliği, alan girişinin geçerli olup olmadığını anlamak için sunucu üzerinde bir yöntem çağrılmasını gerektiren istemci tarafı doğrulaması uygular. Örneğin, uygulamanın bir kullanıcı adının zaten kullanımda olup olmadığını doğrulaması gerekebilir.

Uzaktan doğrulamayı uygulamak için:

1. JavaScript 'e çağırmak için bir eylem yöntemi oluşturun.  JQuery Validate [uzak](https://jqueryvalidation.org/remote-method/) YÖNTEMI bir JSON yanıtı bekliyor:

   * `"true"`giriş verilerinin geçerli olduğu anlamına gelir.
   * `"false"`, `undefined` ya`null` da girişin geçersiz olduğu anlamına gelir.  Varsayılan hata iletisini görüntüler.
   * Diğer herhangi bir dize, girişin geçersiz olduğu anlamına gelir. Dizeyi özel bir hata iletisi olarak görüntüleyin.

   Özel bir hata iletisi döndüren eylem yöntemine bir örnek aşağıda verilmiştir:

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. Model sınıfında, aşağıdaki örnekte gösterildiği gibi, doğrulama eylemi `[Remote]` yöntemine işaret eden bir özniteliğe sahip özelliğe açıklama ekleyin:

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   `[Remote]` Özniteliği`Microsoft.AspNetCore.Mvc` ad alanıdır. `Microsoft.AspNetCore.App` Veya metapackage`Microsoft.AspNetCore.All` kullanmıyorsanız [Microsoft. aspnetcore. Mvc. viewfeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet paketini yükleyebilirsiniz.
   
### <a name="additional-fields"></a>Ek alanlar

`[Remote]` Özniteliğinin özelliği, sunucudaki verilere karşı alan birleşimlerini doğrulamanızı sağlar. `AdditionalFields` Örneğin, `User` `FirstName` model ve`LastName` özellikleri varsa, var olan hiçbir kullanıcının bu ad çiftine sahip olmadığını doğrulamak isteyebilirsiniz. Aşağıdaki örnek nasıl kullanılacağını `AdditionalFields`gösterir:

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

`AdditionalFields`açıkça dizelere `"FirstName"` ayarlanabilir, ancak [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) işleci kullanmak daha `"LastName"`sonra yeniden düzenlemeyi basitleştirir. Bu doğrulama için eylem yöntemi hem adı hem de soyadı bağımsız değişkenlerini kabul etmelidir:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

Kullanıcı adı veya soyadı girdiğinde JavaScript, bu ad çiftinin alındığını görmek için uzak bir çağrı yapar.

İki veya daha fazla ek alanı doğrulamak için bunları virgülle ayrılmış bir liste olarak belirtin. Örneğin, modele bir `MiddleName` özellik eklemek için aşağıdaki örnekte gösterildiği gibi `[Remote]` özniteliği ayarlayın:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`Tüm öznitelik bağımsız değişkenleri gibi, sabit bir ifade olmalıdır. Bu nedenle, bir ara [değerli dize](/dotnet/csharp/language-reference/keywords/interpolated-strings) veya başlatmak <xref:System.String.Join*> `AdditionalFields`için çağrı kullanmayın.

## <a name="alternatives-to-built-in-attributes"></a>Yerleşik özniteliklerin alternatifleri

Yerleşik öznitelikler tarafından sağlanmayan doğrulamaya ihtiyacınız varsa şunları yapabilirsiniz:

* [Özel öznitelikler oluşturun](#custom-attributes).
* [IValidatableObject uygulayın](#ivalidatableobject).

## <a name="custom-attributes"></a>Özel öznitelikler

Yerleşik doğrulama özniteliklerinin işlemeyen senaryolar için özel doğrulama öznitelikleri oluşturabilirsiniz. Öğesinden <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>devralan bir sınıf oluşturun ve <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> yöntemi geçersiz kılın.

Yöntemi, doğrulanacak girdi olan Value adlı bir nesne kabul eder. `IsValid` Aşırı yükleme, model bağlama `ValidationContext` tarafından oluşturulan model örneği gibi ek bilgiler sağlayan bir nesneyi de kabul eder.

Aşağıdaki örnek, *Klasik* tarz bir filmin yayın tarihinin belirtilen yıldan daha sonra olmadığını doğrular. Öznitelik önce tarzı denetler ve yalnızca klasik ise devam eder. `[ClassicMovie2]` Classics olarak tanımlanan filmler için, öznitelik oluşturucusuna geçirilen sınırdan daha sonra olmadığından emin olmak için yayın tarihini denetler.)

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

Yukarıdaki örnekteki `Movie` değişken, form gönderiminde verileri içeren bir nesneyi temsil eder. `movie` `IsValid` Yöntemi, tarihi ve tarzı denetler. Doğrulama başarıyla tamamlandığında, `IsValid` bir `ValidationResult.Success` kod döndürür. Doğrulama başarısız olduğunda, bir `ValidationResult` hata iletisi döndürür.

## <a name="ivalidatableobject"></a>IValidatableObject

Yukarıdaki örnek yalnızca türlerle birlikte `Movie` kullanılabilir. Aşağıdaki örnekte gösterildiği gibi, sınıf düzeyi doğrulama için başka `IValidatableObject` bir seçenek de model sınıfında uygulanır:

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a>Üst düzey düğüm doğrulaması

Üst düzey düğümler şunları içerir:

* Eylem parametreleri
* Denetleyici özellikleri
* Sayfa işleyici parametreleri
* Sayfa modeli özellikleri

Model bağlantılı üst düzey düğümler, model özelliklerini doğrulamaya ek olarak onaylanır. Örnek uygulamadan aşağıdaki örnekte, `VerifyPhone` yöntemi `phone` eylem parametresini doğrulamak <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> için öğesini kullanır:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

En üst düzey düğümler, doğrulama <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> öznitelikleriyle birlikte kullanılabilir. Örnek uygulamadaki aşağıdaki örnekte, `CheckAge` yöntemi, form gönderildiğinde `age` parametrenin sorgu dizesinden bağlanması gerektiğini belirtir:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

Denetim yaşı sayfasında (*Checkage. cshtml*) iki form vardır. İlk form bir `Age` `99` değeri sorgu dizesi olarak gönderir: `https://localhost:5001/Users/CheckAge?Age=99`.

Sorgu dizesinden düzgün şekilde `age` biçimlendirilen bir parametre gönderildiğinde, form doğrular.

Denetim yaşı sayfasındaki ikinci form, isteğin gövdesindeki `Age` değeri gönderir ve doğrulama başarısız olur. `age` Parametre bir sorgu dizesinden gelmesi gerektiğinden bağlama başarısız olur.

`CompatibilityVersion.Version_2_1` Veya sonraki sürümleriyle çalışırken, en üst düzey düğüm doğrulama varsayılan olarak etkindir. Aksi takdirde, üst düzey düğüm doğrulaması devre dışı bırakılır. Varsayılan seçenek, ( <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> `Startup.ConfigureServices`) içindeki özelliği burada gösterildiği gibi ayarlanarak geçersiz kılınabilir:

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a>En fazla hata sayısı

En fazla hata sayısına ulaşıldığında doğrulama durduruluyor (varsayılan olarak 200). Bu numarayı içinde `Startup.ConfigureServices`aşağıdaki kodla yapılandırabilirsiniz:

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a>En yüksek özyineleme

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor>doğrulanan modelin nesne grafiğinin gezgeçer. Çok derin olan veya sonsuz özyinelemeli özyinelemeli modeller için, doğrulama yığın taşmasına neden olabilir. [Mvcoptions. MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) , ziyaretçi özyineleme yapılandırılmış bir derinliği aşarsa doğrulamanın erken durdurulması için bir yol sağlar. Varsayılan değeri `MvcOptions.MaxValidationDepth` , veya ile `CompatibilityVersion.Version_2_2` çalışırken 200 ' dir. Önceki sürümler için değer null, bu da derinlemesine bir kısıtlama kısıtlaması anlamına gelir.

## <a name="automatic-short-circuit"></a>Otomatik kısa devre

Model grafı doğrulama gerektirmiyorsa, doğrulama otomatik olarak kısa devre dışı (atlandı). Çalışma zamanının, hiçbir doğrulayıcıya sahip olmayan temel elemanlar koleksiyonları ( `byte[]`, `string[]`,,, `Dictionary<string, string>`, ve gibi) için doğrulamayı atlayan nesneler.

## <a name="disable-validation"></a>Doğrulamayı devre dışı bırak

Doğrulamayı devre dışı bırakmak için:

1. Hiçbir alanı geçersiz olarak `IObjectModelValidator` işaretlememeyen bir uygulama oluşturun.

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. Bağımlılık ekleme kapsayıcısında varsayılan `Startup.ConfigureServices` `IObjectModelValidator` uygulamayı değiştirmek için aşağıdaki kodu ekleyin.

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

Model bağlamalarından kaynaklanan model durumu hatalarını görmeye devam edebilirsiniz.

## <a name="client-side-validation"></a>İstemci tarafı doğrulaması

İstemci tarafı doğrulama, form geçerli olana kadar gönderimi önler. Gönder düğmesi, formu gönderen veya hata iletilerini görüntüleyen JavaScript 'ı çalıştırır.

Bir form üzerinde giriş hataları olduğunda, istemci tarafı doğrulaması sunucuya gereksiz gidiş dönüş önler. *_Layout. cshtml* ve *_Validationscriptspartial. cshtml* içindeki aşağıdaki betik başvuruları istemci tarafı doğrulamayı destekler:

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

[JQuery unobtrusive doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiği, popüler [jQuery Validate](https://jqueryvalidation.org/) eklentisi üzerinde derleme yapan özel bir Microsoft ön uç kitaplığıdır. JQuery unobtrusive doğrulaması olmadan, iki yerde aynı doğrulama mantığını kodlamakta olmanız gerekir: model özelliklerindeki Sunucu tarafı doğrulama özniteliklerinde bir kez ve sonra istemci tarafı betiklerimizde. Bunun yerine, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , doğrulama gerektiren form öğeleri için HTML 5 `data-` özniteliklerini işlemek üzere model özelliklerinden doğrulama özniteliklerini ve tür meta verilerini kullanır. jQuery unobtrusive doğrulaması `data-` öznitelikleri ayrıştırır ve mantığı, sunucu tarafı doğrulama mantığını istemciye, etkili bir şekilde "kopyalamak" amacıyla jQuery doğrulamasına geçirir. Aşağıda gösterildiği gibi, etiket yardımcıları kullanarak istemcisinde doğrulama hatalarını görüntüleyebilirsiniz:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

Önceki etiket yardımcıları aşağıdaki HTML 'yi işler.

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
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

HTML çıkışındaki `ReleaseDate` özniteliklerin, özelliği için doğrulama özniteliklerine karşılık geldiğini unutmayın. `data-` `data-val-required` Öznitelik, Kullanıcı Yayın tarihi alanını doldurmazsa, görüntülenecek bir hata iletisi içerir. jQuery unobtrusive doğrulaması bu değeri jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) yöntemine geçirir, daha sonra bu iletiyi eşlik eden  **\<yayılma >** öğesinde görüntüler.

Veri türü doğrulama, bir `[DataType]` öznitelik tarafından geçersiz kılınmadığı müddetçe, özelliğin .NET türünü temel alır. Tarayıcıların kendi varsayılan hata iletileri vardır ancak jQuery doğrulaması unobtrusive doğrulama paketi bu iletileri geçersiz kılabilir. `[DataType]`gibi öznitelikler ve alt sınıflar `[EmailAddress]` , hata iletisini belirtmenize izin verir.

### <a name="add-validation-to-dynamic-forms"></a>Dinamik formlara doğrulama ekleme

jQuery unobtrusive doğrulaması, sayfa ilk yüklendiğinde jQuery doğrulaması için doğrulama mantığını ve parametreleri geçirir. Bu nedenle, doğrulama dinamik olarak üretilen formlarda otomatik olarak çalışmaz. Doğrulamayı etkinleştirmek için, jQuery 'ten kaçınmaya yönelik doğrulamayı, dinamik formu oluşturduktan hemen sonra ayrıştırmaya söyleyin. Örneğin, aşağıdaki kod, AJAX aracılığıyla eklenen bir formda istemci tarafı doğrulamayı ayarlar.

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

Yöntemi `$.validator.unobtrusive.parse()` , bir bağımsız değişkeni olarak bir jQuery seçiciyi kabul eder. Bu yöntem, `data-` jQuery 'in bu seçicideki formların özniteliklerini ayrıştırmasına izin vermez. Daha sonra bu özniteliklerin değerleri jQuery Validate eklentisine geçirilir.

### <a name="add-validation-to-dynamic-controls"></a>Dinamik denetimlere doğrulama ekleme

Yöntemi, `<input>` ve`<select/>`gibi dinamik olarak üretilen denetimlerde değil, formun tamamında işe yarar. `$.validator.unobtrusive.parse()` Formu yeniden oluşturmak için, aşağıdaki örnekte gösterildiği gibi, form daha önce ayrıştırıldığında eklenen doğrulama verilerini kaldırın:

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

## <a name="custom-client-side-validation"></a>Özel istemci tarafı doğrulaması

Özel istemci tarafı doğrulama, özel bir jQuery Validate `data-` bağdaştırıcısıyla çalışan HTML öznitelikleri oluşturarak yapılır. Aşağıdaki örnek bağdaştırıcı kodu, `ClassicMovie` Bu makalede daha önce sunulan ve `ClassicMovie2` öznitelikleri için yazılmıştır:

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

Bağdaştırıcıların nasıl yazılacağı hakkında daha fazla bilgi için [jQuery Validate belgelerine](https://jqueryvalidation.org/documentation/)bakın.

Belirli bir alan için bir bağdaştırıcının kullanımı, şu öznitelikler tarafından `data-` tetiklenir:

* Alana, doğrulamaya (`data-val="true"`) tabi olacak şekilde bayrak ekleyin.
* Bir doğrulama kuralı adı ve hata iletisi metni (örneğin, `data-val-rulename="Error message."`) belirler.
* Doğrulayıcı ihtiyaçlarına ek parametreler sağlayın (örneğin, `data-val-rulename-parm1="value"`).

Aşağıdaki örnek, örnek `data-` `ClassicMovie` uygulamanın özniteliği için öznitelikleri gösterir:

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

Daha önce belirtildiği gibi, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) , öznitelikleri işlemek `data-` için doğrulama özniteliklerinden bilgileri kullanır. Özel `data-` HTML özniteliklerinin oluşturulmasına neden olan kod yazmak için iki seçenek vardır:

* `AttributeAdapterBase<TAttribute>` Öğesinden`IValidationAttributeAdapterProvider`türeten bir sınıf oluşturun ve özniteliği ve kendi bağdaştırıcısını dı olarak kaydedin. Bu yöntem, sunucu ile ilgili ve istemciyle ilgili doğrulama kodundaki [tek sorumluluk sorumlusunu](https://wikipedia.org/wiki/Single_responsibility_principle) , ayrı sınıflarda izler. Ayrıca bağdaştırıcı, DI ' de kaydolduğundan bu yana de bunun avantajına sahiptir.
* `IClientModelValidator` Sınıfınıza`ValidationAttribute` uygulayın. Bu yöntem, öznitelik herhangi bir sunucu tarafı doğrulaması yapamazsa ve hiçbir hizmete gerek duymazsa uygun olabilir.

### <a name="attributeadapter-for-client-side-validation"></a>İstemci tarafı doğrulaması için AttributeAdapter

HTML 'de öznitelikleri işleme `data-` yöntemi örnek uygulamadaki `ClassicMovie` özniteliği tarafından kullanılır. Bu yöntemi kullanarak istemci doğrulaması eklemek için:

1. Özel doğrulama özniteliği için bir öznitelik bağdaştırıcı sınıfı oluşturun. Özniteliği [attributeadapterbase\<T >](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2)'ten türet. Aşağıdaki örnekte `AddValidation` gösterildiği gibi, `data-` işlenen çıktıya öznitelikler ekleyen bir yöntem oluşturun:

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. Uygulayan <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>bir bağdaştırıcı sağlayıcısı sınıfı oluşturun. `GetAttributeAdapter` Yöntemi içinde, aşağıdaki örnekte gösterildiği gibi, özel özniteliğini bağdaştırıcının oluşturucusuna geçirin:

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. Dı için bağdaştırıcı sağlayıcısını Kaydet `Startup.ConfigureServices`:

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a>İstemci tarafı doğrulaması için ılientmodelvalidator

HTML 'de öznitelikleri işleme `data-` yöntemi örnek uygulamadaki `ClassicMovie2` özniteliği tarafından kullanılır. Bu yöntemi kullanarak istemci doğrulaması eklemek için:

* Özel doğrulama özniteliğinde, `IClientModelValidator` arabirimini uygulayın ve bir `AddValidation` yöntem oluşturun. Yönteminde, aşağıdaki örnekte gösterildiği gibi, doğrulama için öznitelikler ekleyin `data-`: `AddValidation`

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a>İstemci tarafı doğrulamayı devre dışı bırak

Aşağıdaki kod, MVC görünümlerinde istemci doğrulamasını devre dışı bırakır:

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

Ve Razor Pages:

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

İstemci doğrulamasını devre dışı bırakmaya yönelik başka bir seçenek `_ValidationScriptsPartial` de, *. cshtml* dosyanızdaki başvurusunu açıklama olarak yorumlamadır.

## <a name="additional-resources"></a>Ek kaynaklar

* [System. ComponentModel. Dataaçıklamalarda ad alanı](xref:System.ComponentModel.DataAnnotations)
* [Model Bağlamaları](model-binding.md)
