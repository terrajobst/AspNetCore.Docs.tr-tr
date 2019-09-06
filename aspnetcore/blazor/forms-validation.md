---
title: ASP.NET Core Blazor formları ve doğrulaması
author: guardrex
description: Blazor içinde Forms ve alan doğrulama senaryolarını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 4531ef44a7df3951f3bebdf88e597165fa75f06e
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310322"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core Blazor formları ve doğrulaması

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

Forms ve doğrulama, Blazor içinde [veri ek açıklamaları](xref:mvc/models/validation)kullanılarak desteklenir.

> [!NOTE]
> Formlar ve doğrulama senaryoları, her önizleme sürümü ile değiştirilebilir.

Aşağıdaki `ExampleModel` tür, veri ek açıklamalarını kullanarak doğrulama mantığını tanımlar:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Bir form, `EditForm` bileşeni kullanılarak tanımlanır. Aşağıdaki formda tipik öğeler, bileşenler ve Razor kodu gösterilmektedir:

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

* Form, `name` `ExampleModel` türünde tanımlanan doğrulamayı kullanarak alanda Kullanıcı girişini doğrular. Model bileşen `@code` bloğunda oluşturulur ve özel bir alanda (`exampleModel`) tutulur. Alanı, `Model` `<EditForm>` öğesinin özniteliğine atanır.
* Bileşen `DataAnnotationsValidator` , veri ek açıklamalarını kullanarak doğrulama desteği ekler.
* `ValidationSummary` Bileşen doğrulama iletilerini özetler.
* `HandleValidSubmit`Form başarıyla gönderdiğinde tetiklenir (doğrulamayı geçirir).

Kullanıcı girişini almak ve doğrulamak için yerleşik bir giriş bileşenleri kümesi vardır. Girişler değiştirildiklerinde ve bir form gönderildiğinde onaylanır. Kullanılabilir giriş bileşenleri aşağıdaki tabloda gösterilmiştir.

| Giriş bileşeni | Olarak işlendi&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Dahil olmak üzere `EditForm`tüm giriş bileşenleri, rastgele öznitelikleri destekler. Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik işlenmiş HTML öğesine eklenir.

Giriş bileşenleri, düzenleme sırasında doğrulamak ve CSS sınıfını alan durumunu yansıtacak şekilde değiştirmek için varsayılan davranışı sağlar. Bazı bileşenler, yararlı ayrıştırma mantığını içerir. Örneğin, `InputDate` `InputNumber` bunları doğrulama hatası olarak kaydederek düzeltilemez değerleri düzgün şekilde işleyin. Null değerleri kabul edebilecek türler, hedef alanın null değer alabilme durumunu da destekler (örneğin, `int?`).

Aşağıdaki `Starship` tür, daha önce daha büyük bir özellik kümesi ve daha önceki `ExampleModel`veri açıklamalarını kullanarak doğrulama mantığını tanımlar:

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

Önceki örnekte, `Description` hiçbir veri ek açıklaması mevcut olmadığından isteğe bağlıdır.

Aşağıdaki form, `Starship` modelde tanımlanan doğrulamayı kullanarak Kullanıcı girişini doğrular:

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

, `EditForm` Hangi alanların `EditContext` değiştirildiği ve geçerli doğrulama iletileri de dahil olmak üzere düzenleme işlemiyle ilgili meta verileri izleyen [basamaklı bir değer](xref:blazor/components#cascading-values-and-parameters) olarak oluşturur. Ayrıca geçerli ve geçersiz Gönderimlerle (`OnValidSubmit`, `OnInvalidSubmit`) uygun olaylar sağlar. `EditForm` Alternatif olarak, `OnSubmit` doğrulamayı tetiklemek ve alan değerlerini özel doğrulama kodu ile denetlemek için kullanın.

## <a name="inputtext-based-on-the-input-event"></a>Giriş olayına göre InputText

Olayı yerine `input`olayınıkullanan özel bir bileşen oluşturmak için bileşeninikullanın.`InputText` `change`

Aşağıdaki biçimlendirmeye sahip bir bileşen oluşturun ve bileşeni tıpkı kullanıldığı gibi `InputText` kullanın:

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

Bileşen, Basamaklandırılan `EditContext`veri açıklamalarını kullanarak doğrulama desteğini iliştirir. `DataAnnotationsValidator` Veri ek açıklamalarını kullanarak doğrulama desteğinin etkinleştirilmesi Şu anda bu açık hareketi gerektirir, ancak bunu daha sonra geçersiz kılabileceğiniz varsayılan davranışı yapmayı düşünüyoruz. Veri ek açıklamalarıyla farklı bir doğrulama sistemi kullanmak için, `DataAnnotationsValidator` öğesini özel bir uygulamayla değiştirin. ASP.NET Core uygulama, başvuru kaynağında İnceleme için kullanılabilir: [Dataannotationsvalidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[adddataannotationsvalidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *ASP.NET Core uygulama, önizleme sürümü döneminde hızlı güncelleştirmelere tabidir.*

Bileşen, [doğrulama özeti etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)'na benzer olan tüm doğrulama iletilerini özetler. `ValidationSummary`

Bileşeni, [doğrulama iletisi etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)'na benzer şekilde belirli bir alan için doğrulama iletileri görüntüler. `ValidationMessage` `For` Özniteliği ile doğrulama için alanı ve model özelliğini adlandırırken bir lambda ifadesini belirtin:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

`ValidationMessage` Ve`ValidationSummary` bileşenleri, rastgele öznitelikleri destekler. Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik oluşturulan `<div>` or `<ul>` öğesine eklenir.

### <a name="validation-of-complex-or-collection-type-properties"></a>Karmaşık veya koleksiyon türü özelliklerinin doğrulanması

Bir modelin özelliklerine uygulanan doğrulama öznitelikleri, form gönderildiğinde doğrular. Ancak, koleksiyonların özellikleri veya bir modelin karmaşık veri türleri form gönderimi üzerinde doğrulanmaz. Bu senaryoda iç içe geçmiş doğrulama özniteliklerini kabul etmek için özel bir doğrulama bileşeni kullanın. Bir örnek için, [ASPNET/Samples GitHub deposundaki Blazor doğrulama örneğine](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)bakın.
