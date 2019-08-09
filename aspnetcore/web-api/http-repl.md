---
title: HTTP REPL ile Web API 'Lerini test etme
author: scottaddie
description: HTTP REPL .NET Core küresel aracının bir ASP.NET Core Web API 'sini taramak ve test etmek için nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2019
uid: web-api/http-repl
ms.openlocfilehash: 0e80fcd76a4d3efcd35140c52e0f6f0ae0f27932
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862961"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="92865-103">HTTP REPL ile Web API 'Lerini test etme</span><span class="sxs-lookup"><span data-stu-id="92865-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="92865-104">[Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="92865-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="92865-105">HTTP okuma-değerlendirme-yazdırma döngüsü (REPL):</span><span class="sxs-lookup"><span data-stu-id="92865-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="92865-106">.NET Core 'un her yerde desteklenen basit, platformlar arası bir komut satırı aracı desteklenir.</span><span class="sxs-lookup"><span data-stu-id="92865-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="92865-107">ASP.NET Core Web API 'Lerini (ve non-ASP.NET çekirdek Web API 'Leri) test etmek ve sonuçlarını görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="92865-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="92865-108">Localhost ve Azure App Service dahil olmak üzere herhangi bir ortamda barındırılan Web API 'Lerini test etme özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="92865-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="92865-109">Aşağıdaki [http fiilleri](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) desteklenir:</span><span class="sxs-lookup"><span data-stu-id="92865-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="92865-110">SILMELI</span><span class="sxs-lookup"><span data-stu-id="92865-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="92865-111">GET</span><span class="sxs-lookup"><span data-stu-id="92865-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="92865-112">BAŞLI</span><span class="sxs-lookup"><span data-stu-id="92865-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="92865-113">SEÇENEKLER</span><span class="sxs-lookup"><span data-stu-id="92865-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="92865-114">DÜZELTMESI</span><span class="sxs-lookup"><span data-stu-id="92865-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="92865-115">POST</span><span class="sxs-lookup"><span data-stu-id="92865-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="92865-116">KONUR</span><span class="sxs-lookup"><span data-stu-id="92865-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="92865-117">Takip etmek için, [örnek ASP.NET Core Web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) 'sini ([indirme](xref:index#how-to-download-a-sample)) görüntüleyin veya indirin.</span><span class="sxs-lookup"><span data-stu-id="92865-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92865-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="92865-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="92865-119">Yükleme</span><span class="sxs-lookup"><span data-stu-id="92865-119">Installation</span></span>

<span data-ttu-id="92865-120">HTTP REPL 'u yüklemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-120">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="92865-121">[.NET Core küresel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) , [Microsoft. DotNet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet paketinden yüklenir.</span><span class="sxs-lookup"><span data-stu-id="92865-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="92865-122">Kullanım</span><span class="sxs-lookup"><span data-stu-id="92865-122">Usage</span></span>

<span data-ttu-id="92865-123">Aracın başarıyla yüklenmesinden sonra, HTTP REPL 'u başlatmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="92865-124">Kullanılabilir HTTP REPL komutlarını görüntülemek için aşağıdaki komutlardan birini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="92865-125">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="92865-125">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Sets the swagger document to use for information about the current server
ls             Show all endpoints for the current path
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands
exit           Exit the shell

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\\Program Files\\Microsoft VS Code\\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line
ui             Displays the Swagger UI page, if available, in the default browser

Use `help <COMMAND>` for more detail on an individual command. e.g. `help get`.
For detailed tool info, see https://aka.ms/http-repl-doc.
```

<span data-ttu-id="92865-126">HTTP REPL komut tamamlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="92865-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="92865-127"><kbd>Sekme</kbd> tuşuna basıldığında, yazdığınız KARAKTERLERI veya API uç noktasını tamamlayacak komutların listesi üzerinden yinelenir.</span><span class="sxs-lookup"><span data-stu-id="92865-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="92865-128">Aşağıdaki bölümlerde kullanılabilir CLı komutları ana hatlarıyla verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="92865-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="92865-129">Web API 'sine bağlanma</span><span class="sxs-lookup"><span data-stu-id="92865-129">Connect to the web API</span></span>

<span data-ttu-id="92865-130">Aşağıdaki komutu çalıştırarak bir Web API 'sine bağlanın:</span><span class="sxs-lookup"><span data-stu-id="92865-130">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="92865-131">`<BASE URI>`, Web API 'sinin temel URI 'sidir.</span><span class="sxs-lookup"><span data-stu-id="92865-131">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="92865-132">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-132">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="92865-133">Alternatif olarak, HTTP REPL çalışırken istediğiniz zaman aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="92865-134">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-134">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="92865-135">Web API 'SI için Swagger belgesine işaret edin</span><span class="sxs-lookup"><span data-stu-id="92865-135">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="92865-136">Web API 'sini düzgün bir şekilde incelemek için göreli URI 'yi Web API 'SI için Swagger belgesine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92865-136">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="92865-137">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-137">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="92865-138">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-138">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="92865-139">Web API 'sinde gezin</span><span class="sxs-lookup"><span data-stu-id="92865-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="92865-140">Kullanılabilir uç noktaları görüntüle</span><span class="sxs-lookup"><span data-stu-id="92865-140">View available endpoints</span></span>

<span data-ttu-id="92865-141">Web API adresinin geçerli yolundaki farklı uç noktaları (denetleyiciler) listelemek için, `ls` veya `dir` komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="92865-142">Aşağıdaki çıkış biçimi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="92865-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="92865-143">Yukarıdaki çıkış, kullanılabilir iki denetleyici olduğunu gösterir: `Fruits` ve. `People`</span><span class="sxs-lookup"><span data-stu-id="92865-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="92865-144">Her iki denetleyici de parametresiz HTTP GET ve POST işlemlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="92865-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="92865-145">Belirli bir denetleyicide gezinmek daha ayrıntılı bilgi gösterir.</span><span class="sxs-lookup"><span data-stu-id="92865-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="92865-146">Örneğin, aşağıdaki komut çıktısı, `Fruits` denetleyiciyi de http get, put ve DELETE işlemlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="92865-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="92865-147">Bu işlemlerin her biri, rotada `id` bir parametre bekler:</span><span class="sxs-lookup"><span data-stu-id="92865-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="92865-148">Alternatif olarak, Web `ui` API 'sinin Swagger Kullanıcı Arabirimi sayfasını bir tarayıcıda açmak için komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="92865-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="92865-149">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="92865-150">Bir uç noktaya gitme</span><span class="sxs-lookup"><span data-stu-id="92865-150">Navigate to an endpoint</span></span>

<span data-ttu-id="92865-151">Web API 'sindeki farklı bir uç noktaya gitmek için `cd` komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="92865-152">`cd` Komutu izleyen yol büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="92865-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="92865-153">Aşağıdaki çıkış biçimi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="92865-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="92865-154">HTTP REPL 'ı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="92865-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="92865-155">HTTP REPL 'un varsayılan [renkleri](#set-color-preferences) özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="92865-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="92865-156">Ayrıca, [varsayılan bir metin Düzenleyicisi](#set-the-default-text-editor) tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="92865-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="92865-157">HTTP REPL tercihleri geçerli oturum genelinde kalıcı hale getirilir ve gelecekteki oturumlarda kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="92865-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="92865-158">Değiştirildikten sonra, Tercihler aşağıdaki dosyada depolanır:</span><span class="sxs-lookup"><span data-stu-id="92865-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="92865-159">Linux</span><span class="sxs-lookup"><span data-stu-id="92865-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="92865-160">*% GIRIŞ%/. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="92865-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="92865-161">macOS</span><span class="sxs-lookup"><span data-stu-id="92865-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="92865-162">*% GIRIŞ%/. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="92865-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="92865-163">Windows</span><span class="sxs-lookup"><span data-stu-id="92865-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="92865-164">*% USERPROFILE%\\. httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="92865-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="92865-165">*. Httpreplprefs* dosyası başlangıçta yüklendi ve çalışma zamanında değişiklikler için izlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="92865-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="92865-166">Dosyada el ile yapılan değişiklikler yalnızca araç yeniden başlatıldıktan sonra devreye girer.</span><span class="sxs-lookup"><span data-stu-id="92865-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="92865-167">Ayarları görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="92865-167">View the settings</span></span>

<span data-ttu-id="92865-168">Kullanılabilir ayarları görüntülemek için `pref get` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="92865-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="92865-169">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="92865-170">Yukarıdaki komut, kullanılabilir anahtar-değer çiftlerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="92865-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="92865-171">Renk tercihlerini ayarla</span><span class="sxs-lookup"><span data-stu-id="92865-171">Set color preferences</span></span>

<span data-ttu-id="92865-172">Yanıt renklendirme Şu anda yalnızca JSON için destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="92865-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="92865-173">Varsayılan HTTP REPL aracı renklendirmesini özelleştirmek için, değiştirilecek renge karşılık gelen anahtarı bulun.</span><span class="sxs-lookup"><span data-stu-id="92865-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="92865-174">Anahtarları bulma hakkında yönergeler için bkz. [ayarları görüntüleme](#view-the-settings) bölümü.</span><span class="sxs-lookup"><span data-stu-id="92865-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="92865-175">Örneğin, `colors.json` anahtar `Green` değerini şu şekilde olacak `White` şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="92865-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="92865-176">Yalnızca [izin verilen renkler](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="92865-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="92865-177">Sonraki HTTP istekleri, yeni renklendirmesi ile çıktıyı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="92865-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="92865-178">Belirli renk anahtarları ayarlanmamışsa, daha genel anahtarlar kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="92865-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="92865-179">Bu geri dönüş davranışını göstermek için aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="92865-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="92865-180">Eğer `colors.json.name` bir değeri yoksa, `colors.json.string` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="92865-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="92865-181">Eğer `colors.json.string` bir değeri yoksa, `colors.json.literal` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="92865-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="92865-182">Eğer `colors.json.literal` bir değeri yoksa, `colors.json` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="92865-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="92865-183">Bir `colors.json` değere sahip değilse, komut kabuğun varsayılan metin rengi (`AllowedColors.None`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="92865-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="92865-184">Girinti boyutunu ayarla</span><span class="sxs-lookup"><span data-stu-id="92865-184">Set indentation size</span></span>

<span data-ttu-id="92865-185">Yanıt girintileme boyut özelleştirmesi Şu anda yalnızca JSON için destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="92865-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="92865-186">Varsayılan boyut iki boşluklardan oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="92865-186">The default size is two spaces.</span></span> <span data-ttu-id="92865-187">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-187">For example:</span></span>

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

<span data-ttu-id="92865-188">Varsayılan boyutu değiştirmek için `formatting.json.indentSize` anahtarı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92865-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="92865-189">Örneğin, her zaman dört boşluk kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="92865-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="92865-190">Sonraki yanıtlar dört boşluk ayarına uyar:</span><span class="sxs-lookup"><span data-stu-id="92865-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="92865-191">Girinti boyutunu ayarla</span><span class="sxs-lookup"><span data-stu-id="92865-191">Set indentation size</span></span>

<span data-ttu-id="92865-192">Yanıt girintileme boyut özelleştirmesi Şu anda yalnızca JSON için destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="92865-192">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="92865-193">Varsayılan boyut iki boşluklardan oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="92865-193">The default size is two spaces.</span></span> <span data-ttu-id="92865-194">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-194">For example:</span></span>

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

<span data-ttu-id="92865-195">Varsayılan boyutu değiştirmek için `formatting.json.indentSize` anahtarı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92865-195">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="92865-196">Örneğin, her zaman dört boşluk kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="92865-196">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="92865-197">Sonraki yanıtlar dört boşluk ayarına uyar:</span><span class="sxs-lookup"><span data-stu-id="92865-197">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="92865-198">Varsayılan metin düzenleyiciyi ayarlama</span><span class="sxs-lookup"><span data-stu-id="92865-198">Set the default text editor</span></span>

<span data-ttu-id="92865-199">Varsayılan olarak, HTTP REPL 'un kullanılmak üzere yapılandırılmış metin Düzenleyicisi yok.</span><span class="sxs-lookup"><span data-stu-id="92865-199">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="92865-200">HTTP istek gövdesi gerektiren Web API yöntemlerini test etmek için varsayılan metin Düzenleyicisi ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="92865-200">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="92865-201">HTTP REPL Aracı, istek gövdesini oluşturma amacıyla yapılandırılmış metin düzenleyicisini başlatır.</span><span class="sxs-lookup"><span data-stu-id="92865-201">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="92865-202">Tercih ettiğiniz metin düzenleyiciyi varsayılan olarak ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-202">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="92865-203">Yukarıdaki komutta, `<EXECUTABLE>` metin düzenleyicisinin yürütülebilir dosyasının tam yoludur.</span><span class="sxs-lookup"><span data-stu-id="92865-203">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="92865-204">Örneğin, Visual Studio Code varsayılan metin düzenleyicisi olarak ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-204">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="92865-205">Linux</span><span class="sxs-lookup"><span data-stu-id="92865-205">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="92865-206">macOS</span><span class="sxs-lookup"><span data-stu-id="92865-206">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="92865-207">Windows</span><span class="sxs-lookup"><span data-stu-id="92865-207">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="92865-208">Varsayılan metin düzenleyiciyi belirli CLI bağımsız değişkenleriyle başlatmak için `editor.command.default.arguments` anahtarı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92865-208">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="92865-209">Örneğin, Visual Studio Code varsayılan metin Düzenleyicisi olduğunu ve her zaman HTTP REPL 'un uzantıları devre dışı bırakılmış yeni bir oturumda Visual Studio Code açmasını istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="92865-209">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="92865-210">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-210">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="92865-211">HTTP GET isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="92865-211">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="92865-212">Özeti</span><span class="sxs-lookup"><span data-stu-id="92865-212">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="92865-213">Arguments</span><span class="sxs-lookup"><span data-stu-id="92865-213">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="92865-214">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="92865-214">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="92865-215">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="92865-215">Options</span></span>

<span data-ttu-id="92865-216">`get` Komutu için aşağıdaki seçenekler kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="92865-216">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="92865-217">Örnek</span><span class="sxs-lookup"><span data-stu-id="92865-217">Example</span></span>

<span data-ttu-id="92865-218">HTTP GET isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="92865-218">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="92865-219">`get` Komutu onu destekleyen bir uç noktada çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-219">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="92865-220">Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="92865-220">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="92865-221">`get` Komutuna bir parametre geçirerek belirli bir kaydı alın:</span><span class="sxs-lookup"><span data-stu-id="92865-221">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="92865-222">Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="92865-222">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="92865-223">HTTP POST isteklerini test et</span><span class="sxs-lookup"><span data-stu-id="92865-223">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="92865-224">Özeti</span><span class="sxs-lookup"><span data-stu-id="92865-224">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="92865-225">Arguments</span><span class="sxs-lookup"><span data-stu-id="92865-225">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="92865-226">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="92865-226">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="92865-227">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="92865-227">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="92865-228">Örnek</span><span class="sxs-lookup"><span data-stu-id="92865-228">Example</span></span>

<span data-ttu-id="92865-229">HTTP POST isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="92865-229">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="92865-230">`post` Komutu onu destekleyen bir uç noktada çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-230">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="92865-231">Önceki komutta, `Content-Type` http istek üst bilgisi, JSON türünde bir istek gövdesi medya türünü gösterecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="92865-231">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="92865-232">Varsayılan metin Düzenleyicisi, HTTP istek gövdesini temsil eden bir JSON şablonuyla bir *. tmp* dosyası açar.</span><span class="sxs-lookup"><span data-stu-id="92865-232">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="92865-233">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-233">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="92865-234">Varsayılan metin düzenleyicisini ayarlamak için [varsayılan metin düzenleyiciyi ayarlama](#set-the-default-text-editor) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="92865-234">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="92865-235">JSON şablonunu model doğrulama gereksinimlerini karşılayacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="92865-235">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="92865-236">*. Tmp* dosyasını kaydedin ve metin düzenleyicisini kapatın.</span><span class="sxs-lookup"><span data-stu-id="92865-236">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="92865-237">Aşağıdaki çıktı komut kabuğu 'nda görünür:</span><span class="sxs-lookup"><span data-stu-id="92865-237">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="92865-238">HTTP PUT isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="92865-238">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="92865-239">Özeti</span><span class="sxs-lookup"><span data-stu-id="92865-239">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="92865-240">Arguments</span><span class="sxs-lookup"><span data-stu-id="92865-240">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="92865-241">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="92865-241">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="92865-242">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="92865-242">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="92865-243">Örnek</span><span class="sxs-lookup"><span data-stu-id="92865-243">Example</span></span>

<span data-ttu-id="92865-244">HTTP PUT isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="92865-244">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="92865-245">*Isteğe bağlı*: Verileri değiştirmeden önce görüntülemek için komutunuçalıştırın:`get`</span><span class="sxs-lookup"><span data-stu-id="92865-245">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="92865-246">Önceki komutta, `Content-Type` http istek üst bilgisi, JSON türünde bir istek gövdesi medya türünü gösterecek şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="92865-246">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="92865-247">Varsayılan metin Düzenleyicisi, HTTP istek gövdesini temsil eden bir JSON şablonuyla bir *. tmp* dosyası açar.</span><span class="sxs-lookup"><span data-stu-id="92865-247">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="92865-248">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-248">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="92865-249">Varsayılan metin düzenleyicisini ayarlamak için [varsayılan metin düzenleyiciyi ayarlama](#set-the-default-text-editor) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="92865-249">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="92865-250">JSON şablonunu model doğrulama gereksinimlerini karşılayacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="92865-250">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="92865-251">*. Tmp* dosyasını kaydedin ve metin düzenleyicisini kapatın.</span><span class="sxs-lookup"><span data-stu-id="92865-251">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="92865-252">Aşağıdaki çıktı komut kabuğu 'nda görünür:</span><span class="sxs-lookup"><span data-stu-id="92865-252">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="92865-253">*Isteğe bağlı*: Değişiklikleri görmek `get` için bir komut verin.</span><span class="sxs-lookup"><span data-stu-id="92865-253">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="92865-254">Örneğin, metin düzenleyicisinde "chraz" yazdıysanız, bir `get` , şunu döndürür:</span><span class="sxs-lookup"><span data-stu-id="92865-254">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="92865-255">HTTP SILME isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="92865-255">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="92865-256">Özeti</span><span class="sxs-lookup"><span data-stu-id="92865-256">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="92865-257">Arguments</span><span class="sxs-lookup"><span data-stu-id="92865-257">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="92865-258">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="92865-258">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="92865-259">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="92865-259">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="92865-260">Örnek</span><span class="sxs-lookup"><span data-stu-id="92865-260">Example</span></span>

<span data-ttu-id="92865-261">HTTP SILME isteği vermek için:</span><span class="sxs-lookup"><span data-stu-id="92865-261">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="92865-262">*Isteğe bağlı*: Verileri değiştirmeden önce görüntülemek için komutunuçalıştırın:`get`</span><span class="sxs-lookup"><span data-stu-id="92865-262">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="92865-263">Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="92865-263">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="92865-264">*Isteğe bağlı*: Değişiklikleri görmek `get` için bir komut verin.</span><span class="sxs-lookup"><span data-stu-id="92865-264">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="92865-265">Bu örnekte, bir `get` , şunu döndürür:</span><span class="sxs-lookup"><span data-stu-id="92865-265">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="92865-266">HTTP PATCH isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="92865-266">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="92865-267">Özeti</span><span class="sxs-lookup"><span data-stu-id="92865-267">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="92865-268">Arguments</span><span class="sxs-lookup"><span data-stu-id="92865-268">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="92865-269">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="92865-269">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="92865-270">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="92865-270">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="92865-271">HTTP HEAD isteklerini test etme</span><span class="sxs-lookup"><span data-stu-id="92865-271">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="92865-272">Özeti</span><span class="sxs-lookup"><span data-stu-id="92865-272">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="92865-273">Arguments</span><span class="sxs-lookup"><span data-stu-id="92865-273">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="92865-274">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="92865-274">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="92865-275">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="92865-275">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="92865-276">Sınama HTTP SEÇENEKLERI istekleri</span><span class="sxs-lookup"><span data-stu-id="92865-276">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="92865-277">Özeti</span><span class="sxs-lookup"><span data-stu-id="92865-277">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="92865-278">Arguments</span><span class="sxs-lookup"><span data-stu-id="92865-278">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="92865-279">Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.</span><span class="sxs-lookup"><span data-stu-id="92865-279">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="92865-280">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="92865-280">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="92865-281">HTTP istek üst bilgilerini ayarla</span><span class="sxs-lookup"><span data-stu-id="92865-281">Set HTTP request headers</span></span>

<span data-ttu-id="92865-282">Bir HTTP istek üst bilgisi ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="92865-282">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="92865-283">HTTP isteğiyle satır içi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92865-283">Set inline with the HTTP request.</span></span> <span data-ttu-id="92865-284">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-284">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="92865-285">Önceki yaklaşımla, her ayrı http istek üst bilgisi kendi `-h` seçeneğini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="92865-285">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="92865-286">HTTP isteğini göndermeden önce ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92865-286">Set before sending the HTTP request.</span></span> <span data-ttu-id="92865-287">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-287">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="92865-288">Bir isteği göndermeden önce üst bilgi ayarlanırken üst bilgi, komut kabuğu oturumunun süresi boyunca ayarlanmış olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="92865-288">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="92865-289">Üstbilgiyi temizlemek için boş bir değer sağlayın.</span><span class="sxs-lookup"><span data-stu-id="92865-289">To clear the header, provide an empty value.</span></span> <span data-ttu-id="92865-290">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-290">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="92865-291">HTTP istek görüntüsünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="92865-291">Toggle HTTP request display</span></span>

<span data-ttu-id="92865-292">Varsayılan olarak, gönderilmekte olan HTTP isteğinin görüntüsü bastırılır.</span><span class="sxs-lookup"><span data-stu-id="92865-292">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="92865-293">Komut kabuğu oturumunun süresi boyunca ilgili ayarı değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="92865-293">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="92865-294">İstek görüntülemesini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="92865-294">Enable request display</span></span>

<span data-ttu-id="92865-295">`echo on` Komutunu çalıştırarak gönderilmekte olan http isteğini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="92865-295">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="92865-296">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-296">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="92865-297">Geçerli oturumdaki sonraki HTTP istekleri, istek üst bilgilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="92865-297">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="92865-298">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-298">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="92865-299">İstek görüntüsünü devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="92865-299">Disable request display</span></span>

<span data-ttu-id="92865-300">`echo off` Komutunu çalıştırarak gönderilen http isteğinin görüntülenmesini gizleyin.</span><span class="sxs-lookup"><span data-stu-id="92865-300">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="92865-301">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-301">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="92865-302">Betik çalıştırma</span><span class="sxs-lookup"><span data-stu-id="92865-302">Run a script</span></span>

<span data-ttu-id="92865-303">Aynı HTTP REPL komutları kümesini sıklıkla yürütüyorsanız bunları bir metin dosyasında depolamayı göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="92865-303">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="92865-304">Dosyadaki komutlar, komut satırında el ile çalıştıranlarla aynı formu alır.</span><span class="sxs-lookup"><span data-stu-id="92865-304">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="92865-305">Komutlar, `run` komutu kullanılarak toplanmış bir biçimde yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="92865-305">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="92865-306">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-306">For example:</span></span>

1. <span data-ttu-id="92865-307">Yeni satır için ayrılmış komutlar kümesini içeren bir metin dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="92865-307">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="92865-308">Göstermek için aşağıdaki komutları içeren bir *People-Script. txt* dosyası düşünün:</span><span class="sxs-lookup"><span data-stu-id="92865-308">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="92865-309">Metin dosyasının yolunu geçirerek komutunuyürütün.`run`</span><span class="sxs-lookup"><span data-stu-id="92865-309">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="92865-310">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92865-310">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="92865-311">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="92865-311">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="92865-312">Çıktıyı temizle</span><span class="sxs-lookup"><span data-stu-id="92865-312">Clear the output</span></span>

<span data-ttu-id="92865-313">HTTP REPL aracı tarafından komut kabuğu 'na yazılan tüm çıktıyı kaldırmak için `clear` veya `cls` komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="92865-313">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="92865-314">Göstermek için komut kabuğu 'nun aşağıdaki çıktıyı içerdiğini düşünün:</span><span class="sxs-lookup"><span data-stu-id="92865-314">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="92865-315">Çıktıyı temizlemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92865-315">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="92865-316">Yukarıdaki komutu çalıştırdıktan sonra, komut kabuğu yalnızca şu çıktıyı içerir:</span><span class="sxs-lookup"><span data-stu-id="92865-316">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="92865-317">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="92865-317">Additional resources</span></span>

* [<span data-ttu-id="92865-318">REST API istekleri</span><span class="sxs-lookup"><span data-stu-id="92865-318">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="92865-319">HTTP REPL GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="92865-319">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
