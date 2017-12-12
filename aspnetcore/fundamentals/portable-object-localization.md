---
title: "Taşınabilir nesne yerelleştirme yapılandırın"
author: sebastienros
description: "Bu makalede, taşınabilir nesne dosyaları tanıtır ve ASP.NET Core uygulamayla Orchard çekirdek çerçevenin kullanılarak adımlarını özetler."
keywords: "ASP.NET Core, yerelleştirme, kültür, dil, taşınabilir nesnesi"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="4096e-104">Taşınabilir nesne yerelleştirme Orchard çekirdek ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4096e-104">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="4096e-105">Tarafından [Sébastien Ros](https://github.com/sebastienros) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="4096e-105">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="4096e-106">Bu makalede taşınabilir nesne (SAS) dosyalarında ASP.NET Core uygulamayla kullanmak için adım adım anlatılmaktadır [Orchard çekirdek](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="4096e-106">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="4096e-107">**Not:** Orchard çekirdek bir Microsoft ürünü değil.</span><span class="sxs-lookup"><span data-stu-id="4096e-107">**Note:** Orchard Core is not a Microsoft product.</span></span> <span data-ttu-id="4096e-108">Sonuç olarak, Microsoft bu özellik için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="4096e-108">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="4096e-109">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4096e-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="4096e-110">Bir SAS dosyası nedir?</span><span class="sxs-lookup"><span data-stu-id="4096e-110">What is a PO file?</span></span>

<span data-ttu-id="4096e-111">SAS dosyaları, belirli bir dile çevrilen dizelerin bulunduğu metin dosyaları olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="4096e-111">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="4096e-112">SAS dosyalar yerine kullanmanın bazı avantajları *.resx* dosyaları içerir:</span><span class="sxs-lookup"><span data-stu-id="4096e-112">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="4096e-113">SAS dosyaları çoğullaştırma destekler; *.resx* dosyaları çoğullaştırma desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4096e-113">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="4096e-114">Olmayan SAS dosyaları derlenmiş gibi *.resx* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="4096e-114">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="4096e-115">Bu nedenle, özel araçları ve yapılandırma adımları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4096e-115">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="4096e-116">SAS dosyaları iyi işbirliği çevrimiçi düzenleme araçları ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="4096e-116">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="4096e-117">Örnek</span><span class="sxs-lookup"><span data-stu-id="4096e-117">Example</span></span>

<span data-ttu-id="4096e-118">Örnek, Fransızca, kendi çoğul biriyle dahil olmak üzere iki dizeyi çevirisi içeren bir SAS dosyası şöyledir:</span><span class="sxs-lookup"><span data-stu-id="4096e-118">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="4096e-119">*fr.PO*</span><span class="sxs-lookup"><span data-stu-id="4096e-119">*fr.po*</span></span>

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

<span data-ttu-id="4096e-120">Bu örnekte, aşağıdaki söz dizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="4096e-120">This example uses the following syntax:</span></span>

- <span data-ttu-id="4096e-121">`#:`: Çevrilecek dize bağlamında belirten bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="4096e-121">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="4096e-122">Burada kullanılan bağlı olarak farklı şekilde bu aynı dize çevrilmesi.</span><span class="sxs-lookup"><span data-stu-id="4096e-122">The same string might be translated differently depending on where it is being used.</span></span>
- <span data-ttu-id="4096e-123">`msgid`: Untranslated dize.</span><span class="sxs-lookup"><span data-stu-id="4096e-123">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="4096e-124">`msgstr`: Çevrilen dize.</span><span class="sxs-lookup"><span data-stu-id="4096e-124">`msgstr`: The translated string.</span></span>

<span data-ttu-id="4096e-125">Çoğullaştırma desteği söz konusu olduğunda, daha fazla girdi tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4096e-125">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="4096e-126">`msgid_plural`: Untranslated çoğul dize.</span><span class="sxs-lookup"><span data-stu-id="4096e-126">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="4096e-127">`msgstr[0]`: 0 çalışması için çevrilmiş dize.</span><span class="sxs-lookup"><span data-stu-id="4096e-127">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="4096e-128">`msgstr[N]`: Büyük küçük harf n çevrilmiş dizesi</span><span class="sxs-lookup"><span data-stu-id="4096e-128">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="4096e-129">SAS dosya belirtimi bulunabilir [burada](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="4096e-129">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="4096e-130">ASP.NET Core SAS dosya desteğini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4096e-130">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="4096e-131">Bu örnek bir Visual Studio 2017 proje şablondan oluşturulmuş bir ASP.NET Core MVC uygulaması temel alır.</span><span class="sxs-lookup"><span data-stu-id="4096e-131">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="4096e-132">Paket başvurma</span><span class="sxs-lookup"><span data-stu-id="4096e-132">Referencing the package</span></span>

<span data-ttu-id="4096e-133">Bir başvuru ekleyin `OrchardCore.Localization.Core` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="4096e-133">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="4096e-134">Üzerinde kullanılabilir [MyGet](https://www.myget.org/) aşağıdaki paket kaynağında: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="4096e-134">It is available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="4096e-135">*.Csproj* dosyası artık aşağıdakine benzer bir satır içerir (sürüm numarası değişebilir):</span><span class="sxs-lookup"><span data-stu-id="4096e-135">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="4096e-136">Hizmeti kaydetme</span><span class="sxs-lookup"><span data-stu-id="4096e-136">Registering the service</span></span>

<span data-ttu-id="4096e-137">Gerekli hizmetler eklemek `ConfigureServices` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="4096e-137">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="4096e-138">Gerekli Ara eklemek `Configure` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="4096e-138">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="4096e-139">Aşağıdaki kod, seçim Razor görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4096e-139">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="4096e-140">*About.cshtml* Bu örnekte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4096e-140">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="4096e-141">Bir `IViewLocalizer` örneği eklenen ve "Hello world!" metin çevirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4096e-141">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="4096e-142">Bir SAS dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="4096e-142">Creating a PO file</span></span>

<span data-ttu-id="4096e-143">Adlı bir dosya oluşturun  *<culture code>.po* uygulama kök klasörünüzde.</span><span class="sxs-lookup"><span data-stu-id="4096e-143">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="4096e-144">Bu örnekte, dosya adıdır *fr.po* Fransızca Dil kullanıldığından:</span><span class="sxs-lookup"><span data-stu-id="4096e-144">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="4096e-145">Bu dosyayı çevirmek için dize ve Fransızca çevrilen dizesi depolar.</span><span class="sxs-lookup"><span data-stu-id="4096e-145">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="4096e-146">Çeviriler, gerekirse üst kültüre geri dönün.</span><span class="sxs-lookup"><span data-stu-id="4096e-146">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="4096e-147">Bu örnekte, *fr.po* istenen kültürü ise dosya kullanılan `fr-FR` veya `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="4096e-147">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="4096e-148">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="4096e-148">Testing the application</span></span>

<span data-ttu-id="4096e-149">Uygulamanızı çalıştırın ve URL'ye `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="4096e-149">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="4096e-150">Metin **Merhaba Dünya!**</span><span class="sxs-lookup"><span data-stu-id="4096e-150">The text **Hello world!**</span></span> <span data-ttu-id="4096e-151">görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4096e-151">is displayed.</span></span>

<span data-ttu-id="4096e-152">URL'ye `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="4096e-152">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="4096e-153">Metin **Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="4096e-153">The text **Bonjour le monde!**</span></span> <span data-ttu-id="4096e-154">görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4096e-154">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="4096e-155">Çoğullaştırma</span><span class="sxs-lookup"><span data-stu-id="4096e-155">Pluralization</span></span>

<span data-ttu-id="4096e-156">SAS dosyaları aynı dize çevrilmesi farklı bir kardinalite üzerinde temel gerektiğinde faydalı olan çoğullaştırma forms destekler.</span><span class="sxs-lookup"><span data-stu-id="4096e-156">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="4096e-157">Bu görevi her dil üzerinde kardinalite kullanmak için hangi dize tabanlıdır seçmek için özel kurallar tanımlar olarak karmaşık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="4096e-157">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="4096e-158">Orchard yerelleştirme paketi farklı bu çoğul formlar otomatik olarak çağırmak için bir API sağlar.</span><span class="sxs-lookup"><span data-stu-id="4096e-158">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="4096e-159">Çoğullaştırma SAS dosyaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="4096e-159">Creating pluralization PO files</span></span>

<span data-ttu-id="4096e-160">Aşağıdaki içerik yukarıda açıklanan eklemek *fr.po* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4096e-160">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="4096e-161">Bkz: [SAS dosyası nedir?](#what-is-a-po-file) ne Bu örnekte her girişini temsil eden bir açıklama için.</span><span class="sxs-lookup"><span data-stu-id="4096e-161">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="4096e-162">Farklı çoğullaştırma formları kullanarak bir dil ekleme</span><span class="sxs-lookup"><span data-stu-id="4096e-162">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="4096e-163">İngilizce ve Fransızca dizeleri önceki örnekte kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="4096e-163">English and French strings were used in the previous example.</span></span> <span data-ttu-id="4096e-164">İngilizce ve Fransızca yalnızca iki çoğullaştırma forms sahip ve aynı form kuralları paylaşmak hangi kardinalitesi birinin ilk çoğul eşlendi.</span><span class="sxs-lookup"><span data-stu-id="4096e-164">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="4096e-165">Diğer bir önem düzeyi ikinci çoğul eşlenir.</span><span class="sxs-lookup"><span data-stu-id="4096e-165">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="4096e-166">Tüm diller aynı kurallar paylaşır.</span><span class="sxs-lookup"><span data-stu-id="4096e-166">Not all languages share the same rules.</span></span> <span data-ttu-id="4096e-167">Bu üç çoğul forms sahip Çekçe Dil ile gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4096e-167">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="4096e-168">Oluşturma `cs.po` gibi dosya ve nasıl çoğullaştırma üç farklı çevirileri gerektiğine dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="4096e-168">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="4096e-169">Çekçe yerelleştirmeler kabul etmek için add `"cs"` içinde desteklenen kültürler listesine `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4096e-169">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

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

<span data-ttu-id="4096e-170">Düzen *Views/Home/About.cshtml* birkaç cardinalities için yerelleştirilmiş, çoğul dizelerini oluşturmak için dosya:</span><span class="sxs-lookup"><span data-stu-id="4096e-170">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="4096e-171">**Not:** gerçek dünya senaryoda, bir değişken sayısı temsil etmek için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="4096e-171">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="4096e-172">Burada, belirli bir durumu göstermek için üç farklı değerlerle aynı kodu yineleyin.</span><span class="sxs-lookup"><span data-stu-id="4096e-172">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="4096e-173">Kültür geçiş sırasında aşağıdakilere bakın:</span><span class="sxs-lookup"><span data-stu-id="4096e-173">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="4096e-174">İçin `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="4096e-174">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="4096e-175">İçin `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="4096e-175">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="4096e-176">İçin `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="4096e-176">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="4096e-177">Çekçe kültürü için üç çevirileri farklı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4096e-177">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="4096e-178">İngilizce ve Fransızca kültürler aynı yapımı için son iki çevrilen dizeleri paylaşır.</span><span class="sxs-lookup"><span data-stu-id="4096e-178">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="4096e-179">Gelişmiş görevler</span><span class="sxs-lookup"><span data-stu-id="4096e-179">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="4096e-180">Contextualizing dizeleri</span><span class="sxs-lookup"><span data-stu-id="4096e-180">Contextualizing strings</span></span>

<span data-ttu-id="4096e-181">Uygulamalar genellikle çeşitli yerlerde çevrilecek dizeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="4096e-181">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="4096e-182">Aynı dize farklı bir çeviri (Razor görünümleri veya sınıf dosyaları) bir uygulama içinde belirli bir konumda olabilir.</span><span class="sxs-lookup"><span data-stu-id="4096e-182">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="4096e-183">Bir SAS dosya temsil ettiği dize sınıflandırmak için kullanılan bir dosya bağlam kavramını destekler.</span><span class="sxs-lookup"><span data-stu-id="4096e-183">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="4096e-184">Bir dosya bağlam kullanılarak, bir dize farklı şekilde, dosya içeriği (veya bir dosya bağlam eksikliği) bağlı olarak çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="4096e-184">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="4096e-185">SAS yerelleştirme Hizmetleri tam sınıfı veya bir dize çevirirken kullanılan görünüm adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="4096e-185">The PO localization services use the name of the full class or the view that is used when translating a string.</span></span> <span data-ttu-id="4096e-186">Bu değer ayarı gerçekleştirilir `msgctxt` girişi.</span><span class="sxs-lookup"><span data-stu-id="4096e-186">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="4096e-187">Önceki bir ikincil eklemeyi göz önünde bulundurun *fr.po* örnek.</span><span class="sxs-lookup"><span data-stu-id="4096e-187">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="4096e-188">Razor görünüm konumundaki *Views/Home/About.cshtml* ayrılmış ayarlayarak dosya bağlamı olarak tanımlanabilir `msgctxt` girişin değeri:</span><span class="sxs-lookup"><span data-stu-id="4096e-188">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="4096e-189">İle `msgctxt` şekilde ayarlanırsa, metin için gezinirken çevirmesinin `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="4096e-189">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="4096e-190">Çeviri için gezinirken karşılaşılmaz `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="4096e-190">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="4096e-191">Belirli bir giriş verilen dosya bağlamla eşleştiğinde Orchard çekirdek'ın geri dönüş mekanizması bir bağlam olmadan uygun bir SAS dosyasını arar.</span><span class="sxs-lookup"><span data-stu-id="4096e-191">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="4096e-192">Olmadığı varsayılarak olduğu için tanımlanan belirli bir dosya bağlam *Views/Home/Contact.cshtml*, gezinme için `/Home/Contact?culture=fr-FR` bir SAS dosya gibi yükler:</span><span class="sxs-lookup"><span data-stu-id="4096e-192">Assuming there is no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="4096e-193">SAS dosyalarının konumunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="4096e-193">Changing the location of PO files</span></span>

<span data-ttu-id="4096e-194">SAS dosyalarının varsayılan konumu değiştirilebilir `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4096e-194">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="4096e-195">Bu örnekte, gelen SAS dosyalar yüklenir *yerelleştirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="4096e-195">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="4096e-196">Yerelleştirme dosyaları bulmak için özel bir mantık uygulama</span><span class="sxs-lookup"><span data-stu-id="4096e-196">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="4096e-197">SAS dosyalarını bulmak için daha karmaşık mantığı gerektiğinde `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` arabirimi uygulanabilir ve bir hizmet olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="4096e-197">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="4096e-198">Bu, SAS dosyaları değişen konumlarda depolanabilir veya ne zaman dosyaları bir klasör hiyerarşisi içinde bulunması yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="4096e-198">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="4096e-199">Pluralized farklı varsayılan dilini kullanma</span><span class="sxs-lookup"><span data-stu-id="4096e-199">Using a different default pluralized language</span></span>

<span data-ttu-id="4096e-200">Paketi içeren bir `Plural` iki çoğul formlara özgü genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4096e-200">The package includes a `Plural` extension method that is specific to two plural forms.</span></span> <span data-ttu-id="4096e-201">Daha fazla çoğul forms gerektiren diller için bir genişletme yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4096e-201">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="4096e-202">Bir genişletme yöntemi ile varsayılan dil için tüm yerelleştirme dosyası girmenize gerek yoktur &mdash; özgün dizeleri zaten doğrudan kod içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4096e-202">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="4096e-203">Daha fazla genel kullanabilirsiniz `Plural(int count, string[] pluralForms, params object[] arguments)` çevirileri bir dize dizisi kabul eden aşırı.</span><span class="sxs-lookup"><span data-stu-id="4096e-203">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>