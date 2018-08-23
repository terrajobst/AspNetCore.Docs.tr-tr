---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: ASP.NET Web API 2 Azure çalışan rolünde konak | Microsoft Docs
author: MikeWasson
description: Bu öğreticide bir Azure çalışan rolünde ASP.NET Web API'yi barındırmak nasıl OWIN barındırma Web API çerçevesi kullanmayı gösterir. Web arabirimi için .NET (OWIN) de Aç...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: cabf88e4e6c946f92a9e4534a4db5ae15dd8cae5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753599"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>ASP.NET Web API 2 Azure çalışan rolünde barındırın
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Bu öğreticide bir Azure çalışan rolünde ASP.NET Web API'yi barındırmak nasıl OWIN barındırma Web API çerçevesi kullanmayı gösterir.
> 
> [.NET için açık Web arabirimi](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar. OWIN ayırır OWIN kendi işleminizde IIS dışında bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucu web uygulamasından – Örneğin, bir Azure çalışan rolü içinde.
> 
> Bu öğreticide, Microsoft.Owin.Host.HttpListener paket kullanacağınız, bir HTTP sunucusu sağlayan Self OWIN uygulamalarını barındırmak için kullanılabilir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - [.NET 2.3 için Azure SDK](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Microsoft Azure projesi oluşturma

Visual Studio'yu yönetici ayrıcalıklarıyla başlatın. Azure işlem öykünücüsü kullanarak uygulamayı yerel olarak hata ayıklama için yönetici ayrıcalıkları gerekir.

Üzerinde **dosya** menüsünde tıklayın **yeni**, ardından **proje**. Gelen **yüklü şablonlar**, Visual C# altında tıklayın **bulut** ve ardından **Windows Azure bulut hizmeti**. "AzureApp" Projeyi adlandırın ve tıklayın **Tamam**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, çift **çalışan rolü**. ("WorkerRole1") varsayılan adı bırakın. Bu adım, bir çalışan rolü çözüme ekler. **Tamam**'ı tıklatın.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Oluşturulan Visual Studio çözümünü iki proje içerir:

- &quot;AzureApp&quot; Azure uygulaması için yapılandırma ve rolleri tanımlar.
- &quot;WorkerRole1&quot; çalışan rolü kodunu içerir.

Genel olarak, bu öğreticide tek bir rol olsa da Azure uygulaması birden çok rol içerebilir.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API ve OWIN paketleri Ekle

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.

Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Bir HTTP uç noktası ekleme

Çözüm Gezgini'nde AzureApp projeyi genişletin. Rolleri düğümünü genişletin, WorkerRole1 sağ tıklatın ve seçin **özellikleri**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Tıklayın **uç noktaları**ve ardından **uç nokta Ekle**.

İçinde **Protokolü** açılan listeyi seçin "http". İçinde **genel bağlantı noktası** ve **özel bağlantı noktası**, 80 yazın. Bu bağlantı noktası numaraları farklı olabilir. Role bir isteği gönderdiğinizde istemciler kullandıklarınız genel bağlantı noktasıdır.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Web API'si için yapılandırma barındırma

Çözüm Gezgini'nde WorkerRole1 projeyi sağ tıklatın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Web API denetleyici ekleme

Ardından, bir Web API denetleyici sınıfı ekleyin. WorkerRole1 projeye sağ tıklayıp **Ekle** / **sınıfı**. TestController sınıfı adı. Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Kolaylık olması için bu denetleyici yalnızca düz metin döndürür iki GET yöntemleri tanımlar.

## <a name="start-the-owin-host"></a>OWIN konağını Başlat

WorkerRole.cs dosyasını açın. Bu sınıf, çalışan rolü başlatıldığında ve durdurulduğunda çalışan kodu tanımlar.

Aşağıdaki using deyimi:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Ekleme bir **IDisposable** üyesine `WorkerRole` sınıfı:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

İçinde `OnStart` yöntemi, ana bilgisayarı başlatmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start** yöntemi OWIN ana bilgisayarı başlatır. Adını `Startup` yöntem bir tür parametresine bir sınıftır. Kural gereği, konak çağıracak `Configure` bu sınıfın yöntemi.

Geçersiz kılma `OnStop` elden çıkarmak  *\_uygulama* örneği:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

WorkerRole.cs için tam kod aşağıdaki gibidir:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Çözümü derleyin ve uygulamayı Azure işlem öykünücüsü'nde yerel olarak çalıştırmak için F5 tuşuna basın. Güvenlik duvarı ayarlarınıza bağlı olarak, güvenlik duvarı üzerinden öykünücü izin gerekebilir.

> [!NOTE]
> Lütfen aşağıdaki gibi bir özel durum alırsanız bkz [bu blog gönderisini](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) geçici bir çözüm için. "Dosyası veya bütünleştirilmiş kod yüklenemedi ' Microsoft.Owin, sürüm 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' veya bağımlılıklarından biri. Bildirim tanımı bulunduğu derlemenin, derleme başvurusunu eşleşmiyor. (HRESULT özel durum: 0x80131040) "


İşlem öykünücüsü, uç nokta için bir yerel IP adresi atar. IP adresi, işlem öykünücüsü kullanıcı Arabiriminde görüntüleyerek bulabilirsiniz. Görev çubuğu bildirim alanında bulunan öykünücü simgesine sağ tıklayın ve seçin **Göster işlem öykünücüsü kullanıcı Arabiriminde**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

IP adresi hizmet dağıtımları, dağıtım [ID] hizmet ayrıntıları altında bulabilirsiniz. Bir web tarayıcısı açın ve http:// gidin<em>adresi</em>/test/1, burada <em>adresi</em> ; işlem öykünücüsü tarafından atanan IP adresi gibi `http://127.0.0.1:80/test/1`. Web API denetleyicisi yanıtı görmeniz gerekir:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adım için bir Azure hesabınızın olması gerekir. Zaten yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Microsoft Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Çözüm Gezgini'nde AzureApp projeye sağ tıklayın. Seçin **yayımlama**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Azure hesabınızda oturum açmadınız tıklatmak **oturum**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Oturum açtıktan sonra bir abonelik seçin ve tıklayın **sonraki**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Bulut hizmeti için bir ad girin ve bir bölge seçin. **Oluştur**'u tıklatın.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Tıklayın **yayımlama**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Azure Etkinlik Günlüğü penceresini dağıtımın ilerleme durumunu gösterir. Uygulama dağıtıldığında, Gözat http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [Project Katana’ya Genel Bakış](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Github'da Katana proje](https://github.com/aspnet/AspNetKatana)
