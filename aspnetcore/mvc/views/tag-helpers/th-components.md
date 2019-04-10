---
title: ASP.NET core'da etiket Yardımcısı bileşenleri
author: scottaddie
description: Etiket Yardımcısı bileşenler nelerdir ve ASP.NET Core nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: fdad4ae367245cd3beabaf90587c1fe5e9162afe
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468601"
---
# <a name="tag-helper-components-in-aspnet-core"></a>ASP.NET core'da etiket Yardımcısı bileşenleri

Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)

Bir etiket Yardımcısı koşullu olarak değiştirmek veya HTML öğelerinin sunucu tarafı koddan eklemek izin veren bir etiket Yardımcısı bileşenidir. Bu özellik, ASP.NET Core 2.0 veya sonraki sürümlerinde kullanılabilir.

ASP.NET Core, iki yerleşik etiket Yardımcısı bileşenleri içerir: `head` ve `body`. Bulunan <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> ad alanı ve MVC ve Razor sayfaları için kullanılabilir. Etiket Yardımcısı bileşenleri yoksa uygulamayı kayıt gerektiren *_viewımports.cshtml*.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="use-cases"></a>Uygulama alanları

Etiket Yardımcısı bileşenlerinin iki yaygın kullanım örnekleri şunlardır:

1. [Ekleme bir `<link>` içine `<head>`.](#inject-into-html-head-element)
1. [Ekleme bir `<script>` içine `<body>`.](#inject-into-html-body-element)

Bunlar aşağıdaki bölümlerde açıklanmaktadır durumlarda kullanın.

### <a name="inject-into-html-head-element"></a>HTML baş öğesinin içine ekleme

HTML içinde `<head>` öğesi, CSS dosyaları HTML ile sık alınan `<link>` öğesi. Aşağıdaki kod eklediği bir `<link>` öğesine `<head>` öğesini kullanarak `head` etiket Yardımcısı bileşeni:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

Yukarıdaki kodda:

* `AddressStyleTagHelperComponent` Implements <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>. Özet:
  * Başlatma ile sınıfının sağlayan bir <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.
  * Etiket Yardımcısı bileşenleri, ekleyin veya HTML öğeleri değiştirmek için kullanılmasını sağlar.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> Özelliği bileşenleri işlenir sırasını tanımlar. `Order` Etiket Yardımcısı bileşenlerin bir uygulamada birden fazla kullanımları olduğunda gereklidir.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> yürütme bağlamı'nın karşılaştırır <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> özellik değerini `head`. Karşılaştırma true olarak içeriğini değerlendirilirse `_style` alan HTML'e eklenen `<head>` öğesi.

### <a name="inject-into-html-body-element"></a>HTML gövde öğesi ekleme

`body` Etiket Yardımcısı bileşeni ekleme bir `<script>` öğesine `<body>` öğesi. Aşağıdaki kod, bu tekniği gösterir:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

Ayrı bir HTML dosyası depolamak için kullanılan `<script>` öğesi. HTML dosyası daha temiz ve daha sürdürülebilir kod yapar. Yukarıdaki kod içeriğini okur *TagHelpers/Templates/AddressToolTipScript.html* ve etiket Yardımcısı çıkışıyla ekler. *AddressToolTipScript.html* dosyası aşağıdaki biçimlendirme içerir:

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

Yukarıdaki kod bağlayan bir [Bootstrap araç ipucu pencere öğesi](https://getbootstrap.com/docs/3.3/javascript/#tooltips) herhangi `<address>` içeren öğesi bir `printable` özniteliği. Fare işaretçisi öğenin bekletildiğinde etkili görülebilir.

## <a name="register-a-component"></a>Bir bileşeni kayıt

Bir etiket Yardımcısı bileşeni uygulamanın etiket Yardımcısı bileşenleri koleksiyona eklenmesi gerekir. Koleksiyona eklemenin üç yolu vardır:

1. [Kapsayıcı Hizmetleri üzerinden kayıt](#registration-via-services-container)
1. [Razor dosya üzerinden kayıt](#registration-via-razor-file)
1. [Sayfa modeli veya denetleyicisi üzerinden kayıt](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a>Kapsayıcı Hizmetleri üzerinden kayıt

Etiket Yardımcısı bileşen sınıfı ile yönetilmiyorsa <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) sistem. Aşağıdaki `Startup.ConfigureServices` kod kayıtları `AddressStyleTagHelperComponent` ve `AddressScriptTagHelperComponent` ile sınıfları bir [geçici ömrü](xref:fundamentals/dependency-injection#lifetime-and-registration-options):

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a>Razor dosya üzerinden kayıt

Etiket Yardımcısı bileşen DI, kayıtlı değilse bir Razor sayfaları sayfası ya da bir MVC görünümü kaydedilebilir. Bu teknik, eklenen biçimlendirme ve Razor dosyasından bileşen yürütme sırasını denetlemek için kullanılır.

`ITagHelperComponentManager` Etiket Yardımcısı bileşenleri ekleyip bunları uygulamadan kaldırmak için kullanılır. Aşağıdaki kod bu teknikte gösterir `AddressTagHelperComponent`:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

Yukarıdaki kodda:

* `@inject` Yönergenin sağladığı örneğini `ITagHelperComponentManager`. Örnek adlı bir değişkene atanır `manager` aşağı yönde Razor dosyasına erişim için.
* Örneği `AddressTagHelperComponent` uygulamanın etiket Yardımcısı bileşenleri koleksiyona eklenir.

`AddressTagHelperComponent` kabul eden bir oluşturucu içerecek şekilde değişiklik `markup` ve `order` parametreleri:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

Sağlanan `markup` parametresi kullanılır `ProcessAsync` gibi:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a>Sayfa modeli veya denetleyicisi üzerinden kayıt

Etiket Yardımcısı bileşen DI, kayıtlı değilse, Razor sayfaları sayfa modeli veya bir MVC denetleyicisi kaydedilebilir. Bu teknik, C# Razor dosyaları mantığından ayırmak için kullanışlıdır.

Oluşturucu ekleme örneği erişmek için kullanılan `ITagHelperComponentManager`. Etiket Yardımcısı bileşen örneğinin etiket Yardımcısı bileşenleri koleksiyona eklenir. Bu teknikte aşağıdaki Razor sayfaları sayfa modeli gösterir `AddressTagHelperComponent`:

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

Yukarıdaki kodda:

* Oluşturucu ekleme örneği erişmek için kullanılan `ITagHelperComponentManager`.
* Örneği `AddressTagHelperComponent` uygulamanın etiket Yardımcısı bileşenleri koleksiyona eklenir.

## <a name="create-a-component"></a>Bir bileşen oluşturun

Bir özel etiket Yardımcısı bileşeni oluşturmak için:

* Türetilen bir ortak sınıf oluşturun <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.
* Geçerli bir [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) öznitelik sınıfı. Hedef HTML öğesinin adını belirtin.
* *İsteğe bağlı*: Geçerli bir [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) türün görüntülenmesine IntelliSense içinde Sınıf özniteliği.

Aşağıdaki kod bir özel etiket Yardımcısı bileşen hedefleyen oluşturur `<address>` HTML öğesi:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

Özel kullanmanın `address` gibi HTML biçimlendirmesi eklemesine etiket Yardımcısı bileşeni:

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open("
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

Önceki `ProcessAsync` yöntemi için sağlanan HTML eklediği <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> eşleşen içine `<address>` öğesi. Ekleme gerçekleşir olduğunda:

* Yürütme bağlamı'nın `TagName` özellik değeri eşittir `address`.
* Buna karşılık gelen `<address>` öğeye sahip bir `printable` özniteliği.

Örneğin, `if` ifadesi, aşağıdaki işleme sırasında true olarak değerlendirilir `<address>` öğesi:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
