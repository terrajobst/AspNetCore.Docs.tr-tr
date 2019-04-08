---
title: Razor bileşenleri formlar ve doğrulama
author: guardrex
description: Formlar ve alan doğrulama senaryoları Razor bileşenlerde kullanmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/01/2019
uid: razor-components/forms-validation
ms.openlocfilehash: b80b553a224ff439865d0e3554746d1788f9dfcb
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069767"
---
# <a name="razor-components-forms-and-validation"></a><span data-ttu-id="53d15-103">Razor bileşenleri formlar ve doğrulama</span><span class="sxs-lookup"><span data-stu-id="53d15-103">Razor Components forms and validation</span></span>

<span data-ttu-id="53d15-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="53d15-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="53d15-105">Formlar ve doğrulama kullanarak Razor bileşenlerde desteklenir [veri ek açıklamaları](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="53d15-105">Forms and validation are supported in Razor Components using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="53d15-106">Formlar ve doğrulama senaryoları her Önizleme sürümü ile değiştirme olasılığı düşüktür.</span><span class="sxs-lookup"><span data-stu-id="53d15-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="53d15-107">Aşağıdaki `ExampleModel` doğrulama mantığını veri ek açıklamalarını kullanma türünü tanımlar:</span><span class="sxs-lookup"><span data-stu-id="53d15-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="53d15-108">Bir form kullanılarak tanımlanan `<EditForm>` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="53d15-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="53d15-109">Aşağıdaki formu tipik öğeleri, bileşenler ve Razor kod gösterir:</span><span class="sxs-lookup"><span data-stu-id="53d15-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="53d15-110">Form uygulamasında kullanıcı girdisi doğrulama `name` tanımlanan doğrulama kullanarak alan `ExampleModel` türü.</span><span class="sxs-lookup"><span data-stu-id="53d15-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="53d15-111">Model bileşenin içinde oluşturulur `@functions` engelleme ve özel bir alanda tutulan (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="53d15-111">The model is created in the component's `@functions` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="53d15-112">Alana atanan `Model` özniteliği `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="53d15-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="53d15-113">Veri ek açıklamaları Doğrulayıcısı bileşeni (`<DataAnnotationsValidator>`) veri ek açıklamalarını kullanma doğrulama desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="53d15-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="53d15-114">Doğrulama Özeti bileşeni (`<ValidationSummary>`) doğrulama iletilerinin özetler.</span><span class="sxs-lookup"><span data-stu-id="53d15-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* `HandleValidSubmit` <span data-ttu-id="53d15-115">formu başarıyla (Pass doğrulama) gönderdiğinde tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="53d15-115">is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="53d15-116">Bir dizi yerleşik giriş bileşenlerini almak ve kullanıcı girişini doğrulama kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="53d15-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="53d15-117">Girişler, bunlar değiştirilme zamanı ve bir form gönderildiğinde doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="53d15-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="53d15-118">Aşağıdaki tabloda kullanılabilir giriş bileşenleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="53d15-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="53d15-119">Giriş bileşeni</span><span class="sxs-lookup"><span data-stu-id="53d15-119">Input component</span></span>   | <span data-ttu-id="53d15-120">Çizilir&hellip;</span><span class="sxs-lookup"><span data-stu-id="53d15-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="53d15-121">Giriş bileşenleri düzenlendiğinde doğrulama ve değiştirme alanı durumunu yansıtacak şekilde onların CSS sınıfı için varsayılan davranış sağlar.</span><span class="sxs-lookup"><span data-stu-id="53d15-121">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="53d15-122">Bazı bileşenler yararlı ayrıştırma mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="53d15-122">Some components include useful parsing logic.</span></span> <span data-ttu-id="53d15-123">Örneğin, `<InputDate>` ve `<InputNumber>` ayrıştırılamayan değerler işleme düzgün bir şekilde doğrulama hataları kaydederek.</span><span class="sxs-lookup"><span data-stu-id="53d15-123">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="53d15-124">Null değerlerini kabul edebilir türleri de hedef alan NULL atanabilirliği destekler (örneğin, `int?`).</span><span class="sxs-lookup"><span data-stu-id="53d15-124">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="53d15-125">Aşağıdaki `Starship` türü doğrulama mantığını kullanarak daha büyük bir özellikler kümesini tanımlar ve [veri ek açıklamaları](xref:mvc/models/validation) daha önceki `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="53d15-125">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="53d15-126">Aşağıdaki formu içinde tanımlanan doğrulama kullanan kullanıcı girişi doğrulama `Starship` modeli:</span><span class="sxs-lookup"><span data-stu-id="53d15-126">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="53d15-127">`<EditForm>` Oluşturur bir `EditContext` olarak bir [basamaklı değeri](xref:razor-components/components#cascading-values-and-parameters) düzenleme işlemi, hangi alanların değiştirilmiş dahil olmak üzere hakkındaki meta verileri ve geçerli doğrulama iletileri izler.</span><span class="sxs-lookup"><span data-stu-id="53d15-127">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:razor-components/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="53d15-128">`<EditForm>` De uygun olayları sağlar için geçerli ve geçersiz gönderir (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="53d15-128">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="53d15-129">Alternatif olarak, `OnSubmit` doğrulamayı tetiklemek için ve özel doğrulama kodu ile alan değerlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="53d15-129">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="53d15-130">Veri ek açıklamaları Doğrulayıcısı bileşeni (`<DataAnnotationsValidator>`) art arda için veri ek açıklamalarını kullanma doğrulama desteği ekler `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="53d15-130">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="53d15-131">Bu açık hareket veri ek açıklamaları şu anda kullanarak doğrulama için desteği etkinleştirme gerektirir, ancak bu daha sonra geçersiz varsayılan davranışı yapmadan düşündüğümüz.</span><span class="sxs-lookup"><span data-stu-id="53d15-131">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="53d15-132">Veri ek açıklamaları farklı doğrulama sistemdekinden kullanmak için veri ek açıklamaları Doğrulayıcı özel bir uygulama ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="53d15-132">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="53d15-133">ASP.NET Core uygulaması için başvuru kaynağı incelemesinin kullanılabilir: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="53d15-133">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> *<span data-ttu-id="53d15-134">ASP.NET Core uygulaması Önizleme yayın süresince hızlı güncelleştirmeler tabidir.</span><span class="sxs-lookup"><span data-stu-id="53d15-134">The ASP.NET Core implementation is subject to rapid updates during the preview release period.</span></span>*

<span data-ttu-id="53d15-135">Doğrulama Özeti bileşeni (`<ValidationSummary>`) tüm doğrulama iletilerinin özetlediği benzer [doğrulama özeti etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="53d15-135">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="53d15-136">Doğrulama iletisi bileşeni (`<ValidationMessage>`) için benzer bir özel alan doğrulama iletilerinin görüntüler [doğrulama iletisi etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="53d15-136">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="53d15-137">Alan ile doğrulama için belirttiğiniz `For` özniteliğini ve model özelliğine adlandırma bir lambda ifadesi:</span><span class="sxs-lookup"><span data-stu-id="53d15-137">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> <span data-ttu-id="53d15-138">Yerleşik giriş bileşenleri, gelecek sürümlerde çözmek için bekliyoruz sınırlamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="53d15-138">Built-in input components have limitations that we expect to resolve in future releases.</span></span> <span data-ttu-id="53d15-139">Örneğin, isteğe bağlı öznitelikleri üzerinde oluşturulan belirtilemez `<input>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="53d15-139">For example, you can't specify arbitrary attributes on the generated `<input>` tags.</span></span> <span data-ttu-id="53d15-140">Kullanılamayan senaryoları işlemek için kendi bileşeni alt sınıflar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="53d15-140">Build your own component subclasses to handle unavailable scenarios.</span></span>
