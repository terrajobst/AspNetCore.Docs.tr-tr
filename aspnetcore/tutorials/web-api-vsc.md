---
title: ASP.NET Çekirdeği ve Visual Studio Code ile Web API oluşturma
author: rick-anderson
description: MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile web API'si oluşturma
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4c41c949a9b5ca8db8928a0a53aff928fd7c8a4e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291677"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>ASP.NET Çekirdeği ve Visual Studio Code ile Web API oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)

Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma. Bir kullanıcı Arabirimi oluşturulan değil.

Bu öğretici için üç sürümü vardır:

* macOS, Linux, Windows: Visual Studio Code (Bu öğretici) ile Web API
* macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)
* Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)

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

*TodoApi* klasörünü Visual Studio Code (VS Code) açar. Seçin *haline* dosya.

* Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'TodoApi' eksik. ileti Bunları eklensin mi?"
* Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'TodoApi' eksik. Bunları eklensin mi? Şimdi değil Evet sorma](web-api-vsc/_static/vsc_restore.png)

Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5). Bir tarayıcıda gidin http://localhost:5000/api/values. Şu çıktı görüntülenir:

```json
["value1","value2"]
```

Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) VS Code kullanma hakkında ipuçları için.

## <a name="add-support-for-entity-framework-core"></a>Entity Framework Çekirdek desteği ekleme

:::moniker range="<= aspnetcore-2.0"
ASP.NET Core 2. 0 ' yeni bir proje oluşturma ekler [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paketini referansı *TodoApi.csproj* dosyası:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
ASP.NET Core 2.1 veya daha sonra yeni bir proje oluşturma ekler [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) paketini referansı *TodoApi.csproj* dosyası:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

Yüklemek için gerek yoktur [Entity Framework Çekirdek Inmemory](/ef/core/providers/in-memory/) sağlayıcı ayrı olarak veritabanı. Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.

## <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulamanızdaki verileri temsil eden bir nesne modelidir. Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.

Adlı bir klasör ekleme *modelleri*. Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.

Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.

## <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır. Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Denetleyici ekleme

İçinde *denetleyicileri* klasörünü adlı bir sınıf oluşturun `TodoController`. İçeriğini aşağıdaki kodla değiştirin:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

VS Code'da uygulamayı başlatmak için F5 tuşuna basın. Gidin http://localhost:5000/api/todo ( `Todo` oluşturduğumuz denetleyicisi).

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
