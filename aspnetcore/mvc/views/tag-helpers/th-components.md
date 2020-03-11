---
title: ASP.NET Core içindeki etiket Yardımcısı bileşenleri
author: scottaddie
description: Etiket Yardımcısı bileşenlerinin ne olduğunu ve ASP.NET Core nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 06/12/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: 5e2eb2d4322068c5864fbe49acaa6d0859bd319a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660771"
---
# <a name="tag-helper-components-in-aspnet-core"></a>ASP.NET Core içindeki etiket Yardımcısı bileşenleri

[Scott Ade](https://twitter.com/Scott_Addie) ve [fiyaz bin hasan](https://github.com/fiyazbinhasan) tarafından

Etiket Yardımcısı bileşeni, sunucu tarafı kodundan HTML öğelerini koşullu olarak değiştirmenize veya eklemenize olanak sağlayan bir etiket yardımcıdır. Bu özellik ASP.NET Core 2,0 veya üzeri sürümlerde kullanılabilir.

ASP.NET Core iki yerleşik etiket Yardımcısı bileşeni içerir: `head` ve `body`. <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> ad alanında konumlanır ve hem MVC hem de Razor Pages kullanılabilir. Etiket Yardımcısı bileşenleri *_ViewImports. cshtml*'de uygulamayla kayıt gerektirmez.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="use-cases"></a>Uygulama alanları

Etiket Yardımcısı bileşenlerinin iki yaygın kullanım durumu şunlardır:

1. [`<head>`bir `<link>` ekleme.](#inject-into-html-head-element)
1. [`<body>`bir `<script>` ekleme.](#inject-into-html-body-element)

Aşağıdaki bölümlerde bu kullanım durumları açıklanır.

### <a name="inject-into-html-head-element"></a>HTML Head öğesine Ekle

HTML `<head>` öğesinin içinde, CSS dosyaları genellikle HTML `<link>` öğesiyle içeri aktarılır. Aşağıdaki kod `head` etiketi Yardımcısı bileşenini kullanarak bir `<link>` öğesini `<head>` öğesine çıkartır:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

Önceki kodda:

* `AddressStyleTagHelperComponent` <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>uygular. Soyutlama:
  * <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>ile sınıfın başlatılmasına izin verir.
  * HTML öğeleri eklemek veya değiştirmek için etiket Yardımcısı bileşenlerinin kullanılmasını sağlar.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> özelliği, bileşenlerin işlendiği sırayı tanımlar. bir uygulamada birden çok etiket Yardımcısı bileşeni kullanımı olduğunda `Order` gereklidir.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*>, yürütme bağlamının <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> özellik değerini `head`karşılaştırır. Karşılaştırma true olarak değerlendirilirse, `_style` alanının içeriği HTML `<head>` öğesine eklenir.

### <a name="inject-into-html-body-element"></a>HTML Body öğesine Ekle

`body` Tag yardımcı bileşeni `<body>` öğesine bir `<script>` öğesi ekleyebilir. Aşağıdaki kod bu tekniği göstermektedir:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

`<script>` öğesini depolamak için ayrı bir HTML dosyası kullanılır. HTML dosyası, kod temizleyici ve daha sürdürülebilir hale gelir. Önceki kod *TagHelpers/Templates/AddressToolTipScript.html* içeriğini okur ve etiket Yardımcısı çıktısına ekler. *Addresstooltipscript. html* dosyası aşağıdaki biçimlendirmeyi içerir:

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

Yukarıdaki kod, bir [önyükleme araç ipucu pencere öğesini](https://getbootstrap.com/docs/3.3/javascript/#tooltips) bir `printable` özniteliği içeren herhangi bir `<address>` öğesine bağlar. Bir fare işaretçisi öğenin üzerine geldiğinde efekt görünür.

## <a name="register-a-component"></a>Bir bileşeni kaydetme

Uygulamanın etiket Yardımcısı bileşenleri koleksiyonuna bir etiket Yardımcısı bileşeni eklenmelidir. Koleksiyona eklemenin üç yolu vardır:

* [Hizmetler kapsayıcısı aracılığıyla kayıt](#registration-via-services-container)
* [Razor dosyası aracılığıyla kayıt](#registration-via-razor-file)
* [Sayfa modeli veya denetleyici aracılığıyla kaydolma](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a>Hizmetler kapsayıcısı aracılığıyla kayıt

Etiket Yardımcısı bileşen sınıfı <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>ile yönetilmemişse, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) sistemine kaydedilmelidir. Aşağıdaki `Startup.ConfigureServices` kod, `AddressStyleTagHelperComponent` ve `AddressScriptTagHelperComponent` sınıflarını [geçici bir yaşam süresine](xref:fundamentals/dependency-injection#lifetime-and-registration-options)kaydeder:

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a>Razor dosyası aracılığıyla kayıt

Etiket Yardımcısı bileşeni, DI ile kayıtlı değilse, bir Razor Pages sayfasından veya bir MVC görünümünden kayıt olabilir. Bu teknik, eklenen biçimlendirmeyi ve bir Razor dosyasından bileşen yürütme sırasını denetlemek için kullanılır.

`ITagHelperComponentManager` etiket Yardımcısı bileşenleri eklemek veya uygulamadan kaldırmak için kullanılır. Aşağıdaki kod, `AddressTagHelperComponent`ile bu tekniği gösterir:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

Önceki kodda:

* `@inject` yönergesi `ITagHelperComponentManager`örneğini sağlar. Örnek, Razor dosyasındaki erişim yönündeki `manager` adlı bir değişkene atanır.
* Uygulamanın etiket Yardımcısı bileşenleri koleksiyonuna bir `AddressTagHelperComponent` örneği eklenir.

`AddressTagHelperComponent`, `markup` ve `order` parametrelerini kabul eden bir oluşturucuya uyacak şekilde değiştirilmiştir:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

Belirtilen `markup` parametresi `ProcessAsync` içinde aşağıdaki gibi kullanılır:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a>Sayfa modeli veya denetleyici aracılığıyla kaydolma

Etiket Yardımcısı bileşeni, DI ile kayıtlı değilse, bir Razor Pages sayfa modelinden veya bir MVC denetleyicisinden kaydedilebilir. Bu teknik, Razor dosyalarından mantık C# ayırmak için faydalıdır.

`ITagHelperComponentManager`örneğine erişmek için Oluşturucu ekleme kullanılır. Etiket Yardımcısı bileşeni, örneğin etiket Yardımcısı bileşenleri koleksiyonuna eklenir. Aşağıdaki Razor Pages sayfa modelinde bu teknik `AddressTagHelperComponent`gösterilmektedir:

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

Önceki kodda:

* `ITagHelperComponentManager`örneğine erişmek için Oluşturucu ekleme kullanılır.
* Uygulamanın etiket Yardımcısı bileşenleri koleksiyonuna bir `AddressTagHelperComponent` örneği eklenir.

## <a name="create-a-component"></a>Bileşen oluşturma

Özel bir etiket Yardımcısı bileşeni oluşturmak için:

* <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>türeten bir ortak sınıf oluşturun.
* Sınıfa bir [`[HtmlTargetElement]`](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) özniteliği uygulayın. Hedef HTML öğesinin adını belirtin.
* *Isteğe bağlı*: türün IntelliSense 'de görüntülenmesini engellemek için sınıfa bir [`[EditorBrowsable(EditorBrowsableState.Never)]`](xref:System.ComponentModel.EditorBrowsableAttribute) özniteliği uygulayın.

Aşağıdaki kod, `<address>` HTML öğesini hedefleyen özel bir etiket Yardımcısı bileşeni oluşturur:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

HTML işaretlemesini aşağıdaki gibi eklemek için özel `address` Tag yardımcı bileşenini kullanın:

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open(" +
        "'https://binged.it/2AXRRYw')\">" +
        "<span class='glyphicon glyphicon-road' aria-hidden='true'></span>" +
        "</button>";

    public override int Order => 3;

    public override async Task ProcessAsync(TagHelperContext context,
                                            TagHelperOutput output)
    {
        if (string.Equals(context.TagName, "address",
                StringComparison.OrdinalIgnoreCase) &&
            output.Attributes.ContainsName("printable"))
        {
            var content = await output.GetChildContentAsync();
            output.Content.SetHtmlContent(
                $"<div>{content.GetContent()}</div>{_printableButton}");
        }
    }
}
```

Yukarıdaki `ProcessAsync` yöntemi, eşleşen `<address>` öğesine <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> için belirtilen HTML 'yi çıkarır. Ekleme şu durumlarda oluşur:

* Yürütme bağlamının `TagName` Özellik değeri `address`eşittir.
* Karşılık gelen `<address>` öğesi `printable` bir özniteliğe sahip.

Örneğin, aşağıdaki `<address>` öğesi işlenirken `if` deyiminin doğru sonucu verilmiştir:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
