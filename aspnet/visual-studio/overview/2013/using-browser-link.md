---
uid: visual-studio/overview/2013/using-browser-link
title: Visual Studio 2013'te tarayıcı bağlantısı kullanma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: f470aa7e425d16aec3f67d2a0ebb664a3e7eac41
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755115"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Visual Studio 2013'te tarayıcı bağlantısı kullanma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Tarayıcı bağlantısı, geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturan Visual Studio 2013'te yeni bir özelliktir. Çapraz tarayıcı test etmek için faydalı olan bazı tarayıcılarda, web uygulamanızın tek bir seferde yenilemek için tarayıcı bağlantısını kullanabilirsiniz.

- [Tarayıcı Yenile](#browser-refresh)
- [Tarayıcı bağlantı Panosu görüntüleme](#dashboard)
- [Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirme](#static-html)
- [Tarayıcı bağlantısı devre dışı bırakma](#disabling)
- [Nasıl çalışır?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Tarayıcı Yenile

Tarayıcıyı yenileyin ile Visual Studio tarayıcı bağlantısı aracılığıyla bağlı birden çok tarayıcı yenileyebilirsiniz.

Tarayıcıyı yenileyin kullanmak için ilk proje şablonlarını kullanarak bir ASP.NET uygulaması oluşturun. Uygulama, F5 tuşuna basarak veya araç çubuğunda ok simgesine tıklayarak hata ayıklama:

![](using-browser-link/_static/image1.png)

Hata ayıklama için belirli bir tarayıcı seçmek için açılan listeyi de kullanabilirsiniz.

![](using-browser-link/_static/image2.png)

Birden çok tarayıcı ile hata ayıklamak için seçin **şununla Gözat**. İçinde **şununla Gözat** iletişim kutusunda birden fazla tarayıcı seçmek için CTRL tuşunu basılı tutun. Tıklayın **Gözat** seçili tarayıcıları ile hata ayıklamak için. Visual Studio dışında bir tarayıcıdan başlatın ve uygulama URL'sine gidin, tarayıcı bağlantısı da çalışır.

![](using-browser-link/_static/image3.png)

Tarayıcı bağlantısı denetimleri yuvarlak ok simgesi açılan listesi bulunur. Ok simgesi **Yenile** düğmesi.

![](using-browser-link/_static/image4.png)

Hangi tarayıcılar bağlı olduğunu görmek için fareyi üzerine **Yenile** hata ayıklama sırasında düğmesi. Bağlı tarayıcıların bir araç ipucu penceresi gösterilir.

![](using-browser-link/_static/image5.png)

Bağlı tarayıcıyı yenilemek için şuna tıklayın **Yenile** düğmesine veya CTRL + ALT + ENTER tuşlarına basın. Örneğin, aşağıdaki ekran görüntüsünde MVC 5 proje şablonunu kullanarak oluşturduğum bir ASP.NET projesi gösterir. Üstteki iki tarayıcılarında çalışan uygulama görebilirsiniz. Altındaki Proje Visual Studio'da açın.

![](using-browser-link/_static/image6.png)

Visual Studio'da değiştirdim &lt;h1&gt; giriş sayfası başlığı:

![](using-browser-link/_static/image7.png)

Ne zaman tıkladım **Yenile** düğmesi, değişikliği hem de Tarayıcı pencerelerinde görüntülenmektedir:

![](using-browser-link/_static/image8.png)

**Notlar**

- Tarayıcı bağlantısını etkinleştirmek için ayarlanmış `debug=true` içinde [ &lt;derleme&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) projenin Web.config dosyasında öğe.
- Uygulamayı, komut localhost üzerinde çalıştırılması gerekir.
- Uygulama, .NET 4.0 veya üzerini hedeflemelidir.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Tarayıcı bağlantı Panosu görüntüleme

Tarayıcı bağlantı Panosu, tarayıcı bağlantısı bağlantıları hakkındaki bilgileri gösterir. Panoyu görüntülemek için tarayıcı bağlantısı açılan menüyü seçin (yanındaki küçük ok **Yenile** düğmesi). Ardından **tarayıcı bağlantı Panosu**.

![](using-browser-link/_static/image9.png)

Pano, bağlı tarayıcıların ve istediğiniz her tarayıcıda gezinen URL listeler.

![](using-browser-link/_static/image10.png)

**Önkoşulları** bölümünde o proje için tarayıcı bağlantısını etkinleştirmek için gerekli tüm adımları gösterilir. Örneğin, aşağıdaki ekran görüntüsünde, "debug" false olarak Web.config dosyasında ayarlandığı bir proje gösterir.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirme

Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirmek için Web.config dosyasına aşağıdakileri ekleyin.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Projenizi yayımladığınızda performansla ilgili nedenlerden dolayı bu ayarı kaldırın.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Tarayıcı bağlantısı devre dışı bırakma

Tarayıcı bağlantısı varsayılan olarak etkindir. Devre dışı bırakmak için birkaç yol vardır:

- Tarayıcı bağlantısı açılan menüde'seçeneğinin işaretini kaldırın **tarayıcı bağlantısını etkinleştir**. 

    ![](using-browser-link/_static/image12.png)
- Web.config dosyasında "vs: EnableBrowserLink" değer "false" ile appSettings bölümündeki adlı bir anahtar ekleyin. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Web.config dosyasında hata ayıklama false olarak ayarlayın. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Nasıl çalışır?

Tarayıcı bağlantısı kullanan [SignalR](../../../signalr/index.md) Visual Studio ile tarayıcı arasında bir iletişim kanalı oluşturmak için. Tarayıcı bağlantısı etkin olduğunda, Visual Studio için birden çok istemci (tarayıcı) bağlanabilen bir SignalR sunucusu olarak görev yapar. Tarayıcı bağlantısı, ASP.NET ile bir HTTP modülü de kaydeder. Bu modül özel eklediği &lt;betik&gt; sunucudan her sayfa isteği halinde başvuruları. Komut dosyası başvuruları, tarayıcıda "Kaynağı görüntüle" seçeneğini belirleyerek görebilirsiniz.

![](using-browser-link/_static/image13.png)

Kaynak dosyalarını değiştirilmez. HTTP modülü, komut dosyası başvuruları dinamik olarak ekler.

Tarayıcı tarafı kod, tüm JavaScript olduğundan, tüm tarayıcılarla çalışır, [SignalR destekler](../../../signalr/overview/getting-started/supported-platforms.md), herhangi bir tarayıcı eklentisini gerektirmeden.
