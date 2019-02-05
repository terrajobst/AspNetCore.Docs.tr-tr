---
title: ASP.NET core'da Yazar etiket Yardımcıları
author: rick-anderson
description: ASP.NET core'da etiket Yardımcıları yazmak öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 3e266bc435ff7e4a15655276c581ac171f0de47c
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712282"
---
# <a name="author-tag-helpers-in-aspnet-core"></a><span data-ttu-id="fee7a-103">ASP.NET core'da Yazar etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="fee7a-103">Author Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="fee7a-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fee7a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fee7a-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fee7a-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="fee7a-106">Etiket Yardımcıları ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="fee7a-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="fee7a-107">Bu öğreticide programlama etiket Yardımcıları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="fee7a-108">[Etiket Yardımcıları giriş](intro.md) etiket Yardımcıları sağlayan avantajlarına açıklar.</span><span class="sxs-lookup"><span data-stu-id="fee7a-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="fee7a-109">Etiket Yardımcısı uygulayan sınıf olan `ITagHelper` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="fee7a-110">Etiket Yardımcısı geliştirdiğinizde, Bununla birlikte, genellikle öğesinden türetilen `TagHelper`, yapmak için bunu erişmenizi `Process` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="fee7a-111">Adlı yeni bir ASP.NET Core projesi oluşturma **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="fee7a-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="fee7a-112">Bu proje için kimlik doğrulaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fee7a-112">You won't need authentication for this project.</span></span>

1. <span data-ttu-id="fee7a-113">Adlı etiket Yardımcıları tutmak için bir klasör oluşturun *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="fee7a-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="fee7a-114">*TagHelpers* klasördür *değil* gerekli, ancak bunu makul bir kuraldır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="fee7a-115">Şimdi bazı basit etiket Yardımcıları yazmaya başlayalım.</span><span class="sxs-lookup"><span data-stu-id="fee7a-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="fee7a-116">En az bir etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="fee7a-116">A minimal Tag Helper</span></span>

<span data-ttu-id="fee7a-117">Bu bölümde, bir e-posta etiketi güncelleştiren etiket Yardımcısı yazmaya.</span><span class="sxs-lookup"><span data-stu-id="fee7a-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="fee7a-118">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fee7a-118">For example:</span></span>

```html
<email>Support</email>
```

<span data-ttu-id="fee7a-119">Sunucu, bizim e-posta etiketi Yardımcısı, biçimlendirme ve bunlar aşağıdaki şekilde dönüştürmek için kullanır:</span><span class="sxs-lookup"><span data-stu-id="fee7a-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="fee7a-120">Diğer bir deyişle, bir yer işareti etiketi, bu e-posta bağlantısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="fee7a-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="fee7a-121">Blog altyapısıdır ve yazma ve aynı etki alanı için pazarlama desteği ve diğer kişiler için e-posta göndermek istiyorsanız bunu yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fee7a-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1. <span data-ttu-id="fee7a-122">Aşağıdaki `EmailTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="fee7a-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * <span data-ttu-id="fee7a-123">Etiket Yardımcıları kök sınıf adı öğelerini hedefleyen bir adlandırma kuralını kullanın (eksi *TagHelper* sınıf adı kısmı).</span><span class="sxs-lookup"><span data-stu-id="fee7a-123">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="fee7a-124">Bu örnekte, kök adı **EmailTagHelper** olduğu *e-posta*, bu nedenle `<email>` etiketi hedeflenecek.</span><span class="sxs-lookup"><span data-stu-id="fee7a-124">In this example, the root name of **EmailTagHelper** is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="fee7a-125">Bu adlandırma çoğu etiket Yardımcıları için çalışır, daha sonra bu geçersiz kılma göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="fee7a-125">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>

   * <span data-ttu-id="fee7a-126">`EmailTagHelper` Sınıf türetilir `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-126">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="fee7a-127">`TagHelper` SAX yöntemleri ve özellikleri etiket Yardımcıları yazmak için.</span><span class="sxs-lookup"><span data-stu-id="fee7a-127">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>

   * <span data-ttu-id="fee7a-128">Geçersiz kılınan `Process` yöntemi denetimleri etiketi Yardımcısı yürütüldüğünde yapar.</span><span class="sxs-lookup"><span data-stu-id="fee7a-128">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="fee7a-129">`TagHelper` Sınıfı zaman uyumsuz bir sürümü sağlar (`ProcessAsync`) aynı parametrelere sahip.</span><span class="sxs-lookup"><span data-stu-id="fee7a-129">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>

   * <span data-ttu-id="fee7a-130">Bağlam parametresi `Process` (ve `ProcessAsync`) geçerli HTML etiketinin yürütme ile ilgili bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-130">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>

   * <span data-ttu-id="fee7a-131">Çıkış parametresi `Process` (ve `ProcessAsync`) bir HTML etiketi ve içerik oluşturmak için kullanılan özgün kaynağını temsil eden bir durum bilgisi olan HTML öğesi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="fee7a-131">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>

   * <span data-ttu-id="fee7a-132">Bizim sınıf adı son eki olan **TagHelper**, olduğu *değil* gerekli, ancak bir en iyi yöntem kuralı dikkate almıştır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-132">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="fee7a-133">Sınıf olarak bildirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fee7a-133">You could declare the class as:</span></span>

   ```csharp
   public class Email : TagHelper
   ```

1. <span data-ttu-id="fee7a-134">Yapmak `EmailTagHelper` bizim Razor görünümleri tüm kullanılabilir sınıf, ekleme `addTagHelper` yönergesini *Views/_ViewImports.cshtml* dosyası: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="fee7a-134">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>

   <span data-ttu-id="fee7a-135">Yukarıdaki kod, bizim derlemedeki tüm etiket Yardımcıları kullanılabilir olacağını belirtmek için joker karakter sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-135">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="fee7a-136">Sonra ilk dize `@addTagHelper` yüklemek için etiket Yardımcısı belirtir (kullan "\*" tüm etiket Yardımcıları için), ve ikinci dize "AuthoringTagHelpers" etiketi Yardımcısı olan derlemeyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-136">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="fee7a-137">Ayrıca, ikinci satır joker karakter sözdizimini kullanarak ASP.NET Core MVC etiket Yardımcıları getirir unutmayın (Bu Yardımcıları ele alınmıştır [etiket Yardımcıları giriş](intro.md).) Bu `@addTagHelper` etiketi Yardımcısı Razor görünüm kullanılabilmesini yönergesi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-137">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="fee7a-138">Alternatif olarak, aşağıda gösterildiği gibi bir etiket Yardımcısı'nın (FQN) tam nitelikli ad sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fee7a-138">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

<span data-ttu-id="fee7a-139">Etiket Yardımcısı kullanarak bir FQN görünümüne eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından **derleme adı** (*AuthoringTagHelpers*, değil necessarly `namespace`).</span><span class="sxs-lookup"><span data-stu-id="fee7a-139">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the **assembly name** (*AuthoringTagHelpers*, not necessarly the `namespace`).</span></span> <span data-ttu-id="fee7a-140">Çoğu geliştirici, joker karakter sözdizimini kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="fee7a-140">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="fee7a-141">[Etiket Yardımcıları giriş](intro.md) etiket Yardımcısı ekleme, kaldırma, hiyerarşi ve joker karakter sözdizimi, ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="fee7a-141">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>

1. <span data-ttu-id="fee7a-142">Biçimlendirme içinde güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası:</span><span class="sxs-lookup"><span data-stu-id="fee7a-142">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="fee7a-143">Uygulamayı çalıştırın ve e-posta etiketler ile bağlantı biçimlendirme değiştirilir doğrulayabilmeniz için HTML kaynağını görüntülemek için sık kullandığınız tarayıcıyı kullanabilirsiniz (örneğin, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="fee7a-143">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="fee7a-144">*Destek* ve *pazarlama* bir bağlantı olarak işlenir ancak bir `href` işlevsel hale getirmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="fee7a-144">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="fee7a-145">Sonraki bölümde, gidereceğiz.</span><span class="sxs-lookup"><span data-stu-id="fee7a-145">We'll fix that in the next section.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="fee7a-146">SetAttribute ve SetContent</span><span class="sxs-lookup"><span data-stu-id="fee7a-146">SetAttribute and SetContent</span></span>

<span data-ttu-id="fee7a-147">Bu bölümde, güncelleştireceğiz `EmailTagHelper` böylece e-posta için bir geçerli yer işareti etiketi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fee7a-147">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="fee7a-148">Razor görünümü bilgilerinden gerçekleştirecek şekilde güncelleştireceğiz (biçiminde bir `mail-to` özniteliği) ve bağlantı oluşturmak kullanın.</span><span class="sxs-lookup"><span data-stu-id="fee7a-148">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="fee7a-149">Güncelleştirme `EmailTagHelper` aşağıdaki sınıfı:</span><span class="sxs-lookup"><span data-stu-id="fee7a-149">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* <span data-ttu-id="fee7a-150">İçine çevrilmiş Pascal büyük küçük harfleri sınıf ve özellik adlarını etiket Yardımcıları için kendi [kebab çalışması](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="fee7a-150">Pascal-cased class and property names for tag helpers are translated into their [kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="fee7a-151">Bu nedenle, kullanılacak `MailTo` kullanacağınız özniteliği `<email mail-to="value"/>` eşdeğer.</span><span class="sxs-lookup"><span data-stu-id="fee7a-151">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="fee7a-152">Son satırı tamamlanmış içeriği bizim için en düşük düzeyde işlevsel etiket Yardımcısı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fee7a-152">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="fee7a-153">Vurgulanan satır öznitelikleri eklemek için sözdizimini gösterir:</span><span class="sxs-lookup"><span data-stu-id="fee7a-153">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="fee7a-154">Şu anda öznitelikleri koleksiyonda yok sürece bu yaklaşımı "href =" özniteliği için çalışır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-154">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="fee7a-155">Ayrıca `output.Attributes.Add` etiketi yardımcı öznitelik etiketi özniteliklerinin koleksiyonunu sonuna eklemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-155">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1. <span data-ttu-id="fee7a-156">Biçimlendirme içinde güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="fee7a-156">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

1. <span data-ttu-id="fee7a-157">Uygulamayı çalıştırın ve doğrulayın, doğru bağlantıları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fee7a-157">Run the app and verify that it generates the correct links.</span></span>

<a name="self-closing"></a>

   > [!NOTE]
   > <span data-ttu-id="fee7a-158">E-posta kendi kendine kapanan etiket yazmak için olsaydı (`<email mail-to="Rick" />`), son Çıkışta aynı zamanda kendi kendine kapanan olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-158">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="fee7a-159">Etiket yalnızca bir başlangıç etiketi ile yazma olanağı sağlamak (`<email mail-to="Rick">`) aşağıdaki sınıf tasarlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="fee7a-159">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   <span data-ttu-id="fee7a-160">Bir kendi kendine kapanan e-posta etiket Yardımcısı ile çıkış olacaktır `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-160">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="fee7a-161">Kendi kendine kapanan bağlantı etiketler geçerli HTML oluşturmak istemezsiniz, ancak kendi kendine kapanan etiket Yardımcısı oluşturmak isteyebilirsiniz değildir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-161">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="fee7a-162">Etiket Yardımcıları türünü ayarlayın `TagMode` sonra bir etiketi okuma özelliği.</span><span class="sxs-lookup"><span data-stu-id="fee7a-162">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>

### <a name="processasync"></a><span data-ttu-id="fee7a-163">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="fee7a-163">ProcessAsync</span></span>

<span data-ttu-id="fee7a-164">Bu bölümde, biz bir zaman uyumsuz bir e-posta yardımcı yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="fee7a-164">In this section, we'll write an asynchronous email helper.</span></span>

1. <span data-ttu-id="fee7a-165">Değiştirin `EmailTagHelper` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="fee7a-165">Replace the `EmailTagHelper` class with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   <span data-ttu-id="fee7a-166">**Notlar:**</span><span class="sxs-lookup"><span data-stu-id="fee7a-166">**Notes:**</span></span>

   * <span data-ttu-id="fee7a-167">Bu sürüm, zaman uyumsuz kullanır `ProcessAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-167">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="fee7a-168">Zaman uyumsuz `GetChildContentAsync` döndürür bir `Task` içeren `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-168">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

   * <span data-ttu-id="fee7a-169">Kullanım `output` parametresi HTML öğesinden içeriği alamadı.</span><span class="sxs-lookup"><span data-stu-id="fee7a-169">Use the `output` parameter to get contents of the HTML element.</span></span>

1. <span data-ttu-id="fee7a-170">Aşağıdaki değişiklik *Views/Home/Contact.cshtml* etiketi Yardımcısı hedef e-posta alabilmeniz için dosya.</span><span class="sxs-lookup"><span data-stu-id="fee7a-170">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="fee7a-171">Uygulamayı çalıştırın ve geçerli bir e-posta bağlantılarını oluşturur doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fee7a-171">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="fee7a-172">RemoveAll ve PreContent.SetHtmlContent PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="fee7a-172">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1. <span data-ttu-id="fee7a-173">Aşağıdaki `BoldTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="fee7a-173">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * <span data-ttu-id="fee7a-174">`[HtmlTargetElement]` Özniteliği belirten bir HTML öznitelik içeren herhangi bir HTML öğesi "bold" adlı bir öznitelik parametresi eşleşir, geçişleri ve `Process` sınıfı yöntemi geçersiz kılma çalışır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-174">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="fee7a-175">Bizim örnek içinde `Process` yöntemi "kalın" özniteliğini kaldırır ve içeren biçimlendirme çevreleyen `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-175">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>

   * <span data-ttu-id="fee7a-176">İçerik var olan etiket değiştirilecek istemediğinden açılış yazmanız gereken `<strong>` ile etiketi `PreContent.SetHtmlContent` yöntemi ve kapatma `</strong>` ile etiketi `PostContent.SetHtmlContent` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-176">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>

1. <span data-ttu-id="fee7a-177">Değiştirme *About.cshtml* görünümü içerecek bir `bold` öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="fee7a-177">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="fee7a-178">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-178">The completed code is shown below.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. <span data-ttu-id="fee7a-179">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fee7a-179">Run the app.</span></span> <span data-ttu-id="fee7a-180">Kaynak inceleyin ve biçimlendirme doğrulamak için sık kullandığınız tarayıcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fee7a-180">You can use your favorite browser to inspect the source and verify the markup.</span></span>

   <span data-ttu-id="fee7a-181">`[HtmlTargetElement]` Yukarıdaki özniteliği bir öznitelik adı "kalın" sağlayan HTML biçimlendirmesi hedefler.</span><span class="sxs-lookup"><span data-stu-id="fee7a-181">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="fee7a-182">`<bold>` Öğesi etiketi Yardımcısı tarafından değiştirilen değildi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-182">The `<bold>` element wasn't modified by the tag helper.</span></span>

1. <span data-ttu-id="fee7a-183">Açıklama `[HtmlTargetElement]` özniteliği satır ve varsayılan hedeflemeye `<bold>` etiketleri, diğer bir deyişle, HTML biçimlendirmesi formun `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-183">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="fee7a-184">Unutmayın, varsayılan adlandırma kuralı, sınıf adı eşleşir **kalın**için TagHelper `<bold>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="fee7a-184">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

1. <span data-ttu-id="fee7a-185">Uygulamayı çalıştırın ve doğrulayın `<bold>` etiketin, etiket Yardımcısı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-185">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="fee7a-186">Bir sınıf birden çok dekorasyon `[HtmlTargetElement]` hedefleri mantıksal-veya sonuçlarını öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="fee7a-186">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="fee7a-187">Örneğin, aşağıdaki kodu kullanarak, kalın bir etiket veya bold özniteliğinin eşleşir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-187">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="fee7a-188">Birden çok öznitelik için aynı deyimde eklendiğinde, çalışma zamanı bunları bir mantıksal-and davranır</span><span class="sxs-lookup"><span data-stu-id="fee7a-188">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="fee7a-189">Örneğin, aşağıdaki kod bir HTML öğesi "bold" adlı bir öznitelik ile "bold" olarak adlandırılmalıdır (`<bold bold />`) eşleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="fee7a-189">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="fee7a-190">Ayrıca `[HtmlTargetElement]` hedeflenen öğesi adını değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="fee7a-190">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="fee7a-191">İsterseniz örneğin `BoldTagHelper` hedef `<MyBold>` etiketleri, aşağıdaki öznitelik kullanmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="fee7a-191">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="fee7a-192">Etiket Yardımcısı için bir model geçirin</span><span class="sxs-lookup"><span data-stu-id="fee7a-192">Pass a model to a Tag Helper</span></span>

1. <span data-ttu-id="fee7a-193">Ekleme bir *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="fee7a-193">Add a *Models* folder.</span></span>

1. <span data-ttu-id="fee7a-194">Aşağıdaki `WebsiteContext` sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="fee7a-194">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. <span data-ttu-id="fee7a-195">Aşağıdaki `WebsiteInformationTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="fee7a-195">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * <span data-ttu-id="fee7a-196">Daha önce belirtildiği gibi etiket Yardımcıları çevirir Pascal büyük küçük harfleri C# sınıf adları ve Özellikler etiket Yardımcıları için [kebab çalışması](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="fee7a-196">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="fee7a-197">Bu nedenle, kullanılacak `WebsiteInformationTagHelper` Razor içinde yazacaksınız `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-197">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>

   * <span data-ttu-id="fee7a-198">Açıkça hedef öğeyi tanımlayan değil `[HtmlTargetElement]` özniteliği, bunu varsayılan `website-information` hedeflenir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-198">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="fee7a-199">Aşağıdaki özniteliği (kebab durum geçerli değildir ama sınıfın adıyla eşleşen unutmayın) uygulandığında:</span><span class="sxs-lookup"><span data-stu-id="fee7a-199">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   <span data-ttu-id="fee7a-200">Kebab case etiketi `<website-information />` eşleşen mıydı.</span><span class="sxs-lookup"><span data-stu-id="fee7a-200">The kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="fee7a-201">Kullanmak istediğiniz `[HtmlTargetElement]` özniteliği, aşağıda gösterildiği gibi kebab çalışması kullanmanız:</span><span class="sxs-lookup"><span data-stu-id="fee7a-201">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * <span data-ttu-id="fee7a-202">Kendi kendine kapanan öğeler hiçbir içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="fee7a-202">Elements that are self-closing have no content.</span></span> <span data-ttu-id="fee7a-203">Bu örnekte, bir kendi kendine kapanan etiketi Razor işaretlemesi kullanır, ancak etiketi Yardımcısı oluşturma bir [bölümü](http://www.w3.org/TR/html5/sections.html#the-section-element) öğesi (hangi kendi kendine kapanan değildir ve içeriği yazıyorsanız `section` öğesi).</span><span class="sxs-lookup"><span data-stu-id="fee7a-203">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="fee7a-204">Bu nedenle, ayarlanacak ihtiyacınız `TagMode` için `StartTagAndEndTag` çıkışını yazmak için.</span><span class="sxs-lookup"><span data-stu-id="fee7a-204">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="fee7a-205">Satır ayarını WMI'ya alternatif olarak, yorum `TagMode` ve bir kapatma etiketi ile işaretleme yazma.</span><span class="sxs-lookup"><span data-stu-id="fee7a-205">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="fee7a-206">(Daha sonra Bu öğreticide örnek biçimlendirme sağlanır.)</span><span class="sxs-lookup"><span data-stu-id="fee7a-206">(Example markup is provided later in this tutorial.)</span></span>

   * <span data-ttu-id="fee7a-207">`$` (Dolar işareti) dosyasında aşağıdaki satırı kullanan bir [ilişkilendirilmiş bir dizedir](/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="fee7a-207">The `$` (dollar sign) in the following line uses an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. <span data-ttu-id="fee7a-208">İçin aşağıdaki işaretlemeyi ekleyin *About.cshtml* görünümü.</span><span class="sxs-lookup"><span data-stu-id="fee7a-208">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="fee7a-209">Vurgulanan biçimlendirme web site bilgilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fee7a-209">The highlighted markup displays the web site information.</span></span>

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,4-8, 18-999)]

   > [!NOTE]
   > <span data-ttu-id="fee7a-210">Aşağıda gösterilen Razor biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="fee7a-210">In the Razor markup shown below:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=18-18)]
   >
   > <span data-ttu-id="fee7a-211">Razor bilir `info` özniteliği bir sınıf, bir dize değil ve C# kodu yazmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="fee7a-211">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="fee7a-212">Herhangi bir dize olmayan etiketi yardımcı öznitelik olmadan yazılmalıdır `@` karakter.</span><span class="sxs-lookup"><span data-stu-id="fee7a-212">Any non-string tag helper attribute should be written without the `@` character.</span></span>

1. <span data-ttu-id="fee7a-213">Uygulamayı çalıştırın ve web sitesi bilgileri görmek için hakkında görünümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="fee7a-213">Run the app, and navigate to the About view to see the web site information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="fee7a-214">Aşağıdaki biçimlendirme bir kapatma etiketi ile kullanın ve içeren satırı Kaldır `TagMode.StartTagAndEndTag` etiketi Yardımcısı'nda:</span><span class="sxs-lookup"><span data-stu-id="fee7a-214">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=20-21)]

## <a name="condition-tag-helper"></a><span data-ttu-id="fee7a-215">Koşul etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="fee7a-215">Condition Tag Helper</span></span>

<span data-ttu-id="fee7a-216">True değeri geçirildiğinde çıkış koşulu etiket Yardımcısı işler.</span><span class="sxs-lookup"><span data-stu-id="fee7a-216">The condition tag helper renders output when passed a true value.</span></span>

1. <span data-ttu-id="fee7a-217">Aşağıdaki `ConditionTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="fee7a-217">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. <span data-ttu-id="fee7a-218">Öğesinin içeriğini değiştirin *Views/Home/Index.cshtml* aşağıdaki işaretlemeyle dosyası:</span><span class="sxs-lookup"><span data-stu-id="fee7a-218">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. <span data-ttu-id="fee7a-219">Değiştirin `Index` yönteminde `Home` aşağıdaki kodla denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="fee7a-219">Replace the `Index` method in the `Home` controller with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. <span data-ttu-id="fee7a-220">Uygulamayı çalıştırın ve giriş sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="fee7a-220">Run the app and browse to the home page.</span></span> <span data-ttu-id="fee7a-221">Koşullu biçimlendirmeyi `div` işlenmez.</span><span class="sxs-lookup"><span data-stu-id="fee7a-221">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="fee7a-222">Sorgu dizesini URL'ye `?approved=true` URL'sine (örneğin, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="fee7a-222">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="fee7a-223">`approved` TRUE olarak ve koşullu biçimlendirme görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fee7a-223">`approved` is set to true and the conditional markup will be displayed.</span></span>

> [!NOTE]
> <span data-ttu-id="fee7a-224">Kullanım [nameof](/dotnet/csharp/language-reference/keywords/nameof) işleci ile kalın etiket Yardımcısı yaptığınız gibi dize belirtme yerine hedef özniteliğe belirtmek için:</span><span class="sxs-lookup"><span data-stu-id="fee7a-224">Use the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> <span data-ttu-id="fee7a-225">[Nameof](/dotnet/csharp/language-reference/keywords/nameof) işleci korumak kod hiç olmadığı kadar yeniden (biz adını değiştirmek de isteyebilirsiniz `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="fee7a-225">The [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="fee7a-226">Etiket Yardımcısı çakışmaları</span><span class="sxs-lookup"><span data-stu-id="fee7a-226">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="fee7a-227">Bu bölümde, otomatik bağlama etiket Yardımcıları çifti yazın.</span><span class="sxs-lookup"><span data-stu-id="fee7a-227">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="fee7a-228">İlk biçimlendirme içeren bir HTML yer işareti etiketi aynı URL'yi içeren (ve bu nedenle bağlantı URL'si sonuçlanmıyor) HTTP ile başlayan bir URL yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-228">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="fee7a-229">İkinci aynı WWW ile başlayan bir URL yapar.</span><span class="sxs-lookup"><span data-stu-id="fee7a-229">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="fee7a-230">Bu iki Yardımcıları yakından ilgili ve gelecekte yeniden çünkü bunların aynı dosyada saklayacağız.</span><span class="sxs-lookup"><span data-stu-id="fee7a-230">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1. <span data-ttu-id="fee7a-231">Aşağıdaki `AutoLinkerHttpTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="fee7a-231">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   ><span data-ttu-id="fee7a-232">`AutoLinkerHttpTagHelper` Sınıfı hedefler `p` öğeleri ve kullanıyor [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) bağlantı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="fee7a-232">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

1. <span data-ttu-id="fee7a-233">Sonuna aşağıdaki işaretlemeyi ekleyin *Views/Home/Contact.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="fee7a-233">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. <span data-ttu-id="fee7a-234">Uygulamayı çalıştırın ve etiket Yardımcısı bağlantı doğru şekilde işlediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="fee7a-234">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

1. <span data-ttu-id="fee7a-235">Güncelleştirme `AutoLinker` içerecek şekilde sınıfı `AutoLinkerWwwTagHelper` hangi www metne dönüştürme ayrıca orijinal www metni içeren bir yer işareti etiketi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-235">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="fee7a-236">Güncelleştirilmiş kod aşağıda vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="fee7a-236">The updated code is highlighted below:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. <span data-ttu-id="fee7a-237">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fee7a-237">Run the app.</span></span> <span data-ttu-id="fee7a-238">Www metin bağlantı olarak işlenir ancak HTTP metin değil dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="fee7a-238">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="fee7a-239">Her iki sınıflarda kesme noktası koymak, HTTP etiket Yardımcısı sınıfı ilk çalıştığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fee7a-239">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="fee7a-240">Etiket Yardımcısı çıkışı önbelleğe alınır ve WWW etiketi Yardımcısı çalıştırdığınızda, önbelleğe alınan HTTP etiketi Yardımcısı çıktısı üzerine yazar sorunudur.</span><span class="sxs-lookup"><span data-stu-id="fee7a-240">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="fee7a-241">Etiket Yardımcıları Çalıştır sırasını denetlemek nasıl öğreticinin ilerleyen bölümlerinde göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="fee7a-241">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="fee7a-242">Kodu aşağıdakiler ile gidereceğiz:</span><span class="sxs-lookup"><span data-stu-id="fee7a-242">We'll fix the code with the following:</span></span>

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > <span data-ttu-id="fee7a-243">Otomatik bağlama etiket Yardımcıları, ilk sürümünde, aşağıdaki kod ile hedef içeriğini alındı:</span><span class="sxs-lookup"><span data-stu-id="fee7a-243">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > <span data-ttu-id="fee7a-244">Diğer bir deyişle, çağırma `GetChildContentAsync` kullanarak `TagHelperOutput` yöntemlere geçirilen `ProcessAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fee7a-244">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="fee7a-245">Belirtildiği gibi daha önce çıktıyı önbelleğe alındığından, son etiket WINS çalışmasına yardımcı.</span><span class="sxs-lookup"><span data-stu-id="fee7a-245">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="fee7a-246">Aşağıdaki kod ile ilgili sorun düzeltildi:</span><span class="sxs-lookup"><span data-stu-id="fee7a-246">You fixed that problem with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > <span data-ttu-id="fee7a-247">Yukarıdaki kod içeriği değiştirildi ve varsa, çıkış arabellek içeriği alır olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="fee7a-247">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

1. <span data-ttu-id="fee7a-248">Uygulamayı çalıştırın ve iki bağlantı beklendiği gibi çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fee7a-248">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="fee7a-249">Bizim otomatik bağlayıcı etiket Yardımcısı doğru ve eksiksiz görünebilir, ancak ince bir sorun var.</span><span class="sxs-lookup"><span data-stu-id="fee7a-249">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="fee7a-250">WWW etiketi Yardımcısı ilk çalıştırıyorsa, www bağlantıları doğru olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="fee7a-250">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="fee7a-251">Ekleyerek kodu güncelleştirmeniz `Order` etiket çalışan sırasını denetlemek için aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="fee7a-251">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="fee7a-252">`Order` Özelliği aynı öğeye hedefleyen diğer etiket Yardımcıları göre yürütme sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="fee7a-252">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="fee7a-253">Varsayılan sıra değeri sıfırdır ve düşük değerler örnekleriyle önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fee7a-253">The default order value is zero and instances with lower values are executed first.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   <span data-ttu-id="fee7a-254">Yukarıdaki kod, HTTP etiketi Yardımcısı önce WWW etiketi Yardımcısı çalıştığını garanti eder.</span><span class="sxs-lookup"><span data-stu-id="fee7a-254">The preceding code guarantees that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="fee7a-255">Değişiklik `Order` için `MaxValue` ve WWW etiketi için oluşturulmuş biçimlendirmeyi yanlış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="fee7a-255">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="fee7a-256">Alt içeriği almak ve İnceleme</span><span class="sxs-lookup"><span data-stu-id="fee7a-256">Inspect and retrieve child content</span></span>

<span data-ttu-id="fee7a-257">Etiket Yardımcıları içerik almak için çeşitli özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="fee7a-257">The tag helpers provide several properties to retrieve content.</span></span>

* <span data-ttu-id="fee7a-258">Sonucu `GetChildContentAsync` eklenerek `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-258">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
* <span data-ttu-id="fee7a-259">Sonucu inceleyebilirsiniz `GetChildContentAsync` ile `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="fee7a-259">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
* <span data-ttu-id="fee7a-260">Değiştirirseniz `output.Content`, TagHelper gövdesi olmaz yürütülen veya siz işlenen `GetChildContentAsync` otomatik bağlayıcı örneğimizi olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="fee7a-260">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* <span data-ttu-id="fee7a-261">Birden çok çağrılar `GetChildContentAsync` aynı değeri döndürür ve yeniden yürütülmez `TagHelper` önbelleğe alınan sonuç kullanmayı gösteren yanlış parametre geçirmezseniz gövde.</span><span class="sxs-lookup"><span data-stu-id="fee7a-261">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
