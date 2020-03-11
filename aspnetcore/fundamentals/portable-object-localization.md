---
title: ASP.NET Core taşınabilir nesne yerelleştirmesini yapılandırma
author: sebastienros
description: Bu makale, taşınabilir nesne dosyalarını tanıtır ve bunları Orchard Core çerçevesiyle ASP.NET Core bir uygulamada kullanmaya yönelik adımları özetler.
ms.author: scaddie
ms.date: 09/26/2017
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 08002564eb68bc04eebaeafed560202d0d69958a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656193"
---
# <a name="configure-portable-object-localization-in-aspnet-core"></a><span data-ttu-id="8c5b0-103">ASP.NET Core taşınabilir nesne yerelleştirmesini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8c5b0-103">Configure portable object localization in ASP.NET Core</span></span>

<span data-ttu-id="8c5b0-104">[Sébastien Ros](https://github.com/sebastienros) ve [Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="8c5b0-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8c5b0-105">Bu makalede, [Orchard Core](https://github.com/OrchardCMS/OrchardCore) Framework ile bir ASP.NET Core uygulamasında taşınabilir nesne (PO) dosyalarını kullanma adımları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="8c5b0-106">**Note:** Orchard Core bir Microsoft ürünü değildir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="8c5b0-107">Sonuç olarak, Microsoft bu özellik için destek sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="8c5b0-108">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c5b0-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="8c5b0-109">PO dosyası nedir?</span><span class="sxs-lookup"><span data-stu-id="8c5b0-109">What is a PO file?</span></span>

<span data-ttu-id="8c5b0-110">PO dosyaları, belirli bir dil için çevrilmiş dizeleri içeren metin dosyaları olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="8c5b0-111">*. Resx* dosyaları yerine PO dosyalarını kullanmanın bazı avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="8c5b0-112">PO dosyaları, pluralization 'ı destekler; *. resx* dosyaları pluralization 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="8c5b0-113">PO dosyaları *. resx* dosyaları gibi derlenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="8c5b0-114">Bu nedenle, özelleştirilmiş araç ve derleme adımları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="8c5b0-115">PO dosyaları işbirliğine dayalı çevrimiçi Düzenle araçlarıyla iyi çalışır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="8c5b0-116">Örnek</span><span class="sxs-lookup"><span data-stu-id="8c5b0-116">Example</span></span>

<span data-ttu-id="8c5b0-117">Aşağıda, çoğul biçimi ile birlikte olmak üzere Fransızca 'daki iki dizenin çevirisini içeren örnek bir PO dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="8c5b0-118">*fr. Po*</span><span class="sxs-lookup"><span data-stu-id="8c5b0-118">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="8c5b0-119">Bu örnek aşağıdaki sözdizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="8c5b0-120">`#:`: çevrilecek dizenin bağlamını gösteren bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="8c5b0-121">Aynı dize, kullanıldığı yere bağlı olarak farklı şekilde çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="8c5b0-122">`msgid`: çevrilmemiş dize.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="8c5b0-123">`msgstr`: çevrilmiş dize.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="8c5b0-124">Plurselme desteği söz konusu olduğunda, daha fazla girdi tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="8c5b0-125">`msgid_plural`: çevrilmemiş çoğul dize.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="8c5b0-126">`msgstr[0]`: 0 durumu için çevrilmiş dize.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="8c5b0-127">`msgstr[N]`: N. durum için çevrilmiş dize.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="8c5b0-128">PO dosya belirtimi [burada](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html)bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="8c5b0-129">ASP.NET Core 'de PO dosya desteğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8c5b0-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="8c5b0-130">Bu örnek, Visual Studio 2017 proje şablonundan oluşturulan ASP.NET Core MVC uygulamasını temel alır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="8c5b0-131">Pakete başvurma</span><span class="sxs-lookup"><span data-stu-id="8c5b0-131">Referencing the package</span></span>

<span data-ttu-id="8c5b0-132">`OrchardCore.Localization.Core` NuGet paketine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="8c5b0-133">Şu paket kaynağında [Myget](https://www.myget.org/) üzerinde kullanılabilir: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="8c5b0-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="8c5b0-134">*. Csproj* dosyası artık aşağıdakine benzer bir satır içerir (sürüm numarası farklılık gösterebilir):</span><span class="sxs-lookup"><span data-stu-id="8c5b0-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="8c5b0-135">Hizmet kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="8c5b0-135">Registering the service</span></span>

<span data-ttu-id="8c5b0-136">Gerekli Hizmetleri *Startup.cs*`ConfigureServices` metoduna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="8c5b0-137">Gerekli ara yazılımı *Startup.cs*`Configure` yöntemine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="8c5b0-138">Aşağıdaki kodu tercih ettiğiniz Razor görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="8c5b0-139">Bu örnekte *. cshtml hakkında* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="8c5b0-140">Bir `IViewLocalizer` örneği eklenen ve "Hello World!" metnini dönüştürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="8c5b0-141">PO dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="8c5b0-141">Creating a PO file</span></span>

<span data-ttu-id="8c5b0-142">Uygulama kök klasörünüzde *\<kültür kodu >. Po* adlı bir dosya oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-142">Create a file named *\<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="8c5b0-143">Bu örnekte, Fransızca dili kullanıldığından dosya adı *fr. Po* olur:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="8c5b0-144">Bu dosya, hem çevrilecek dizeyi hem de Fransızca çevrilmiş dizeyi depolar.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="8c5b0-145">Çevirileri, gerekirse üst kültürüne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="8c5b0-146">Bu örnekte, istenen kültür `fr-FR` veya `fr-CA`ise *fr. Po* dosyası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="8c5b0-147">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="8c5b0-147">Testing the application</span></span>

<span data-ttu-id="8c5b0-148">Uygulamanızı çalıştırın ve `/Home/About`URL 'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="8c5b0-149">**Merhaba Dünya metni!**</span><span class="sxs-lookup"><span data-stu-id="8c5b0-149">The text **Hello world!**</span></span> <span data-ttu-id="8c5b0-150">görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-150">is displayed.</span></span>

<span data-ttu-id="8c5b0-151">`/Home/About?culture=fr-FR`URL 'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="8c5b0-152">**Bonjour Le Monde metni!**</span><span class="sxs-lookup"><span data-stu-id="8c5b0-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="8c5b0-153">görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="8c5b0-154">Çoğullaştırma</span><span class="sxs-lookup"><span data-stu-id="8c5b0-154">Pluralization</span></span>

<span data-ttu-id="8c5b0-155">Po dosyaları, aynı dizenin kardinalite göre farklı şekilde çevrilmesi gerektiğinde yararlı olan çoğullaştırma formlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="8c5b0-156">Bu görev, her dilin kardinalite göre hangi dizenin kullanılacağını seçmek için özel kurallar tanımladığı konusunda karmaşık hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="8c5b0-157">Orchard yerelleştirme paketi, bu farklı çoğul formları otomatik olarak çağırmak için bir API sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="8c5b0-158">Çoğullaştırma Po dosyaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="8c5b0-158">Creating pluralization PO files</span></span>

<span data-ttu-id="8c5b0-159">Aşağıdaki içeriği daha önce bahsedilen *fr. Po* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="8c5b0-160">Bu örnekteki her girdinin ne olduğunu gösteren bir açıklama için bkz. [po dosyası nedir?](#what-is-a-po-file) .</span><span class="sxs-lookup"><span data-stu-id="8c5b0-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="8c5b0-161">Farklı çoğullaştırma formları kullanarak dil ekleme</span><span class="sxs-lookup"><span data-stu-id="8c5b0-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="8c5b0-162">Önceki örnekte İngilizce ve Fransızca dizeleri kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="8c5b0-163">İngilizce ve Fransızca yalnızca iki plurtasyon biçimine sahiptir ve aynı form kurallarını paylaşır, bu da bir kardinalitesi ilk plural formuyla eşlenir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="8c5b0-164">Diğer herhangi bir kardinalite ikinci çoğul formla eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="8c5b0-165">Dillerin hepsi aynı kuralları paylaşmaz.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-165">Not all languages share the same rules.</span></span> <span data-ttu-id="8c5b0-166">Bu, üç plural formu bulunan Çekçe dil ile gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="8c5b0-167">`cs.po` dosyasını aşağıdaki gibi oluşturun ve plurun üç farklı çeviriyi nasıl ihtiyacı olduğunu aklınızda yapın:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="8c5b0-168">Çekçe yerelleştirmeleri kabul etmek için, `ConfigureServices` yönteminde desteklenen kültürlerin listesine `"cs"` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="8c5b0-169">Farklı kart aralıkları için yerelleştirilmiş, çoğul dizeleri işlemek üzere *Görünümler/Home/about. cshtml* dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="8c5b0-170">**Note:** Gerçek bir Dünya senaryosunda, sayıyı temsil etmek için bir değişken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="8c5b0-171">Burada, çok özel bir durumu ortaya çıkarmak için aynı kodu üç farklı değerle tekrarlarız.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="8c5b0-172">Kültürleri değiştirme sırasında, aşağıdakileri görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="8c5b0-173">`/Home/About` için:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="8c5b0-174">`/Home/About?culture=fr` için:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="8c5b0-175">`/Home/About?culture=cs` için:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="8c5b0-176">Çekçe kültür için, üç çevirilerin farklı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="8c5b0-177">Fransızca ve Ingilizce kültürler, son çevrilen iki dize için aynı yapıyı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="8c5b0-178">Gelişmiş görevler</span><span class="sxs-lookup"><span data-stu-id="8c5b0-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="8c5b0-179">Contextualleme dizeleri</span><span class="sxs-lookup"><span data-stu-id="8c5b0-179">Contextualizing strings</span></span>

<span data-ttu-id="8c5b0-180">Uygulamalar genellikle birkaç yerde çevrilecek dizeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="8c5b0-181">Aynı dize bir uygulama içindeki belirli konumlarda farklı bir çeviriye sahip olabilir (Razor görünümleri veya sınıf dosyaları).</span><span class="sxs-lookup"><span data-stu-id="8c5b0-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="8c5b0-182">Bir PO dosyası, temsil edilen dizeyi kategorilere ayırmak için kullanılabilecek bir dosya bağlamı kavramını destekler.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="8c5b0-183">Dosya bağlamını kullanarak, dosya bağlamına (veya bir dosya bağlamının olmamasından) bağlı olarak bir dize farklı şekilde çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="8c5b0-184">PO yerelleştirme hizmetleri, tam sınıfın veya bir dize çevrilirken kullanılan görünümün adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="8c5b0-185">Bu, `msgctxt` girişi üzerinde değer ayarlanarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="8c5b0-186">Önceki *fr. Po* örneğine küçük bir ek göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="8c5b0-187">*Görünümler/Home/about. cshtml* konumunda bulunan bir Razor görünümü, ayrılmış `msgctxt` girişinin değerini ayarlayarak dosya bağlamı olarak tanımlanabilir:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="8c5b0-188">`msgctxt` ayarlandığı için, `/Home/About?culture=fr-FR`' a gidildiğinde metin çevirisi oluşur.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="8c5b0-189">Çeviri `/Home/Contact?culture=fr-FR`gidildiğinde gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="8c5b0-190">Belirli bir giriş belirli bir dosya bağlamıyla eşleşmediğinde, Orchard Core 'un geri dönüş mekanizması bağlam olmadan uygun bir PO dosyası arar.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="8c5b0-191">*Görünümler/Home/Contact. cshtml*için tanımlı özel dosya bağlamı olmadığı varsayılarak, `/Home/Contact?culture=fr-FR` gezinmek, şöyle bir PO dosyası yükler:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="8c5b0-192">PO dosyalarının konumunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="8c5b0-192">Changing the location of PO files</span></span>

<span data-ttu-id="8c5b0-193">PO dosyalarının varsayılan konumu `ConfigureServices`' de değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="8c5b0-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="8c5b0-194">Bu örnekte, PO dosyaları *Yerelleştirme* klasöründen yüklenir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="8c5b0-195">Yerelleştirme dosyalarını bulmak için özel bir mantık uygulama</span><span class="sxs-lookup"><span data-stu-id="8c5b0-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="8c5b0-196">PO dosyalarını bulmak için daha karmaşık mantık gerektiğinde, `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` arabirimi bir hizmet olarak uygulanabilir ve kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="8c5b0-197">Bu, PO dosyaları farklı konumlarda depolanabileceği veya dosyaların bir klasör hiyerarşisi içinde bulunması gerektiğinde kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="8c5b0-198">Farklı bir varsayılan plurar dili kullanma</span><span class="sxs-lookup"><span data-stu-id="8c5b0-198">Using a different default pluralized language</span></span>

<span data-ttu-id="8c5b0-199">Paket, iki plural formlarına özgü bir `Plural` uzantısı yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="8c5b0-200">Daha fazla çoğul biçim gerektiren diller için bir genişletme yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="8c5b0-201">Uzantı yöntemiyle, varsayılan dil için herhangi bir yerelleştirme dosyası sağlamanız gerekmez &mdash; özgün dizeler doğrudan kodda zaten kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="8c5b0-202">Bir dizi çeviriyi kabul eden daha genel `Plural(int count, string[] pluralForms, params object[] arguments)` aşırı yüklemeyi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c5b0-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
