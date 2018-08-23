---
uid: web-api/overview/older-versions/self-host-a-web-api
title: ASP.NET Web API 1 (C#) barındırma | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API, IIS gerektirmez. Web API'si kendi ana bilgisayar işlemi kendi kendine barındırabilirsiniz. Bu öğreticide, bir konsolu applic içinde bir web API'sini barındıran gösterilmektedir...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: cac0d5aeaf49f45051d062935e0e9207ce59c7eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756147"
---
<a name="self-host-aspnet-web-api-1-c"></a>Barındırılan ASP.NET Web API 1 (C#)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API, IIS gerektirmez. Web API'si kendi ana bilgisayar işlemi kendi kendine barındırabilirsiniz. Bu öğreticide, bir konsol uygulaması içinde bir web API'sini barındıran gösterilmektedir.
> 
> **Yeni uygulama, barındırılan Web API'si için OWIN kullanmanız gerekir.** Bkz: [OWIN, ASP.NET Web API 2 barındırma kullanmasını](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Konsol uygulama projesi oluşturma

Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası. Veya **dosya** menüsünde **yeni** ardından **proje**.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Windows**. Proje şablonları listesinde seçin **konsol uygulaması**. Projeyi adlandırın &quot;SelfHost&quot; tıklatıp **Tamam**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Hedef Framework'ü (Visual Studio 2010) ayarlama

Visual Studio 2010 kullanıyorsanız, hedef çerçeveyi .NET Framework 4.0 ile değiştirin. (Varsayılan olarak, proje şablonunun hedeflediği [.Net Framework istemci profili](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

Çözüm Gezgini'nde projeye sağ tıklayıp seçin **özellikleri**. İçinde **hedef Framework'ü** açılan listesinde, .NET Framework 4.0 için hedef Framework'ü değiştirin. Değişikliği uygulamak için sorulduğunda **Evet**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>NuGet Paket Yöneticisi'ni yükleyin

NuGet Paket Yöneticisi, bir ASP.NET olmayan projeye Web API derlemeleri eklemek için en kolay yoludur.

NuGet Paket Yöneticisi'nin yüklü olup olmadığını denetlemek için **Araçları** Visual Studio'daki menü. Bir menü öğesi adlı görürseniz **kitaplık Paket Yöneticisi**, NuGet Paket Yöneticisi sahip.

NuGet Paket Yöneticisi'ni yüklemek için:

1. Visual Studio'yu başlatın.
2. Gelen **Araçları** menüsünde **Uzantılar ve güncelleştirmeler**.
3. İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda **çevrimiçi**.
4. "Nuget Paket Yöneticisi", "NuGet Paket Yöneticisi" ifadesini görmüyorsanız arama kutusuna yazın.
5. NuGet Paket Yöneticisi'ni seçip tıklayın **indirme**.
6. İndirme tamamlandıktan sonra yüklemeniz istenir.
7. Yükleme tamamlandıktan sonra Visual Studio'yu yeniden başlatmanız istenebilir.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Web API NuGet paketi ekleme

NuGet Paket Yöneticisi'ni yükledikten sonra Web API Self-Host paketini projenize ekleyin.

1. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**. *Not*: Bu menü öğesi, bu NuGet Paket Yöneticisi'nin yüklendiğinden emin olun görmüyorsanız.
2. Seçin **çözüm için NuGet paketlerini Yönet...**
3. İçinde **NugGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.
4. Arama kutusuna &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. ASP.NET Web API Self konağı paketi seçin ve tıklayın **yükleme**.
6. Paket yüklendikten sonra tıklayın **kapatmak** iletişim kutusunu kapatmak için.

> [!NOTE]
> Microsoft.AspNet.WebApi.SelfHost, değil AspNetWebApi.SelfHost adlı paketi yüklediğinizden emin olun.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Model ve denetleyici oluşturma

Bu öğreticide aynı model ve denetleyici sınıflar olarak [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) öğretici.

Adlı bir ortak Sınıf Ekle `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Adlı bir ortak Sınıf Ekle `ProductsController`. Bu sınıftan türetilen **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Bu denetleyicideki kod hakkında daha fazla bilgi için bkz. [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) öğretici. Bu denetleyici üç GET eylemleri tanımlar:

| URI | Açıklama |
| --- | --- |
| / api/ürünleri | Tüm ürünlerin listesini alın. |
| /API'si/ürünler/*kimliği* | Bir ürün kimliğine göre Al |
| / API'si/ürünler /? kategori =*kategorisi* | Kategoriye göre ürünlerin listesini alın. |

## <a name="host-the-web-api"></a>Web API'si barındırın

Program.cs dosyasını açın ve aşağıdaki using deyimlerini:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Aşağıdaki kodu ekleyin **Program** sınıfı.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(İsteğe bağlı) Bir HTTP URL'si Namespace ayırma ekleyin

Bu uygulamasının dinleyeceği `http://localhost:8080/`. Varsayılan olarak, belirli bir HTTP adreste dinleme yönetici ayrıcalıkları gerektirir. Öğreticiyi çalıştırdığınızda, bu nedenle, bu hatayı alabilirsiniz: "HTTP URL'si kaydedemedi http://+:8080/" Bu hatayı önlemek için iki yolu vardır:

- Visual Studio'yu yönetici izinleriyle çalıştırın veya
- Netsh.exe URL ayırmak için hesap izinlerinize vermek için kullanın.

Netsh.exe kullanmak için yönetici ayrıcalıklarıyla bir komut istemi açın ve aşağıdaki komutu: aşağıdaki komutu girin:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

Burada *Makine\kullanıcı* kullanıcı hesabıdır.

İşiniz bittiğinde kendi kendine barındırma, rezervasyon sildiğinizden emin olun:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Bir istemci uygulamasından (C#) Web API'si çağırma

Şimdi web API'si çağıran basit bir konsol uygulaması yazma.

Yeni bir konsol uygulaması projesi çözüme ekleyin:

- Çözüm Gezgini'nde çözüme sağ tıklayıp seçin **Yeni Proje Ekle**.
- Adlı yeni bir konsol uygulaması oluşturun &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

NuGet paketi ASP.NET Web API çekirdek kitaplıkları paketini eklemek için Yöneticisi'ni kullanın:

- Araçlar menüsü'nden seçin **kitaplık Paket Yöneticisi**.
- Seçin **çözüm için NuGet paketlerini Yönet...**
- İçinde **NuGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.
- Arama kutusuna &quot;System.NET.http.Formatting&quot;.
- Microsoft ASP.NET Web API İstemci Kitaplığı paketi seçin ve tıklayın **yükleme**.

Bir başvuru ClientApp SelfHost projeye ekleyin:

- Çözüm Gezgini'nde ClientApp projeye sağ tıklayın.
- Seçin **Başvurusu Ekle**.
- İçinde **başvuru Yöneticisi** iletişim altında **çözüm**seçin **projeleri**.
- SelfHost projeyi seçin.
- **Tamam**'ı tıklatın.

![](self-host-a-web-api/_static/image6.png)

Client/Program.cs dosyasını açın. Aşağıdaki **kullanarak** deyimi:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Statik bir ekleme **HttpClient** örneği:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Tüm ürünler, bir ürün Kimliğine göre listesi ve ürünleri listeler, kategoriye göre listelemek için aşağıdaki yöntemleri ekleyin.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Bu yöntemlerin her biri aynı deseni izler:

1. Çağrı **HttpClient.GetAsync** uygun URI'ye bir GET isteği göndermek için.
2. Çağrı **HttpResponseMessage.EnsureSuccessStatusCode**. HTTP yanıtı durum bir hata kodu ise bu yöntem bir özel durum oluşturur.
3. Çağrı **ReadAsAsync&lt;T&gt;**  HTTP yanıtı gelen bir CLR türü seri durumdan çıkarılacak. Bu yöntem içinde tanımlanan bir genişletme yöntemi olduğunu **System.Net.Http.HttpContentExtensions**.

**GetAsync** ve **ReadAsAsync** hem de zaman uyumsuz yöntemler şunlardır. Döndürmeleri **görev** zaman uyumsuz işlemi temsil eden nesneleri. Başlama **sonucu** özelliği işlem tamamlanana kadar iş parçacığını engeller.

HttpClient, engellemeyen çağrı yapmak nasıl dahil olmak üzere kullanma hakkında daha fazla bilgi için bkz. [bir Web API'si bir .NET istemcinin çağırma](../advanced/calling-a-web-api-from-a-net-client.md).

Bu yöntemleri çağrılmadan önce BaseAddress özelliği için HttpClient örneği üzerinde ayarlanmış "`http://localhost:8080`". Örneğin:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Bu aşağıdaki çıkışı oluşturmalıdır. (İlk SelfHost uygulamayı çalıştırmak unutmayın.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
