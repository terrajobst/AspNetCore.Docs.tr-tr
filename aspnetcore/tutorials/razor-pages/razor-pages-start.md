---
title: 'Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama'
author: rick-anderson
description: Bu öğretici serisinde, ASP.NET Core Razor sayfaları kullanma işlemi gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturmak, veri erişimi için Entity Framework Core ve SQL Server kullanmak, arama işlevi eklemek, giriş doğrulaması eklemek ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d264ca4a605d8291e273a8f054c92e7eefa5548
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903296"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu, bir serinin ilk öğreticidir. [Serinin](xref:tutorials/razor-pages/index) bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.

[!INCLUDE[](~/includes/advancedRP.md)]

Serinin sonunda bir film veritabanı yöneten bir uygulaması oluşturmuş olacaksınız.  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Razor sayfaları web uygulaması oluşturun.
> * Uygulamayı çalıştırın.
> * Proje dosyalarını inceleyin.

Bu öğreticinin sonunda çalışan bir sonraki öğreticilerde oluşturacağınız Razor sayfaları web uygulaması gerekir.

![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a>Razor sayfaları web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.

* Yeni bir ASP.NET Core Web uygulaması oluşturun. Projeyi adlandırın **RazorPagesMovie**. Projeyi adlandırın önemlidir *RazorPagesMovie* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)

* Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.

  ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.2.png)

  Aşağıdaki başlangıç projesini oluşturulur:

  ![Çözüm Gezgini](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Dizinleri (`cd`) proje içeren bir klasör.

* Aşağıdaki komutları çalıştırın:

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * `dotnet new` Komut yeni bir Razor sayfaları projesindeki oluşturur *RazorPagesMovie* klasör.
  * `code` Komutu açılır *RazorPagesMovie* Visual Studio Code yeni bir örneğini klasöründe.

  Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'RazorPagesMovie' eksik. Bunları eklensin mi?**

* Seçin **Evet**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Bir terminalde aşağıdaki komutu çalıştırın:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) Razor sayfaları projesi oluşturmak için.

## <a name="open-the-project"></a>Projeyi açın

Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Hata Ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır. Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart yerel bilgisayar adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür. Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.

* Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.

  Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* Tuşuna **Ctrl-F5** hata ayıklayıcı olmadan çalıştırılacak.

  Visual Studio Code başlar [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`. Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`. Çünkü `localhost` standart yerel bilgisayar adıdır. Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.

* Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.

  Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2.png)

  İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* Tuşuna **Cmd iyileştirilmiş F5** hata ayıklayıcı olmadan çalıştırılacak.

  Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5001`.

* Uygulamanın giriş sayfasında, seçin **kabul** izleme için onay verme.

  Bu uygulama, kişisel bilgi izlemez ancak Avrupa Birliği'ile nin uymak için ihtiyaç durumunda proje şablonu, onay özelliği içerir. [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR2.2_safari.png)

  İzleme için onay verdikten sonra aşağıdaki görüntüde uygulama gösterilir:

  ![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a>Proje dosyalarını inceleyin

Sonraki öğreticilerde ile çalışacaksınız dosyaları ve klasörleri ana proje genel bakış aşağıda verilmiştir.

### <a name="pages-folder"></a>Sayfaları klasörü

Razor sayfaları ve Destek dosyalarını içerir. Her bir Razor sayfası dosyalarının bir çiftini şöyledir:

* A *.cshtml* ile HTML biçimlendirmesini içeren dosya C# Razor söz dizimini kullanarak kodu.
* A *. cshtml.cs* içeren dosya C# sayfası olayları işleyen kodu.

Destekleyici dosyaları bir alt çizgi ile başlayan adları vardır. Örneğin, *_Layout.cshtml* dosyası tüm sayfalar için ortak kullanıcı Arabirimi öğeleri yapılandırır. Bu dosya sayfanın üst gezinti menüsünde ve sayfanın alt kısmındaki telif hakkı bildirimi ayarlar. Daha fazla bilgi için bkz. <xref:mvc/views/layout>.

### <a name="wwwroot-folder"></a>wwwroot klasörü

HTML dosyaları, JavaScript dosyaları ve CSS dosyaları gibi statik dosyalar içerir. Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.

### <a name="appsettingsjson"></a>appSettings.json

Bağlantı dizeleri gibi yapılandırma verilerini içerir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

### <a name="programcs"></a>Program.cs

Programın giriş noktası içerir. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.

### <a name="startupcs"></a>Startup.cs

Olup olmadığı için tanımlama bilgileri onayı gerektirir gibi uygulama davranışını yapılandıran kodunu içerir. Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticide YouTube sürümü](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Razor sayfaları web uygulaması oluşturdunuz.
> * Bir uygulamayı çalıştırdınız.
> * Proje dosyalarını incelenir.

Serinin sonraki öğreticiye ilerleyin:

> [!div class="step-by-step"]
> [Model ekleme](xref:tutorials/razor-pages/model)
