---
title: ASP.NET Core MVC model doğrulama
author: tdykstra
description: ASP.NET Core MVC model doğrulama hakkında bilgi edinin.
ms.author: riande
ms.date: 07/31/2018
uid: mvc/models/validation
ms.openlocfilehash: f407903577e40b6501737ef5b78d90e1e3e60c06
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39378673"
---
# <a name="model-validation-in-aspnet-core-mvc"></a>ASP.NET Core MVC model doğrulama

Tarafından [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Model doğrulama giriş

Uygulama, bir veritabanına veri depolayan önce uygulama verileri doğrulamak gerekir. Veri türü ve boyutu ile uygun şekilde biçimlendirilir ve, kurallara uymalıdır doğrulandı, olası güvenlik tehditlerini denetlenmesi gerekir. Yedekli ve uygulamak yorucu bir süreç olabilir ancak doğrulama gerekli değildir. MVC'de, doğrulama istemci ve sunucu üzerinde gerçekleşir.

Neyse ki, .NET doğrulama içinde doğrulama özniteliklerinin soyutlanır. Bu öznitelikler, böylece yazmanız gereken kod miktarını azaltarak bir doğrulama kodu içerir.

[Görüntülemek veya örnek Github'dan indirin](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Doğrulama öznitelikleri

Doğrulama özniteliklerinin model doğrulama veritabanı tablolarındaki alanlarda doğrulama için kavramsal olarak benzer şekilde yapılandırmak için bir yoludur. Bu, veri türleri veya gerekli alanları atama gibi kısıtlamalar içerir. E-posta adresi veya telefon numarası, kredi kartı gibi iş kuralları zorlamak için veri desenleri uygulama doğrulama diğer türleri içerir. Doğrulama özniteliklerinin çok daha basit ve daha kolay kullanılan bu gereksinimleri zorunlu yapın.

Ek açıklama aşağıdadır `Movie` film ve TV programları hakkında bilgi depolayan bir uygulamadan model. Özelliklerin çoğu gerekli ve çeşitli dize özellikleri uzunluk gereksinimlerine sahiptir. Ayrıca, bir sayısal aralık kısıtlaması için bir yerde yoktur `Price` özelliğini 0 $999.99, birlikte özel doğrulama özniteliği.

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

Yalnızca modeliyle okuma kodun bakımını yapma, bu uygulamaya ilişkin veriler hakkında kurallar ortaya çıkarır. Aşağıda birkaç yaygın olarak kullanılan yerleşik doğrulama öznitelikleri şunlardır:

* `[CreditCard]`: Doğrular özelliği kredi kartı biçimdedir.

* `[Compare]`: Bir modeli eşleşme iki özelliklerinde doğrular.

* `[EmailAddress]`: Doğrulama e-posta biçimini özelliğine sahiptir.

* `[Phone]`: Doğrulama telefon biçimi özelliğine sahiptir.

* `[Range]`: Belirtilen aralık içinde özellik değeri düştüğünde doğrular.

* `[RegularExpression]`: Veri belirtilen normal ifadeyle eşleştiğini doğrular.

* `[Required]`: Gerekli bir özellik sağlar.

* `[StringLength]`: Bir dize özelliğini verilen en fazla uzunluğu en fazla sahip olduğunu doğrular.

* `[Url]`: Doğrulama özellik bir URL biçimine sahip.

MVC destekler, türetilen herhangi bir öznitelik `ValidationAttribute` doğrulama amacıyla. Birçok yararlı doğrulama öznitelikleri bulunabilir [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) ad alanı.

Yerleşik özniteliklerini sağlamak çok daha fazla özellik gerek duyduğunuz örnekleri olabilir. Bu kez türeterek özel doğrulama öznitelikleri oluşturabilirsiniz `ValidationAttribute` veya uygulamak için modelinizi değiştirme `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Gerekli öznitelik kullanımı ile ilgili notlar

Atanamayan [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) (gibi `decimal`, `int`, `float`, ve `DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği. Uygulama işaretli olmayan-boş değer atanabilir türler için herhangi bir sunucu tarafı doğrulama denetimi gerçekleştirir `Required`.

Doğrulama öznitelikleri doğrulama ile ilgili değil, MVC model bağlama için alamayan bir tür boşluk veya eksik bir değer içeren bir form alanını gönderme reddeder. Mevcut olmadığında bir `BindRequired` özniteliği hedef özelliği, model bağlama form alanı olan eksik atanamaz türler için eksik veri yok sayar. gelen form verileri.

[BindRequired özniteliği](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (Ayrıca bkz: [model bağlama davranışı öznitelikleri olan özelleştirme](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) form verilerini tam olduğundan emin olmak kullanışlıdır. Bir özelliğe uygulandığında, model bağlama sistemi bu özellik için bir değer gerektirir. Bir türe başvurulduğunda, model bağlama sistemi tüm bu tür özellikleri için değerleri gerektirir.

Kullandığınızda, bir [Nullable\<T > türü](/dotnet/csharp/programming-guide/nullable-types/) (örneğin, `decimal?` veya `System.Nullable<decimal>`) ve işaretleyin `Required`, bir sunucu tarafı doğrulama denetimi özelliği (için standart bir boş değer atanabilir tür adlarıymış gerçekleştirilir Örneğin, bir `string`).

İstemci tarafı doğrulama, işaretlediğiniz bir model özelliğine karşılık gelen bir form alanı için bir değer gerektiriyor `Required` ve işaretlenmiş henüz bir değer atanamayan tür özelliği için `Required`. `Required` istemci tarafı doğrulama hata iletisini kontrol etmek için kullanılabilir.

## <a name="model-state"></a>Model durumu

Model durumu doğrulama hatalarını gönderilen HTML form değerleri temsil eder.

MVC kadar ulaştığında alanları doğrulama hataları (varsayılan olarak, 200) sayısı devam eder. Aşağıdaki kodu ekleyerek bu sayı yapılandırabilirsiniz `ConfigureServices` yönteminde *Startup.cs* dosyası:

[!code-csharp[](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>İşleme Model durumu hataları

Model doğrulama çağrılan her denetleyici eylemi önce oluşur ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki verin. Çoğu durumda, uygun tepki ideal model doğrulamasının başarısız olmasının nedeni gerçekleşen bir hata yanıtı geri döndürmektir.

Bazı uygulamalar, durum filtre böyle bir ilke uygulamak için uygun bir yere olabilir, model doğrulama hataları başa çıkmak için standart bir kural uygulayın seçeceksiniz. Eylemlerinizi ile geçerli ve geçersiz model durumlarının nasıl davranacağını test etmeniz gerekir.

## <a name="manual-validation"></a>El ile doğrulama

Model bağlama ve doğrulama tamamlandıktan sonra bazı bölümleri yineleyin isteyebilirsiniz. Örneğin, bir kullanıcı metin alanı bir tamsayı bekleniyor girmiş veya bir modelin özelliği için bir değer işlem gerekebilir.

Doğrulama el ile çalıştırmanız gerekebilir. Bunu yapmak için çağrı `TryValidateModel` burada gösterildiği gibi yöntemi:

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Özel doğrulama

Çoğu doğrulama gereksinimi için doğrulama öznitelikleri çalışır. Ancak, bazı doğrulama kuralları işletmenize özgü değildir. Kurallarınızı ortak veri doğrulama teknikleri gibi bir alan sağlama gereklidir veya değer aralığı için uygun olmayabilir. Bu senaryolar için özel doğrulama öznitelikleri harika bir çözümdür. Kendi özel doğrulama öznitelikleri MVC'de oluşturmak kolaydır. Yalnızca devralınacak `ValidationAttribute`ve geçersiz kılma `IsValid` yöntemi. `IsValid` Yöntemi, iki parametre kabul eden ilk adlı bir nesnedir *değer* ve ikincisi ise bir `ValidationContext` adlı nesne *validationContext*. *Değer* , özel Doğrulayıcı doğrulama alanından gerçek değeri gösterir.

Kullanıcılar Tarz ayarlamaz, aşağıdaki örnekte, bir iş kuralı durumları *Klasik* 1960 sonra yayımlanmış bir filmi için. `[ClassicMovie]` Özniteliği Tarz ilk denetler ve klasik ise, yayın tarihi 1960 sonraki olup olmadığını denetler. Yayımlandıktan sonra 1960 doğrulama başarısız olur. Öznitelik verileri doğrulamak için kullanabileceğiniz bir yıl temsil eden bir tamsayı parametre kabul eder. Öznitelik oluşturucu parametresi değerini, aşağıda gösterildiği gibi yakalayabilirsiniz:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

`movie` Temsil yukarıda değişken bir `Movie` doğrulamak için form gönderme verilerini içeren nesne. Bu durumda, tarih ve tarzında bir doğrulama kodu denetler `IsValid` yöntemi `ClassicMovieAttribute` kurallara göre sınıfı. Doğrulama başarılı bağlı`IsValid` döndürür bir `ValidationResult.Success` kod. Doğrulama başarısız olduğunda bir `ValidationResult` ile ilgili bir hata iletisi döndürülür:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=55-58)]

Ne zaman bir kullanıcıyı değiştirir `Genre` alan ve formu gönderdiği `IsValid` yöntemi `ClassicMovieAttribute` film Klasik olup olmadığını doğrulayın. Yerleşik herhangi bir öznitelik gibi uygulama `ClassicMovieAttribute` gibi bir özelliğe `ReleaseDate` doğrulama olduğunda, önceki kod örneğinde gösterildiği gibi emin olmak için. Yalnızca örnek çalışır olduğundan `Movie` türleri, daha iyi bir seçenek ise `IValidatableObject` içine aşağıdaki paragrafı gösterildiği gibi.

Alternatif olarak, bu aynı kod modelde uygulayarak yerleştirilebilir `Validate` metodunda `IValidatableObject` arabirimi. Özel doğrulama öznitelikleri de ayrı ayrı özellikler doğrulamak için çalışırken, uygulama `IValidatableObject` sınıf düzeyinde doğrulama, burada görüldüğü gibi uygulamak için kullanılabilir.

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>İstemci tarafı doğrulama

İstemci tarafı doğrulama, kullanıcılar için harika bir kolaylığıdır. Gidiş dönüş için bekleme zaman aksi açabiliyorduk sunucuya kaydeder. İş dünyasında bile birkaç kez her gün yüzlerce çarpılan saniyelerin kesirlerini ekler kadar çok fazla zaman harcama ve sıkıntıya olabilir. Basit ve anında doğrulama kullanıcıların giriş ve çıkış daha iyi kalite üreten ve daha verimli çalışmanıza olanak tanır.

Burada gördüğünüz şekilde çalışması istemci tarafı doğrulama yerinde uygun JavaScript komut dosyası başvuruları olan bir görünümü olması gerekir.

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[JQuery örtük doğrulaması](https://github.com/aspnet/jquery-validation-unobtrusive) betiğidir popüler üzerinde oluşturan özel bir ön uç Microsoft Kitaplığı [jQuery doğrulama](https://jqueryvalidation.org/) eklentisi. JQuery örtük doğrulaması aynı doğrulama mantığı iki yerde kod gerekirdi: model özellikleri, sunucu tarafı doğrulama özniteliklerinin kez ve ardından tekrar istemci tarafı betikleri (örnekler için jQuery doğrulama 's [ `validate()` ](https://jqueryvalidation.org/validate/) yöntemi karmaşık nasıl bu duruma gelebilir gösterir). Bunun yerine, MVC'nin [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) doğrulama öznitelikleri kullanın ve türü meta verileri HTML 5 işlemek için model özelliklerinden [veri öznitelikleri](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) içinde doğrulama gerekli form öğeleri. MVC oluşturur `data-` yerleşik ve özel öznitelikler için öznitelikler. jQuery doğrulaması örtük ardından ayrıştırır `data-` öznitelikleri ve jQuery doğrulama, etkili bir şekilde "sunucu tarafı doğrulama mantığını istemciye kopyalama" mantığı geçirir. Burada gösterildiği gibi ilgili etiket Yardımcıları kullanılarak istemcide doğrulama hataları görüntüleyebilirsiniz:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Yukarıdaki etiket Yardımcıları aşağıdaki HTML'yi işleyin. Dikkat `data-` HTML özniteliklerinde çıkış için doğrulama öznitelikleri karşılık `ReleaseDate` özelliği. `data-val-required` Özniteliği aşağıdaki kullanıcı yayın tarih alanı doldurmazsa görüntülenecek hata iletisi içerir. jQuery doğrulaması örtük jQuery doğrulama için bu değer geçirir [ `required()` ](https://jqueryvalidation.org/required-method/) sonra bu iletiyi eşlik yöntemini  **\<span >** öğesi.

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

Formun geçerli olana kadar istemci tarafı doğrulama gönderimi engeller. Gönder düğmesine formu gönderdiği veya hata iletilerini görüntüler JavaScript çalışır.

MVC kullanarak büyük olasılıkla geçersiz bir özelliği .NET veri türüne göre türü öznitelik değerleri belirler `[DataType]` öznitelikleri. Temel `[DataType]` öznitelik gerçek sunucu tarafı doğrulama yapar. Tarayıcılar, kendi hata iletileri'ı seçin ve gibi istedikleri, ancak jQuery doğrulama örtük paket mesajların ve diğerleri ile tutarlı bir şekilde görüntülemek, bu hataları görüntüleyin. En açıkça kullanıcılar uyguladığınızda böyle `[DataType]` kılabileceği gibi `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Doğrulama için dinamik formlar ekleyin

Sayfa ilk yüklendiğinde örtük doğrulama jQuery Doğrulama mantığı ve parametreleri jQuery doğrulama geçirdiği için dinamik olarak oluşturulan form doğrulama otomatik olarak göstermesi gerekmez. Bunun yerine, jQuery hemen oluşturduktan sonra dinamik biçimi ayrıştırmak için örtük doğrulaması söylemeniz gerekir. Örneğin, aşağıdaki kodu, AJAX eklenen bir form üzerinde istemci tarafı doğrulamasını nasıl ayarlayabilir gösterir.

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

`$.validator.unobtrusive.parse()` Yöntemi jQuery Seçicisi kendi bir bağımsız değişkeni kabul eder. Bu yöntem jQuery ayrıştırmak için örtük doğrulaması söyler `data-` formları içinde seçicinin öznitelikleri. Böylece form istenen istemci tarafı doğrulama kurallarını sergiler bu özniteliklerinin değerlerini sonra için jQuery doğrulama eklentisi geçirilir.

### <a name="add-validation-to-dynamic-controls"></a>Dinamik denetimlere doğrulama ekleme

Tek denetimleri gibi bir form üzerinde doğrulama kuralları da güncelleştirebilirsiniz `<input/>`s ve `<select/>`s, dinamik olarak oluşturulur. Bu öğeleri için seçiciler geçirilemez `parse()` yöntemi doğrudan çevresindeki formu zaten ayrıştırılır ve güncelleştirme olmaz. Bunun yerine, önce varolan doğrulama verileri kaldırın ve sonra aşağıda gösterildiği gibi tüm formu yeniden ayrıştırma:

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

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Özel özniteliği için istemci tarafı mantığı oluşturabilir ve [örtük doğrulama](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) bir bağdaştırıcıya oluşturan [jquery doğrulama](http://jqueryvalidation.org/documentation/) parçası olarak otomatik olarak sizin için istemcide çalıştırır doğrulama. Hangi veri öznitelikleri uygulayarak eklenen denetlemek için ilk adımıdır `IClientModelValidator` arabirim burada gösterildiği gibi:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Bu arabirimi uygulayan öznitelikleri HTML öznitelikleri için oluşturulan alanlar ekleyebilirsiniz. İçin çıktıyı İnceleme `ReleaseDate` ortaya artık dışında önceki örneğe benzer bir HTML öğesi bir `data-val-classicmovie` tanımlanan öznitelik `AddValidation` yöntemi `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Örtük doğrulama verilerde kullanan `data-` hata iletilerini görüntülemek için öznitelikler. Ancak, jQuery kurallar hakkında bilmez veya jQuery için 's ekleyene kadar iletileri `validator` nesne. Bu özel ekleyen aşağıdaki örnekte gösterilen `classicmovie` istemci doğrulama yöntemi için jQuery `validator` nesne. Bir açıklaması için `unobtrusive.adapters.add` yöntemi bkz [ASP.NET mvc'de örtük istemci doğrulama](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html).

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?name=snippet_UnobtrusiveValidation)]

Yukarıdaki kod ile `classicmovie` yöntemi film yayın tarih istemci tarafı doğrulama gerçekleştirir. Yöntem döndürürse, hata iletisi görüntüler `false`.

## <a name="remote-validation"></a>Uzak doğrulama

Uzak doğrulama, sunucu üzerindeki verileri karşı istemcide verilerin doğrulamak gerektiğinde kullanmak için harika bir özelliktir. Örneğin, bir e-posta veya kullanıcı adı zaten kullanımda ve çok miktarda Bunu yapmak için veri sorgulaması gereken olup olmadığını doğrulamak, uygulamanız gerekebilir. Büyük indirme doğrulamak için bir veri kümelerini veya birkaç alan çok fazla kaynak tüketir. Ayrıca hassas bilgilerin ortaya çıkarabilir. Bir alanı doğrulamak için bir gidiş dönüş istekte bulunmak için bir alternatiftir.

Uzak doğrulama iki adımlı bir işlemin uygulayabilirsiniz. İlk olarak, modelinizi açıklama `[Remote]` özniteliği. `[Remote]` Özniteliği doğrudan çağırmak için uygun kodu için istemci tarafı JavaScript için kullanabileceğiniz birden çok aşırı yükleme kabul eder. Aşağıdaki örnekte işaret `VerifyEmail` eylem yöntemi `Users` denetleyicisi.

[!code-csharp[](validation/sample/User.cs?range=7-8)]

İkinci adım bir doğrulama kodu karşılık gelen eylem yönteminde tanımlanan koyuyor `[Remote]` özniteliği. JQuery doğrulama göre [uzak](https://jqueryvalidation.org/remote-method/) yöntemi belgeleri, sunucu yanıtı geçerli bir JSON dizesi olmalıdır:

* `"true"` Geçerli öğeler için.
* `"false"`, `undefined`, veya `null` geçersiz öğeler için varsayılan hata iletisini kullanarak.

Sunucu yanıtı bir dize ise (örneğin, `"That name is already taken, try peter123 instead"`), dize olarak varsayılan dize yerine bir özel hata iletisi görüntülenir.

Tanımı `VerifyEmail` yöntemi aşağıda gösterildiği gibi şu kuralları takip eder. Doğrulama hatasını döndürür e-posta alınmışsa, ileti veya `true` e-posta ücretsizdir ve sonuçta saran bir `JsonResult` nesne. İstemci tarafı hata gerekirse görüntülemek veya devam etmek için döndürülen değer sonra kullanabilirsiniz.

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

Artık kullanıcılar bir e-posta girdiğinde, JavaScript görünümünde bu e-posta alınmış ve bu durumda, hata iletisi görüntüler, görmek için Uzak çağrıda bulunur. Aksi takdirde, kullanıcı, formu her zaman olduğu gibi gönderebilirsiniz.

`AdditionalFields` Özelliği `[Remote]` özniteliği karşı sunucuda veri alanlarının kombinasyonları doğrulamak için kullanışlıdır. Örneğin, varsa `User` yukarıdaki modeli olan adlı iki ek özellik `FirstName` ve `LastName`, mevcut hiçbir kullanıcı adları, çiftinin olduğunu doğrulamak isteyebilirsiniz. Yeni özellikleri, aşağıdaki kodda gösterildiği gibi tanımlayın:

[!code-csharp[](validation/sample/User.cs?range=10-13)]

`AdditionalFields` açıkça dizeleri için ayarlanmış `"FirstName"` ve `"LastName"`, ancak kullanarak [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) işleci bu gibi daha sonra yeniden düzenleme basitleştirir. Doğrulamayı gerçekleştirmek için bir eylem yönteminin ardından değeri için iki bağımsız kabul etmelisiniz `FirstName` değeri için ve biri `LastName`.

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

Artık, kullanıcıların bir adı ve Soyadı, JavaScript girin:

* Bu adları çiftinin geçen görmek için bir uzak çağrı yapar.
* Çift alınmış bir hata iletisi görüntülenir.
* Geçen değil, kullanıcı formu gönderebilirsiniz.

İki veya daha fazla ek alanlar doğrulamak gereken `[Remote]` özniteliği, sağladığınız bunları bir virgülle ayrılmış liste olarak. Örneğin, eklemek için bir `MiddleName` model özelliğine ayarlayın `[Remote]` öznitelik aşağıdaki kodda gösterildiği gibi:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, tüm öznitelik bağımsız değişkenleri gibi bir sabit ifade olmalıdır. Bu nedenle, değil kullanmalısınız bir [ilişkilendirilmiş bir dizedir](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) veya çağrı [ `string.Join()` ](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) başlatmak için `AdditionalFields`. Eklediğiniz her ek alan için `[Remote]` özniteliği, karşılık gelen denetleyici eylem yöntemine başka bir bağımsız değişken eklemeniz gerekir.
