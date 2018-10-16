---
title: ASP.NET Core ve Visual Studio Code ile Web API'si oluşturma
author: rick-anderson
description: MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile bir web API'si oluşturma
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: e549bc3adf3efa32b3ac975cf04a35f508a554d5
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325633"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>ASP.NET Core ve Visual Studio Code ile Web API'si oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)

Bu öğreticide, bir "Yapılacaklar" öğelerinin listesi yönetmek için bir web API'si oluşturma. Bir kullanıcı Arabirimi oluşturulur değil.

Bu öğretici üç sürümü vardır:

* macOS, Linux, Windows: Visual Studio Code (Bu öğreticide) ile Web API
* macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)
* Windows: [Windows için Visual Studio ile API Web](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Projeyi oluşturma

Bir konsoldan, aşağıdaki komutları çalıştırın:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

*TodoApi* Visual Studio Code'da (VS Code) klasörünü açar. Seçin *Startup.cs* dosya.

* Seçin **Evet** için **uyar** ileti "derleme ve hata ayıklamak için gerekli varlıkları 'TodoApi' eksik. Bunları eklensin mi?"
* Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıklar vardır" iletisi.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklamak için gerekli uyar varlıklar ile VS Code 'TodoApi' eksik. Bunları eklensin mi? Şimdi değil, Evet daha sorma](web-api-vsc/_static/vsc_restore.png)

Tuşuna **hata ayıklama** oluşturup programı çalıştırın (F5). Bir tarayıcıda gidin http://localhost:5000/api/values. Aşağıdaki çıktı görüntülenir:

```json
["value1","value2"]
```

Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) için VS Code kullanma hakkında ipuçları.

## <a name="add-support-for-entity-framework-core"></a>Entity Framework Core desteği eklendi

:::moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 veya daha sonra yeni proje oluşturma ekler [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) paketini başvuru *TodoApi.csproj* dosyası:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

ASP.NET Core 2.0 sürümünde yeni proje oluşturma ekler [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paketini başvuru *TodoApi.csproj* dosyası:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

Yüklemek için gerek yoktur [Entity Framework Core Inmemory](/ef/core/providers/in-memory/) sağlayıcısı ayrıca veritabanı. Bu veritabanı sağlayıcısı, bir bellek içi veritabanı ile kullanılacak Entity Framework Core sağlar.

## <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulamanızda veri temsil eden bir nesne modelidir. Bu durumda, bir yapılacak iş öğesi tek model budur.

Adlı bir klasör ekleme *modelleri*. Model sınıfları, projenizdeki herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural olarak kullanılır.

Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` olduğunda bir `TodoItem` oluşturulur.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturur

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Denetleyici ekleme

İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`. Dosyanın içeriğini aşağıdaki kodla değiştirin:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

VS Code'da uygulamasını başlatmak için F5 tuşuna basın. Gidin http://localhost:5000/api/todo ( `Todo` oluşturduğumuz denetleyicisi).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code Yardım

* [Başlarken](https://code.visualstudio.com/docs)
* [Hata Ayıklama](https://code.visualstudio.com/docs/editor/debugging)
* [Tümleşik terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Klavye kısayolları](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [macOS klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Linux klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Windows klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
