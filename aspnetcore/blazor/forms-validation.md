---
title: ASP.NET Core Blazor formları ve doğrulama
author: guardrex
description: Blazor'de formları ve alan doğrulama senaryolarını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/forms-validation
ms.openlocfilehash: a94a433f26e451bbadc73615e502e46d273f05c2
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828145"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a>ASP.NET Core Blazor formları ve doğrulama

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

Forms ve doğrulama, [veri ek açıklamaları](xref:mvc/models/validation)kullanılarak Blazor desteklenir.

Aşağıdaki `ExampleModel` türü, veri ek açıklamalarını kullanarak doğrulama mantığını tanımlar:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Bir form `EditForm` bileşeni kullanılarak tanımlanır. Aşağıdaki formda tipik öğeler, bileşenler ve Razor kodu gösterilmektedir:

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* Form, `ExampleModel` türünde tanımlanan doğrulamayı kullanarak `name` alanında Kullanıcı girişini doğrular. Model, bileşenin `@code` bloğunda oluşturulur ve özel bir alanda tutulur (`exampleModel`). Alan, `<EditForm>` öğesinin `Model` özniteliğine atanır.
* `DataAnnotationsValidator` bileşeni, veri açıklamalarını kullanarak doğrulama desteğini ekler.
* `ValidationSummary` bileşeni doğrulama iletilerini özetler.
* `HandleValidSubmit`, form başarıyla gönderdiğinde tetiklenir (doğrulamayı geçirir).

Kullanıcı girişini almak ve doğrulamak için yerleşik bir giriş bileşenleri kümesi vardır. Girişler değiştirildiklerinde ve bir form gönderildiğinde onaylanır. Kullanılabilir giriş bileşenleri aşağıdaki tabloda gösterilmiştir.

| Giriş bileşeni | &hellip; olarak işlendi       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

`EditForm`dahil olmak üzere tüm giriş bileşenleri, rastgele öznitelikleri destekler. Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik işlenmiş HTML öğesine eklenir.

Giriş bileşenleri, düzenleme sırasında doğrulamak ve CSS sınıfını alan durumunu yansıtacak şekilde değiştirmek için varsayılan davranışı sağlar. Bazı bileşenler, yararlı ayrıştırma mantığını içerir. Örneğin, `InputDate` ve `InputNumber`, onları doğrulama hatası olarak kaydederek düzeltilemez değerleri düzgün şekilde işleyebilir. Null değerleri kabul edebilecek türler, hedef alanın null değer alabilme durumunu da destekler (örneğin, `int?`).

Aşağıdaki `Starship` türü, önceki `ExampleModel`daha büyük bir özellik kümesi ve veri ek açıklamaları kullanarak doğrulama mantığını tanımlar:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

Yukarıdaki örnekte, hiçbir veri ek açıklaması mevcut olmadığından `Description` isteğe bağlıdır.

Aşağıdaki form, `Starship` modelinde tanımlanan doğrulamayı kullanarak Kullanıcı girişini doğrular:

```razor
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

`EditForm`, hangi alanların değiştirildiği ve geçerli doğrulama iletileri de dahil olmak üzere düzenleme işlemiyle ilgili meta verileri izleyen [basamaklı bir değer](xref:blazor/components#cascading-values-and-parameters) olarak `EditContext` oluşturur. `EditForm` ayrıca geçerli ve geçersiz gönderir (`OnValidSubmit`, `OnInvalidSubmit`) için uygun olaylar sağlar. Alternatif olarak, doğrulamayı tetiklemek ve alan değerlerini özel doğrulama kodu ile denetlemek için `OnSubmit` kullanın.

## <a name="inputtext-based-on-the-input-event"></a>Giriş olayına göre InputText

`change` olayı yerine `input` olayını kullanan özel bir bileşen oluşturmak için `InputText` bileşenini kullanın.

Aşağıdaki biçimlendirmeye sahip bir bileşen oluşturun ve bileşeni tıpkı `InputText` kullanıldığı gibi kullanın:

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Doğrulama desteği

`DataAnnotationsValidator` bileşeni, basamaklı `EditContext`veri açıklamalarını kullanarak doğrulama desteğini iliştirir. Veri ek açıklamalarını kullanarak doğrulama desteğinin etkinleştirilmesi bu açık hareketi gerektirir. Veri ek açıklamalarıyla farklı bir doğrulama sistemi kullanmak için `DataAnnotationsValidator` özel bir uygulamayla değiştirin. ASP.NET Core uygulama, başvuru kaynağında İnceleme için kullanılabilir: [Dataannotationsvalidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[Adddataannotationsvalidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Blazor iki tür doğrulama gerçekleştirir:

* *Alan doğrulama* , Kullanıcı bir alanın dışına eklendiğinde gerçekleştirilir. Alan doğrulaması sırasında, `DataAnnotationsValidator` bileşen bildirilen tüm doğrulama sonuçlarını alanla ilişkilendirir.
* Kullanıcı formu gönderdiğinde *model doğrulaması* gerçekleştirilir. Model doğrulaması sırasında `DataAnnotationsValidator` bileşeni, doğrulama sonucunun raporlandığı üye adına göre alanı saptamaya çalışır. Tek bir üyeyle ilişkilendirilmeyen doğrulama sonuçları, bir alan yerine modeliyle ilişkilendirilir.

### <a name="validation-summary-and-validation-message-components"></a>Doğrulama özeti ve doğrulama Iletisi bileşenleri

`ValidationSummary` bileşeni, [doğrulama özeti etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)'na benzer olan tüm doğrulama iletilerini özetler:

```razor
<ValidationSummary />
```

`Model` parametresine sahip belirli bir model için çıkış doğrulama iletileri:
  
```razor
<ValidationSummary Model="@starship" />
```

`ValidationMessage` bileşeni, [doğrulama Iletisi etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)'na benzer olan belirli bir alan için doğrulama iletilerini görüntüler. `For` özniteliğiyle doğrulama için alanı ve model özelliğini adlandırırken bir lambda ifadesini belirtin:

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

`ValidationMessage` ve `ValidationSummary` bileşenleri rastgele öznitelikleri destekler. Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik, oluşturulan `<div>` veya `<ul>` öğesine eklenir.

### <a name="custom-validation-attributes"></a>Özel doğrulama öznitelikleri

Bir doğrulama sonucunun [özel bir doğrulama özniteliği](xref:mvc/models/validation#custom-attributes)kullanılırken bir alanla doğru bir şekilde ilişkilendirildiğinden emin olmak için, <xref:System.ComponentModel.DataAnnotations.ValidationResult>oluştururken doğrulama bağlamının <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> geçirin:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

private class MyCustomValidator : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, 
        ValidationContext validationContext)
    {
        ...

        return new ValidationResult("Validation message to user.",
            new[] { validationContext.MemberName });
    }
}
```

::: moniker range=">= aspnetcore-3.1"

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor veri ek açıklamaları doğrulama paketi

[Microsoft. AspNetCore.Blazor. Dataaçıklamalarda. doğrulama](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) , `DataAnnotationsValidator` bileşenini kullanarak doğrulama deneyimini boşlukları dolduran bir pakettir. Paket şu anda *deneysel*.

### <a name="compareproperty-attribute"></a>[CompareProperty] özniteliği

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> `DataAnnotationsValidator` bileşeniyle iyi çalışmaz. [Microsoft. AspNetCore.Blazor. Datareek açıklamaları. doğrulaması](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *deneysel* paketi, bu sınırlamalar etrafında çalıştırılan `ComparePropertyAttribute`ek bir doğrulama özniteliği tanıtır. Blazor bir uygulamada, `[CompareProperty]` `[Compare]` özniteliğinin doğrudan bir değiştirme işlemi olur. Daha fazla bilgi için bkz. [CompareAttribute Onvalidgönderim EditForm ile yoksayıldı (DotNet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).

### <a name="nested-models-collection-types-and-complex-types"></a>İç içe modeller, koleksiyon türleri ve karmaşık türler

Blazor, yerleşik `DataAnnotationsValidator`veri açıklamalarını kullanarak form girişini doğrulamaya yönelik destek sağlar. Ancak `DataAnnotationsValidator`, yalnızca koleksiyon veya karmaşık tür özellikleri olmayan forma bağlanan modelin en üst düzey özelliklerini doğrular.

Koleksiyon ve karmaşık tür özellikleri dahil olmak üzere, bağlantılı modelin tüm nesne grafiğini doğrulamak için, *deneysel* [Microsoft. aspnetcore.Blazortarafından sunulan `ObjectGraphDataAnnotationsValidator` kullanın. Dataaçıklamalarda. doğrulama](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) paketi:

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

Model özelliklerine `[ValidateComplexType]`ekleyin. Aşağıdaki model sınıflarında, `ShipDescription` sınıfı model forma bağlandığında doğrulanacak ek veri açıklamalarını içerir:

*Starship.cs*:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    ...

    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; }

    ...
}
```

*ShipDescription.cs*:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class ShipDescription
{
    [Required]
    [StringLength(40, ErrorMessage = "Description too long (40 char).")]
    public string ShortDescription { get; set; }
    
    [Required]
    [StringLength(240, ErrorMessage = "Description too long (240 char).")]
    public string LongDescription { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Karmaşık veya koleksiyon türü özelliklerinin doğrulanması

Bir modelin özelliklerine uygulanan doğrulama öznitelikleri, form gönderildiğinde doğrular. Ancak, koleksiyonların özellikleri veya bir modelin karmaşık veri türleri `DataAnnotationsValidator` bileşeni tarafından form gönderimi üzerinde doğrulanmaz. Bu senaryoda iç içe geçmiş doğrulama özniteliklerini kabul etmek için özel bir doğrulama bileşeni kullanın. Bir örnek için [Blazor doğrulama örneğine (ASPNET/Samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)bakın.

::: moniker-end
