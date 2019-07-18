---
title: HTTP REPL ile Web API 'Lerini test etme
author: scottaddie
description: HTTP REPL .NET Core küresel aracının bir ASP.NET Core Web API 'sini taramak ve test etmek için nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/12/2019
uid: web-api/http-repl
ms.openlocfilehash: 1774382305cc3d479291700390807d277a24bfa7
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308349"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="ba826-103">HTTP REPL ile Web API 'Lerini test etme</span><span class="sxs-lookup"><span data-stu-id="ba826-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="ba826-104">[Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="ba826-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ba826-105">HTTP okuma-değerlendirme-yazdırma döngüsü (REPL):</span><span class="sxs-lookup"><span data-stu-id="ba826-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="ba826-106">.NET Core 'un her yerde desteklenen basit, platformlar arası bir komut satırı aracı desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ba826-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="ba826-107">ASP.NET Core Web API 'Lerini test etmek ve sonuçlarını görüntülemek için HTTP istekleri yapmak amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ba826-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="ba826-108">Aşağıdaki [http fiilleri](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) desteklenir:</span><span class="sxs-lookup"><span data-stu-id="ba826-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="ba826-109">SILMELI</span><span class="sxs-lookup"><span data-stu-id="ba826-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="ba826-110">GET</span><span class="sxs-lookup"><span data-stu-id="ba826-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="ba826-111">BAŞLI</span><span class="sxs-lookup"><span data-stu-id="ba826-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="ba826-112">SEÇENEKLER</span><span class="sxs-lookup"><span data-stu-id="ba826-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="ba826-113">DÜZELTMESI</span><span class="sxs-lookup"><span data-stu-id="ba826-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="ba826-114">POST</span><span class="sxs-lookup"><span data-stu-id="ba826-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="ba826-115">KONUR</span><span class="sxs-lookup"><span data-stu-id="ba826-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="ba826-116">Takip etmek için, [örnek ASP.NET Core Web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) 'sini ([indirme](xref:index#how-to-download-a-sample)) görüntüleyin veya indirin.</span><span class="sxs-lookup"><span data-stu-id="ba826-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba826-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ba826-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="ba826-118">Yükleme</span><span class="sxs-lookup"><span data-stu-id="ba826-118">Installation</span></span>

<span data-ttu-id="ba826-119">HTTP REPL 'u yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g dotnet-httprepl
    --version 2.2.0-*
    --add-source https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
```

<span data-ttu-id="ba826-120">[.NET Core küresel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) , [myget üzerinde barındırılan DotNet-httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet paketinden yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ba826-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [dotnet-httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet package hosted on MyGet.</span></span>

## <a name="usage"></a><span data-ttu-id="ba826-121">Kullanım</span><span class="sxs-lookup"><span data-stu-id="ba826-121">Usage</span></span>

<span data-ttu-id="ba826-122">Aracın başarıyla yüklenmesinden sonra, HTTP REPL 'u başlatmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="ba826-123">Kullanılabilir HTTP REPL komutlarını görüntülemek için aşağıdaki komutlardan birini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="ba826-124">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ba826-124">The following output is displayed:</span></span>

```console
Usage: dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  --help - Show help information.

Once the REPL starts, these commands are valid:

HTTP Commands:
Use these commands to execute requests against your application.

GET            Issues a GET request.
POST           Issues a POST request.
PUT            Issues a PUT request.
DELETE         Issues a DELETE request.
PATCH          Issues a PATCH request.
HEAD           Issues a HEAD request.
OPTIONS        Issues an OPTIONS request.

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`


Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Set the URI, relative to your base if set, of the Swagger document for this API. e.g. `set swagger /swagger/v1/swagger.json`
ls             Show all endpoints for the current path.
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`.

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell.
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands.
exit           Exit the shell.

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\Program Files\Microsoft VS Code\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line.
ui             Displays the Swagger UI page, if available, in the default browser.

Use help <COMMAND> to learn more details about individual commands. e.g. `help get`
```

<span data-ttu-id="ba826-125">HTTP REPL komut tamamlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="ba826-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="ba826-126"><kbd>Sekme</kbd> tuşuna basıldığında, yazdığınız KARAKTERLERI veya API uç noktasını tamamlayacak komutların listesi üzerinden yinelenir.</span><span class="sxs-lookup"><span data-stu-id="ba826-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="ba826-127">Aşağıdaki bölümlerde kullanılabilir CLı komutları ana hatlarıyla verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ba826-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="ba826-128">Web API 'sine bağlanma</span><span class="sxs-lookup"><span data-stu-id="ba826-128">Connect to the web API</span></span>

<span data-ttu-id="ba826-129">Aşağıdaki komutu çalıştırarak bir Web API 'sine bağlanın:</span><span class="sxs-lookup"><span data-stu-id="ba826-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="ba826-130">`<BASE URI>`, Web API 'sinin temel URI 'sidir.</span><span class="sxs-lookup"><span data-stu-id="ba826-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="ba826-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="ba826-132">Alternatif olarak, HTTP REPL çalışırken istediğiniz zaman aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="ba826-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="ba826-134">Web API 'SI için Swagger belgesine işaret edin</span><span class="sxs-lookup"><span data-stu-id="ba826-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="ba826-135">Web API 'sini düzgün bir şekilde incelemek için göreli URI 'yi Web API 'SI için Swagger belgesine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ba826-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="ba826-136">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="ba826-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="ba826-138">Web API 'sinde gezin</span><span class="sxs-lookup"><span data-stu-id="ba826-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="ba826-139">Kullanılabilir uç noktaları görüntüle</span><span class="sxs-lookup"><span data-stu-id="ba826-139">View available endpoints</span></span>

<span data-ttu-id="ba826-140">Web API adresinin geçerli yolundaki farklı uç noktaları (denetleyiciler) listelemek için, `ls` veya `dir` komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="ba826-141">Aşağıdaki çıkış biçimi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ba826-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="ba826-142">Yukarıdaki çıkış, kullanılabilir iki denetleyici olduğunu gösterir: `Fruits` ve. `People`</span><span class="sxs-lookup"><span data-stu-id="ba826-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="ba826-143">Her iki denetleyici de parametresiz HTTP GET ve POST işlemlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="ba826-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="ba826-144">Belirli bir denetleyicide gezinmek daha ayrıntılı bilgi gösterir.</span><span class="sxs-lookup"><span data-stu-id="ba826-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="ba826-145">Örneğin, aşağıdaki komut çıktısı, `Fruits` denetleyiciyi de http get, put ve DELETE işlemlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="ba826-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="ba826-146">Bu işlemlerin her biri, rotada `id` bir parametre bekler:</span><span class="sxs-lookup"><span data-stu-id="ba826-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="ba826-147">Alternatif olarak, Web `ui` API 'sinin Swagger Kullanıcı Arabirimi sayfasını bir tarayıcıda açmak için komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ba826-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="ba826-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="ba826-149">Bir uç noktaya gitme</span><span class="sxs-lookup"><span data-stu-id="ba826-149">Navigate to an endpoint</span></span>

<span data-ttu-id="ba826-150">Web API 'sindeki farklı bir uç noktaya gitmek için `cd` komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="ba826-151">`cd` Komutu izleyen yol büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="ba826-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="ba826-152">Aşağıdaki çıkış biçimi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ba826-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="ba826-153">HTTP REPL 'ı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ba826-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="ba826-154">HTTP REPL 'un varsayılan [renkleri](#set-color-preferences) özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ba826-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="ba826-155">Ayrıca, [varsayılan bir metin Düzenleyicisi](#set-the-default-text-editor) tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="ba826-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="ba826-156">HTTP REPL tercihleri geçerli oturum genelinde kalıcı hale getirilir ve gelecekteki oturumlarda kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="ba826-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="ba826-157">Değiştirildikten sonra, Tercihler aşağıdaki dosyada depolanır:</span><span class="sxs-lookup"><span data-stu-id="ba826-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ba826-158">Linux</span><span class="sxs-lookup"><span data-stu-id="ba826-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="ba826-159">*% GIRIŞ%/. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="ba826-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ba826-160">macOS</span><span class="sxs-lookup"><span data-stu-id="ba826-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="ba826-161">*% GIRIŞ%/. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="ba826-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ba826-162">Windows</span><span class="sxs-lookup"><span data-stu-id="ba826-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="ba826-163">*% USERPROFILE%\\. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="ba826-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="ba826-164">*. Httpreplprefs* dosyası başlangıçta yüklendi ve çalışma zamanında değişiklikler için izlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="ba826-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="ba826-165">Dosyada el ile yapılan değişiklikler yalnızca araç yeniden başlatıldıktan sonra devreye girer.</span><span class="sxs-lookup"><span data-stu-id="ba826-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="ba826-166">Ayarları görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="ba826-166">View the settings</span></span>

<span data-ttu-id="ba826-167">Kullanılabilir ayarları görüntülemek için `pref get` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ba826-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="ba826-168">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="ba826-169">Yukarıdaki komut, kullanılabilir anahtar-değer çiftlerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="ba826-169">The preceding command displays the available key-value pairs:</span></span>

```console
colors.json=Green
colors.json.arrayBrace=BoldCyan
colors.json.comma=BoldYellow
colors.json.name=BoldMagenta
colors.json.nameSeparator=BoldWhite
colors.json.objectBrace=Cyan
colors.protocol=BoldGreen
colors.status=BoldYellow
```

### <a name="set-color-preferences"></a><span data-ttu-id="ba826-170">Renk tercihlerini ayarla</span><span class="sxs-lookup"><span data-stu-id="ba826-170">Set color preferences</span></span>

<span data-ttu-id="ba826-171">Yanıt renklendirme Şu anda yalnızca JSON için destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="ba826-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="ba826-172">Varsayılan HTTP REPL aracı renklendirmesini özelleştirmek için, değiştirilecek renge karşılık gelen anahtarı bulun.</span><span class="sxs-lookup"><span data-stu-id="ba826-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="ba826-173">Anahtarları bulma hakkında yönergeler için bkz. [ayarları görüntüleme](#view-the-settings) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ba826-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="ba826-174">Örneğin, `colors.json` anahtar `Green` değerini şu şekilde olacak `White` şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ba826-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="ba826-175">Yalnızca [izin verilen renkler](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ba826-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="ba826-176">Sonraki HTTP istekleri, yeni renklendirmesi ile çıktıyı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ba826-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="ba826-177">Belirli renk anahtarları ayarlanmamışsa, daha genel anahtarlar kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="ba826-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="ba826-178">Bu geri dönüş davranışını göstermek için aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ba826-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="ba826-179">Eğer `colors.json.name` bir değeri yoksa, `colors.json.string` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ba826-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="ba826-180">Eğer `colors.json.string` bir değeri yoksa, `colors.json.literal` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ba826-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="ba826-181">Eğer `colors.json.literal` bir değeri yoksa, `colors.json` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ba826-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="ba826-182">Bir `colors.json` değere sahip değilse, komut kabuğun varsayılan metin rengi (`AllowedColors.None`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ba826-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="ba826-183">Girinti boyutunu ayarla</span><span class="sxs-lookup"><span data-stu-id="ba826-183">Set indentation size</span></span>

<span data-ttu-id="ba826-184">Yanıt girintileme boyut özelleştirmesi Şu anda yalnızca JSON için destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="ba826-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="ba826-185">Varsayılan boyut iki boşluklardan oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="ba826-185">The default size is two spaces.</span></span> <span data-ttu-id="ba826-186">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-186">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="ba826-187">Varsayılan boyutu değiştirmek için `formatting.json.indentSize` anahtarı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ba826-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="ba826-188">Örneğin, her zaman dört boşluk kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="ba826-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="ba826-189">Sonraki yanıtlar dört boşluk ayarına uyar:</span><span class="sxs-lookup"><span data-stu-id="ba826-189">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-indentation-size"></a><span data-ttu-id="ba826-190">Girinti boyutunu ayarla</span><span class="sxs-lookup"><span data-stu-id="ba826-190">Set indentation size</span></span>

<span data-ttu-id="ba826-191">Yanıt girintileme boyut özelleştirmesi Şu anda yalnızca JSON için destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="ba826-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="ba826-192">Varsayılan boyut iki boşluklardan oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="ba826-192">The default size is two spaces.</span></span> <span data-ttu-id="ba826-193">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-193">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="ba826-194">Varsayılan boyutu değiştirmek için `formatting.json.indentSize` anahtarı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ba826-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="ba826-195">Örneğin, her zaman dört boşluk kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="ba826-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="ba826-196">Sonraki yanıtlar dört boşluk ayarına uyar:</span><span class="sxs-lookup"><span data-stu-id="ba826-196">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-the-default-text-editor"></a><span data-ttu-id="ba826-197">Varsayılan metin düzenleyiciyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="ba826-197">Set the default text editor</span></span>

<span data-ttu-id="ba826-198">Varsayılan olarak, HTTP REPL 'un kullanılmak üzere yapılandırılmış metin Düzenleyicisi yok.</span><span class="sxs-lookup"><span data-stu-id="ba826-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="ba826-199">HTTP istek gövdesi gerektiren Web API yöntemlerini test etmek için varsayılan metin Düzenleyicisi ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ba826-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="ba826-200">HTTP REPL Aracı, istek gövdesini oluşturma amacıyla yapılandırılmış metin düzenleyicisini başlatır.</span><span class="sxs-lookup"><span data-stu-id="ba826-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="ba826-201">Tercih ettiğiniz metin düzenleyiciyi varsayılan olarak ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="ba826-202">Yukarıdaki komutta, `<EXECUTABLE>` metin düzenleyicisinin yürütülebilir dosyasının tam yoludur.</span><span class="sxs-lookup"><span data-stu-id="ba826-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="ba826-203">Örneğin, Visual Studio Code varsayılan metin düzenleyicisi olarak ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ba826-204">Linux</span><span class="sxs-lookup"><span data-stu-id="ba826-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ba826-205">macOS</span><span class="sxs-lookup"><span data-stu-id="ba826-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="ba826-206">Windows</span><span class="sxs-lookup"><span data-stu-id="ba826-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="ba826-207">Varsayılan metin düzenleyiciyi belirli CLI bağımsız değişkenleriyle başlatmak için `editor.command.default.arguments` anahtarı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ba826-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="ba826-208">Örneğin, Visual Studio Code varsayılan metin Düzenleyicisi olduğunu ve her zaman HTTP REPL 'un uzantıları devre dışı bırakılmış yeni bir oturumda Visual Studio Code açmasını istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="ba826-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="ba826-209">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="ba826-210">HTTP GET isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="ba826-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="ba826-211">Özeti</span><span class="sxs-lookup"><span data-stu-id="ba826-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="ba826-212">Arguments</span><span class="sxs-lookup"><span data-stu-id="ba826-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="ba826-213">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="ba826-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="ba826-214">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ba826-214">Options</span></span>

<span data-ttu-id="ba826-215">`get` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="ba826-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="ba826-216">Örnek</span><span class="sxs-lookup"><span data-stu-id="ba826-216">Example</span></span>

<span data-ttu-id="ba826-217">HTTP GET isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="ba826-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="ba826-218">`get` Komutu onu destekleyen bir uç noktada çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="ba826-219">Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="ba826-219">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 03:38:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "name": "Scott Hunter"
      },
      {
        "id": 2,
        "name": "Scott Hanselman"
      },
      {
        "id": 3,
        "name": "Scott Guthrie"
      }
    ]


    https://localhost:5001/people~
    ```

1. <span data-ttu-id="ba826-220">`get` Komutuna bir parametre geçirerek belirli bir kaydı alın:</span><span class="sxs-lookup"><span data-stu-id="ba826-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="ba826-221">Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="ba826-221">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 06:17:57 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 2,
        "name": "Scott Hanselman"
      }
    ]


    https://localhost:5001/people~
    ```

## <a name="test-http-post-requests"></a><span data-ttu-id="ba826-222">HTTP POST isteklerini test et</span><span class="sxs-lookup"><span data-stu-id="ba826-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="ba826-223">Özeti</span><span class="sxs-lookup"><span data-stu-id="ba826-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="ba826-224">Arguments</span><span class="sxs-lookup"><span data-stu-id="ba826-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="ba826-225">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="ba826-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="ba826-226">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ba826-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="ba826-227">Örnek</span><span class="sxs-lookup"><span data-stu-id="ba826-227">Example</span></span>

<span data-ttu-id="ba826-228">HTTP POST isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="ba826-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="ba826-229">`post` Komutu onu destekleyen bir uç noktada çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="ba826-230">Önceki komutta, `Content-Type` http istek üst bilgisi, JSON türünde bir istek gövdesi medya türünü gösterecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ba826-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="ba826-231">Varsayılan metin Düzenleyicisi, HTTP istek gövdesini temsil eden bir JSON şablonuyla bir *. tmp* dosyası açar.</span><span class="sxs-lookup"><span data-stu-id="ba826-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="ba826-232">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="ba826-233">Varsayılan metin düzenleyicisini ayarlamak için [varsayılan metin düzenleyiciyi ayarlama](#set-the-default-text-editor) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ba826-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="ba826-234">JSON şablonunu model doğrulama gereksinimlerini karşılayacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ba826-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="ba826-235">*. Tmp* dosyasını kaydedin ve metin düzenleyicisini kapatın.</span><span class="sxs-lookup"><span data-stu-id="ba826-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="ba826-236">Aşağıdaki çıktı komut kabuğu 'nda görünür:</span><span class="sxs-lookup"><span data-stu-id="ba826-236">The following output appears in the command shell:</span></span>

    ```console
    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Thu, 27 Jun 2019 21:24:18 GMT
    Location: https://localhost:5001/people/4
    Server: Kestrel
    Transfer-Encoding: chunked

    {
      "id": 4,
      "name": "Scott Addie"
    }


    https://localhost:5001/people~
    ```

## <a name="test-http-put-requests"></a><span data-ttu-id="ba826-237">HTTP PUT isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="ba826-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="ba826-238">Özeti</span><span class="sxs-lookup"><span data-stu-id="ba826-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="ba826-239">Arguments</span><span class="sxs-lookup"><span data-stu-id="ba826-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="ba826-240">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="ba826-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="ba826-241">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ba826-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="ba826-242">Örnek</span><span class="sxs-lookup"><span data-stu-id="ba826-242">Example</span></span>

<span data-ttu-id="ba826-243">HTTP PUT isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="ba826-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="ba826-244">*Isteğe bağlı*: Verileri değiştirmeden önce görüntülemek için komutunuçalıştırın:`get`</span><span class="sxs-lookup"><span data-stu-id="ba826-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `put` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ put 2 -h Content-Type=application/json
    ```

    <span data-ttu-id="ba826-245">Önceki komutta, `Content-Type` http istek üst bilgisi, JSON türünde bir istek gövdesi medya türünü gösterecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ba826-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="ba826-246">Varsayılan metin Düzenleyicisi, HTTP istek gövdesini temsil eden bir JSON şablonuyla bir *. tmp* dosyası açar.</span><span class="sxs-lookup"><span data-stu-id="ba826-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="ba826-247">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="ba826-248">Varsayılan metin düzenleyicisini ayarlamak için [varsayılan metin düzenleyiciyi ayarlama](#set-the-default-text-editor) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ba826-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="ba826-249">JSON şablonunu model doğrulama gereksinimlerini karşılayacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ba826-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="ba826-250">*. Tmp* dosyasını kaydedin ve metin düzenleyicisini kapatın.</span><span class="sxs-lookup"><span data-stu-id="ba826-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="ba826-251">Aşağıdaki çıktı komut kabuğu 'nda görünür:</span><span class="sxs-lookup"><span data-stu-id="ba826-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="ba826-252">*Isteğe bağlı*: Değişiklikleri görmek `get` için bir komut verin.</span><span class="sxs-lookup"><span data-stu-id="ba826-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="ba826-253">Örneğin, metin düzenleyicisinde "chraz" yazdıysanız, bir `get` , şunu döndürür:</span><span class="sxs-lookup"><span data-stu-id="ba826-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:08:20 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Cherry"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-delete-requests"></a><span data-ttu-id="ba826-254">HTTP SILME isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="ba826-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="ba826-255">Özeti</span><span class="sxs-lookup"><span data-stu-id="ba826-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="ba826-256">Arguments</span><span class="sxs-lookup"><span data-stu-id="ba826-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="ba826-257">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="ba826-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="ba826-258">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ba826-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="ba826-259">Örnek</span><span class="sxs-lookup"><span data-stu-id="ba826-259">Example</span></span>

<span data-ttu-id="ba826-260">HTTP SILME isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="ba826-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="ba826-261">*Isteğe bağlı*: Verileri değiştirmeden önce görüntülemek için komutunuçalıştırın:`get`</span><span class="sxs-lookup"><span data-stu-id="ba826-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `delete` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ delete 2
    ```

    <span data-ttu-id="ba826-262">Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="ba826-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="ba826-263">*Isteğe bağlı*: Değişiklikleri görmek `get` için bir komut verin.</span><span class="sxs-lookup"><span data-stu-id="ba826-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="ba826-264">Bu örnekte, bir `get` , şunu döndürür:</span><span class="sxs-lookup"><span data-stu-id="ba826-264">In this example, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:16:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-patch-requests"></a><span data-ttu-id="ba826-265">HTTP PATCH isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="ba826-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="ba826-266">Özeti</span><span class="sxs-lookup"><span data-stu-id="ba826-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="ba826-267">Arguments</span><span class="sxs-lookup"><span data-stu-id="ba826-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="ba826-268">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="ba826-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="ba826-269">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ba826-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="ba826-270">HTTP HEAD isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="ba826-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="ba826-271">Özeti</span><span class="sxs-lookup"><span data-stu-id="ba826-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="ba826-272">Arguments</span><span class="sxs-lookup"><span data-stu-id="ba826-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="ba826-273">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="ba826-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="ba826-274">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ba826-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="ba826-275">Sınama HTTP SEÇENEKLERI istekleri</span><span class="sxs-lookup"><span data-stu-id="ba826-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="ba826-276">Özeti</span><span class="sxs-lookup"><span data-stu-id="ba826-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="ba826-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="ba826-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="ba826-278">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="ba826-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="ba826-279">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="ba826-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="ba826-280">HTTP istek üst bilgilerini ayarla</span><span class="sxs-lookup"><span data-stu-id="ba826-280">Set HTTP request headers</span></span>

<span data-ttu-id="ba826-281">Bir HTTP istek üst bilgisi ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="ba826-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="ba826-282">HTTP isteğiyle satır içi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ba826-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="ba826-283">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="ba826-284">Önceki yaklaşımla, her ayrı http istek üst bilgisi kendi `-h` seçeneğini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ba826-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="ba826-285">HTTP isteğini göndermeden önce ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ba826-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="ba826-286">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="ba826-287">Bir isteği göndermeden önce üst bilgi ayarlanırken üst bilgi, komut kabuğu oturumunun süresi boyunca ayarlanmış olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="ba826-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="ba826-288">Üstbilgiyi temizlemek için boş bir değer sağlayın.</span><span class="sxs-lookup"><span data-stu-id="ba826-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="ba826-289">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="ba826-290">HTTP istek görüntüsünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="ba826-290">Toggle HTTP request display</span></span>

<span data-ttu-id="ba826-291">Varsayılan olarak, gönderilmekte olan HTTP isteğinin görüntüsü bastırılır.</span><span class="sxs-lookup"><span data-stu-id="ba826-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="ba826-292">Komut kabuğu oturumunun süresi boyunca ilgili ayarı değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ba826-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="ba826-293">İstek görüntülemesini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="ba826-293">Enable request display</span></span>

<span data-ttu-id="ba826-294">`echo on` Komutunu çalıştırarak gönderilmekte olan http isteğini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="ba826-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="ba826-295">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="ba826-296">Geçerli oturumdaki sonraki HTTP istekleri, istek üst bilgilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ba826-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="ba826-297">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-297">For example:</span></span>

```console
https://localhost:5001/people~ post

[main 2019-06-28T18:50:11.930Z] update#setState idle
Request to https://localhost:5001...

POST /people HTTP/1.1
Content-Length: 41
Content-Type: application/json
User-Agent: HTTP-REPL

{
  "id": 0,
  "name": "Scott Addie"
}

Response from https://localhost:5001...

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 28 Jun 2019 18:50:21 GMT
Location: https://localhost:5001/people/4
Server: Kestrel
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Scott Addie"
}


https://localhost:5001/people~
```

### <a name="disable-request-display"></a><span data-ttu-id="ba826-298">İstek görüntüsünü devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="ba826-298">Disable request display</span></span>

<span data-ttu-id="ba826-299">`echo off` Komutunu çalıştırarak gönderilen http isteğinin görüntülenmesini gizleyin.</span><span class="sxs-lookup"><span data-stu-id="ba826-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="ba826-300">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="ba826-301">Betik çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ba826-301">Run a script</span></span>

<span data-ttu-id="ba826-302">Aynı HTTP REPL komutları kümesini sıklıkla yürütüyorsanız bunları bir metin dosyasında depolamayı göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="ba826-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="ba826-303">Dosyadaki komutlar, komut satırında el ile çalıştıranlarla aynı formu alır.</span><span class="sxs-lookup"><span data-stu-id="ba826-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="ba826-304">Komutlar, `run` komutu kullanılarak toplanmış bir biçimde yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="ba826-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="ba826-305">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-305">For example:</span></span>

1. <span data-ttu-id="ba826-306">Yeni satır için ayrılmış komutlar kümesini içeren bir metin dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ba826-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="ba826-307">Göstermek için aşağıdaki komutları içeren bir *People-Script. txt* dosyası düşünün:</span><span class="sxs-lookup"><span data-stu-id="ba826-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="ba826-308">Metin dosyasının yolunu geçirerek komutunuyürütün.`run`</span><span class="sxs-lookup"><span data-stu-id="ba826-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="ba826-309">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ba826-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="ba826-310">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ba826-310">The following output appears:</span></span>

    ```console
    https://localhost:5001/~ set base https://localhost:5001
    Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json
    
    https://localhost:5001/~ ls
    .        []
    Fruits   [get|post]
    People   [get|post]
    
    https://localhost:5001/~ cd People
    /People    [get|post]
    
    https://localhost:5001/People~ ls
    .      [get|post]
    ..     []
    {id}   [get|put|delete]
    
    https://localhost:5001/People~ get 1
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 12 Jul 2019 19:20:10 GMT
    Server: Kestrel
    Transfer-Encoding: chunked
    
    {
      "id": 1,
      "name": "Scott Hunter"
    }
    
    
    https://localhost:5001/People~
    ```

## <a name="clear-the-output"></a><span data-ttu-id="ba826-311">Çıktıyı temizle</span><span class="sxs-lookup"><span data-stu-id="ba826-311">Clear the output</span></span>

<span data-ttu-id="ba826-312">HTTP REPL aracı tarafından komut kabuğu 'na yazılan tüm çıktıyı kaldırmak için `clear` veya `cls` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ba826-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="ba826-313">Göstermek için komut kabuğu 'nun aşağıdaki çıktıyı içerdiğini düşünün:</span><span class="sxs-lookup"><span data-stu-id="ba826-313">To illustrate, imagine the command shell contains the following output:</span></span>

```console
dotnet httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="ba826-314">Çıktıyı temizlemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ba826-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="ba826-315">Yukarıdaki komutu çalıştırdıktan sonra, komut kabuğu yalnızca şu çıktıyı içerir:</span><span class="sxs-lookup"><span data-stu-id="ba826-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="ba826-316">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ba826-316">Additional resources</span></span>

* [<span data-ttu-id="ba826-317">REST API istekleri</span><span class="sxs-lookup"><span data-stu-id="ba826-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="ba826-318">HTTP REPL GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="ba826-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
