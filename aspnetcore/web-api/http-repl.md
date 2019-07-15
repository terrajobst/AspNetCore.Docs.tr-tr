---
title: Web API HTTP REPL ile test
author: scottaddie
description: HTTP REPL .NET Core genel aracı göz atın ve bir ASP.NET Core web API'sini test etmek için kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/12/2019
uid: web-api/http-repl
ms.openlocfilehash: 920561fc86ed90eef64af49fa5936a080a096de2
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67919263"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="bfab8-103">Web API HTTP REPL ile test</span><span class="sxs-lookup"><span data-stu-id="bfab8-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="bfab8-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="bfab8-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="bfab8-105">HTTP okuma-Eval-Print Loop (REPL) aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="bfab8-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="bfab8-106">.NET Core her yerde desteklenen basit, platformlar arası komut satırı aracı desteklenir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="bfab8-107">ASP.NET Core web API'ları sınamak ve sonuçları görüntülemek için HTTP isteği yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="bfab8-108">Aşağıdaki [HTTP fiilleri](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) desteklenir:</span><span class="sxs-lookup"><span data-stu-id="bfab8-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="bfab8-109">DELETE</span><span class="sxs-lookup"><span data-stu-id="bfab8-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="bfab8-110">GET</span><span class="sxs-lookup"><span data-stu-id="bfab8-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="bfab8-111">HEAD</span><span class="sxs-lookup"><span data-stu-id="bfab8-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="bfab8-112">SEÇENEKLER</span><span class="sxs-lookup"><span data-stu-id="bfab8-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="bfab8-113">DÜZELTME EKİ</span><span class="sxs-lookup"><span data-stu-id="bfab8-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="bfab8-114">POST</span><span class="sxs-lookup"><span data-stu-id="bfab8-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="bfab8-115">PUT</span><span class="sxs-lookup"><span data-stu-id="bfab8-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="bfab8-116">Örneği takip etmek için [görüntüleme veya indirme örnek ASP.NET Core web API'si](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="bfab8-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bfab8-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="bfab8-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="bfab8-118">Yükleme</span><span class="sxs-lookup"><span data-stu-id="bfab8-118">Installation</span></span>

<span data-ttu-id="bfab8-119">HTTP REPL yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g dotnet-httprepl
    --version 2.2.0-*
    --add-source https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
```

<span data-ttu-id="bfab8-120">A [.NET Core genel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) gelen yüklü [dotnet httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet paketini Myget'te barındırılan.</span><span class="sxs-lookup"><span data-stu-id="bfab8-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [dotnet-httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet package hosted on MyGet.</span></span>

## <a name="usage"></a><span data-ttu-id="bfab8-121">Kullanım</span><span class="sxs-lookup"><span data-stu-id="bfab8-121">Usage</span></span>

<span data-ttu-id="bfab8-122">Aracı başarıyla yüklendikten sonra HTTP REPL başlatmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="bfab8-123">Kullanılabilir tüm HTTP REPL komutları görmek için aşağıdaki komutlardan birini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="bfab8-124">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="bfab8-124">The following output is displayed:</span></span>

```console
Usage: dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  --help - Show help information.

REPL Commands:

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

<span data-ttu-id="bfab8-125">HTTP REPL komut tamamlama sunar.</span><span class="sxs-lookup"><span data-stu-id="bfab8-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="bfab8-126">Tuşuna basarak <kbd>sekmesini</kbd> anahtar tam karakter veya yazdığınız API uç noktası komutları listesi üzerinden yinelenir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="bfab8-127">Aşağıdaki bölümlerde kullanılabilir CLI komutları özetler.</span><span class="sxs-lookup"><span data-stu-id="bfab8-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="bfab8-128">Web API'sine bağlanma</span><span class="sxs-lookup"><span data-stu-id="bfab8-128">Connect to the web API</span></span>

<span data-ttu-id="bfab8-129">Aşağıdaki komutu çalıştırarak bir web API'sine bağlanma:</span><span class="sxs-lookup"><span data-stu-id="bfab8-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="bfab8-130">`<BASE URI>` web API'si için temel URI'dir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="bfab8-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="bfab8-132">Alternatif olarak, HTTP REPL çalışırken herhangi bir zamanda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="bfab8-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="bfab8-134">Web API'si için Swagger belgesinin noktası</span><span class="sxs-lookup"><span data-stu-id="bfab8-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="bfab8-135">Web API'si düzgün bir şekilde incelemek için web API'si için Swagger belgesinin göreli URI ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bfab8-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="bfab8-136">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="bfab8-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="bfab8-138">Web API'si gidin</span><span class="sxs-lookup"><span data-stu-id="bfab8-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="bfab8-139">Kullanılabilir uç noktalarını görüntüle</span><span class="sxs-lookup"><span data-stu-id="bfab8-139">View available endpoints</span></span>

<span data-ttu-id="bfab8-140">Web API adresi geçerli yolda farklı uç noktalar (denetleyiciler) listelemek için çalıştırma `ls` veya `dir` komutu:</span><span class="sxs-lookup"><span data-stu-id="bfab8-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="bfab8-141">Aşağıdaki çıktı biçimi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="bfab8-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="bfab8-142">Önceki çıkış iki denetleyici kullanılabilir olduğunu gösterir: `Fruits` ve `People`.</span><span class="sxs-lookup"><span data-stu-id="bfab8-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="bfab8-143">Her iki denetleyicilerinin parametresiz bir HTTP GET ve POST işlemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="bfab8-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="bfab8-144">Belirli bir Denetleyicisi'nde giderek daha fazla ayrıntı ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="bfab8-145">Örneğin, aşağıdaki komutu gösterir çıkışını `Fruits` denetleyicisi Ayrıca HTTP GET, PUT ve DELETE işlemlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="bfab8-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="bfab8-146">Bu işlemlerden her biriyle bekliyor bir `id` rota parametresi:</span><span class="sxs-lookup"><span data-stu-id="bfab8-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="bfab8-147">Alternatif olarak, çalıştırma `ui` web API'SİNİN Swagger kullanıcı arabirimini sayfasını bir tarayıcıda açmak için komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="bfab8-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="bfab8-149">Bir uç noktasına gidin</span><span class="sxs-lookup"><span data-stu-id="bfab8-149">Navigate to an endpoint</span></span>

<span data-ttu-id="bfab8-150">Web API üzerinde farklı bir uç noktasına gitmek için çalıştırma `cd` komutu:</span><span class="sxs-lookup"><span data-stu-id="bfab8-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="bfab8-151">Yol aşağıdaki `cd` komutu büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="bfab8-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="bfab8-152">Aşağıdaki çıktı biçimi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="bfab8-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="bfab8-153">HTTP REPL özelleştirme</span><span class="sxs-lookup"><span data-stu-id="bfab8-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="bfab8-154">HTTP REPL varsayılan [renkleri](#set-color-preferences) özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="bfab8-155">Ayrıca, bir [varsayılan metin düzenleyici](#set-the-default-text-editor) tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="bfab8-156">HTTP REPL tercihleri geçerli oturumu arasında kalıcıdır ve gelecek oturumlar yerine getirilmez.</span><span class="sxs-lookup"><span data-stu-id="bfab8-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="bfab8-157">Değiştirilen sonra aşağıdaki dosya tercihleri depolanır:</span><span class="sxs-lookup"><span data-stu-id="bfab8-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="bfab8-158">Linux</span><span class="sxs-lookup"><span data-stu-id="bfab8-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="bfab8-159">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="bfab8-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="bfab8-160">macOS</span><span class="sxs-lookup"><span data-stu-id="bfab8-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="bfab8-161">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="bfab8-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bfab8-162">Windows</span><span class="sxs-lookup"><span data-stu-id="bfab8-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="bfab8-163">*% USERPROFILE %\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="bfab8-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="bfab8-164">*.Httpreplprefs* dosya başlangıçta yüklendi ve değişikliklerin çalışma zamanında izlenen değil.</span><span class="sxs-lookup"><span data-stu-id="bfab8-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="bfab8-165">Dosyayı el ile yapılan değişiklikler yalnızca bir aracı yeniden başlatıldıktan sonra etkili olur.</span><span class="sxs-lookup"><span data-stu-id="bfab8-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="bfab8-166">Ayarları görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="bfab8-166">View the settings</span></span>

<span data-ttu-id="bfab8-167">Kullanılabilir ayarları görüntülemek için çalıştırın `pref get` komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="bfab8-168">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="bfab8-169">Önceki komutta, mevcut anahtar-değer çiftleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="bfab8-169">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="bfab8-170">Rengi tercihlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="bfab8-170">Set color preferences</span></span>

<span data-ttu-id="bfab8-171">Yanıt renklendirme şu anda yalnızca JSON için desteklenir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="bfab8-172">Renklendirme varsayılan HTTP REPL aracı özelleştirmek için anahtar karşılık gelen rengi değiştirilecek bulun.</span><span class="sxs-lookup"><span data-stu-id="bfab8-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="bfab8-173">Anahtarların bulma hakkında yönergeler için bkz: [ayarları görüntüleyin](#view-the-settings) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bfab8-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="bfab8-174">Örneğin, değiştirme `colors.json` anahtar değerinden `Green` için `White` gibi:</span><span class="sxs-lookup"><span data-stu-id="bfab8-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="bfab8-175">Yalnızca [renkleri izin](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="bfab8-176">Sonraki HTTP yeni renklendirme ile görüntü çıkış ister.</span><span class="sxs-lookup"><span data-stu-id="bfab8-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="bfab8-177">Belirli renk anahtarları olmayan ayarlarken daha genel anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="bfab8-178">Bu geri dönüş davranışını göstermek için aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="bfab8-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="bfab8-179">Varsa `colors.json.name` bir değere sahip `colors.json.string` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="bfab8-180">Varsa `colors.json.string` bir değere sahip `colors.json.literal` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="bfab8-181">Varsa `colors.json.literal` bir değere sahip `colors.json` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="bfab8-182">Varsa `colors.json` komut kabuğu'nın varsayılan metin rengi bir değere sahip değil (`AllowedColors.None`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="bfab8-183">Girinti boyutunu Ayarla</span><span class="sxs-lookup"><span data-stu-id="bfab8-183">Set indentation size</span></span>

<span data-ttu-id="bfab8-184">Yanıt girinti boyutu özelleştirmesi şu anda yalnızca JSON için desteklenir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="bfab8-185">Varsayılan iki boşluk boyutudur.</span><span class="sxs-lookup"><span data-stu-id="bfab8-185">The default size is two spaces.</span></span> <span data-ttu-id="bfab8-186">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-186">For example:</span></span>

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

<span data-ttu-id="bfab8-187">Varsayılan boyutu değiştirmek için Ayarla `formatting.json.indentSize` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="bfab8-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="bfab8-188">Örneğin, her zaman dört alanları kullanın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="bfab8-189">Dört alan ayarını yanıtlar sonraki dikkate:</span><span class="sxs-lookup"><span data-stu-id="bfab8-189">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="bfab8-190">Girinti boyutunu Ayarla</span><span class="sxs-lookup"><span data-stu-id="bfab8-190">Set indentation size</span></span>

<span data-ttu-id="bfab8-191">Yanıt girinti boyutu özelleştirmesi şu anda yalnızca JSON için desteklenir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="bfab8-192">Varsayılan iki boşluk boyutudur.</span><span class="sxs-lookup"><span data-stu-id="bfab8-192">The default size is two spaces.</span></span> <span data-ttu-id="bfab8-193">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-193">For example:</span></span>

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

<span data-ttu-id="bfab8-194">Varsayılan boyutu değiştirmek için Ayarla `formatting.json.indentSize` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="bfab8-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="bfab8-195">Örneğin, her zaman dört alanları kullanın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="bfab8-196">Dört alan ayarını yanıtlar sonraki dikkate:</span><span class="sxs-lookup"><span data-stu-id="bfab8-196">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="bfab8-197">Varsayılan metin Düzenleyicisi'ni ayarlayın</span><span class="sxs-lookup"><span data-stu-id="bfab8-197">Set the default text editor</span></span>

<span data-ttu-id="bfab8-198">Varsayılan olarak, HTTP REPL kullanılmak üzere yapılandırılmış hiçbir metin düzenleyicisi içerir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="bfab8-199">Bir HTTP isteği gövdesinin gerektiren web API yöntemleri test etmek için bir varsayılan metin düzenleyicisi ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bfab8-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="bfab8-200">HTTP REPL aracı yapılandırılmış metin düzenleyici tek amacı, istek gövdesi oluşturma işlemi başlatır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="bfab8-201">Varsayılan olarak, tercih edilen metin düzenleyiciyi ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="bfab8-202">Önceki komutta `<EXECUTABLE>` Metin Düzenleyicisi'nin yürütülebilir dosyasının tam yoludur.</span><span class="sxs-lookup"><span data-stu-id="bfab8-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="bfab8-203">Örneğin, varsayılan metin düzenleyici olarak Visual Studio Code ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="bfab8-204">Linux</span><span class="sxs-lookup"><span data-stu-id="bfab8-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="bfab8-205">macOS</span><span class="sxs-lookup"><span data-stu-id="bfab8-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="bfab8-206">Windows</span><span class="sxs-lookup"><span data-stu-id="bfab8-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="bfab8-207">Belirli CLI bağımsız değişkenlerle varsayılan metin düzenleyiciyi başlatmak için ayarlanmış `editor.command.default.arguments` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="bfab8-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="bfab8-208">Örneğin, varsayılan metin düzenleyicisi Visual Studio Code, ve her zaman Visual Studio Code uzantıları devre dışı yeni bir oturum açmak için HTTP REPL istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="bfab8-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="bfab8-209">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="bfab8-210">HTTP GET isteklerini test</span><span class="sxs-lookup"><span data-stu-id="bfab8-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="bfab8-211">Synopsis</span><span class="sxs-lookup"><span data-stu-id="bfab8-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="bfab8-212">Arguments</span><span class="sxs-lookup"><span data-stu-id="bfab8-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="bfab8-213">Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="bfab8-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="bfab8-214">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bfab8-214">Options</span></span>

<span data-ttu-id="bfab8-215">Aşağıdaki seçenekler kullanılabilir `get` komutu:</span><span class="sxs-lookup"><span data-stu-id="bfab8-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="bfab8-216">Örnek</span><span class="sxs-lookup"><span data-stu-id="bfab8-216">Example</span></span>

<span data-ttu-id="bfab8-217">Bir HTTP GET isteği göndermektir almak için:</span><span class="sxs-lookup"><span data-stu-id="bfab8-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="bfab8-218">Çalıştırma `get` desteklediği uç noktasında komutu:</span><span class="sxs-lookup"><span data-stu-id="bfab8-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="bfab8-219">Yukarıdaki komut, aşağıdaki çıkış biçimi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="bfab8-219">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="bfab8-220">Belirli bir kaydı için bir parametre geçirerek almak `get` komutu:</span><span class="sxs-lookup"><span data-stu-id="bfab8-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="bfab8-221">Yukarıdaki komut, aşağıdaki çıkış biçimi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="bfab8-221">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="bfab8-222">Testi HTTP POST istekleri</span><span class="sxs-lookup"><span data-stu-id="bfab8-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="bfab8-223">Synopsis</span><span class="sxs-lookup"><span data-stu-id="bfab8-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="bfab8-224">Arguments</span><span class="sxs-lookup"><span data-stu-id="bfab8-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="bfab8-225">Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="bfab8-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="bfab8-226">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bfab8-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="bfab8-227">Örnek</span><span class="sxs-lookup"><span data-stu-id="bfab8-227">Example</span></span>

<span data-ttu-id="bfab8-228">Bir HTTP POST isteği göndermektir almak için:</span><span class="sxs-lookup"><span data-stu-id="bfab8-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="bfab8-229">Çalıştırma `post` desteklediği uç noktasında komutu:</span><span class="sxs-lookup"><span data-stu-id="bfab8-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="bfab8-230">Önceki komutta `Content-Type` HTTP isteği üst bilgisi, JSON isteği gövdesi medya türünü belirtmek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="bfab8-231">Varsayılan metin düzenleyicisi açılır bir *.tmp* HTTP isteği gövdesinin temsil eden bir JSON şablon dosyası.</span><span class="sxs-lookup"><span data-stu-id="bfab8-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="bfab8-232">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="bfab8-233">Varsayılan metin Düzenleyicisi'ni ayarlamak için bkz [kümesi varsayılan metin düzenleyici](#set-the-default-text-editor) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bfab8-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="bfab8-234">Model doğrulama gereksinimlerini karşılamak için JSON şablonunu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="bfab8-235">Kaydet *.tmp* dosya ve Metin Düzenleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="bfab8-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="bfab8-236">Komut kabuğu'nda aşağıdaki çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="bfab8-236">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="bfab8-237">HTTP PUT isteklerini test</span><span class="sxs-lookup"><span data-stu-id="bfab8-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="bfab8-238">Synopsis</span><span class="sxs-lookup"><span data-stu-id="bfab8-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="bfab8-239">Arguments</span><span class="sxs-lookup"><span data-stu-id="bfab8-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="bfab8-240">Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="bfab8-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="bfab8-241">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bfab8-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="bfab8-242">Örnek</span><span class="sxs-lookup"><span data-stu-id="bfab8-242">Example</span></span>

<span data-ttu-id="bfab8-243">Bir HTTP PUT isteği göndermek için:</span><span class="sxs-lookup"><span data-stu-id="bfab8-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="bfab8-244">*İsteğe bağlı*: Çalıştırma `get` değiştirmeden önce verileri görüntülemek için komut:</span><span class="sxs-lookup"><span data-stu-id="bfab8-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="bfab8-245">Önceki komutta `Content-Type` HTTP isteği üst bilgisi, JSON isteği gövdesi medya türünü belirtmek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="bfab8-246">Varsayılan metin düzenleyicisi açılır bir *.tmp* HTTP isteği gövdesinin temsil eden bir JSON şablon dosyası.</span><span class="sxs-lookup"><span data-stu-id="bfab8-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="bfab8-247">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="bfab8-248">Varsayılan metin Düzenleyicisi'ni ayarlamak için bkz [kümesi varsayılan metin düzenleyici](#set-the-default-text-editor) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bfab8-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="bfab8-249">Model doğrulama gereksinimlerini karşılamak için JSON şablonunu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="bfab8-250">Kaydet *.tmp* dosya ve Metin Düzenleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="bfab8-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="bfab8-251">Komut kabuğu'nda aşağıdaki çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="bfab8-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="bfab8-252">*İsteğe bağlı*: Sorun bir `get` değişiklikleri görmek için komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="bfab8-253">Örneğin, Metin Düzenleyici'de "Tek tek seçme" yazılı bir `get` aşağıdaki döndürür:</span><span class="sxs-lookup"><span data-stu-id="bfab8-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="bfab8-254">HTTP DELETE isteklerini test</span><span class="sxs-lookup"><span data-stu-id="bfab8-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="bfab8-255">Synopsis</span><span class="sxs-lookup"><span data-stu-id="bfab8-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="bfab8-256">Arguments</span><span class="sxs-lookup"><span data-stu-id="bfab8-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="bfab8-257">Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="bfab8-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="bfab8-258">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bfab8-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="bfab8-259">Örnek</span><span class="sxs-lookup"><span data-stu-id="bfab8-259">Example</span></span>

<span data-ttu-id="bfab8-260">Bir HTTP DELETE isteği göndermek için:</span><span class="sxs-lookup"><span data-stu-id="bfab8-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="bfab8-261">*İsteğe bağlı*: Çalıştırma `get` değiştirmeden önce verileri görüntülemek için komut:</span><span class="sxs-lookup"><span data-stu-id="bfab8-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="bfab8-262">Yukarıdaki komut, aşağıdaki çıkış biçimi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="bfab8-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="bfab8-263">*İsteğe bağlı*: Sorun bir `get` değişiklikleri görmek için komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="bfab8-264">Bu örnekte, bir `get` aşağıdaki döndürür:</span><span class="sxs-lookup"><span data-stu-id="bfab8-264">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="bfab8-265">HTTP PATCH isteği test</span><span class="sxs-lookup"><span data-stu-id="bfab8-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="bfab8-266">Synopsis</span><span class="sxs-lookup"><span data-stu-id="bfab8-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="bfab8-267">Arguments</span><span class="sxs-lookup"><span data-stu-id="bfab8-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="bfab8-268">Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="bfab8-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="bfab8-269">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bfab8-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="bfab8-270">HTTP HEAD isteklerini test</span><span class="sxs-lookup"><span data-stu-id="bfab8-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="bfab8-271">Synopsis</span><span class="sxs-lookup"><span data-stu-id="bfab8-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="bfab8-272">Arguments</span><span class="sxs-lookup"><span data-stu-id="bfab8-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="bfab8-273">Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="bfab8-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="bfab8-274">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bfab8-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="bfab8-275">HTTP OPTIONS istekleri test</span><span class="sxs-lookup"><span data-stu-id="bfab8-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="bfab8-276">Synopsis</span><span class="sxs-lookup"><span data-stu-id="bfab8-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="bfab8-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="bfab8-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="bfab8-278">Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="bfab8-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="bfab8-279">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bfab8-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="bfab8-280">HTTP istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="bfab8-280">Set HTTP request headers</span></span>

<span data-ttu-id="bfab8-281">Bir HTTP isteği üstbilgisini ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="bfab8-282">Satır içi HTTP isteği ile ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bfab8-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="bfab8-283">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="bfab8-284">Önceki yaklaşımda her ayrı HTTP isteği üstbilgisini kendi gerektirir `-h` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="bfab8-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="bfab8-285">HTTP isteği göndermeden önce ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bfab8-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="bfab8-286">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="bfab8-287">Üst bilgi isteği göndermeden önce ayarlarken, üst bilgi komut kabuk oturumu süresi için ayarlanmış olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="bfab8-288">Üst bilgi temizlemek için boş bir değer sağlayın.</span><span class="sxs-lookup"><span data-stu-id="bfab8-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="bfab8-289">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="bfab8-290">HTTP isteği görüntüleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="bfab8-290">Toggle HTTP request display</span></span>

<span data-ttu-id="bfab8-291">Varsayılan olarak, görüntü gönderilen HTTP isteğinin bastırılır.</span><span class="sxs-lookup"><span data-stu-id="bfab8-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="bfab8-292">Komut kabuk oturumu süresi için karşılık gelen ayarı değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="bfab8-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="bfab8-293">Etkinleştirme isteği görüntüleme</span><span class="sxs-lookup"><span data-stu-id="bfab8-293">Enable request display</span></span>

<span data-ttu-id="bfab8-294">Çalıştırarak gönderilmekte olan HTTP isteğini `echo on` komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="bfab8-295">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="bfab8-296">Geçerli oturumda sonraki HTTP istekleri, istek üst bilgilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="bfab8-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="bfab8-297">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-297">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="bfab8-298">İstek ekran devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="bfab8-298">Disable request display</span></span>

<span data-ttu-id="bfab8-299">Çalışan tarafından gönderilen HTTP isteğinin bastır `echo off` komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="bfab8-300">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="bfab8-301">Betik çalıştırma</span><span class="sxs-lookup"><span data-stu-id="bfab8-301">Run a script</span></span>

<span data-ttu-id="bfab8-302">Sık aynı HTTP REPL komutları kümesi yürütün, bir metin dosyasına saklamadan göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="bfab8-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="bfab8-303">Bu komut satırında el ile yürütülen aynı biçime dosyasındaki komutlar yararlanın.</span><span class="sxs-lookup"><span data-stu-id="bfab8-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="bfab8-304">Komutları kullanarak bir toplu şekilde yürütülebilir `run` komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="bfab8-305">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-305">For example:</span></span>

1. <span data-ttu-id="bfab8-306">Yeni satır ayrılmış komut kümesini içeren bir metin dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bfab8-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="bfab8-307">Göstermek için göz önünde bir *kişiler script.txt* aşağıdaki komutları içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="bfab8-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="bfab8-308">Yürütme `run` komutu, metin dosyasının yolu geçirme.</span><span class="sxs-lookup"><span data-stu-id="bfab8-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="bfab8-309">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bfab8-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="bfab8-310">Aşağıdaki çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="bfab8-310">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="bfab8-311">Çıkışı Temizle</span><span class="sxs-lookup"><span data-stu-id="bfab8-311">Clear the output</span></span>

<span data-ttu-id="bfab8-312">Komut kabuğu için HTTP REPL araç tarafından yazılan tüm çıkış kaldırmak için çalıştırın `clear` veya `cls` komutu.</span><span class="sxs-lookup"><span data-stu-id="bfab8-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="bfab8-313">Göstermek için aşağıdaki çıktıyı Komut kabuğunu içerir düşünün:</span><span class="sxs-lookup"><span data-stu-id="bfab8-313">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="bfab8-314">Çıkış temizlemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bfab8-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="bfab8-315">Önceki komutu çalıştırdıktan sonra yalnızca aşağıdaki çıktıyı Komut kabuğunu içerir:</span><span class="sxs-lookup"><span data-stu-id="bfab8-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="bfab8-316">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bfab8-316">Additional resources</span></span>

* [<span data-ttu-id="bfab8-317">REST API istekleri</span><span class="sxs-lookup"><span data-stu-id="bfab8-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="bfab8-318">HTTP REPL GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="bfab8-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
