---
title: MacOS, Linux veya Windows üzerinde ASP.NET Core MVC'ye giriş
author: rick-anderson
description: MacOS, Linux ve Windows üzerinde ASP.NET Core MVC ve Visual Studio Code ile çalışmaya başlama hakkında bilgi edinin
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 6b5835a7900c580cb88c004d3c8e7f70ad06c588
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861972"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>MacOS, Linux veya Windows üzerinde ASP.NET Core MVC'ye giriş

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide bir ASP.NET Core MVC kullanarak web uygulamasını oluşturmaya ilişkin temel bilgileri öğretir [Visual Studio Code](https://code.visualstudio.com) (VS Code). Bu öğretici, VS Code ile familarity varsayar. Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.

[!INCLUDE [consider RP](../../includes/razor.md)]

Bu öğreticinin 3 sürümü vardır:

* macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Dotnet ile bir web uygulaması oluşturma

Bir terminalde aşağıdaki komutları çalıştırın:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Açık *MvcMovie* klasör Visual Studio Code (VS Code) seçip *Startup.cs* dosya.

* Seçin **Evet** için **uyar** ileti "derleme ve hata ayıklamak için gerekli varlıkları 'MvcMovie' eksik. Bunları eklensin mi?"
* Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıklar vardır" iletisi.

![Derleme ve hata ayıklamak için gerekli uyar varlıklar ile VS Code 'MvcMovie' eksik. Bunları eklensin mi? Evet ve ayrıca bilgileri yeniden şimdi değil sorma - çözümlenmemiş bağımlılıklar vardır - geri yükleme - Kapat](../web-api-vsc/_static/vsc_restore.png)

Tuşuna **hata ayıklama** oluşturup programı çalıştırın (F5).

![Uygulamayı çalıştırma](../first-mvc-app/start-mvc/_static/1.png)

VS Code başlar [Kestrel](xref:fundamentals/servers/kestrel) sunucu ve uygulamanızı çalışır. Adres çubuğuna gösteren uyarı `localhost:5000` gibi bir şey `example.com`. Çünkü `localhost` standart, yerel bilgisayar adıdır.

Varsayılan şablonu size çalışma **giriş**, **hakkında**, ve **kişi** bağlantıları. Önceki tarayıcı resimde, bu bağlantıları göstermez. Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.

![sağ üstteki gezinti simgesi](../first-mvc-app/start-mvc/_static/2.png)

Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.

## <a name="visual-studio-code-help"></a>Visual Studio Code Yardım

* [Başlarken](https://code.visualstudio.com/docs)
* [Hata Ayıklama](https://code.visualstudio.com/docs/editor/debugging)
* [Tümleşik terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Klavye kısayolları](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [macOS klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Linux klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Windows klavye kısayolları](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Sonraki - denetleyici ekleme](adding-controller.md)
