---
title: "ASP.NET Core içinde etiket Yardımcıları"
author: rick-anderson
description: "Etiket Yardımcıları nelerdir ve bunları ASP.NET Core nasıl kullanacağınızı öğrenin."
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c198ccc3e3e2c11f3e2b9379bc63bd6428dbf69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="0e489-103">ASP.NET Core içinde etiket Yardımcıları giriş</span><span class="sxs-lookup"><span data-stu-id="0e489-103">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="0e489-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0e489-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="0e489-105">Etiket Yardımcıları nelerdir?</span><span class="sxs-lookup"><span data-stu-id="0e489-105">What are Tag Helpers?</span></span>

<span data-ttu-id="0e489-106">Etiket Yardımcıları sunucu tarafı kodu oluşturma ve Razor dosyalarında HTML öğelerin işlenmesi katılmayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0e489-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="0e489-107">Örneğin, yerleşik `ImageTagHelper` görüntü adı için bir sürüm numarası ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0e489-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="0e489-108">Görüntünün her değiştiğinde, istemciler (eski önbelleğe alınmış bir görüntü) yerine geçerli görüntü alma garanti şekilde sunucu görüntüsü için yeni bir benzersiz sürümünü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0e489-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="0e489-109">Formları, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi genel görevleri - birçok yerleşik etiket Yardımcılarında vardır.</span><span class="sxs-lookup"><span data-stu-id="0e489-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="0e489-110">Etiket Yardımcıları C# dilinde yazılan ve öğe adı, öznitelik adı veya üst etiket göre HTML öğeleri hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="0e489-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="0e489-111">Örneğin, yerleşik `LabelTagHelper` HTML hedefleyebilirsiniz `<label>` öğesi zaman `LabelTagHelper` öznitelikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0e489-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="0e489-112">Hakkında bilginiz varsa [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), etiket Yardımcıları HTML ve C# arasında açık geçişler Razor görünümlerinde azaltın.</span><span class="sxs-lookup"><span data-stu-id="0e489-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="0e489-113">Çoğu durumda, HTML Yardımcıları alternatif bir yaklaşım için belirli bir etiket Yardımcısı sağlar, ancak etiket Yardımcıları HTML Yardımcıları Değiştir yoktur ve her HTML Yardımcısı için bir etiket Yardımcısı değil bilmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="0e489-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="0e489-114">[Etiket Yardımcıları için HTML Yardımcıları ile karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0e489-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="0e489-115">Neler etiket Yardımcıları sağlar</span><span class="sxs-lookup"><span data-stu-id="0e489-115">What Tag Helpers provide</span></span>

<span data-ttu-id="0e489-116">**Bir HTML kolay geliştirme deneyimi** gibi standart HTML etiket Yardımcıları kullanarak Razor biçimlendirmesi en bölümü için görünür.</span><span class="sxs-lookup"><span data-stu-id="0e489-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="0e489-117">Ön uç tasarımcıları HTML/CSS/JavaScript ile conversant, C# Razor sözdizimi öğrenmeden Razor düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0e489-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="0e489-118">**HTML ve Razor biçimlendirmesi oluşturmak için zengin bir IntelliSense ortam** HTML Yardımcıları, Razor görünümleri biçimlendirmede sunucu tarafı oluşturulmasını önceki yaklaşımı sharp Karşıtlık de budur.</span><span class="sxs-lookup"><span data-stu-id="0e489-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="0e489-119">[Etiket Yardımcıları için HTML Yardımcıları ile karşılaştırıldığında](#tag-helpers-compared-to-html-helpers) farklar daha ayrıntılı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0e489-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="0e489-120">[Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) IntelliSense ortamı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0e489-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="0e489-121">C# Razor biçimlendirme yazmaktan etiket Yardımcıları kullanarak daha üretken bile geliştiriciler Razor C# sözdizimi ile karşılaştı.</span><span class="sxs-lookup"><span data-stu-id="0e489-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="0e489-122">**Daha güçlü, güvenilir daha üretken ve üretmek mümkün kılmak için yolu ve yalnızca sunucu üzerinde bilgileriyle Bakımı yapılabilir kodu** Örneğin, geçmişte görüntüleri güncelleştirme mantra, görüntü adı değiştirdiğinizde edildi Resim.</span><span class="sxs-lookup"><span data-stu-id="0e489-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="0e489-123">Görüntüleri performans nedenleriyle titizlikle önbelleğe alınması gereken ve bir görüntü adı değiştirmediğiniz sürece, eski bir kopyasını alma istemcileri risk.</span><span class="sxs-lookup"><span data-stu-id="0e489-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="0e489-124">Tarihsel olarak, görüntünün düzenlendi sonra adı değiştirilmesi gerekti ve görüntünün web uygulamasında her başvuru güncelleştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="0e489-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="0e489-125">Yalnızca bu yoğun, aynı zamanda hataya (, bir başvurusu eksik, yanlışlıkla girin yanlış dize, vb.) olup çok emek mi Yerleşik `ImageTagHelper` sizin için otomatik olarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0e489-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="0e489-126">`ImageTagHelper` Bir sürüm ekleyebilirsiniz görüntü değiştiğinde sunucu görüntüsü için yeni bir benzersiz sürümü otomatik olarak oluşturur. böylece sayı görüntü adı.</span><span class="sxs-lookup"><span data-stu-id="0e489-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="0e489-127">Geçerli görüntü almak için istemcileri garanti.</span><span class="sxs-lookup"><span data-stu-id="0e489-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="0e489-128">Kullanarak bu sağlamlık ve işgücü tasarrufları temelde ücretsiz gelen `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="0e489-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="0e489-129">Yerleşik etiket Yardımcıları çoğu olan HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="0e489-129">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="0e489-130">Örneğin, `<input>` birçok görünümleri kullanılan öğe *görünümler/hesap* klasörde `asp-for` belirtilen model özelliğinin adı işlenmiş HTML'e ayıklar özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0e489-130">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="0e489-131">Aşağıdaki Razor biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="0e489-131">The following Razor markup:</span></span>

```cshtml
<label asp-for="Email"></label>
```

<span data-ttu-id="0e489-132">Aşağıdaki HTML oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0e489-132">Generates the following HTML:</span></span>

```cshtml
<label for="Email">Email</label>
```

<span data-ttu-id="0e489-133">`asp-for` Özniteliği tarafından kullanılabilir hale `For` özelliğinde `LabelTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="0e489-133">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="0e489-134">Bkz: [yazma etiket Yardımcıları](authoring.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0e489-134">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="0e489-135">Etiket Yardımcısı kapsam yönetme</span><span class="sxs-lookup"><span data-stu-id="0e489-135">Managing Tag Helper scope</span></span>

<span data-ttu-id="0e489-136">Etiket Yardımcıları kapsam bir birleşimi tarafından denetlenen `@addTagHelper`, `@removeTagHelper`ve "!" çevirin karakter.</span><span class="sxs-lookup"><span data-stu-id="0e489-136">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="0e489-137">`@addTagHelper`Etiket Yardımcıları kullanılabilir hale getirir</span><span class="sxs-lookup"><span data-stu-id="0e489-137">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="0e489-138">Adlı yeni bir ASP.NET Core web uygulaması oluşturursanız *AuthoringTagHelpers* (ile kimlik doğrulaması yok), aşağıdaki *Views/_ViewImports.cshtml* dosyayı projenize eklenir:</span><span class="sxs-lookup"><span data-stu-id="0e489-138">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="0e489-139">`@addTagHelper` Yönergesi etiket Yardımcıları görüntülemek için kullanılabilir yapar.</span><span class="sxs-lookup"><span data-stu-id="0e489-139">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="0e489-140">Bu durumda, görünüm dosyasıdır *Views/_ViewImports.cshtml*, varsayılan olarak tüm görünüm dosyaları tarafından devralınır *görünümleri* klasörü ve alt dizinleri; etiket Yardımcıları kullanılabilir hale getirme.</span><span class="sxs-lookup"><span data-stu-id="0e489-140">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="0e489-141">Yukarıdaki kod joker sözdizimini kullanır ("\*") belirtmek için belirtilen derlemedeki tüm etiket Yardımcıları (*Microsoft.AspNetCore.Mvc.TagHelpers*) her görünüm dosyaya kullanılabilecek *görünümleri* dizini veya alt dizin.</span><span class="sxs-lookup"><span data-stu-id="0e489-141">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="0e489-142">İlk parametre sonra `@addTagHelper` yüklemek için etiket Yardımcıları belirtir (kullanıyoruz "\*" tüm etiket Yardımcıları için), ve ikinci parametre "Microsoft.AspNetCore.Mvc.TagHelpers" etiket Yardımcıları içeren derlemenin belirtir.</span><span class="sxs-lookup"><span data-stu-id="0e489-142">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="0e489-143">*Microsoft.AspNetCore.Mvc.TagHelpers* yerleşik ASP.NET Core etiket Yardımcıları için derlemesidir.</span><span class="sxs-lookup"><span data-stu-id="0e489-143">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="0e489-144">Tüm bu projede etiket Yardımcıları kullanıma sunmak için (adlı bir derleme oluşturur *AuthoringTagHelpers*), aşağıdaki kullanırsınız:</span><span class="sxs-lookup"><span data-stu-id="0e489-144">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="0e489-145">Projeniz içeriyorsa bir `EmailTagHelper` varsayılan ad alanı (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), etiket Yardımcısı tam adını (FQN) sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0e489-145">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="0e489-146">Bir FQN kullanarak görünüm için bir etiket Yardımcısı eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="0e489-146">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="0e489-147">Çoğu geliştirici kullanmayı tercih ederseniz "\*" joker karakter sözdizimi.</span><span class="sxs-lookup"><span data-stu-id="0e489-147">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="0e489-148">Joker karakter sözdizimi, joker karakter eklemesini sağlar "\*" bir FQN soneki olarak.</span><span class="sxs-lookup"><span data-stu-id="0e489-148">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="0e489-149">Örneğin, aşağıdaki yönergeleri hiçbirini getirecek `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="0e489-149">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="0e489-150">Daha önce belirtildiği gibi ekleme `@addTagHelper` için yönerge *Views/_ViewImports.cshtml* dosyasını kullanılabilir hale getirir etiket Yardımcısı tüm görünüm dosyalarında *görünümleri* dizin ve alt dizinleri.</span><span class="sxs-lookup"><span data-stu-id="0e489-150">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="0e489-151">Kullanabileceğiniz `@addTagHelper` , yalnızca bu görünümlere etiket Yardımcısı gösterme için katılımı istiyorsanız belirli görünüm dosyalarında yönergesi.</span><span class="sxs-lookup"><span data-stu-id="0e489-151">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="0e489-152">`@removeTagHelper`Etiket Yardımcıları kaldırır</span><span class="sxs-lookup"><span data-stu-id="0e489-152">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="0e489-153">`@removeTagHelper` Olarak aynı iki parametreye sahip `@addTagHelper`, ve daha önce eklediğiniz bir etiket Yardımcısı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="0e489-153">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="0e489-154">Örneğin, `@removeTagHelper` belirli görünüm kaldırır belirtilen etiket Yardımcısı görünümünden uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="0e489-154">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="0e489-155">Kullanarak `@removeTagHelper` içinde bir *Views/Folder/_ViewImports.cshtml* dosyasını tüm görünümleri belirtilen etiket Yardımcısı kaldırır *klasörü*.</span><span class="sxs-lookup"><span data-stu-id="0e489-155">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="0e489-156">Etiket Yardımcısı kapsamıyla denetleme *_viewımports.cshtml* dosyası</span><span class="sxs-lookup"><span data-stu-id="0e489-156">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="0e489-157">Ekleyebileceğiniz bir *_viewımports.cshtml* herhangi bir görünüm klasörü ve görünüm altyapısı yönergeleri hem bu dosyadan uygular. ve *Views/_ViewImports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="0e489-157">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="0e489-158">Boş bir eklediyseniz *Views/Home/_ViewImports.cshtml* dosya *giriş* görünümleri, çünkü herhangi bir değişiklik olacak *_viewımports.cshtml* dosya eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="0e489-158">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="0e489-159">Tüm `@addTagHelper` eklemek için yönergeleri *Views/Home/_ViewImports.cshtml* dosyası (varsayılan olmayan *Views/_ViewImports.cshtml* dosya) Bu etiket Yardımcıları görünümlere kullanılabilmesini yalnızca *Giriş* klasör.</span><span class="sxs-lookup"><span data-stu-id="0e489-159">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="0e489-160">Ayrı ayrı öğeler dışında kullanmama</span><span class="sxs-lookup"><span data-stu-id="0e489-160">Opting out of individual elements</span></span>

<span data-ttu-id="0e489-161">Bir etiket Yardımcısı etiket Yardımcısı çevirin karakteri ile öğe düzeyinde devre dışı bırakabilirsiniz ("!").</span><span class="sxs-lookup"><span data-stu-id="0e489-161">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="0e489-162">Örneğin, `Email` doğrulama devre dışı olarak `<span>` etiket Yardımcısı çevirin karakteriyle:</span><span class="sxs-lookup"><span data-stu-id="0e489-162">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="0e489-163">Etiket Yardımcısı çevirin karakter açma ve kapatma etiketi uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0e489-163">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="0e489-164">(Bir açma etiketi eklediğinizde, Visual Studio düzenleyicisinde otomatik olarak vazgeçme karakter kapanış etiketi ekler).</span><span class="sxs-lookup"><span data-stu-id="0e489-164">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="0e489-165">Vazgeçme karakter ekledikten sonra etiket Yardımcısı öznitelikleri ve öğesi artık farklı bir yazı tipiyle görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0e489-165">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="0e489-166">Kullanarak `@tagHelperPrefix` etiket Yardımcısı kullanım açık yapma</span><span class="sxs-lookup"><span data-stu-id="0e489-166">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="0e489-167">`@tagHelperPrefix` Yönergesi etiket Yardımcısı desteğinin etkinleştirmek ve etiket Yardımcısı kullanım açık yapmak için bir etiket önek dizesi belirtmenize olanak verir.</span><span class="sxs-lookup"><span data-stu-id="0e489-167">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="0e489-168">Örneğin, aşağıdaki biçimlendirme ekleyebilirsiniz *Views/_ViewImports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0e489-168">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="0e489-169">Aşağıdaki kod görüntüde etiket Yardımcısı önek ayarlamak `th:`, bu nedenle yalnızca önek kullanarak öğeleri `th:` etiket Yardımcıları (etiket Yardımcısı etkin öğelere sahip farklı bir yazı tipi) destekler.</span><span class="sxs-lookup"><span data-stu-id="0e489-169">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="0e489-170">`<label>` Ve `<input>` öğeleri etiket Yardımcısı önekine sahip ve etiket Yardımcısı etkinleştirilmiş sırasında `<span>` öğesi değil.</span><span class="sxs-lookup"><span data-stu-id="0e489-170">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![görüntü](intro/_static/thp.png)

<span data-ttu-id="0e489-172">Geçerli aynı hiyerarşiyi kurallarına `@addTagHelper` için de geçerlidir `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="0e489-172">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="0e489-173">Etiket Yardımcıları için IntelliSense desteği</span><span class="sxs-lookup"><span data-stu-id="0e489-173">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="0e489-174">Visual Studio'da yeni bir ASP.NET web uygulaması oluşturduğunuzda, NuGet paketi "Microsoft.AspNetCore.Razor.Tools" ekler.</span><span class="sxs-lookup"><span data-stu-id="0e489-174">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="0e489-175">Bu etiket Yardımcısı araçları ekleyen paketidir.</span><span class="sxs-lookup"><span data-stu-id="0e489-175">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="0e489-176">Bir HTML yazma göz önünde bulundurun `<label>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="0e489-176">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="0e489-177">Girdiğiniz hemen `<l` Visual Studio düzenleyicisinde IntelliSense eşleşen öğeleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0e489-177">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![görüntü](intro/_static/label.png)

<span data-ttu-id="0e489-179">Yalnızca simge ancak HTML Yardım alın ("@" symbol with "<>" altındaki).</span><span class="sxs-lookup"><span data-stu-id="0e489-179">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![görüntü](intro/_static/tagSym.png)

<span data-ttu-id="0e489-181">öğenin etiketi Yardımcılar tarafından hedeflenen olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0e489-181">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="0e489-182">Saf HTML öğeleri (gibi `fieldset`) "<>" simgesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0e489-182">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="0e489-183">Saf HTML `<label>` öznitelik değerleri mavi renkte ve etiket öznitelikleri kırmızı, Kahverengi bir yazı HTML etiketi (varsayılan Visual Studio renk temasını) görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0e489-183">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![görüntü](intro/_static/LableHtmlTag.png)

<span data-ttu-id="0e489-185">Girdikten sonra `<label`, IntelliSense kullanılabilir HTML/CSS öznitelikleri ve etiket Yardımcısı hedeflenen öznitelikleri listeler:</span><span class="sxs-lookup"><span data-stu-id="0e489-185">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![görüntü](intro/_static/labelattr.png)

<span data-ttu-id="0e489-187">IntelliSense deyim tamamlama, seçili değer deyimiyle tamamlamak için SEKME tuşunu girmenizi sağlar:</span><span class="sxs-lookup"><span data-stu-id="0e489-187">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![görüntü](intro/_static/stmtcomplete.png)

<span data-ttu-id="0e489-189">Bir etiketi yardımcı öznitelik girilen hemen etiketini ve öznitelik yazı tiplerini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0e489-189">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="0e489-190">Varsayılan Visual Studio "Mavi" veya "Açık" renk temasını kullanarak, yazı tipi kalın mor renkte.</span><span class="sxs-lookup"><span data-stu-id="0e489-190">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="0e489-191">"Koyu" tema kullanıyorsanız, yazı tipi kalın Deniz ' dir.</span><span class="sxs-lookup"><span data-stu-id="0e489-191">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="0e489-192">Bu belge görüntülerinde varsayılan tema kullanılarak gerçekleştirilen.</span><span class="sxs-lookup"><span data-stu-id="0e489-192">The images in this document were taken using the default theme.</span></span>

![görüntü](intro/_static/labelaspfor2.png)

<span data-ttu-id="0e489-194">Visual Studio girebilirsiniz *CompleteWord* kısayol (Ctrl + Ara çubuğu olan [varsayılan](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) çift tırnak içine (""), ve bir C# sınıfta olması gibi C# ' ta sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="0e489-194">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="0e489-195">IntelliSense sayfa model üzerinde tüm yöntemleri ve özellikleri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0e489-195">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="0e489-196">Özellik türü olduğundan yöntemleri ve özellikleri kullanılabilir `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="0e489-196">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="0e489-197">Aşağıdaki resimde ı düzenleme `Register` görünümü, böylece `RegisterViewModel` kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0e489-197">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![görüntü](intro/_static/intellemail.png)

<span data-ttu-id="0e489-199">IntelliSense modeline sayfasında kullanılabilir yöntemleri ve özellikleri listeler.</span><span class="sxs-lookup"><span data-stu-id="0e489-199">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="0e489-200">Zengin IntelliSense ortamı CSS sınıfı seçmenize yardımcı olur:</span><span class="sxs-lookup"><span data-stu-id="0e489-200">The rich IntelliSense environment helps you select the CSS class:</span></span>

![görüntü](intro/_static/iclass.png)

![görüntü](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="0e489-203">HTML Yardımcıları ile karşılaştırıldığında etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="0e489-203">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="0e489-204">Etiket Yardımcıları ekleme Razor görünümleri, HTML öğelerine sırada [HTML Yardımcıları](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) yöntemleri HTML ile Razor görünümleri interspersed olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0e489-204">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="0e489-205">CSS sınıfı "Açıklama" ile bir HTML etiketi oluşturur aşağıdaki Razor biçimlendirme göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="0e489-205">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="0e489-206">Konumunda (`@`) simgesi söyler Razor kod başlangıcı budur.</span><span class="sxs-lookup"><span data-stu-id="0e489-206">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="0e489-207">Sonraki iki parametre ("Ad" ve "ad:"), bu nedenle dizelerdir [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) yardımcı olamaz.</span><span class="sxs-lookup"><span data-stu-id="0e489-207">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="0e489-208">Son bağımsız değişken:</span><span class="sxs-lookup"><span data-stu-id="0e489-208">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="0e489-209">Bir anonim nesnenin özniteliklerini temsil etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0e489-209">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="0e489-210">Çünkü **sınıfı** ayrılmış bir anahtar C# ' ta, kullandığınız `@` yorumlamak için C# zorlamak için simge "@class=" (özellik adı) sembol olarak.</span><span class="sxs-lookup"><span data-stu-id="0e489-210">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="0e489-211">Ön uç tasarımcıya (birisi HTML/CSS/JavaScript ve diğer istemci teknolojileri hakkında bilgi sahibi ancak C# ve Razor tanıdık), satır çoğunu yabancı.</span><span class="sxs-lookup"><span data-stu-id="0e489-211">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="0e489-212">Tüm satırı IntelliSense hiçbir Yardım ile yazılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="0e489-212">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="0e489-213">Kullanarak `LabelTagHelper`, aynı biçimlendirme olarak yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="0e489-213">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![görüntü](intro/_static/label2.png)

<span data-ttu-id="0e489-215">Girdiğiniz hemen etiket Yardımcısı sürümüyle `<l` Visual Studio düzenleyicisinde IntelliSense eşleşen öğeleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0e489-215">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![görüntü](intro/_static/label.png)

<span data-ttu-id="0e489-217">IntelliSense tüm satırı yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="0e489-217">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="0e489-218">`LabelTagHelper` İçeriğini ayarını da varsayılanları `asp-for` öznitelik değeri ("FirstName") "İlk adına"; Her yeni büyük harf oluştuğu boşluk içeren özellik adı oluşan bir cümle başlamalıdır özellikleri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="0e489-218">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="0e489-219">Aşağıdaki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="0e489-219">In the following markup:</span></span>

![görüntü](intro/_static/label2.png)

<span data-ttu-id="0e489-221">oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0e489-221">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="0e489-222">İçeriği eklerseniz başlamalıdır cümle ortası içeriğe kullanılmaz `<label>`.</span><span class="sxs-lookup"><span data-stu-id="0e489-222">The camel-cased to sentence-cased content isn't used if you add content to the `<label>`.</span></span> <span data-ttu-id="0e489-223">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0e489-223">For example:</span></span>

![görüntü](intro/_static/1stName.png)

<span data-ttu-id="0e489-225">oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0e489-225">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="0e489-226">Aşağıdaki kodu görüntüsünü Form bölümünü gösterir *Views/Account/Register.cshtml* Visual Studio 2015 ile birlikte dahil eski ASP.NET 4.5.x MVC şablondan oluşturulan Razor görünümü.</span><span class="sxs-lookup"><span data-stu-id="0e489-226">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![görüntü](intro/_static/regCS.png)

<span data-ttu-id="0e489-228">C# kodu gri arka plan ile Visual Studio düzenleyicisinde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0e489-228">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="0e489-229">Örneğin, `AntiForgeryToken` HTML Yardımcısı:</span><span class="sxs-lookup"><span data-stu-id="0e489-229">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="0e489-230">gri bir arka plan görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0e489-230">is displayed with a grey background.</span></span> <span data-ttu-id="0e489-231">Kayıt görünümünde biçimlendirme çoğunu olan C#.</span><span class="sxs-lookup"><span data-stu-id="0e489-231">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="0e489-232">Bu etiket Yardımcıları kullanarak eşdeğer bir yaklaşım karşılaştırın:</span><span class="sxs-lookup"><span data-stu-id="0e489-232">Compare that to the equivalent approach using Tag Helpers:</span></span>

![görüntü](intro/_static/regTH.png)

<span data-ttu-id="0e489-234">İşaretleme çok temizleyici ve okuma, düzenleme ve HTML Yardımcıları yaklaşım sürdürmek daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="0e489-234">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="0e489-235">C# kodu sunucunun hakkında bilmeniz gereken en düşük azalır.</span><span class="sxs-lookup"><span data-stu-id="0e489-235">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="0e489-236">Visual Studio düzenleyicisinde farklı bir yazı tipi etiket Yardımcısı tarafından hedeflenen biçimlendirme görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0e489-236">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="0e489-237">Göz önünde bulundurun *e-posta* Grup:</span><span class="sxs-lookup"><span data-stu-id="0e489-237">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="0e489-238">"Asp-" özniteliklerinin her biriyle "E-posta" değerine sahip, ancak "E-posta" bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="0e489-238">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="0e489-239">Bu bağlamda "E-posta" C# modeli ifade özelliğidir `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="0e489-239">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="0e489-240">Visual Studio düzenleyicisinde yazmanıza yardımcı **tüm** biçimlendirme etiket Yardımcısı yaklaşımda kayıt formunun HTML Yardımcıları yaklaşım kodda çoğu için Visual Studio Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="0e489-240">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="0e489-241">[Etiket Yardımcıları için IntelliSense desteği](#intellisense-support-for-tag-helpers) Visual Studio düzenleyicisinde etiket Yardımcıları ile çalışma hakkında ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="0e489-241">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="0e489-242">Web sunucusu denetimleri karşılaştırıldığında etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="0e489-242">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="0e489-243">Etiket Yardımcıları ilişkili oldukları öğesi yok; Bunlar yalnızca öğe ve içerik işlemede katılın.</span><span class="sxs-lookup"><span data-stu-id="0e489-243">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="0e489-244">ASP.NET [Web sunucusu denetimleri](https://msdn.microsoft.com/library/7698y1f0.aspx) bildirilir ve bir sayfada çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0e489-244">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="0e489-245">[Web sunucusu denetimleri](https://msdn.microsoft.com/library/zsyt68f1.aspx) geliştirme ve hata ayıklama zorlaştırabilir Önemsiz olmayan bir yaşam döngüleri vardır.</span><span class="sxs-lookup"><span data-stu-id="0e489-245">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="0e489-246">Web sunucusu denetimleri istemci denetimi kullanarak istemci belge nesne modeli (DOM) öğelerine işlevselliği eklemenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="0e489-246">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="0e489-247">Etiket Yardımcıları hiçbir DOM sahip</span><span class="sxs-lookup"><span data-stu-id="0e489-247">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="0e489-248">Web sunucusu denetimleri otomatik tarayıcı algılama içerir.</span><span class="sxs-lookup"><span data-stu-id="0e489-248">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="0e489-249">Etiket Yardımcıları tarayıcı hiçbir bilgiye sahip.</span><span class="sxs-lookup"><span data-stu-id="0e489-249">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="0e489-250">Aynı öğede birden çok etiket Yardımcıları davranamaz (bkz [etiket Yardımcısı önlemenin çakışmaları](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) Web sunucusu denetimleri genellikle oluşturamazsınız sırada.</span><span class="sxs-lookup"><span data-stu-id="0e489-250">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="0e489-251">Etiket Yardımcıları etiketini ve kapsamlı HTML öğelerinin içeriğini değiştirebilirsiniz, ancak bir sayfa üzerinde başka bir şey doğrudan değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="0e489-251">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="0e489-252">Web sunucusu denetimleri daha az belirli bir kapsamda sahip ve diğer sayfanızı bölümlerini etkileyen eylemler gerçekleştirebilir; istenmeyen yan etkileri etkinleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="0e489-252">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="0e489-253">Web sunucusu denetimleri dizeleri nesnelerine dönüştürmek için tür dönüştürücüleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="0e489-253">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="0e489-254">Tür dönüşümü gerek kalmaması etiket Yardımcıları ile yerel olarak C# ile çalışırsınız.</span><span class="sxs-lookup"><span data-stu-id="0e489-254">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="0e489-255">Web sunucusu denetimleri kullanın [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) bileşenleri ve denetimleri çalıştırma ve tasarım zamanı davranışını uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="0e489-255">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="0e489-256">`System.ComponentModel`temel sınıflar ve öznitelikler ve tür dönüştürücüleri uygulama, veri kaynakları bağlama ve bileşenleri lisans arabirimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="0e489-256">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="0e489-257">Genellikle türetin etiket Yardımcıları için Karşıtlık `TagHelper`ve `TagHelper` temel sınıfı yalnızca iki yöntem sunar `Process` ve `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="0e489-257">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="0e489-258">Etiketi yardımcı öğe yazı tipi özelleştirme</span><span class="sxs-lookup"><span data-stu-id="0e489-258">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="0e489-259">Yazı tipi ve gelen renklendirme özelleştirebilirsiniz **Araçları** > **seçenekleri** > **ortam** > **yazı tipleri ve renkleri**:</span><span class="sxs-lookup"><span data-stu-id="0e489-259">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![görüntü](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="0e489-261">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0e489-261">Additional Resources</span></span>

* [<span data-ttu-id="0e489-262">Etiket Yardımcıları yazma</span><span class="sxs-lookup"><span data-stu-id="0e489-262">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="0e489-263">Formları ile çalışma</span><span class="sxs-lookup"><span data-stu-id="0e489-263">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="0e489-264">[Github'da TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) ile çalışmak için etiket Yardımcısı örnekleri içeren [önyükleme](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="0e489-264">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
