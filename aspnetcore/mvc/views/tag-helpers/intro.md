---
title: ASP.NET core'da etiket Yardımcıları
author: rick-anderson
description: Etiket Yardımcıları nelerdir ve bunların içinde ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2018
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: c2af9099fe439e1cdbf9ba86ffae3b2b0f67391e
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757069"
---
# <a name="tag-helpers-in-aspnet-core"></a><span data-ttu-id="178b4-103">ASP.NET core'da etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="178b4-103">Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="178b4-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="178b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="178b4-105">Etiket Yardımcıları nelerdir?</span><span class="sxs-lookup"><span data-stu-id="178b4-105">What are Tag Helpers?</span></span>

<span data-ttu-id="178b4-106">Etiket Yardımcıları oluşturma ve Razor dosyalarında HTML öğeleri işleme katılmak sunucu tarafı kodu etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="178b4-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="178b4-107">Örneğin, yerleşik `ImageTagHelper` görüntü adı için bir sürüm numarası ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="178b4-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="178b4-108">Görüntü değiştiğinde geçerli resim (yerine, eski bir önbelleğe alınan görüntü) almak için istemcilerini garanti için sunucu görüntüsü için benzersiz yeni bir sürümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="178b4-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="178b4-109">Birçok yerleşik etiket Yardımcıları için formlar, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi ortak görevler - vardır.</span><span class="sxs-lookup"><span data-stu-id="178b4-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="178b4-110">Etiket Yardımcıları C# dilinde yazılmış ve öğe adı, öznitelik adı veya üst etiketi dayalı HTML öğeleri hedeflenir.</span><span class="sxs-lookup"><span data-stu-id="178b4-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="178b4-111">Örneğin, yerleşik `LabelTagHelper` HTML hedefleyebilir `<label>` öğesi olduğunda `LabelTagHelper` öznitelikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="178b4-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="178b4-112">Aşina değilseniz [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), HTML ve C# arasında açık geçişler Razor görünümleri, etiket Yardımcıları azaltın.</span><span class="sxs-lookup"><span data-stu-id="178b4-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="178b4-113">Çoğu durumda, HTML Yardımcıları, belirli bir etiketi Yardımcısı için alternatif bir yaklaşım sağlar, ancak etiket Yardımcıları HTML Yardımcıları değiştirin yoktur ve her HTML Yardımcısı için bir etiket Yardımcısı olmadığını bilmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="178b4-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="178b4-114">[Etiket Yardımcıları için HTML Yardımcıları karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="178b4-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="178b4-115">Etiket Yardımcıları neler sağlar</span><span class="sxs-lookup"><span data-stu-id="178b4-115">What Tag Helpers provide</span></span>

<span data-ttu-id="178b4-116">**Bir HTML kullanımı kolay geliştirme deneyimi** en bölümü için etiket Yardımcıları kullanarak Razor işaretlemesi standart HTML gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="178b4-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="178b4-117">HTML/CSS/JavaScript ile conversant ön uç tasarımcılar #c Razor sözdizimi öğrenmeden Razor düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="178b4-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="178b4-118">**HTML ve Razor biçimlendirme oluşturmak için zengin IntelliSense ortamı** sharp Karşıtlık HTML Yardımcıları, sunucu tarafı Razor görünümleri işaretlemede oluşturulmasını önceki yaklaşım budur.</span><span class="sxs-lookup"><span data-stu-id="178b4-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="178b4-119">[Etiket Yardımcıları için HTML Yardımcıları karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="178b4-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="178b4-120">[Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) IntelliSense ortamı açıklar.</span><span class="sxs-lookup"><span data-stu-id="178b4-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="178b4-121">C# Razor işaretlemesi yazmaktan etiket Yardımcıları kullanarak daha üretken bile geliştiricilerin Razor C# sözdizimi ile karşılaştı.</span><span class="sxs-lookup"><span data-stu-id="178b4-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="178b4-122">**Daha üretken ve üretmek için yaptığınız daha sağlam, güvenilir şekilde ve yalnızca sunucu üzerinde bilgileriyle Bakımı yapılabilir kodu** Örneğin, geçmişte görüntüleri güncelleştirme mantra değiştirdiğinizde, görüntünün adını değiştirmek için oluştu Resim.</span><span class="sxs-lookup"><span data-stu-id="178b4-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="178b4-123">Görüntüleri performansla ilgili nedenlerden dolayı agresif önbelleğe alınması gerektiğini ve görüntü adını değiştirmediğiniz sürece, eski bir kopyasını alma istemciler risk.</span><span class="sxs-lookup"><span data-stu-id="178b4-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="178b4-124">Tarihsel olarak, görüntü düzenlendi sonra adı değiştirilmesi gerekti ve web uygulamasında görüntü her başvuru güncelleştirilmesi gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="178b4-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="178b4-125">Yalnızca bu, bu da hataya (, bir başvurusu eksik olabilir, yanlışlıkla girin yanlış dize, vb.) yoğundur çok emek mi Yerleşik `ImageTagHelper` sizin için otomatik olarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="178b4-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="178b4-126">`ImageTagHelper` Bir sürüm ekleyebilir görüntü değiştiğinde sunucu görüntüsü için benzersiz yeni bir sürümü otomatik olarak oluşturur. Bu nedenle sayı görüntü adı için.</span><span class="sxs-lookup"><span data-stu-id="178b4-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="178b4-127">İstemciler, geçerli görüntü almak için garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="178b4-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="178b4-128">Kullanarak bu sağlamlık ve iş Pazarı tasarruf temelde ücretsiz gelen `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="178b4-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="178b4-129">En çok yerleşik etiket Yardımcıları standart HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="178b4-129">Most built-in Tag Helpers target standard HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="178b4-130">Örneğin, `<input>` birçok görünümlerde kullanılan öğe *görünümler/hesap* klasörde `asp-for` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="178b4-130">For example, the `<input>` element used in many views in the *Views/Account* folder contains the `asp-for` attribute.</span></span> <span data-ttu-id="178b4-131">Bu öznitelik, İşlenmiş HTML belirtilen model özelliğin adını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="178b4-131">This attribute extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="178b4-132">Bir Razor görünüm aşağıdaki modeliyle göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="178b4-132">Consider a Razor view with the following model:</span></span>

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

<span data-ttu-id="178b4-133">Aşağıdaki Razor işaretlemesi:</span><span class="sxs-lookup"><span data-stu-id="178b4-133">The following Razor markup:</span></span>

```cshtml
<label asp-for="Movie.Title"></label>
```

<span data-ttu-id="178b4-134">Aşağıdaki HTML'yi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="178b4-134">Generates the following HTML:</span></span>

```html
<label for="Movie_Title">Title</label>
```

<span data-ttu-id="178b4-135">`asp-for` Özniteliği tarafından kullanılabilir yapılan `For` özelliğinde [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="178b4-135">The `asp-for` attribute is made available by the `For` property in the [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span></span> <span data-ttu-id="178b4-136">Bkz: [Yazar etiket Yardımcıları](xref:mvc/views/tag-helpers/authoring) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="178b4-136">See [Author Tag Helpers](xref:mvc/views/tag-helpers/authoring) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="178b4-137">Kapsam etiketi Yardımcısı'nı yönetme</span><span class="sxs-lookup"><span data-stu-id="178b4-137">Managing Tag Helper scope</span></span>

<span data-ttu-id="178b4-138">Etiket Yardımcıları kapsamı, bir birleşimiyle denetlenir `@addTagHelper`, `@removeTagHelper`ve "!" çevirme karakter.</span><span class="sxs-lookup"><span data-stu-id="178b4-138">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="178b4-139">`@addTagHelper` Etiket Yardımcıları kullanılabilir hale getirir</span><span class="sxs-lookup"><span data-stu-id="178b4-139">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="178b4-140">Adlı yeni bir ASP.NET Core web uygulaması oluşturursanız *AuthoringTagHelpers*, aşağıdaki *Views/_ViewImports.cshtml* dosyası projenize eklenecek:</span><span class="sxs-lookup"><span data-stu-id="178b4-140">If you create a new ASP.NET Core web app named *AuthoringTagHelpers*, the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="178b4-141">`@addTagHelper` Yönergesi etiket Yardımcıları için görüntüleme kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="178b4-141">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="178b4-142">Bu durumda, görünüm dosyasıdır *Pages/_ViewImports.cshtml*, varsayılan olarak tüm dosyaları tarafından devralınır *sayfaları* klasörü ve alt klasörleri; etiket Yardımcıları kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="178b4-142">In this case, the view file is *Pages/_ViewImports.cshtml*, which by default is inherited by all files in the *Pages* folder and sub-folders; making Tag Helpers available.</span></span> <span data-ttu-id="178b4-143">Yukarıdaki kod joker karakter sözdizimini kullanır ("\*") olduğunu belirtmek için belirtilen derlemedeki tüm etiket Yardımcıları (*Microsoft.AspNetCore.Mvc.TagHelpers*) her bir görünüm dosyanın çalıştırılabilecek *görünümleri* dizin veya alt dizini.</span><span class="sxs-lookup"><span data-stu-id="178b4-143">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="178b4-144">İlk parametresinden sonra `@addTagHelper` yüklemek için etiket Yardımcıları belirtir (kullanıyoruz "\*" tüm etiket Yardımcıları için), ve ikinci parametre "Microsoft.AspNetCore.Mvc.TagHelpers" etiket Yardımcıları içeren derlemeyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="178b4-144">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="178b4-145">*Microsoft.AspNetCore.Mvc.TagHelpers* yerleşik ASP.NET Core etiket Yardımcıları için derleme.</span><span class="sxs-lookup"><span data-stu-id="178b4-145">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="178b4-146">Etiket Yardımcıları bu projedeki tüm kullanıma sunmak için (adlı bir derleme oluşturur *AuthoringTagHelpers*), aşağıdakileri kullanmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="178b4-146">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="178b4-147">Projeniz varsa bir `EmailTagHelper` varsayılan ad alanı (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), etiket Yardımcısı'nın tam adı (FQN) sağlayabilir:</span><span class="sxs-lookup"><span data-stu-id="178b4-147">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="178b4-148">Etiket Yardımcısı kullanarak bir FQN görünümüne eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="178b4-148">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="178b4-149">Çoğu geliştirici kullanmayı tercih eder "\*" joker karakter sözdizimini.</span><span class="sxs-lookup"><span data-stu-id="178b4-149">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="178b4-150">Joker karakter eklemek joker karakter sözdizimini sağlar "\*" bir FQN soneki olarak.</span><span class="sxs-lookup"><span data-stu-id="178b4-150">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="178b4-151">Örneğin, aşağıdaki yönergeleri hiçbirini getirecek `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="178b4-151">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="178b4-152">Daha önce belirtildiği gibi ekleme `@addTagHelper` yönergesini *Views/_ViewImports.cshtml* dosya etiketi Yardımcısı tüm görünüm dosyalarında kullanıma sunar *görünümleri* dizin ve alt dizinleri.</span><span class="sxs-lookup"><span data-stu-id="178b4-152">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="178b4-153">Kullanabileceğiniz `@addTagHelper` istediğinizde bu görünümleri için etiket Yardımcısı kullanıma sunmak için katılım belirli görünüm dosyalarında yönergesi.</span><span class="sxs-lookup"><span data-stu-id="178b4-153">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="178b4-154">`@removeTagHelper` Etiket Yardımcıları kaldırır</span><span class="sxs-lookup"><span data-stu-id="178b4-154">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="178b4-155">`@removeTagHelper` Olarak aynı iki parametreye sahip `@addTagHelper`, ve daha önce eklediğiniz etiket Yardımcısı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="178b4-155">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="178b4-156">Örneğin, `@removeTagHelper` görünümünden belirli görünüm kaldırır belirtilen etiketi Yardımcısı uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="178b4-156">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="178b4-157">Kullanarak `@removeTagHelper` içinde bir *Views/Folder/_ViewImports.cshtml* dosyasını kaldırır belirtilen etiketi Yardımcısı tüm görünümlerde *klasör*.</span><span class="sxs-lookup"><span data-stu-id="178b4-157">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="178b4-158">Etiket Yardımcısı kapsamlı denetleme *_viewımports.cshtml* dosyası</span><span class="sxs-lookup"><span data-stu-id="178b4-158">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="178b4-159">Ekleyebileceğiniz bir *_viewımports.cshtml* herhangi bir görünüm klasörü ve görünüm altyapısı yönergeleri Bu iki dosyadan uygulanır ve *Views/_ViewImports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="178b4-159">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="178b4-160">Boş bir eklediyseniz *Views/Home/_ViewImports.cshtml* dosya *giriş* görünümleri olduğundan değişiklik olur *_viewımports.cshtml* dosya eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="178b4-160">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="178b4-161">Tüm `@addTagHelper` eklemek için yönergeleri *Views/Home/_ViewImports.cshtml* dosyası (varsayılan olmayan *Views/_ViewImports.cshtml* dosya) Bu etiket Yardımcıları görünümlere kullanılabilmesini yalnızca *Giriş* klasör.</span><span class="sxs-lookup"><span data-stu-id="178b4-161">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="178b4-162">Tek tek öğelerin seçim yapma</span><span class="sxs-lookup"><span data-stu-id="178b4-162">Opting out of individual elements</span></span>

<span data-ttu-id="178b4-163">Etiket Yardımcısı etiketi Yardımcısı çevirme karakteri ile öğe düzeyinde devre dışı bırakabilirsiniz ("!").</span><span class="sxs-lookup"><span data-stu-id="178b4-163">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="178b4-164">Örneğin, `Email` doğrulama devre dışı `<span>` etiketi Yardımcısı çevirme karakteri ile:</span><span class="sxs-lookup"><span data-stu-id="178b4-164">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="178b4-165">Etiket Yardımcısı çevirme karakterin açılış ve kapanış etiketi uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="178b4-165">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="178b4-166">(Bir açma etiketi eklediğinizde Visual Studio Düzenleyicisi'ni otomatik olarak geri çevirme karakter için kapanış etiketi ekler).</span><span class="sxs-lookup"><span data-stu-id="178b4-166">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="178b4-167">Vazgeçme karakter ekledikten sonra etiket Yardımcısı öznitelikleri ve öğe artık farklı bir yazı tipinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="178b4-167">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="178b4-168">Kullanarak `@tagHelperPrefix` etiketi Yardımcısı kullanım açık hale getirmek için</span><span class="sxs-lookup"><span data-stu-id="178b4-168">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="178b4-169">`@tagHelperPrefix` Yönergesi, etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık hale getirmek için bir etiket önek dizesi belirtmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="178b4-169">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="178b4-170">Örneğin, aşağıdaki işaretlemede ekleyebilirsiniz *Views/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="178b4-170">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="178b4-171">Kod aşağıdaki resimde, etiket Yardımcısı ön ek ayarlanır `th:`, bu nedenle yalnızca önekini kullanarak öğeleri `th:` etiket Yardımcıları (etiket Yardımcısı etkin öğeleri sahip farklı bir yazı tipi) destekler.</span><span class="sxs-lookup"><span data-stu-id="178b4-171">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="178b4-172">`<label>` Ve `<input>` öğeleri etiketi Yardımcısı önekine sahip ve etiket Yardımcısı etkinleştirilmiş, çalışırken `<span>` öğesi değil.</span><span class="sxs-lookup"><span data-stu-id="178b4-172">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![görüntü](intro/_static/thp.png)

<span data-ttu-id="178b4-174">Geçerli aynı hiyerarşi kuralları `@addTagHelper` için de geçerli `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="178b4-174">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="178b4-175">Etiket Yardımcıları için IntelliSense desteği</span><span class="sxs-lookup"><span data-stu-id="178b4-175">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="178b4-176">Visual Studio'da yeni bir ASP.NET Core web uygulaması oluşturduğunuzda, "Microsoft.AspNetCore.Razor.Tools" NuGet paketini ekler.</span><span class="sxs-lookup"><span data-stu-id="178b4-176">When you create a new ASP.NET Core web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="178b4-177">Bu etiket Yardımcısı araçları ekleyen paketidir.</span><span class="sxs-lookup"><span data-stu-id="178b4-177">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="178b4-178">Bir HTML yazmayı düşünebilirsiniz `<label>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="178b4-178">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="178b4-179">Girdiğiniz hemen sonra `<l` Visual Studio düzenleyicisinde, IntelliSense eşleşen öğeleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="178b4-179">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![görüntü](intro/_static/label.png)

<span data-ttu-id="178b4-181">Yalnızca simgesi ancak HTML Yardımı alın ("@" symbol with "<>" altındaki).</span><span class="sxs-lookup"><span data-stu-id="178b4-181">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![görüntü](intro/_static/tagSym.png)

<span data-ttu-id="178b4-183">Etiket Yardımcıları tarafından hedeflenen öğe tanıtır.</span><span class="sxs-lookup"><span data-stu-id="178b4-183">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="178b4-184">Saf HTML öğeleri (gibi `fieldset`) "<>" simgesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="178b4-184">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="178b4-185">Saf bir HTML `<label>` etiketi kahverengi yazı tipi özniteliklerini kırmızı, HTML etiket (ile varsayılan Visual Studio renk teması) görüntüler ve öznitelik değerleri içinde mavi.</span><span class="sxs-lookup"><span data-stu-id="178b4-185">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![görüntü](intro/_static/LableHtmlTag.png)

<span data-ttu-id="178b4-187">Girdikten sonra `<label`, IntelliSense, kullanılabilir HTML/CSS öznitelikleri ve etiket Yardımcısı hedeflemeli öznitelikler listelenir:</span><span class="sxs-lookup"><span data-stu-id="178b4-187">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![görüntü](intro/_static/labelattr.png)

<span data-ttu-id="178b4-189">IntelliSense deyim tamamlamada seçilen değeri deyimiyle tamamlamak için SEKME tuşunu girmenizi sağlar:</span><span class="sxs-lookup"><span data-stu-id="178b4-189">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![görüntü](intro/_static/stmtcomplete.png)

<span data-ttu-id="178b4-191">Bir etiketi yardımcı öznitelik girildikten hemen sonra etiketi ve özniteliği yazı tiplerini değiştirme.</span><span class="sxs-lookup"><span data-stu-id="178b4-191">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="178b4-192">Varsayılan Visual Studio "Mavi" veya "Açık" renk teması kullanarak yazı tipinin kalın mor renkte.</span><span class="sxs-lookup"><span data-stu-id="178b4-192">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="178b4-193">"Koyu" tema kullanıyorsanız, koyu Deniz Mavisi yazı tipidir.</span><span class="sxs-lookup"><span data-stu-id="178b4-193">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="178b4-194">Bu belgedeki görüntüleri varsayılan tema kullanılarak alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="178b4-194">The images in this document were taken using the default theme.</span></span>

![görüntü](intro/_static/labelaspfor2.png)

<span data-ttu-id="178b4-196">Visual Studio girebilirsiniz *CompleteWord* kısayol (Ctrl + Ara çubuğu olan [varsayılan](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) çift tırnak işareti içinde (""), ve bir C# sınıfta olması gibi C# dilinde sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="178b4-196">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="178b4-197">IntelliSense, sayfa modeli üzerinde tüm yöntemleri ve özellikleri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="178b4-197">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="178b4-198">Özellik türü olduğundan yöntemleri ve özellikleri kullanılabilir `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="178b4-198">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="178b4-199">Aşağıdaki görüntüde, ı düzenleme `Register` görünümü, bu nedenle `RegisterViewModel` kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="178b4-199">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![görüntü](intro/_static/intellemail.png)

<span data-ttu-id="178b4-201">IntelliSense özellikleri ve yöntemleri bir Modeli'ne sayfasında kullanılabilir listeler.</span><span class="sxs-lookup"><span data-stu-id="178b4-201">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="178b4-202">Zengin IntelliSense ortam CSS sınıfının seçmenize yardımcı olur:</span><span class="sxs-lookup"><span data-stu-id="178b4-202">The rich IntelliSense environment helps you select the CSS class:</span></span>

![görüntü](intro/_static/iclass.png)

![görüntü](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="178b4-205">HTML Yardımcıları karşılaştırıldığında etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="178b4-205">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="178b4-206">Etiket Yardımcıları ekleme Razor görünümleri, HTML öğeleri için sırada [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) yöntemleri HTML Razor görünümleri interspersed şekilde çağırılır.</span><span class="sxs-lookup"><span data-stu-id="178b4-206">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="178b4-207">CSS sınıfı "Başlık" ile bir HTML etiketi oluşturur aşağıdaki Razor işaretlemesi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="178b4-207">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="178b4-208">Konumunda (`@`) sembolü söyler Razor kod başlangıcını budur.</span><span class="sxs-lookup"><span data-stu-id="178b4-208">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="178b4-209">Sonraki iki parametre ("FirstName" ve "ad:"), bu nedenle dizelerdir [IntelliSense](/visualstudio/ide/using-intellisense) yardımcı olamaz.</span><span class="sxs-lookup"><span data-stu-id="178b4-209">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="178b4-210">Son bağımsız değişken:</span><span class="sxs-lookup"><span data-stu-id="178b4-210">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="178b4-211">Bir anonim nesnenin öznitelikleri temsil etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="178b4-211">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="178b4-212">Çünkü <strong>sınıfı</strong> ayrılmış bir anahtar sözcük C# ' ta, kullandığınız `@` yorumlamak için C# zorlamak için Sembol "@class=" (özellik adı) sembol olarak.</span><span class="sxs-lookup"><span data-stu-id="178b4-212">Because <strong>class</strong> is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="178b4-213">Ön uç tasarımcıya (birisi HTML/CSS/JavaScript ve diğer istemci teknolojileri hakkında bilgi sahibi ancak C# ve Razor ile tanıdık), satır çoğunu yabancı.</span><span class="sxs-lookup"><span data-stu-id="178b4-213">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="178b4-214">Tüm satırı IntelliSense hiçbir yardımıyla yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="178b4-214">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="178b4-215">Kullanarak `LabelTagHelper`, aynı biçimlendirme olarak yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="178b4-215">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![görüntü](intro/_static/label2.png)

<span data-ttu-id="178b4-217">Etiket Yardımcısı sürümüyle girdiğiniz hemen sonra `<l` Visual Studio düzenleyicisinde, IntelliSense eşleşen öğeleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="178b4-217">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![görüntü](intro/_static/label.png)

<span data-ttu-id="178b4-219">IntelliSense, tüm satırı yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="178b4-219">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="178b4-220">`LabelTagHelper` İçeriğine ayarını da varsayılanları `asp-for` öznitelik değeri ("FirstName") "İlk adı için"; Özellik adı her yeni büyük harf oluştuğu boşluk oluşan bir cümle başlamalıdır özellikleri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="178b4-220">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="178b4-221">Aşağıdaki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="178b4-221">In the following markup:</span></span>

![görüntü](intro/_static/label2.png)

<span data-ttu-id="178b4-223">oluşturur:</span><span class="sxs-lookup"><span data-stu-id="178b4-223">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="178b4-224">İçeriği eklerseniz başlamalıdır cümle büyük küçük harfleri içeriğe kullanılmayan `<label>`.</span><span class="sxs-lookup"><span data-stu-id="178b4-224">The camel-cased to sentence-cased content isn't used if you add content to the `<label>`.</span></span> <span data-ttu-id="178b4-225">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="178b4-225">For example:</span></span>

![görüntü](intro/_static/1stName.png)

<span data-ttu-id="178b4-227">oluşturur:</span><span class="sxs-lookup"><span data-stu-id="178b4-227">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="178b4-228">Aşağıdaki kodu görüntüsünü Form bölümü gösterilmektedir *Views/Account/Register.cshtml* eski ASP.NET 4.5.x MVC şablonu ile Visual Studio 2015 dahil oluşturulmuş Razor görünüm.</span><span class="sxs-lookup"><span data-stu-id="178b4-228">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![görüntü](intro/_static/regCS.png)

<span data-ttu-id="178b4-230">Visual Studio Düzenleyicisi, C# gri arka plan kodu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="178b4-230">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="178b4-231">Örneğin, `AntiForgeryToken` HTML Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="178b4-231">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="178b4-232">gri arka plan görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="178b4-232">is displayed with a grey background.</span></span> <span data-ttu-id="178b4-233">Kayıt görünümünde biçimlendirme çoğunu olan C#.</span><span class="sxs-lookup"><span data-stu-id="178b4-233">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="178b4-234">Bu etiket Yardımcıları kullanarak eşdeğer bir yaklaşım karşılaştırın:</span><span class="sxs-lookup"><span data-stu-id="178b4-234">Compare that to the equivalent approach using Tag Helpers:</span></span>

![görüntü](intro/_static/regTH.png)

<span data-ttu-id="178b4-236">Biçimlendirme, çok daha net ve okuma, düzenlemek ve sürdürmek için HTML Yardımcıları yaklaşım kolay.</span><span class="sxs-lookup"><span data-stu-id="178b4-236">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="178b4-237">C# kodu sunucu hakkında bilmesi gereken minimum azaltılır.</span><span class="sxs-lookup"><span data-stu-id="178b4-237">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="178b4-238">Visual Studio Düzenleyicisi, farklı bir yazı tipi etiket Yardımcısı tarafından hedeflenen biçimlendirme görüntüler.</span><span class="sxs-lookup"><span data-stu-id="178b4-238">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="178b4-239">Göz önünde bulundurun *e-posta* Grup:</span><span class="sxs-lookup"><span data-stu-id="178b4-239">Consider the *Email* group:</span></span>

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="178b4-240">Her biri "asp-" öznitelikler "Email" değerine sahiptir ancak "Email" bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="178b4-240">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="178b4-241">Bu bağlamda "Email" C# model ifade özelliğidir `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="178b4-241">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="178b4-242">Visual Studio Düzenleyicisi yazmanıza yardımcı olur. **tüm** sağlarken, Visual Studio, HTML Yardımcıları yaklaşım kodun çoğu Yardım sağlar. kayıt formun etiketi Yardımcısı yaklaşımda biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="178b4-242">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="178b4-243">[Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) Visual Studio Düzenleyicisi'nde etiket Yardımcıları ile çalışma konusunda ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="178b4-243">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="178b4-244">Web sunucu denetimlerine kıyasla etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="178b4-244">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="178b4-245">Etiket Yardımcıları, ilişkili oldukları öğesi sahip değil; Bunlar, yalnızca öğe ve içeriği işlemede katılın.</span><span class="sxs-lookup"><span data-stu-id="178b4-245">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="178b4-246">ASP.NET [Web sunucusu denetimleri](https://msdn.microsoft.com/library/7698y1f0.aspx) bildirildi ve bir sayfada çağrılır.</span><span class="sxs-lookup"><span data-stu-id="178b4-246">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="178b4-247">[Web sunucusu denetimleri](https://msdn.microsoft.com/library/zsyt68f1.aspx) geliştirmeye ve hata ayıklamayı zorlaştırabilir Önemsiz olmayan bir yaşam döngüleri vardır.</span><span class="sxs-lookup"><span data-stu-id="178b4-247">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="178b4-248">Web sunucusu denetimleri istemci denetimi kullanarak istemci belge nesne modeli (DOM) öğelerine işlevselliğini eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="178b4-248">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="178b4-249">Etiket Yardımcıları hiçbir yerli sahip</span><span class="sxs-lookup"><span data-stu-id="178b4-249">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="178b4-250">Web sunucusu denetimleri otomatik tarayıcı Algılama'yı içerir.</span><span class="sxs-lookup"><span data-stu-id="178b4-250">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="178b4-251">Etiket Yardımcıları tarayıcı hiçbir bilgiye sahip.</span><span class="sxs-lookup"><span data-stu-id="178b4-251">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="178b4-252">Aynı öğede birden çok etiket Yardımcıları davranabilir (bkz [çakışmaları önleme etiketi Yardımcısı](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) Web sunucusu denetimleri genellikle oluşturamazsınız ancak.</span><span class="sxs-lookup"><span data-stu-id="178b4-252">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="178b4-253">Etiket Yardımcıları için kapsamlı HTML öğelerinin içeriğini ve etiketi değiştirebilirsiniz ancak doğrudan bir sayfada bir şeyi değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="178b4-253">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="178b4-254">Web sunucusu denetimleri daha az belirli bir kapsama sahip ve diğer sayfanızı bölümlerini etkileyen eylemler gerçekleştirebilir; istenmeyen yan etkileri etkinleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="178b4-254">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="178b4-255">Web sunucusu denetimleri, dizeleri nesnelerine dönüştürmek için tür dönüştürücüleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="178b4-255">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="178b4-256">Tür dönüştürme yapmak zorunda kalmazsınız etiket Yardımcıları ile yerel olarak C# ile çalışırsınız.</span><span class="sxs-lookup"><span data-stu-id="178b4-256">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="178b4-257">Web sunucusu denetimleri kullanın [System.ComponentModel](/dotnet/api/system.componentmodel) bileşenlerin ve denetimlerin çalışma zamanı ve tasarım zamanı davranışını uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="178b4-257">Web Server controls use [System.ComponentModel](/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="178b4-258">`System.ComponentModel` temel sınıflar ve öznitelikler ve tür dönüştürücüleri uygulama, veri kaynaklarını bağlama ve bileşenleri lisanslama arabirimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="178b4-258">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="178b4-259">Genellikle türetilmesi etiket Yardımcıları için Karşıtlık `TagHelper`ve `TagHelper` temel sınıfı yalnızca iki yöntem sunar `Process` ve `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="178b4-259">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="178b4-260">Etiket Yardımcısı öğesi yazı tipi özelleştirme</span><span class="sxs-lookup"><span data-stu-id="178b4-260">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="178b4-261">Gelen renklendirme ve yazı tipini özelleştirebilirsiniz **Araçları** > **seçenekleri** > **ortam** > **yazı tipleri ve renkler**:</span><span class="sxs-lookup"><span data-stu-id="178b4-261">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![görüntü](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="178b4-263">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="178b4-263">Additional resources</span></span>

* [<span data-ttu-id="178b4-264">Yazma etiketi Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="178b4-264">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="178b4-265">Formlarla çalışma </span><span class="sxs-lookup"><span data-stu-id="178b4-265">Working with Forms </span></span>](xref:mvc/views/working-with-forms)
* <span data-ttu-id="178b4-266">[GitHub üzerinde TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) ile çalışmak için etiket Yardımcısı örnekler içeren [önyükleme](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="178b4-266">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>
