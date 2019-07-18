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
# <a name="test-web-apis-with-the-http-repl"></a>HTTP REPL ile Web API 'Lerini test etme

[Scott Ade](https://twitter.com/Scott_Addie) tarafından

HTTP okuma-değerlendirme-yazdırma döngüsü (REPL):

* .NET Core 'un her yerde desteklenen basit, platformlar arası bir komut satırı aracı desteklenir.
* ASP.NET Core Web API 'Lerini test etmek ve sonuçlarını görüntülemek için HTTP istekleri yapmak amacıyla kullanılır.

Aşağıdaki [http fiilleri](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) desteklenir:

* [SILMELI](#test-http-delete-requests)
* [GET](#test-http-get-requests)
* [BAŞLI](#test-http-head-requests)
* [SEÇENEKLER](#test-http-options-requests)
* [DÜZELTMESI](#test-http-patch-requests)
* [POST](#test-http-post-requests)
* [KONUR](#test-http-put-requests)

Takip etmek için, [örnek ASP.NET Core Web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) 'sini ([indirme](xref:index#how-to-download-a-sample)) görüntüleyin veya indirin.

## <a name="prerequisites"></a>Önkoşullar

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a>Yükleme

HTTP REPL 'u yüklemek için aşağıdaki komutu çalıştırın:

```console
dotnet tool install -g dotnet-httprepl
    --version 2.2.0-*
    --add-source https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
```

[.NET Core küresel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) , [myget üzerinde barındırılan DotNet-httprepl](https://dotnet.myget.org/feed/dotnet-core/package/nuget/dotnet-httprepl
) NuGet paketinden yüklenir.

## <a name="usage"></a>Kullanım

Aracın başarıyla yüklenmesinden sonra, HTTP REPL 'u başlatmak için aşağıdaki komutu çalıştırın:

```console
dotnet httprepl
```

Kullanılabilir HTTP REPL komutlarını görüntülemek için aşağıdaki komutlardan birini çalıştırın:

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

HTTP REPL komut tamamlama sağlar. <kbd>Sekme</kbd> tuşuna basıldığında, yazdığınız KARAKTERLERI veya API uç noktasını tamamlayacak komutların listesi üzerinden yinelenir. Aşağıdaki bölümlerde kullanılabilir CLı komutları ana hatlarıyla verilmiştir.

## <a name="connect-to-the-web-api"></a>Web API 'sine bağlanma

Aşağıdaki komutu çalıştırarak bir Web API 'sine bağlanın:

```console
dotnet httprepl <BASE URI>
```

`<BASE URI>`, Web API 'sinin temel URI 'sidir. Örneğin:

```console
dotnet httprepl https://localhost:5001
```

Alternatif olarak, HTTP REPL çalışırken istediğiniz zaman aşağıdaki komutu çalıştırın:

```console
set base <BASE URI>
```

Örneğin:

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a>Web API 'SI için Swagger belgesine işaret edin

Web API 'sini düzgün bir şekilde incelemek için göreli URI 'yi Web API 'SI için Swagger belgesine ayarlayın. Şu komutu çalıştırın:

```console
set swagger <RELATIVE URI>
```

Örneğin:

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a>Web API 'sinde gezin

### <a name="view-available-endpoints"></a>Kullanılabilir uç noktaları görüntüle

Web API adresinin geçerli yolundaki farklı uç noktaları (denetleyiciler) listelemek için, `ls` veya `dir` komutunu çalıştırın:

```console
https://localhot:5001/~ ls
```

Aşağıdaki çıkış biçimi görüntülenir:

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

Yukarıdaki çıkış, kullanılabilir iki denetleyici olduğunu gösterir: `Fruits` ve. `People` Her iki denetleyici de parametresiz HTTP GET ve POST işlemlerini destekler.

Belirli bir denetleyicide gezinmek daha ayrıntılı bilgi gösterir. Örneğin, aşağıdaki komut çıktısı, `Fruits` denetleyiciyi de http get, put ve DELETE işlemlerini destekler. Bu işlemlerin her biri, rotada `id` bir parametre bekler:

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

Alternatif olarak, Web `ui` API 'sinin Swagger Kullanıcı Arabirimi sayfasını bir tarayıcıda açmak için komutunu çalıştırın. Örneğin:

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a>Bir uç noktaya gitme

Web API 'sindeki farklı bir uç noktaya gitmek için `cd` komutunu çalıştırın:

```console
https://localhost:5001/~ cd people
```

`cd` Komutu izleyen yol büyük/küçük harfe duyarlıdır. Aşağıdaki çıkış biçimi görüntülenir:

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a>HTTP REPL 'ı özelleştirme

HTTP REPL 'un varsayılan [renkleri](#set-color-preferences) özelleştirilebilir. Ayrıca, [varsayılan bir metin Düzenleyicisi](#set-the-default-text-editor) tanımlanabilir. HTTP REPL tercihleri geçerli oturum genelinde kalıcı hale getirilir ve gelecekteki oturumlarda kabul edilir. Değiştirildikten sonra, Tercihler aşağıdaki dosyada depolanır:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

*% GIRIŞ%/. httpreplprefs*

# <a name="macostabmacos"></a>[macOS](#tab/macos)

*% GIRIŞ%/. httpreplprefs*

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

*% USERPROFILE%\\. httpreplprefs*

---

*. Httpreplprefs* dosyası başlangıçta yüklendi ve çalışma zamanında değişiklikler için izlenmiyor. Dosyada el ile yapılan değişiklikler yalnızca araç yeniden başlatıldıktan sonra devreye girer.

### <a name="view-the-settings"></a>Ayarları görüntüleyin

Kullanılabilir ayarları görüntülemek için `pref get` komutunu çalıştırın. Örneğin:

```console
https://localhost:5001/~ pref get
```

Yukarıdaki komut, kullanılabilir anahtar-değer çiftlerini görüntüler:

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

### <a name="set-color-preferences"></a>Renk tercihlerini ayarla

Yanıt renklendirme Şu anda yalnızca JSON için destekleniyor. Varsayılan HTTP REPL aracı renklendirmesini özelleştirmek için, değiştirilecek renge karşılık gelen anahtarı bulun. Anahtarları bulma hakkında yönergeler için bkz. [ayarları görüntüleme](#view-the-settings) bölümü. Örneğin, `colors.json` anahtar `Green` değerini şu şekilde olacak `White` şekilde değiştirin:

```console
https://localhost:5001/people~ pref set colors.json White
```

Yalnızca [izin verilen renkler](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) kullanılabilir. Sonraki HTTP istekleri, yeni renklendirmesi ile çıktıyı görüntüler.

Belirli renk anahtarları ayarlanmamışsa, daha genel anahtarlar kabul edilir. Bu geri dönüş davranışını göstermek için aşağıdaki örneği göz önünde bulundurun:

* Eğer `colors.json.name` bir değeri yoksa, `colors.json.string` kullanılır.
* Eğer `colors.json.string` bir değeri yoksa, `colors.json.literal` kullanılır.
* Eğer `colors.json.literal` bir değeri yoksa, `colors.json` kullanılır. 
* Bir `colors.json` değere sahip değilse, komut kabuğun varsayılan metin rengi (`AllowedColors.None`) kullanılır.

### <a name="set-indentation-size"></a>Girinti boyutunu ayarla

Yanıt girintileme boyut özelleştirmesi Şu anda yalnızca JSON için destekleniyor. Varsayılan boyut iki boşluklardan oluşamaz. Örneğin:

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

Varsayılan boyutu değiştirmek için `formatting.json.indentSize` anahtarı ayarlayın. Örneğin, her zaman dört boşluk kullanmak için:

```console
pref set formatting.json.indentSize 4
```

Sonraki yanıtlar dört boşluk ayarına uyar:

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

### <a name="set-indentation-size"></a>Girinti boyutunu ayarla

Yanıt girintileme boyut özelleştirmesi Şu anda yalnızca JSON için destekleniyor. Varsayılan boyut iki boşluklardan oluşamaz. Örneğin:

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

Varsayılan boyutu değiştirmek için `formatting.json.indentSize` anahtarı ayarlayın. Örneğin, her zaman dört boşluk kullanmak için:

```console
pref set formatting.json.indentSize 4
```

Sonraki yanıtlar dört boşluk ayarına uyar:

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

### <a name="set-the-default-text-editor"></a>Varsayılan metin düzenleyiciyi ayarlama

Varsayılan olarak, HTTP REPL 'un kullanılmak üzere yapılandırılmış metin Düzenleyicisi yok. HTTP istek gövdesi gerektiren Web API yöntemlerini test etmek için varsayılan metin Düzenleyicisi ayarlanmalıdır. HTTP REPL Aracı, istek gövdesini oluşturma amacıyla yapılandırılmış metin düzenleyicisini başlatır. Tercih ettiğiniz metin düzenleyiciyi varsayılan olarak ayarlamak için aşağıdaki komutu çalıştırın:

```console
pref set editor.command.default "<EXECUTABLE>"
```

Yukarıdaki komutta, `<EXECUTABLE>` metin düzenleyicisinin yürütülebilir dosyasının tam yoludur. Örneğin, Visual Studio Code varsayılan metin düzenleyicisi olarak ayarlamak için aşağıdaki komutu çalıştırın:

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

Varsayılan metin düzenleyiciyi belirli CLI bağımsız değişkenleriyle başlatmak için `editor.command.default.arguments` anahtarı ayarlayın. Örneğin, Visual Studio Code varsayılan metin Düzenleyicisi olduğunu ve her zaman HTTP REPL 'un uzantıları devre dışı bırakılmış yeni bir oturumda Visual Studio Code açmasını istediğinizi varsayalım. Şu komutu çalıştırın:

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a>HTTP GET isteklerini test etme

### <a name="synopsis"></a>Özeti

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.

### <a name="options"></a>Seçenekler

`get` Komutu için aşağıdaki seçenekler kullanılabilir:

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a>Örnek

HTTP GET isteği vermek için:

1. `get` Komutu onu destekleyen bir uç noktada çalıştırın:

    ```console
    https://localhost:5001/people~ get
    ```

    Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:

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

1. `get` Komutuna bir parametre geçirerek belirli bir kaydı alın:

    ```console
    https://localhost:5001/people~ get 2
    ```

    Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:

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

## <a name="test-http-post-requests"></a>HTTP POST isteklerini test et

### <a name="synopsis"></a>Özeti

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a>Örnek

HTTP POST isteği vermek için:

1. `post` Komutu onu destekleyen bir uç noktada çalıştırın:

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    Önceki komutta, `Content-Type` http istek üst bilgisi, JSON türünde bir istek gövdesi medya türünü gösterecek şekilde ayarlanır. Varsayılan metin Düzenleyicisi, HTTP istek gövdesini temsil eden bir JSON şablonuyla bir *. tmp* dosyası açar. Örneğin:

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > Varsayılan metin düzenleyicisini ayarlamak için [varsayılan metin düzenleyiciyi ayarlama](#set-the-default-text-editor) bölümüne bakın.

1. JSON şablonunu model doğrulama gereksinimlerini karşılayacak şekilde değiştirin:

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. *. Tmp* dosyasını kaydedin ve metin düzenleyicisini kapatın. Aşağıdaki çıktı komut kabuğu 'nda görünür:

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

## <a name="test-http-put-requests"></a>HTTP PUT isteklerini test etme

### <a name="synopsis"></a>Özeti

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a>Örnek

HTTP PUT isteği vermek için:

1. *Isteğe bağlı*: Verileri değiştirmeden önce görüntülemek için komutunuçalıştırın:`get`

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

    Önceki komutta, `Content-Type` http istek üst bilgisi, JSON türünde bir istek gövdesi medya türünü gösterecek şekilde ayarlanır. Varsayılan metin Düzenleyicisi, HTTP istek gövdesini temsil eden bir JSON şablonuyla bir *. tmp* dosyası açar. Örneğin:

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > Varsayılan metin düzenleyicisini ayarlamak için [varsayılan metin düzenleyiciyi ayarlama](#set-the-default-text-editor) bölümüne bakın.

1. JSON şablonunu model doğrulama gereksinimlerini karşılayacak şekilde değiştirin:

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. *. Tmp* dosyasını kaydedin ve metin düzenleyicisini kapatın. Aşağıdaki çıktı komut kabuğu 'nda görünür:

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. *Isteğe bağlı*: Değişiklikleri görmek `get` için bir komut verin. Örneğin, metin düzenleyicisinde "chraz" yazdıysanız, bir `get` , şunu döndürür:

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

## <a name="test-http-delete-requests"></a>HTTP SILME isteklerini test etme

### <a name="synopsis"></a>Özeti

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a>Örnek

HTTP SILME isteği vermek için:

1. *Isteğe bağlı*: Verileri değiştirmeden önce görüntülemek için komutunuçalıştırın:`get`

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

    Yukarıdaki komut aşağıdaki çıkış biçimini görüntüler:

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. *Isteğe bağlı*: Değişiklikleri görmek `get` için bir komut verin. Bu örnekte, bir `get` , şunu döndürür:

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

## <a name="test-http-patch-requests"></a>HTTP PATCH isteklerini test etme

### <a name="synopsis"></a>Özeti

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a>HTTP HEAD isteklerini test etme

### <a name="synopsis"></a>Özeti

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a>Sınama HTTP SEÇENEKLERI istekleri

### <a name="synopsis"></a>Özeti

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a>Arguments

`PARAMETER`

Varsa, ilişkili denetleyici eylem yöntemi tarafından beklenen rota parametresi.

### <a name="options"></a>Seçenekler

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a>HTTP istek üst bilgilerini ayarla

Bir HTTP istek üst bilgisi ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:

1. HTTP isteğiyle satır içi ayarlayın. Örneğin:

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  Önceki yaklaşımla, her ayrı http istek üst bilgisi kendi `-h` seçeneğini gerektirir.

1. HTTP isteğini göndermeden önce ayarlayın. Örneğin:

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  Bir isteği göndermeden önce üst bilgi ayarlanırken üst bilgi, komut kabuğu oturumunun süresi boyunca ayarlanmış olarak kalır. Üstbilgiyi temizlemek için boş bir değer sağlayın. Örneğin:

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a>HTTP istek görüntüsünü değiştirme

Varsayılan olarak, gönderilmekte olan HTTP isteğinin görüntüsü bastırılır. Komut kabuğu oturumunun süresi boyunca ilgili ayarı değiştirmek mümkündür.

### <a name="enable-request-display"></a>İstek görüntülemesini etkinleştir

`echo on` Komutunu çalıştırarak gönderilmekte olan http isteğini görüntüleyin. Örneğin:

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

Geçerli oturumdaki sonraki HTTP istekleri, istek üst bilgilerini görüntüler. Örneğin:

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

### <a name="disable-request-display"></a>İstek görüntüsünü devre dışı bırak

`echo off` Komutunu çalıştırarak gönderilen http isteğinin görüntülenmesini gizleyin. Örneğin:

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a>Betik çalıştırma

Aynı HTTP REPL komutları kümesini sıklıkla yürütüyorsanız bunları bir metin dosyasında depolamayı göz önünde bulundurun. Dosyadaki komutlar, komut satırında el ile çalıştıranlarla aynı formu alır. Komutlar, `run` komutu kullanılarak toplanmış bir biçimde yürütülebilir. Örneğin:

1. Yeni satır için ayrılmış komutlar kümesini içeren bir metin dosyası oluşturun. Göstermek için aşağıdaki komutları içeren bir *People-Script. txt* dosyası düşünün:

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. Metin dosyasının yolunu geçirerek komutunuyürütün.`run` Örneğin:

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    Aşağıdaki çıktı görüntülenir:

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

## <a name="clear-the-output"></a>Çıktıyı temizle

HTTP REPL aracı tarafından komut kabuğu 'na yazılan tüm çıktıyı kaldırmak için `clear` veya `cls` komutunu çalıştırın. Göstermek için komut kabuğu 'nun aşağıdaki çıktıyı içerdiğini düşünün:

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

Çıktıyı temizlemek için aşağıdaki komutu çalıştırın:

```console
https://localhost:5001/~ clear
```

Yukarıdaki komutu çalıştırdıktan sonra, komut kabuğu yalnızca şu çıktıyı içerir:

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a>Ek kaynaklar

* [REST API istekleri](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [HTTP REPL GitHub deposu](https://github.com/aspnet/HttpRepl)
