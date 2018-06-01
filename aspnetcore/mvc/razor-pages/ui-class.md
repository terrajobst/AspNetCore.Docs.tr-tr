---
title: Yeniden kullanılabilir Razor kullanıcı Arabiriminde sınıf kitaplıkları ile ASP.NET çekirdek
author: Rick-Anderson
description: Bir sınıf kitaplığı'nda yeniden kullanılabilir Razor kullanıcı Arabirimi oluşturma açıklanmaktadır.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: b6658186adf5ccd74e1d0f0e925627f50bad250c
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687557"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Yeniden kullanılabilir kullanıcı Arabirimi kullanarak ASP.NET Core Razor sınıf kitaplığı projesi oluşturun.

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor görünümleri, sayfalar, denetleyicileri, sayfa modelleri [görüntülemek bileşenleri](xref:mvc/views/view-components), ve veri modelleri bir Razor sınıf kitaplığı (RCL) içine oluşturulabilir. RCL paketlenir ve yeniden kullanılabilir. Uygulamalar görünümler ve içerdiği sayfaları geçersiz kılmak ve RCL içerir. Ne zaman görünümü, kısmi görünümü veya Razor sayfasını web uygulaması ve Razor biçimlendirme RCL bulunduğunda (*.cshtml* dosyası) web uygulaması önceliklidir.

Bu özellik gerektirir [!INCLUDE[](~/includes/2.1-SDK.md)]

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Razor UI içeren bir sınıf kitaplığı oluşturun

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* Seçin **Razor sınıf kitaplığı** > **Tamam**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırı ' çalıştırma `dotnet new razorclasslib`. Örneğin:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Daha fazla bilgi için bkz: [dotnet yeni](/dotnet/core/tools/dotnet-new).

------
Razor dosyaları RCL ekleyin.

İçinde içerik Git RCL öneririz *alanları* klasör. 


## <a name="referencing-razor-class-library-content"></a>Razor sınıf kitaplığı içerik başvurma

RCL tarafından başvurulabilir:

* NuGet paketi. Bkz: [oluşturma NuGet paketlerini](/nuget/create-packages/creating-a-package) ve [dotnet paket ekleme](/dotnet/core/tools/dotnet-add-package) ve [oluşturma ve bir NuGet Paketi Yayımlama](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName} .csproj*. Bkz: [dotnet-Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference).

### <a name="partial-files-access-in-the-rcl"></a>RCL kısmi dosya erişimi

RCL dışında içerik için ASP.NET çekirdeği çalışma zamanı RCL kısmi dosyalarında aramaz.

Örneğin, örnek indirme içinde *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görüntüleyebilecek **değil** başvuruda bulunulamıyor *WebApp1\Pages\About.cshtml* . Ancak, RCL sayfaları ( *RazorUIClassLib /* **için** erişim *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>İzlenecek yol: bir Razor sınıf kitaplığı proje oluşturma ve Razor sayfalarının projeden kullanma

İndirebilirsiniz [tam projesini](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ve yerine bunu oluşturmayı sınayın. Örnek indirme ek kod ve projeyi test kolaylaştırmak bağlantılar içerir. Geri bildirim bırakabilirsiniz [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6098) yorumlarınızı adım adım yönergeler karşı indirme örnekleri üzerinde ile.

### <a name="test-the-download-app"></a>İndirme uygulamayı test etme

Tamamlanan uygulama indirilen henüz ve gözden geçirme proje yerine oluşturacak, geçin [sonraki bölümde](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Açık *.sln* dosyasını Visual Studio'da. Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Bir komut isteminden *CLI* dizin RCL oluşturmak ve web uygulaması.

``` CLI
dotnet build
```

Taşıma *WebApp1* dizin ve uygulamayı çalıştırın:

``` CLI
dotnet run
```
------

' Ndaki yönergeleri izleyin [Test WebApp1](#test)

## <a name="create-a-razor-class-library"></a>Bir Razor sınıf kitaplığı oluşturun

Bu bölümde, bir Razor sınıf kitaplığı (RCL) oluşturulur. Razor dosyaları RCL eklenir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

RCL projesi oluşturun:

* Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* Uygulama adı **RazorUIClassLib**.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* Seçin **Razor sınıf kitaplığı** > **Tamam**.

Razor sayfalarının web uygulaması oluşturun:

* Gelen **Çözüm Gezgini**, çözüme sağ tıklayın > **Ekle** >  **yeni proje**.
* Seçin **ASP.NET Core Web uygulaması**.
* Uygulama adı **WebApp1**.
* Doğrulama **ASP.NET Core 2.1** veya daha sonra seçilir.
* Seçin **Web uygulaması** > **Tamam**.

### <a name="add-razor-files-and-folders-to-the-project"></a>Razor dosyaları ve klasörleri projeye ekleyin.

* Adlı bir Razor kısmi görünüm dosya eklemek *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.
* Biçimlendirme Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Kopya *_ViewStart.cshtml* WebApp1 proje dosyasından *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.

  [Viewstart](xref:mvc/views/layout#running-code-before-each-view) dosya Razor sayfalarının proje düzeni kullanmak için gereklidir.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut satırından aşağıdakileri çalıştırın:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Yukarıdaki komutlar:

* Oluşturur `RazorUIClassLib` Razor sınıf kitaplığı (RCL).
* Bir Razor _Message sayfası oluşturur ve RCL ekler. `-np` Parametresi oluşturur sayfa olmadan bir `PageModel`.
* Oluşturur bir [viewstart](xref:mvc/views/layout#running-code-before-each-view) dosya ve RCL ekler.

Viewstart dosyası (sonraki bölümde eklenir) Razor sayfalarının projesi düzenini kullanmak için gereklidir.

Razor sayfalarının güncelleştirin:

* Biçimlendirme Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* aşağıdaki kod ile:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Biçimlendirme Değiştir *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* aşağıdaki kod ile:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Kısmi görünümü kullanmak için gereklidir (`<partial name="_Message" />`). Dahil olmak üzere yerine `@addTagHelper` yönergesi ekleyebileceğiniz bir *_viewımports.cshtml* dosya. Örneğin:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Viewimports hakkında daha fazla bilgi için bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives)

* Derleyici hatası olmadığını doğrulamak için sınıf kitaplığı oluşturun:

``` CLI
dotnet build RazorUIClassLib
```

Derleme çıktı içeren *RazorUIClassLib.dll* ve *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* derlenmiş Razor içeriği içerir.

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Bir Razor sayfalarının projeden Razor kullanıcı Arabirimi kitaplığı kullanma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Çözüm Gezgini**, sağ tıklayın **WebApp1** seçip **başlangıç projesi olarak ayarla**.
* Gelen **Çözüm Gezgini**, sağ tıklayın **WebApp1** seçip **derleme bağımlılıkları** > **proje bağımlılıkları**.
* Denetleme **RazorUIClassLib** bir bağımlılık olarak **WebApp1**.
* Gelen **Çözüm Gezgini**, sağ tıklayın **WebApp1** seçip **Ekle** > **başvuru**.
* İçinde **başvuru Yöneticisi** iletişim kutusunda, onay **RazorUIClassLib** > **Tamam**.

Uygulamayı çalıştırın.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Razor sayfalarının web app ve Razor sayfalarının uygulama ve Razor sınıf kitaplığı içeren bir çözüm dosyasını oluşturun:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Derleme ve web uygulaması çalıştırın:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>Test WebApp1

Razor UI sınıf kitaplığı kullanılan doğrulayın.

* Gözat `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Görünümleri, kısmi görünümleri ve sayfaları geçersiz kıl

Ne zaman görünümü, kısmi görünümü veya Razor sayfasını bulundu hem web uygulamasını hem de Razor sınıf kitaplığı, Razor biçimlendirme (*.cshtml* dosyası) web uygulaması önceliklidir. Örneğin, ekleyin *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* WebApp1 için ve WebApp1 Page1 sürer öncelik Page1in Razor sınıf kitaplığı.

Örnek indirme yeniden adlandırma *alanları/WebApp1/MyFeature2* için *alanları/WebApp1/MyFeature* öncelik test etmek için.

Kopya *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* kısmi görünüme *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Yeni konumu belirtmek için biçimlendirme güncelleştirin. Derleme ve kısmi uygulamanın sürümü kullanılan doğrulamak üzere uygulamasını çalıştırın.
