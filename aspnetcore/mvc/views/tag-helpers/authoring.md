---
title: ASP.NET core'da Yazar etiket Yardımcıları
author: rick-anderson
description: ASP.NET core'da etiket Yardımcıları yazmak öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 43bd4eccfc06d27ade5de0e3387247a753609336
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662374"
---
# <a name="author-tag-helpers-in-aspnet-core"></a><span data-ttu-id="d750c-103">ASP.NET core'da Yazar etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="d750c-103">Author Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="d750c-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d750c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d750c-105">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d750c-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="d750c-106">Etiket Yardımcıları ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d750c-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="d750c-107">Bu öğreticide programlama etiket Yardımcıları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="d750c-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="d750c-108">[Etiket yardımcılarına giriş](intro.md) , etiket yardımcılarının sağladığı avantajları açıklar.</span><span class="sxs-lookup"><span data-stu-id="d750c-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="d750c-109">Etiket Yardımcısı `ITagHelper` arabirimini uygulayan herhangi bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="d750c-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="d750c-110">Ancak, bir etiket Yardımcısı yazdığınızda genellikle `TagHelper`' den türetirsiniz, bunu yapmanız `Process` yöntemine erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="d750c-111">**Authoringtaghelmakası**adlı yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d750c-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="d750c-112">Bu proje için kimlik doğrulaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d750c-112">You won't need authentication for this project.</span></span>

1. <span data-ttu-id="d750c-113">*Taghelmakası*adlı etiket yardımcılarını tutacak bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d750c-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="d750c-114">*Taghelmakacıları* klasörü gerekli *değildir* , ancak makul bir kuraldır.</span><span class="sxs-lookup"><span data-stu-id="d750c-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="d750c-115">Şimdi bazı basit etiket Yardımcıları yazmaya başlayalım.</span><span class="sxs-lookup"><span data-stu-id="d750c-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="d750c-116">En az bir etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="d750c-116">A minimal Tag Helper</span></span>

<span data-ttu-id="d750c-117">Bu bölümde, bir e-posta etiketi güncelleştiren etiket Yardımcısı yazmaya.</span><span class="sxs-lookup"><span data-stu-id="d750c-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="d750c-118">Örnek:</span><span class="sxs-lookup"><span data-stu-id="d750c-118">For example:</span></span>

```html
<email>Support</email>
```

<span data-ttu-id="d750c-119">Sunucu, bizim e-posta etiketi Yardımcısı, biçimlendirme ve bunlar aşağıdaki şekilde dönüştürmek için kullanır:</span><span class="sxs-lookup"><span data-stu-id="d750c-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="d750c-120">Diğer bir deyişle, bir yer işareti etiketi, bu e-posta bağlantısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="d750c-121">Blog altyapısıdır ve yazma ve aynı etki alanı için pazarlama desteği ve diğer kişiler için e-posta göndermek istiyorsanız bunu yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1. <span data-ttu-id="d750c-122">Aşağıdaki `EmailTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d750c-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * <span data-ttu-id="d750c-123">Etiket Yardımcıları, kök sınıf adı öğelerini hedefleyen bir adlandırma kuralı kullanır (sınıf adının sonunda *Taghelof* kısmı).</span><span class="sxs-lookup"><span data-stu-id="d750c-123">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="d750c-124">Bu örnekte, **Emailtaghelper** 'nin kök adı *e-postadır*, bu nedenle `<email>` etiketi hedeflenmiş olur.</span><span class="sxs-lookup"><span data-stu-id="d750c-124">In this example, the root name of **EmailTagHelper** is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="d750c-125">Bu adlandırma çoğu etiket Yardımcıları için çalışır, daha sonra bu geçersiz kılma göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="d750c-125">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>

   * <span data-ttu-id="d750c-126">`EmailTagHelper` sınıfı `TagHelper`türetilir.</span><span class="sxs-lookup"><span data-stu-id="d750c-126">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="d750c-127">`TagHelper` sınıfı, etiket yardımcıları yazmak için yöntemler ve özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-127">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>

   * <span data-ttu-id="d750c-128">Geçersiz kılınan `Process` yöntemi, yürütüldüğünde etiket Yardımcısı 'nın ne yaptığını denetler.</span><span class="sxs-lookup"><span data-stu-id="d750c-128">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="d750c-129">`TagHelper` sınıfı ayrıca aynı parametrelerle zaman uyumsuz bir sürüm (`ProcessAsync`) sağlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-129">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>

   * <span data-ttu-id="d750c-130">`Process` (ve `ProcessAsync`) için bağlam parametresi, geçerli HTML etiketinin yürütülmesiyle ilişkili bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="d750c-130">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>

   * <span data-ttu-id="d750c-131">`Process` (ve `ProcessAsync`) çıkış parametresi, bir HTML etiketi ve içeriği oluşturmak için kullanılan orijinal kaynağın durum bilgisi olan HTML öğesi temsilcisini içerir.</span><span class="sxs-lookup"><span data-stu-id="d750c-131">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>

   * <span data-ttu-id="d750c-132">Sınıfımızın adı, gerekli *olmayan* bir **ıghelper**sonekine sahiptir ancak en iyi yöntem kuralı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d750c-132">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="d750c-133">Sınıf olarak bildirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d750c-133">You could declare the class as:</span></span>

   ```csharp
   public class Email : TagHelper
   ```

1. <span data-ttu-id="d750c-134">`EmailTagHelper` sınıfını tüm Razor görünümlerimize kullanılabilir hale getirmek için, `addTagHelper` yönergesini *views/_ViewImports. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d750c-134">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file:</span></span>

   [!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]

   <span data-ttu-id="d750c-135">Yukarıdaki kod, bizim derlemedeki tüm etiket Yardımcıları kullanılabilir olacağını belirtmek için joker karakter sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="d750c-135">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="d750c-136">`@addTagHelper` sonraki ilk dize, yüklenecek etiket yardımcısını (tüm etiket yardımcıları için "\*" kullanın) ve ikinci dize olan "Authoringtaghelmakaı", etiket Yardımcısı 'nın bulunduğu derlemeyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="d750c-136">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="d750c-137">Ayrıca, ikinci satırın joker karakter söz dizimini kullanarak ASP.NET Core MVC etiket yardımcılarına getirdiğine unutmayın (Bu yardımcılar [etiket yardımcılarına giriş](intro.md)bölümünde ele alınmıştır.) Bu, etiket yardımcısını Razor görünümü için kullanılabilir hale getiren `@addTagHelper` yönergedir.</span><span class="sxs-lookup"><span data-stu-id="d750c-137">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="d750c-138">Alternatif olarak, aşağıda gösterildiği gibi bir etiket Yardımcısı'nın (FQN) tam nitelikli ad sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d750c-138">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

<span data-ttu-id="d750c-139">FQN kullanarak bir görünüme bir etiket Yardımcısı eklemek için, önce FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından **derleme adı** (*authoringtaghel,* `namespace`) eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-139">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the **assembly name** (*AuthoringTagHelpers*, not necessarily the `namespace`).</span></span> <span data-ttu-id="d750c-140">Çoğu geliştirici, joker karakter sözdizimini kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="d750c-140">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="d750c-141">[Etiket yardımcılarına giriş](intro.md) , etiket Yardımcısı ekleme, kaldırma, hiyerarşi ve joker karakter sözdizimi hakkında ayrıntılı bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-141">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>

1. <span data-ttu-id="d750c-142">*Görünümler/Home/Contact. cshtml* dosyasındaki biçimlendirmeyi şu değişikliklerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d750c-142">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="d750c-143">Uygulamayı çalıştırın ve HTML kaynağını görüntülemek için en sevdiğiniz tarayıcıyı kullanın, böylece e-posta etiketlerinin yer işareti işaretlemesi ile değiştirildiğini doğrulayabilirsiniz (örneğin, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="d750c-143">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="d750c-144">*Destek* ve *Pazarlama* bir bağlantı olarak işlenir, ancak bunları işlevsel hale getirmek için `href` bir özniteliğe sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="d750c-144">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="d750c-145">Sonraki bölümde, gidereceğiz.</span><span class="sxs-lookup"><span data-stu-id="d750c-145">We'll fix that in the next section.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="d750c-146">SetAttribute ve SetContent</span><span class="sxs-lookup"><span data-stu-id="d750c-146">SetAttribute and SetContent</span></span>

<span data-ttu-id="d750c-147">Bu bölümde, e-posta için geçerli bir tutturucu etiketi oluşturacak şekilde `EmailTagHelper` güncelleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="d750c-147">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="d750c-148">Bunu, Razor görünümünden bilgi almak (`mail-to` özniteliği biçiminde) ve bağlayıcıyı oluştururken kullanmak için güncelleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="d750c-148">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="d750c-149">`EmailTagHelper` sınıfını şu şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d750c-149">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* <span data-ttu-id="d750c-150">Etiket Yardımcıları için Pascal özellikli sınıf ve özellik adları, [Kebab durumlarına](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)çevrilir.</span><span class="sxs-lookup"><span data-stu-id="d750c-150">Pascal-cased class and property names for tag helpers are translated into their [kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="d750c-151">Bu nedenle, `MailTo` özniteliğini kullanmak için `<email mail-to="value"/>` eşdeğerini kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="d750c-151">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="d750c-152">Son satırı tamamlanmış içeriği bizim için en düşük düzeyde işlevsel etiket Yardımcısı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-152">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="d750c-153">Vurgulanan satır öznitelikleri eklemek için sözdizimini gösterir:</span><span class="sxs-lookup"><span data-stu-id="d750c-153">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="d750c-154">Şu anda öznitelikleri koleksiyonda yok sürece bu yaklaşımı "href =" özniteliği için çalışır.</span><span class="sxs-lookup"><span data-stu-id="d750c-154">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="d750c-155">Etiket öznitelikleri koleksiyonunun sonuna bir etiket Yardımcısı özniteliği eklemek için `output.Attributes.Add` yöntemini de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-155">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1. <span data-ttu-id="d750c-156">*Görünümler/Home/Contact. cshtml* dosyasındaki biçimlendirmeyi şu değişikliklerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d750c-156">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

1. <span data-ttu-id="d750c-157">Uygulamayı çalıştırın ve doğrulayın, doğru bağlantıları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d750c-157">Run the app and verify that it generates the correct links.</span></span>

<a name="self-closing"></a>

   > [!NOTE]
   > <span data-ttu-id="d750c-158">E-posta etiketi kendinden kapanış (`<email mail-to="Rick" />`) yazarsanız, nihai çıkış de kendi kendini kapatıyor.</span><span class="sxs-lookup"><span data-stu-id="d750c-158">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="d750c-159">Etiketi yalnızca bir başlangıç etiketiyle (`<email mail-to="Rick">`) yazma özelliğini etkinleştirmek için, sınıfı şu şekilde işaretlemeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="d750c-159">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must mark the class with the following:</span></span>
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   <span data-ttu-id="d750c-160">Kendi kendine kapanan bir e-posta etiketi Yardımcısı ile çıkış `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="d750c-160">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="d750c-161">Kendi kendine kapanan bağlantı etiketler geçerli HTML oluşturmak istemezsiniz, ancak kendi kendine kapanan etiket Yardımcısı oluşturmak isteyebilirsiniz değildir.</span><span class="sxs-lookup"><span data-stu-id="d750c-161">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="d750c-162">Etiket Yardımcıları, bir etiketi okuduktan sonra `TagMode` özelliğinin türünü ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-162">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>

### <a name="processasync"></a><span data-ttu-id="d750c-163">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="d750c-163">ProcessAsync</span></span>

<span data-ttu-id="d750c-164">Bu bölümde, biz bir zaman uyumsuz bir e-posta yardımcı yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d750c-164">In this section, we'll write an asynchronous email helper.</span></span>

1. <span data-ttu-id="d750c-165">`EmailTagHelper` sınıfını aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d750c-165">Replace the `EmailTagHelper` class with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   <span data-ttu-id="d750c-166">**Notlar:**</span><span class="sxs-lookup"><span data-stu-id="d750c-166">**Notes:**</span></span>

   * <span data-ttu-id="d750c-167">Bu sürüm, zaman uyumsuz `ProcessAsync` yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="d750c-167">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="d750c-168">Zaman uyumsuz `GetChildContentAsync`, `TagHelperContent`içeren bir `Task` döndürür.</span><span class="sxs-lookup"><span data-stu-id="d750c-168">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

   * <span data-ttu-id="d750c-169">HTML öğesinin içeriğini almak için `output` parametresini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d750c-169">Use the `output` parameter to get contents of the HTML element.</span></span>

1. <span data-ttu-id="d750c-170">Etiket Yardımcısı 'nın hedef e-postayı kullanabilmesi için *Görünümler/giriş/ilgili. cshtml* dosyasında aşağıdaki değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="d750c-170">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="d750c-171">Uygulamayı çalıştırın ve geçerli bir e-posta bağlantılarını oluşturur doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d750c-171">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="d750c-172">RemoveAll ve PreContent.SetHtmlContent PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="d750c-172">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1. <span data-ttu-id="d750c-173">Aşağıdaki `BoldTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d750c-173">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * <span data-ttu-id="d750c-174">`[HtmlTargetElement]` özniteliği, "Bold" adlı bir HTML özniteliği içeren herhangi bir HTML öğesinin eşleşeceğini ve sınıftaki `Process` geçersiz kılma yönteminin çalıştırılacağını belirten bir öznitelik parametresi geçirir.</span><span class="sxs-lookup"><span data-stu-id="d750c-174">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="d750c-175">Örneğimizde, `Process` yöntemi "Bold" özniteliğini kaldırır ve içeren biçimlendirmeyi `<strong></strong>`ile çevreler.</span><span class="sxs-lookup"><span data-stu-id="d750c-175">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>

   * <span data-ttu-id="d750c-176">Varolan etiket içeriğini değiştirmek istemediğiniz için, açma `<strong>` etiketini `PreContent.SetHtmlContent` yöntemi ve Closing `</strong>` etiketiyle `PostContent.SetHtmlContent` yöntemiyle yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d750c-176">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>

1. <span data-ttu-id="d750c-177">*About. cshtml* görünümünü bir `bold` özniteliği değeri içerecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d750c-177">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="d750c-178">Tamamlanan kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d750c-178">The completed code is shown below.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. <span data-ttu-id="d750c-179">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d750c-179">Run the app.</span></span> <span data-ttu-id="d750c-180">Kaynak inceleyin ve biçimlendirme doğrulamak için sık kullandığınız tarayıcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-180">You can use your favorite browser to inspect the source and verify the markup.</span></span>

   <span data-ttu-id="d750c-181">Yukarıdaki `[HtmlTargetElement]` özniteliği yalnızca "Bold" öznitelik adını sağlayan HTML işaretlemesini hedefler.</span><span class="sxs-lookup"><span data-stu-id="d750c-181">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="d750c-182">`<bold>` öğesi etiket Yardımcısı tarafından değiştirilmedi.</span><span class="sxs-lookup"><span data-stu-id="d750c-182">The `<bold>` element wasn't modified by the tag helper.</span></span>

1. <span data-ttu-id="d750c-183">`[HtmlTargetElement]` öznitelik satırına açıklama ekleyin ve `<bold>` etiketlerin hedeflenmesi, diğer bir deyişle, form `<bold>`HTML biçimlendirmesi.</span><span class="sxs-lookup"><span data-stu-id="d750c-183">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="d750c-184">Varsayılan adlandırma kuralının, `<bold>` etiketleri ile **kalın**taghelper sınıf adıyla eşleştiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d750c-184">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

1. <span data-ttu-id="d750c-185">Uygulamayı çalıştırın ve `<bold>` etiketinin etiket Yardımcısı tarafından işlendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d750c-185">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="d750c-186">Birden çok `[HtmlTargetElement]` özniteliği olan bir sınıfı dekorasyon, hedeflerin mantıksal veya mantıksal bir sonucu olarak sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="d750c-186">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="d750c-187">Örneğin, aşağıdaki kodu kullanarak, kalın bir etiket veya bold özniteliğinin eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d750c-187">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="d750c-188">Birden çok öznitelik için aynı deyimde eklendiğinde, çalışma zamanı bunları bir mantıksal-and davranır</span><span class="sxs-lookup"><span data-stu-id="d750c-188">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="d750c-189">Örneğin, aşağıdaki kodda, bir HTML öğesinin eşleşmesi için "Bold" (`<bold bold />`) adlı bir özniteliğe sahip "Bold" olarak adlandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d750c-189">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="d750c-190">Ayrıca, hedeflenen öğenin adını değiştirmek için `[HtmlTargetElement]` de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-190">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="d750c-191">Örneğin, `BoldTagHelper` `<MyBold>` etiketleri hedeflemesini istediyseniz, aşağıdaki özniteliği kullanacaksınız:</span><span class="sxs-lookup"><span data-stu-id="d750c-191">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="d750c-192">Etiket Yardımcısı için bir model geçirin</span><span class="sxs-lookup"><span data-stu-id="d750c-192">Pass a model to a Tag Helper</span></span>

1. <span data-ttu-id="d750c-193">*Modeller* klasörü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d750c-193">Add a *Models* folder.</span></span>

1. <span data-ttu-id="d750c-194">Aşağıdaki `WebsiteContext` sınıfını *modeller* klasörüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d750c-194">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. <span data-ttu-id="d750c-195">Aşağıdaki `WebsiteInformationTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d750c-195">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * <span data-ttu-id="d750c-196">Daha önce belirtildiği gibi, etiket yardımcıları, etiket yardımcıları için C# Pascal özellikli sınıf adlarını ve özellikleri [Kebab örneğine](https://wiki.c2.com/?KebabCase)çevirir.</span><span class="sxs-lookup"><span data-stu-id="d750c-196">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [kebab case](https://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="d750c-197">Bu nedenle `WebsiteInformationTagHelper` Razor 'de kullanmak için `<website-information />`yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d750c-197">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>

   * <span data-ttu-id="d750c-198">Hedef öğeyi `[HtmlTargetElement]` özniteliğiyle açıkça tanımlamamanız, bu nedenle `website-information` varsayılan olarak hedeflenecek.</span><span class="sxs-lookup"><span data-stu-id="d750c-198">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="d750c-199">Aşağıdaki özniteliği (kebab durum geçerli değildir ama sınıfın adıyla eşleşen unutmayın) uygulandığında:</span><span class="sxs-lookup"><span data-stu-id="d750c-199">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   <span data-ttu-id="d750c-200">Kebab Case etiketi `<website-information />` eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="d750c-200">The kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="d750c-201">`[HtmlTargetElement]` özniteliğini kullanmak istiyorsanız, aşağıda gösterildiği gibi Kebab durumunu kullanacaksınız:</span><span class="sxs-lookup"><span data-stu-id="d750c-201">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * <span data-ttu-id="d750c-202">Kendi kendine kapanan öğeler hiçbir içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="d750c-202">Elements that are self-closing have no content.</span></span> <span data-ttu-id="d750c-203">Bu örnekte, Razor biçimlendirmesi bir kendi kendine kapatma etiketi kullanacaktır, ancak etiket Yardımcısı bir [bölüm](https://www.w3.org/TR/html5/sections.html#the-section-element) öğesi oluşturacak (kendi kendini kapatmakta ve `section` öğesi içinde içerik yazıyor).</span><span class="sxs-lookup"><span data-stu-id="d750c-203">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](https://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="d750c-204">Bu nedenle, çıkış yazmak için `StartTagAndEndTag` `TagMode` ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d750c-204">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="d750c-205">Alternatif olarak, `TagMode` satır ayarı ve bir kapanış etiketiyle biçimlendirme yazma ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-205">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="d750c-206">(Daha sonra Bu öğreticide örnek biçimlendirme sağlanır.)</span><span class="sxs-lookup"><span data-stu-id="d750c-206">(Example markup is provided later in this tutorial.)</span></span>

   * <span data-ttu-id="d750c-207">Aşağıdaki satırdaki `$` (dolar işareti), bir [enterpolasyonlu dize](/dotnet/csharp/language-reference/keywords/interpolated-strings)kullanır:</span><span class="sxs-lookup"><span data-stu-id="d750c-207">The `$` (dollar sign) in the following line uses an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. <span data-ttu-id="d750c-208">Aşağıdaki işaretlemeyi *About. cshtml* görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d750c-208">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="d750c-209">Vurgulanan biçimlendirme web site bilgilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d750c-209">The highlighted markup displays the web site information.</span></span>

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,4-8, 18-999)]

   > [!NOTE]
   > <span data-ttu-id="d750c-210">Aşağıda gösterilen Razor biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="d750c-210">In the Razor markup shown below:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=18-18)]
   >
   > <span data-ttu-id="d750c-211">Razor `info` özniteliğin bir dize değil bir sınıf olduğunu ve kod yazmak C# istediğinizi bilir.</span><span class="sxs-lookup"><span data-stu-id="d750c-211">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="d750c-212">Dize olmayan herhangi bir etiket Yardımcısı özniteliği `@` karakteri olmadan yazılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d750c-212">Any non-string tag helper attribute should be written without the `@` character.</span></span>

1. <span data-ttu-id="d750c-213">Uygulamayı çalıştırın ve web sitesi bilgileri görmek için hakkında görünümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="d750c-213">Run the app, and navigate to the About view to see the web site information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d750c-214">Aşağıdaki biçimlendirmeyi bir kapanış etiketiyle kullanabilir ve etiket yardımından `TagMode.StartTagAndEndTag` satırı kaldırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d750c-214">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=20-21)]

## <a name="condition-tag-helper"></a><span data-ttu-id="d750c-215">Koşul etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="d750c-215">Condition Tag Helper</span></span>

<span data-ttu-id="d750c-216">True değeri geçirildiğinde çıkış koşulu etiket Yardımcısı işler.</span><span class="sxs-lookup"><span data-stu-id="d750c-216">The condition tag helper renders output when passed a true value.</span></span>

1. <span data-ttu-id="d750c-217">Aşağıdaki `ConditionTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d750c-217">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. <span data-ttu-id="d750c-218">*Views/Home/Index. cshtml* dosyasının içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d750c-218">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. <span data-ttu-id="d750c-219">`Home` denetleyicisindeki `Index` yöntemini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d750c-219">Replace the `Index` method in the `Home` controller with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. <span data-ttu-id="d750c-220">Uygulamayı çalıştırın ve giriş sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="d750c-220">Run the app and browse to the home page.</span></span> <span data-ttu-id="d750c-221">Koşullu `div` işaretleme işlenmez.</span><span class="sxs-lookup"><span data-stu-id="d750c-221">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="d750c-222">URL 'ye `?approved=true` sorgu dizesi ekleyin (örneğin, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="d750c-222">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="d750c-223">`approved` true olarak ayarlanır ve koşullu biçimlendirme görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d750c-223">`approved` is set to true and the conditional markup will be displayed.</span></span>

> [!NOTE]
> <span data-ttu-id="d750c-224">Kalın etiket Yardımcısı ile yaptığınız gibi bir dize belirtmek yerine, hedeflenecek özniteliği belirtmek için [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecini kullanın:</span><span class="sxs-lookup"><span data-stu-id="d750c-224">Use the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> <span data-ttu-id="d750c-225">[NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kodu korumasını yeniden düzenlenmiş olması gerekir (adı `RedCondition`olarak değiştirmek isteyebilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="d750c-225">The [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="d750c-226">Etiket Yardımcısı çakışmaları</span><span class="sxs-lookup"><span data-stu-id="d750c-226">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="d750c-227">Bu bölümde, otomatik bağlama etiket Yardımcıları çifti yazın.</span><span class="sxs-lookup"><span data-stu-id="d750c-227">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="d750c-228">İlk biçimlendirme içeren bir HTML yer işareti etiketi aynı URL'yi içeren (ve bu nedenle bağlantı URL'si sonuçlanmıyor) HTTP ile başlayan bir URL yerini alır.</span><span class="sxs-lookup"><span data-stu-id="d750c-228">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="d750c-229">İkinci aynı WWW ile başlayan bir URL yapar.</span><span class="sxs-lookup"><span data-stu-id="d750c-229">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="d750c-230">Bu iki Yardımcıları yakından ilgili ve gelecekte yeniden çünkü bunların aynı dosyada saklayacağız.</span><span class="sxs-lookup"><span data-stu-id="d750c-230">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1. <span data-ttu-id="d750c-231">Aşağıdaki `AutoLinkerHttpTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d750c-231">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   ><span data-ttu-id="d750c-232">`AutoLinkerHttpTagHelper` sınıfı, `p` öğeleri hedefler ve bağlayıcıyı oluşturmak için [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) kullanır.</span><span class="sxs-lookup"><span data-stu-id="d750c-232">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

1. <span data-ttu-id="d750c-233">*Görünümler/Home/Contact. cshtml* dosyasının sonuna aşağıdaki biçimlendirmeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d750c-233">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. <span data-ttu-id="d750c-234">Uygulamayı çalıştırın ve etiket Yardımcısı bağlantı doğru şekilde işlediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="d750c-234">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

1. <span data-ttu-id="d750c-235">Www metnini, özgün www metnini de içeren bir tutturucu etiketine dönüştürecek `AutoLinkerWwwTagHelper` eklemek için `AutoLinker` sınıfını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d750c-235">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="d750c-236">Güncelleştirilmiş kod aşağıda vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="d750c-236">The updated code is highlighted below:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. <span data-ttu-id="d750c-237">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d750c-237">Run the app.</span></span> <span data-ttu-id="d750c-238">Www metin bağlantı olarak işlenir ancak HTTP metin değil dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d750c-238">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="d750c-239">Her iki sınıflarda kesme noktası koymak, HTTP etiket Yardımcısı sınıfı ilk çalıştığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-239">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="d750c-240">Etiket Yardımcısı çıkışı önbelleğe alınır ve WWW etiketi Yardımcısı çalıştırdığınızda, önbelleğe alınan HTTP etiketi Yardımcısı çıktısı üzerine yazar sorunudur.</span><span class="sxs-lookup"><span data-stu-id="d750c-240">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="d750c-241">Etiket Yardımcıları Çalıştır sırasını denetlemek nasıl öğreticinin ilerleyen bölümlerinde göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="d750c-241">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="d750c-242">Kodu aşağıdakiler ile gidereceğiz:</span><span class="sxs-lookup"><span data-stu-id="d750c-242">We'll fix the code with the following:</span></span>

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > <span data-ttu-id="d750c-243">Otomatik bağlama etiket Yardımcıları, ilk sürümünde, aşağıdaki kod ile hedef içeriğini alındı:</span><span class="sxs-lookup"><span data-stu-id="d750c-243">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > <span data-ttu-id="d750c-244">Yani, `ProcessAsync` yöntemine geçirilen `TagHelperOutput` kullanarak `GetChildContentAsync` çağıralırsınız.</span><span class="sxs-lookup"><span data-stu-id="d750c-244">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="d750c-245">Belirtildiği gibi daha önce çıktıyı önbelleğe alındığından, son etiket WINS çalışmasına yardımcı.</span><span class="sxs-lookup"><span data-stu-id="d750c-245">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="d750c-246">Aşağıdaki kod ile ilgili sorun düzeltildi:</span><span class="sxs-lookup"><span data-stu-id="d750c-246">You fixed that problem with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > <span data-ttu-id="d750c-247">Yukarıdaki kod içeriği değiştirildi ve varsa, çıkış arabellek içeriği alır olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="d750c-247">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

1. <span data-ttu-id="d750c-248">Uygulamayı çalıştırın ve iki bağlantı beklendiği gibi çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d750c-248">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="d750c-249">Bizim otomatik bağlayıcı etiket Yardımcısı doğru ve eksiksiz görünebilir, ancak ince bir sorun var.</span><span class="sxs-lookup"><span data-stu-id="d750c-249">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="d750c-250">WWW etiketi Yardımcısı ilk çalıştırıyorsa, www bağlantıları doğru olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d750c-250">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="d750c-251">Etiketin içinde çalıştığı sırayı denetlemek için `Order` aşırı yüklemesini ekleyerek kodu güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d750c-251">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="d750c-252">`Order` özelliği, aynı öğeyi hedefleyen diğer etiket yardımcıları ile ilişkili yürütme sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="d750c-252">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="d750c-253">Varsayılan sıra değeri sıfırdır ve düşük değerler örnekleriyle önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="d750c-253">The default order value is zero and instances with lower values are executed first.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   <span data-ttu-id="d750c-254">Yukarıdaki kod, HTTP etiketi Yardımcısı önce WWW etiketi Yardımcısı çalıştığını garanti eder.</span><span class="sxs-lookup"><span data-stu-id="d750c-254">The preceding code guarantees that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="d750c-255">`Order` `MaxValue` olarak değiştirin ve WWW etiketi için oluşturulan biçimlendirmenin yanlış olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d750c-255">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="d750c-256">Alt içeriği almak ve İnceleme</span><span class="sxs-lookup"><span data-stu-id="d750c-256">Inspect and retrieve child content</span></span>

<span data-ttu-id="d750c-257">Etiket Yardımcıları içerik almak için çeşitli özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="d750c-257">The tag helpers provide several properties to retrieve content.</span></span>

* <span data-ttu-id="d750c-258">`GetChildContentAsync` sonucu `output.Content`eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d750c-258">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
* <span data-ttu-id="d750c-259">`GetChildContentAsync` sonucunu `GetContent`ile inceleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d750c-259">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
* <span data-ttu-id="d750c-260">`output.Content`değiştirirseniz, otomatik bağlayıcı örneğimizde olduğu gibi `GetChildContentAsync` çağırmadığınız sürece TagHelper gövdesi yürütülmez veya işlenmez:</span><span class="sxs-lookup"><span data-stu-id="d750c-260">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* <span data-ttu-id="d750c-261">Birden çok `GetChildContentAsync` çağrısı aynı değeri döndürür ve önbelleğe alınmış sonucu kullanmamasını belirten yanlış bir parametre geçirmediğiniz takdirde `TagHelper` gövdesini yeniden çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="d750c-261">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>

## <a name="load-minified-partial-view-taghelper"></a><span data-ttu-id="d750c-262">Mini yükleme küçük bir kısmı görüntüleme TagHelper</span><span class="sxs-lookup"><span data-stu-id="d750c-262">Load minified partial view TagHelper</span></span>

<span data-ttu-id="d750c-263">Üretim ortamlarında, Mini karşılaşılan kısmi görünümler yüklenirken performans artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d750c-263">In production environments, performance can be improved by loading minified partial views.</span></span> <span data-ttu-id="d750c-264">Üretimde küçültülmüş kısmi görünümden faydalanmak için:</span><span class="sxs-lookup"><span data-stu-id="d750c-264">To take advantage of minified partial view in production:</span></span>

* <span data-ttu-id="d750c-265">Kısmi görünümleri mini görüntülemesi için derleme öncesi bir işlem oluşturun/ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d750c-265">Create/set up a pre-build process that minifies partial views.</span></span>
* <span data-ttu-id="d750c-266">Geliştirme dışı ortamlarda mini kullanılan kısmi görünümleri yüklemek için aşağıdaki kodu kullanın.</span><span class="sxs-lookup"><span data-stu-id="d750c-266">Use the following code to load  minified partial views in non-development environments.</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/MinifiedVersionTagHelper.cs?name=snippet)]
