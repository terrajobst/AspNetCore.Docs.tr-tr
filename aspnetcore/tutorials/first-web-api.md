---
title: Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma
author: rick-anderson
description: ASP.NET Core MVC ve Windows için Visual Studio ile web API'si oluşturma
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 92b1b28205584d2f08a5dc8124e5c50aa938c80f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)

Bu öğretici, "Yapılacaklar" öğeleri listesini yönetmek için bir web API oluşturur. Bir kullanıcı arabirimi (UI) oluşturulmaz.

Bu öğretici 3 sürümü vardır:

* Windows: Web API ile Windows için Visual Studio (Bu öğretici)
* macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'dan seçin **dosya** menüsünde > **yeni** > **proje**.

Seçin **.NET Core** > **ASP.NET çekirdek Web uygulaması** proje şablonu. Proje adı `TodoApi` seçip **Tamam**.

![Yeni Proje iletişim kutusu](first-web-api/_static/new-project.png)

İçinde **yeni ASP.NET çekirdek Web uygulaması - TodoApi** iletişim kutusunda **API** şablonu. Seçin **Tamam**. Yapmak **değil** seçin **Docker desteğini etkinleştir**.

![ASP.NET Core şablonlardan seçili Web API projesi şablonuyla yeni ASP.NET Web uygulaması iletişim kutusu](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın. Visual Studio bir tarayıcı ile başlatarak `http://localhost:port/api/values`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır. Chrome, Microsoft Edge ve Firefox aşağıdaki çıkış görüntüler:

```
["value1","value2"]
```

### <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulama verileri temsil eden bir nesne modelidir. Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.

"Modelleri" adlı bir klasör ekleyin. Çözüm Gezgini'nde projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

Not: Modeli sınıfları projede herhangi bir yere gidin. *Modelleri* klasörü kural tarafından model sınıfları için kullanılır.

Ekleme bir `TodoItem` sınıfı. Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adını `TodoItem` seçip **Ekle**.

Güncelleştirme `TodoItem` aşağıdaki kodla sınıfı:

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.

### <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır. Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Ekleme bir `TodoContext` sınıfı. Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adını `TodoContext` seçip **Ekle**.

Sınıfı aşağıdaki kodla değiştirin:

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Denetleyici ekleme

Çözüm Gezgini'nde sağ *denetleyicileri* klasör. Seçin **ekleme** > **yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web API denetleyicisi sınıfı** şablonu. Sınıf adını `TodoController`.

![Yeni öğe iletişim denetleyicisiyle seçili arama kutusu ve web API denetleyicisi ekleyin](first-web-api/_static/new_controller.png)

Sınıfı aşağıdaki kodla değiştirin:

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın. Visual Studio bir tarayıcı ile başlatarak `http://localhost:port/api/values`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır. Gidin `Todo` denetleyicisinde `http://localhost:port/api/todo`.

[!INCLUDE [last part of web API](../includes/webApi/end.md)]

[!INCLUDE[Javascript Jquery](../includes/add-javascript-jquery/index.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

