---
uid: visual-studio/overview/2013/using-browser-link
title: "Visual Studio 2013'te tarayıcı bağlantısı kullanarak | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 14f67d81a5b460da591b8fb27fedf53d228e7717
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-browser-link-in-visual-studio-2013"></a>Visual Studio 2013'te tarayıcı bağlantısı kullanma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Tarayıcı bağlantısı, geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturur Visual Studio 2013'te yeni bir özelliktir. Tarayıcılar arası test etmek için faydalı olan bazı tarayıcılar, web uygulamanızda aynı anda yenilemek için tarayıcı bağlantısı kullanabilirsiniz.

- [Tarayıcı Yenile](#browser-refresh)
- [Tarayıcı bağlantısı panoyu görüntüleme](#dashboard)
- [Tarayıcı bağlantısı statik HTML dosyaları için etkinleştirme](#static-html)
- [Tarayıcı bağlantısı devre dışı bırakma](#disabling)
- [Nasıl çalışır?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Tarayıcı Yenile

Tarayıcıyı yenilemek ile Visual Studio tarayıcı bağlantısı üzerinden bağlanan birden çok tarayıcı yenileyebilirsiniz.

Tarayıcıyı yenilemek kullanmak için ilk proje şablonları birini kullanarak bir ASP.NET uygulaması oluşturun. Uygulama hata ayıklama F5 tuşuna basarak veya araç çubuğunda ok simgesini tıklatarak:

![](using-browser-link/_static/image1.png)

Hata ayıklama için belirli bir tarayıcı seçmek için açılır de kullanabilirsiniz.

![](using-browser-link/_static/image2.png)

Birden çok tarayıcı ile hata ayıklamak için seçin **Gözat ile**. İçinde **Gözat ile** iletişim kutusunda, birden fazla tarayıcı seçmek için CTRL tuşunu basılı tutun. Tıklatın **Gözat** seçili tarayıcılarla hata ayıklamak için. Dış Visual Studio'dan bir tarayıcıyı başlatın ve uygulamayı URL'ye gidin tarayıcı bağlantısı de çalışır.

![](using-browser-link/_static/image3.png)

Tarayıcı bağlantısı denetimleri döngüsel oku simgesiyle açılır bulunur. Ok simgesi **yenileme** düğmesi.

![](using-browser-link/_static/image4.png)

Hangi tarayıcılar bağlı olduğunu görmek için fare üzerine gelerek **yenileme** hata ayıklama sırasında düğmesi. Bağlı tarayıcıları bir araç ipucu penceresinde görüntülenir.

![](using-browser-link/_static/image5.png)

Bağlı tarayıcı yenilemek için tıklatın **yenileme** düğmesini tıklatın veya CTRL + ALT + ENTER tuşuna basın. Örneğin, aşağıdaki ekran görüntüsünde ı MVC 5 proje şablonu kullanılarak oluşturulan bir ASP.NET projesi gösterir. Üst iki tarayıcılarında çalışan uygulama görebilirsiniz. Alt kısmındaki projeyi Visual Studio'da açın.

![](using-browser-link/_static/image6.png)

Visual Studio'da değiştirdim &lt;h1&gt; başlık için giriş sayfası:

![](using-browser-link/_static/image7.png)

I tıklandığında **yenileme** düğmesi, her iki tarayıcı pencerelerini değişikliği görünen:

![](using-browser-link/_static/image8.png)

**Notlar**

- Tarayıcı bağlantısını etkinleştirmek için ayarlanmış `debug=true` içinde [ &lt;derleme&gt; ](https://msdn.microsoft.com/en-us/library/s10awwz0(v=vs.85).aspx) projenin Web.config dosyasında öğesi.
- Uygulama localhost üzerinde çalışmalıdır.
- Uygulama, .NET 4.0 veya üzeri hedeflemesi gerekir.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Tarayıcı bağlantısı panoyu görüntüleme

Tarayıcı bağlantısı Panosu tarayıcı bağlantısı bağlantılar hakkında bilgi gösterir. Panoyu görüntülemek için tarayıcı bağlantısı açılır menüsünü seçin (yanında küçük oka **yenileme** düğmesi). Ardından **tarayıcı bağlantı Pano**.

![](using-browser-link/_static/image9.png)

Pano bağlı tarayıcılar ve her tarayıcı gittiğinizde URL listeler.

![](using-browser-link/_static/image10.png)

**Önkoşullar** bölümünde bu proje için tarayıcı bağlantısını etkinleştirmek için gereken adımlar gösterilir. Örneğin, aşağıdaki ekran görüntüsünde, "hata ayıklama" false olarak Web.config dosyasında ayarlandığı bir proje gösterir.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Tarayıcı bağlantısı statik HTML dosyaları için etkinleştirme

Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirmek için aşağıdaki Web.config dosyanıza ekleyin.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Projenizi yayımladığınızda, performansı artırmak için bu ayarı kaldırın.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Tarayıcı bağlantısı devre dışı bırakma

Tarayıcı bağlantısı varsayılan olarak etkindir. Devre dışı bırakmak birkaç yolu vardır:

- Tarayıcı bağlantısı açılır menüde işaretini **tarayıcı bağlantısını etkinleştirmek**. 

    ![](using-browser-link/_static/image12.png)
- Web.config dosyasında "vs: EnableBrowserLink" "false" değeri ile appSettings bölümünde adlı bir anahtar ekleyin. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Web.config dosyasında hata ayıklama false olarak ayarlayın. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Nasıl çalışır?

Tarayıcı bağlantısını kullanan [SignalR](../../../signalr/index.md) Visual Studio ve tarayıcı arasındaki iletişim kanalını oluşturmak için. Tarayıcı bağlantısı etkin olduğunda, Visual Studio'nun birden çok istemci (tarayıcı) bağlanabileceği bir SignalR sunucusu olarak görev yapar. Tarayıcı bağlantısı, bir HTTP modülü ASP.NET ile de kaydeder. Bu modül özel yerleştirir &lt;betik&gt; sunucudan her sayfa isteği içine başvuruları. Komut dosyası başvuruları tarayıcıda "Kaynağı görüntüle" seçerek görebilirsiniz.

![](using-browser-link/_static/image13.png)

Kaynak dosyalarınız değiştirilmez. HTTP modülü, komut dosyası başvuruları dinamik olarak yerleştirir.

Gözatıcı kod tüm JavaScript olduğundan, tüm tarayıcılarla çalışır, [SignalR destekleyen](../../../signalr/overview/getting-started/supported-platforms.md), tüm tarayıcı eklentisi gerektirmeden.
