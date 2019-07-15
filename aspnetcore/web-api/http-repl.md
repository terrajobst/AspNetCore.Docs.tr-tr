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
# <a name="test-web-apis-with-the-http-repl"></a>Web API HTTP REPL ile test

Tarafından [Scott Addie](https://twitter.com/Scott_Addie)

HTTP okuma-Eval-Print Loop (REPL) aşağıdaki gibidir:

* .NET Core her yerde desteklenen basit, platformlar arası komut satırı aracı desteklenir.
* ASP.NET Core web API'ları sınamak ve sonuçları görüntülemek için HTTP isteği yapmak için kullanılır.

Aşağıdaki [HTTP fiilleri](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) desteklenir:

* [DELETE](#test-http-delete-requests)
* [GET](#test-http-get-requests)
* [HEAD](#test-http-head-requests)
* [SEÇENEKLER](#test-http-options-requests)
* [DÜZELTME EKİ](#test-http-patch-requests)
* [POST](#test-http-post-requests)
* [PUT](#test-http-put-requests)

Örneği takip etmek için [görüntüleme veya indirme örnek ASP.NET Core web API'si](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Önkoşullar

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a>Yükleme

HTTP REPL yüklemek için aşağıdaki komutu çalıştırın:

```console
dotnet tool install -g dotnet-httprepl
    --version 2.2.0-*
    --add-source https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
```

A [.NET Core genel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) gelen yüklü [dotnet httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet paketini Myget'te barındırılan.

## <a name="usage"></a>Kullanım

Aracı başarıyla yüklendikten sonra HTTP REPL başlatmak için aşağıdaki komutu çalıştırın:

```console
dotnet httprepl
```

Kullanılabilir tüm HTTP REPL komutları görmek için aşağıdaki komutlardan birini çalıştırın:

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

Aşağıdaki çıktı görüntülenir:

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

HTTP REPL komut tamamlama sunar. Tuşuna basarak <kbd>sekmesini</kbd> anahtar tam karakter veya yazdığınız API uç noktası komutları listesi üzerinden yinelenir. Aşağıdaki bölümlerde kullanılabilir CLI komutları özetler.

## <a name="connect-to-the-web-api"></a>Web API'sine bağlanma

Aşağıdaki komutu çalıştırarak bir web API'sine bağlanma:

```console
dotnet httprepl <BASE URI>
```

`<BASE URI>` web API'si için temel URI'dir. Örneğin:

```console
dotnet httprepl https://localhost:5001
```

Alternatif olarak, HTTP REPL çalışırken herhangi bir zamanda aşağıdaki komutu çalıştırın:

```console
set base <BASE URI>
```

Örneğin:

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a>Web API'si için Swagger belgesinin noktası

Web API'si düzgün bir şekilde incelemek için web API'si için Swagger belgesinin göreli URI ayarlayın. Şu komutu çalıştırın:

```console
set swagger <RELATIVE URI>
```

Örneğin:

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a>Web API'si gidin

### <a name="view-available-endpoints"></a>Kullanılabilir uç noktalarını görüntüle

Web API adresi geçerli yolda farklı uç noktalar (denetleyiciler) listelemek için çalıştırma `ls` veya `dir` komutu:

```console
https://localhot:5001/~ ls
```

Aşağıdaki çıktı biçimi görüntülenir:

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

Önceki çıkış iki denetleyici kullanılabilir olduğunu gösterir: `Fruits` ve `People`. Her iki denetleyicilerinin parametresiz bir HTTP GET ve POST işlemleri destekler.

Belirli bir Denetleyicisi'nde giderek daha fazla ayrıntı ortaya çıkarır. Örneğin, aşağıdaki komutu gösterir çıkışını `Fruits` denetleyicisi Ayrıca HTTP GET, PUT ve DELETE işlemlerini destekler. Bu işlemlerden her biriyle bekliyor bir `id` rota parametresi:

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

Alternatif olarak, çalıştırma `ui` web API'SİNİN Swagger kullanıcı arabirimini sayfasını bir tarayıcıda açmak için komutu. Örneğin:

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a>Bir uç noktasına gidin

Web API üzerinde farklı bir uç noktasına gitmek için çalıştırma `cd` komutu:

```console
https://localhost:5001/~ cd people
```

Yol aşağıdaki `cd` komutu büyük küçük harfe duyarlı. Aşağıdaki çıktı biçimi görüntülenir:

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a>HTTP REPL özelleştirme

HTTP REPL varsayılan [renkleri](#set-color-preferences) özelleştirilebilir. Ayrıca, bir [varsayılan metin düzenleyici](#set-the-default-text-editor) tanımlanabilir. HTTP REPL tercihleri geçerli oturumu arasında kalıcıdır ve gelecek oturumlar yerine getirilmez. Değiştirilen sonra aşağıdaki dosya tercihleri depolanır:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

*%HOME%/.httpreplprefs*

# <a name="macostabmacos"></a>[macOS](#tab/macos)

*%HOME%/.httpreplprefs*

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

*% USERPROFILE %\\.httpreplprefs*

---

*.Httpreplprefs* dosya başlangıçta yüklendi ve değişikliklerin çalışma zamanında izlenen değil. Dosyayı el ile yapılan değişiklikler yalnızca bir aracı yeniden başlatıldıktan sonra etkili olur.

### <a name="view-the-settings"></a>Ayarları görüntüleyin

Kullanılabilir ayarları görüntülemek için çalıştırın `pref get` komutu. Örneğin:

```console
https://localhost:5001/~ pref get
```

Önceki komutta, mevcut anahtar-değer çiftleri görüntüler:

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

### <a name="set-color-preferences"></a>Rengi tercihlerini ayarlama

Yanıt renklendirme şu anda yalnızca JSON için desteklenir. Renklendirme varsayılan HTTP REPL aracı özelleştirmek için anahtar karşılık gelen rengi değiştirilecek bulun. Anahtarların bulma hakkında yönergeler için bkz: [ayarları görüntüleyin](#view-the-settings) bölümü. Örneğin, değiştirme `colors.json` anahtar değerinden `Green` için `White` gibi:

```console
https://localhost:5001/people~ pref set colors.json White
```

Yalnızca [renkleri izin](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) kullanılabilir. Sonraki HTTP yeni renklendirme ile görüntü çıkış ister.

Belirli renk anahtarları olmayan ayarlarken daha genel anahtarlar olarak kabul edilir. Bu geri dönüş davranışını göstermek için aşağıdaki örneği göz önünde bulundurun:

* Varsa `colors.json.name` bir değere sahip `colors.json.string` kullanılır.
* Varsa `colors.json.string` bir değere sahip `colors.json.literal` kullanılır.
* Varsa `colors.json.literal` bir değere sahip `colors.json` kullanılır. 
* Varsa `colors.json` komut kabuğu'nın varsayılan metin rengi bir değere sahip değil (`AllowedColors.None`) kullanılır.

### <a name="set-indentation-size"></a>Girinti boyutunu Ayarla

Yanıt girinti boyutu özelleştirmesi şu anda yalnızca JSON için desteklenir. Varsayılan iki boşluk boyutudur. Örneğin:

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

Varsayılan boyutu değiştirmek için Ayarla `formatting.json.indentSize` anahtarı. Örneğin, her zaman dört alanları kullanın:

```console
pref set formatting.json.indentSize 4
```

Dört alan ayarını yanıtlar sonraki dikkate:

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

### <a name="set-indentation-size"></a>Girinti boyutunu Ayarla

Yanıt girinti boyutu özelleştirmesi şu anda yalnızca JSON için desteklenir. Varsayılan iki boşluk boyutudur. Örneğin:

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

Varsayılan boyutu değiştirmek için Ayarla `formatting.json.indentSize` anahtarı. Örneğin, her zaman dört alanları kullanın:

```console
pref set formatting.json.indentSize 4
```

Dört alan ayarını yanıtlar sonraki dikkate:

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

### <a name="set-the-default-text-editor"></a>Varsayılan metin Düzenleyicisi'ni ayarlayın

Varsayılan olarak, HTTP REPL kullanılmak üzere yapılandırılmış hiçbir metin düzenleyicisi içerir. Bir HTTP isteği gövdesinin gerektiren web API yöntemleri test etmek için bir varsayılan metin düzenleyicisi ayarlamanız gerekir. HTTP REPL aracı yapılandırılmış metin düzenleyici tek amacı, istek gövdesi oluşturma işlemi başlatır. Varsayılan olarak, tercih edilen metin düzenleyiciyi ayarlamak için aşağıdaki komutu çalıştırın:

```console
pref set editor.command.default "<EXECUTABLE>"
```

Önceki komutta `<EXECUTABLE>` Metin Düzenleyicisi'nin yürütülebilir dosyasının tam yoludur. Örneğin, varsayılan metin düzenleyici olarak Visual Studio Code ayarlamak için aşağıdaki komutu çalıştırın:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

Belirli CLI bağımsız değişkenlerle varsayılan metin düzenleyiciyi başlatmak için ayarlanmış `editor.command.default.arguments` anahtarı. Örneğin, varsayılan metin düzenleyicisi Visual Studio Code, ve her zaman Visual Studio Code uzantıları devre dışı yeni bir oturum açmak için HTTP REPL istediğinizi varsayalım. Şu komutu çalıştırın:

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a>HTTP GET isteklerini test

### <a name="synopsis"></a>Synopsis

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `get` komutu:

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a>Örnek

Bir HTTP GET isteği göndermektir almak için:

1. Çalıştırma `get` desteklediği uç noktasında komutu:

    ```console
    https://localhost:5001/people~ get
    ```

    Yukarıdaki komut, aşağıdaki çıkış biçimi görüntüler:

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

1. Belirli bir kaydı için bir parametre geçirerek almak `get` komutu:

    ```console
    https://localhost:5001/people~ get 2
    ```

    Yukarıdaki komut, aşağıdaki çıkış biçimi görüntüler:

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

## <a name="test-http-post-requests"></a>Testi HTTP POST istekleri

### <a name="synopsis"></a>Synopsis

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a>Örnek

Bir HTTP POST isteği göndermektir almak için:

1. Çalıştırma `post` desteklediği uç noktasında komutu:

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    Önceki komutta `Content-Type` HTTP isteği üst bilgisi, JSON isteği gövdesi medya türünü belirtmek için ayarlanır. Varsayılan metin düzenleyicisi açılır bir *.tmp* HTTP isteği gövdesinin temsil eden bir JSON şablon dosyası. Örneğin:

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > Varsayılan metin Düzenleyicisi'ni ayarlamak için bkz [kümesi varsayılan metin düzenleyici](#set-the-default-text-editor) bölümü.

1. Model doğrulama gereksinimlerini karşılamak için JSON şablonunu değiştirin:

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. Kaydet *.tmp* dosya ve Metin Düzenleyicisi'ni kapatın. Komut kabuğu'nda aşağıdaki çıktı görünür:

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

## <a name="test-http-put-requests"></a>HTTP PUT isteklerini test

### <a name="synopsis"></a>Synopsis

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a>Örnek

Bir HTTP PUT isteği göndermek için:

1. *İsteğe bağlı*: Çalıştırma `get` değiştirmeden önce verileri görüntülemek için komut:

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

    Önceki komutta `Content-Type` HTTP isteği üst bilgisi, JSON isteği gövdesi medya türünü belirtmek için ayarlanır. Varsayılan metin düzenleyicisi açılır bir *.tmp* HTTP isteği gövdesinin temsil eden bir JSON şablon dosyası. Örneğin:

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > Varsayılan metin Düzenleyicisi'ni ayarlamak için bkz [kümesi varsayılan metin düzenleyici](#set-the-default-text-editor) bölümü.

1. Model doğrulama gereksinimlerini karşılamak için JSON şablonunu değiştirin:

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. Kaydet *.tmp* dosya ve Metin Düzenleyicisi'ni kapatın. Komut kabuğu'nda aşağıdaki çıktı görünür:

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. *İsteğe bağlı*: Sorun bir `get` değişiklikleri görmek için komutu. Örneğin, Metin Düzenleyici'de "Tek tek seçme" yazılı bir `get` aşağıdaki döndürür:

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

## <a name="test-http-delete-requests"></a>HTTP DELETE isteklerini test

### <a name="synopsis"></a>Synopsis

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a>Örnek

Bir HTTP DELETE isteği göndermek için:

1. *İsteğe bağlı*: Çalıştırma `get` değiştirmeden önce verileri görüntülemek için komut:

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

    Yukarıdaki komut, aşağıdaki çıkış biçimi görüntüler:

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. *İsteğe bağlı*: Sorun bir `get` değişiklikleri görmek için komutu. Bu örnekte, bir `get` aşağıdaki döndürür:

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

## <a name="test-http-patch-requests"></a>HTTP PATCH isteği test

### <a name="synopsis"></a>Synopsis

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a>HTTP HEAD isteklerini test

### <a name="synopsis"></a>Synopsis

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a>HTTP OPTIONS istekleri test

### <a name="synopsis"></a>Synopsis

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Rota parametresi varsa, ilişkilendirilen denetleyiciye eylem yöntemi tarafından bekleniyor.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a>HTTP istek üstbilgilerini Ayarla

Bir HTTP isteği üstbilgisini ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:

1. Satır içi HTTP isteği ile ayarlayın. Örneğin:

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  Önceki yaklaşımda her ayrı HTTP isteği üstbilgisini kendi gerektirir `-h` seçeneği.

1. HTTP isteği göndermeden önce ayarlayın. Örneğin:

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  Üst bilgi isteği göndermeden önce ayarlarken, üst bilgi komut kabuk oturumu süresi için ayarlanmış olarak kalır. Üst bilgi temizlemek için boş bir değer sağlayın. Örneğin:

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a>HTTP isteği görüntüleme Değiştir

Varsayılan olarak, görüntü gönderilen HTTP isteğinin bastırılır. Komut kabuk oturumu süresi için karşılık gelen ayarı değiştirmek mümkündür.

### <a name="enable-request-display"></a>Etkinleştirme isteği görüntüleme

Çalıştırarak gönderilmekte olan HTTP isteğini `echo on` komutu. Örneğin:

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

Geçerli oturumda sonraki HTTP istekleri, istek üst bilgilerini görüntüler. Örneğin:

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

### <a name="disable-request-display"></a>İstek ekran devre dışı bırak

Çalışan tarafından gönderilen HTTP isteğinin bastır `echo off` komutu. Örneğin:

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a>Betik çalıştırma

Sık aynı HTTP REPL komutları kümesi yürütün, bir metin dosyasına saklamadan göz önünde bulundurun. Bu komut satırında el ile yürütülen aynı biçime dosyasındaki komutlar yararlanın. Komutları kullanarak bir toplu şekilde yürütülebilir `run` komutu. Örneğin:

1. Yeni satır ayrılmış komut kümesini içeren bir metin dosyası oluşturun. Göstermek için göz önünde bir *kişiler script.txt* aşağıdaki komutları içeren dosya:

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. Yürütme `run` komutu, metin dosyasının yolu geçirme. Örneğin:

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    Aşağıdaki çıktı görünür:

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

## <a name="clear-the-output"></a>Çıkışı Temizle

Komut kabuğu için HTTP REPL araç tarafından yazılan tüm çıkış kaldırmak için çalıştırın `clear` veya `cls` komutu. Göstermek için aşağıdaki çıktıyı Komut kabuğunu içerir düşünün:

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

Çıkış temizlemek için aşağıdaki komutu çalıştırın:

```console
https://localhost:5001/~ clear
```

Önceki komutu çalıştırdıktan sonra yalnızca aşağıdaki çıktıyı Komut kabuğunu içerir:

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a>Ek kaynaklar

* [REST API istekleri](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [HTTP REPL GitHub deposu](https://github.com/aspnet/HttpRepl)
