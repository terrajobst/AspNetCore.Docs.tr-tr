---
title: ASP.NET Core Sınıf Kitaplığı'nda yeniden kullanılabilir Razor UI
author: Rick-Anderson
description: Yeniden kullanılabilir Razor UI bir sınıf kitaplığında oluşturulması açıklanmaktadır.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39378676"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Yeniden kullanılabilir kullanıcı Arabirimi ASP.NET Core Razor sınıf kitaplığı projesini kullanarak oluşturun.

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor görünümleri, sayfalar, denetleyicileri, sayfa modelleri [görüntülemek bileşenleri](xref:mvc/views/view-components), ve veri modelleri bir Razor sınıf kitaplığı (RCL) içine derlenebilir. RCL paketlenir ve yeniden kullanılabilir. Uygulamalar RCL içerir ve görünümlere ve sayfalara içerdiği geçersiz kılar. Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de RCL Razor işaretlemesi bulunur (*.cshtml* dosya) web uygulaması önceliklidir.

Bu özellik gerektirir [!INCLUDE[](~/includes/2.1-SDK.md)]

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Razor UI içeren bir sınıf kitaplığı oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* (Örneğin, "RazorClassLib") kitaplığı adı > **Tamam**. Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* Seçin **Razor sınıf kitaplığı** > **Tamam**.

Razor sınıf kitaplığı aşağıdaki proje dosyası vardır:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından çalıştırmak `dotnet new razorclasslib`. Örneğin:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new). Oluşturulan görünüm kitaplığı ile bir dosya adı çakışması önlemek için kitaplık adını bitmiyor olun `.Views`.

------
Razor dosyaları için RCL ekleyin.

ASP.NET Core şablonları RCL içeriği olduğu varsayılır *alanları* klasör. Bkz: [RCL sayfa düzeni](#afs) kullanıma sunan bir RCL içeriği oluşturmak için `~/Pages` yerine `~/Areas/Pages`.

## <a name="referencing-razor-class-library-content"></a>Razor sınıf kitaplığı içeriği başvurma

RCL tarafından başvurulabilir:

* NuGet paketi. Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) ve [oluştur ve NuGet paket yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName} .csproj*. Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>İzlenecek yol: bir Razor sınıf kitaplığı projesi oluşturun ve bir Razor sayfaları projeden kullanın

İndirebileceğiniz [tam proje](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ve yerine oluşturma test edin. Örnek indirme, ek kod ve test etmek proje kolaylaştırmak bağlantılar içerir. Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6098) yorumlarınızı üzerinde adım adım yönergeler ve örnekleri indirme ile.

### <a name="test-the-download-app"></a>İndirme uygulamayı test etme

Tamamlanmış uygulamayı karşıdan henüz ve bunun yerine izlenecek projesi oluşturacak, atlamak [sonraki bölümde](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Açık *.sln* dosyasını Visual Studio'da. Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Bir komut isteminden *CLI* dizin RCL oluşturun ve web uygulaması.

``` CLI
dotnet build
```

Taşı *WebApp1* dizin ve uygulamayı çalıştırın:

``` CLI
dotnet run
```
------

Bölümündeki yönergeleri [Test WebApp1](#test)

## <a name="create-a-razor-class-library"></a>Razor sınıf kitaplığı oluşturma

Bu bölümde, Razor sınıf kitaplığı (RCL) oluşturulur. Razor dosyaları için RCL eklenir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

RCL projesi oluşturun:

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* Uygulamayı adlandırma **RazorUIClassLib** > **Tamam**.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* Seçin **Razor sınıf kitaplığı** > **Tamam**.
* Razor kısmi Görünüm adlı bir dosya ekleyin *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından şu komutu çalıştırın:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Yukarıdaki komutlar:

* Oluşturur `RazorUIClassLib` Razor sınıf kitaplığı (RCL).
* Bir Razor il_eti sayfası oluşturur ve için RCL ekler. `-np` Parametresi olmadan sayfa oluşturur bir `PageModel`.
* Oluşturur bir [viewstart](xref:mvc/views/layout#running-code-before-each-view) dosya ve için RCL ekler.

Viewstart dosya (sonraki bölümde eklenir) Razor sayfaları proje düzenini kullanmak için gereklidir.

------

### <a name="add-razor-files-and-folders-to-the-project"></a>Razor dosyaları ve klasörleri projeye ekleyin.

* Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Biçimlendirmeyi Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`). Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya. Örneğin:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Viewimports hakkında daha fazla bilgi için bkz. [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)

* Derleyici hata doğrulamak için sınıf kitaplığı derleme:

``` CLI
dotnet build RazorUIClassLib
```

Derleme çıktısını içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* derlenmiş Razor içeriği.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Bir Razor sayfaları projeden Razor kullanıcı Arabirimi kitaplığını kullanma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Razor sayfaları web uygulaması oluşturun:

* Gelen **Çözüm Gezgini**, çözüme sağ tıklayın > **Ekle** >  **yeni proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* Uygulamayı adlandırma **WebApp1**.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* Seçin **Web uygulaması** > **Tamam**.

* Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **başlangıç projesi olarak ayarla**.
* Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **yapı bağımlılıkları** > **proje bağımlılıkları**.
* Denetleme **RazorUIClassLib** bağımlılık olarak **WebApp1**.
* Gelen **Çözüm Gezgini**, sağ **WebApp1** seçip **Ekle** > **başvuru**.
* İçinde **başvuru Yöneticisi** iletişim kutusunda, onay **RazorUIClassLib** > **Tamam**.

Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Razor sayfaları web uygulaması ve Razor sayfaları uygulama ve Razor sınıf kitaplığı içeren bir çözüm dosyası oluşturun:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

Web uygulaması derleyebilir ve:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>Test WebApp1

Razor UI sınıf kitaplığı kullanılan doğrulayın.

* Gözat `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Görünümleri, kısmi görünümleri ve sayfa geçersiz kıl

Zaman görünümü, kısmi görünüm veya Razor sayfası hem web uygulaması hem de Razor sınıf kitaplığı, Razor işaretlemesi bulunur (*.cshtml* dosya) web uygulaması önceliklidir. Örneğin, ekleme *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* WebApp1 için ve Sayfa1 WebApp1 içinde önceliklidir Page1in üzerinde Razor sınıf kitaplığı.

Örnek indirme Yeniden Adlandır *WebApp1/alanlar/MyFeature2* için *WebApp1/alanlar/MyFeature* öncelik test etmek için.

Kopyalama *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Yeni bir konum belirtmek için işaretleme güncelleştirin. Derleme ve uygulamanın sürümünü kısmi kullanılan doğrulamak için uygulamayı çalıştırın.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>RCL sayfa düzeni

Web uygulaması'nın sayfalar klasöründe bir parçasıdır ancak RCL içeriği başvurusu yapmak için aşağıdaki dosya yapısı ile RCL proje oluşturun:

* *RazorUIClassLib/sayfaları*
* *RazorUIClassLib/sayfalar/paylaşılan*

Varsayalım *RazorUIClassLib/sayfaları/paylaşılan* iki kısmi dosyalarını içeren *_Header.cshtml* ve *_Footer.cshtml*. <partial> Etiketleri için eklenebiliyordu *_Layout.cshtml* dosyası: 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```
