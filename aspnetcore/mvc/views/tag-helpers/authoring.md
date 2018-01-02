---
title: "ASP.NET Core içinde etiket Yardımcıları yazma"
author: rick-anderson
description: "ASP.NET Core içinde etiket Yardımcıları yazmak öğrenin."
keywords: "ASP.NET Core, etiket Yardımcıları"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cbe46ee1d3cd9f7a30a87d364074f1302f9af7ab
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="60607-104">Etiket Yardımcıları ASP.NET Core, örnekleri ile ilgili bir kılavuz olarak yazma</span><span class="sxs-lookup"><span data-stu-id="60607-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="60607-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="60607-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="60607-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60607-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="60607-107">Etiket Yardımcıları ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="60607-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="60607-108">Bu öğretici programlama etiket Yardımcıları tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="60607-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="60607-109">[Etiket Yardımcıları giriş](intro.md) etiket Yardımcıları sağlamak faydalarını anlatır.</span><span class="sxs-lookup"><span data-stu-id="60607-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="60607-110">Bir etiket Yardımcısı uygulayan herhangi bir sınıftır `ITagHelper` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="60607-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="60607-111">Bir etiket Yardımcısı yazarken, Bununla birlikte, genellikle öğesinden türetilen `TagHelper`, sizin için bunu erişmenizi yapılması `Process` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="60607-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="60607-112">Adlı yeni bir ASP.NET Core projesi oluşturma **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="60607-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="60607-113">Bu proje için kimlik doğrulama gerekmez.</span><span class="sxs-lookup"><span data-stu-id="60607-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="60607-114">Adlı etiket Yardımcıları tutmak için bir klasör oluşturun *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="60607-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="60607-115">*TagHelpers* klasörü *değil* gerekli, ancak makul bir kuraldır.</span><span class="sxs-lookup"><span data-stu-id="60607-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="60607-116">Şimdi bazı basit etiket Yardımcıları yazmak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="60607-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="60607-117">En az bir etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="60607-117">A minimal Tag Helper</span></span>

<span data-ttu-id="60607-118">Bu bölümde, bir e-posta etiketi güncelleştiren bir etiket Yardımcısı yazma.</span><span class="sxs-lookup"><span data-stu-id="60607-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="60607-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="60607-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="60607-120">Sunucu, bu biçimlendirme aşağıdaki dönüştürmek için bizim e-posta etiket Yardımcısı kullanır:</span><span class="sxs-lookup"><span data-stu-id="60607-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="60607-121">Diğer bir deyişle, bir yer işareti etiketi, bu bir e-posta bağlantısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="60607-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="60607-122">Blog altyapısıdır ve yazma ve aynı etki alanına tüm pazarlama, destek ve diğer kişiler için e-posta göndermek istiyorsanız bunu yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="60607-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="60607-123">Aşağıdakileri ekleyin `EmailTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="60607-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="60607-124">**Notlar:**</span><span class="sxs-lookup"><span data-stu-id="60607-124">**Notes:**</span></span>
    
    * <span data-ttu-id="60607-125">Etiket Yardımcıları kök sınıf adının öğeleri hedefleyen bir adlandırma kuralını kullanın (eksi *TagHelper* sınıf adı bölümü).</span><span class="sxs-lookup"><span data-stu-id="60607-125">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="60607-126">Bu örnekte, kök adı **e-posta**TagHelper olan *e-posta*, böylece `<email>` etiketi hedeflenir.</span><span class="sxs-lookup"><span data-stu-id="60607-126">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="60607-127">Bu adlandırma kuralını çoğu etiket yardımcılarında çalışması gerekir, daha sonra onu geçersiz kılmak nasıl göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="60607-127">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="60607-128">`EmailTagHelper` Sınıfı türer `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="60607-128">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="60607-129">`TagHelper` Sınıfı sağlar yöntemleri ve özellikleri etiket Yardımcıları yazmak için.</span><span class="sxs-lookup"><span data-stu-id="60607-129">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="60607-130">Geçersiz kılınan `Process` yöntemini kontrol etiket Yardımcısı çalıştırıldığında yapar.</span><span class="sxs-lookup"><span data-stu-id="60607-130">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="60607-131">`TagHelper` Sınıfı zaman uyumsuz bir sürümünü sağlar (`ProcessAsync`) aynı parametrelere sahip.</span><span class="sxs-lookup"><span data-stu-id="60607-131">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="60607-132">Bağlam parametresi `Process` (ve `ProcessAsync`) geçerli HTML etiket yürütülmesi ile ilgili bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="60607-132">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="60607-133">Çıktı parametresi `Process` (ve `ProcessAsync`) bir HTML etiketi ve içeriği oluşturmak için kullanılan özgün kaynağını temsili bir durum bilgisi olan HTML öğesi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="60607-133">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="60607-134">Bizim sınıf adı sonekine sahip **TagHelper**, olduğu *değil* gerekli, ancak en iyi yöntem kuralı dikkate almıştır.</span><span class="sxs-lookup"><span data-stu-id="60607-134">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="60607-135">Sınıf olarak bildirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="60607-135">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="60607-136">Yapmak için `EmailTagHelper` bizim tüm Razor görünümleri kullanılabilir sınıfı, ekleme `addTagHelper` için yönerge *Views/_ViewImports.cshtml* dosyası:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="60607-136">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="60607-137">Yukarıdaki kod bizim derlemesindeki tüm etiket Yardımcıları kullanılabilir olacağını belirtmek için joker karakter sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="60607-137">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="60607-138">İlk dizesinden sonra `@addTagHelper` yüklemek için etiket Yardımcısı belirtir (kullan "*" tüm etiket Yardımcıları için), ve ikinci dize "AuthoringTagHelpers" etiketi yardımcı olan derleme belirtir.</span><span class="sxs-lookup"><span data-stu-id="60607-138">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="60607-139">Ayrıca, ikinci satır joker karakter sözdizimini kullanarak ASP.NET Core MVC etiket Yardımcıları getirir unutmayın (Bu Yardımcıları ele alınmıştır [etiket Yardımcıları giriş](intro.md).) Bu `@addTagHelper` etiket Yardımcısı Razor görünüm için kullanılabilir hale getirir yönergesi.</span><span class="sxs-lookup"><span data-stu-id="60607-139">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="60607-140">Alternatif olarak, aşağıda gösterildiği gibi bir etiket Yardımcısı, tam adı (FQN) sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="60607-140">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="60607-141">Bir FQN kullanarak görünüm için bir etiket Yardımcısı eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="60607-141">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="60607-142">Çoğu geliştirici, joker karakter sözdizimi kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="60607-142">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="60607-143">[Etiket Yardımcıları giriş](intro.md) etiket Yardımcısı ekleme, kaldırma, hiyerarşi ve joker karakter sözdizimi, ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="60607-143">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="60607-144">Biçimlendirme güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası:</span><span class="sxs-lookup"><span data-stu-id="60607-144">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="60607-145">Uygulamayı çalıştırın ve e-posta etiketleri olan bağlantı biçimlendirme değiştirilir doğrulayabilmeniz için HTML kaynağını görüntülemek için sık kullanılan tarayıcınızı kullanın (örneğin, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="60607-145">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="60607-146">*Destek* ve *pazarlama* bağlantılar olarak işlenir ancak sahip olmayan bir `href` çalışır duruma getirme özniteliği.</span><span class="sxs-lookup"><span data-stu-id="60607-146">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="60607-147">Biz, sonraki bölümde düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="60607-147">We'll fix that in the next section.</span></span>

<span data-ttu-id="60607-148">Not: HTML etiketleri ve öznitelikleri, etiketler, sınıf adları ve Razor ve C# öznitelikleri gibi büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="60607-148">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="60607-149">SetAttribute ve SetContent</span><span class="sxs-lookup"><span data-stu-id="60607-149">SetAttribute and SetContent</span></span>

<span data-ttu-id="60607-150">Bu bölümde, biz güncelleştireceğim `EmailTagHelper` böylece e-posta için geçerli yer işareti etiketi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="60607-150">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="60607-151">Razor görünüm bilgilerinden gerçekleştirecek şekilde güncelleştiriyoruz (biçiminde bir `mail-to` özniteliği) ve bağlantı oluşturma kullanan.</span><span class="sxs-lookup"><span data-stu-id="60607-151">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="60607-152">Güncelleştirme `EmailTagHelper` aşağıdaki sınıfı:</span><span class="sxs-lookup"><span data-stu-id="60607-152">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="60607-153">**Notlar:**</span><span class="sxs-lookup"><span data-stu-id="60607-153">**Notes:**</span></span>

* <span data-ttu-id="60607-154">İçine çevrilmiş Pascal ortası sınıf ve özellik adlarını etiket Yardımcıları için kendi [alt kebab durumda](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="60607-154">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="60607-155">Bu nedenle, kullanılacak `MailTo` kullanın, öznitelik `<email mail-to="value"/>` eşdeğer.</span><span class="sxs-lookup"><span data-stu-id="60607-155">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="60607-156">Son satırı tamamlanmış içerik bizim için en düşük düzeyde işlev etiket Yardımcısı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="60607-156">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="60607-157">Vurgulanan satırı öznitelikler eklemek için sözdizimi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="60607-157">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="60607-158">Şu anda öznitelikleri koleksiyonunda yok sürece bu yaklaşımı "href" özniteliği için çalışır.</span><span class="sxs-lookup"><span data-stu-id="60607-158">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="60607-159">Aynı zamanda `output.Attributes.Add` yöntemi etiketi yardımcı öznitelik etiketi özniteliklerin koleksiyonu sonuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="60607-159">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="60607-160">Biçimlendirme güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="60607-160">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="60607-161">Uygulamayı çalıştırın ve doğru bağlantıları oluşturur doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="60607-161">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="60607-162">E-posta otomatik olarak kapanış etiketi yazmak için olsaydı (`<email mail-to="Rick" />`), son çıkışı da kendi kendine kapanan olacaktır.</span><span class="sxs-lookup"><span data-stu-id="60607-162">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="60607-163">Yalnızca bir başlangıç etiketi etiketiyle yazma özelliği etkinleştirmek için (`<email mail-to="Rick">`) aşağıdaki sınıfıyla tasarlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="60607-163">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="60607-164">Bir kendi kendine kapanan e-posta etiket Yardımcısı ile çıkış olacaktır `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="60607-164">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="60607-165">Kendi kendine kapanan bağlayıcı etiketler geçerli HTML oluşturmak istemezsiniz ancak kendi kendine kapanan etiketi yardımcı oluşturmak isteyebilirsiniz değildir.</span><span class="sxs-lookup"><span data-stu-id="60607-165">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="60607-166">Etiket Yardımcıları türünü ayarlayın `TagMode` bir etiket okuma sonra özelliği.</span><span class="sxs-lookup"><span data-stu-id="60607-166">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="60607-167">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="60607-167">ProcessAsync</span></span>

<span data-ttu-id="60607-168">Bu bölümde, biz bir zaman uyumsuz e-posta Yardımcısı yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="60607-168">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="60607-169">Değiştir `EmailTagHelper` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="60607-169">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="60607-170">**Notlar:**</span><span class="sxs-lookup"><span data-stu-id="60607-170">**Notes:**</span></span>

    * <span data-ttu-id="60607-171">Bu sürümü zaman uyumsuz kullanan `ProcessAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="60607-171">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="60607-172">Zaman uyumsuz `GetChildContentAsync` döndüren bir `Task` içeren `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="60607-172">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="60607-173">Kullanım `output` HTML öğesi içeriğini almak için parametre.</span><span class="sxs-lookup"><span data-stu-id="60607-173">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="60607-174">Aşağıdaki değişiklik *Views/Home/Contact.cshtml* etiket Yardımcısı hedef e-posta sınıflandırıp dosya.</span><span class="sxs-lookup"><span data-stu-id="60607-174">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="60607-175">Uygulamayı çalıştırın ve geçerli e-posta bağlantılar oluşturup doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="60607-175">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="60607-176">RemoveAll, PreContent.SetHtmlContent ve PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="60607-176">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="60607-177">Aşağıdakileri ekleyin `BoldTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="60607-177">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="60607-178">**Notlar:**</span><span class="sxs-lookup"><span data-stu-id="60607-178">**Notes:**</span></span>
    
    * <span data-ttu-id="60607-179">`[HtmlTargetElement]` Öznitelik HTML özniteliği içeren herhangi bir HTML öğesi "kalın" adlı belirten bir öznitelik parametre eşleşir, geçişleri ve `Process` sınıfında geçersiz kılma yöntemi çalışır.</span><span class="sxs-lookup"><span data-stu-id="60607-179">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="60607-180">Bizim örnek `Process` yöntemi "kalın" özniteliğini kaldırır ve içeren biçimlendirme ile çevreleyen `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="60607-180">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="60607-181">İçerik varolan etiketi değiştirmek istemeyeceğiniz için açılış yazmalısınız `<strong>` ile etiketi `PreContent.SetHtmlContent` yöntemi ve kapanış `</strong>` ile etiketi `PostContent.SetHtmlContent` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="60607-181">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="60607-182">Değiştirme *About.cshtml* görünüm içeren bir `bold` öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="60607-182">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="60607-183">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="60607-183">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="60607-184">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="60607-184">Run the app.</span></span> <span data-ttu-id="60607-185">Sık kullanılan tarayıcınız kaynak inceleyin ve biçimlendirmesini doğrulamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="60607-185">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="60607-186">`[HtmlTargetElement]` Yukarıda öznitelik yalnızca hedef bir özniteliğin adını "kalın" sağlayan HTML biçimlendirmesi.</span><span class="sxs-lookup"><span data-stu-id="60607-186">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="60607-187">`<bold>` Öğesi değil etiket Yardımcısı tarafından değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="60607-187">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="60607-188">Out açıklama `[HtmlTargetElement]` özniteliği satır ve varsayılan olarak hedefleniyor `<bold>` etiketleri, diğer bir deyişle, HTML biçimlendirmesi biçiminde `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="60607-188">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="60607-189">Unutmayın, varsayılan adlandırma kuralını sınıf adı eşleşir **kalın**TagHelper için `<bold>` etiketler.</span><span class="sxs-lookup"><span data-stu-id="60607-189">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="60607-190">Uygulamayı çalıştırın ve doğrulayın `<bold>` etiket etiket Yardımcısı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="60607-190">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="60607-191">Birden çok sınıfıyla dekorasyon `[HtmlTargetElement]` hedefleri veya mantıksal sonuçlarını öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="60607-191">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="60607-192">Örneğin, aşağıdaki kodu kullanarak, bir kalın etiketi veya kalın özniteliği ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="60607-192">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="60607-193">Birden çok öznitelik aynı ifadesine eklendiğinde, çalışma zamanı bunları bir mantıksal-and değerlendirir</span><span class="sxs-lookup"><span data-stu-id="60607-193">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="60607-194">Örneğin kodda bir HTML öğesi "kalın" adlı bir özniteliği "bold" olarak adlandırılmalıdır (`<bold bold />`) eşleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="60607-194">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="60607-195">Aynı zamanda `[HtmlTargetElement]` hedeflenen öğesinin adını değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="60607-195">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="60607-196">İsterseniz örneğin `BoldTagHelper` hedef `<MyBold>` etiketleri, aşağıdaki öznitelik kullanırsınız:</span><span class="sxs-lookup"><span data-stu-id="60607-196">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="60607-197">Bir model için bir etiket Yardımcısı geçirme</span><span class="sxs-lookup"><span data-stu-id="60607-197">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="60607-198">Ekleme bir *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="60607-198">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="60607-199">Aşağıdakileri ekleyin `WebsiteContext` sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="60607-199">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="60607-200">Aşağıdakileri ekleyin `WebsiteInformationTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="60607-200">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="60607-201">**Notlar:**</span><span class="sxs-lookup"><span data-stu-id="60607-201">**Notes:**</span></span>
    
    * <span data-ttu-id="60607-202">Daha önce belirtildiği gibi etiket Yardımcıları çevirir Pascal ortası C# sınıf adları ve özellikleri etiket Yardımcıları için [alt kebab durumda](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="60607-202">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="60607-203">Bu nedenle, kullanılacak `WebsiteInformationTagHelper` Razor içinde yazacaksınız `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="60607-203">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="60607-204">Açıkça hedef öğeyle belirlemek değil `[HtmlTargetElement]` özniteliği, bunu varsayılan değer olan `website-information` hedeflenir.</span><span class="sxs-lookup"><span data-stu-id="60607-204">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="60607-205">Aşağıdaki öznitelik (kebab söz konusu değildir ancak sınıfı adıyla eşleşen unutmayın) uyguladıysanız:</span><span class="sxs-lookup"><span data-stu-id="60607-205">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="60607-206">Daha düşük kebab durum etiketini `<website-information />` değil eşleşir.</span><span class="sxs-lookup"><span data-stu-id="60607-206">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="60607-207">Kullanmak istiyorsanız, `[HtmlTargetElement]` özniteliği, aşağıda gösterildiği gibi kebab durumda kullanmak:</span><span class="sxs-lookup"><span data-stu-id="60607-207">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="60607-208">Kendi kendine kapanan öğe hiçbir içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="60607-208">Elements that are self-closing have no content.</span></span> <span data-ttu-id="60607-209">Bu örnekte, kendi kendine kapanan etiketi Razor biçimlendirme kullanır, ancak etiket Yardımcısı oluşturma bir [bölüm](http://www.w3.org/TR/html5/sections.html#the-section-element) öğesi (kendi kendine kapanan değil ve içeriğini yazıyorsanız `section` öğesi).</span><span class="sxs-lookup"><span data-stu-id="60607-209">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="60607-210">Bu nedenle, ayarlamanız gerekir `TagMode` için `StartTagAndEndTag` çıkışını yazmak için.</span><span class="sxs-lookup"><span data-stu-id="60607-210">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="60607-211">Satır ayarını WMI'ya alternatif olarak, açıklama `TagMode` ve kapanış etiketi ile işaretleme yazma.</span><span class="sxs-lookup"><span data-stu-id="60607-211">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="60607-212">(Örneğin biçimlendirme daha sonra Bu öğreticide sağlanır.)</span><span class="sxs-lookup"><span data-stu-id="60607-212">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="60607-213">`$` (Dolar işareti) aşağıdaki satırda kullanan bir [Ara değerli dize](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="60607-213">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="60607-214">Aşağıdaki biçimlendirmeleri eklemek *About.cshtml* görünümü.</span><span class="sxs-lookup"><span data-stu-id="60607-214">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="60607-215">Vurgulanan biçimlendirme web sitesi bilgilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="60607-215">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="60607-216">Aşağıda gösterilen Razor biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="60607-216">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="60607-217">Razor bilir `info` özniteliği bir sınıf, bir dize değil ve C# kod yazmak istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="60607-217">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="60607-218">Herhangi bir dize olmayan etiketi yardımcı öznitelik olmadan yazılması gereken `@` karakter.</span><span class="sxs-lookup"><span data-stu-id="60607-218">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="60607-219">Uygulamayı çalıştırın ve web sitesi bilgileri görmek için hakkında görünümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="60607-219">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="60607-220">Aşağıdaki biçimlendirmede kapanış etiketi ile kullanın ve içeren satırı Kaldır `TagMode.StartTagAndEndTag` etiket Yardımcısı içinde:</span><span class="sxs-lookup"><span data-stu-id="60607-220">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="60607-221">Koşul etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="60607-221">Condition Tag Helper</span></span>

<span data-ttu-id="60607-222">Koşul etiket Yardımcısı true değeri geçirildiğinde çıkış işler.</span><span class="sxs-lookup"><span data-stu-id="60607-222">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="60607-223">Aşağıdakileri ekleyin `ConditionTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="60607-223">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="60607-224">Değiştir *Views/Home/Index.cshtml* aşağıdaki biçimlendirme dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="60607-224">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="60607-225">Değiştir `Index` yönteminde `Home` aşağıdaki kodla denetleyicisi:</span><span class="sxs-lookup"><span data-stu-id="60607-225">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="60607-226">Uygulamayı çalıştırın ve giriş sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="60607-226">Run the app and browse to the home page.</span></span> <span data-ttu-id="60607-227">Koşullu biçimlendirme `div` değil işlenir.</span><span class="sxs-lookup"><span data-stu-id="60607-227">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="60607-228">Sorgu dizesi eklemek `?approved=true` URL (örneğin, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="60607-228">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="60607-229">`approved`TRUE olarak ve koşullu biçimlendirme görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="60607-229">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="60607-230">Kullanım [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) işleci kalın etiket Yardımcısı'nı kullanarak yaptığınız gibi bir dize belirtme yerine hedef özniteliği belirtin:</span><span class="sxs-lookup"><span data-stu-id="60607-230">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="60607-231">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) işleci korumak kodu herhangi bir zamanda yeniden (biz adına değiştirmek isteyebileceğiniz `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="60607-231">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="60607-232">Etiket Yardımcısı çakışmaları önleme</span><span class="sxs-lookup"><span data-stu-id="60607-232">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="60607-233">Bu bölümde, etiket Yardımcıları otomatik bağlama çifti yazma.</span><span class="sxs-lookup"><span data-stu-id="60607-233">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="60607-234">İlk biçimlendirme içeren bir HTML yer işareti etiketi aynı URL'sini içeren (ve bu nedenle bağlantı URL'sini verir) HTTP ile başlayan URL yerini alır.</span><span class="sxs-lookup"><span data-stu-id="60607-234">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="60607-235">İkinci aynı URL'sini WWW ile başlayan yapın.</span><span class="sxs-lookup"><span data-stu-id="60607-235">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="60607-236">Bu iki Yardımcıları yakından ilişkili olan ve gelecekte yeniden çünkü biz aynı dosyada tutarsınız.</span><span class="sxs-lookup"><span data-stu-id="60607-236">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="60607-237">Aşağıdakileri ekleyin `AutoLinkerHttpTagHelper` sınıfının *TagHelpers* klasör.</span><span class="sxs-lookup"><span data-stu-id="60607-237">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="60607-238">`AutoLinkerHttpTagHelper` Sınıfı hedefler `p` öğeleri ve kullandığı [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) bağlantı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="60607-238">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="60607-239">Aşağıdaki biçimlendirmede sonuna ekleyin *Views/Home/Contact.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="60607-239">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="60607-240">Uygulamayı çalıştırın ve etiket Yardımcısı bağlantı düzgün işler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="60607-240">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="60607-241">Güncelleştirme `AutoLinker` eklemek için sınıfı `AutoLinkerWwwTagHelper` hangi www Metne Dönüştür Ayrıca özgün www metin içeren bir yer işareti etiketi.</span><span class="sxs-lookup"><span data-stu-id="60607-241">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="60607-242">Güncelleştirilmiş kod aşağıda vurgulanmıştır:</span><span class="sxs-lookup"><span data-stu-id="60607-242">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="60607-243">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="60607-243">Run the app.</span></span> <span data-ttu-id="60607-244">Www metin bağlantı olarak işlenir ancak HTTP metin değil dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="60607-244">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="60607-245">Her iki sınıflarda bir kesme noktası yerleştirirseniz HTTP etiket Yardımcısı sınıfı ilk çalıştığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="60607-245">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="60607-246">, Etiket Yardımcısı çıkış önbelleğe alınır ve WWW etiket Yardımcısı çalıştırdığınızda, önbelleğe alınan çıktısı HTTP etiket Yardımcısı, üzerine yazar sorunudur.</span><span class="sxs-lookup"><span data-stu-id="60607-246">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="60607-247">Etiket Yardımcıları Çalıştır sırasını denetlemek nasıl daha sonra öğreticide göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="60607-247">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="60607-248">Size kodu aşağıdakilerle düzeltme:</span><span class="sxs-lookup"><span data-stu-id="60607-248">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="60607-249">Aşağıdaki kod ile hedef içeriği aldığınız otomatik bağlama etiket Yardımcıları ilk sürümü:</span><span class="sxs-lookup"><span data-stu-id="60607-249">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="60607-250">Diğer bir deyişle, çağrı `GetChildContentAsync` kullanarak `TagHelperOutput` içine geçirilen `ProcessAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="60607-250">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="60607-251">Belirtildiği gibi daha önce çıktıyı önbelleğe olduğundan, son etiket WINS çalışmasına yardımcı.</span><span class="sxs-lookup"><span data-stu-id="60607-251">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="60607-252">Aşağıdaki kod ile bu sorun düzeltilmiştir:</span><span class="sxs-lookup"><span data-stu-id="60607-252">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="60607-253">Yukarıdaki kod içeriği değiştirilmiş ve varsa, çıktı arabelleğinden içeriği alır bakar.</span><span class="sxs-lookup"><span data-stu-id="60607-253">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="60607-254">Uygulamayı çalıştırın ve iki bağlantı beklendiği gibi çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="60607-254">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="60607-255">Bizim otomatik bağlayıcı etiket Yardımcısı doğru ve eksiksiz görünebilir, ancak zarif bir sorun vardır.</span><span class="sxs-lookup"><span data-stu-id="60607-255">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="60607-256">WWW etiket Yardımcısı ilk çalıştırıyorsa, www bağlantılar doğru olmaz.</span><span class="sxs-lookup"><span data-stu-id="60607-256">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="60607-257">Kodu ekleyerek güncelleştirme `Order` etiket çalışan sırasını denetlemek için aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="60607-257">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="60607-258">`Order` Özelliği aynı öğeye hedefleme diğer etiket Yardımcıları göre yürütme sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="60607-258">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="60607-259">Varsayılan sıra değeri sıfırdır ve düşük değerler örnekleriyle önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="60607-259">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="60607-260">Yukarıdaki kod HTTP etiket Yardımcısı WWW etiket Yardımcısı önce çalışır garanti.</span><span class="sxs-lookup"><span data-stu-id="60607-260">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="60607-261">Değişiklik `Order` için `MaxValue` ve WWW etiketi için oluşturulan biçimlendirme yanlış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="60607-261">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="60607-262">İnceleme ve alt içerik alınıyor</span><span class="sxs-lookup"><span data-stu-id="60607-262">Inspecting and retrieving child content</span></span>

<span data-ttu-id="60607-263">Etiket Yardımcıları içerik almak için çeşitli özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="60607-263">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="60607-264">Sonucu `GetChildContentAsync` için eklenen `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="60607-264">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="60607-265">Sonucu inceleyebilirsiniz `GetChildContentAsync` ile `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="60607-265">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="60607-266">Değiştirirseniz `output.Content`, TagHelper gövde yürütülen veya çağırmanız sürece çizilir `GetChildContentAsync` otomatik bağlayıcı örneğimizde olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="60607-266">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="60607-267">Birden çok çağrılar `GetChildContentAsync` aynı değere döndürür ve yeniden yürütülemeyecek `TagHelper` yanlış parametre kullanılmıyor belirten önbelleğe alınan sonuç geçirmezseniz gövde.</span><span class="sxs-lookup"><span data-stu-id="60607-267">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
