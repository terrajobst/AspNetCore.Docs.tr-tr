---
title: DotNet aspnet codegenerator komutu
author: rick-anderson
ms.author: riande
description: ASP.NET Core projeleri dotnet aspnet codegenerator komut iskele oluşturulduğunu
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 55b592d9d203970777c84438e210519957abb35d
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561741"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="eee28-103">DotNet aspnet CodeGenerator öğesinden</span><span class="sxs-lookup"><span data-stu-id="eee28-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="eee28-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eee28-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eee28-105">`dotnet aspnet-codegenerator` -ASP.NET Core yapı iskelesi altyapısı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="eee28-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="eee28-106">`dotnet aspnet-codegenerator` olduğunu komut satırından iskelesini gerekli yalnızca, yapı iskelesi Visual Studio ile kullanmak için gereksinim değildir.</span><span class="sxs-lookup"><span data-stu-id="eee28-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not need to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="eee28-107">Bu makalede açıklanan [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) ve daha sonra.</span><span class="sxs-lookup"><span data-stu-id="eee28-107">This article applies to [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="eee28-108">ASP.NET codegenerator yükleme</span><span class="sxs-lookup"><span data-stu-id="eee28-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="eee28-109">`aspnet-codegenerator` olan bir [genel aracı](/dotnet/core/tools/global-tools) , yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="eee28-109">`aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="eee28-110">Aşağıdaki komut, en son kararlı sürümünü yükler `aspnet-codegenerator` aracı:</span><span class="sxs-lookup"><span data-stu-id="eee28-110">The following command installs the latest stable version of the `aspnet-codegenerator` tool:</span></span>

```console
dotnet tool install -g aspnet-codegenerator
```

<span data-ttu-id="eee28-111">Aşağıdaki komutu kullanarak güncelleştirmeleri `aspnet-codegenerator` en son kararlı sürümü kullanıma yüklü .NET Core SDK'ları için:</span><span class="sxs-lookup"><span data-stu-id="eee28-111">The following command updates `aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```console
dotnet tool update -g aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="eee28-112">Synopsis</span><span class="sxs-lookup"><span data-stu-id="eee28-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="eee28-113">Açıklama</span><span class="sxs-lookup"><span data-stu-id="eee28-113">Description</span></span>

<span data-ttu-id="eee28-114">`dotnet aspnet-codegenerator ` Genel komutu, ASP.NET Core Kod Oluşturucusu ve yapı iskelesi altyapısı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="eee28-114">The `dotnet aspnet-codegenerator ` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="eee28-115">Arguments</span><span class="sxs-lookup"><span data-stu-id="eee28-115">Arguments</span></span>

`generator`

<span data-ttu-id="eee28-116">Çalıştırılacak Kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="eee28-116">The code generator to run.</span></span> <span data-ttu-id="eee28-117">Aşağıdaki oluşturucuları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="eee28-117">The following generators are available:</span></span>

| <span data-ttu-id="eee28-118">Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="eee28-118">Generator</span></span> | <span data-ttu-id="eee28-119">Çalışma</span><span class="sxs-lookup"><span data-stu-id="eee28-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="eee28-120">Alan</span><span class="sxs-lookup"><span data-stu-id="eee28-120">area</span></span>      | [<span data-ttu-id="eee28-121">Bir alan iskelesini kurar.</span><span class="sxs-lookup"><span data-stu-id="eee28-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="eee28-122">denetleyici</span><span class="sxs-lookup"><span data-stu-id="eee28-122">controller</span></span>| [<span data-ttu-id="eee28-123">Bir denetleyici iskele oluşturulduğunu</span><span class="sxs-lookup"><span data-stu-id="eee28-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="eee28-124">kimlik</span><span class="sxs-lookup"><span data-stu-id="eee28-124">identity</span></span>  | [<span data-ttu-id="eee28-125">İskelesini kurar kimlik</span><span class="sxs-lookup"><span data-stu-id="eee28-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="eee28-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="eee28-126">razorpage</span></span> | [<span data-ttu-id="eee28-127">Razor sayfaları iskelesini kurar</span><span class="sxs-lookup"><span data-stu-id="eee28-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="eee28-128">Görünümü</span><span class="sxs-lookup"><span data-stu-id="eee28-128">view</span></span>      | [<span data-ttu-id="eee28-129">Bir görünüm iskele oluşturulduğunu</span><span class="sxs-lookup"><span data-stu-id="eee28-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="eee28-130">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="eee28-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="eee28-131">NuGet paket dizinini belirtir.</span><span class="sxs-lookup"><span data-stu-id="eee28-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="eee28-132">Derleme yapılandırmasını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="eee28-132">Defines the build configuration.</span></span> <span data-ttu-id="eee28-133">Varsayılan değer `Debug` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="eee28-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="eee28-134">Hedef [Framework](/dotnet/standard/frameworks) kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="eee28-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="eee28-135">Örneğin: `net46`</span><span class="sxs-lookup"><span data-stu-id="eee28-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="eee28-136">Derleme temel yol.</span><span class="sxs-lookup"><span data-stu-id="eee28-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="eee28-137">Komut için kısa bir Yardım yazdırır.</span><span class="sxs-lookup"><span data-stu-id="eee28-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="eee28-138">Çalıştırmadan önce projeyi derle değil.</span><span class="sxs-lookup"><span data-stu-id="eee28-138">Doesn't build the project before running.</span></span> <span data-ttu-id="eee28-139">Ayrıca örtülü olarak ayarlar `--no-restore` bayrağı.</span><span class="sxs-lookup"><span data-stu-id="eee28-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="eee28-140">(Klasör adı veya tam yolu) çalıştırmak için proje dosyasının yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="eee28-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="eee28-141">Belirtilmezse, geçerli dizin için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="eee28-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="eee28-142">Oluşturucu seçenekleri</span><span class="sxs-lookup"><span data-stu-id="eee28-142">Generator options</span></span>

<span data-ttu-id="eee28-143">Aşağıdaki bölümlerde seçenekler için desteklenen oluşturucuları vermektedir:</span><span class="sxs-lookup"><span data-stu-id="eee28-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="eee28-144">Alan</span><span class="sxs-lookup"><span data-stu-id="eee28-144">Area</span></span>
* <span data-ttu-id="eee28-145">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="eee28-145">Controller</span></span>
* <span data-ttu-id="eee28-146">Kimlik</span><span class="sxs-lookup"><span data-stu-id="eee28-146">Identity</span></span>  
* <span data-ttu-id="eee28-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="eee28-147">Razorpage</span></span>
* <span data-ttu-id="eee28-148">Görüntüle</span><span class="sxs-lookup"><span data-stu-id="eee28-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="eee28-149">Alan seçenekleri</span><span class="sxs-lookup"><span data-stu-id="eee28-149">Area options</span></span>

<span data-ttu-id="eee28-150">Bu araç, denetleyicileri ve görünümleri ile ASP.NET Core web projeleri için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="eee28-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="eee28-151">Razor sayfaları uygulamalar için tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="eee28-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="eee28-152">Kullanım: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="eee28-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="eee28-153">Önceki komutta aşağıdaki klasörleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="eee28-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="eee28-154">*Alanlar*</span><span class="sxs-lookup"><span data-stu-id="eee28-154">*Areas*</span></span>
  * <span data-ttu-id="eee28-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="eee28-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="eee28-156">*Denetleyiciler*</span><span class="sxs-lookup"><span data-stu-id="eee28-156">*Controllers*</span></span>
    * <span data-ttu-id="eee28-157">*Veri*</span><span class="sxs-lookup"><span data-stu-id="eee28-157">*Data*</span></span>
    * <span data-ttu-id="eee28-158">*Modelleri*</span><span class="sxs-lookup"><span data-stu-id="eee28-158">*Models*</span></span>
    * <span data-ttu-id="eee28-159">*Görünümler*</span><span class="sxs-lookup"><span data-stu-id="eee28-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="eee28-160">Denetleyici seçenekleri</span><span class="sxs-lookup"><span data-stu-id="eee28-160">Controller options</span></span>

<span data-ttu-id="eee28-161">Aşağıdaki tabloda seçeneklerini listeler `aspnet-codegenerator` `controller` ve `razorpage`:</span><span class="sxs-lookup"><span data-stu-id="eee28-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="eee28-162">Benzersiz seçenekleri aşağıdaki tabloda listelenmektedir `aspnet-codegenerator controller`:</span><span class="sxs-lookup"><span data-stu-id="eee28-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="eee28-163">Seçenek</span><span class="sxs-lookup"><span data-stu-id="eee28-163">Option</span></span>               | <span data-ttu-id="eee28-164">Açıklama</span><span class="sxs-lookup"><span data-stu-id="eee28-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="eee28-165">--controllerName veya - ad</span><span class="sxs-lookup"><span data-stu-id="eee28-165">--controllerName or -name</span></span> | <span data-ttu-id="eee28-166">Denetleyicinin adı.</span><span class="sxs-lookup"><span data-stu-id="eee28-166">Name of the controller.</span></span> |
| <span data-ttu-id="eee28-167">--useAsyncActions veya zaman - uyumsuz</span><span class="sxs-lookup"><span data-stu-id="eee28-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="eee28-168">Zaman uyumsuz denetleyici eylemleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="eee28-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="eee28-169">--noViews veya -nv</span><span class="sxs-lookup"><span data-stu-id="eee28-169">--noViews or -nv</span></span> | <span data-ttu-id="eee28-170">Oluşturma **hiçbir** görünümleri.</span><span class="sxs-lookup"><span data-stu-id="eee28-170">Generate **no** views.</span></span> |
| <span data-ttu-id="eee28-171">--restWithNoViews veya -API</span><span class="sxs-lookup"><span data-stu-id="eee28-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="eee28-172">Bir REST stili API denetleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="eee28-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="eee28-173">`noViews` kabul edilir ve tüm ilgili seçenekleri görüntüleyin göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="eee28-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="eee28-174">--readWriteActions veya - Eylemler</span><span class="sxs-lookup"><span data-stu-id="eee28-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="eee28-175">Bir model olmadan okuma/yazma eylemleri ile denetleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="eee28-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="eee28-176">Kullanım `-h` geçiş Yardımı `aspnet-codegenerator controller` komutu:</span><span class="sxs-lookup"><span data-stu-id="eee28-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="eee28-177">Bkz: [film modeli iskelesini](/aspnet/core/tutorials/razor-pages/model) ilişkin bir örnek `dotnet aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="eee28-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="eee28-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="eee28-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="eee28-179">Razor sayfaları kullanılacak şablonu ve yeni sayfa adı belirterek ayrı ayrı başladınız.</span><span class="sxs-lookup"><span data-stu-id="eee28-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="eee28-180">Desteklenen şablonları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="eee28-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="eee28-181">Örneğin, aşağıdaki komutu oluşturmak için Düzen şablonunu kullanır *MyEdit.cshtml* ve *MyEdit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="eee28-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="eee28-182">Genellikle, şablon ve oluşturulan dosya adı belirtilmedi ve aşağıdaki şablonlardan oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="eee28-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="eee28-183">Aşağıdaki tabloda seçeneklerini listeler `aspnet-codegenerator` `razorpage` ve `controller`:</span><span class="sxs-lookup"><span data-stu-id="eee28-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="eee28-184">Benzersiz seçenekleri aşağıdaki tabloda listelenmektedir `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="eee28-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="eee28-185">Seçenek</span><span class="sxs-lookup"><span data-stu-id="eee28-185">Option</span></span>               | <span data-ttu-id="eee28-186">Açıklama</span><span class="sxs-lookup"><span data-stu-id="eee28-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="eee28-187">--namespaceName veya - ad alanı</span><span class="sxs-lookup"><span data-stu-id="eee28-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="eee28-188">Oluşturulan PageModel için kullanılacak ad alanı adı</span><span class="sxs-lookup"><span data-stu-id="eee28-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="eee28-189">--partialView veya - kısmi</span><span class="sxs-lookup"><span data-stu-id="eee28-189">--partialView or -partial</span></span> | <span data-ttu-id="eee28-190">Kısmi görünüm oluşturur.</span><span class="sxs-lookup"><span data-stu-id="eee28-190">Generate a partial view.</span></span> <span data-ttu-id="eee28-191">Belirtilirse, bu düzen seçenekleri -l ve - udl göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="eee28-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="eee28-192">--noPageModel veya - npm</span><span class="sxs-lookup"><span data-stu-id="eee28-192">--noPageModel or -npm</span></span> | <span data-ttu-id="eee28-193">Boş şablon için bir PageModel sınıfı oluşturun değil geç</span><span class="sxs-lookup"><span data-stu-id="eee28-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="eee28-194">Kullanım `-h` geçiş Yardımı `aspnet-codegenerator razorpage` komutu:</span><span class="sxs-lookup"><span data-stu-id="eee28-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="eee28-195">Bkz: [film modeli iskelesini](/aspnet/core/tutorials/razor-pages/model) ilişkin bir örnek `dotnet aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="eee28-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### <a name="identity"></a><span data-ttu-id="eee28-196">Kimlik</span><span class="sxs-lookup"><span data-stu-id="eee28-196">Identity</span></span>

<span data-ttu-id="eee28-197">Bkz: [iskelesini kimlik](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="eee28-197">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>