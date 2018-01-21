---
title: "ASP.NET Core formlarında etiket Yardımcıları"
author: rick-anderson
description: "Etiket Yardımcıları formlarla kullanılan yerleşik açıklar."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9fbe2c5cb495aabee0e1f0bdb3871641efa03599
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>ASP.NET Core formlarında etiket Yardımcıları kullanmaya giriş

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), ve [Jerrie Pelser](https://github.com/jerriep)

Bu belge, formlar ve bir Form üzerinde kullanılan HTML öğeleri ile çalışma gösterir. HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) öğesi, sunucunun geri veri göndermek için birincil mekanizmayı web apps kullanımı sağlar. Bu belge çoğunu açıklar [etiket Yardımcıları](tag-helpers/intro.md) ve nasıl bunlar üretken sağlam HTML formları oluşturmanıza yardımcı olabilir. Okumanızı öneririz [etiket Yardımcıları giriş](tag-helpers/intro.md) önce bu belgeyi okuyun.

Çoğu durumda, HTML Yardımcıları alternatif bir yaklaşım için belirli bir etiket Yardımcısı sağlar, ancak etiket Yardımcıları HTML Yardımcıları değiştirmeyin ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir. HTML Yardımcısı alternatif mevcut olduğunda belirtiliyor.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Form etiketi Yardımcısı

[Form](https://www.w3.org/TR/html401/interact/forms.html) etiket Yardımcısı:

* HTML oluşturan [ \<FORM >](https://www.w3.org/TR/html401/interact/forms.html) `action` MVC denetleyici eylemi veya adlandırılmış rota için öznitelik değeri

* Gizli oluşturur [istek doğrulama belirteci](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) siteler arası istek sahteciliğini önlemek için (kullanıldığında `[ValidateAntiForgeryToken]` HTTP Post eylem yönteminde öznitelik)

* Sağlar `asp-route-<Parameter Name>` özniteliği, burada `<Parameter Name>` rota değerleri eklenir. `routeValues` Parametreleri `Html.BeginForm` ve `Html.BeginRouteForm` benzer işlevsellik sağlar.

* HTML Yardımcısı alternatif sahip `Html.BeginForm` ve`Html.BeginRouteForm`

Örnek:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Aşağıdaki HTML Form etiketi yardımcı yukarıdaki oluşturur:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

MVC çalışma zamanı oluşturur `action` öznitelik değeri Form etiketi yardımcı öznitelikleri `asp-controller` ve `asp-action`. Form etiketi yardımcı Ayrıca gizli oluşturur [istek doğrulama belirteci](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) siteler arası istek sahteciliğini önlemek için (kullanıldığında `[ValidateAntiForgeryToken]` HTTP Post eylem yöntemindeki özniteliği). Saf HTML Form siteler arası istek sahteciliğine koruma zordur, bu hizmet, Form etiketi yardımcı sağlar.

### <a name="using-a-named-route"></a>Adlandırılmış bir rotayı kullanma

`asp-route` Etiketi yardımcı öznitelik için HTML biçimlendirmesi de üretebilir `action` özniteliği. Bir uygulama ile bir [rota](../../fundamentals/routing.md) adlı `register` aşağıdaki biçimlendirme kayıt sayfası için kullanabilirsiniz:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Görünümlerde birçoğu *görünümler/hesap* klasörü (ile yeni bir web uygulaması oluşturduğunuzda oluşturulan *tek tek kullanıcı hesaplarını*) içeren [asp rota returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) özniteliği:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Yerleşik şablonlarıyla `returnUrl` , yetkili bir kaynağa erişmeyi deneyin ancak kimlik doğrulaması veya yetkili zaman yalnızca otomatik olarak doldurulur. Yetkisiz erişim denediğinizde, güvenlik ara yazılım, ile oturum açma sayfasına yönlendirir `returnUrl` ayarlayın.

## <a name="the-input-tag-helper"></a>Giriş etiketi Yardımcısı

Bir HTML giriş etiketi yardımcı bağlar [ \<Giriş >](https://www.w3.org/wiki/HTML/Elements/input) bir model ifadesi razor görünümünüzde öğesi.

Sözdizimi:

```HTML
<input asp-for="<Expression Name>" />
```

Giriş etiketi Yardımcısı:

* Oluşturur `id` ve `name` belirtilen ifade adı için HTML özniteliklerini `asp-for` özniteliği. `asp-for="Property1.Property2"`eşdeğer olan `m => m.Property1.Property2`. İfade için kullanılan adıdır `asp-for` öznitelik değeri. Bkz: [ifade adlarının](#expression-names) ek bilgi için bölüm.

* HTML ayarlar `type` öznitelik değeri model türüne göre ve [veri ek açıklamasını](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) model özelliğine uygulanan öznitelikleri

* HTML üzerine yazmaz `type` biri belirtildiğinde, öznitelik değeri

* Oluşturur [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) doğrulama öznitelikleri [veri ek açıklamasını](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) modeli özelliklerine uygulanan öznitelikleri

* Bir HTML Yardımcısı özelliği ile üst üste olan `Html.TextBoxFor` ve `Html.EditorFor`. Bkz: **giriş etiketi yardımcı HTML Yardımcısı alternatifleri** ayrıntıları bölümü.

* Güçlü yazarak sağlar. Özellik değişikliklerini ve adını etiket Yardımcısı güncelleştirmezseniz aşağıdakine benzer bir hata alırsınız:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` Etiket Yardımcısı ayarlar HTML `type` öznitelik tabanlı .NET türü. Aşağıdaki tabloda, bazı ortak .NET türleri ve oluşturulan HTML türü (her .NET türü listelenir) listeler.

|.NET türü|Giriş türü|
|---|---|
|bool|type=”checkbox”|
|Dize|type=”text”|
|DateTime|type=”datetime”|
|Bayt|type=”number”|
|int|type=”number”|
|Tek, çift|type=”number”|


Aşağıdaki tabloda, bazı ortak gösterilmektedir [veri ek açıklamaları](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) giriş etiketi Yardımcısı (her doğrulama özniteliği listelenir) belirli giriş türleri için eşler öznitelikleri:


|Öznitelik|Giriş türü|
|---|---|
|[EmailAddress]|türü "e-posta" =|
|[Url]|tür = "url"|
|[HiddenInput]|type=”hidden”|
|[Phone]|type=”tel”|
|[DataType(DataType.Password)]| tür = "parola"|
|[DataType(DataType.Date)]| type=”date”|
|[DataType(DataType.Time)]| türü "zaman" =|


Örnek:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Yukarıdaki kod aşağıdaki HTML oluşturur:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

Uygulanan veri ek açıklamaları `Email` ve `Password` özellikleri model meta verilerini oluşturur. Giriş etiketi yardımcı model meta verilerini kullanır ve üreten [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` öznitelikleri (bkz [Model doğrulama](../models/validation.md)). Bu öznitelikler için girdi alanlarının eklenecek doğrulayıcıları açıklanmaktadır. Bu örtük HTML5 sağlar ve [jQuery](https://jquery.com/) doğrulama. Örtük özniteliklerinin biçimine sahip `data-val-rule="Error Message"`, burada, kural doğrulama kuralının adıdır (gibi `data-val-required`, `data-val-email`, `data-val-maxlength`vb..) Bir hata iletisi özniteliğinde sağlanırsa, değeri olarak görüntülenir `data-val-rule` özniteliği. Ayrıca formun özniteliklerini vardır `data-val-ruleName-argumentName="argumentValue"` Örneğin, kural hakkındaki ek ayrıntıları sağlayın `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Giriş etiketi yardımcı HTML Yardımcısı alternatifleri

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` ve `Html.EditorFor` giriş etiketi yardımcı özelliklerle çakışmamalıdır. Giriş etiketi yardımcı otomatik olarak ayarlayacak `type` özniteliği; `Html.TextBox` ve `Html.TextBoxFor` almayacak. `Html.Editor`ve `Html.EditorFor` işlemek koleksiyonlar, karmaşık nesneler ve şablonları; giriş etiketi yardımcı desteklemez. Giriş etiketi yardımcı `Html.EditorFor` ve `Html.TextBoxFor` kesin türü belirtilmiş (bunlar lambda ifadeleri kullanma); `Html.TextBox` ve `Html.Editor` değil (bunlar ifade adlarının kullanın).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`ve `@Html.EditorFor()` özel bir kullanmak `ViewDataDictionary` adlı girdi `htmlAttributes` kendi varsayılan şablonları yürütülürken. Bu davranış kullanarak isteğe bağlı olarak engagement'ta `additionalViewData` parametreleri. Anahtarı "htmlAttributes" büyük/küçük harf duyarlıdır. "HtmlAttributes" anahtar benzer şekilde işlenir `htmlAttributes` nesnesi geçirildi gibi Yardımcıları giriş `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>İfade adları

`asp-for` Öznitelik değeri bir `ModelExpression` ve sağ tarafındaki bir lambda ifadesi. Bu nedenle, `asp-for="Property1"` hale `m => m.Property1` olmasının nedeni oluşturulan kodda ile önek gerekmez `Model`. Kullanabileceğiniz "@" satır içi ifadeye başlatmak ve önce taşımak için karakter `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Aşağıdaki oluşturur:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Koleksiyon Özellikleri ile `asp-for="CollectionProperty[23].Member"` aynı adı taşıyan oluşturur `asp-for="CollectionProperty[i].Member"` zaman `i` değerine sahip `23`.

### <a name="navigating-child-properties"></a>Alt özellikleri gezinme

Özellik yolu görünüm modeli kullanarak alt özelliklerine de gidebilirsiniz. Bir alt içeren daha karmaşık bir model sınıfı göz önünde bulundurun `Address` özelliği.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

Görünümü'nde biz bağlamak `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Aşağıdaki HTML için oluşturulan `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>İfade adlarının ve koleksiyonları

Örnek, bir dizi içeren bir model `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Eylem yöntemi:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

Belirli bir erişim nasıl aşağıdaki Razor gösterir `Color` öğe:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml* şablonu:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Kullanarak örnek `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Aşağıdaki Razor, koleksiyon üzerinde yinelenecek gösterilmektedir:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml* şablonu:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Her zaman kullanmak `for` (ve *değil* `foreach`) listesini yineleme. Bir dizin oluşturucu bir LINQ değerlendirme ifadesi pahalı olabilir ve küçültülmesine.

&nbsp;

>[!NOTE]
>Lambda ifadesi ile nasıl değiştirirsiniz yukarıdaki açıklamalı örnek kod gösterir `@` her erişmek için işleci `ToDoItem` listesinde.

## <a name="the-textarea-tag-helper"></a>Textarea etiket Yardımcısı

`Textarea Tag Helper` Etiket Yardımcısı giriş etiketi yardımcıya benzerdir.

* Oluşturur `id` ve `name` öznitelikleri ve model için veri doğrulama öznitelikleri bir [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) öğesi.

* Güçlü yazarak sağlar.

* HTML Yardımcısı alternatif:`Html.TextAreaFor`

Örnek:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Aşağıdaki HTML oluşturulur:

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>Etiket etiket Yardımcısı

* Etiket resim yazısı oluşturur ve `for` özniteliği bir [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) öğesi için bir ifade adı

* HTML Yardımcısı alternatif: `Html.LabelFor`.

`Label Tag Helper` Saf bir HTML label öğesini aşağıdaki avantajları sağlar:

* Açıklayıcı bir etiket değeri otomatik olarak Al `Display` özniteliği. Hedeflenen görünen adı zaman ve birleşimi değişebilir `Display` özniteliği ve etiket etiket Yardımcısı geçerli `Display` her yerde kullanılır.

* Kaynak kodu daha az biçimlendirme

* Model özelliğiyle yazarak tanımlayıcı.

Örnek:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Aşağıdaki HTML için oluşturulan `<label>` öğe:

```HTML
<label for="Email">Email Address</label>
```

Oluşturulan etiket etiket Yardımcısı `for` kimliği "e" öznitelik değeri ilişkili `<input>` öğesi. Etiket Yardımcıları tutarlı oluşturmak `id` ve `for` öğeleri doğru bir şekilde ilişkili olabilir. Bu örnekte resim yazısı geldiği `Display` özniteliği. Model içermeyen varsa bir `Display` özniteliği, resim yazısını ifade özellik adı olacaktır.

## <a name="the-validation-tag-helpers"></a>Doğrulama etiket Yardımcıları

İki doğrulama etiket Yardımcıları vardır. `Validation Message Tag Helper` (Görüntüleyen bir doğrulama ileti tek bir özellik için model üzerinde) ve `Validation Summary Tag Helper` (doğrulama hatalarının özetini görüntüleyen). `Input Tag Helper` Öğeleri temel verilerini, model sınıflarınızı ek açıklama özniteliklerinde giriş HTML5 istemci tarafı doğrulama öznitelikleri ekler. Doğrulama da sunucu üzerinde gerçekleştirilir. Bir doğrulama hatası oluştuğunda doğrulama etiket Yardımcısı bu hata iletileri görüntüler.

### <a name="the-validation-message-tag-helper"></a>Doğrulama ileti etiketi Yardımcısı

* Ekler [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` özniteliğini [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) giriş alanını belirtilen model özelliğinin bir doğrulama hata iletisi ekler öğesi. Bir istemci tarafı doğrulama hatası oluştuğunda [jQuery](https://jquery.com/) hata iletisi görüntüler `<span>` öğesi.

* Doğrulama sunucuda da gerçekleşir. İstemcileri JavaScript devre dışı olabilir ve bazı doğrulaması yalnızca sunucu tarafında yapılabilir.

* HTML Yardımcısı alternatif:`Html.ValidationMessageFor`

`Validation Message Tag Helper` İle kullanılan `asp-validation-for` bir HTML özniteliğinin [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) öğesi.

```HTML
<span asp-validation-for="Email"></span>
```

Doğrulama ileti etiketi Yardımcısı aşağıdaki HTML üretir:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Genellikle kullandığınız `Validation Message Tag Helper` sonra bir `Input` aynı özelliği için etiket Yardımcısı. Bunun yapılması hataya giriş yakın herhangi bir doğrulama hata iletisi görüntüler.

> [!NOTE]
> Bir görünümü doğru JavaScript ile olmalıdır ve [jQuery](https://jquery.com/) komut dosyası, istemci tarafı doğrulama yerinde başvuruları. Bkz: [Model doğrulama](../models/validation.md) daha fazla bilgi için.

(Örneğin ne zaman özel sunucu tarafı doğrulama sahip veya istemci tarafı doğrulama devre dışı) bir sunucu tarafı doğrulama hatası meydana geldiğinde, MVC, hata iletisi gövdesi olarak yerleştirir. `<span>` öğesi.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Doğrulama Özeti etiket Yardımcısı

* Hedefleri `<div>` öğeleriyle `asp-validation-summary` özniteliği

* HTML Yardımcısı alternatif:`@Html.ValidationSummary`

`Validation Summary Tag Helper` Doğrulama iletilerinin bir özetini görüntülemek için kullanılır. `asp-validation-summary` Öznitelik değeri aşağıdakilerden biri olabilir:

|ASP doğrulama özeti|Görüntülenen doğrulama iletileri|
|--- |--- |
|ValidationSummary.All|Özellik ve model düzeyi|
|ValidationSummary.ModelOnly|Model|
|ValidationSummary.None|Yok.|

### <a name="sample"></a>Örnek

Aşağıdaki örnekte, veri modeli ile donatılmış `DataAnnotation` üzerinde doğrulama hatası iletilerini oluşturur özniteliklerini `<input>` öğesi.  Bir doğrulama hatası oluştuğunda doğrulama etiket Yardımcısı hata iletisini görüntüler:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Oluşturulan HTML (model geçerli olduğunda):

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>Select etiket Yardımcısı

* Oluşturur [seçin](https://www.w3.org/wiki/HTML/Elements/select) ve ilişkili [seçeneği](https://www.w3.org/wiki/HTML/Elements/option) modelinizi özelliklerini için öğeleri.

* HTML Yardımcısı alternatif sahip `Html.DropDownListFor` ve`Html.ListBoxFor`

`Select Tag Helper` `asp-for` Modeli özellik adını belirtir [seçin](https://www.w3.org/wiki/HTML/Elements/select) öğesi ve `asp-items` belirtir [seçeneği](https://www.w3.org/wiki/HTML/Elements/option) öğeleri.  Örneğin:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Örnek:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` Yöntemi başlatır `CountryViewModel`, seçilen ülke ayarlar ve buna ileten `Index` görünümü.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST `Index` yöntemi seçimi görüntüler:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` Görünümü:

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Aşağıdaki HTML ("CA" Seçili ile) oluşturur:

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> Kullanmanızı önermiyoruz `ViewBag` veya `ViewData` seçin etiket Yardımcısı ile. Bir görünüm MVC meta verileri sağlayarak, daha sağlam ve genellikle daha az sorunlu modelidir.

`asp-for` Öznitelik değeri özel bir durumdur ve gerektirmeyen bir `Model` önek, bir etiketi yardımcı öznitelik yapın (gibi `asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Enum bağlama

Genellikle kullanmak uygun olduğunu `<select>` ile bir `enum` özelliği ve oluşturmak `SelectListItem` öğelerinden `enum` değerleri.

Örnek:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` Yöntem oluşturur bir `SelectList` bir numaralandırma nesnesi.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Numaralandırıcı listenizi tasarlamanız `Display` özniteliğini daha zengin bir kullanıcı Arabirimi almak için:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Aşağıdaki HTML oluşturulur:

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a>Seçenek grubu

HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) öğenin bir veya daha fazla görünüm modeli içerdiğinde, oluşturulan `SelectListGroup` nesneleri.

`CountryViewModelGroup` Grupları `SelectListItem` "Kuzey Amerika" ve "Avrupa" gruplar halinde öğeleri:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

İki grup aşağıda verilmiştir:

![seçenek grubu örneği](working-with-forms/_static/grp.png)

Oluşturulan HTML:

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>Çoklu seçim

Seçin etiket Yardımcısı otomatik olarak oluşturur [birden çok "birden çok" =](http://w3c.github.io/html-reference/select.html) özelliği olarak belirtilmişse özniteliği `asp-for` özniteliği bir `IEnumerable`. Örneğin, aşağıdaki model verilen:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Aşağıdaki görünümü ile:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Aşağıdaki HTML oluşturur:

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>Herhangi bir seçim

Birden çok sayfada "belirtilmedi" seçeneğini kullanarak kendiniz bulursanız, HTML yinelenen ortadan kaldırmak için bir şablon oluşturabilirsiniz:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml* şablonu:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

HTML ekleme [ \<seçeneği >](https://www.w3.org/wiki/HTML/Elements/option) öğeleri sınırlı değil *herhangi bir seçim* durumda. Örneğin, aşağıdaki görünümü ve eylem yöntemi HTML Yukarıdaki kod benzer üretir:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Doğru `<option>` öğesi seçilir (içeren `selected="selected"` özniteliği) geçerli bağlı olarak `Country` değeri.

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>Ek Kaynaklar

* [Etiket Yardımcıları](tag-helpers/intro.md)

* [HTML Form öğesi](https://www.w3.org/TR/html401/interact/forms.html)

* [İstek doğrulama belirteci](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [Model Bağlamaları](../models/model-binding.md)

* [Model doğrulama](../models/validation.md)

* [Veri ek açıklamaları](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* [Kod parçacıkları bu belge için](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).
