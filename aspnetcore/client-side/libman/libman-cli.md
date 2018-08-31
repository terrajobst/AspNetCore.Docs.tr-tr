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
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>LibMan komut satırı arabirimi (CLI) ile ASP.NET Core kullanın

Tarafından [Scott Addie](https://twitter.com/Scott_Addie)

[LibMan](xref:client-side/libman/index) CLI, .NET Core desteklendiği her yerde desteklenen bir platformlar arası araçtır.

## <a name="prerequisites"></a>Önkoşullar

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Yükleme

LibMan CLI'yı yüklemek için:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

A [.NET Core genel aracı](/dotnet/core/tools/global-tools#install-a-global-tool) gelen yüklü [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet paketi.

Belirli bir NuGet paketini kaynaktan LibMan CLI'yı yüklemek için:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

Önceki örnekte, yerel Windows makineden ait bir .NET Core genel aracı yüklü *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* dosya.

## <a name="usage"></a>Kullanım

CLI'ın başarılı yüklemeden sonra aşağıdaki komut kullanılabilir:

```console
libman
```

Yüklü CLI sürümünü görüntülemek için:

```console
libman --version
```

Kullanılabilir tüm CLI komutları görmek için:

```console
libman --help
```

Yukarıdaki komut, aşağıdakine benzer çıkış görüntüler:

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

Aşağıdaki bölümlerde kullanılabilir CLI komutları özetler.

## <a name="initialize-libman-in-the-project"></a>Projede LibMan Başlat

`libman init` Komut oluşturur bir *libman.json* yoksa dosya. Dosyanın varsayılan öğe şablonu içeriği ile oluşturulur.

### <a name="synopsis"></a>Özeti

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `libman init` komutu:

* `-d|--default-destination <PATH>`

  Geçerli klasörüne göreli bir yol. Kitaplık dosyaları, bu konumda yüklenir, hiçbir `destination` özelliği bir kitaplıkta tanımlaması *libman.json*. `<PATH>` Değer yazılır `defaultDestination` özelliği *libman.json*.

* `-p|--default-provider <PROVIDER>`

  Belirli bir kitaplık için bir sağlayıcı tanımlanmışsa kullanılacak sağlayıcı. `<PROVIDER>` Değer yazılır `defaultProvider` özelliği *libman.json*. Değiştirin `<PROVIDER>` aşağıdaki değerlerden biriyle:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Örnekler

Oluşturmak için bir *libman.json* bir ASP.NET Core proje dosyasında:

* Proje kök dizinine gidin.
* Şu komutu çalıştırın:

  ```console
  libman init
  ```

* Tuşuna basın veya varsayılan sağlayıcı adı `Enter` varsayılan CDNJS sağlayıcıyı kullanmak için. Geçerli değerler şunlardır:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init komutunu - varsayılan sağlayıcı](_static/libman-init-provider.png)

A *libman.json* dosya aşağıdaki içeriğe sahip proje kök eklenir:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Kitaplık dosyaları Ekle

`libman install` Komut indirir ve projeye kitaplık dosyalarını yükler. A *libman.json* dosya yoksa eklenir. *Libman.json* yapılandırma ayrıntıları için kitaplık dosyalarını depolamak için dosya değiştirilmiş.

### <a name="synopsis"></a>Özeti

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Arguments

`LIBRARY`

Yüklemek için kitaplık adı. Bu ad, sürüm numarası gösterimi içerebilir (örneğin, `@1.2.0`).

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `libman install` komutu:

* `-d|--destination <PATH>`

  Kitaplığı yüklemek için konum. Belirtilmezse, varsayılan konumu kullanılır. Hayır ise `defaultDestination` özelliği belirtilen *libman.json*, bu seçenek gereklidir.

* `--files <FILE>`

  Kitaplıktan yüklenecek dosyanın adını belirtin. Belirtilmezse, tüm dosyaları Kitaplığı'ndan yüklenir. Bir tane sağlayın `--files` yüklenmesi için dosya başına seçeneği. Göreli yollar çok desteklenir. Örneğin: `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  Kitaplık alımı için kullanılan sağlayıcının adı. Değiştirin `<PROVIDER>` aşağıdaki değerlerden biriyle:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Belirtilmezse, `defaultProvider` özelliğinde *libman.json* kullanılır. Hayır ise `defaultProvider` özelliği belirtilen *libman.json*, bu seçenek gereklidir.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Örnekler

Aşağıdakileri göz önünde bulundurun *libman.json* dosyası:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

3.2.1 jQuery sürümünü yüklemek için *jquery.min.js* dosyasını *wwwroot\scripts\jquery* CDNJS sağlayıcısını kullanarak klasör:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot\scripts\jquery --files jquery.min.js
```

*Libman.json* dosyası aşağıdakine benzer:

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

Yüklenecek *calendar.js* ve *calendar.css* dosyalarını *C:\\temp\\contosoCalendar\\*  dosya sistemini kullanma Sağlayıcı:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

Aşağıdaki istemi iki nedenden dolayı görünür:

* *Libman.json* dosya içermediğinden bir `defaultDestination` özelliği.
* `libman install` Komut içermiyor `-d|--destination` seçeneği.

![libman yükleme komutunu - hedef](_static/libman-install-destination.png)

Sonra varsayılan hedef olarak kabul *libman.json* dosyası aşağıdakine benzer:

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

## <a name="restore-library-files"></a>Kitaplık dosyaları geri yükleme

`libman restore` Komutu kitaplık dosyaları içinde tanımlanan yükler *libman.json*. Aşağıdaki kurallar geçerlidir:

* Hayır ise *libman.json* dosyası, proje kök dizininde mevcut, hata döndürülür.
* Kitaplığa bir sağlayıcı belirtiyorsa `defaultProvider` özelliğinde *libman.json* göz ardı edilir.
* Kitaplığa bir hedef belirtiyorsa `defaultDestination` özelliğinde *libman.json* göz ardı edilir.

### <a name="synopsis"></a>Özeti

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `libman restore` komutu:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Örnekler

Tanımlanan kitaplık dosyaları geri yüklemek için *libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>Kitaplık dosyaları sil

`libman clean` Komut, daha önce LibMan geri kitaplık dosyalarını siler. Bu işlemden sonra boş duruma klasörler silinir. Kitaplık dosyaları ilişkili yapılandırmalarında `libraries` özelliği *libman.json* kaldırılmaz.

### <a name="synopsis"></a>Özeti

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `libman clean` komutu:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Örnekler

Yüklü LibMan kitaplık dosyalarını silmek için:

```console
libman clean
```

## <a name="uninstall-library-files"></a>Kitaplık dosyaları kaldırma

`libman uninstall` Komutu:

* Belirtilen hedef kitaplıktan ile ilişkili tüm dosyaları siler *libman.json*.
* İlişkili kitaplık yapılandırmasını kaldırır *libman.json*.

Bir hata meydana gelir olduğunda:

* Hayır *libman.json* dosyası, proje kök dizininde mevcut.
* Belirtilen kitaplık yok.

Aynı ada sahip birden fazla kitaplık yüklüyse bunlardan birini seçmeniz istenir.

### <a name="synopsis"></a>Özeti

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Arguments

`LIBRARY`

Kaldırmak için kitaplığının adı. Bu ad, sürüm numarası gösterimi içerebilir (örneğin, `@1.2.0`).

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `libman uninstall` komutu:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Örnekler

Aşağıdakileri göz önünde bulundurun *libman.json* dosyası:

[!code-json[](samples/LibManSample/libman.json)]

* JQuery kaldırmak için aşağıdaki komutlardan birini başarılı:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Aracılığıyla yüklenen Lodash dosyaları kaldırmak için `filesystem` sağlayıcısı:

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Kitaplık sürümünü güncelleştirme

`libman update` Komutu LibMan belirtilen sürümü için yüklü bir kitaplık güncelleştirir.

Bir hata meydana gelir olduğunda:

* Hayır *libman.json* dosyası, proje kök dizininde mevcut.
* Belirtilen kitaplık yok.

Aynı ada sahip birden fazla kitaplık yüklüyse bunlardan birini seçmeniz istenir.

### <a name="synopsis"></a>Özeti

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Arguments

`LIBRARY`

Güncelleştirilecek kitaplığının adı.

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `libman update` komutu:

* `-pre`

  Kitaplığı'nın en son yayın öncesi sürümünü edinin.

* `--to <VERSION>`

  Belirli bir kitaplığı sürümünü edinin.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Örnekler

* JQuery en son sürüme güncelleştirmek için:

  ```console
  libman update jquery
  ```

* JQuery 3.3.1 sürümüne güncelleştirmek için:

  ```console
  libman update jquery --to 3.3.1
  ```

* JQuery yayın öncesi en son sürüme güncelleştirmek için:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Kitaplık önbelleğini yönetme

`libman cache` Komut LibMan kitaplığı önbelleğini yönetir. `filesystem` Sağlayıcısı, kitaplık önbelleği kullanmaz.

### <a name="synopsis"></a>Özeti

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Arguments

`PROVIDER`

Yalnızca kullanılan `clean` komutu. Sağlayıcısı önbelleği temizlemek için belirtir. Geçerli değerler şunlardır:

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Seçenekler

Aşağıdaki seçenekler kullanılabilir `libman cache` komutu:

* `--files`

  Önbelleğe alınmış dosya adlarını listeler.

* `--libraries`

  Önbelleğe alınan kitaplıkları adlarını listeler.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Örnekler

* Önbelleğe alınan kitaplıkları sağlayıcı başına adlarını görüntülemek için aşağıdaki komutlardan birini kullanın:

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  Çıktı aşağıdaki gibi görüntülenir:

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

* Sağlayıcı başına önbelleğe alınmış kitaplık dosyalarının adlarını görüntülemek için:

  ```console
  libman cache list --files
  ```

  Çıktı aşağıdaki gibi görüntülenir:

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

  Önceki çıkış bildirimi CDNJS sağlayıcı altında 3.2.1 ve 3.3.1 sürümlerini önbelleğe alınır, jQuery gösterir.

* Kitaplık önbelleği için CDNJS sağlayıcısı boşaltmak için:

  ```console
  libman cache clean cdnjs
  ```

  CDNJS sağlayıcısı önbelleği boşaltma sonra `libman cache list` komutu şunları görüntüler:

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

* Tüm desteklenen sağlayıcılar için önbelleği boşaltmak için:

  ```console
  libman cache clean
  ```

  Tüm sağlayıcısı önbellekleri boşaltma sonra `libman cache list` komutu şunları görüntüler:

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>Ek kaynaklar

* [Genel bir aracı yükleme](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [LibMan GitHub deposu](https://github.com/aspnet/LibraryManager)
