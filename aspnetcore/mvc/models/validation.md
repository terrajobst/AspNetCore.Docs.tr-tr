---
title: "ASP.NET Core MVC model doğrulama"
author: rachelappel
description: "ASP.NET Core MVC model doğrulama hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: a0c7de12e0d9abbe5d1706cf775dfeb2c067c760
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>Model doğrulama ASP.NET Core mvc'de giriş

Tarafından [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Model doğrulama giriş

Bir uygulama, bir veritabanında veri depolayan önce uygulama verileri doğrulamanız gerekir. Uygun şekilde türüne ve boyutuna göre biçimlendirilmiş olan ve sizin kurallarına uymalıdır doğrulandı, olası güvenlik tehditlerini için veri denetlenmesi gerekir. Doğrulama gerekli olsa yedekli ve uygulamak can sıkıcı olabilir. MVC uygulamasında istemci ve sunucu üzerinde doğrulama olur.

Neyse ki, .NET doğrulama doğrulama öznitelikleri içine soyutlanır. Bu öznitelikler doğrulama kodu, böylece kod yazmanız gerekir miktarının azaltılması içerir.

[Görüntülemek veya örnek Github'dan indirdiğinizde](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Doğrulama öznitelikleri

Doğrulama öznitelikleri model doğrulama kavramsal doğrulama, veritabanı tablolarındaki alanlar için benzer şekilde yapılandırmak için bir yoldur. Bu, veri türleri veya gerekli alanları atama gibi kısıtlamaları içerir. Desenler bir kredi kartı, telefon numarası gibi iş kurallarını uygulayabilir veya e-posta adresi için verilere uygulamadan doğrulama diğer türleri içerir. Bu gereksinimleri çok daha basit ve kullanmayı daha kolay zorlamayı doğrulama öznitelikleri olun.

Ek açıklama aşağıdadır `Movie` model bir uygulamadan filmler ve TV programları ile ilgili bilgileri depolar. Özelliklerin çoğu gereklidir ve birkaç dize özellikleri uzunluk gereksinimlerine sahip. Ayrıca, için yerinde sayısal aralığın bir kısıtlama yoktur `Price` özel doğrulama özniteliği birlikte $999.99, 0'dan özelliğine.

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

Yalnızca modeli aracılığıyla okuma kuralları hakkında daha kolay kod korumak, bu uygulama için veri ortaya çıkarır. Aşağıda birkaç popüler yerleşik doğrulama öznitelikleri şunlardır:

* `[CreditCard]`: Doğrular özellik bir kredi kartı biçime sahip.

* `[Compare]`: Bir model eşleşme iki özelliklerinde doğrular.

* `[EmailAddress]`: Doğrular özellik bir e-posta biçime sahip.

* `[Phone]`: Doğrular özellik bir telefon biçime sahip.

* `[Range]`: Özellik değeri düştüğünde belirtilen aralıkta doğrular.

* `[RegularExpression]`: Veri belirtilen normal ifadeyle eşleşen doğrular.

* `[Required]`: Gerekli bir özellik sağlar.

* `[StringLength]`: Bir dize özelliği verilen en fazla uzunluğu en fazla sahip olduğunu doğrular.

* `[Url]`: Doğrular bir URL biçimi özelliği vardır.

MVC destekleyen türetilen herhangi bir öznitelik `ValidationAttribute` doğrulama amacıyla. Çok sayıda kullanışlı doğrulama öznitelikleri bulunabilir [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) ad alanı.

Yerleşik öznitelikleri sağlamak özelliklerden daha fazlasını ihtiyaç duyacağınız örneği olabilir. Bu kez, türetilen özel doğrulama öznitelikleri oluşturabilirsiniz `ValidationAttribute` veya uygulamak için modelinizin değiştirme `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Gerekli öznitelik kullanımı ile ilgili notlar

Olamayan [değer türleri](/dotnet/csharp/language-reference/keywords/value-types) (gibi `decimal`, `int`, `float`, ve `DateTime`) kendiliğinden gereklidir ve gerekmeyen `Required` özniteliği. Uygulama yok işaretlenmiş null türleri için sunucu tarafında doğrulama denetler `Required`.

Doğrulama ve doğrulama öznitelikleri ile ilgili değil, MVC model bağlama alamayan bir tür için boşluk veya eksik bir değer içeren bir form alanı gönderme reddeder. Olmadığında bir `BindRequired` özniteliği hedef özelliği, model bağlama null türleri için form alanı olan etmeksizin, eksik verilerin yoksayar gelen form verileri.

[BindRequired özniteliği](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (Ayrıca bkz. [model bağlama davranışı öznitelikleri olan özelleştirme](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) form verilerini tam olduğundan emin olmak yararlıdır. Bir özelliğe uygulandığında, model bağlama sistem bu özellik için bir değer gerektirir. Bir türe uygulandığında, model bağlama sistem türü özelliklerinin tümü için değerleri gerektirir.

Kullandığınızda, bir [null atanabilir\<T > türü](/dotnet/csharp/programming-guide/nullable-types/) (örneğin, `decimal?` veya `System.Nullable<decimal>`) ve işaretleyin `Required`, özelliği (için standart null olabilir bir tür değilmiş gibi bir sunucu tarafı doğrulama denetimi gerçekleştirilir Örneğin, bir `string`).

İstemci tarafı doğrulama işaretlenen model özelliğine karşılık gelen bir form alanı için bir değer gerektiren `Required` ve işaretli olmayan bir null tür özelliği için `Required`. `Required`istemci tarafı doğrulama hata iletisi denetlemek için kullanılabilir.

## <a name="model-state"></a>Model durumu

Model durumu doğrulama hataları gönderilen HTML form değerleri temsil eder.

MVC alanları kadar ulaştığında doğrulama hatası (200 varsayılan olarak) sayısı devam eder. Aşağıdaki kodu ekleyerek bu numarayı yapılandırabilirsiniz `ConfigureServices` yönteminde *haline* dosyası:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>İşleme Model durumu hataları

Model doğrulama çağrılan her denetleyici eylemi önce gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki. Çoğu durumda uygun tepki ideal neden model doğrulama başarısızlık nedenini ayrıntılı bir hata yanıtı getirmektir.

Bazı uygulamalar, model doğrulama hataları, durum filtre böyle bir ilke uygulamak için uygun bir yerdir olabilir ilgilenmek için standart bir kuralı izlemek seçeceksiniz. Geçerli ve geçersiz model durumlarıyla eylemlerinizi nasıl davranacağını test etmeniz gerekir.

## <a name="manual-validation"></a>El ile doğrulama

Model bağlama ve doğrulama tamamlandıktan sonra bazı bölümleri yineleyin isteyebilirsiniz. Örneğin, bir kullanıcı metin tamsayı bekleniyor bir alana girmiş veya bir modelin özelliği için bir değer işlem gerekebilir.

Doğrulama el ile çalıştırmanız gerekebilir. Bunu yapmak için arama `TryValidateModel` aşağıda gösterildiği gibi yöntemi:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Özel doğrulama

Doğrulama özniteliklerinin çoğu doğrulama ihtiyaçları için çalışır. Ancak, bazı doğrulama kuralları, işletmenizle özgüdür. Kurallarınızı, gerekli bir alan sağlama veya değer aralığı için uyumlu olduğunu gibi ortak veri doğrulama teknikleri olmayabilir. Bu senaryolarda, özel doğrulama öznitelikleri harika bir çözümdür. MVC'de kendi özel doğrulama öznitelikleri oluşturmak kolaydır. Yalnızca devralınan `ValidationAttribute`ve geçersiz kılma `IsValid` yöntemi. `IsValid` Yöntemi iki parametre kabul eden ilk adlı bir nesnedir *değeri* ve ikincisi ise bir `ValidationContext` adlı nesne *validationContext*. *Değer* özel Doğrulayıcı doğrulama alanından gerçek değeri gösterir.

Kullanıcılar bir tarzını ayarlamaz, aşağıdaki örnekte, bir iş kuralı durumları *Klasik* 1960 sonra yayımlanan film için. `[ClassicMovie]` Özniteliği Tarz ilk denetler ve klasik ise, yayın tarihi 1960 sonraki olup olmadığını denetler. 1960 sonra yayımlanan doğrulama başarısız olur. Öznitelik verileri doğrulamak için kullanabileceğiniz yılı temsil eden bir tamsayı parametresini kabul eder. Aşağıda gösterildiği gibi özniteliğin oluşturucuda parametresinin değeri yakalamak için:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

`movie` Temsil yukarıda değişken bir `Movie` doğrulamak için form gönderme verilerini içeren nesne. İçinde bir tarzını ve tarihi bu durumda, doğrulama kodu denetler `IsValid` yöntemi `ClassicMovieAttribute` sınıfı kuralları göredir. Doğrulama başarılı bağlı `IsValid` döndürür bir `ValidationResult.Success` kodunu ve doğrulama başarısız olduğunda bir `ValidationResult` hata iletisiyle. Ne zaman bir kullanıcıyı değiştirir `Genre` alan ve formu gönderdikten `IsValid` yöntemi `ClassicMovieAttribute` film Klasik olup olmadığını doğrular. Yerleşik herhangi bir öznitelik gibi uygulama `ClassicMovieAttribute` gibi bir özelliğe `ReleaseDate` doğrulama olur, önceki kod örneğinde gösterildiği gibi sağlamak için. Bu örnek yalnızca birlikte çalışır. bu yana `Movie` türleri, daha iyi bir seçenektir kullanmak için `IValidatableObject` aşağıdaki paragrafta gösterildiği gibi.

Alternatif olarak, bu aynı kodu modelde uygulayarak yerleştirilemedi `Validate` yöntemi `IValidatableObject` arabirimi. Özel doğrulama öznitelikleri de ayrı ayrı özellikler doğrulamak için çalışırken, uygulama `IValidatableObject` sınıf düzeyi doğrulama burada görüldüğü gibi uygulamak için kullanılabilir.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>İstemci tarafı doğrulama

İstemci tarafı doğrulama kullanıcıların harika bir kolaylık sağlamaya yöneliktir. Gidiş dönüş bekleniyor aksi harcadığınız zamanı sunucuya kaydeder. İş bağlamında, saniyede yüzlerce kez her gün çarpılan bile birkaç kesirlerini ekler kadar çok zaman, gider ve aksiliklerin olması. Basit ve hemen doğrulama daha verimli olarak çalışabilir ve giriş ve çıkış daha iyi kalite üretmek kullanıcıların sağlar.

Burada gördüğünüz şekilde çalışması istemci tarafı doğrulama yerinde uygun JavaScript komut dosyası başvuruları görünümüyle olması gerekir.

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[JQuery örtük doğrulama](https://github.com/aspnet/jquery-validation-unobtrusive) betiğidir popüler üzerinde oluşturan özel bir ön uç Microsoft Kitaplığı [jQuery doğrulama](https://jqueryvalidation.org/) eklentisi. JQuery örtük doğrulama iki yerde de aynı doğrulama mantığını kod gerekir: model özellikleri, sunucu tarafı doğrulama özniteliklerinde kez ve ardından yeniden istemci tarafı komut dosyalarını (jQuery doğrulama ait örnekler [ `validate()` ](https://jqueryvalidation.org/validate/) yöntemi gösterilir nasıl karmaşık bu hale gelebilir). Bunun yerine, MVC'ın [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) ve [HTML Yardımcıları](xref:mvc/views/overview) doğrulama öznitelikleri kullanın ve tür meta verileri HTML 5 işlemek için model özelliklerinden [veri öznitelikleri](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) içinde doğrulama gerekli form öğeleri. MVC oluşturur `data-` yerleşik ve özel öznitelikleri için öznitelikler. jQuery örtük doğrulama sonra bu ayrıştırır `data-` öznitelikleri ve jQuery etkili bir şekilde "sunucu tarafı doğrulama mantığını istemciye kopyalama" Doğrulama mantığı geçirir. Aşağıda gösterildiği gibi ilgili etiket Yardımcıları kullanılarak istemcide doğrulama hataları görüntüleyebilirsiniz:

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Yukarıdaki etiket Yardımcıları HTML işleme. Dikkat `data-` HTML öznitelikleri çıktı için doğrulama öznitelikleri karşılık `ReleaseDate` özelliği. `data-val-required` Özniteliği aşağıdaki kullanıcı yayın tarihi alanına doldurmazsa görüntülenecek hata iletisi içeriyor. jQuery örtük doğrulama jQuery doğrulama için bu değeri geçirir [ `required()` ](https://jqueryvalidation.org/required-method/) daha sonra bu iletiyi eşlik görüntüler yöntemi  **\<span >** öğesi.

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

Formu geçerli olana kadar istemci tarafı doğrulama gönderimi engeller. Gönder düğmesine formu gönderdikten veya hata iletileri görüntüler JavaScript çalışır.

MVC belirler türü öznitelik değerleri büyük olasılıkla kullanarak geçersiz kılınan bir özellik .NET veri türüne göre `[DataType]` öznitelikleri. Temel `[DataType]` öznitelik hiçbir gerçek sunucu tarafında doğrulama yapar. Tarayıcılar kendi hata iletileri seçin ve bu hataların gibi istedikleri, ancak jQuery doğrulama örtük paket iletileri geçersiz kılar ve bunları tutarlı bir şekilde başkalarıyla görüntülemek görüntüleyin. Bu en açıkça kullanıcılar uygulandığında gerçekleşir `[DataType]` gibi alt sınıfların `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Dinamik formlarına doğrulama ekleme

Sayfa ilk kez yüklediğinde jQuery örtük doğrulama doğrulama mantığını ve parametreleri jQuery doğrulama atladığı için dinamik olarak üretilen forms otomatik olarak doğrulama sergilemesine olmaz. Bunun yerine, jQuery hemen oluşturduktan sonra dinamik formun ayrıştırmak için örtük doğrulama bildirmeniz gerekir. Örneğin, aşağıdaki kodu, AJAX eklenen bir form üzerinde istemci tarafı doğrulamasını nasıl ayarlayabilir gösterir.

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

`$.validator.unobtrusive.parse()` Yöntemi için bağımsız değişken jQuery Seçici kabul eder. Bu yöntem jQuery ayrıştırmak için örtük doğrulama söyler `data-` forms seçicinin içinde öznitelikleri. Böylece form istenen istemci tarafı doğrulama kurallarını sergiler özniteliklerle değerlerini sonra jQuery doğrulama eklentisi için geçirilir.

### <a name="add-validation-to-dynamic-controls"></a>Doğrulama için dinamik denetimleri ekleme

Tek denetimleri gibi bir form üzerinde doğrulama kuralları da güncelleştirebilirsiniz `<input/>`s ve `<select/>`s, dinamik olarak oluşturulur. Bu öğeler için seçiciler geçiremezsiniz `parse()` doğrudan yöntemi çünkü çevresindeki formu zaten ayrıştırılır ve güncelleştirme olmaz. Bunun yerine, önce varolan doğrulama verileri kaldırın, ardından aşağıda gösterildiği gibi tüm formu yeniden ayrıştırma:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

İstemci tarafı mantığı, özel öznitelik için oluşturabilir ve [örtük doğrulama](http://jqueryvalidation.org/documentation/) , doğrulama bir parçası olarak otomatik olarak istemcide çalıştırır. Hangi veri öznitelikleri uygulayarak eklenen denetlemek için ilk adımdır `IClientModelValidator` arabirim aşağıda gösterildiği gibi:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Bu arabirimi uygulayan öznitelikleri HTML öznitelikleri için oluşturulan alanlar ekleyebilirsiniz. Çıkış için inceleniyor `ReleaseDate` öğesi artık dışında önceki örneğe benzer HTML ortaya çıkarır bir `data-val-classicmovie` tanımlanan öznitelik `AddValidation` yöntemi `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Örtük doğrulama kullanır verilerde `data-` hata iletilerini görüntülemek için öznitelikler. Ancak, jQuery kuralları hakkında bilmiyor veya jQuery için 's ekleyene kadar iletileri `validator` nesnesi. Bu adlı bir yöntem ekleyen aşağıdaki örnekte gösterilen `classicmovie` jQuery için özel istemci doğrulama kodunu içeren `validator` nesnesi.

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

Artık jQuery özel JavaScript doğrulama ve bunun yanı sıra bu doğrulama kodu false değeri döndürülürse görüntülenecek hata iletisi yürütme bilgilere sahiptir.

## <a name="remote-validation"></a>Uzak doğrulama

Uzak doğrulama, sunucu üzerindeki veriler karşı istemcide verilerin doğrulamak gerektiğinde kullanmak için harika bir özelliktir. Örneğin, bir e-posta veya kullanıcı adı zaten kullanımda ve büyük miktarda veri Bunu yapmak için sorgu gerekir olup olmadığını doğrulamak, uygulamanız gerekebilir. Bir doğrulama veri kümelerini büyük indirme veya birkaç alan çok fazla kaynak tüketen. Ayrıca hassas bilgileri açığa. Bir alan doğrulamak üzere gidiş dönüş isteğinde bir alternatiftir.

Uzak doğrulama iki adımda uygulayabilirsiniz. İlk olarak, modelinizi açıklama `[Remote]` özniteliği. `[Remote]` Özniteliği doğrudan çağırmak için uygun kodu için istemci tarafı JavaScript için kullanabileceğiniz birden çok aşırı kabul eder. Aşağıdaki örnek işaret `VerifyEmail` eylem yöntemi `Users` denetleyicisi.

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

İkinci adım doğrulama kodu karşılık gelen eylem yönteminde tanımlandığı şekilde koyuyor `[Remote]` özniteliği. JQuery doğrulama göre [ `remote()` ](https://jqueryvalidation.org/remote-method/) yöntemi belgelerine:

> Gelince yanıt olmalıdır bir JSON dizesinde olmalıdır `"true"` geçerli öğeler için ve `"false"`, `undefined`, veya `null` geçersiz öğeleri için varsayılan hata iletisini kullanarak. Gelince yanıt örneğin bir dize ise. `"That name is already taken, try peter123 instead"`, bu dize olarak varsayılan yerine bir özel hata iletisi görüntülenir.

Tanımını `VerifyEmail()` yöntemi aşağıda gösterildiği gibi bu kurallar izler. Doğrulama hatasını döndürür e-posta alınmışsa, ileti veya `true` e-posta ücretsizdir ve sonuçta sarmalar bir `JsonResult` nesnesi. İstemci tarafı sonra devam etmek gerekirse hata görüntülemek için döndürülen değer'nı kullanabilirsiniz.

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

Artık kullanıcıların bir e-posta girdiğinizde, JavaScript görünümünde e-posta alındıktan ve bu durumda, hata iletisi görüntüler varsa, görmek için Uzak çağrıda bulunur. Aksi takdirde kullanıcı formu her zamanki gibi gönderebilirsiniz.

`AdditionalFields` Özelliği `[Remote]` öznitelik alanları sunucuda veri karşı birleşimlerini doğrulamak için yararlıdır. Örneğin, varsa `User` modeli üstten vardı adlı iki ek özellikler `FirstName` ve `LastName`, varolan kullanıcı adları bu çifti zaten yüklü olduğunu doğrulamak isteyebilirsiniz. Aşağıdaki kodda gösterildiği gibi yeni özellikler tanımlayın:

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields`açıkça dizelere ayarlanmış `"FirstName"` ve `"LastName"`, ancak kullanarak [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) bu like işleci basitleştirir daha sonra yeniden düzenleme. Doğrulamayı gerçekleştirmek için eylem yönteminin ardından değeri için iki bağımsız kabul etmeniz gerekir `FirstName` değeri için bir tane `LastName`.

[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

Bir ad ve Soyadı, JavaScript artık kullanıcıların girdiğinizde:

* Bu adları çiftinin gerçekleştirilecek görmek için Uzak çağrıda bulunur.
* Çift gerçekleştirilecek, bir hata iletisi görüntülenir. 
* Gerçekleştirilecek değil, kullanıcı formu gönderebilirsiniz.

İki veya daha fazla ek alanlar doğrulamak gereken `[Remote]` özniteliği, sağladığınız bunları bir virgülle ayrılmış liste olarak. Örneğin, eklemek için bir `MiddleName` model özelliğine ayarlayın `[Remote]` özniteliği aşağıdaki kodda gösterildiği gibi:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, tüm öznitelik bağımsız değişkenleri gibi sabit bir ifade olması gerekir. Bu nedenle, değil kullanmalısınız bir [Ara değerli dize](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) veya arama [ `string.Join()` ](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) başlatmak için `AdditionalFields`. Eklediğiniz her ek alan için `[Remote]` özniteliği, başka bir bağımsız değişken için karşılık gelen denetleyici eylem yöntemi eklemeniz gerekir.
