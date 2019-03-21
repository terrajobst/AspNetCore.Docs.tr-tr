---
title: ASP.NET Core taşınabilir nesne yerelleştirmesi yapılandırma
author: sebastienros
description: Bu makalede, taşınabilir nesne dosyaları tanıtır ve bunları Orchard Core framework ile ASP.NET Core uygulamasını kullanarak adımlarını özetler.
ms.author: scaddie
ms.date: 09/26/2017
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 466759b30e756a7cac8abab7352025df0462bb6f
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210099"
---
# <a name="configure-portable-object-localization-in-aspnet-core"></a><span data-ttu-id="63d0e-103">ASP.NET Core taşınabilir nesne yerelleştirmesi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="63d0e-103">Configure portable object localization in ASP.NET Core</span></span>

<span data-ttu-id="63d0e-104">Tarafından [Sébastien Ros](https://github.com/sebastienros) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="63d0e-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="63d0e-105">Bu makale, bir ASP.NET Core uygulaması ile taşınabilir nesne (SAS) dosyalarında kullanma adımları size [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="63d0e-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="63d0e-106">**Not:** Orchard Core Microsoft Ürün değildir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="63d0e-107">Sonuç olarak, Microsoft bu özellik için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="63d0e-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="63d0e-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63d0e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="63d0e-109">Bir SAS dosyası nedir?</span><span class="sxs-lookup"><span data-stu-id="63d0e-109">What is a PO file?</span></span>

<span data-ttu-id="63d0e-110">PO dosyaları, belirli bir dile çevrilen dizelerin bulunduğu metin dosyaları olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="63d0e-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="63d0e-111">PO dosyaları bunun yerine kullanmanın bazı avantajları *.resx* dosyaları içerir:</span><span class="sxs-lookup"><span data-stu-id="63d0e-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="63d0e-112">PO dosyaları çoğullaştırma destekler; *.resx* dosyaları çoğullaştırma desteklemez.</span><span class="sxs-lookup"><span data-stu-id="63d0e-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="63d0e-113">PO dosyaları gibi derlenmiş olmayan *.resx* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="63d0e-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="63d0e-114">Bu nedenle, özel araçlar ve yapılandırma adımları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="63d0e-115">PO dosyaları da işbirliğine dayalı çevrimiçi düzenleme araçları ile çalışma.</span><span class="sxs-lookup"><span data-stu-id="63d0e-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="63d0e-116">Örnek</span><span class="sxs-lookup"><span data-stu-id="63d0e-116">Example</span></span>

<span data-ttu-id="63d0e-117">Çeviri için Fransızca kendi çoğul biriyle dahil olmak üzere, iki dizeyi içeren örnek bir SAS dosyası aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="63d0e-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="63d0e-118">*fr.po*</span><span class="sxs-lookup"><span data-stu-id="63d0e-118">*fr.po*</span></span>

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

<span data-ttu-id="63d0e-119">Bu örnek, aşağıdaki sözdizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="63d0e-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="63d0e-120">`#:`: Çevrilecek dize bağlamında belirten bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="63d0e-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="63d0e-121">Burada kullanıldığı bağlı olarak farklı bu aynı dize çevrilmesi.</span><span class="sxs-lookup"><span data-stu-id="63d0e-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="63d0e-122">`msgid`: Çevrilmemiş dize.</span><span class="sxs-lookup"><span data-stu-id="63d0e-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="63d0e-123">`msgstr`: Çevrilmiş dize.</span><span class="sxs-lookup"><span data-stu-id="63d0e-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="63d0e-124">Çoğullaştırmayı desteği söz konusu olduğunda, daha fazla giriş tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="63d0e-125">`msgid_plural`: Çevrilmemiş çoğul dize.</span><span class="sxs-lookup"><span data-stu-id="63d0e-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="63d0e-126">`msgstr[0]`: 0 çalışması için çevrilmiş dize.</span><span class="sxs-lookup"><span data-stu-id="63d0e-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="63d0e-127">`msgstr[N]`: Çevrilmiş dize için büyük/küçük harfe n</span><span class="sxs-lookup"><span data-stu-id="63d0e-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="63d0e-128">SAS dosya belirtimi bulunabilir [burada](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="63d0e-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="63d0e-129">ASP.NET core'da yapılandırma PO dosya desteği</span><span class="sxs-lookup"><span data-stu-id="63d0e-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="63d0e-130">Bu örnek, bir Visual Studio 2017 proje şablonundan oluşturulan bir ASP.NET Core MVC uygulaması dayanır.</span><span class="sxs-lookup"><span data-stu-id="63d0e-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="63d0e-131">Bu paketi</span><span class="sxs-lookup"><span data-stu-id="63d0e-131">Referencing the package</span></span>

<span data-ttu-id="63d0e-132">Bir başvuru ekleyin `OrchardCore.Localization.Core` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="63d0e-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="63d0e-133">Üzerinde kullanılabilir [MyGet](https://www.myget.org/) aşağıdaki paket kaynağında: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="63d0e-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="63d0e-134">*.Csproj* dosyayı şimdi aşağıdakine benzer bir satır içeren (sürüm numarası değişebilir):</span><span class="sxs-lookup"><span data-stu-id="63d0e-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="63d0e-135">Hizmeti kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="63d0e-135">Registering the service</span></span>

<span data-ttu-id="63d0e-136">Gerekli hizmetlere ekleme `ConfigureServices` yöntemi *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="63d0e-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="63d0e-137">Eklemek için gerekli olan bir ara yazılım `Configure` yöntemi *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="63d0e-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="63d0e-138">Razor görünümünü seçim için aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63d0e-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="63d0e-139">*About.cshtml* Bu örnekte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63d0e-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="63d0e-140">Bir `IViewLocalizer` örneği eklenen ve "Hello world!" metni çevirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63d0e-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="63d0e-141">Bir SAS dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="63d0e-141">Creating a PO file</span></span>

<span data-ttu-id="63d0e-142">Adlı bir dosya oluşturun  *\<kültür kodu > .po* uygulama kök klasörünüzde.</span><span class="sxs-lookup"><span data-stu-id="63d0e-142">Create a file named *\<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="63d0e-143">Bu örnekte, dosya adı olan *fr.po* Fransızca Dil kullanılır çünkü:</span><span class="sxs-lookup"><span data-stu-id="63d0e-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="63d0e-144">Bu dosya, dize çevirmek için hem Fransızca çevrilmiş dize depolar.</span><span class="sxs-lookup"><span data-stu-id="63d0e-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="63d0e-145">Çevirileri üst kültürünü gerekirse geri dönün.</span><span class="sxs-lookup"><span data-stu-id="63d0e-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="63d0e-146">Bu örnekte, *fr.po* istenen kültürü ise dosya kullanılan `fr-FR` veya `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="63d0e-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="63d0e-147">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="63d0e-147">Testing the application</span></span>

<span data-ttu-id="63d0e-148">Uygulamanızı çalıştırın ve URL'ye `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="63d0e-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="63d0e-149">Metin **Merhaba Dünya!**</span><span class="sxs-lookup"><span data-stu-id="63d0e-149">The text **Hello world!**</span></span> <span data-ttu-id="63d0e-150">görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-150">is displayed.</span></span>

<span data-ttu-id="63d0e-151">URL'ye `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="63d0e-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="63d0e-152">Metin **Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="63d0e-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="63d0e-153">görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="63d0e-154">Çoğullaştırmayı</span><span class="sxs-lookup"><span data-stu-id="63d0e-154">Pluralization</span></span>

<span data-ttu-id="63d0e-155">PO dosyaları, aynı dize dönüştürülür. farklı bir kardinalite üzerinde temel gerektiğinde faydalı olan çoğullaştırma forms destekler.</span><span class="sxs-lookup"><span data-stu-id="63d0e-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="63d0e-156">Bu görevi, her bir dilin kardinalite üzerinde kullanmak için hangi dize tabanlı seçmek için özel kurallar tanımlar olarak karmaşık yapılır.</span><span class="sxs-lookup"><span data-stu-id="63d0e-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="63d0e-157">Orchard yerelleştirme paket çoğul bu farklı biçimlerin otomatik olarak çağırmak için bir API sağlar.</span><span class="sxs-lookup"><span data-stu-id="63d0e-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="63d0e-158">Çoğullaştırmayı PO dosyaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="63d0e-158">Creating pluralization PO files</span></span>

<span data-ttu-id="63d0e-159">Aşağıdaki içeriği daha önce bahsedilen ekleyin *fr.po* dosyası:</span><span class="sxs-lookup"><span data-stu-id="63d0e-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="63d0e-160">Bkz: [PO dosyası nedir?](#what-is-a-po-file) ne Bu örnekte her giriş temsil açıklaması.</span><span class="sxs-lookup"><span data-stu-id="63d0e-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="63d0e-161">Farklı çoğullaştırma formları kullanarak bir dil ekleme</span><span class="sxs-lookup"><span data-stu-id="63d0e-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="63d0e-162">Önceki örnekte kullanılan İngilizce ve Fransızca dizeler.</span><span class="sxs-lookup"><span data-stu-id="63d0e-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="63d0e-163">İngilizce ve Fransızca yalnızca iki çoğullaştırma formları ve aynı form kuralları paylaşın olduğu bir kardinalite birinin ilk çoğul eşlendi.</span><span class="sxs-lookup"><span data-stu-id="63d0e-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="63d0e-164">Diğer bir kardinalite ikinci çoğul eşlenir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="63d0e-165">Tüm diller, aynı kurallara paylaşın.</span><span class="sxs-lookup"><span data-stu-id="63d0e-165">Not all languages share the same rules.</span></span> <span data-ttu-id="63d0e-166">Bu üç çoğul forma sahip Çekçe Dil ile gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="63d0e-167">Oluşturma `cs.po` aşağıdaki gibi ve nasıl çoğullaştırma üç farklı çevirilere gereken dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="63d0e-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="63d0e-168">Çekçe yerelleştirmeler kabul etmek için ekleme `"cs"` içinde desteklenen kültürler listesine `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="63d0e-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

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

<span data-ttu-id="63d0e-169">Düzen *Views/Home/About.cshtml* yerelleştirilmiş, plural dizeler için birden fazla kardinaliteleri işlenecek dosya:</span><span class="sxs-lookup"><span data-stu-id="63d0e-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="63d0e-170">**Not:** Gerçek hayattaki bir senaryoda, bir değişken sayısı temsil etmek için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="63d0e-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="63d0e-171">Burada, aynı kodu belirli bir servis talebi göstermek için üç farklı değerlerle tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="63d0e-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="63d0e-172">Kültürler geçiş sırasında aşağıdakilere bakın:</span><span class="sxs-lookup"><span data-stu-id="63d0e-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="63d0e-173">için `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="63d0e-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="63d0e-174">için `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="63d0e-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="63d0e-175">için `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="63d0e-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="63d0e-176">Çekçe kültürü için üç çevirileri farklı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="63d0e-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="63d0e-177">Fransızca ve İngilizce kültürlerini aynı yapımı için son iki çevrilmiş dizeleri paylaşın.</span><span class="sxs-lookup"><span data-stu-id="63d0e-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="63d0e-178">Gelişmiş görevler</span><span class="sxs-lookup"><span data-stu-id="63d0e-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="63d0e-179">Contextualizing dizeleri</span><span class="sxs-lookup"><span data-stu-id="63d0e-179">Contextualizing strings</span></span>

<span data-ttu-id="63d0e-180">Uygulamalar genellikle birçok yerde çevrilemeyen dizeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="63d0e-181">Aynı dize farklı bir çeviri (Razor görünümleri ya da sınıf dosyaları) bir uygulama içinde belirli bir konumda olabilir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="63d0e-182">Bir SAS dosya temsil ettiği dizeyi sınıflandırmak için kullanılan bir dosya bağlamı kavramını destekler.</span><span class="sxs-lookup"><span data-stu-id="63d0e-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="63d0e-183">Dosya bağlamını kullanarak, bir dize farklı dosya içeriği (veya dosya bağlam eksikliği) bağlı olarak çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="63d0e-184">SAS yerelleştirme Hizmetleri, tam sınıf veya bir dize çevirirken kullanılan görünüm adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="63d0e-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="63d0e-185">Bu değeri ayarı gerçekleştirilir `msgctxt` girişi.</span><span class="sxs-lookup"><span data-stu-id="63d0e-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="63d0e-186">Önceki bir ikincil eklemeyi göz önünde bulundurun *fr.po* örnek.</span><span class="sxs-lookup"><span data-stu-id="63d0e-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="63d0e-187">Razor görünümü konumundaki *Views/Home/About.cshtml* ayrılmış ayarlayarak dosya bağlamı olarak tanımlanabilir `msgctxt` girişin değeri:</span><span class="sxs-lookup"><span data-stu-id="63d0e-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="63d0e-188">İle `msgctxt` şekilde ayarlanırsa, metin çevirisi giderek oluşur `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="63d0e-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="63d0e-189">Çeviri için gezinirken gerçekleşmeyeceğini `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="63d0e-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="63d0e-190">Özel giriş verilen dosya bağlamı ile eşleştiğinde, Orchard Core'nın geri dönüş mekanizması bir bağlam olmadan uygun bir SAS dosyasını arar.</span><span class="sxs-lookup"><span data-stu-id="63d0e-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="63d0e-191">İçin tanımlı hiçbir belirli dosya bağlam olup olmadığı varsayılarak *Views/Home/Contact.cshtml*, gezinme için `/Home/Contact?culture=fr-FR` bir SAS dosya gibi yükler:</span><span class="sxs-lookup"><span data-stu-id="63d0e-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="63d0e-192">PO dosyaları konumunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="63d0e-192">Changing the location of PO files</span></span>

<span data-ttu-id="63d0e-193">PO dosyaları varsayılan konumunu değiştirilebilir `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="63d0e-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="63d0e-194">Bu örnekte, gelen PO dosyaları yüklenir *yerelleştirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="63d0e-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="63d0e-195">Yerelleştirme dosyaları bulmak için özel bir mantıksal uygulama</span><span class="sxs-lookup"><span data-stu-id="63d0e-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="63d0e-196">PO dosyaları bulmak için daha karmaşık mantık gerektiğinde `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` arabirimi uygulanabilir ve hizmet olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="63d0e-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="63d0e-197">Bu, SAS dosyaları farklı konumlarda depolanabilir veya dosyaları bir klasör hiyerarşisi içinde bulunması gerektiğinde kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="63d0e-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="63d0e-198">Pluralized farklı varsayılan dilini kullanma</span><span class="sxs-lookup"><span data-stu-id="63d0e-198">Using a different default pluralized language</span></span>

<span data-ttu-id="63d0e-199">Paketi içeren bir `Plural` iki çoğul biçimi için özel bir genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63d0e-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="63d0e-200">Daha fazla çoğul forms gerektiren diller için bir genişletme yöntemi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63d0e-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="63d0e-201">Bir uzantı yönteminiz, varsayılan dil için herhangi bir yerelleştirme dosyası sağlamanız gerekmez &mdash; özgün dizeleri zaten doğrudan kod içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63d0e-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="63d0e-202">Daha fazla genel kullanabileceğiniz `Plural(int count, string[] pluralForms, params object[] arguments)` çevirileri dize dizisi kabul eden aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="63d0e-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
