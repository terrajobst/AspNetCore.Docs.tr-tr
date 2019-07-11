---
title: ASP.NET core'da etiket Yardımcıları
author: rick-anderson
description: Etiket Yardımcıları nelerdir ve bunların içinde ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 03/18/2019
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 870ce2eb28f384b380cc1178842325dc28199f09
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814986"
---
# <a name="tag-helpers-in-aspnet-core"></a><span data-ttu-id="1738f-103">ASP.NET core'da etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="1738f-103">Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="1738f-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1738f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="1738f-105">Etiket Yardımcıları nelerdir</span><span class="sxs-lookup"><span data-stu-id="1738f-105">What are Tag Helpers</span></span>

<span data-ttu-id="1738f-106">Etiket Yardımcıları oluşturma ve Razor dosyalarında HTML öğeleri işleme katılmak sunucu tarafı kodu etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="1738f-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1738f-107">Örneğin, yerleşik `ImageTagHelper` görüntü adı için bir sürüm numarası ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1738f-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="1738f-108">Görüntü değiştiğinde geçerli resim (yerine, eski bir önbelleğe alınan görüntü) almak için istemcilerini garanti için sunucu görüntüsü için benzersiz yeni bir sürümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1738f-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="1738f-109">Birçok yerleşik etiket Yardımcıları için formlar, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi ortak görevler - vardır.</span><span class="sxs-lookup"><span data-stu-id="1738f-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="1738f-110">Etiket Yardımcıları C# dilinde yazılmış ve öğe adı, öznitelik adı veya üst etiketi dayalı HTML öğeleri hedeflenir.</span><span class="sxs-lookup"><span data-stu-id="1738f-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="1738f-111">Örneğin, yerleşik `LabelTagHelper` HTML hedefleyebilir `<label>` öğesi olduğunda `LabelTagHelper` öznitelikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1738f-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="1738f-112">Aşina değilseniz [HTML Yardımcıları](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), HTML ve C# arasında açık geçişler Razor görünümleri, etiket Yardımcıları azaltın.</span><span class="sxs-lookup"><span data-stu-id="1738f-112">If you're familiar with [HTML Helpers](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="1738f-113">Çoğu durumda, HTML Yardımcıları, belirli bir etiketi Yardımcısı için alternatif bir yaklaşım sağlar, ancak etiket Yardımcıları HTML Yardımcıları değiştirin yoktur ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1738f-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="1738f-114">[Etiket Yardımcıları için HTML Yardımcıları karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1738f-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="1738f-115">Etiket Yardımcıları neler sağlar</span><span class="sxs-lookup"><span data-stu-id="1738f-115">What Tag Helpers provide</span></span>

<span data-ttu-id="1738f-116">**Bir HTML kullanımı kolay geliştirme deneyimi** en bölümü için etiket Yardımcıları kullanarak Razor işaretlemesi standart HTML gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="1738f-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="1738f-117">Ön uç tasarımcılar ile HTML/CSS/JavaScript conversant öğrenme olmadan Razor düzenleyebilir C# Razor söz dizimi.</span><span class="sxs-lookup"><span data-stu-id="1738f-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="1738f-118">**HTML ve Razor biçimlendirme oluşturmak için zengin IntelliSense ortamı** sharp Karşıtlık HTML Yardımcıları, sunucu tarafı Razor görünümleri işaretlemede oluşturulmasını önceki yaklaşım budur.</span><span class="sxs-lookup"><span data-stu-id="1738f-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="1738f-119">[Etiket Yardımcıları için HTML Yardımcıları karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1738f-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="1738f-120">[Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) IntelliSense ortamı açıklar.</span><span class="sxs-lookup"><span data-stu-id="1738f-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="1738f-121">C# Razor işaretlemesi yazmaktan etiket Yardımcıları kullanarak daha üretken bile geliştiricilerin Razor C# sözdizimi ile karşılaştı.</span><span class="sxs-lookup"><span data-stu-id="1738f-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="1738f-122">**Daha üretken ve üretmek için yaptığınız daha sağlam, güvenilir şekilde ve yalnızca sunucu üzerinde bilgileriyle Bakımı yapılabilir kodu** Örneğin, geçmişte görüntüleri güncelleştirme mantra değiştirdiğinizde, görüntünün adını değiştirmek için oluştu Resim.</span><span class="sxs-lookup"><span data-stu-id="1738f-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="1738f-123">Görüntüleri performansla ilgili nedenlerden dolayı agresif önbelleğe alınması gerektiğini ve görüntü adını değiştirmediğiniz sürece, eski bir kopyasını alma istemciler risk.</span><span class="sxs-lookup"><span data-stu-id="1738f-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="1738f-124">Tarihsel olarak, görüntü düzenlendi sonra adı değiştirilmesi gerekti ve web uygulamasında görüntü her başvuru güncelleştirilmesi gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="1738f-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="1738f-125">Yalnızca bu, bu da hataya (, bir başvurusu eksik olabilir, yanlışlıkla girin yanlış dize, vb.) yoğundur çok emek mi Yerleşik `ImageTagHelper` sizin için otomatik olarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1738f-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="1738f-126">`ImageTagHelper` Bir sürüm ekleyebilir görüntü değiştiğinde sunucu görüntüsü için benzersiz yeni bir sürümü otomatik olarak oluşturur. Bu nedenle sayı görüntü adı için.</span><span class="sxs-lookup"><span data-stu-id="1738f-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="1738f-127">İstemciler, geçerli görüntü almak için garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="1738f-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="1738f-128">Kullanarak bu sağlamlık ve iş Pazarı tasarruf temelde ücretsiz gelen `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="1738f-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="1738f-129">En çok yerleşik etiket Yardımcıları standart HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="1738f-129">Most built-in Tag Helpers target standard HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="1738f-130">Örneğin, `<input>` birçok görünümlerde kullanılan öğe *görünümler/hesap* klasörde `asp-for` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1738f-130">For example, the `<input>` element used in many views in the *Views/Account* folder contains the `asp-for` attribute.</span></span> <span data-ttu-id="1738f-131">Bu öznitelik, İşlenmiş HTML belirtilen model özelliğin adını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="1738f-131">This attribute extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="1738f-132">Bir Razor görünüm aşağıdaki modeliyle göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1738f-132">Consider a Razor view with the following model:</span></span>

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

<span data-ttu-id="1738f-133">Aşağıdaki Razor işaretlemesi:</span><span class="sxs-lookup"><span data-stu-id="1738f-133">The following Razor markup:</span></span>

```cshtml
<label asp-for="Movie.Title"></label>
```

<span data-ttu-id="1738f-134">Aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1738f-134">Generates the following HTML:</span></span>

```html
<label for="Movie_Title">Title</label>
```

<span data-ttu-id="1738f-135">`asp-for` Özniteliği tarafından kullanılabilir yapılan `For` özelliğinde [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1738f-135">The `asp-for` attribute is made available by the `For` property in the [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span></span> <span data-ttu-id="1738f-136">Bkz: [Yazar etiket Yardımcıları](xref:mvc/views/tag-helpers/authoring) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1738f-136">See [Author Tag Helpers](xref:mvc/views/tag-helpers/authoring) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="1738f-137">Kapsam etiketi Yardımcısı'nı yönetme</span><span class="sxs-lookup"><span data-stu-id="1738f-137">Managing Tag Helper scope</span></span>

<span data-ttu-id="1738f-138">Etiket Yardımcıları kapsamı, bir birleşimiyle denetlenir `@addTagHelper`, `@removeTagHelper`ve "!" çevirme karakter.</span><span class="sxs-lookup"><span data-stu-id="1738f-138">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="1738f-139">`@addTagHelper` Etiket Yardımcıları kullanılabilir hale getirir</span><span class="sxs-lookup"><span data-stu-id="1738f-139">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="1738f-140">Adlı yeni bir ASP.NET Core web uygulaması oluşturursanız *AuthoringTagHelpers*, aşağıdaki *Views/_ViewImports.cshtml* dosyası projenize eklenecek:</span><span class="sxs-lookup"><span data-stu-id="1738f-140">If you create a new ASP.NET Core web app named *AuthoringTagHelpers*, the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="1738f-141">`@addTagHelper` Yönergesi etiket Yardımcıları için görüntüleme kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="1738f-141">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="1738f-142">Bu durumda, görünüm dosyasıdır *Pages/_ViewImports.cshtml*, varsayılan olarak tüm dosyaları tarafından devralınır *sayfaları* klasörü ve alt klasörleri; etiket Yardımcıları kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="1738f-142">In this case, the view file is *Pages/_ViewImports.cshtml*, which by default is inherited by all files in the *Pages* folder and subfolders; making Tag Helpers available.</span></span> <span data-ttu-id="1738f-143">Yukarıdaki kod joker karakter sözdizimini kullanır ("\*") olduğunu belirtmek için belirtilen derlemedeki tüm etiket Yardımcıları (*Microsoft.AspNetCore.Mvc.TagHelpers*) her bir görünüm dosyanın çalıştırılabilecek *görünümleri* dizin veya alt dizin.</span><span class="sxs-lookup"><span data-stu-id="1738f-143">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or subdirectory.</span></span> <span data-ttu-id="1738f-144">İlk parametresinden sonra `@addTagHelper` yüklemek için etiket Yardımcıları belirtir (kullanıyoruz "\*" tüm etiket Yardımcıları için), ve ikinci parametre "Microsoft.AspNetCore.Mvc.TagHelpers" etiket Yardımcıları içeren derlemeyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="1738f-144">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="1738f-145">*Microsoft.AspNetCore.Mvc.TagHelpers* yerleşik ASP.NET Core etiket Yardımcıları için derleme.</span><span class="sxs-lookup"><span data-stu-id="1738f-145">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="1738f-146">Etiket Yardımcıları bu projedeki tüm kullanıma sunmak için (adlı bir derleme oluşturur *AuthoringTagHelpers*), aşağıdakileri kullanmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="1738f-146">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="1738f-147">Projeniz varsa bir `EmailTagHelper` varsayılan ad alanı (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), etiket Yardımcısı'nın tam adı (FQN) sağlayabilir:</span><span class="sxs-lookup"><span data-stu-id="1738f-147">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="1738f-148">Etiket Yardımcısı kullanarak bir FQN görünümüne eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="1738f-148">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="1738f-149">Çoğu geliştirici kullanmayı tercih eder "\*" joker karakter sözdizimini.</span><span class="sxs-lookup"><span data-stu-id="1738f-149">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="1738f-150">Joker karakter eklemek joker karakter sözdizimini sağlar "\*" bir FQN soneki olarak.</span><span class="sxs-lookup"><span data-stu-id="1738f-150">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="1738f-151">Örneğin, aşağıdaki yönergeleri hiçbirini getirecek `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="1738f-151">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="1738f-152">Daha önce belirtildiği gibi ekleme `@addTagHelper` yönergesini *Views/_ViewImports.cshtml* dosya etiketi Yardımcısı tüm görünüm dosyalarında kullanıma sunar *görünümleri* dizin ve alt dizinleri.</span><span class="sxs-lookup"><span data-stu-id="1738f-152">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and subdirectories.</span></span> <span data-ttu-id="1738f-153">Kullanabileceğiniz `@addTagHelper` istediğinizde bu görünümleri için etiket Yardımcısı kullanıma sunmak için katılım belirli görünüm dosyalarında yönergesi.</span><span class="sxs-lookup"><span data-stu-id="1738f-153">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="1738f-154">`@removeTagHelper` Etiket Yardımcıları kaldırır</span><span class="sxs-lookup"><span data-stu-id="1738f-154">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="1738f-155">`@removeTagHelper` Olarak aynı iki parametreye sahip `@addTagHelper`, ve daha önce eklediğiniz etiket Yardımcısı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1738f-155">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="1738f-156">Örneğin, `@removeTagHelper` görünümünden belirli görünüm kaldırır belirtilen etiketi Yardımcısı uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="1738f-156">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="1738f-157">Kullanarak `@removeTagHelper` içinde bir *Views/Folder/_ViewImports.cshtml* dosyasını kaldırır belirtilen etiketi Yardımcısı tüm görünümlerde *klasör*.</span><span class="sxs-lookup"><span data-stu-id="1738f-157">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="1738f-158">Etiket Yardımcısı kapsamlı denetleme *_viewımports.cshtml* dosyası</span><span class="sxs-lookup"><span data-stu-id="1738f-158">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="1738f-159">Ekleyebileceğiniz bir *_viewımports.cshtml* herhangi bir görünüm klasörü ve görünüm altyapısı yönergeleri Bu iki dosyadan uygulanır ve *Views/_ViewImports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="1738f-159">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1738f-160">Boş bir eklediyseniz *Views/Home/_ViewImports.cshtml* dosya *giriş* görünümleri olduğundan değişiklik olur *_viewımports.cshtml* dosya eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="1738f-160">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="1738f-161">Tüm `@addTagHelper` eklemek için yönergeleri *Views/Home/_ViewImports.cshtml* dosyası (varsayılan olmayan *Views/_ViewImports.cshtml* dosya) Bu etiket Yardımcıları görünümlere kullanılabilmesini yalnızca *Giriş* klasör.</span><span class="sxs-lookup"><span data-stu-id="1738f-161">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="1738f-162">Tek tek öğelerin seçim yapma</span><span class="sxs-lookup"><span data-stu-id="1738f-162">Opting out of individual elements</span></span>

<span data-ttu-id="1738f-163">Etiket Yardımcısı etiketi Yardımcısı çevirme karakteri ile öğe düzeyinde devre dışı bırakabilirsiniz ("!").</span><span class="sxs-lookup"><span data-stu-id="1738f-163">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="1738f-164">Örneğin, `Email` doğrulama devre dışı `<span>` etiketi Yardımcısı çevirme karakteri ile:</span><span class="sxs-lookup"><span data-stu-id="1738f-164">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="1738f-165">Etiket Yardımcısı çevirme karakterin açılış ve kapanış etiketi uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1738f-165">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="1738f-166">(Bir açma etiketi eklediğinizde Visual Studio Düzenleyicisi'ni otomatik olarak geri çevirme karakter için kapanış etiketi ekler).</span><span class="sxs-lookup"><span data-stu-id="1738f-166">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="1738f-167">Vazgeçme karakter ekledikten sonra etiket Yardımcısı öznitelikleri ve öğe artık farklı bir yazı tipinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1738f-167">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="1738f-168">Kullanarak `@tagHelperPrefix` etiketi Yardımcısı kullanım açık hale getirmek için</span><span class="sxs-lookup"><span data-stu-id="1738f-168">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="1738f-169">`@tagHelperPrefix` Yönergesi, etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket önek dizesi belirtmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1738f-169">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="1738f-170">Örneğin, aşağıdaki işaretlemede ekleyebilirsiniz *Views/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1738f-170">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```

<span data-ttu-id="1738f-171">Kod aşağıdaki resimde, etiket Yardımcısı ön ek ayarlanır `th:`, bu nedenle yalnızca önekini kullanarak öğeleri `th:` etiket Yardımcıları (etiket Yardımcısı etkin öğeleri sahip farklı bir yazı tipi) destekler.</span><span class="sxs-lookup"><span data-stu-id="1738f-171">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="1738f-172">`<label>` Ve `<input>` öğeleri etiketi Yardımcısı önekine sahip ve etiket Yardımcısı etkinleştirilmiş, çalışırken `<span>` öğesi değil.</span><span class="sxs-lookup"><span data-stu-id="1738f-172">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![görüntü](intro/_static/thp.png)

<span data-ttu-id="1738f-174">Geçerli aynı hiyerarşi kuralları `@addTagHelper` için de geçerli `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="1738f-174">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="self-closing-tag-helpers"></a><span data-ttu-id="1738f-175">Kendi kendine kapanan etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="1738f-175">Self-closing Tag Helpers</span></span>

<span data-ttu-id="1738f-176">Çok sayıda etiket Yardımcıları etiketleri kendi kendine kapanan olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="1738f-176">Many Tag Helpers can't be used as self-closing tags.</span></span> <span data-ttu-id="1738f-177">Bazı etiket Yardımcıları, kendi kendine kapanan etiketleri için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1738f-177">Some Tag Helpers are designed to be self-closing tags.</span></span> <span data-ttu-id="1738f-178">Kendi kendine kapanan olacak şekilde tasarlanmamıştır etiket Yardımcısı kullanarak işlenen çıkışı bastırır.</span><span class="sxs-lookup"><span data-stu-id="1738f-178">Using a Tag Helper that was not designed to be self-closing suppresses the rendered output.</span></span> <span data-ttu-id="1738f-179">Etiket Yardımcısı kendi kendine kapanan işlenen çıkışı bir kendi kendine kapanan etiket sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="1738f-179">Self-closing a Tag Helper results in a self-closing tag in the rendered output.</span></span> <span data-ttu-id="1738f-180">Daha fazla bilgi için [Bu Not](xref:mvc/views/tag-helpers/authoring#self-closing) içinde [yazma etiketi Yardımcıları](xref:mvc/views/tag-helpers/authoring).</span><span class="sxs-lookup"><span data-stu-id="1738f-180">For more information, see [this note](xref:mvc/views/tag-helpers/authoring#self-closing) in [Authoring Tag Helpers](xref:mvc/views/tag-helpers/authoring).</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="1738f-181">Etiket Yardımcıları için IntelliSense desteği</span><span class="sxs-lookup"><span data-stu-id="1738f-181">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="1738f-182">Visual Studio'da yeni bir ASP.NET Core web uygulaması oluşturduğunuzda, "Microsoft.AspNetCore.Razor.Tools" NuGet paketini ekler.</span><span class="sxs-lookup"><span data-stu-id="1738f-182">When you create a new ASP.NET Core web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="1738f-183">Bu etiket Yardımcısı araçları ekleyen paketidir.</span><span class="sxs-lookup"><span data-stu-id="1738f-183">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="1738f-184">Bir HTML yazmayı düşünebilirsiniz `<label>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="1738f-184">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="1738f-185">Girdiğiniz hemen sonra `<l` Visual Studio düzenleyicisinde, IntelliSense eşleşen öğeleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="1738f-185">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![görüntü](intro/_static/label.png)

<span data-ttu-id="1738f-187">Yalnızca simgesi ancak HTML Yardımı alın ("@" symbol with "<>" altındaki).</span><span class="sxs-lookup"><span data-stu-id="1738f-187">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![görüntü](intro/_static/tagSym.png)

<span data-ttu-id="1738f-189">Etiket Yardımcıları tarafından hedeflenen öğe tanıtır.</span><span class="sxs-lookup"><span data-stu-id="1738f-189">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="1738f-190">Saf HTML öğeleri (gibi `fieldset`) "<>" simgesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1738f-190">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="1738f-191">Saf bir HTML `<label>` etiketi kahverengi yazı tipi özniteliklerini kırmızı, HTML etiket (ile varsayılan Visual Studio renk teması) görüntüler ve öznitelik değerleri içinde mavi.</span><span class="sxs-lookup"><span data-stu-id="1738f-191">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![görüntü](intro/_static/LableHtmlTag.png)

<span data-ttu-id="1738f-193">Girdikten sonra `<label`, IntelliSense, kullanılabilir HTML/CSS öznitelikleri ve etiket Yardımcısı hedeflemeli öznitelikler listelenir:</span><span class="sxs-lookup"><span data-stu-id="1738f-193">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![görüntü](intro/_static/labelattr.png)

<span data-ttu-id="1738f-195">IntelliSense deyim tamamlamada seçilen değeri deyimiyle tamamlamak için SEKME tuşunu girmenizi sağlar:</span><span class="sxs-lookup"><span data-stu-id="1738f-195">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![görüntü](intro/_static/stmtcomplete.png)

<span data-ttu-id="1738f-197">Bir etiketi yardımcı öznitelik girildikten hemen sonra etiketi ve özniteliği yazı tiplerini değiştirme.</span><span class="sxs-lookup"><span data-stu-id="1738f-197">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="1738f-198">Varsayılan Visual Studio "Mavi" veya "Açık" renk teması kullanarak yazı tipinin kalın mor renkte.</span><span class="sxs-lookup"><span data-stu-id="1738f-198">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="1738f-199">"Koyu" tema kullanıyorsanız, koyu Deniz Mavisi yazı tipidir.</span><span class="sxs-lookup"><span data-stu-id="1738f-199">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="1738f-200">Bu belgedeki görüntüleri varsayılan tema kullanılarak alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1738f-200">The images in this document were taken using the default theme.</span></span>

![görüntü](intro/_static/labelaspfor2.png)

<span data-ttu-id="1738f-202">Visual Studio girebilirsiniz *CompleteWord* kısayol (Ctrl + Ara çubuğu olan [varsayılan](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) çift tırnak işareti içinde (""), ve bir C# sınıfta olması gibi C# dilinde sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1738f-202">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="1738f-203">IntelliSense, sayfa modeli üzerinde tüm yöntemleri ve özellikleri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1738f-203">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="1738f-204">Özellik türü olduğundan yöntemleri ve özellikleri kullanılabilir `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="1738f-204">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="1738f-205">Aşağıdaki görüntüde, ı düzenleme `Register` görünümü, bu nedenle `RegisterViewModel` kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1738f-205">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![görüntü](intro/_static/intellemail.png)

<span data-ttu-id="1738f-207">IntelliSense özellikleri ve yöntemleri bir Modeli'ne sayfasında kullanılabilir listeler.</span><span class="sxs-lookup"><span data-stu-id="1738f-207">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="1738f-208">Zengin IntelliSense ortam CSS sınıfının seçmenize yardımcı olur:</span><span class="sxs-lookup"><span data-stu-id="1738f-208">The rich IntelliSense environment helps you select the CSS class:</span></span>

![görüntü](intro/_static/iclass.png)

![görüntü](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="1738f-211">HTML Yardımcıları karşılaştırıldığında etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="1738f-211">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="1738f-212">Etiket Yardımcıları ekleme Razor görünümleri, HTML öğeleri için sırada [HTML Yardımcıları](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) yöntemleri HTML Razor görünümleri interspersed şekilde çağırılır.</span><span class="sxs-lookup"><span data-stu-id="1738f-212">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="1738f-213">CSS sınıfı "Başlık" ile bir HTML etiketi oluşturur aşağıdaki Razor işaretlemesi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="1738f-213">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="1738f-214">Konumunda (`@`) sembolü söyler Razor kod başlangıcını budur.</span><span class="sxs-lookup"><span data-stu-id="1738f-214">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="1738f-215">Sonraki iki parametre ("FirstName" ve "ad:"), bu nedenle dizelerdir [IntelliSense](/visualstudio/ide/using-intellisense) yardımcı olamaz.</span><span class="sxs-lookup"><span data-stu-id="1738f-215">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="1738f-216">Son bağımsız değişken:</span><span class="sxs-lookup"><span data-stu-id="1738f-216">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="1738f-217">Bir anonim nesnenin öznitelikleri temsil etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1738f-217">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="1738f-218">Çünkü `class` ayrılmış bir anahtar sözcük C#, kullandığınız `@` zorlamak için Sembol C# yorumlamak için `@class=` sembol (özellik adı) olarak.</span><span class="sxs-lookup"><span data-stu-id="1738f-218">Because `class` is a reserved keyword in C#, you use the `@` symbol to force C# to interpret `@class=` as a symbol (property name).</span></span> <span data-ttu-id="1738f-219">Ön uç tasarımcıya (birisi HTML/CSS/JavaScript ve diğer istemci teknolojileri hakkında bilgi sahibi ancak C# ve Razor ile tanıdık), satır çoğunu yabancı.</span><span class="sxs-lookup"><span data-stu-id="1738f-219">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="1738f-220">Tüm satırı IntelliSense hiçbir yardımıyla yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1738f-220">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="1738f-221">Kullanarak `LabelTagHelper`, aynı biçimlendirme olarak yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="1738f-221">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

```cshtml
<label class="caption" asp-for="FirstName"></label>
```

<span data-ttu-id="1738f-222">Etiket Yardımcısı sürümüyle girdiğiniz hemen sonra `<l` Visual Studio düzenleyicisinde, IntelliSense eşleşen öğeleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="1738f-222">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![görüntü](intro/_static/label.png)

<span data-ttu-id="1738f-224">IntelliSense, tüm satırı yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="1738f-224">IntelliSense helps you write the entire line.</span></span>

<span data-ttu-id="1738f-225">Aşağıdaki kodu görüntüsünü Form bölümü gösterilmektedir *Views/Account/Register.cshtml* Visual Studio'ya dahil edildi ASP.NET 4.5.x MVC şablonu oluşturulmuş Razor görünüm.</span><span class="sxs-lookup"><span data-stu-id="1738f-225">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the ASP.NET 4.5.x MVC template included with Visual Studio.</span></span>

![görüntü](intro/_static/regCS.png)

<span data-ttu-id="1738f-227">Visual Studio Düzenleyicisi, C# gri arka plan kodu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1738f-227">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="1738f-228">Örneğin, `AntiForgeryToken` HTML Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="1738f-228">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="1738f-229">gri arka plan görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1738f-229">is displayed with a grey background.</span></span> <span data-ttu-id="1738f-230">Kayıt görünümünde biçimlendirme çoğunu olan C#.</span><span class="sxs-lookup"><span data-stu-id="1738f-230">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="1738f-231">Bu etiket Yardımcıları kullanarak eşdeğer bir yaklaşım karşılaştırın:</span><span class="sxs-lookup"><span data-stu-id="1738f-231">Compare that to the equivalent approach using Tag Helpers:</span></span>

![görüntü](intro/_static/regTH.png)

<span data-ttu-id="1738f-233">Biçimlendirme, çok daha net ve okuma, düzenlemek ve sürdürmek için HTML Yardımcıları yaklaşım kolay.</span><span class="sxs-lookup"><span data-stu-id="1738f-233">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="1738f-234">C# kodu sunucu hakkında bilmesi gereken minimum azaltılır.</span><span class="sxs-lookup"><span data-stu-id="1738f-234">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="1738f-235">Visual Studio Düzenleyicisi, farklı bir yazı tipi etiket Yardımcısı tarafından hedeflenen biçimlendirme görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1738f-235">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="1738f-236">Göz önünde bulundurun *e-posta* Grup:</span><span class="sxs-lookup"><span data-stu-id="1738f-236">Consider the *Email* group:</span></span>

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="1738f-237">Her biri "asp-" öznitelikler "Email" değerine sahiptir ancak "Email" bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="1738f-237">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="1738f-238">Bu bağlamda "Email" C# model ifade özelliğidir `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="1738f-238">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="1738f-239">Visual Studio Düzenleyicisi yazmanıza yardımcı olur. **tüm** sağlarken, Visual Studio, HTML Yardımcıları yaklaşım kodun çoğu Yardım sağlar. kayıt formun etiketi Yardımcısı yaklaşımda biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="1738f-239">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="1738f-240">[Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) Visual Studio Düzenleyicisi'nde etiket Yardımcıları ile çalışma konusunda ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="1738f-240">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="1738f-241">Web sunucu denetimlerine kıyasla etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="1738f-241">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="1738f-242">Etiket Yardımcıları, ilişkili oldukları öğesi sahip değil; Bunlar, yalnızca öğe ve içeriği işlemede katılın.</span><span class="sxs-lookup"><span data-stu-id="1738f-242">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="1738f-243">ASP.NET [Web sunucusu denetimleri](https://msdn.microsoft.com/library/7698y1f0.aspx) bildirildi ve bir sayfada çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1738f-243">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="1738f-244">[Web sunucusu denetimleri](https://msdn.microsoft.com/library/zsyt68f1.aspx) geliştirmeye ve hata ayıklamayı zorlaştırabilir Önemsiz olmayan bir yaşam döngüleri vardır.</span><span class="sxs-lookup"><span data-stu-id="1738f-244">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="1738f-245">Web sunucusu denetimleri istemci denetimi kullanarak istemci belge nesne modeli (DOM) öğelerine işlevselliğini eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1738f-245">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="1738f-246">Etiket Yardımcıları hiçbir yerli sahip</span><span class="sxs-lookup"><span data-stu-id="1738f-246">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="1738f-247">Web sunucusu denetimleri otomatik tarayıcı Algılama'yı içerir.</span><span class="sxs-lookup"><span data-stu-id="1738f-247">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="1738f-248">Etiket Yardımcıları tarayıcı hiçbir bilgiye sahip.</span><span class="sxs-lookup"><span data-stu-id="1738f-248">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="1738f-249">Aynı öğede birden çok etiket Yardımcıları davranabilir (bkz [çakışmaları önleme etiketi Yardımcısı](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) Web sunucusu denetimleri genellikle oluşturamazsınız ancak.</span><span class="sxs-lookup"><span data-stu-id="1738f-249">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="1738f-250">Etiket Yardımcıları için kapsamlı HTML öğelerinin içeriğini ve etiketi değiştirebilirsiniz ancak doğrudan bir sayfada bir şeyi değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="1738f-250">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="1738f-251">Web sunucusu denetimleri daha az belirli bir kapsama sahip ve diğer sayfanızı bölümlerini etkileyen eylemler gerçekleştirebilir; istenmeyen yan etkileri etkinleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="1738f-251">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="1738f-252">Web sunucusu denetimleri, dizeleri nesnelerine dönüştürmek için tür dönüştürücüleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="1738f-252">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="1738f-253">Tür dönüştürme yapmak zorunda kalmazsınız etiket Yardımcıları ile yerel olarak C# ile çalışırsınız.</span><span class="sxs-lookup"><span data-stu-id="1738f-253">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="1738f-254">Web sunucusu denetimleri kullanın [System.ComponentModel](/dotnet/api/system.componentmodel) bileşenlerin ve denetimlerin çalışma zamanı ve tasarım zamanı davranışını uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="1738f-254">Web Server controls use [System.ComponentModel](/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="1738f-255">`System.ComponentModel` temel sınıflar ve öznitelikler ve tür dönüştürücüleri uygulama, veri kaynaklarını bağlama ve bileşenleri lisanslama arabirimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="1738f-255">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="1738f-256">Genellikle türetilmesi etiket Yardımcıları için Karşıtlık `TagHelper`ve `TagHelper` temel sınıfı yalnızca iki yöntem sunar `Process` ve `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="1738f-256">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="1738f-257">Etiket Yardımcısı öğesi yazı tipi özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1738f-257">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="1738f-258">Gelen renklendirme ve yazı tipini özelleştirebilirsiniz **Araçları** > **seçenekleri** > **ortam** > **yazı tipleri ve renkler**:</span><span class="sxs-lookup"><span data-stu-id="1738f-258">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![görüntü](intro/_static/fontoptions2.png)

[!INCLUDE[](~/includes/built-in-TH.md)]

## <a name="additional-resources"></a><span data-ttu-id="1738f-260">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1738f-260">Additional resources</span></span>

* [<span data-ttu-id="1738f-261">Yazma etiketi Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="1738f-261">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="1738f-262">Formlarla Çalışma</span><span class="sxs-lookup"><span data-stu-id="1738f-262">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* <span data-ttu-id="1738f-263">[GitHub üzerinde TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) ile çalışmak için etiket Yardımcısı örnekler içeren [önyükleme](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="1738f-263">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](https://getbootstrap.com/).</span></span>
