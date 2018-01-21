---
title: "ASP.NET Core tarayıcı bağlantısı"
author: ncarandini
description: "Tarayıcı bağlantısı bir veya daha fazla web tarayıcıları ile geliştirme ortamı bağlanan bir Visual Studio özelliği ne olduğunu açıklar."
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5db65c268923e96c45b034639437fc3496ccac1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="browser-link-in-aspnet-core"></a>ASP.NET Core tarayıcı bağlantısı 

Tarafından [Nicolò Carandini](https://github.com/ncarandini), [CAN Wasson](https://github.com/MikeWasson), ve [zel Dykstra](https://github.com/tdykstra)

Tarayıcı bağlantısı Visual Studio'da geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturan bir özelliktir. Tarayıcılar arası test etmek için faydalı olan bazı tarayıcılar, web uygulamanızda aynı anda yenilemek için tarayıcı bağlantısı kullanabilirsiniz.

## <a name="browser-link-setup"></a>Tarayıcı bağlantı Kur

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x **Web uygulaması**, **boş**, ve **Web API** şablon projeleri kullanım [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) için bir paket başvuru içeren meta-package [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Bu nedenle, kullanarak `Microsoft.AspNetCore.All` meta paketi tarayıcı bağlantısı kullanmak için kullanılabilir hale getirmek için başka bir eylem gerektirir.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x **Web uygulaması** proje şablonu için bir paket başvuru sahip [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paket. **Boş** veya **Web API** şablon projelerini paket başvuru eklemek gerekli `Microsoft.VisualStudio.Web.BrowserLink`.

Bu Visual Studio, en kolay yolu pakete eklemek için bir özelliktir bu yana bir **boş** veya **Web API** şablon proje mi açmak için **Paket Yöneticisi Konsolu** (**Görünüm** > **diğer pencereler** > **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu çalıştırın:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Alternatif olarak, kullanabileceğiniz **NuGet Paket Yöneticisi**. Proje adına sağ tıklayın **Çözüm Gezgini** ve **NuGet paketlerini Yönet**:

![Açık NuGet Paket Yöneticisi](using-browserlink/_static/open-nuget-package-manager.png)

Bul ve paketi yükleyin:

![Paket ile NuGet Paket Yöneticisi ekleme](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>Yapılandırma

İçinde `Configure` yöntemi *haline* dosyası:

```csharp
app.UseBrowserLink();
```

Kod içindedir genellikle bir `if` aşağıda gösterildiği gibi tarayıcı bağlantısı geliştirme ortamında yalnızca etkinleştirir blok:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Tarayıcı bağlantısı kullanma

Açık bir ASP.NET Core projeniz varsa, Visual Studio tarayıcı bağlantısı araç çubuğu denetimi yanına gösterir **hata ayıklama hedefi** araç çubuğu denetimi:

![Tarayıcı bağlantısı açılır menü](using-browserlink/_static/browserLink-dropdown-menu.png)

Tarayıcı bağlantısı araç denetiminden şunları yapabilirsiniz:

* Aynı anda web uygulaması birkaç tarayıcılarda yenileyin.
* Açık **tarayıcı bağlantı Pano**.
* Etkinleştirmek veya devre dışı **tarayıcı bağlantısı**. Not: Tarayıcı bağlantısı Visual Studio 2017 (15.3) varsayılan olarak devre dışıdır.
* Etkinleştirmek veya devre dışı [CSS otomatik eşitleme](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Bazı Visual Studio eklentiler, özellikle *Web uzantısı paketi 2015* ve *Web uzantısı paketi 2017*, tarayıcı bağlantısı için genişletilmiş işlevselliği sunar, ancak bazı ek özelliklerini ASP ile çalışmaz. NET çekirdek projeleri.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Aynı anda web uygulaması birkaç tarayıcılarda Yenile

Proje başlatma sırasında başlatmak için bir tek bir web tarayıcısı seçmek için açılır menüde kullanmak **hata ayıklama hedefi** araç çubuğu denetimi:

![F5 açılır menü](using-browserlink/_static/debug-target-dropdown-menu.png)

Aynı anda birden çok tarayıcı açmak için **ile Gözat...**  gelen aynı açılır. İstediğiniz tarayıcılar seçmek için CTRL tuşunu basılı tutun ve ardından **Gözat**:

![Aynı anda birçok tarayıcısı açın](using-browserlink/_static/open-many-browsers-at-once.png)

Visual Studio dizin görünümünün açık gösteren ekran görüntüsü ve iki açık tarayıcılar şöyledir:

![İki tarayıcılar örnek ile eşitleme](using-browserlink/_static/sync-with-two-browsers-example.png)

Projeye bağlı tarayıcılarda görmek için tarayıcı bağlantısı araç çubuğu denetimi üzerine getirin:

![İpucu getirin](using-browserlink/_static/hoover-tip.png)

Dizin görünümünün değiştirmek ve tarayıcı bağlantısı Yenile düğmesini tıklatın, tüm bağlı tarayıcılar güncelleştirilir:

![değişiklik tarayıcılar eşitleme](using-browserlink/_static/browsers-sync-to-changes.png)

Tarayıcı bağlantısı dış Visual Studio'dan başlatın ve uygulamayı URL'ye tarayıcıları ile de çalışır.

### <a name="the-browser-link-dashboard"></a>Tarayıcı bağlantısı Panosu

Tarayıcı bağlantısı açılan menüden bağlantı açık tarayıcılar ile yönetmek için tarayıcı bağlantı panosunu açın:

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

Hiçbir tarayıcı bağlıysa, seçerek hata ayıklama olmayan bir oturum başlatabilirsiniz *tarayıcıda görüntüle* bağlantı:

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Aksi takdirde, her tarayıcı gösteren sayfasının yolu ile bağlı tarayıcılarda gösterilmektedir:

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

İsterseniz, bu tek tarayıcıyı yenilemek için listelenen tarayıcı adına tıklayabilirsiniz.

### <a name="enable-or-disable-browser-link"></a>Etkinleştirmek veya devre dışı tarayıcı bağlantısı

Tarayıcı bağlantısı devre dışı bıraktıktan sonra yeniden etkinleştirdiğinizde, bunları yeniden bağlanmayı tarayıcılar yenilemeniz gerekir.

### <a name="enable-or-disable-css-auto-sync"></a>Etkinleştirmek veya CSS otomatik eşitleme devre dışı bırakma

CSS otomatik eşitleme etkinleştirildiğinde, CSS dosyaları herhangi bir değişiklik yaptığınızda bağlı tarayıcılar otomatik olarak yenilenir.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Tarayıcı bağlantısı SignalR Visual Studio ve tarayıcı arasındaki iletişim kanalını oluşturmak için kullanır. Tarayıcı bağlantısı etkin olduğunda, Visual Studio'nun birden çok istemci (tarayıcı) bağlanabileceği bir SignalR sunucusu olarak görev yapar. Tarayıcı bağlantısı, bir ara yazılım bileşeni ASP.NET istek ardışık düzeninde de kaydeder. Bu bileşen özel yerleştirir `<script>` sunucudan her sayfa isteği içine başvuruları. Betik başvuruların seçerek gördüğünüz **kaynağı görüntüle** tarayıcı ve sonuna kaydırma `<body>` etiketi içerik:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Kaynak dosyalarınız değiştiren değil. Ara yazılım bileşeni komut dosyası başvuruları dinamik olarak yerleştirir. 

Gözatıcı kod tüm JavaScript olduğundan, bir tarayıcı eklentisi gerek kalmadan SignalR destekleyen tüm tarayıcılar üzerinde çalışır.
