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
ms.openlocfilehash: f4c1845ee4b6ff9274b7117167367ccdd9f36c12
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943699"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="cd1d9-103">ASP.NET Core Blazor formları ve doğrulama</span><span class="sxs-lookup"><span data-stu-id="cd1d9-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="cd1d9-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="cd1d9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cd1d9-105">Forms ve doğrulama, [veri ek açıklamaları](xref:mvc/models/validation)kullanılarak Blazor desteklenir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="cd1d9-106">Aşağıdaki `ExampleModel` türü, veri ek açıklamalarını kullanarak doğrulama mantığını tanımlar:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="cd1d9-107">Bir form `EditForm` bileşeni kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="cd1d9-108">Aşağıdaki formda tipik öğeler, bileşenler ve Razor kodu gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="cd1d9-109">Form, `ExampleModel` türünde tanımlanan doğrulamayı kullanarak `name` alanında Kullanıcı girişini doğrular.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="cd1d9-110">Model, bileşenin `@code` bloğunda oluşturulur ve özel bir alanda tutulur (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="cd1d9-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="cd1d9-111">Alan, `<EditForm>` öğesinin `Model` özniteliğine atanır.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="cd1d9-112">`DataAnnotationsValidator` bileşeni, veri açıklamalarını kullanarak doğrulama desteğini ekler.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="cd1d9-113">`ValidationSummary` bileşeni doğrulama iletilerini özetler.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="cd1d9-114">`HandleValidSubmit`, form başarıyla gönderdiğinde tetiklenir (doğrulamayı geçirir).</span><span class="sxs-lookup"><span data-stu-id="cd1d9-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="cd1d9-115">Kullanıcı girişini almak ve doğrulamak için yerleşik bir giriş bileşenleri kümesi vardır.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="cd1d9-116">Girişler değiştirildiklerinde ve bir form gönderildiğinde onaylanır.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="cd1d9-117">Kullanılabilir giriş bileşenleri aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="cd1d9-118">Giriş bileşeni</span><span class="sxs-lookup"><span data-stu-id="cd1d9-118">Input component</span></span> | <span data-ttu-id="cd1d9-119">&hellip; olarak işlendi</span><span class="sxs-lookup"><span data-stu-id="cd1d9-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="cd1d9-120">`EditForm`dahil olmak üzere tüm giriş bileşenleri, rastgele öznitelikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="cd1d9-121">Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik işlenmiş HTML öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="cd1d9-122">Giriş bileşenleri, düzenleme sırasında doğrulamak ve CSS sınıfını alan durumunu yansıtacak şekilde değiştirmek için varsayılan davranışı sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="cd1d9-123">Bazı bileşenler, yararlı ayrıştırma mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="cd1d9-124">Örneğin, `InputDate` ve `InputNumber`, onları doğrulama hatası olarak kaydederek düzeltilemez değerleri düzgün şekilde işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="cd1d9-125">Null değerleri kabul edebilecek türler, hedef alanın null değer alabilme durumunu da destekler (örneğin, `int?`).</span><span class="sxs-lookup"><span data-stu-id="cd1d9-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="cd1d9-126">Aşağıdaki `Starship` türü, önceki `ExampleModel`daha büyük bir özellik kümesi ve veri ek açıklamaları kullanarak doğrulama mantığını tanımlar:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="cd1d9-127">Yukarıdaki örnekte, hiçbir veri ek açıklaması mevcut olmadığından `Description` isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="cd1d9-128">Aşağıdaki form, `Starship` modelinde tanımlanan doğrulamayı kullanarak Kullanıcı girişini doğrular:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="cd1d9-129">`EditForm`, hangi alanların değiştirildiği ve geçerli doğrulama iletileri de dahil olmak üzere düzenleme işlemiyle ilgili meta verileri izleyen [basamaklı bir değer](xref:blazor/components#cascading-values-and-parameters) olarak `EditContext` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="cd1d9-130">`EditForm` ayrıca geçerli ve geçersiz gönderir (`OnValidSubmit`, `OnInvalidSubmit`) için uygun olaylar sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="cd1d9-131">Alternatif olarak, doğrulamayı tetiklemek ve alan değerlerini özel doğrulama kodu ile denetlemek için `OnSubmit` kullanın.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="cd1d9-132">Giriş olayına göre InputText</span><span class="sxs-lookup"><span data-stu-id="cd1d9-132">InputText based on the input event</span></span>

<span data-ttu-id="cd1d9-133">`change` olayı yerine `input` olayını kullanan özel bir bileşen oluşturmak için `InputText` bileşenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="cd1d9-134">Aşağıdaki biçimlendirmeye sahip bir bileşen oluşturun ve bileşeni tıpkı `InputText` kullanıldığı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="cd1d9-135">Doğrulama desteği</span><span class="sxs-lookup"><span data-stu-id="cd1d9-135">Validation support</span></span>

<span data-ttu-id="cd1d9-136">`DataAnnotationsValidator` bileşeni, basamaklı `EditContext`veri açıklamalarını kullanarak doğrulama desteğini iliştirir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="cd1d9-137">Veri ek açıklamalarını kullanarak doğrulama desteğinin etkinleştirilmesi bu açık hareketi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="cd1d9-138">Veri ek açıklamalarıyla farklı bir doğrulama sistemi kullanmak için `DataAnnotationsValidator` özel bir uygulamayla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="cd1d9-139">ASP.NET Core uygulama, başvuru kaynağında İnceleme için kullanılabilir: [Dataannotationsvalidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[Adddataannotationsvalidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="cd1d9-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="cd1d9-140"> iki tür doğrulama gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-140"> performs two types of validation:</span></span>

* <span data-ttu-id="cd1d9-141">*Alan doğrulama* , Kullanıcı bir alanın dışına eklendiğinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-141">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="cd1d9-142">Alan doğrulaması sırasında, `DataAnnotationsValidator` bileşen bildirilen tüm doğrulama sonuçlarını alanla ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-142">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="cd1d9-143">Kullanıcı formu gönderdiğinde *model doğrulaması* gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-143">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="cd1d9-144">Model doğrulaması sırasında `DataAnnotationsValidator` bileşeni, doğrulama sonucunun raporlandığı üye adına göre alanı saptamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-144">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="cd1d9-145">Tek bir üyeyle ilişkilendirilmeyen doğrulama sonuçları, bir alan yerine modeliyle ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-145">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="cd1d9-146">Doğrulama özeti ve doğrulama Iletisi bileşenleri</span><span class="sxs-lookup"><span data-stu-id="cd1d9-146">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="cd1d9-147">`ValidationSummary` bileşeni, [doğrulama özeti etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)'na benzer olan tüm doğrulama iletilerini özetler:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-147">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="cd1d9-148">`Model` parametresine sahip belirli bir model için çıkış doğrulama iletileri:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-148">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="cd1d9-149">`ValidationMessage` bileşeni, [doğrulama Iletisi etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)'na benzer olan belirli bir alan için doğrulama iletilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-149">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="cd1d9-150">`For` özniteliğiyle doğrulama için alanı ve model özelliğini adlandırırken bir lambda ifadesini belirtin:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-150">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="cd1d9-151">`ValidationMessage` ve `ValidationSummary` bileşenleri rastgele öznitelikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-151">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="cd1d9-152">Bir bileşen parametresiyle eşleşmeyen herhangi bir öznitelik, oluşturulan `<div>` veya `<ul>` öğesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-152">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="cd1d9-153">Özel doğrulama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="cd1d9-153">Custom validation attributes</span></span>

<span data-ttu-id="cd1d9-154">Bir doğrulama sonucunun [özel bir doğrulama özniteliği](xref:mvc/models/validation#custom-attributes)kullanılırken bir alanla doğru bir şekilde ilişkilendirildiğinden emin olmak için, <xref:System.ComponentModel.DataAnnotations.ValidationResult>oluştururken doğrulama bağlamının <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> geçirin:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-154">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="cd1d9-155"> veri ek açıklamaları doğrulama paketi</span><span class="sxs-lookup"><span data-stu-id="cd1d9-155"> data annotations validation package</span></span>

<span data-ttu-id="cd1d9-156">[Microsoft. AspNetCore.Blazor. Dataaçıklamalarda. doğrulama](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) , `DataAnnotationsValidator` bileşenini kullanarak doğrulama deneyimini boşlukları dolduran bir pakettir.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-156">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="cd1d9-157">Paket şu anda *deneysel*.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-157">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="cd1d9-158">[CompareProperty] özniteliği</span><span class="sxs-lookup"><span data-stu-id="cd1d9-158">[CompareProperty] attribute</span></span>

<span data-ttu-id="cd1d9-159"><xref:System.ComponentModel.DataAnnotations.CompareAttribute> `DataAnnotationsValidator` bileşeniyle iyi çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-159">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="cd1d9-160">[Microsoft. AspNetCore.Blazor. Datareek açıklamaları. doğrulaması](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *deneysel* paketi, bu sınırlamalar etrafında çalıştırılan `ComparePropertyAttribute`ek bir doğrulama özniteliği tanıtır.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-160">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="cd1d9-161">Blazor bir uygulamada, `[CompareProperty]` `[Compare]` özniteliğinin doğrudan bir değiştirme işlemi olur.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-161">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span> <span data-ttu-id="cd1d9-162">Daha fazla bilgi için bkz. [CompareAttribute Onvalidgönderim EditForm ile yoksayıldı (ASPNET/AspNetCore #10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).</span><span class="sxs-lookup"><span data-stu-id="cd1d9-162">For more information, see [CompareAttribute ignored with OnValidSubmit EditForm (aspnet/AspNetCore #10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="cd1d9-163">İç içe modeller, koleksiyon türleri ve karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="cd1d9-163">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="cd1d9-164">, yerleşik `DataAnnotationsValidator`veri açıklamalarını kullanarak form girişini doğrulamaya yönelik destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-164"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="cd1d9-165">Ancak `DataAnnotationsValidator`, yalnızca koleksiyon veya karmaşık tür özellikleri olmayan forma bağlanan modelin en üst düzey özelliklerini doğrular.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-165">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="cd1d9-166">Koleksiyon ve karmaşık tür özellikleri dahil olmak üzere, bağlantılı modelin tüm nesne grafiğini doğrulamak için, *deneysel* [Microsoft. aspnetcore.Blazortarafından sunulan `ObjectGraphDataAnnotationsValidator` kullanın. Dataaçıklamalarda. doğrulama](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) paketi:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-166">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="cd1d9-167">Model özelliklerine `[ValidateComplexType]`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-167">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="cd1d9-168">Aşağıdaki model sınıflarında, `ShipDescription` sınıfı model forma bağlandığında doğrulanacak ek veri açıklamalarını içerir:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-168">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="cd1d9-169">*Starship.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-169">*Starship.cs*:</span></span>

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

<span data-ttu-id="cd1d9-170">*ShipDescription.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd1d9-170">*ShipDescription.cs*:</span></span>

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

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="cd1d9-171">Karmaşık veya koleksiyon türü özelliklerinin doğrulanması</span><span class="sxs-lookup"><span data-stu-id="cd1d9-171">Validation of complex or collection type properties</span></span>

<span data-ttu-id="cd1d9-172">Bir modelin özelliklerine uygulanan doğrulama öznitelikleri, form gönderildiğinde doğrular.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-172">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="cd1d9-173">Ancak, koleksiyonların özellikleri veya bir modelin karmaşık veri türleri `DataAnnotationsValidator` bileşeni tarafından form gönderimi üzerinde doğrulanmaz.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-173">However, the properties of collections or complex data types of a model aren't validated on form submission by the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="cd1d9-174">Bu senaryoda iç içe geçmiş doğrulama özniteliklerini kabul etmek için özel bir doğrulama bileşeni kullanın.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-174">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="cd1d9-175">Bir örnek için [Blazor doğrulama örneğine (ASPNET/Samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation)bakın.</span><span class="sxs-lookup"><span data-stu-id="cd1d9-175">For an example, see the [Blazor Validation sample (aspnet/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

::: moniker-end
