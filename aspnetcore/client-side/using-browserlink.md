---
title: ASP.NET core'da tarayıcı bağlantısı
author: ncarandini
description: Tarayıcı bağlantısı bir veya daha fazla web tarayıcıları geliştirme ortamıyla bağlanan bir Visual Studio özelliği ne olduğunu açıklar.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 5ab15c841c472e6c9d47bad70fcf5e0c6dc3010f
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894185"
---
# <a name="browser-link-in-aspnet-core"></a>ASP.NET core'da tarayıcı bağlantısı

Tarafından [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), ve [Tom Dykstra](https://github.com/tdykstra)

Tarayıcı bağlantısı, Visual Studio'da geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturan bir özelliktir. Çapraz tarayıcı test etmek için faydalı olan bazı tarayıcılarda, web uygulamanızın tek bir seferde yenilemek için tarayıcı bağlantısını kullanabilirsiniz.

## <a name="browser-link-setup"></a>Tarayıcı bağlantısı Kurulum

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 ve geçiş için bir ASP.NET Core 2.0 proje dönüştürülürken [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), yükleme [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) için paket BrowserLink işlevselliği. ASP.NET Core 2.1 proje şablonlarını kullanma `Microsoft.AspNetCore.App` metapackage varsayılan olarak.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **Web uygulaması**, **boş**, ve **Web API** proje şablonları kullanın [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) , bir paket başvurusu için içeren [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Bu nedenle, kullanarak `Microsoft.AspNetCore.All` metapackage tarayıcı bağlantısı kullanmak için kullanılabilir hale getirmek için başka bir işlem gerektirir.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1.x **Web uygulaması** proje şablonu için bir paket başvurusu var. [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paket. **Boş** veya **Web API** şablonu projeleri için bir paket başvurusu ekleme gerektirir `Microsoft.VisualStudio.Web.BrowserLink`.

Bu, bir Visual Studio özellik en kolay yolu pakete eklenecek olduğundan bir **boş** veya **Web API** şablon projedir açmak için **Paket Yöneticisi Konsolu** (**Görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu çalıştırın:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Alternatif olarak, **NuGet Paket Yöneticisi**. İçinde proje adınıza sağ **Çözüm Gezgini** ve **NuGet paketlerini Yönet**:

![Açık NuGet Paket Yöneticisi](using-browserlink/_static/open-nuget-package-manager.png)

Bul ve paket yükleyin:

![Paket ile NuGet Paket Yöneticisi ekleme](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Yapılandırma

İçinde `Startup.Configure` yöntemi:

```csharp
app.UseBrowserLink();
```

Kodu içindedir genellikle bir `if` yalnızca tarayıcı bağlantısı burada gösterildiği gibi geliştirme ortamında sağlayan bloğu:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Tarayıcı bağlantısı kullanma

Bir ASP.NET Core projesi açık olduğunda, Visual Studio tarayıcı bağlantısı araç çubuğu denetimi yanındaki gösteren **hata ayıklama hedefi** araç çubuğu denetimi:

![Tarayıcı bağlantısı aşağı açılan menüsü](using-browserlink/_static/browserLink-dropdown-menu.png)

Tarayıcı bağlantısı araç denetiminden şunları yapabilirsiniz:

* Web uygulamasını aynı anda birkaç tarayıcılarda yenileyin.
* Açık **tarayıcı bağlantı Panosu**.
* Etkinleştirmek veya devre dışı **tarayıcı bağlantısı**. Not: Tarayıcı bağlantısı Visual Studio 2017 (15.3) varsayılan olarak devre dışıdır.
* Etkinleştirmek veya devre dışı [CSS otomatik eşitleme](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Bazı Visual Studio eklentileri, özellikle *Web uzantı paketi 2015* ve *Web uzantı paketi 2017*genişletilmiş işlevselliği için tarayıcı bağlantısı sunar, ancak bazı ek özellikleri ASP ile çalışmaz. NET Core projeleri.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Çeşitli tarayıcılarda web uygulamasını aynı anda Yenile

Proje başlatılırken başlatmak için tek bir web tarayıcısı seçmek için aşağı açılan menüden kullanın **hata ayıklama hedefi** araç çubuğu denetimi:

![F5 açılan menüsü](using-browserlink/_static/debug-target-dropdown-menu.png)

Aynı anda birden çok tarayıcı açmak için seçin **birlikte Gözat...**  aynı açılır listeden. İstediğiniz tarayıcıları seçmek için CTRL tuşunu basılı tutun ve ardından **Gözat**:

![Tek seferde birçok tarayıcıda aç](using-browserlink/_static/open-many-browsers-at-once.png)

Visual Studio açık şekilde dizin görünümünün gösteren bir ekran ve iki açık tarayıcılar aşağıda verilmiştir:

![İki tarayıcılar örnek ile eşitleme](using-browserlink/_static/sync-with-two-browsers-example.png)

Tarayıcı bağlantısı araç çubuğu denetimi projesine bağlı tarayıcıları görmek için üzerine:

![Vurgulu İpucu](using-browserlink/_static/hoover-tip.png)

Index görünümünü değiştirin ve tarayıcı bağlantısı yenile düğmesine tıkladığınızda, tüm bağlı tarayıcıların güncelleştirilir:

![değişiklik tarayıcılar eşitleme](using-browserlink/_static/browsers-sync-to-changes.png)

Tarayıcı bağlantısı da dış Visual Studio'dan başlatmak ve uygulama URL'sine gidin tarayıcılarla çalışır.

### <a name="the-browser-link-dashboard"></a>Tarayıcı bağlantı Panosu

Tarayıcı bağlantısı açılan menüden açık tarayıcı bağlantısı yönetmek için tarayıcı bağlantı Panosu açın:

![Açık browserslink Panosu](using-browserlink/_static/open-browserlink-dashboard.png)

Hiçbir tarayıcı bağlıysa, seçerek hata ayıklama olmayan bir oturum başlatabilirsiniz *tarayıcıda görüntüle* bağlantı:

![Pano no bağlantıları browserlink](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Aksi takdirde, bağlı tarayıcıların her tarayıcıda gösterildiğini sayfasının yolu ile gösterilir:

![browserlink Panosu iki bağlantı](using-browserlink/_static/browserlink-dashboard-two-connections.png)

İsterseniz, tek bir tarayıcıyı yenilemek için listelenen tarayıcı adına tıklayabilirsiniz.

### <a name="enable-or-disable-browser-link"></a>Etkinleştirmek veya devre dışı tarayıcı bağlantısı

Tarayıcı bağlantısı devre dışı bırakıldıktan sonra yeniden etkinleştirdiğinizde, bunları yeniden bağlanmayı tarayıcılar yenilemeniz gerekir.

### <a name="enable-or-disable-css-auto-sync"></a>CSS otomatik eşitleme devre dışı bırakma veya etkinleştirme

CSS otomatik eşitleme etkinleştirildiğinde, CSS dosyaları için herhangi bir değişiklik yaptığınızda bağlı tarayıcıları otomatik olarak yenilenir.

## <a name="how-it-works"></a>Nasıl çalışır?

Tarayıcı bağlantısı, Visual Studio ile tarayıcı arasında bir iletişim kanalı oluşturmak için SignalR kullanır. Tarayıcı bağlantısı etkin olduğunda, Visual Studio için birden çok istemci (tarayıcı) bağlanabilen bir SignalR sunucusu olarak görev yapar. Tarayıcı bağlantısı, aynı zamanda ASP.NET istek işlem hattı, bir ara yazılım bileşeni kaydeder. Bu bileşen özel eklediği `<script>` sunucudan her sayfa isteği halinde başvuruları. Komut dosyası başvuruları seçerek gördüğünüz **kaynağı görüntüle** tarayıcı ve sonuna kadar kaydırma `<body>` etiketi içeriği:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Kaynak dosyalarını değiştiren değildir. Ara yazılım bileşeni dinamik olarak komut dosyası başvuruları ekler.

Tarayıcı tarafı kod, tüm JavaScript olduğundan, bir tarayıcı eklentisini gerek kalmadan SignalR destekleyen tüm tarayıcılarda çalışır.
