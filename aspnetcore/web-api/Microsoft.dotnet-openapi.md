---
title: Openapı kullanarak ASP.NET Core uygulamaları geliştirme
author: ryanbrandenburg
description: Openapı dosyalarına başvurular eklemek için ' Microsoft. DotNet-openapı ' aracının nasıl kullanılacağını gösterir.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317770"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="a63a7-103">Openapı araçlarını kullanarak ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="a63a7-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="a63a7-104">İle Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="a63a7-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="a63a7-105">[Microsoft. DotNet-openapı](https://www.nuget.org/packages/Microsoft.dotnet-openapi) , bir proje Içindeki [openapı](https://github.com/OAI/OpenAPI-Specification) başvurularını yönetmek için [.NET Core küresel bir araçtır](/dotnet/core/tools/global-tools) .</span><span class="sxs-lookup"><span data-stu-id="a63a7-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="a63a7-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="a63a7-106">Installation</span></span>

<span data-ttu-id="a63a7-107">Yüklemek `Microsoft.dotnet-openapi`için şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a63a7-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="a63a7-108">Ekle</span><span class="sxs-lookup"><span data-stu-id="a63a7-108">Add</span></span>

<span data-ttu-id="a63a7-109">Bu sayfadaki komutlardan herhangi birini kullanarak bir openapı başvurusu eklemek, *. csproj* dosyasına `<OpenApiReference />` aşağıdakine benzer bir öğe ekler:</span><span class="sxs-lookup"><span data-stu-id="a63a7-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="a63a7-110">Uygulamanın oluşturulan istemci kodunu çağırması için önceki başvuru gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a63a7-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="a63a7-111">Dosya Ekle</span><span class="sxs-lookup"><span data-stu-id="a63a7-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="a63a7-112">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="a63a7-112">Options</span></span>

| <span data-ttu-id="a63a7-113">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-113">Short option</span></span>| <span data-ttu-id="a63a7-114">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-114">Long option</span></span>| <span data-ttu-id="a63a7-115">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-115">Description</span></span> | <span data-ttu-id="a63a7-116">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="a63a7-117">-v</span><span class="sxs-lookup"><span data-stu-id="a63a7-117">-v</span></span>|<span data-ttu-id="a63a7-118">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="a63a7-118">--verbose</span></span> | <span data-ttu-id="a63a7-119">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="a63a7-119">Show verbose output.</span></span> |<span data-ttu-id="a63a7-120">DotNet openapı dosya Ekle *-v* .\Openapi.exe</span><span class="sxs-lookup"><span data-stu-id="a63a7-120">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="a63a7-121">-p</span><span class="sxs-lookup"><span data-stu-id="a63a7-121">-p</span></span>|<span data-ttu-id="a63a7-122">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a63a7-122">--updateProject</span></span> | <span data-ttu-id="a63a7-123">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="a63a7-123">The project to operate on.</span></span> |<span data-ttu-id="a63a7-124">DotNet openapı dosya Ekle *--updateproject .\Ref.exe* .\Openapi.exe</span><span class="sxs-lookup"><span data-stu-id="a63a7-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="a63a7-125">-c</span><span class="sxs-lookup"><span data-stu-id="a63a7-125">-c</span></span>|<span data-ttu-id="a63a7-126">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="a63a7-126">--code-generator</span></span>| <span data-ttu-id="a63a7-127">Başvuruya uygulanacak kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="a63a7-127">The code generator to apply to the reference.</span></span> <span data-ttu-id="a63a7-128">Seçenekler ve `NSwagCSharp` ' `NSwagTypeScript`dir.</span><span class="sxs-lookup"><span data-stu-id="a63a7-128">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="a63a7-129">Belirtilmemişse, araçları varsayılan olarak `NSwagCSharp` ayarlanır.`--code-generator`</span><span class="sxs-lookup"><span data-stu-id="a63a7-129">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="a63a7-130">DotNet openapı Add dosyası .\Openapi.exe JSON--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="a63a7-130">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="a63a7-131">-h</span><span class="sxs-lookup"><span data-stu-id="a63a7-131">-h</span></span>|<span data-ttu-id="a63a7-132">--yardım</span><span class="sxs-lookup"><span data-stu-id="a63a7-132">--help</span></span>|<span data-ttu-id="a63a7-133">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="a63a7-133">Show help information</span></span>|<span data-ttu-id="a63a7-134">DotNet openapı dosya Ekle--yardım</span><span class="sxs-lookup"><span data-stu-id="a63a7-134">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="a63a7-135">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="a63a7-135">Arguments</span></span>

|  <span data-ttu-id="a63a7-136">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="a63a7-136">Argument</span></span>  | <span data-ttu-id="a63a7-137">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-137">Description</span></span> | <span data-ttu-id="a63a7-138">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-138">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="a63a7-139">Kaynak dosya</span><span class="sxs-lookup"><span data-stu-id="a63a7-139">source-file</span></span> | <span data-ttu-id="a63a7-140">Başvuru oluşturulacak kaynak.</span><span class="sxs-lookup"><span data-stu-id="a63a7-140">The source to create a reference from.</span></span> <span data-ttu-id="a63a7-141">Bir Openapı dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a63a7-141">Must be an OpenAPI file.</span></span> |<span data-ttu-id="a63a7-142">DotNet openapı dosya ekleme *.\Openapi.exe*</span><span class="sxs-lookup"><span data-stu-id="a63a7-142">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="a63a7-143">URL ekle</span><span class="sxs-lookup"><span data-stu-id="a63a7-143">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="a63a7-144">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="a63a7-144">Options</span></span>

| <span data-ttu-id="a63a7-145">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-145">Short option</span></span>| <span data-ttu-id="a63a7-146">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-146">Long option</span></span>| <span data-ttu-id="a63a7-147">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-147">Description</span></span> | <span data-ttu-id="a63a7-148">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-148">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="a63a7-149">-v</span><span class="sxs-lookup"><span data-stu-id="a63a7-149">-v</span></span>|<span data-ttu-id="a63a7-150">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="a63a7-150">--verbose</span></span> | <span data-ttu-id="a63a7-151">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="a63a7-151">Show verbose output.</span></span> |<span data-ttu-id="a63a7-152">DotNet openapı URL 'si Ekle *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a63a7-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="a63a7-153">-p</span><span class="sxs-lookup"><span data-stu-id="a63a7-153">-p</span></span>|<span data-ttu-id="a63a7-154">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a63a7-154">--updateProject</span></span> | <span data-ttu-id="a63a7-155">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="a63a7-155">The project to operate on.</span></span> |<span data-ttu-id="a63a7-156">DotNet openapı Add URL *--updateproject .\Ref.exe*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a63a7-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="a63a7-157">-o</span><span class="sxs-lookup"><span data-stu-id="a63a7-157">-o</span></span>|<span data-ttu-id="a63a7-158">--Çıkış-dosya</span><span class="sxs-lookup"><span data-stu-id="a63a7-158">--output-file</span></span> | <span data-ttu-id="a63a7-159">Openapı dosyasının yerel kopyasının yerleştirileceği yer.</span><span class="sxs-lookup"><span data-stu-id="a63a7-159">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="a63a7-160">DotNet openapı Add URL `https://contoso.com/openapi.json` *--çıktı-File MyClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="a63a7-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="a63a7-161">-c</span><span class="sxs-lookup"><span data-stu-id="a63a7-161">-c</span></span>|<span data-ttu-id="a63a7-162">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="a63a7-162">--code-generator</span></span>| <span data-ttu-id="a63a7-163">Başvuruya uygulanacak kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="a63a7-163">The code generator to apply to the reference.</span></span> <span data-ttu-id="a63a7-164">Seçenekler ve `NSwagCSharp` ' `NSwagTypeScript`dir.</span><span class="sxs-lookup"><span data-stu-id="a63a7-164">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="a63a7-165">DotNet openapı Add dosyası .\Openapi.exe JSON--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="a63a7-165">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="a63a7-166">-h</span><span class="sxs-lookup"><span data-stu-id="a63a7-166">-h</span></span>|<span data-ttu-id="a63a7-167">--yardım</span><span class="sxs-lookup"><span data-stu-id="a63a7-167">--help</span></span>|<span data-ttu-id="a63a7-168">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="a63a7-168">Show help information</span></span>|<span data-ttu-id="a63a7-169">DotNet openapı URL ekleme--Yardım</span><span class="sxs-lookup"><span data-stu-id="a63a7-169">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="a63a7-170">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="a63a7-170">Arguments</span></span>

|  <span data-ttu-id="a63a7-171">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="a63a7-171">Argument</span></span>  | <span data-ttu-id="a63a7-172">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-172">Description</span></span> | <span data-ttu-id="a63a7-173">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-173">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="a63a7-174">Kaynak-URL</span><span class="sxs-lookup"><span data-stu-id="a63a7-174">source-URL</span></span> | <span data-ttu-id="a63a7-175">Başvuru oluşturulacak kaynak.</span><span class="sxs-lookup"><span data-stu-id="a63a7-175">The source to create a reference from.</span></span> <span data-ttu-id="a63a7-176">Bir URL olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a63a7-176">Must be a URL.</span></span> |<span data-ttu-id="a63a7-177">DotNet openapı URL 'si Ekle`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a63a7-177">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="a63a7-178">Kaldır</span><span class="sxs-lookup"><span data-stu-id="a63a7-178">Remove</span></span>

<span data-ttu-id="a63a7-179">Verilen dosya adıyla eşleşen Openapı başvurusunu *. csproj* dosyasından kaldırır.</span><span class="sxs-lookup"><span data-stu-id="a63a7-179">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="a63a7-180">Openapı başvurusu kaldırıldığında, istemciler oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="a63a7-180">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="a63a7-181">Local *. JSON* ve *. YAML* dosyaları silinir.</span><span class="sxs-lookup"><span data-stu-id="a63a7-181">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="a63a7-182">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="a63a7-182">Options</span></span>

| <span data-ttu-id="a63a7-183">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-183">Short option</span></span>| <span data-ttu-id="a63a7-184">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-184">Long option</span></span>| <span data-ttu-id="a63a7-185">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-185">Description</span></span>| <span data-ttu-id="a63a7-186">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-186">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="a63a7-187">-v</span><span class="sxs-lookup"><span data-stu-id="a63a7-187">-v</span></span>|<span data-ttu-id="a63a7-188">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="a63a7-188">--verbose</span></span> | <span data-ttu-id="a63a7-189">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="a63a7-189">Show verbose output.</span></span> |<span data-ttu-id="a63a7-190">DotNet openapı Remove *-v*</span><span class="sxs-lookup"><span data-stu-id="a63a7-190">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="a63a7-191">-p</span><span class="sxs-lookup"><span data-stu-id="a63a7-191">-p</span></span>|<span data-ttu-id="a63a7-192">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a63a7-192">--updateProject</span></span> | <span data-ttu-id="a63a7-193">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="a63a7-193">The project to operate on.</span></span> |<span data-ttu-id="a63a7-194">DotNet openapı Remove *--updateproject .\ref. csproj* .\openapi.exe</span><span class="sxs-lookup"><span data-stu-id="a63a7-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="a63a7-195">-h</span><span class="sxs-lookup"><span data-stu-id="a63a7-195">-h</span></span>|<span data-ttu-id="a63a7-196">--yardım</span><span class="sxs-lookup"><span data-stu-id="a63a7-196">--help</span></span>|<span data-ttu-id="a63a7-197">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="a63a7-197">Show help information</span></span>|<span data-ttu-id="a63a7-198">DotNet openapı Remove--Help</span><span class="sxs-lookup"><span data-stu-id="a63a7-198">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="a63a7-199">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="a63a7-199">Arguments</span></span>

|  <span data-ttu-id="a63a7-200">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="a63a7-200">Argument</span></span>  | <span data-ttu-id="a63a7-201">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-201">Description</span></span>| <span data-ttu-id="a63a7-202">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-202">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="a63a7-203">Kaynak dosya</span><span class="sxs-lookup"><span data-stu-id="a63a7-203">source-file</span></span> | <span data-ttu-id="a63a7-204">Başvurunun kaldırılacağı kaynak.</span><span class="sxs-lookup"><span data-stu-id="a63a7-204">The source to remove the reference to.</span></span> |<span data-ttu-id="a63a7-205">DotNet openapı kaldırma *.\Openapi.exe*</span><span class="sxs-lookup"><span data-stu-id="a63a7-205">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="a63a7-206">Yenile</span><span class="sxs-lookup"><span data-stu-id="a63a7-206">Refresh</span></span>

<span data-ttu-id="a63a7-207">İndirme URL 'sindeki en son içerik kullanılarak indirilen bir dosyanın yerel sürümünü yeniler.</span><span class="sxs-lookup"><span data-stu-id="a63a7-207">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="a63a7-208">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="a63a7-208">Options</span></span>

| <span data-ttu-id="a63a7-209">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-209">Short option</span></span>| <span data-ttu-id="a63a7-210">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="a63a7-210">Long option</span></span>| <span data-ttu-id="a63a7-211">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-211">Description</span></span> | <span data-ttu-id="a63a7-212">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-212">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="a63a7-213">-v</span><span class="sxs-lookup"><span data-stu-id="a63a7-213">-v</span></span>|<span data-ttu-id="a63a7-214">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="a63a7-214">--verbose</span></span> | <span data-ttu-id="a63a7-215">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="a63a7-215">Show verbose output.</span></span> | <span data-ttu-id="a63a7-216">DotNet openapı yenileme *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a63a7-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="a63a7-217">-p</span><span class="sxs-lookup"><span data-stu-id="a63a7-217">-p</span></span>|<span data-ttu-id="a63a7-218">--updateProject</span><span class="sxs-lookup"><span data-stu-id="a63a7-218">--updateProject</span></span> | <span data-ttu-id="a63a7-219">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="a63a7-219">The project to operate on.</span></span> | <span data-ttu-id="a63a7-220">DotNet openapı yenileme *--updateproject .\ref.exe*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a63a7-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="a63a7-221">-h</span><span class="sxs-lookup"><span data-stu-id="a63a7-221">-h</span></span>|<span data-ttu-id="a63a7-222">--yardım</span><span class="sxs-lookup"><span data-stu-id="a63a7-222">--help</span></span>|<span data-ttu-id="a63a7-223">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="a63a7-223">Show help information</span></span>|<span data-ttu-id="a63a7-224">DotNet openapı yenilemesi--yardım</span><span class="sxs-lookup"><span data-stu-id="a63a7-224">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="a63a7-225">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="a63a7-225">Arguments</span></span>

|  <span data-ttu-id="a63a7-226">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="a63a7-226">Argument</span></span>  | <span data-ttu-id="a63a7-227">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a63a7-227">Description</span></span> | <span data-ttu-id="a63a7-228">Örnek</span><span class="sxs-lookup"><span data-stu-id="a63a7-228">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="a63a7-229">Kaynak-URL</span><span class="sxs-lookup"><span data-stu-id="a63a7-229">source-URL</span></span> | <span data-ttu-id="a63a7-230">Başvurunun yenileneceği URL.</span><span class="sxs-lookup"><span data-stu-id="a63a7-230">The URL to refresh the reference from.</span></span> | <span data-ttu-id="a63a7-231">DotNet openapı yenilemesi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="a63a7-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
