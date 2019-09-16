---
title: ASP.NET Core Blazor formları ve doğrulaması
author: guardrex
description: Blazor içinde Forms ve alan doğrulama senaryolarını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2019
uid: blazor/forms-validation
ms.openlocfilehash: 2fd76db90a53e328cd2ac8f452fba58365db0384
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011066"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="5efc0-103">ASP.NET Core Blazor formları ve doğrulaması</span><span class="sxs-lookup"><span data-stu-id="5efc0-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="5efc0-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="5efc0-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5efc0-105">Forms ve doğrulama, Blazor içinde [veri ek açıklamaları](xref:mvc/models/validation)kullanılarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="5efc0-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="5efc0-106">Formlar ve doğrulama senaryoları, her önizleme sürümü ile değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5efc0-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="5efc0-107">Aşağıdaki `ExampleModel` tür, veri ek açıklamalarını kullanarak doğrulama mantığını tanımlar:</span><span class="sxs-lookup"><span data-stu-id="5efc0-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="5efc0-108">Bir form, `EditForm` bileşeni kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="5efc0-108">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="5efc0-109">Aşağıdaki formda tipik öğeler, bileşenler ve Razor kodu gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5efc0-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="5efc0-110">Form, `name` `ExampleModel` türünde tanımlanan doğrulamayı kullanarak alanda Kullanıcı girişini doğrular.</span><span class="sxs-lookup"><span data-stu-id="5efc0-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="5efc0-111">Model bileşen `@code` bloğunda oluşturulur ve özel bir alanda (`exampleModel`) tutulur.</span><span class="sxs-lookup"><span data-stu-id="5efc0-111">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="5efc0-112">Alanı, `Model` `<EditForm>` öğesinin özniteliğine atanır.</span><span class="sxs-lookup"><span data-stu-id="5efc0-112">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="5efc0-113">Bileşen `DataAnnotationsValidator` , veri ek açıklamalarını kullanarak doğrulama desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="5efc0-113">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="5efc0-114">`ValidationSummary` Bileşen doğrulama iletilerini özetler.</span><span class="sxs-lookup"><span data-stu-id="5efc0-114">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="5efc0-115">`HandleValidSubmit`Form başarıyla gönderdiğinde tetiklenir (doğrulamayı geçirir).</span><span class="sxs-lookup"><span data-stu-id="5efc0-115">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="5efc0-116">Kullanıcı girişini almak ve doğrulamak için yerleşik bir giriş bileşenleri kümesi vardır.</span><span class="sxs-lookup"><span data-stu-id="5efc0-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="5efc0-117">Girişler değiştirildiklerinde ve bir form gönderildiğinde onaylanır.</span><span class="sxs-lookup"><span data-stu-id="5efc0-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="5efc0-118">Kullanılabilir giriş bileşenleri aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5efc0-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="5efc0-119">Giriş bileşeni</span><span class="sxs-lookup"><span data-stu-id="5efc0-119">Input component</span></span> | <span data-ttu-id="5efc0-120">Olarak işlendi&hellip;</span><span class="sxs-lookup"><span data-stu-id="5efc0-120">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="5efc0-121">Dahil olmak üzere `EditForm`tüm giriş bileşenleri, rastgele öznitelikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="5efc0-121">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="5efc0-122">Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik işlenmiş HTML öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="5efc0-122">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="5efc0-123">Giriş bileşenleri, düzenleme sırasında doğrulamak ve CSS sınıfını alan durumunu yansıtacak şekilde değiştirmek için varsayılan davranışı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5efc0-123">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="5efc0-124">Bazı bileşenler, yararlı ayrıştırma mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="5efc0-124">Some components include useful parsing logic.</span></span> <span data-ttu-id="5efc0-125">Örneğin, `InputDate` `InputNumber` bunları doğrulama hatası olarak kaydederek düzeltilemez değerleri düzgün şekilde işleyin.</span><span class="sxs-lookup"><span data-stu-id="5efc0-125">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="5efc0-126">Null değerleri kabul edebilecek türler, hedef alanın null değer alabilme durumunu da destekler (örneğin, `int?`).</span><span class="sxs-lookup"><span data-stu-id="5efc0-126">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="5efc0-127">Aşağıdaki `Starship` tür, daha önce daha büyük bir özellik kümesi ve daha önceki `ExampleModel`veri açıklamalarını kullanarak doğrulama mantığını tanımlar:</span><span class="sxs-lookup"><span data-stu-id="5efc0-127">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="5efc0-128">Önceki örnekte, `Description` hiçbir veri ek açıklaması mevcut olmadığından isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5efc0-128">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="5efc0-129">Aşağıdaki form, `Starship` modelde tanımlanan doğrulamayı kullanarak Kullanıcı girişini doğrular:</span><span class="sxs-lookup"><span data-stu-id="5efc0-129">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="5efc0-130">, `EditForm` Hangi alanların `EditContext` değiştirildiği ve geçerli doğrulama iletileri de dahil olmak üzere düzenleme işlemiyle ilgili meta verileri izleyen [basamaklı bir değer](xref:blazor/components#cascading-values-and-parameters) olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5efc0-130">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="5efc0-131">Ayrıca geçerli ve geçersiz Gönderimlerle (`OnValidSubmit`, `OnInvalidSubmit`) uygun olaylar sağlar. `EditForm`</span><span class="sxs-lookup"><span data-stu-id="5efc0-131">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="5efc0-132">Alternatif olarak, `OnSubmit` doğrulamayı tetiklemek ve alan değerlerini özel doğrulama kodu ile denetlemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="5efc0-132">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="5efc0-133">Giriş olayına göre InputText</span><span class="sxs-lookup"><span data-stu-id="5efc0-133">InputText based on the input event</span></span>

<span data-ttu-id="5efc0-134">Olayı yerine `input`olayınıkullanan özel bir bileşen oluşturmak için bileşeninikullanın.`InputText` `change`</span><span class="sxs-lookup"><span data-stu-id="5efc0-134">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="5efc0-135">Aşağıdaki biçimlendirmeye sahip bir bileşen oluşturun ve bileşeni tıpkı kullanıldığı gibi `InputText` kullanın:</span><span class="sxs-lookup"><span data-stu-id="5efc0-135">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="5efc0-136">Doğrulama desteği</span><span class="sxs-lookup"><span data-stu-id="5efc0-136">Validation support</span></span>

<span data-ttu-id="5efc0-137">Bileşen, Basamaklandırılan `EditContext`veri açıklamalarını kullanarak doğrulama desteğini iliştirir. `DataAnnotationsValidator`</span><span class="sxs-lookup"><span data-stu-id="5efc0-137">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="5efc0-138">Veri ek açıklamalarını kullanarak doğrulama desteğinin etkinleştirilmesi bu açık hareketi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5efc0-138">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="5efc0-139">Veri ek açıklamalarıyla farklı bir doğrulama sistemi kullanmak için, `DataAnnotationsValidator` öğesini özel bir uygulamayla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5efc0-139">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="5efc0-140">ASP.NET Core uygulama, başvuru kaynağında İnceleme için kullanılabilir: [Dataannotationsvalidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[adddataannotationsvalidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="5efc0-140">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

<span data-ttu-id="5efc0-141">Bileşen, [doğrulama özeti etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)'na benzer olan tüm doğrulama iletilerini özetler. `ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="5efc0-141">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="5efc0-142">Bileşeni, [doğrulama iletisi etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)'na benzer şekilde belirli bir alan için doğrulama iletileri görüntüler. `ValidationMessage`</span><span class="sxs-lookup"><span data-stu-id="5efc0-142">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="5efc0-143">`For` Özniteliği ile doğrulama için alanı ve model özelliğini adlandırırken bir lambda ifadesini belirtin:</span><span class="sxs-lookup"><span data-stu-id="5efc0-143">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="5efc0-144">`ValidationMessage` Ve`ValidationSummary` bileşenleri, rastgele öznitelikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="5efc0-144">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="5efc0-145">Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik oluşturulan `<div>` or `<ul>` öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="5efc0-145">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="5efc0-146">Karmaşık veya koleksiyon türü özelliklerinin doğrulanması</span><span class="sxs-lookup"><span data-stu-id="5efc0-146">Validation of complex or collection type properties</span></span>

<span data-ttu-id="5efc0-147">Bir modelin özelliklerine uygulanan doğrulama öznitelikleri, form gönderildiğinde doğrular.</span><span class="sxs-lookup"><span data-stu-id="5efc0-147">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="5efc0-148">Ancak, koleksiyonların özellikleri veya bir modelin karmaşık veri türleri form gönderimi üzerinde doğrulanmaz.</span><span class="sxs-lookup"><span data-stu-id="5efc0-148">However, the properties of collections or complex data types of a model aren't validated on form submission.</span></span> <span data-ttu-id="5efc0-149">Bu senaryoda iç içe geçmiş doğrulama özniteliklerini kabul etmek için özel bir doğrulama bileşeni kullanın.</span><span class="sxs-lookup"><span data-stu-id="5efc0-149">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="5efc0-150">Bir örnek için, [ASPNET/Samples GitHub deposundaki Blazor doğrulama örneğine](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)bakın.</span><span class="sxs-lookup"><span data-stu-id="5efc0-150">For an example, see the [Blazor Validation sample in the aspnet/samples GitHub repository](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>
