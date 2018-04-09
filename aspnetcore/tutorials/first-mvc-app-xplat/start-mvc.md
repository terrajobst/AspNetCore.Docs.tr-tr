---
title: ASP.NET Core MVC giriş macOS, Linux veya Windows
author: rick-anderson
description: MacOS, Linux ve Windows ASP.NET Core MVC ve Visual Studio Code ile çalışmaya başlama öğrenin
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>ASP.NET Core MVC giriş macOS, Linux veya Windows

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici bir ASP.NET Core MVC web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio Code](https://code.visualstudio.com) (VS Code). Öğretici VS Code ile familarity varsayar. Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için. 

[!INCLUDE [consider RP](../../includes/razor.md)]

Bu öğretici 3 sürümü vardır:

* macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Dotnet ile bir web uygulaması oluşturma

Terminal durumundan, aşağıdaki komutları çalıştırın:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Açık *MvcMovie* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.

- Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'MvcMovie' eksik. ileti Bunları eklensin mi?"
- Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'MvcMovie' eksik. Bunları eklensin mi? Evet ve ayrıca bilgisi yeniden şimdi değil sorma - çözümlenmemiş bağımlılıklar vardır - Restore - Kapat](../web-api-vsc/_static/vsc_restore.png)

Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).

![Uygulamayı çalıştırma](../first-mvc-app/start-mvc/_static/1.png)

VS Code başlatır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu ve uygulamanızı çalışır. Adres çubuğunun bildirim `localhost:5000` bir şey yok gibi ve `example.com`. Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.

Varsayılan şablonu, çalışma sunar **hakkında giriş** ve **kişi** bağlantılar. Yukarıdaki tarayıcı resimde bu bağlantıları göstermez. Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.

![sağ üstteki gezinti simgesi](../first-mvc-app/start-mvc/_static/2.png)

Bu öğreticinin sonraki bölümünde, biz MVC hakkında bilgi edinin ve biraz kod yazmaya başlamadan.

## <a name="visual-studio-code-help"></a>Visual Studio Code help

- [Başlarken](https://code.visualstudio.com/docs)
- [Hata Ayıklama](https://code.visualstudio.com/docs/editor/debugging)
- [Tümleşik terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Klavye kısayolları](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [macOS klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Sonraki - denetleyici ekleme](adding-controller.md)
