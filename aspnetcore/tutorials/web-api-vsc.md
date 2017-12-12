---
title: "ASP.NET Çekirdeği ve VS Code'u Web API'si oluşturma"
description: "MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile web API'si oluşturma"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, Webapı, Web API'si, REST, Mac, Linux, HTTP, hizmeti, HTTP hizmeti, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>ASP.NET Core MVC ve Linux, macOS ve Windows Visual Studio Code ile Web API oluşturun

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)

Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma. Bir kullanıcı Arabirimi yapı olmaz.

Bu öğretici 3 sürümü vardır:

* macOS, Linux, Windows: Visual Studio Code (Bu öğretici) ile Web API
* macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)
* Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı ayarlama

İndirin ve yükleyin:
- [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.
- [Visual Studio Code](https://code.visualstudio.com)
- Visual Studio Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## <a name="create-the-project"></a>Projeyi oluşturma

Bir konsoldan, aşağıdaki komutları çalıştırın:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Açık *TodoApi* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.

- Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'TodoApi' eksik. ileti Bunları eklensin mi?"
- Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'TodoApi' eksik. Bunları eklensin mi? Evet ve ayrıca bilgisi yeniden şimdi değil sorma - çözümlenmemiş bağımlılıklar vardır - Restore - Kapat](web-api-vsc/_static/vsc_restore.png)

Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5). Bir tarayıcıda http://localhost: 5000/api/değerleri gidin. Aşağıdaki görüntülenir:

`["value1","value2"]`

Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) VS Code kullanma hakkında ipuçları için.

## <a name="add-support-for-entity-framework-core"></a>Entity Framework Çekirdek desteği ekleme

Düzen *TodoApi.csproj* yüklemek için dosya [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) veritabanı sağlayıcısı. Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

Çalıştırma `dotnet restore` indirip EF çekirdek Inmemory DB Sağlayıcısı'nı yükleyin. Çalıştırabilirsiniz `dotnet restore` terminalde girin veya `⌘⇧P` (macOS) veya `Ctrl+Shift+P` (Linux) VS Code ve ardından yazın **.NET**. Seçin **.NET: geri yükleme paketleri**.

## <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulamanızdaki verileri temsil eden bir nesne modelidir. Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.

Adlı bir klasör ekleme *modelleri*. Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.

Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır. Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Denetleyici ekleme

İçinde *denetleyicileri* klasörünü adlı bir sınıf oluşturun `TodoController`. Aşağıdaki kodu ekleyin:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

VS Code'da uygulamayı başlatmak için F5 tuşuna basın. API/http://localhost: 5000/todo gidin ( `Todo` yeni oluşturduğumuz denetleyicisi).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code Yardım

- [Başlarken](https://code.visualstudio.com/docs)
- [Hata Ayıklama](https://code.visualstudio.com/docs/editor/debugging)
- [Tümleşik terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Klavye kısayolları](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


