---
title: ASP.NET Core Razor sayfaları kullanmaya başlama
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Bu öğretici serisinde, ASP.NET Core Razor sayfaları kullanma işlemi gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturmak, veri erişimi için Entity Framework Core ve SQL Server kullanmak, arama işlevi eklemek, giriş doğrulaması eklemek ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861634"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlayın

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.

Uygulama bir veritabanı başlık yönetir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Razor sayfaları web uygulaması oluşturun.
> * Ekleme ve bir modeli iskelesini.
> * Bir veritabanı ile çalışır.
> * Arama ve doğrulama ekleyin.

Sonunda, yönetebilir ve film başlıkları öğeleri görüntülemek bir uygulamaya sahip.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>Razor web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Yeni bir ASP.NET Core Web uygulaması oluşturun. Projeyi adlandırın **RazorPagesMovie**. Projeyi adlandırın önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod olduğunda eşleşecek şekilde.
 ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  Aşağıdaki başlangıç projesini oluşturulur:

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

* Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.

  Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır. Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart yerel bilgisayar adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür. Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır. Yukarıdaki görüntüde, 5001 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

  Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar. Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje içeren bir klasör.
* Şu komutu çalıştırın:

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'RazorPagesMovie' eksik. Bunları eklensin mi?**  Seçin **Evet**

  * `dotnet new webapp -o RazorPagesMovie`: yeni bir Razor sayfaları projesindeki oluşturur *RazorPagesMovie* klasör.
  * `code -r RazorPagesMovie`: Yükleyen *RazorPagesMovie.csproj* proje dosyası Visual Studio code'da.

### <a name="launch-the-app"></a>Uygulamayı başlatın

* Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.

  Visual Studio Code başlatıldığında başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`. Adres çubuğu gösterir `localhost:port:5001` gibi bir şey `example.com`. Çünkü `localhost` standart yerel bilgisayar adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.

  Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar. Geliştiricilerin çoğu, sayfayı yenileyin ve değişiklikleri görüntülemek için hata ayıklama olmayan modu kullanmayı tercih eder.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Bir terminalde aşağıdaki komutları çalıştırın:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) oluşturup bir Razor sayfaları projeyi çalıştırın. Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.

## <a name="open-the-project"></a>Projeyi açın

Uygulamayı kapatmak için CTRL + C tuşlarına basın.

Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.

### <a name="launch-the-app"></a>Uygulamayı başlatın

Seçin **çalıştırın > hata ayıklama olmadan Başlat** uygulamayı başlatın. Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.

<!-- End of VS tabs -->

---

* Seçin **kabul** izleme için onay verme. Bu uygulama, kişisel bilgi izlemez. Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>Proje dosyaları ve klasörleri

Aşağıdaki tabloda, proje klasörleri ve dosyaları listeler. Öğreticinin bu noktasında *Startup.cs* dosyasıdır anlamak en önemli. Aşağıda sağlanan her bir bağlantısını incelemeniz gerek yoktur. Bir dosya veya klasör proje hakkında daha fazla bilgi gerektiğinde bağlantılar bir başvuru olarak sağlanır.

| Dosya veya klasör              | Amaç |
| ----------------- | ------------ |
| *wwwroot* | Statik dosyaları içerir. Bkz: [statik dosyalar](xref:fundamentals/static-files). |
| *Sayfalar* | Klasör [Razor sayfaları](xref:razor-pages/index). |
| *appsettings.json* | [Yapılandırma](xref:fundamentals/configuration/index) |
| *Program.cs* | [Konaklar](xref:fundamentals/host/index) ASP.NET Core uygulaması.|
| *Startup.cs* | Hizmetler ve istek ardışık düzenini yapılandırır. Bkz: [başlangıç](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Sayfalar klasöründe

*_Layout.cshtml* dosya ortak HTML öğeleri (betikleri ve stil sayfalarını) içerir ve uygulama düzenini ayarlar. Örneğin, tıkladığınızda **RazorPagesMovie**, **giriş**, veya **gizlilik**, aynı öğelere bakın. Ortak öğeler, üst ve alt pencerenin üst gezinti menüsünde içerir. Bkz: [Düzen](xref:mvc/views/layout) daha fazla bilgi için.

*_Viewımports.cshtml* dosyası her bir Razor sayfası alınan Razor yönergeleri içerir. Bkz: [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives) daha fazla bilgi için.

*_ViewStart.cshtml* Razor sayfaları ayarlar `Layout` kullanılacak özellik *_Layout.cshtml* dosya. Bkz: [Düzen](xref:mvc/views/layout) daha fazla bilgi için.

*_ValidationScriptsPartial.cshtml* dosyası bir başvuru sağlar [jQuery](https://jquery.com/) doğrulama komut. Zaman `Create` ve `Edit` sayfaları öğreticide daha sonra eklenen *_ValidationScriptsPartial.cshtml* dosya kullanılır.

`Index`, `Error`, ve `Privacy` sayfaları için sağlanır:

* `Index`: Bir uygulamayı başlatın.
* `Error`: Hata bilgileri görüntüler.
* `Privacy`: Sitenin gizlilik ilkesi hakkındaki ayrıntıları belirtin.

Bu öğreticide, önceki sayfaları kullanılmaz.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>Bir Razor sayfası ve PageModel arasında geçiş yapmak için F7 kullanın

F7 bir Razor sayfası arasında geçiş yapar (*\*.cshtml* dosyası) ve C# dosyası (*\*. cshtml.cs*).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

Kural olarak, Razor sayfası (*\*.cshtml* dosyası) ve ilişkili `PageModel` aynı kök dosya adını içerir.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Kural olarak, Razor sayfası (*\*.cshtml* dosyası) ve ilişkili `PageModel` aynı kök dosya adını içerir.

---

> [!div class="step-by-step"]
> [Sonraki: model ekleme](xref:tutorials/razor-pages/model)