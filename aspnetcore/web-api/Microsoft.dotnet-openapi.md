---
title: Openapı kullanarak ASP.NET Core uygulamaları geliştirme
author: ryanbrandenburg
description: Openapı dosyalarına başvurular eklemek için ' Microsoft. DotNet-openapı ' aracının nasıl kullanılacağını gösterir.
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187462"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="2f28f-103">Openapı araçlarını kullanarak ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="2f28f-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="2f28f-104">İle Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="2f28f-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="2f28f-105">`Microsoft.dotnet-openapi`, bir proje içindeki [Openapı](https://github.com/OAI/OpenAPI-Specification) başvurularını yönetmek Için .NET Core küresel bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="2f28f-105">`Microsoft.dotnet-openapi` is a .NET Core global tool for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="2f28f-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="2f28f-106">Installation</span></span>

<span data-ttu-id="2f28f-107">Yüklemek `Microsoft.dotnet-openapi`için şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2f28f-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

<span data-ttu-id="2f28f-108">`Microsoft.dotnet-openapi`, [.NET Core küresel bir araçtır](/dotnet/core/tools/global-tools).</span><span class="sxs-lookup"><span data-stu-id="2f28f-108">`Microsoft.dotnet-openapi` is a [.NET Core Global Tool](/dotnet/core/tools/global-tools).</span></span>

## <a name="add"></a><span data-ttu-id="2f28f-109">Ekle</span><span class="sxs-lookup"><span data-stu-id="2f28f-109">Add</span></span>

<span data-ttu-id="2f28f-110">Bu sayfadaki komutlardan herhangi birini kullanarak bir openapı başvurusu eklemek, *. csproj* dosyasına `<OpenApiReference />` aşağıdakine benzer bir öğe ekler:</span><span class="sxs-lookup"><span data-stu-id="2f28f-110">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />`  element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="2f28f-111">Uygulamanın oluşturulan istemci kodunu çağırması için önceki başvuru gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2f28f-111">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="2f28f-112">Dosya Ekle</span><span class="sxs-lookup"><span data-stu-id="2f28f-112">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="2f28f-113">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="2f28f-113">Options</span></span>

| <span data-ttu-id="2f28f-114">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-114">Short option</span></span>| <span data-ttu-id="2f28f-115">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-115">Long option</span></span>| <span data-ttu-id="2f28f-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-116">Description</span></span> | <span data-ttu-id="2f28f-117">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-117">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="2f28f-118">-v</span><span class="sxs-lookup"><span data-stu-id="2f28f-118">-v</span></span>|<span data-ttu-id="2f28f-119">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="2f28f-119">--verbose</span></span> | <span data-ttu-id="2f28f-120">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="2f28f-120">Show verbose output.</span></span> |<span data-ttu-id="2f28f-121">DotNet openapı dosya Ekle *-v* .\Openapi.exe</span><span class="sxs-lookup"><span data-stu-id="2f28f-121">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="2f28f-122">-p</span><span class="sxs-lookup"><span data-stu-id="2f28f-122">-p</span></span>|<span data-ttu-id="2f28f-123">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2f28f-123">--updateProject</span></span> | <span data-ttu-id="2f28f-124">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="2f28f-124">The project to operate on.</span></span> |<span data-ttu-id="2f28f-125">DotNet openapı dosya Ekle *--updateproject .\Ref.exe* .\Openapi.exe</span><span class="sxs-lookup"><span data-stu-id="2f28f-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="2f28f-126">-c</span><span class="sxs-lookup"><span data-stu-id="2f28f-126">-c</span></span>|<span data-ttu-id="2f28f-127">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="2f28f-127">--code-generator</span></span>| <span data-ttu-id="2f28f-128">Başvuruya uygulanacak kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="2f28f-128">The code generator to apply to the reference.</span></span> <span data-ttu-id="2f28f-129">Seçenekler ve `NSwagCSharp` ' `NSwagTypeScript`dir.</span><span class="sxs-lookup"><span data-stu-id="2f28f-129">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="2f28f-130">Belirtilmemişse, araçları varsayılan olarak `NSwagCSharp` ayarlanır.`--code-generator`</span><span class="sxs-lookup"><span data-stu-id="2f28f-130">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="2f28f-131">DotNet openapı Add dosyası .\Openapi.exe JSON--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="2f28f-131">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="2f28f-132">-h</span><span class="sxs-lookup"><span data-stu-id="2f28f-132">-h</span></span>|<span data-ttu-id="2f28f-133">--yardım</span><span class="sxs-lookup"><span data-stu-id="2f28f-133">--help</span></span>|<span data-ttu-id="2f28f-134">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="2f28f-134">Show help information</span></span>|<span data-ttu-id="2f28f-135">DotNet openapı dosya Ekle--yardım</span><span class="sxs-lookup"><span data-stu-id="2f28f-135">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="2f28f-136">Arguments</span><span class="sxs-lookup"><span data-stu-id="2f28f-136">Arguments</span></span>

|  <span data-ttu-id="2f28f-137">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="2f28f-137">Argument</span></span>  | <span data-ttu-id="2f28f-138">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-138">Description</span></span> | <span data-ttu-id="2f28f-139">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-139">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="2f28f-140">Kaynak dosya</span><span class="sxs-lookup"><span data-stu-id="2f28f-140">source-file</span></span> | <span data-ttu-id="2f28f-141">Başvuru oluşturulacak kaynak.</span><span class="sxs-lookup"><span data-stu-id="2f28f-141">The source to create a reference from.</span></span> <span data-ttu-id="2f28f-142">Bir Openapı dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2f28f-142">Must be an OpenAPI file.</span></span> |<span data-ttu-id="2f28f-143">DotNet openapı dosya ekleme *.\Openapi.exe*</span><span class="sxs-lookup"><span data-stu-id="2f28f-143">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="2f28f-144">URL ekle</span><span class="sxs-lookup"><span data-stu-id="2f28f-144">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="2f28f-145">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="2f28f-145">Options</span></span>

| <span data-ttu-id="2f28f-146">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-146">Short option</span></span>| <span data-ttu-id="2f28f-147">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-147">Long option</span></span>| <span data-ttu-id="2f28f-148">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-148">Description</span></span> | <span data-ttu-id="2f28f-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-149">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="2f28f-150">-v</span><span class="sxs-lookup"><span data-stu-id="2f28f-150">-v</span></span>|<span data-ttu-id="2f28f-151">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="2f28f-151">--verbose</span></span> | <span data-ttu-id="2f28f-152">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="2f28f-152">Show verbose output.</span></span> |<span data-ttu-id="2f28f-153">DotNet openapı URL 'si Ekle *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2f28f-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2f28f-154">-p</span><span class="sxs-lookup"><span data-stu-id="2f28f-154">-p</span></span>|<span data-ttu-id="2f28f-155">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2f28f-155">--updateProject</span></span> | <span data-ttu-id="2f28f-156">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="2f28f-156">The project to operate on.</span></span> |<span data-ttu-id="2f28f-157">DotNet openapı Add URL *--updateproject .\Ref.exe*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2f28f-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2f28f-158">-o</span><span class="sxs-lookup"><span data-stu-id="2f28f-158">-o</span></span>|<span data-ttu-id="2f28f-159">--Çıkış-dosya</span><span class="sxs-lookup"><span data-stu-id="2f28f-159">--output-file</span></span> | <span data-ttu-id="2f28f-160">Openapı dosyasının yerel kopyasının yerleştirileceği yer.</span><span class="sxs-lookup"><span data-stu-id="2f28f-160">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="2f28f-161">DotNet openapı Add URL `https://contoso.com/openapi.json` *--çıktı-File MyClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="2f28f-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="2f28f-162">-c</span><span class="sxs-lookup"><span data-stu-id="2f28f-162">-c</span></span>|<span data-ttu-id="2f28f-163">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="2f28f-163">--code-generator</span></span>| <span data-ttu-id="2f28f-164">Başvuruya uygulanacak kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="2f28f-164">The code generator to apply to the reference.</span></span> <span data-ttu-id="2f28f-165">Seçenekler ve `NSwagCSharp` ' `NSwagTypeScript`dir.</span><span class="sxs-lookup"><span data-stu-id="2f28f-165">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="2f28f-166">DotNet openapı Add dosyası .\Openapi.exe JSON--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="2f28f-166">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="2f28f-167">-h</span><span class="sxs-lookup"><span data-stu-id="2f28f-167">-h</span></span>|<span data-ttu-id="2f28f-168">--yardım</span><span class="sxs-lookup"><span data-stu-id="2f28f-168">--help</span></span>|<span data-ttu-id="2f28f-169">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="2f28f-169">Show help information</span></span>|<span data-ttu-id="2f28f-170">DotNet openapı URL ekleme--Yardım</span><span class="sxs-lookup"><span data-stu-id="2f28f-170">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="2f28f-171">Arguments</span><span class="sxs-lookup"><span data-stu-id="2f28f-171">Arguments</span></span>

|  <span data-ttu-id="2f28f-172">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="2f28f-172">Argument</span></span>  | <span data-ttu-id="2f28f-173">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-173">Description</span></span> | <span data-ttu-id="2f28f-174">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-174">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="2f28f-175">Kaynak-URL</span><span class="sxs-lookup"><span data-stu-id="2f28f-175">source-URL</span></span> | <span data-ttu-id="2f28f-176">Başvuru oluşturulacak kaynak.</span><span class="sxs-lookup"><span data-stu-id="2f28f-176">The source to create a reference from.</span></span> <span data-ttu-id="2f28f-177">Bir URL olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2f28f-177">Must be a URL.</span></span> |<span data-ttu-id="2f28f-178">DotNet openapı URL 'si Ekle`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2f28f-178">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="2f28f-179">Kaldır</span><span class="sxs-lookup"><span data-stu-id="2f28f-179">Remove</span></span>

<span data-ttu-id="2f28f-180">Verilen dosya adıyla eşleşen Openapı başvurusunu *. csproj* dosyasından kaldırır.</span><span class="sxs-lookup"><span data-stu-id="2f28f-180">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="2f28f-181">Openapı başvurusu kaldırıldığında, istemciler oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="2f28f-181">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="2f28f-182">Local *. JSON* ve *. YAML* dosyaları silinir.</span><span class="sxs-lookup"><span data-stu-id="2f28f-182">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="2f28f-183">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="2f28f-183">Options</span></span>

| <span data-ttu-id="2f28f-184">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-184">Short option</span></span>| <span data-ttu-id="2f28f-185">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-185">Long option</span></span>| <span data-ttu-id="2f28f-186">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-186">Description</span></span>| <span data-ttu-id="2f28f-187">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-187">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="2f28f-188">-v</span><span class="sxs-lookup"><span data-stu-id="2f28f-188">-v</span></span>|<span data-ttu-id="2f28f-189">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="2f28f-189">--verbose</span></span> | <span data-ttu-id="2f28f-190">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="2f28f-190">Show verbose output.</span></span> |<span data-ttu-id="2f28f-191">DotNet openapı Remove *-v*</span><span class="sxs-lookup"><span data-stu-id="2f28f-191">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="2f28f-192">-p</span><span class="sxs-lookup"><span data-stu-id="2f28f-192">-p</span></span>|<span data-ttu-id="2f28f-193">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2f28f-193">--updateProject</span></span> | <span data-ttu-id="2f28f-194">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="2f28f-194">The project to operate on.</span></span> |<span data-ttu-id="2f28f-195">DotNet openapı Remove *--updateproject .\ref. csproj* .\openapi.exe</span><span class="sxs-lookup"><span data-stu-id="2f28f-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="2f28f-196">-h</span><span class="sxs-lookup"><span data-stu-id="2f28f-196">-h</span></span>|<span data-ttu-id="2f28f-197">--yardım</span><span class="sxs-lookup"><span data-stu-id="2f28f-197">--help</span></span>|<span data-ttu-id="2f28f-198">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="2f28f-198">Show help information</span></span>|<span data-ttu-id="2f28f-199">DotNet openapı Remove--Help</span><span class="sxs-lookup"><span data-stu-id="2f28f-199">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="2f28f-200">Arguments</span><span class="sxs-lookup"><span data-stu-id="2f28f-200">Arguments</span></span>

|  <span data-ttu-id="2f28f-201">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="2f28f-201">Argument</span></span>  | <span data-ttu-id="2f28f-202">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-202">Description</span></span>| <span data-ttu-id="2f28f-203">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-203">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="2f28f-204">Kaynak dosya</span><span class="sxs-lookup"><span data-stu-id="2f28f-204">source-file</span></span> | <span data-ttu-id="2f28f-205">Başvurunun kaldırılacağı kaynak.</span><span class="sxs-lookup"><span data-stu-id="2f28f-205">The source to remove the reference to.</span></span> |<span data-ttu-id="2f28f-206">DotNet openapı kaldırma *.\Openapi.exe*</span><span class="sxs-lookup"><span data-stu-id="2f28f-206">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="2f28f-207">Yenile</span><span class="sxs-lookup"><span data-stu-id="2f28f-207">Refresh</span></span>

<span data-ttu-id="2f28f-208">İndirme URL 'sindeki en son içerik kullanılarak indirilen bir dosyanın yerel sürümünü yeniler.</span><span class="sxs-lookup"><span data-stu-id="2f28f-208">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="2f28f-209">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="2f28f-209">Options</span></span>

| <span data-ttu-id="2f28f-210">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-210">Short option</span></span>| <span data-ttu-id="2f28f-211">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="2f28f-211">Long option</span></span>| <span data-ttu-id="2f28f-212">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-212">Description</span></span> | <span data-ttu-id="2f28f-213">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-213">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="2f28f-214">-v</span><span class="sxs-lookup"><span data-stu-id="2f28f-214">-v</span></span>|<span data-ttu-id="2f28f-215">--ayrıntılı</span><span class="sxs-lookup"><span data-stu-id="2f28f-215">--verbose</span></span> | <span data-ttu-id="2f28f-216">Ayrıntılı çıktıyı göster.</span><span class="sxs-lookup"><span data-stu-id="2f28f-216">Show verbose output.</span></span> | <span data-ttu-id="2f28f-217">DotNet openapı yenileme *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2f28f-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2f28f-218">-p</span><span class="sxs-lookup"><span data-stu-id="2f28f-218">-p</span></span>|<span data-ttu-id="2f28f-219">--updateProject</span><span class="sxs-lookup"><span data-stu-id="2f28f-219">--updateProject</span></span> | <span data-ttu-id="2f28f-220">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="2f28f-220">The project to operate on.</span></span> | <span data-ttu-id="2f28f-221">DotNet openapı yenileme *--updateproject .\ref.exe*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2f28f-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="2f28f-222">-h</span><span class="sxs-lookup"><span data-stu-id="2f28f-222">-h</span></span>|<span data-ttu-id="2f28f-223">--yardım</span><span class="sxs-lookup"><span data-stu-id="2f28f-223">--help</span></span>|<span data-ttu-id="2f28f-224">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="2f28f-224">Show help information</span></span>|<span data-ttu-id="2f28f-225">DotNet openapı yenilemesi--yardım</span><span class="sxs-lookup"><span data-stu-id="2f28f-225">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="2f28f-226">Arguments</span><span class="sxs-lookup"><span data-stu-id="2f28f-226">Arguments</span></span>

|  <span data-ttu-id="2f28f-227">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="2f28f-227">Argument</span></span>  | <span data-ttu-id="2f28f-228">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2f28f-228">Description</span></span> | <span data-ttu-id="2f28f-229">Örnek</span><span class="sxs-lookup"><span data-stu-id="2f28f-229">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="2f28f-230">Kaynak-URL</span><span class="sxs-lookup"><span data-stu-id="2f28f-230">source-URL</span></span> | <span data-ttu-id="2f28f-231">Başvurunun yenileneceği URL.</span><span class="sxs-lookup"><span data-stu-id="2f28f-231">The URL to refresh the reference from.</span></span> | <span data-ttu-id="2f28f-232">DotNet openapı yenilemesi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="2f28f-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
