---
title: ASP.NET Core Blazor formları ve doğrulaması
author: guardrex
description: Blazor içinde Forms ve alan doğrulama senaryolarını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 09281779e7f0b31e525e0e79c2d6d9ce9ca5b8ce
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659780"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Blazor formları ve doğrulaması

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

Forms ve doğrulama, Blazor içinde [veri ek açıklamaları](xref:mvc/models/validation)kullanılarak desteklenir.

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

    <InputText id="name" @bind-Value="@exampleModel.Name" />

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

```cshtml
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

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Doğrulama desteği

`DataAnnotationsValidator` bileşeni, basamaklı `EditContext`veri açıklamalarını kullanarak doğrulama desteğini iliştirir. Veri ek açıklamalarını kullanarak doğrulama desteğinin etkinleştirilmesi bu açık hareketi gerektirir. Veri ek açıklamalarıyla farklı bir doğrulama sistemi kullanmak için `DataAnnotationsValidator` özel bir uygulamayla değiştirin. ASP.NET Core uygulama, başvuru kaynağında İnceleme için kullanılabilir: [Dataannotationsvalidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[Adddataannotationsvalidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

`ValidationSummary` bileşeni, [doğrulama özeti etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)'na benzer olan tüm doğrulama iletilerini özetler.

`ValidationMessage` bileşeni, [doğrulama Iletisi etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)'na benzer olan belirli bir alan için doğrulama iletilerini görüntüler. `For` özniteliğiyle doğrulama için alanı ve model özelliğini adlandırırken bir lambda ifadesini belirtin:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

`ValidationMessage` ve `ValidationSummary` bileşenleri rastgele öznitelikleri destekler. Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik, oluşturulan `<div>` veya `<ul>` öğesine eklenir.

::: moniker range=">= aspnetcore-3.1"

**Microsoft. AspNetCore. Blazor. Dataaçıklamalarda. Validation Package**

[Microsoft. AspNetCore. Blazor. Dataaçıklamalarda. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) , `DataAnnotationsValidator` bileşenini kullanarak doğrulama deneyimini boşlukları dolduran bir pakettir. Paket şu anda *deneysel*bir sürümde bu senaryoları ASP.NET Core çerçevesine eklemeyi planlıyoruz.

`DataAnnotationsValidator` bileşeni, doğrulama modelinde karmaşık özelliklerin alt özelliklerini doğrulamaz. Koleksiyon türü özelliklerinin öğeleri doğrulanmadı. Bu türleri doğrulamak için `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` paketi `ObjectGraphDataAnnotationsValidator` bileşeniyle birlikte çalışarak `ValidateComplexType` doğrulama özniteliğini tanıtır. Bu türlerin kullanıldığı bir örnek için, [ASPNET/Samples GitHub deposundaki Blazor doğrulama örneğine ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)bakın.

<xref:System.ComponentModel.DataAnnotations.CompareAttribute> `DataAnnotationsValidator` bileşeniyle iyi çalışmaz. `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` paketi, `ComparePropertyAttribute`, bu sınırlamalara geçici olarak çalışabilen ek bir doğrulama özniteliği tanıtır. Bir Blazor uygulamasında `ComparePropertyAttribute`, `CompareAttribute`doğrudan bir değiştirme işlemi olur. Daha fazla bilgi için bkz. [CompareAttribute Onvalidgönderim EditForm ile yoksayıldı (ASPNET/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Karmaşık veya koleksiyon türü özelliklerinin doğrulanması

Bir modelin özelliklerine uygulanan doğrulama öznitelikleri, form gönderildiğinde doğrular. Ancak, koleksiyonların özellikleri veya bir modelin karmaşık veri türleri `DataAnnotationsValidator` bileşeni tarafından form gönderimi üzerinde doğrulanmaz. Bu senaryoda iç içe geçmiş doğrulama özniteliklerini kabul etmek için özel bir doğrulama bileşeni kullanın. Bir örnek için, [ASPNET/Samples GitHub deposundaki Blazor doğrulama örneğine](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)bakın.

::: moniker-end
