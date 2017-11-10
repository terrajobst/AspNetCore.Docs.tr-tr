---
title: "ASP.NET Core Mac, Linux veya Windows MVC giriş"
author: rick-anderson
description: "ASP.NET Core MVC ve Mac, Linux ve Windows Visual Studio Code ile çalışmaya başlama"
keywords: ASP.NET Core, MVC, VS kodu, Visual Studio kodu, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>ASP.NET Core MVC Mac, Linux veya Windows ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici bir ASP.NET Core MVC web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio Code](https://code.visualstudio.com) (VS Code). Öğretici VS Code ile familarity varsayar. Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için. [!INCLUDE[consider RP](../../includes/razor.md)]

Bu öğretici 3 sürümü vardır:

* macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>VS Code'da ve .NET Core yükleyin

Bu öğretici gerektirir [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü. Bkz: [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) ASP.NET Core 1.1 sürümünün.

Aşağıdaki yükleyin:

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

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

## <a name="visual-studio-code-help"></a>Visual Studio Code Yardım

- [Başlarken](https://code.visualstudio.com/docs)
- [Hata ayıklama](https://code.visualstudio.com/docs/editor/debugging)
- [Tümleşik terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Klavye kısayolları](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[Sonraki - denetleyici ekleme](adding-controller.md)
