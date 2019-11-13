---
title: ASP.NET Core tarayıcı bağlantısı
author: ncarandini
description: Tarayıcı bağlantısının, geliştirme ortamını bir veya daha fazla Web tarayıcısına bağlayan bir Visual Studio özelliği olduğunu açıklar.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962790"
---
# <a name="browser-link-in-aspnet-core"></a>ASP.NET Core tarayıcı bağlantısı

, [Nicolò Carandini](https://github.com/ncarandini), [Mike, son](https://github.com/MikeWasson)ve [Tom Dykstra](https://github.com/tdykstra) tarafından

Tarayıcı bağlantısı, Visual Studio 'da geliştirme ortamı ile bir veya daha fazla Web tarayıcısı arasında bir iletişim kanalı oluşturan bir özelliktir. Tarayıcı bağlantısını, Web uygulamanızı birden çok tarayıcıda tek seferde yenilemek için kullanabilirsiniz. Bu, tarayıcılar arası testler için kullanışlıdır.

## <a name="browser-link-setup"></a>Tarayıcı bağlantısı kurulumu

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2,0 projesi ASP.NET Core 2,1 ' e dönüştürülürken ve [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e geçiş yaparken, browserlink Işlevselliği için [Microsoft. VisualStudio. Web. browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paketini yükledikten sonra. ASP.NET Core 2,1 proje şablonları varsayılan olarak `Microsoft.AspNetCore.App` metapaketini kullanır.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2,0 **Web uygulaması**, **Empty**ve **Web API** proje şablonları, [Microsoft. VisualStudio. Web. browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)Için bir paket başvurusu içeren [Microsoft. aspnetcore. All meta paketini](xref:fundamentals/metapackage)kullanır. Bu nedenle, `Microsoft.AspNetCore.All` metapackage 'in kullanılması, tarayıcı bağlantısının kullanılabilir olmasını sağlamak için başka bir eylem gerektirmez.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1. x **Web uygulaması** proje şablonunda, [Microsoft. VisualStudio. Web. browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paketi için bir paket başvurusu vardır. **Boş** veya **Web apı** şablonu projeleri `Microsoft.VisualStudio.Web.BrowserLink`için bir paket başvurusu eklemenizi gerektirir.

Bu bir Visual Studio özelliği olduğundan, paketi **boş** veya **Web API** şablonu projesine eklemenin en kolay yolu, **Paket Yöneticisi konsolu 'nu** açmak ( **diğer Windows** > **Paket Yöneticisi konsolunu**>**görüntüleyin** ) ve şu komutu çalıştırmalıdır:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Alternatif olarak, **NuGet Paket Yöneticisi**' ni kullanabilirsiniz. **Çözüm Gezgini** ' de proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin:

![NuGet Paket Yöneticisi 'Ni aç](using-browserlink/_static/open-nuget-package-manager.png)

Paketi bulun ve yükledikten sonra:

![NuGet Paket Yöneticisi ile paket ekleme](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Yapılandırma

`Startup.Configure` yönteminde:

```csharp
app.UseBrowserLink();
```

Genellikle kod, burada gösterildiği gibi yalnızca geliştirme ortamında tarayıcı bağlantısını sağlayan bir `if` bloğunun içindedir:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Daha fazla bilgi için bkz. [birden çok ortam kullanma](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Tarayıcı bağlantısı kullanma

Bir ASP.NET Core projesi açıkken, Visual Studio **hata ayıklama hedefi** araç çubuğu denetiminin yanında tarayıcı bağlantısı araç çubuğu denetimini gösterir:

![Tarayıcı bağlantısı açılan menüsü](using-browserlink/_static/browserLink-dropdown-menu.png)

Tarayıcı bağlantısı araç çubuğu denetiminden şunları yapabilirsiniz:

* Web uygulamasını aynı anda birkaç tarayıcıda yenileyin.
* **Tarayıcı bağlantısı panosunu**açın.
* **Tarayıcı bağlantısını**etkinleştirin veya devre dışı bırakın. Note: tarayıcı bağlantısı, Visual Studio 2017 (15,3) içinde varsayılan olarak devre dışıdır.
* [CSS otomatik eşitlemesini](#enable-or-disable-css-auto-sync)etkinleştirin veya devre dışı bırakın.

> [!NOTE]
> Bazı Visual Studio eklentileri, en önemlisi *Web uzantısı paketi 2015* ve *web uzantısı paketi 2017*, tarayıcı bağlantısı için genişletilmiş işlevsellik sunar, ancak bazı ek özellikler ASP.NET Core projelerle çalışmaz.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Aynı anda birkaç tarayıcıda Web uygulamasını yenileyin

Projeyi başlatırken başlatılacak tek bir Web tarayıcısı seçmek için, **Hata Ayıkla hedef** araç çubuğu denetimindeki açılan menüyü kullanın:

![F5 açılır menüsü](using-browserlink/_static/debug-target-dropdown-menu.png)

Aynı anda birden çok tarayıcı açmak için, aynı açılan listeden **... öğesine gidin** ' i seçin. İstediğiniz tarayıcıları seçmek için CTRL tuşunu basılı tutarak, ardından da **Araştır**' a tıklayın:

![Birçok tarayıcıyı aynı anda aç](using-browserlink/_static/open-many-browsers-at-once.png)

Visual Studio 'Yu Açık Dizin görünümü ve iki açık tarayıcıyla gösteren bir ekran görüntüsü aşağıda verilmiştir:

![İki tarayıcıyla Eşitle örneği](using-browserlink/_static/sync-with-two-browsers-example.png)

Projeye bağlı tarayıcıları görmek için tarayıcı bağlantısı araç çubuğu denetiminin üzerine gelin:

![Üzerine gelme İpucu](using-browserlink/_static/hoover-tip.png)

Dizin görünümünü değiştirin ve tarayıcı bağlantısı yenileme düğmesine tıkladığınızda tüm bağlı tarayıcılar güncelleştirilir:

![tarayıcılar-değişikliklere karşı eşitleme](using-browserlink/_static/browsers-sync-to-changes.png)

Tarayıcı bağlantısı, Visual Studio dışından başlamış ve uygulama URL 'sine gidebileceğiniz tarayıcılarla de kullanılabilir.

### <a name="the-browser-link-dashboard"></a>Tarayıcı bağlantısı panosu

Açık tarayıcılarla bağlantıyı yönetmek için tarayıcı bağlantısı açılan menüsünden tarayıcı bağlantısı panosunu açın:

![Açık-browserslink-Pano](using-browserlink/_static/open-browserlink-dashboard.png)

Hiçbir tarayıcı bağlı değilse, *Tarayıcıda görüntüle* bağlantısını seçerek hata ayıklama olmayan bir oturum başlatabilirsiniz:

![browserlink-Pano-bağlantı yok](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Aksi halde, bağlı tarayıcılar her bir tarayıcının gösterdiği sayfanın yoluyla gösterilir:

![browserlink-Pano-iki bağlantı](using-browserlink/_static/browserlink-dashboard-two-connections.png)

İsterseniz, bu tek tarayıcıyı yenilemek için listelenen bir tarayıcı adına tıklayabilirsiniz.

### <a name="enable-or-disable-browser-link"></a>Tarayıcı bağlantısını etkinleştir veya devre dışı bırak

Tarayıcı bağlantısını devre dışı bıraktıktan sonra yeniden etkinleştirdiğinizde, bunları yeniden bağlamak için tarayıcıları yenilemeniz gerekir.

### <a name="enable-or-disable-css-auto-sync"></a>CSS otomatik eşitlemesini etkinleştir veya devre dışı bırak

CSS otomatik eşitleme etkinleştirildiğinde, CSS dosyalarında herhangi bir değişiklik yaptığınızda bağlantılı tarayıcılar otomatik olarak yenilenir.

## <a name="how-it-works"></a>Nasıl çalıştığı

Tarayıcı bağlantısı, Visual Studio ile tarayıcı arasında bir iletişim kanalı oluşturmak için SignalR kullanır. Tarayıcı bağlantısı etkinleştirildiğinde, Visual Studio birden çok istemcinin (tarayıcının) bağlanabileceği bir SignalR sunucusu gibi davranır. Tarayıcı bağlantısı ayrıca ASP.NET Core isteği ardışık düzenine bir ara yazılım bileşeni kaydeder. Bu bileşen, sunucudan her sayfa isteğine özel `<script>` başvurularını çıkarır. Tarayıcıda **Görünüm kaynağı** ' nı seçerek komut dosyası başvurularını görebilir ve `<body>` Tag içeriğinin sonuna kadar kaydırma yapabilirsiniz:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Kaynak dosyalarınız değiştirilmez. Ara yazılım bileşeni, betik başvurularını dinamik olarak çıkarır.

Tarayıcı tarafı kodu tüm JavaScript olduğundan, tarayıcı eklentisi gerekmeden SignalR desteklediği tüm tarayıcılarda çalışıyor olur.
