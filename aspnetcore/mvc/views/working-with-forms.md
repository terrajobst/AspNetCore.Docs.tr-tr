---
title: ASP.NET Core formlardaki etiket yardımcıları
author: rick-anderson
description: Formlarla kullanılan yerleşik etiket yardımcılarını açıklar.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: 5af532db35b858d157f61a6aca30f55d15e9ff1e
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2020
ms.locfileid: "79416242"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a>ASP.NET Core formlardaki etiket yardımcıları

By [Rick Anderson](https://twitter.com/RickAndMSFT), [N. Taylor Mullen](https://github.com/NTaylorMullen), [Davve Patıı](https://twitter.com/Dave_Paquette)ve [Jerrie Pelser](https://github.com/jerriep)

Bu belge, formlarda ve genellikle form üzerinde kullanılan HTML öğeleriyle çalışmayı gösterir. HTML [form](https://www.w3.org/TR/html401/interact/forms.html) öğesi, Web uygulamalarının sunucuya veri geri göndermek için kullanacağı birincil mekanizmayı sağlar. Bu belgenin çoğunda [Etiket Yardımcıları](tag-helpers/intro.md) ve BUNLARıN güçlü HTML formları oluşturma konusunda nasıl yardımcı olabilecekleri açıklanmaktadır. Bu belgeyi okuyabilmeniz [Için yardımcıları etiketleyerek](tag-helpers/intro.md) okumanız önerilir.

Birçok durumda, HTML Yardımcıları belirli bir etiket Yardımcısı için alternatif bir yaklaşım sağlar, ancak bu etiket yardımcıların HTML yardımcılarını değiştirmez ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir. Bir HTML Yardımcısı alternatifi varsa, bu, bahsedilir.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Form etiketi Yardımcısı

[Form](https://www.w3.org/TR/html401/interact/forms.html) etiketi Yardımcısı:

* MVC denetleyicisi eylemi veya adlandırılmış yol için HTML [\<FORM >](https://www.w3.org/TR/html401/interact/forms.html) `action` öznitelik değeri oluşturur

* Siteler arası istek yasaklamasını engellemek için gizli bir [Istek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) ÜRETIR (http post eylem yönteminde `[ValidateAntiForgeryToken]` özniteliğiyle kullanıldığında)

* Yol değerlerine `<Parameter Name>` eklendiği `asp-route-<Parameter Name>` özniteliğini sağlar. `Html.BeginForm` ve `Html.BeginRouteForm` `routeValues` parametreleri benzer işlevlere sahiptir.

* Bir HTML Yardımcısı alternatifi `Html.BeginForm` ve `Html.BeginRouteForm`

Örnek:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Yukarıdaki form etiketi Yardımcısı aşağıdaki HTML 'yi oluşturur:

```html
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

MVC çalışma zamanı, etiket Yardımcısı öznitelikleri `asp-controller` ve `asp-action`olan `action` öznitelik değerini oluşturur. Form etiketi Yardımcısı ayrıca, siteler arası istek sahteciliği (HTTP POST eylem yönteminde `[ValidateAntiForgeryToken]` özniteliğiyle kullanıldığında) engellemek için gizli bir [Istek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) oluşturur. Bir saf HTML formunun siteler arası istek sahteciliğini önleme 'den korunması zordur, form etiketi Yardımcısı bu hizmeti sizin için sağlar.

### <a name="using-a-named-route"></a>Adlandırılmış yol kullanma

`asp-route` Tag Helper özniteliği, HTML `action` özniteliği için de biçimlendirme oluşturabilir. `register` [adlı bir](../../fundamentals/routing.md) uygulama, kayıt sayfası için aşağıdaki biçimlendirmeyi kullanabilir:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

*Görünümler/hesap* klasöründeki görünümlerin birçoğu ( *bireysel kullanıcı hesaplarıyla*yeni bir Web uygulaması oluşturduğunuzda oluşturulur), [ASP-Route-ReturnUrl](xref:mvc/views/working-with-forms) özniteliğini içerir:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Yerleşik şablonlarla `returnUrl`, yalnızca yetkili bir kaynağa erişmeye çalıştığınızda ancak kimliği doğrulanmamış veya yetkilendirilmeyen otomatik olarak doldurulur. Yetkisiz erişim yapmaya çalıştığınızda, güvenlik ara yazılımı sizi `returnUrl` kümesi ile oturum açma sayfasına yönlendirir.

## <a name="the-form-action-tag-helper"></a>Form eylemi etiketi Yardımcısı

Form eylemi etiketi Yardımcısı, `formaction` özniteliği oluşturulan `<button ...>` veya `<input type="image" ...>` etiketi üzerinde oluşturur. `formaction` özniteliği bir formun verilerini nereden gönderdiğini denetler. `image` ve [\<düğme >](https://www.w3.org/wiki/HTML/Elements/button) öğeleri [\<giriş >](https://www.w3.org/wiki/HTML/Elements/input) öğelerine bağlanır. Form eylemi etiketi Yardımcısı, karşılık gelen öğe için `formaction` bağlantısının oluşturulduğunu denetlemek için birkaç [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` özniteliği kullanımını sağlar.

`formaction`değerini denetlemek için desteklenen [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) öznitelikleri:

|Öznitelik|Açıklama|
|---|---|
|[ASP-Controller](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-controller)|Denetleyicinin adı.|
|[ASP-eylem](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-action)|Eylem yönteminin adı.|
|[ASP-alanı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-area)|Alanın adı.|
|[asp-sayfa](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page)|Razor sayfasının adı.|
|[ASP-Page-Handler](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page-handler)|Razor sayfası işleyicisinin adı.|
|[ASP-Route](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route)|Rotanın adı.|
|[ASP-Route-{Value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route-value)|Tek bir URL yol değeri. Örneğin, `asp-route-id="1234"`.|
|[ASP-All-Route-Data](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-all-route-data)|Tüm rota değerleri.|
|[ASP-Fragment](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-fragment)|URL parçası.|

### <a name="submit-to-controller-example"></a>Denetleyiciye gönder örneği

Aşağıdaki biçimlendirme, giriş veya düğme seçildiğinde formu `HomeController` `Index` eylemine gönderir:

```cshtml
<form method="post">
    <button asp-controller="Home" asp-action="Index">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-controller="Home" 
                                asp-action="Index">
</form>
```

Önceki biçimlendirme, aşağıdaki HTML 'yi oluşturur:

```html
<form method="post">
    <button formaction="/Home">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home">
</form>
```

### <a name="submit-to-page-example"></a>Sayfa örneğine gönder

Aşağıdaki biçimlendirme formu `About` Razor sayfasına gönderir:

```cshtml
<form method="post">
    <button asp-page="About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-page="About">
</form>
```

Önceki biçimlendirme, aşağıdaki HTML 'yi oluşturur:

```html
<form method="post">
    <button formaction="/About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/About">
</form>
```

### <a name="submit-to-route-example"></a>Yönlendirme örneğine gönder

`/Home/Test` uç noktasını göz önünde bulundurun:

```csharp
public class HomeController : Controller
{
    [Route("/Home/Test", Name = "Custom")]
    public string Test()
    {
        return "This is the test page";
    }
}
```

Aşağıdaki biçimlendirme formu `/Home/Test` uç noktasına gönderir.

```cshtml
<form method="post">
    <button asp-route="Custom">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-route="Custom">
</form>
```

Önceki biçimlendirme, aşağıdaki HTML 'yi oluşturur:

```html
<form method="post">
    <button formaction="/Home/Test">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home/Test">
</form>
```

## <a name="the-input-tag-helper"></a>Giriş etiketi Yardımcısı

Giriş etiketi Yardımcısı, bir HTML [\<girişi >](https://www.w3.org/wiki/HTML/Elements/input) öğesini Razor görünüminizdeki bir model ifadesine bağlar.

Söz dizimi:

```cshtml
<input asp-for="<Expression Name>">
```

Giriş etiketi Yardımcısı:

* `asp-for` özniteliğinde belirtilen ifade adı için `id` ve `name` HTML özniteliklerini üretir. `asp-for="Property1.Property2"` `m => m.Property1.Property2`eşdeğerdir. İfadenin adı, `asp-for` özniteliği değeri için kullanılan şeydir. Ek bilgi için [ifade adları](#expression-names) bölümüne bakın.

* Model özelliğine uygulanan model türüne ve [veri ek açıklaması](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) ÖZNITELIKLERINE göre HTML `type` öznitelik değerini ayarlar

* Belirtilirse, HTML `type` öznitelik değerinin üzerine yazmaz

* Model özelliklerine uygulanan [veri ek açıklama](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) özniteliklerinden [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) doğrulama öznitelikleri oluşturur

* `Html.TextBoxFor` ve `Html.EditorFor`bir HTML Yardımcısı özelliği örtüşüyor. Ayrıntılar için bkz. **giriş etiketi Yardımcısı Için HTML Yardımcısı alternatifleri** .

* Güçlü yazma sağlar. Özelliğin adı değişirse ve etiket yardımcısını güncelleştirmezseniz aşağıdakine benzer bir hata alırsınız:

```
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` Tag Yardımcısı, HTML `type` özniteliğini .NET türüne göre ayarlar. Aşağıdaki tabloda bazı ortak .NET türleri ve oluşturulan HTML türü listelenmekte (her .NET türü listelenmemiştir).

|.NET türü|Giriş türü|
|---|---|
|Bool|Type = "onay kutusu"|
|Dize|Type = "metin"|
|DateTime|Type =["TarihSaat-yerel"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)|
|Bayt|Type = "Number"|
|int|Type = "Number"|
|Tek, Çift|Type = "Number"|

Aşağıdaki tabloda, giriş etiketi Yardımcısı 'nın belirli giriş türleriyle eşleşecağı bazı ortak [veri ek açıklamaları](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) (her doğrulama özniteliği listelenmez) gösterilmektedir:

|Öznitelik|Giriş türü|
|---|---|
|EmailAddress|Type = "e-posta"|
|'Deki|Type = "URL"|
|[Hiddenınput]|Type = "Hidden"|
|Numarası|Type = "tel"|
|[DataType (DataType. Password)]|Type = "Password"|
|[DataType (DataType. Date)]|Type = "Date"|
|[DataType (DataType. Time)]|yazın = "Time"|

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Yukarıdaki kod, aşağıdaki HTML 'yi oluşturur:

```html
  <form method="post" action="/Demo/RegisterInput">
      Email:
      <input type="email" data-val="true"
             data-val-email="The Email Address field is not a valid email address."
             data-val-required="The Email Address field is required."
             id="Email" name="Email" value=""><br>
      Password:
      <input type="password" data-val="true"
             data-val-required="The Password field is required."
             id="Password" name="Password"><br>
      <button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
   </form>
```

`Email` ve `Password` özelliklerine uygulanan veri ek açıklamaları modelde meta veriler oluşturur. Giriş etiketi Yardımcısı, model meta verilerini kullanır ve [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` öznitelikleri üretir (bkz. [model doğrulama](../models/validation.md)). Bu öznitelikler, giriş alanlarına iliştirilecek Doğrulayıcıları anlatmaktadır. Bu unobtrusive HTML5 ve [jQuery](https://jquery.com/) doğrulaması sağlar. Unobtrusive özniteliklerinin biçimi `data-val-rule="Error Message"`, burada kural doğrulama kuralının adıdır (örneğin, `data-val-required`, `data-val-email`, `data-val-maxlength`vb.) Öznitelikte bir hata iletisi sağlanırsa, `data-val-rule` özniteliği için değer olarak görüntülenir. Ayrıca, kuralla ilgili ek ayrıntılar sağlayan `data-val-ruleName-argumentName="argumentValue"` form öznitelikleri de vardır, örneğin, `data-val-maxlength-max="1024"`.

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Giriş etiketi Yardımcısı için HTML Yardımcısı alternatifleri

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` ve `Html.EditorFor`, giriş etiketi Yardımcısı ile çakışan özelliklere sahiptir. Giriş etiketi Yardımcısı `type` özniteliğini otomatik olarak ayarlar; `Html.TextBox` ve `Html.TextBoxFor`. `Html.Editor` ve `Html.EditorFor` tanıtıcı koleksiyonları, karmaşık nesneler ve şablonlar; Giriş etiketi Yardımcısı yok. Giriş etiketi Yardımcısı, `Html.EditorFor` ve `Html.TextBoxFor` kesin türdedir (lambda ifadeleri kullanırlar); `Html.TextBox` ve `Html.Editor` değildir (ifade adlarını kullanırlar).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()` ve `@Html.EditorFor()`, varsayılan şablonlarını yürütürken `htmlAttributes` adlı özel bir `ViewDataDictionary` girişi kullanır. Bu davranış, isteğe bağlı olarak `additionalViewData` parametreleri kullanılarak genişletilebilir. "HtmlAttributes" anahtarı büyük/küçük harfe duyarlıdır. "HtmlAttributes" anahtarı, `@Html.TextBox()`gibi giriş yardımcılarını geçirilmiş `htmlAttributes` nesnesine benzer şekilde işlenir.

```cshtml
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>İfade adları

`asp-for` öznitelik değeri, bir lambda ifadesinin bir `ModelExpression` ve sağ tarafıdır. Bu nedenle, `asp-for="Property1"` oluşturulan kodda `m => m.Property1` hale gelir ve bu nedenle `Model`ile önek gerektirmez. "\@" karakterini kullanarak bir satır içi ifadeyi başlatabilir ve `m.`önce taşıyabilirsiniz:

```cshtml
@{
  var joe = "Joe";
}

<input asp-for="@joe">
```

Şunları üretir:

```html
<input type="text" id="joe" name="joe" value="Joe">
```

Koleksiyon özellikleriyle `asp-for="CollectionProperty[23].Member"`, `i` değer `23`olduğunda `asp-for="CollectionProperty[i].Member"` aynı adı üretir.

ASP.NET Core MVC `ModelExpression`değerini hesapladığında `ModelState`dahil olmak üzere çeşitli kaynakları inceler. `<input type="text" asp-for="@Name">`göz önünde bulundurun. Hesaplanan `value` özniteliği, öğesinden gelen ilk null olmayan değerdir:

* "Name" anahtarına sahip giriş `ModelState`.
* İfadenin sonucu `Model.Name`.

### <a name="navigating-child-properties"></a>Alt özelliklerde gezinme

Ayrıca, görünüm modelinin özellik yolunu kullanarak alt Özellikler ' e gidebilirsiniz. Alt `Address` özelliği içeren daha karmaşık bir model sınıfı düşünün.

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

Görünümde `Address.AddressLine1`bağlandık:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

`Address.AddressLine1`için aşağıdaki HTML oluşturulmuştur:

```html
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="">
```

### <a name="expression-names-and-collections"></a>İfade adları ve koleksiyonlar

Örnek, bir dizi `Colors`içeren bir modeldir:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Eylem yöntemi:

```csharp
public IActionResult Edit(int id, int colorIndex)
{
    ViewData["Index"] = colorIndex;
    return View(GetPerson(id));
}
```

Aşağıdaki Razor, belirli bir `Color` öğesine nasıl erişistediğinizi göstermektedir:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String. cshtml* şablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

`List<T>`kullanarak örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Aşağıdaki Razor, bir koleksiyonun üzerinde nasıl yineleme yapılacağını göstermektedir:

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/TodoItem. cshtml* şablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

değer bir `asp-for` veya `Html.DisplayFor` denk bir bağlamda kullanılacaksa, mümkünse `foreach` kullanılmalıdır. Genel olarak, `for` bir Numaralandırıcı ayırması gerekmiyorsa, `foreach` daha iyidir (senaryo buna izin veriyorsa). Ancak, bir LINQ ifadesinde bir dizin oluşturucuyu değerlendirmek pahalı olabilir ve simge durumuna küçültülmüş olmalıdır.

&nbsp;

>[!NOTE]
>Yukarıdaki açıklamalı örnek kod, listedeki her bir `ToDoItem` erişmek için lambda ifadesinin `@` işleçle nasıl değiştirileceğini gösterir.

## <a name="the-textarea-tag-helper"></a>TextArea etiketi Yardımcısı

`Textarea Tag Helper` Tag Yardımcısı giriş etiketi Yardımcısı ile benzerdir.

* `id` ve `name` özniteliklerini ve [\<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) öğesi için modelden veri doğrulama özniteliklerini üretir.

* Güçlü yazma sağlar.

* HTML Yardımcısı alternatifi: `Html.TextAreaFor`

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Aşağıdaki HTML oluşturulur:

```html
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

## <a name="the-label-tag-helper"></a>Etiket etiketi Yardımcısı

* Bir ifade adı için bir [\<label >](https://www.w3.org/wiki/HTML/Elements/label) öğesinde etiket başlık yazısı ve `for` özniteliği oluşturur

* HTML Yardımcısı alternatifi: `Html.LabelFor`.

`Label Tag Helper`, saf HTML etiket öğesi üzerinde aşağıdaki avantajları sağlar:

* `Display` özniteliğinden açıklayıcı etiket değerini otomatik olarak alırsınız. İstenen görünen ad zaman içinde değişebilir ve `Display` özniteliği ve etiket etiketi Yardımcısı 'nın birleşimi, `Display` her yere uygular.

* Kaynak kodunda daha az biçimlendirme

* Model özelliğiyle güçlü yazma.

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

`<label>` öğesi için aşağıdaki HTML oluşturulur:

```html
<label for="Email">Email Address</label>
```

Etiket etiketi Yardımcısı, `<input>` öğesiyle ilişkili KIMLIK olan "e-posta" `for` öznitelik değerini oluşturdu. Etiket Yardımcıları, doğru ilişkilendirilebilen şekilde tutarlı `id` ve `for` öğeleri oluşturur. Bu örnekteki başlık `Display` özniteliğinden gelir. Modelde bir `Display` özniteliği yoksa, başlık ifadenin Özellik adı olacaktır.

## <a name="the-validation-tag-helpers"></a>Doğrulama etiketi yardımcıları

İki doğrulama etiketi yardımcıları vardır. `Validation Message Tag Helper` (modelinizdeki tek bir özellik için bir doğrulama iletisi görüntüler) ve `Validation Summary Tag Helper` (doğrulama hatalarının özetini görüntüler). `Input Tag Helper`, model sınıflarınızda bulunan veri ek açıklaması özniteliklerini temel alan giriş öğelerine HTML5 istemci tarafı doğrulama öznitelikleri ekler. Doğrulama de sunucuda gerçekleştirilir. Doğrulama etiketi Yardımcısı, bir doğrulama hatası oluştuğunda bu hata iletilerini görüntüler.

### <a name="the-validation-message-tag-helper"></a>Doğrulama Iletisi etiketi Yardımcısı

* Belirtilen model özelliğinin giriş alanındaki doğrulama hatası mesajlarını bağlayan [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) öğesine [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)`data-valmsg-for="property"` özniteliğini ekler. İstemci tarafı doğrulama hatası oluştuğunda, [jQuery](https://jquery.com/) `<span>` öğesinde hata iletisini görüntüler.

* Doğrulama de sunucuda gerçekleşir. İstemciler JavaScript devre dışı bırakılmış olabilir ve bazı doğrulamalar yalnızca sunucu tarafında yapılabilir.

* HTML Yardımcısı alternatifi: `Html.ValidationMessageFor`

`Validation Message Tag Helper`, bir HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) öğesinde `asp-validation-for` özniteliğiyle kullanılır.

```cshtml
<span asp-validation-for="Email"></span>
```

Doğrulama Iletisi etiketi Yardımcısı aşağıdaki HTML 'yi oluşturur:

```html
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Aynı özellik için bir `Input` etiketi Yardımcısı sonrasında `Validation Message Tag Helper` genellikle kullanırsınız. Bunun yapılması, hataya neden olan girişin yakınında herhangi bir doğrulama hata iletisi görüntüler.

> [!NOTE]
> İstemci tarafı doğrulaması için doğru JavaScript ve [jQuery](https://jquery.com/) betik başvurularını içeren bir görünümsiniz olmalıdır. Daha fazla bilgi için bkz. [model doğrulaması](../models/validation.md) .

Sunucu tarafı doğrulama hatası oluştuğunda (örneğin, özel sunucu tarafı doğrulamadan veya istemci tarafı doğrulaması devre dışı bırakılmışsa), MVC bu hata iletisini `<span>` öğesinin gövdesi olarak koyar.

```html
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Doğrulama Özeti etiketi Yardımcısı

* `asp-validation-summary` özniteliği olan öğeleri `<div>` hedefleri

* HTML Yardımcısı alternatifi: `@Html.ValidationSummary`

`Validation Summary Tag Helper`, doğrulama iletilerinin özetini göstermek için kullanılır. `asp-validation-summary` öznitelik değeri, aşağıdakilerden herhangi biri olabilir:

|ASP-doğrulama-Özet|Görünen doğrulama iletileri|
|--- |--- |
|ValidationSummary. All|Özellik ve model düzeyi|
|Yalnızca ValidationSummary. model|Model|
|ValidationSummary. None|Yok|

### <a name="sample"></a>Örnek

Aşağıdaki örnekte, veri modelinde `<input>` öğesinde doğrulama hata iletileri üreten `DataAnnotation` öznitelikleri vardır.  Doğrulama hatası oluştuğunda, doğrulama etiketi Yardımcısı şu hata iletisini görüntüler:

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Oluşturulan HTML (model geçerli olduğunda):

```html
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

## <a name="the-select-tag-helper"></a>Etiket Seç Yardımcısı

* Modelinizin özellikleri için [Select](https://www.w3.org/wiki/HTML/Elements/select) ve ilişkili [seçenek](https://www.w3.org/wiki/HTML/Elements/option) öğeleri oluşturur.

* Bir HTML Yardımcısı alternatifi `Html.DropDownListFor` ve `Html.ListBoxFor`

`Select Tag Helper` `asp-for`, [Select](https://www.w3.org/wiki/HTML/Elements/select) öğesi için model özelliği adını belirtir ve `asp-items` [seçenek](https://www.w3.org/wiki/HTML/Elements/option) öğelerini belirtir.  Örnek:

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` yöntemi `CountryViewModel`başlatır, seçilen ülkeyi ayarlar ve `Index` görünümüne geçirir.

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

HTTP POST `Index` yöntemi seçimi görüntüler:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` görünümü:

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Aşağıdaki HTML 'yi üreten ("CA" seçiliyken):

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
   </form>
```

> [!NOTE]
> Etiket Seç Yardımcısı ile `ViewBag` veya `ViewData` kullanmanızı önermiyoruz. Bir görünüm modeli, MVC meta verileri sağlamaya ve genellikle daha az soruna neden olacak daha sağlamdır.

`asp-for` öznitelik değeri özel bir durumdur ve `Model` öneki gerektirmez, diğer etiket Yardımcısı öznitelikleri olur (`asp-items`gibi)

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Sabit Listesi bağlama

`<select>`, `enum` bir özellik ile kullanmak ve `enum` değerlerinden `SelectListItem` öğeleri oluşturmak için kullanışlıdır.

Örnek:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` yöntemi bir numaralandırma için `SelectList` nesnesi oluşturur.

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Daha zengin bir kullanıcı arabirimi almak için, Numaralandırıcı listenizi `Display` özniteliğiyle işaretleyebilirsiniz:

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Aşağıdaki HTML oluşturulur:

```html
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
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
    </form>
```

### <a name="option-group"></a>Seçenek grubu

HTML [\<SeçenekGrubu >](https://www.w3.org/wiki/HTML/Elements/optgroup) öğesi, görünüm modelinde bir veya daha fazla `SelectListGroup` nesnesi içerdiğinde oluşturulur.

`CountryViewModelGroup`, `SelectListItem` öğelerini "Kuzey Amerika" ve "Avrupa" gruplarına gruplandırır:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

İki grup aşağıda gösterilmiştir:

![seçenek grubu örneği](working-with-forms/_static/grp.png)

Oluşturulan HTML:

```html
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
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
 </form>
```

### <a name="multiple-select"></a>Çoklu seçim

`asp-for` özniteliğinde belirtilen özellik bir `IEnumerable`ise, select etiketi Yardımcısı otomatik olarak [birden çok = "Multiple"](https://w3c.github.io/html-reference/select.html) özniteliği oluşturacaktır. Örneğin, aşağıdaki model verildiğinde:

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Aşağıdaki görünümle:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Aşağıdaki HTML 'yi oluşturur:

```html
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
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
</form>
```

### <a name="no-selection"></a>Seçim yok

Birden çok sayfada "belirtilmemiş" seçeneğini kullanarak kendinizi bulursanız, HTML 'yi yinelemeyi ortadan kaldırmak için bir şablon oluşturabilirsiniz:

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel. cshtml* şablonu:

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

HTML [\<seçenek >](https://www.w3.org/wiki/HTML/Elements/option) öğeleri ekleme *hiçbir seçim* durumuyla sınırlı değildir. Örneğin, aşağıdaki görünüm ve eylem yöntemi yukarıdaki koda benzer HTML oluşturur:

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?name=snippetNone)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Geçerli `Country` değerine bağlı olarak doğru `<option>` öğesi seçilecek (`selected="selected"` özniteliğini içerir).

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

```html
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>">
 </form>
 ```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/intro>
* [HTML form öğesi](https://www.w3.org/TR/html401/interact/forms.html)
* [İstek doğrulama belirteci](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [Iattributeadapter arabirimi](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [Bu belge için kod parçacıkları](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
