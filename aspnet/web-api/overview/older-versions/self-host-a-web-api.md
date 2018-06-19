---
uid: web-api/overview/older-versions/self-host-a-web-api
title: ASP.NET Web API 1 (C#) kendini barındırma | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API IIS gerektirmez. Kendi ana bilgisayar işlemi otomatik olarak bir web API barındırabilir. Bu öğretici, bir konsol applic içinde bir web API barındırmak gösterilmektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043359"
---
<a name="self-host-aspnet-web-api-1-c"></a>ASP.NET Web API 1 (C#) kendini barındırma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> ASP.NET Web API IIS gerektirmez. Kendi ana bilgisayar işlemi otomatik olarak bir web API barındırabilir. Bu öğretici, bir konsol uygulaması içindeki bir web API barındırmak gösterilmiştir.
> 
> **Yeni uygulamalar OWIN Web API Self barındırmak için kullanmanız gerekir.** Bkz: [OWIN ASP.NET Web API 2 Self barındırmak için kullandığınız](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Konsol uygulama projesi oluşturma

Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası. Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Windows**. Proje şablonları listesinde seçin **konsol uygulaması**. Proje adı &quot;SelfHost&quot; tıklatıp **Tamam**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Hedef Framework'ü (Visual Studio 2010) ayarlayın

Visual Studio 2010 kullanıyorsanız, .NET Framework 4.0 hedef çerçevesini değiştirebilir. (Varsayılan olarak, proje şablonu hedefleri [.Net Framework istemci profili](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **özellikleri**. İçinde **hedef framework** açılır listesinde, .NET Framework 4.0 hedef çerçevesini değiştirebilir. Değişikliği uygulamak için istendiğinde tıklatın **Evet**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>NuGet Paket Yöneticisi'ni yükleyin

NuGet Paket Yöneticisi, Web API derlemeleri ASP.NET olmayan projeye eklemek için kolay bir yoludur.

NuGet Paket Yöneticisi yüklü olup olmadığını denetlemek için **Araçları** Visual Studio menüsünde. Öğe adı verilen bir menü görürseniz **kitaplık Paket Yöneticisi**, NuGet Paket Yöneticisi sahip.

NuGet Paket Yöneticisi'ni yüklemek için:

1. Visual Studio'yu başlatın.
2. Gelen **Araçları** menüsünde, select **Uzantılar ve güncelleştirmeler**.
3. İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda **çevrimiçi**.
4. "NuGet Paket Yöneticisi" göremiyorsanız, arama kutusuna "nuget Paket Yöneticisi" yazın.
5. NuGet Paket Yöneticisi'ni seçin ve tıklatın **karşıdan**.
6. İndirme tamamlandıktan sonra yüklemeniz istenir.
7. Yükleme tamamlandıktan sonra Visual Studio yeniden başlatmanız istenebilir.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Web API NuGet paketi ekleme

NuGet Paket Yöneticisi yüklendikten sonra Web API Self-Host paketini projenize ekleyin.

1. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**. *Not*: Bu menü öğesi, bu NuGet Paket Yöneticisi'nin yüklendiğinden emin olun görmezseniz.
2. Seçin **çözüm için NuGet paketlerini Yönet...**
3. İçinde **NugGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.
4. Arama kutusuna &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. ASP.NET Web API Self Host paketi seçin ve **yükleme**.
6. Paket yüklendikten sonra tıklatın **kapatmak** iletişim kutusunu kapatmak için.

> [!NOTE]
> Microsoft.AspNet.WebApi.SelfHost, değil AspNetWebApi.SelfHost adlı paketini yüklediğinizden emin olun.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Model ve denetleyici oluşturma

Bu öğretici olarak aynı model ve denetleyici sınıfları kullanır [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Öğreticisi.

Adlı genel bir sınıf ekleyin `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Adlı genel bir sınıf ekleyin `ProductsController`. Bu sınıftan türeyen **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Bu denetleyici kodda hakkında daha fazla bilgi için bkz: [Başlarken](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Öğreticisi. Bu denetleyici üç GET eylemleri tanımlar:

| URI | Açıklama |
| --- | --- |
| / api/ürünleri | Tüm ürünlerin listesini alın. |
| /api/products/*id* | Ürün Kimliği tarafından Al |
| /api/products/?category=*category* | Kategoriye göre ürünlerin listesini alın. |

## <a name="host-the-web-api"></a>Web API ana bilgisayar

Program.cs dosyasını açın ve aşağıdaki using deyimlerini:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Aşağıdaki kodu ekleyin **Program** sınıfı.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(İsteğe bağlı) Bir HTTP URL'si Namespace ayırması ekleme

Bu uygulama dinler `http://localhost:8080/`. Varsayılan olarak, belirli bir HTTP adreste dinleme yönetici ayrıcalıkları gerekiyor. Öğretici çalıştırdığınızda, bu nedenle, bu hatayı alabilirsiniz: "HTTP URL http://+:8080/ kaydedemedi" Bu hatayı önlemek için iki yolu vardır:

- Visual Studio yükseltilmiş yönetici izinleriyle çalışmasını veya
- URL ayırmak için hesap izinlerinize vermek için Netsh.exe kullanın.

Netsh.exe kullanmak için yönetici ayrıcalıklarıyla bir komut istemi açın ve aşağıdaki komutu: aşağıdaki komutu girin:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

Burada *Makine\kullanıcı adı* kullanıcı hesabıdır.

Kendi kendine barındırma tamamladığınızda, ayırmayı silmek emin olun:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Web API çağrısı bir istemci uygulamadan (C#)

Web API'si çağıran basit bir konsol uygulaması yazalım.

Yeni bir konsol uygulama projesi ekleyin:

- Çözüm Gezgini'nde çözüme sağ tıklayın ve seçin **Yeni Proje Ekle**.
- Adlı yeni bir konsol uygulaması oluşturun &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

NuGet paket ASP.NET Web API Core Libraries paketini eklemek için Yöneticisi'ni kullanın:

- Araçlar menüsünden seçin **kitaplık Paket Yöneticisi**.
- Seçin **çözüm için NuGet paketlerini Yönet...**
- İçinde **NuGet paketlerini Yönet** iletişim kutusunda **çevrimiçi**.
- Arama kutusuna &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Microsoft ASP.NET Web API Client Libraries paketi seçin ve **yükleme**.

Bir başvuru ClientApp SelfHost projeye ekleyin:

- Çözüm Gezgini'nde ClientApp projesine sağ tıklayın.
- Seçin **başvuru ekleme**.
- İçinde **başvuru Yöneticisi** iletişim altında **çözüm**seçin **projeleri**.
- SelfHost projesini seçin.
- **Tamam**'ı tıklatın.

![](self-host-a-web-api/_static/image6.png)

Client/Program.cs dosyasını açın. Aşağıdakileri ekleyin **kullanarak** deyimi:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Statik eklemek **HttpClient** örneği:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Tüm ürünler, liste Kimliğine göre bir ürün ve liste ürünler kategoriye göre listelemek için aşağıdaki yöntemleri ekleyin.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Bu yöntemlerin her biri aynı düzeni izler:

1. Çağrı **HttpClient.GetAsync** uygun URI GET isteğini göndermek için.
2. Çağrı **HttpResponseMessage.EnsureSuccessStatusCode**. HTTP yanıt durumu hata kodu ise, bu yöntem bir özel durum oluşturur.
3. Çağrı **ReadAsAsync&lt;T&gt;**  HTTP yanıtı bir CLR türü seri durumdan çıkarılamadı. Tanımlanan bir genişletme yöntemi, bu yöntemdir **System.Net.Http.HttpContentExtensions**.

**GetAsync** ve **ReadAsAsync** yöntemleridir her ikisi de zaman uyumsuz. Döndürmeleri **görev** zaman uyumsuz işlemi temsil eden nesne. Alma **sonuç** özelliği işlemi tamamlanana kadar iş parçacığı engeller.

Engelleyici olmayan aramalarda de dahil olmak üzere HttpClient kullanma hakkında daha fazla bilgi için bkz: [bir Web API öğesinden bir .NET istemci çağırma](../advanced/calling-a-web-api-from-a-net-client.md).

Bu yöntemleri çağrılmadan önce BaseAddress özelliğini HttpClient örneğine ayarlayın "`http://localhost:8080`". Örneğin:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Bu aşağıdaki çıktı. (İlk SelfHost uygulamayı çalıştırmak unutmayın.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
