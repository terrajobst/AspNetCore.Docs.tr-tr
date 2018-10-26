---
title: ASP.NET Core formlarda etiket Yardımcıları
author: rick-anderson
description: Formlarda etiket yardımcılarını kullanılan yerleşik açıklar.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/views/working-with-forms
ms.openlocfilehash: efc71cc8d072769cde07e129b6d9bb933fc2f7af
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090222"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a>ASP.NET Core formlarda etiket Yardımcıları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), ve [Jerrie Pelser](https://github.com/jerriep)

Bu belge, form ve Form üzerinde kullanılan HTML öğeleri ile çalışma gösterir. HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) öğesi, sunucunun geri veri göndermek için birincil mekanizma web apps kullanımını sağlar. Bu belge çoğunu açıklar [etiket Yardımcıları](tag-helpers/intro.md) ve bunların nasıl verimli sağlam HTML formları oluşturmanıza yardımcı. Okumanızı öneririz [etiket Yardımcıları giriş](tag-helpers/intro.md) önce bu belgeyi okuyun.

Çoğu durumda, HTML Yardımcıları, belirli bir etiketi Yardımcısı için alternatif bir yaklaşım sağlar, ancak etiket Yardımcıları HTML Yardımcıları değiştirin yoktur ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir. HTML Yardımcısı alternatif mevcut olduğunda, belirtilmiyor.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Form etiketi Yardımcısı

[Form](https://www.w3.org/TR/html401/interact/forms.html) etiket Yardımcısı:

* HTML oluşturan [ \<FORM >](https://www.w3.org/TR/html401/interact/forms.html) `action` MVC denetleyicisi eylemi veya adlandırılmış bir rota için öznitelik değeri

* Gizli oluşturur [istek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) siteler arası istek sahteciliğini önlemek için (ile kullanıldığında `[ValidateAntiForgeryToken]` HTTP Post eylem yönteminde öznitelik)

* Sağlar `asp-route-<Parameter Name>` özniteliği burada `<Parameter Name>` rota değerleri için eklenir. `routeValues` Parametreleri `Html.BeginForm` ve `Html.BeginRouteForm` benzer bir işlevsellik sağlar.

* HTML Yardımcısı alternatif olan `Html.BeginForm` ve `Html.BeginRouteForm`

Örnek:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Yukarıdaki Form etiketi Yardımcısı aşağıdaki HTML'yi oluşturur:

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

MVC çalışma zamanının oluşturduğu `action` öznitelik değeri Form etiketi Yardımcısı öznitelikleri `asp-controller` ve `asp-action`. Form etiketi Yardımcısı aynı zamanda bir gizli oluşturur [istek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) siteler arası istek sahteciliğini önlemek için (ile kullanıldığında `[ValidateAntiForgeryToken]` HTTP Post eylem yönteminin özniteliği). Saf bir HTML Form siteler arası istek sahteciliğini koruma zordur, bu hizmet, Form etiketi Yardımcısı sağlar.

### <a name="using-a-named-route"></a>Adlandırılmış bir rotayı kullanma

`asp-route` Etiketi yardımcı öznitelik için HTML biçimlendirme da üretebilir `action` özniteliği. Bir uygulama ile bir [rota](../../fundamentals/routing.md) adlı `register` aşağıdaki biçimlendirme için kayıt sayfasına kullanabilirsiniz:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Görünümlerde çoğu *görünümler/hesap* klasörü (ile yeni bir web uygulaması oluşturduğunuzda oluşturulan *bireysel kullanıcı hesapları*) içeren [asp rota returnurl](xref:mvc/views/working-with-forms) özniteliği:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Yerleşik şablonlarla `returnUrl` , yetkili bir kaynağa erişmeyi deneyin ancak kimlik doğrulaması veya yetkili yaptığınızda yalnızca otomatik olarak doldurulur. Yetkisiz erişim denediğinizde, güvenlik ara yazılım, oturum açma sayfasına yönlendirir `returnUrl` ayarlayın.

## <a name="the-input-tag-helper"></a>Giriş etiketi Yardımcısı

Bir HTML giriş etiketi Yardımcısı bağlar [ \<Giriş >](https://www.w3.org/wiki/HTML/Elements/input) , razor görünüm modeli ifadesinde öğesi.

Sözdizimi:

```HTML
<input asp-for="<Expression Name>" />
```

Giriş etiketi Yardımcısı:

* Oluşturur `id` ve `name` belirtilen ifade adı için HTML özniteliklerini `asp-for` özniteliği. `asp-for="Property1.Property2"` eşdeğerdir `m => m.Property1.Property2`. İçin kullanılan ifade adıdır `asp-for` öznitelik değeri. Bkz: [ifade adlarının](#expression-names) ek bilgi bölümü.

* HTML ayarlar `type` öznitelik değeri model türü temel alarak ve [veri ek açıklama](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) model özelliğine uygulanan öznitelikleri

* HTML üzerine yazılmayacak `type` biri belirtildiğinde öznitelik değeri

* Oluşturur [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) doğrulama öznitelikleri gelen [veri ek açıklama](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) modeli özelliklerine uygulanan öznitelikleri

* Bir HTML Yardımcısı özelliği ile çakışan `Html.TextBoxFor` ve `Html.EditorFor`. Bkz: **giriş etiketi Yardımcısı HTML Yardımcısı alternatifleri** ayrıntıları bölümü.

* Güçlü yazmayı sağlar. Özellik değişiklikleri ve adını etiketi Yardımcısı güncelleştirmezseniz aşağıdakine benzer bir hata alırsınız:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` HTML etiketi Yardımcısı ayarlar `type` öznitelik tabanlı üzerinde .NET türü. Aşağıdaki tabloda, bazı yaygın .NET türleri ve oluşturulan HTML türü (her .NET türü listelenir) listeler.

|.NET türü|Giriş türü|
|---|---|
|bool|tür "onay kutusu" =|
|Dize|tür = "text"|
|DateTime|tür "datetime" =|
|Bayt|tür = "number"|
|int|tür = "number"|
|Tek, Double|tür = "number"|


Aşağıdaki tablo bazı yaygın gösterir [veri ek açıklamaları](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) giriş etiketi Yardımcısı (her doğrulama özniteliği listelenir) belirli giriş türleri için eşler öznitelikleri:


|Öznitelik|Giriş türü|
|---|---|
|[EmailAddress]|tür = "email"|
|[Url]|tür = "url"|
|[HiddenInput]|tür "gizli" =|
|[Phone]|tür "tel" =|
|[DataType(DataType.Password)]| tür = "password"|
|[DataType(DataType.Date)]| tür "tarih" =|
|[DataType(DataType.Time)]| tür "zamanda" =|


Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Yukarıdaki kod, aşağıdaki HTML'yi oluşturur:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
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

Uygulanan veri ek açıklamaları `Email` ve `Password` özellikleri model meta verilerini oluşturur. Giriş etiketi Yardımcısı model meta verilerini kullanır ve üretir [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` öznitelikleri (bkz [Model doğrulama](../models/validation.md)). Bu öznitelikler için giriş alanlarını eklemek için doğrulayıcıları açıklanmaktadır. Bu örtük HTML5 sağlar ve [jQuery](https://jquery.com/) doğrulama. Örtük özniteliklerinin biçimine sahip `data-val-rule="Error Message"`kuralı adı doğrulama kuralının olduğu (gibi `data-val-required`, `data-val-email`, `data-val-maxlength`vb..) Öznitelik, bir hata iletisi sağlanırsa, değeri olarak görüntülenir `data-val-rule` özniteliği. Form öznitelikler de vardır `data-val-ruleName-argumentName="argumentValue"` gibi kural hakkındaki ek ayrıntıları sağlayın `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>HTML Yardımcısı alternatifleri giriş etiketi Yardımcısı

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` ve `Html.EditorFor` giriş etiketi Yardımcısı özelliklerle örtüşüyor. Giriş etiketi Yardımcısı otomatik olarak ayarlayacak `type` özniteliği; `Html.TextBox` ve `Html.TextBoxFor` olmaz. `Html.Editor` ve `Html.EditorFor` işlemek koleksiyonlar ve karmaşık nesneler şablonları; girişi etiketi Yardımcısı değil. Giriş etiketi Yardımcısı `Html.EditorFor` ve `Html.TextBoxFor` türü kesin belirlenmiş (bunlar, lambda ifadeleri kullanma); `Html.TextBox` ve `Html.Editor` değildir (bunlar ifade adlarının kullanın).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()` ve `@Html.EditorFor()` özel bir kullanın `ViewDataDictionary` adlı giriş `htmlAttributes` kendi varsayılan şablonları yürütülürken. Bu davranış kullanarak isteğe bağlı olarak Genişletilmiş `additionalViewData` parametreleri. ' % S'anahtarı "htmlAttributes" büyük/küçük harf duyarlıdır. ' % S'anahtarı "htmlAttributes" benzer şekilde işlendiğini `htmlAttributes` nesnesi geçirildi yardımcıları gibi giriş `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>İfade adları

`asp-for` Öznitelik değeri bir `ModelExpression` ve bir lambda ifadesinin sağ tarafı. Bu nedenle, `asp-for="Property1"` olur `m => m.Property1` olmasının nedeni oluşturulan kodda ile önek gerekmez `Model`. Kullanabileceğiniz "\@" satır içi ifadesi başlatmak ve önce taşımak için karakter `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Aşağıdakileri oluşturur:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Koleksiyon Özellikleri ile `asp-for="CollectionProperty[23].Member"` aynı adı taşıyan oluşturur `asp-for="CollectionProperty[i].Member"` olduğunda `i` değerine sahip `23`.

Zaman değerini hesaplar ASP.NET Core MVC `ModelExpression`, dahil, çeşitli kaynaklardan inceler `ModelState`. Göz önünde bulundurun `<input type="text" asp-for="@Name" />`. Hesaplanan `value` özniteliktir boş olmayan ilk değeri:

* `ModelState` "Name" anahtarla girişi.
* İfadenin sonucu `Model.Name`.

### <a name="navigating-child-properties"></a>Alt özellikleri gezinme

Görünüm modeli, özellik yolu kullanarak alt özellikleri için de gidebilirsiniz. Bir alt içeren daha karmaşık bir model sınıfı göz önünde bulundurun `Address` özelliği.

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

Görünümü'nde, biz bağlamak `Address.AddressLine1`:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Aşağıdaki HTML'yi için oluşturulan `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>İfade adlarının ve koleksiyonları

Örnek, bir dizi içeren bir model `Colors`:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Eylem yöntemi:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

Aşağıdaki Razor, belirli bir erişim nasıl gösterir `Color` öğesi:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml* şablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Kullanarak örnek `List<T>`:

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Aşağıdaki Razor, bir koleksiyon üzerinde yinelemek gösterilmektedir:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml* şablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

>[!NOTE]
>Her zaman `for` (ve *değil* `foreach`) bir liste üzerinde yineleme yapmak için. Değerlendirme bir dizin oluşturucu bir LINQ ifadesi, pahalı olabilir ve küçültülmesine.

&nbsp;

>[!NOTE]
>Yukarıdaki açıklamalı örnek kod nasıl lambda ifadesiyle değiştirin gösterir `@` her erişim işleci `ToDoItem` listesinde.

## <a name="the-textarea-tag-helper"></a>Textarea etiketi Yardımcısı

`Textarea Tag Helper` Etiketi Yardımcısı için giriş etiketi Yardımcısı benzerdir.

* Oluşturur `id` ve `name` öznitelikleri ve model için doğrulama öznitelikleri veri bir [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) öğesi.

* Güçlü yazmayı sağlar.

* HTML Yardımcısı alternatif: `Html.TextAreaFor`

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Aşağıdaki HTML'yi oluşturulur:

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

## <a name="the-label-tag-helper"></a>Etiket etiketi Yardımcısı

* Etiket başlığını oluşturur ve `for` özniteliği bir [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) öğesi için bir ifade adı

* HTML Yardımcısı alternatif: `Html.LabelFor`.

`Label Tag Helper` Saf bir HTML label öğesini aşağıdaki avantajları sağlar:

* Açıklayıcı bir etiket değerini otomatik olarak Al `Display` özniteliği. Hedeflenen görünen adı, saati ve birleşimi değişebilir `Display` özniteliği ve etiket etiketi Yardımcısı geçerli `Display` her yerde kullanılır.

* Kaynak kodunda daha az biçimlendirme

* İle model özelliğine yazarak güçlü.

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Aşağıdaki HTML'yi için oluşturulan `<label>` öğesi:

```HTML
<label for="Email">Email Address</label>
```

Oluşturulan etiket etiketi Yardımcısı `for` kimliği "Email" özniteliğinin değerini ilişkili `<input>` öğesi. Etiket Yardımcıları tutarlı oluşturmak `id` ve `for` öğelerinin doğru bir şekilde ilişkili olabilir. Bu örnekte açıklamalı alt yazı geldiği `Display` özniteliği. Modele sahip değilse, bir `Display` özniteliği, açıklamalı alt yazı ifade özellik adı olacaktır.

## <a name="the-validation-tag-helpers"></a>Doğrulama etiket Yardımcıları

İki doğrulama etiket Yardımcıları vardır. `Validation Message Tag Helper` (Görüntüleyen tek bir özellik için bir doğrulama iletisini modelinize göre) ve `Validation Summary Tag Helper` (doğrulama hatalarının özetini görüntüleyen). `Input Tag Helper` HTML5 istemci tarafı doğrulama özniteliklerinin öğeleri model sınıflarınızı ek açıklama özniteliklerinde verileri temel alarak giriş ekler. Doğrulama Ayrıca sunucu üzerinde gerçekleştirilir. Doğrulama etiketi Yardımcısı, bir doğrulama hatası oluştuğunda bu hata iletilerini görüntüler.

### <a name="the-validation-message-tag-helper"></a>Doğrulama iletisi etiketi Yardımcısı

* Ekler [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` özniteliğini [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) öğesinin giriş alanını belirtilen model özelliğinin bir doğrulama hata iletisi ekler. İstemci tarafı doğrulama hatası oluştuğunda [jQuery](https://jquery.com/) hata iletisi görüntüler `<span>` öğesi.

* Doğrulama sunucusundaki de gerçekleşir. JavaScript devre dışı istemciniz ve bazı doğrulama yalnızca sunucu tarafında gerçekleştirilebilir.

* HTML Yardımcısı alternatif: `Html.ValidationMessageFor`

`Validation Message Tag Helper` İle kullanılan `asp-validation-for` bir HTML özniteliğinin [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) öğesi.

```HTML
<span asp-validation-for="Email"></span>
```

Doğrulama iletisi etiketi Yardımcısı aşağıdaki HTML'yi oluşturur:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Genel olarak kullandığınız `Validation Message Tag Helper` sonra bir `Input` aynı özellik için etiket Yardımcısı. Bunun yapılması, hataya neden giriş neredeyse herhangi bir doğrulama hata iletisi görüntüler.

> [!NOTE]
> Doğru JavaScript olan bir görünümü olmalıdır ve [jQuery](https://jquery.com/) başvuruları yerde istemci tarafı doğrulama komut dosyası. Bkz: [Model doğrulama](../models/validation.md) daha fazla bilgi için.

MVC (örneğin, sahip olduğunuz özel sunucu tarafı doğrulama veya istemci tarafı doğrulama devre dışı) bir sunucu tarafı doğrulama hatası oluştuğunda, hata iletisi gövdesi olarak yerleştirir. `<span>` öğesi.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Doğrulama Özeti etiketi Yardımcısı

* Hedefleri `<div>` öğelerle `asp-validation-summary` özniteliği

* HTML Yardımcısı alternatif: `@Html.ValidationSummary`

`Validation Summary Tag Helper` Doğrulama iletilerinin bir özetini görüntülemek için kullanılır. `asp-validation-summary` Öznitelik değeri aşağıdakilerden biri olabilir:

|ASP doğrulama özeti|Görüntülenen doğrulama iletileri|
|--- |--- |
|ValidationSummary.All|Özellik ve model düzeyi|
|ValidationSummary.ModelOnly|Model|
|ValidationSummary.None|Yok.|

### <a name="sample"></a>Örnek

Aşağıdaki örnekte, veri modeli ile donatılmış `DataAnnotation` üzerinde doğrulama hatası iletilerinin oluşturan öznitelikleri `<input>` öğesi.  Doğrulama etiketi Yardımcısı, bir doğrulama hatası oluştuğunda, hata iletisi görüntüler:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Oluşturulan HTML (model geçerli olduğunda):

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
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

## <a name="the-select-tag-helper"></a>Seçim etiketi Yardımcısı

* Oluşturur [seçin](https://www.w3.org/wiki/HTML/Elements/select) ve ilişkili [seçeneği](https://www.w3.org/wiki/HTML/Elements/option) modelinizin özellikleri için öğeleri.

* HTML Yardımcısı alternatif olan `Html.DropDownListFor` ve `Html.ListBoxFor`

`Select Tag Helper` `asp-for` Model özellik adı belirtir [seçin](https://www.w3.org/wiki/HTML/Elements/select) öğesi ve `asp-items` belirtir [seçeneği](https://www.w3.org/wiki/HTML/Elements/option) öğeleri.  Örneğin:

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` Yöntemi başlatır `CountryViewModel`, seçilen ülke ayarlar ve buna ileten `Index` görünümü.

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

HTTP POST `Index` yöntemi seçimi görüntüler:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` Görüntüle:

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Hangi ("CA" Seçili ile) aşağıdaki HTML'yi oluşturur:

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
> Kullanımı önerilmemektedir `ViewBag` veya `ViewData` seçin etiketi Yardımcısı ile. MVC meta verileri sağlayarak, daha güçlü ve genellikle daha az sorunlu bir görünüm modeli.

`asp-for` Öznitelik değeri özel bir durumdur ve gerektirmeyen bir `Model` önek, bir etiket Yardımcısı öznitelikleri yapın (gibi `asp-items`)

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Enum bağlama

Genellikle kullanmak uygun olan `<select>` ile bir `enum` özelliği ve `SelectListItem` öğelerden `enum` değerleri.

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` Yöntemi oluşturur bir `SelectList` nesne için bir sabit listesi.

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Numaralandırıcı listenizi donatmak `Display` daha zengin bir kullanıcı Arabirimi almak için öznitelik:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Aşağıdaki HTML'yi oluşturulur:

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

HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) öğe bir veya daha fazla görünüm modeli içerdiğinde, oluşturulan `SelectListGroup` nesneleri.

`CountryViewModelGroup` Grupları `SelectListItem` "Kuzey Amerika" ve "Avrupa" gruplara öğeleri:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

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

Seçin etiketi Yardımcısı otomatik olarak oluşturacak [birden çok "birden çok" =](http://w3c.github.io/html-reference/select.html) özelliği belirtilen özniteliği `asp-for` özniteliği bir `IEnumerable`. Örneğin, şu model verilen:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Aşağıdaki görünüm ile:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Aşağıdaki HTML'yi oluşturur:

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

### <a name="no-selection"></a>Seçim yok

Birden çok sayfada "belirtilmemiş" seçeneğini kullanarak kendiniz bulursanız, HTML yinelenen ortadan kaldırmak için bir şablon oluşturabilirsiniz:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml* şablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

HTML ekleme [ \<seçeneği >](https://www.w3.org/wiki/HTML/Elements/option) öğeleri sınırlı değildir *seçim* çalışması. Örneğin, aşağıdaki görünüm ve eylem yöntemi HTML yukarıdaki koda benzer oluşturur:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Doğru `<option>` öğe seçilir (içeren `selected="selected"` özniteliği) geçerli bağlı olarak `Country` değeri.

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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/intro>
* [HTML Form öğesi](https://www.w3.org/TR/html401/interact/forms.html)
* [İstek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [IAttributeAdapter arabirimi](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [Bu belge için kod parçacıkları](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
