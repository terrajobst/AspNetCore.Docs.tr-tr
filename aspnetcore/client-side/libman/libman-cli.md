---
title: ASP.NET Core ile LibMan komut satırı arabirimini (CLı) kullanın
author: scottaddie
description: ASP.NET Core projesindeki LibMan komut satırı arabirimini (CLı) kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: cf61bab2f0c3fc33d293968b8ac380cb56958d29
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080629"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="51e08-103">ASP.NET Core ile LibMan komut satırı arabirimini (CLı) kullanın</span><span class="sxs-lookup"><span data-stu-id="51e08-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="51e08-104">[Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="51e08-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="51e08-105">[Libman](xref:client-side/libman/index) CLI, .NET Core 'un desteklendiği her yerde desteklenen platformlar arası bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="51e08-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51e08-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="51e08-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="51e08-107">Yükleme</span><span class="sxs-lookup"><span data-stu-id="51e08-107">Installation</span></span>

<span data-ttu-id="51e08-108">LibMan CLı 'yı yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-108">To install the LibMan CLI:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="51e08-109">[.NET Core küresel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) , [Microsoft. Web. LibraryManager. CLI](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet paketinden yüklenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="51e08-110">Belirli bir NuGet paket kaynağından LibMan CLı 'yı yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="51e08-111">Yukarıdaki örnekte, yerel Windows makinenin *C:\Temp\Microsoft.Web.LibraryManager.cli.1.0.94-g606058a278.nupkg* dosyasından bir .NET Core genel aracı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="51e08-112">Kullanım</span><span class="sxs-lookup"><span data-stu-id="51e08-112">Usage</span></span>

<span data-ttu-id="51e08-113">CLı başarıyla yüklendikten sonra aşağıdaki komut kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="51e08-114">Yüklü CLı sürümünü görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="51e08-115">Kullanılabilir CLı komutlarını görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="51e08-116">Yukarıdaki komut aşağıdakine benzer bir çıktı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="51e08-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="51e08-117">Aşağıdaki bölümlerde kullanılabilir CLı komutları ana hatlarıyla verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="51e08-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="51e08-118">Projedeki LibMan 'ı Başlat</span><span class="sxs-lookup"><span data-stu-id="51e08-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="51e08-119">Bu `libman init` komut, yoksa bir *Libman. JSON* dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="51e08-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="51e08-120">Dosya varsayılan öğe şablonu içeriğiyle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="51e08-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="51e08-121">Özeti</span><span class="sxs-lookup"><span data-stu-id="51e08-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="51e08-122">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51e08-122">Options</span></span>

<span data-ttu-id="51e08-123">`libman init` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="51e08-124">Geçerli klasöre göreli bir yol.</span><span class="sxs-lookup"><span data-stu-id="51e08-124">A path relative to the current folder.</span></span> <span data-ttu-id="51e08-125">*Libman. JSON*içindeki bir kitaplık için hiçbir `destination` özellik tanımlanmazsa kitaplık dosyaları bu konuma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="51e08-126">Değer, *Libman. JSON* `defaultDestination` özelliğine yazılır. `<PATH>`</span><span class="sxs-lookup"><span data-stu-id="51e08-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="51e08-127">Belirli bir kitaplık için hiçbir sağlayıcı tanımlanmamışsa kullanılacak sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="51e08-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="51e08-128">Değer, *Libman. JSON* `defaultProvider` özelliğine yazılır. `<PROVIDER>`</span><span class="sxs-lookup"><span data-stu-id="51e08-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="51e08-129">Aşağıdaki `<PROVIDER>` değerlerden biriyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="51e08-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="51e08-130">Örnekler</span><span class="sxs-lookup"><span data-stu-id="51e08-130">Examples</span></span>

<span data-ttu-id="51e08-131">ASP.NET Core projesinde bir *Libman. JSON* dosyası oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="51e08-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="51e08-132">Proje köküne gidin.</span><span class="sxs-lookup"><span data-stu-id="51e08-132">Navigate to the project root.</span></span>
* <span data-ttu-id="51e08-133">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="51e08-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="51e08-134">Varsayılan sağlayıcının adını yazın veya varsayılan cdnjs sağlayıcısını `Enter` kullanmak için ' a basın.</span><span class="sxs-lookup"><span data-stu-id="51e08-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="51e08-135">Geçerli değerler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="51e08-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![Libman başlatma komutu-varsayılan sağlayıcı](_static/libman-init-provider.png)

<span data-ttu-id="51e08-137">Aşağıdaki içeriğe sahip proje köküne bir *Libman. JSON* dosyası eklenir:</span><span class="sxs-lookup"><span data-stu-id="51e08-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="51e08-138">Kitaplık dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="51e08-138">Add library files</span></span>

<span data-ttu-id="51e08-139">Komutu `libman install` , kitaplık dosyalarını indirir ve projeye yükler.</span><span class="sxs-lookup"><span data-stu-id="51e08-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="51e08-140">Bir *Libman. JSON* dosyası yoksa eklenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="51e08-141">*Libman. JSON* dosyası kitaplık dosyaları için yapılandırma ayrıntılarını depolayacak şekilde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="51e08-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="51e08-142">Özeti</span><span class="sxs-lookup"><span data-stu-id="51e08-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="51e08-143">Arguments</span><span class="sxs-lookup"><span data-stu-id="51e08-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="51e08-144">Yüklenecek kitaplığın adı.</span><span class="sxs-lookup"><span data-stu-id="51e08-144">The name of the library to install.</span></span> <span data-ttu-id="51e08-145">Bu ad, sürüm numarası gösterimi içerebilir (örneğin, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="51e08-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="51e08-146">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51e08-146">Options</span></span>

<span data-ttu-id="51e08-147">`libman install` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="51e08-148">Kitaplığın yükleneceği konum.</span><span class="sxs-lookup"><span data-stu-id="51e08-148">The location to install the library.</span></span> <span data-ttu-id="51e08-149">Belirtilmemişse, varsayılan konum kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51e08-149">If not specified, the default location is used.</span></span> <span data-ttu-id="51e08-150">`defaultDestination` *Libman. JSON*içinde hiçbir özellik belirtilmemişse, bu seçenek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="51e08-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="51e08-151">Kitaplıktan yüklenecek dosyanın adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="51e08-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="51e08-152">Belirtilmemişse, kitaplıktaki tüm dosyalar yüklenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="51e08-153">Yüklenecek dosya `--files` başına bir seçenek belirtin.</span><span class="sxs-lookup"><span data-stu-id="51e08-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="51e08-154">Göreli yollar da desteklenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-154">Relative paths are supported too.</span></span> <span data-ttu-id="51e08-155">Örneğin: `--files dist/browser/signalr.js`</span><span class="sxs-lookup"><span data-stu-id="51e08-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="51e08-156">Kitaplık alımı için kullanılacak sağlayıcının adı.</span><span class="sxs-lookup"><span data-stu-id="51e08-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="51e08-157">Aşağıdaki `<PROVIDER>` değerlerden biriyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="51e08-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="51e08-158">Belirtilmemişse, `defaultProvider` *Libman. JSON* içindeki özelliği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51e08-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="51e08-159">`defaultProvider` *Libman. JSON*içinde hiçbir özellik belirtilmemişse, bu seçenek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="51e08-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="51e08-160">Örnekler</span><span class="sxs-lookup"><span data-stu-id="51e08-160">Examples</span></span>

<span data-ttu-id="51e08-161">Aşağıdaki *Libman. JSON* dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="51e08-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="51e08-162">JQuery sürümü 3.2.1 *jQuery. min. js* dosyasını CDNJS sağlayıcısını kullanarak *Wwwroot/Scripts/jQuery* klasörüne yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="51e08-163">*Libman. JSON* dosyası şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="51e08-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="51e08-164">Dosya sistemi sağlayıcısını kullanarak *C:\\temp\\\\ contosocalendar* konumundaki *Calendar. js* ve *Calendar. css* dosyalarını yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="51e08-165">Aşağıdaki istem iki nedenden dolayı görünür:</span><span class="sxs-lookup"><span data-stu-id="51e08-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="51e08-166">*Libman. JSON* dosyası bir `defaultDestination` özellik içermiyor.</span><span class="sxs-lookup"><span data-stu-id="51e08-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="51e08-167">Komut, `-d|--destination` seçeneğini içermez. `libman install`</span><span class="sxs-lookup"><span data-stu-id="51e08-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![Libman Install komut-hedef](_static/libman-install-destination.png)

<span data-ttu-id="51e08-169">Varsayılan hedefi kabul ettikten sonra, *Libman. JSON* dosyası şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="51e08-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="51e08-170">Kitaplık dosyalarını geri yükleme</span><span class="sxs-lookup"><span data-stu-id="51e08-170">Restore library files</span></span>

<span data-ttu-id="51e08-171">Komut `libman restore` , *Libman. JSON*içinde tanımlanan kitaplık dosyalarını yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="51e08-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="51e08-172">Aşağıdaki kurallar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="51e08-172">The following rules apply:</span></span>

* <span data-ttu-id="51e08-173">Proje kökünde bir *Libman. JSON* dosyası yoksa bir hata döndürülür.</span><span class="sxs-lookup"><span data-stu-id="51e08-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="51e08-174">Bir kitaplık bir sağlayıcıyı belirtiyorsa, `defaultProvider` *Libman. JSON* içindeki özelliği yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="51e08-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="51e08-175">Bir kitaplık bir hedef belirtiyorsa, `defaultDestination` *Libman. JSON* içindeki özelliği yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="51e08-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="51e08-176">Özeti</span><span class="sxs-lookup"><span data-stu-id="51e08-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="51e08-177">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51e08-177">Options</span></span>

<span data-ttu-id="51e08-178">`libman restore` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="51e08-179">Örnekler</span><span class="sxs-lookup"><span data-stu-id="51e08-179">Examples</span></span>

<span data-ttu-id="51e08-180">*Libman. JSON*' da tanımlanan kitaplık dosyalarını geri yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="51e08-181">Kitaplık dosyalarını sil</span><span class="sxs-lookup"><span data-stu-id="51e08-181">Delete library files</span></span>

<span data-ttu-id="51e08-182">`libman clean` Komut daha önce Libman aracılığıyla geri yüklenen kitaplık dosyalarını siler.</span><span class="sxs-lookup"><span data-stu-id="51e08-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="51e08-183">Bu işlemden sonra boş olacak klasörler silinir.</span><span class="sxs-lookup"><span data-stu-id="51e08-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="51e08-184">`libraries` *Libman. JSON* özelliğindeki kitaplık dosyalarının ilişkili yapılandırması kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="51e08-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="51e08-185">Özeti</span><span class="sxs-lookup"><span data-stu-id="51e08-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="51e08-186">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51e08-186">Options</span></span>

<span data-ttu-id="51e08-187">`libman clean` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="51e08-188">Örnekler</span><span class="sxs-lookup"><span data-stu-id="51e08-188">Examples</span></span>

<span data-ttu-id="51e08-189">LibMan aracılığıyla yüklenen kitaplık dosyalarını silmek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="51e08-190">Kitaplık dosyalarını kaldır</span><span class="sxs-lookup"><span data-stu-id="51e08-190">Uninstall library files</span></span>

<span data-ttu-id="51e08-191">`libman uninstall` Komut:</span><span class="sxs-lookup"><span data-stu-id="51e08-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="51e08-192">Belirtilen kitaplıkla ilişkili tüm dosyaları *Libman. JSON*dosyasındaki hedefle siler.</span><span class="sxs-lookup"><span data-stu-id="51e08-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="51e08-193">*Libman. JSON*' dan ilişkili kitaplık yapılandırmasını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="51e08-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="51e08-194">Şu durumlarda bir hata oluşur:</span><span class="sxs-lookup"><span data-stu-id="51e08-194">An error occurs when:</span></span>

* <span data-ttu-id="51e08-195">Proje kökünde *Libman. JSON* dosyası yok.</span><span class="sxs-lookup"><span data-stu-id="51e08-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="51e08-196">Belirtilen kitaplık yok.</span><span class="sxs-lookup"><span data-stu-id="51e08-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="51e08-197">Aynı ada sahip birden fazla kitaplık yüklüyse, bir tane seçmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="51e08-198">Özeti</span><span class="sxs-lookup"><span data-stu-id="51e08-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="51e08-199">Arguments</span><span class="sxs-lookup"><span data-stu-id="51e08-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="51e08-200">Kaldırılacak kitaplığın adı.</span><span class="sxs-lookup"><span data-stu-id="51e08-200">The name of the library to uninstall.</span></span> <span data-ttu-id="51e08-201">Bu ad, sürüm numarası gösterimi içerebilir (örneğin, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="51e08-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="51e08-202">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51e08-202">Options</span></span>

<span data-ttu-id="51e08-203">`libman uninstall` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="51e08-204">Örnekler</span><span class="sxs-lookup"><span data-stu-id="51e08-204">Examples</span></span>

<span data-ttu-id="51e08-205">Aşağıdaki *Libman. JSON* dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="51e08-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="51e08-206">JQuery 'i kaldırmak için aşağıdaki komutlardan biri başarılı olur:</span><span class="sxs-lookup"><span data-stu-id="51e08-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="51e08-207">`filesystem` Sağlayıcı aracılığıyla yüklenen lodash dosyalarını kaldırmak için:</span><span class="sxs-lookup"><span data-stu-id="51e08-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="51e08-208">Kitaplık sürümünü Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="51e08-208">Update library version</span></span>

<span data-ttu-id="51e08-209">`libman update` Komutu Libman aracılığıyla yüklenen bir kitaplığı belirtilen sürüme güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="51e08-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="51e08-210">Şu durumlarda bir hata oluşur:</span><span class="sxs-lookup"><span data-stu-id="51e08-210">An error occurs when:</span></span>

* <span data-ttu-id="51e08-211">Proje kökünde *Libman. JSON* dosyası yok.</span><span class="sxs-lookup"><span data-stu-id="51e08-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="51e08-212">Belirtilen kitaplık yok.</span><span class="sxs-lookup"><span data-stu-id="51e08-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="51e08-213">Aynı ada sahip birden fazla kitaplık yüklüyse, bir tane seçmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="51e08-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="51e08-214">Özeti</span><span class="sxs-lookup"><span data-stu-id="51e08-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="51e08-215">Arguments</span><span class="sxs-lookup"><span data-stu-id="51e08-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="51e08-216">Güncelleştirilecek kitaplığın adı.</span><span class="sxs-lookup"><span data-stu-id="51e08-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="51e08-217">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51e08-217">Options</span></span>

<span data-ttu-id="51e08-218">`libman update` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="51e08-219">Kitaplığın en son yayın öncesi sürümünü edinin.</span><span class="sxs-lookup"><span data-stu-id="51e08-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="51e08-220">Kitaplığın belirli bir sürümünü edinin.</span><span class="sxs-lookup"><span data-stu-id="51e08-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="51e08-221">Örnekler</span><span class="sxs-lookup"><span data-stu-id="51e08-221">Examples</span></span>

* <span data-ttu-id="51e08-222">JQuery 'i en son sürüme güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="51e08-223">JQuery 'i 3.3.1 sürümüne güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="51e08-224">JQuery 'i en son ön sürüm sürümüne güncelleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="51e08-225">Kitaplık önbelleğini Yönet</span><span class="sxs-lookup"><span data-stu-id="51e08-225">Manage library cache</span></span>

<span data-ttu-id="51e08-226">Komut `libman cache` , Libman kitaplık önbelleğini yönetir.</span><span class="sxs-lookup"><span data-stu-id="51e08-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="51e08-227">`filesystem` Sağlayıcı kitaplık önbelleğini kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="51e08-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="51e08-228">Özeti</span><span class="sxs-lookup"><span data-stu-id="51e08-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="51e08-229">Arguments</span><span class="sxs-lookup"><span data-stu-id="51e08-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="51e08-230">Yalnızca `clean` komutla birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51e08-230">Only used with the `clean` command.</span></span> <span data-ttu-id="51e08-231">Temizleyen sağlayıcı önbelleğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="51e08-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="51e08-232">Geçerli değerler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="51e08-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="51e08-233">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51e08-233">Options</span></span>

<span data-ttu-id="51e08-234">`libman cache` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51e08-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="51e08-235">Önbelleğe alınan dosyaların adlarını listeleyin.</span><span class="sxs-lookup"><span data-stu-id="51e08-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="51e08-236">Önbelleğe alınan kitaplıkların adlarını listeleyin.</span><span class="sxs-lookup"><span data-stu-id="51e08-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="51e08-237">Örnekler</span><span class="sxs-lookup"><span data-stu-id="51e08-237">Examples</span></span>

* <span data-ttu-id="51e08-238">Sağlayıcı başına önbelleğe alınmış kitaplıkların adlarını görüntülemek için aşağıdaki komutlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="51e08-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="51e08-239">Aşağıdakine benzer bir çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="51e08-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="51e08-240">Sağlayıcı başına önbelleğe alınmış kitaplık dosyalarının adlarını görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="51e08-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="51e08-241">Aşağıdakine benzer bir çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="51e08-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="51e08-242">Yukarıdaki çıktıda, 3.2.1 ve 3.3.1 jQuery sürümlerinin CDNJS sağlayıcısı altında önbelleğe alındığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="51e08-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="51e08-243">CDNJS sağlayıcısı için kitaplık önbelleğini boşaltmak için:</span><span class="sxs-lookup"><span data-stu-id="51e08-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="51e08-244">Cdnjs sağlayıcı önbelleğini `libman cache list` boşaltdıktan sonra komut şunları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="51e08-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="51e08-245">Tüm desteklenen sağlayıcıların önbelleğini boşaltmak için:</span><span class="sxs-lookup"><span data-stu-id="51e08-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="51e08-246">Tüm sağlayıcı önbellekleri `libman cache list` boşaltıldıktan sonra komut şunları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="51e08-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="51e08-247">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="51e08-247">Additional resources</span></span>

* [<span data-ttu-id="51e08-248">Küresel bir araç yükler</span><span class="sxs-lookup"><span data-stu-id="51e08-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="51e08-249">LibMan GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="51e08-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
