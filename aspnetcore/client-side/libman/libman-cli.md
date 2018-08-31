---
title: LibMan komut satırı arabirimi (CLI) ile ASP.NET Core kullanın
author: scottaddie
description: ASP.NET Core projesinde LibMan komut satırı arabirimi (CLI) kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: ad81af2e789a31382f50ed37754bfc94469eb197
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336043"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="455fb-103">LibMan komut satırı arabirimi (CLI) ile ASP.NET Core kullanın</span><span class="sxs-lookup"><span data-stu-id="455fb-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="455fb-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="455fb-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="455fb-105">[LibMan](xref:client-side/libman/index) CLI, .NET Core desteklendiği her yerde desteklenen bir platformlar arası araçtır.</span><span class="sxs-lookup"><span data-stu-id="455fb-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="455fb-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="455fb-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="455fb-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="455fb-107">Installation</span></span>

<span data-ttu-id="455fb-108">LibMan CLI'yı yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="455fb-109">A [.NET Core genel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) gelen yüklü [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="455fb-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="455fb-110">Belirli bir NuGet paketini kaynaktan LibMan CLI'yı yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="455fb-111">Önceki örnekte, yerel Windows makineden ait bir .NET Core genel aracı yüklü *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* dosya.</span><span class="sxs-lookup"><span data-stu-id="455fb-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="455fb-112">Kullanım</span><span class="sxs-lookup"><span data-stu-id="455fb-112">Usage</span></span>

<span data-ttu-id="455fb-113">CLI'ın başarılı yüklemeden sonra aşağıdaki komut kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="455fb-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="455fb-114">Yüklü CLI sürümünü görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="455fb-115">Kullanılabilir tüm CLI komutları görmek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="455fb-116">Yukarıdaki komut, aşağıdakine benzer çıkış görüntüler:</span><span class="sxs-lookup"><span data-stu-id="455fb-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="455fb-117">Aşağıdaki bölümlerde kullanılabilir CLI komutları özetler.</span><span class="sxs-lookup"><span data-stu-id="455fb-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="455fb-118">Projede LibMan Başlat</span><span class="sxs-lookup"><span data-stu-id="455fb-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="455fb-119">`libman init` Komut oluşturur bir *libman.json* yoksa dosya.</span><span class="sxs-lookup"><span data-stu-id="455fb-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="455fb-120">Dosyanın varsayılan öğe şablonu içeriği ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="455fb-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="455fb-121">Özeti</span><span class="sxs-lookup"><span data-stu-id="455fb-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="455fb-122">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="455fb-122">Options</span></span>

<span data-ttu-id="455fb-123">Aşağıdaki seçenekler kullanılabilir `libman init` komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="455fb-124">Geçerli klasörüne göreli bir yol.</span><span class="sxs-lookup"><span data-stu-id="455fb-124">A path relative to the current folder.</span></span> <span data-ttu-id="455fb-125">Kitaplık dosyaları, bu konumda yüklenir, hiçbir `destination` özelliği bir kitaplıkta tanımlaması *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="455fb-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="455fb-126">`<PATH>` Değer yazılır `defaultDestination` özelliği *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="455fb-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="455fb-127">Belirli bir kitaplık için bir sağlayıcı tanımlanmışsa kullanılacak sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="455fb-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="455fb-128">`<PROVIDER>` Değer yazılır `defaultProvider` özelliği *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="455fb-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="455fb-129">Değiştirin `<PROVIDER>` aşağıdaki değerlerden biriyle:</span><span class="sxs-lookup"><span data-stu-id="455fb-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="455fb-130">Örnekler</span><span class="sxs-lookup"><span data-stu-id="455fb-130">Examples</span></span>

<span data-ttu-id="455fb-131">Oluşturmak için bir *libman.json* bir ASP.NET Core proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="455fb-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="455fb-132">Proje kök dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="455fb-132">Navigate to the project root.</span></span>
* <span data-ttu-id="455fb-133">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="455fb-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="455fb-134">Tuşuna basın veya varsayılan sağlayıcı adı `Enter` varsayılan CDNJS sağlayıcıyı kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="455fb-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="455fb-135">Geçerli değerler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="455fb-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init komutunu - varsayılan sağlayıcı](_static/libman-init-provider.png)

<span data-ttu-id="455fb-137">A *libman.json* dosya aşağıdaki içeriğe sahip proje kök eklenir:</span><span class="sxs-lookup"><span data-stu-id="455fb-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="455fb-138">Kitaplık dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="455fb-138">Add library files</span></span>

<span data-ttu-id="455fb-139">`libman install` Komut indirir ve projeye kitaplık dosyalarını yükler.</span><span class="sxs-lookup"><span data-stu-id="455fb-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="455fb-140">A *libman.json* dosya yoksa eklenir.</span><span class="sxs-lookup"><span data-stu-id="455fb-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="455fb-141">*Libman.json* yapılandırma ayrıntıları için kitaplık dosyalarını depolamak için dosya değiştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="455fb-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="455fb-142">Özeti</span><span class="sxs-lookup"><span data-stu-id="455fb-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="455fb-143">Arguments</span><span class="sxs-lookup"><span data-stu-id="455fb-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="455fb-144">Yüklemek için kitaplık adı.</span><span class="sxs-lookup"><span data-stu-id="455fb-144">The name of the library to install.</span></span> <span data-ttu-id="455fb-145">Bu ad, sürüm numarası gösterimi içerebilir (örneğin, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="455fb-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="455fb-146">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="455fb-146">Options</span></span>

<span data-ttu-id="455fb-147">Aşağıdaki seçenekler kullanılabilir `libman install` komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="455fb-148">Kitaplığı yüklemek için konum.</span><span class="sxs-lookup"><span data-stu-id="455fb-148">The location to install the library.</span></span> <span data-ttu-id="455fb-149">Belirtilmezse, varsayılan konumu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="455fb-149">If not specified, the default location is used.</span></span> <span data-ttu-id="455fb-150">Hayır ise `defaultDestination` özelliği belirtilen *libman.json*, bu seçenek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="455fb-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="455fb-151">Kitaplıktan yüklenecek dosyanın adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="455fb-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="455fb-152">Belirtilmezse, tüm dosyaları Kitaplığı'ndan yüklenir.</span><span class="sxs-lookup"><span data-stu-id="455fb-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="455fb-153">Bir tane sağlayın `--files` yüklenmesi için dosya başına seçeneği.</span><span class="sxs-lookup"><span data-stu-id="455fb-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="455fb-154">Göreli yollar çok desteklenir.</span><span class="sxs-lookup"><span data-stu-id="455fb-154">Relative paths are supported too.</span></span> <span data-ttu-id="455fb-155">Örneğin: `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="455fb-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="455fb-156">Kitaplık alımı için kullanılan sağlayıcının adı.</span><span class="sxs-lookup"><span data-stu-id="455fb-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="455fb-157">Değiştirin `<PROVIDER>` aşağıdaki değerlerden biriyle:</span><span class="sxs-lookup"><span data-stu-id="455fb-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="455fb-158">Belirtilmezse, `defaultProvider` özelliğinde *libman.json* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="455fb-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="455fb-159">Hayır ise `defaultProvider` özelliği belirtilen *libman.json*, bu seçenek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="455fb-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="455fb-160">Örnekler</span><span class="sxs-lookup"><span data-stu-id="455fb-160">Examples</span></span>

<span data-ttu-id="455fb-161">Aşağıdakileri göz önünde bulundurun *libman.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="455fb-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="455fb-162">3.2.1 jQuery sürümünü yüklemek için *jquery.min.js* dosyasını *wwwroot\scripts\jquery* CDNJS sağlayıcısını kullanarak klasör:</span><span class="sxs-lookup"><span data-stu-id="455fb-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot\scripts\jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot\scripts\jquery --files jquery.min.js
```

<span data-ttu-id="455fb-163">*Libman.json* dosyası aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="455fb-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="455fb-164">Yüklenecek *calendar.js* ve *calendar.css* dosyalarını *C:\\temp\\contosoCalendar\\*  dosya sistemini kullanma Sağlayıcı:</span><span class="sxs-lookup"><span data-stu-id="455fb-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="455fb-165">Aşağıdaki istemi iki nedenden dolayı görünür:</span><span class="sxs-lookup"><span data-stu-id="455fb-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="455fb-166">*Libman.json* dosya içermediğinden bir `defaultDestination` özelliği.</span><span class="sxs-lookup"><span data-stu-id="455fb-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="455fb-167">`libman install` Komut içermiyor `-d|--destination` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="455fb-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman yükleme komutunu - hedef](_static/libman-install-destination.png)

<span data-ttu-id="455fb-169">Sonra varsayılan hedef olarak kabul *libman.json* dosyası aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="455fb-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot\\lib\\contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="455fb-170">Kitaplık dosyaları geri yükleme</span><span class="sxs-lookup"><span data-stu-id="455fb-170">Restore library files</span></span>

<span data-ttu-id="455fb-171">`libman restore` Komutu kitaplık dosyaları içinde tanımlanan yükler *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="455fb-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="455fb-172">Aşağıdaki kurallar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="455fb-172">The following rules apply:</span></span>

* <span data-ttu-id="455fb-173">Hayır ise *libman.json* dosyası, proje kök dizininde mevcut, hata döndürülür.</span><span class="sxs-lookup"><span data-stu-id="455fb-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="455fb-174">Kitaplığa bir sağlayıcı belirtiyorsa `defaultProvider` özelliğinde *libman.json* göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="455fb-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="455fb-175">Kitaplığa bir hedef belirtiyorsa `defaultDestination` özelliğinde *libman.json* göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="455fb-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="455fb-176">Özeti</span><span class="sxs-lookup"><span data-stu-id="455fb-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="455fb-177">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="455fb-177">Options</span></span>

<span data-ttu-id="455fb-178">Aşağıdaki seçenekler kullanılabilir `libman restore` komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="455fb-179">Örnekler</span><span class="sxs-lookup"><span data-stu-id="455fb-179">Examples</span></span>

<span data-ttu-id="455fb-180">Tanımlanan kitaplık dosyaları geri yüklemek için *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="455fb-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="455fb-181">Kitaplık dosyaları sil</span><span class="sxs-lookup"><span data-stu-id="455fb-181">Delete library files</span></span>

<span data-ttu-id="455fb-182">`libman clean` Komut, daha önce LibMan geri kitaplık dosyalarını siler.</span><span class="sxs-lookup"><span data-stu-id="455fb-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="455fb-183">Bu işlemden sonra boş duruma klasörler silinir.</span><span class="sxs-lookup"><span data-stu-id="455fb-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="455fb-184">Kitaplık dosyaları ilişkili yapılandırmalarında `libraries` özelliği *libman.json* kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="455fb-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="455fb-185">Özeti</span><span class="sxs-lookup"><span data-stu-id="455fb-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="455fb-186">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="455fb-186">Options</span></span>

<span data-ttu-id="455fb-187">Aşağıdaki seçenekler kullanılabilir `libman clean` komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="455fb-188">Örnekler</span><span class="sxs-lookup"><span data-stu-id="455fb-188">Examples</span></span>

<span data-ttu-id="455fb-189">Yüklü LibMan kitaplık dosyalarını silmek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="455fb-190">Kitaplık dosyaları kaldırma</span><span class="sxs-lookup"><span data-stu-id="455fb-190">Uninstall library files</span></span>

<span data-ttu-id="455fb-191">`libman uninstall` Komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="455fb-192">Belirtilen hedef kitaplıktan ile ilişkili tüm dosyaları siler *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="455fb-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="455fb-193">İlişkili kitaplık yapılandırmasını kaldırır *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="455fb-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="455fb-194">Bir hata meydana gelir olduğunda:</span><span class="sxs-lookup"><span data-stu-id="455fb-194">An error occurs when:</span></span>

* <span data-ttu-id="455fb-195">Hayır *libman.json* dosyası, proje kök dizininde mevcut.</span><span class="sxs-lookup"><span data-stu-id="455fb-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="455fb-196">Belirtilen kitaplık yok.</span><span class="sxs-lookup"><span data-stu-id="455fb-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="455fb-197">Aynı ada sahip birden fazla kitaplık yüklüyse bunlardan birini seçmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="455fb-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="455fb-198">Özeti</span><span class="sxs-lookup"><span data-stu-id="455fb-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="455fb-199">Arguments</span><span class="sxs-lookup"><span data-stu-id="455fb-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="455fb-200">Kaldırmak için kitaplığının adı.</span><span class="sxs-lookup"><span data-stu-id="455fb-200">The name of the library to uninstall.</span></span> <span data-ttu-id="455fb-201">Bu ad, sürüm numarası gösterimi içerebilir (örneğin, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="455fb-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="455fb-202">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="455fb-202">Options</span></span>

<span data-ttu-id="455fb-203">Aşağıdaki seçenekler kullanılabilir `libman uninstall` komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="455fb-204">Örnekler</span><span class="sxs-lookup"><span data-stu-id="455fb-204">Examples</span></span>

<span data-ttu-id="455fb-205">Aşağıdakileri göz önünde bulundurun *libman.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="455fb-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="455fb-206">JQuery kaldırmak için aşağıdaki komutlardan birini başarılı:</span><span class="sxs-lookup"><span data-stu-id="455fb-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="455fb-207">Aracılığıyla yüklenen Lodash dosyaları kaldırmak için `filesystem` sağlayıcısı:</span><span class="sxs-lookup"><span data-stu-id="455fb-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="455fb-208">Kitaplık sürümünü güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="455fb-208">Update library version</span></span>

<span data-ttu-id="455fb-209">`libman update` Komutu LibMan belirtilen sürümü için yüklü bir kitaplık güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="455fb-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="455fb-210">Bir hata meydana gelir olduğunda:</span><span class="sxs-lookup"><span data-stu-id="455fb-210">An error occurs when:</span></span>

* <span data-ttu-id="455fb-211">Hayır *libman.json* dosyası, proje kök dizininde mevcut.</span><span class="sxs-lookup"><span data-stu-id="455fb-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="455fb-212">Belirtilen kitaplık yok.</span><span class="sxs-lookup"><span data-stu-id="455fb-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="455fb-213">Aynı ada sahip birden fazla kitaplık yüklüyse bunlardan birini seçmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="455fb-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="455fb-214">Özeti</span><span class="sxs-lookup"><span data-stu-id="455fb-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="455fb-215">Arguments</span><span class="sxs-lookup"><span data-stu-id="455fb-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="455fb-216">Güncelleştirilecek kitaplığının adı.</span><span class="sxs-lookup"><span data-stu-id="455fb-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="455fb-217">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="455fb-217">Options</span></span>

<span data-ttu-id="455fb-218">Aşağıdaki seçenekler kullanılabilir `libman update` komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="455fb-219">Kitaplığı'nın en son yayın öncesi sürümünü edinin.</span><span class="sxs-lookup"><span data-stu-id="455fb-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="455fb-220">Belirli bir kitaplığı sürümünü edinin.</span><span class="sxs-lookup"><span data-stu-id="455fb-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="455fb-221">Örnekler</span><span class="sxs-lookup"><span data-stu-id="455fb-221">Examples</span></span>

* <span data-ttu-id="455fb-222">JQuery en son sürüme güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="455fb-223">JQuery 3.3.1 sürümüne güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="455fb-224">JQuery yayın öncesi en son sürüme güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="455fb-225">Kitaplık önbelleğini yönetme</span><span class="sxs-lookup"><span data-stu-id="455fb-225">Manage library cache</span></span>

<span data-ttu-id="455fb-226">`libman cache` Komut LibMan kitaplığı önbelleğini yönetir.</span><span class="sxs-lookup"><span data-stu-id="455fb-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="455fb-227">`filesystem` Sağlayıcısı, kitaplık önbelleği kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="455fb-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="455fb-228">Özeti</span><span class="sxs-lookup"><span data-stu-id="455fb-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="455fb-229">Arguments</span><span class="sxs-lookup"><span data-stu-id="455fb-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="455fb-230">Yalnızca kullanılan `clean` komutu.</span><span class="sxs-lookup"><span data-stu-id="455fb-230">Only used with the `clean` command.</span></span> <span data-ttu-id="455fb-231">Sağlayıcısı önbelleği temizlemek için belirtir.</span><span class="sxs-lookup"><span data-stu-id="455fb-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="455fb-232">Geçerli değerler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="455fb-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="455fb-233">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="455fb-233">Options</span></span>

<span data-ttu-id="455fb-234">Aşağıdaki seçenekler kullanılabilir `libman cache` komutu:</span><span class="sxs-lookup"><span data-stu-id="455fb-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="455fb-235">Önbelleğe alınmış dosya adlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="455fb-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="455fb-236">Önbelleğe alınan kitaplıkları adlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="455fb-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="455fb-237">Örnekler</span><span class="sxs-lookup"><span data-stu-id="455fb-237">Examples</span></span>

* <span data-ttu-id="455fb-238">Önbelleğe alınan kitaplıkları sağlayıcı başına adlarını görüntülemek için aşağıdaki komutlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="455fb-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="455fb-239">Çıktı aşağıdaki gibi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="455fb-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="455fb-240">Sağlayıcı başına önbelleğe alınmış kitaplık dosyalarının adlarını görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="455fb-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="455fb-241">Çıktı aşağıdaki gibi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="455fb-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="455fb-242">Önceki çıkış bildirimi CDNJS sağlayıcı altında 3.2.1 ve 3.3.1 sürümlerini önbelleğe alınır, jQuery gösterir.</span><span class="sxs-lookup"><span data-stu-id="455fb-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="455fb-243">Kitaplık önbelleği için CDNJS sağlayıcısı boşaltmak için:</span><span class="sxs-lookup"><span data-stu-id="455fb-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="455fb-244">CDNJS sağlayıcısı önbelleği boşaltma sonra `libman cache list` komutu şunları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="455fb-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="455fb-245">Tüm desteklenen sağlayıcılar için önbelleği boşaltmak için:</span><span class="sxs-lookup"><span data-stu-id="455fb-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="455fb-246">Tüm sağlayıcısı önbellekleri boşaltma sonra `libman cache list` komutu şunları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="455fb-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="455fb-247">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="455fb-247">Additional resources</span></span>

* [<span data-ttu-id="455fb-248">Genel bir aracı yükleme</span><span class="sxs-lookup"><span data-stu-id="455fb-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="455fb-249">LibMan GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="455fb-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
