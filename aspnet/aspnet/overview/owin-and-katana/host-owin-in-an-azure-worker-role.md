---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Bir Azure çalışan rolünde OWIN barındırma | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, bir Microsoft Azure çalışan rolünde OWIN barındırma işlemi gösterilmektedir. Açık Web arabirimi için .NET (OWIN) .NET web sunucusu arasında bir Özet tanımlar...
ms.author: aspnetcontent
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: f62b9299a4e369ae3a938c85e60dd6a79108548d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826487"
---
<a name="host-owin-in-an-azure-worker-role"></a>Bir Azure çalışan rolünde OWIN barındırma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Bu öğreticide, bir Microsoft Azure çalışan rolünde OWIN barındırma işlemi gösterilmektedir.
> 
> [.NET için açık Web arabirimi](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar. OWIN ayırır OWIN kendi işleminizde IIS dışında bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucu web uygulamasından – Örneğin, bir Azure çalışan rolü içinde.
> 
> Bu öğreticide, bir Microsoft Azure çalışan rolü içinde bir OWIN uygulama barındırma öğreneceksiniz. Çalışan rolleri hakkında daha fazla bilgi için bkz: [Azure yürütme modelleri](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [.NET 2.3 için Azure SDK](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Microsoft Azure projesi oluşturma

Visual Studio'yu yönetici ayrıcalıklarıyla başlatın. Azure işlem öykünücüsü kullanarak uygulamayı yerel olarak hata ayıklama için yönetici ayrıcalıkları gerekir.

Üzerinde **dosya** menüsünde tıklayın **yeni**, ardından **proje**. Gelen **yüklü şablonlar**, Visual C# altında tıklayın **bulut** ve ardından **Windows Azure bulut hizmeti**. "AzureApp" Projeyi adlandırın ve tıklayın **Tamam**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, çift **çalışan rolü**. ("WorkerRole1") varsayılan adı bırakın. Bu adım, bir çalışan rolü çözüme ekler. **Tamam**'ı tıklatın.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Oluşturulan Visual Studio çözümünü iki proje içerir:

- &quot;AzureApp&quot; Azure uygulaması için yapılandırma ve rolleri tanımlar.
- &quot;WorkerRole1&quot; çalışan rolü kodunu içerir.

Genel olarak, bu öğreticide tek bir rol olsa da Azure uygulaması birden çok rol içerebilir.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>OWIN barındırma paketleri ekleme

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.

Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Bir HTTP uç noktası ekleme

Çözüm Gezgini'nde AzureApp projeyi genişletin. Rolleri düğümünü genişletin, WorkerRole1 sağ tıklatın ve seçin **özellikleri**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Tıklayın **uç noktaları**ve ardından **uç nokta Ekle**.

İçinde **Protokolü** açılan listeyi seçin "http". İçinde **genel bağlantı noktası** ve **özel bağlantı noktası**, 80 yazın. Bu bağlantı noktası numaraları farklı olabilir. Role bir isteği gönderdiğinizde istemciler kullandıklarınız genel bağlantı noktasıdır.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>OWIN başlangıç sınıfı oluşturma

Çözüm Gezgini'nde WorkerRole1 projeyi sağ tıklatın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `Startup`.

Tüm ortak kod aşağıdakiyle değiştirin:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage` Genişletme yöntemi, uygulamanıza site çalıştığından emin olmak için basit bir HTML sayfası ekler.

## <a name="start-the-owin-host"></a>OWIN konağını Başlat

WorkerRole.cs dosyasını açın. Bu sınıf, çalışan rolü başlatıldığında ve durdurulduğunda çalışan kodu tanımlar.

Aşağıdaki using deyimi:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Ekleme bir **IDisposable** üyesine `WorkerRole` sınıfı:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

İçinde `OnStart` yöntemi, ana bilgisayarı başlatmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start** yöntemi OWIN ana bilgisayarı başlatır. Adını `Startup` yöntem bir tür parametresine bir sınıftır. Kural gereği, konak çağıracak `Configure` bu sınıfın yöntemi.

Geçersiz kılma `OnStop` elden çıkarmak  *\_uygulama* örneği:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

WorkerRole.cs için tam kod aşağıdaki gibidir:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Çözümü derleyin ve uygulamayı Azure işlem öykünücüsü'nde yerel olarak çalıştırmak için F5 tuşuna basın. Güvenlik duvarı ayarlarınıza bağlı olarak, güvenlik duvarı üzerinden öykünücü izin gerekebilir.

İşlem öykünücüsü, uç nokta için bir yerel IP adresi atar. IP adresi, işlem öykünücüsü kullanıcı Arabiriminde görüntüleyerek bulabilirsiniz. Görev çubuğu bildirim alanında bulunan öykünücü simgesine sağ tıklayın ve seçin **Göster işlem öykünücüsü kullanıcı Arabiriminde**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

IP adresi hizmet dağıtımları, dağıtım [ID] hizmet ayrıntıları altında bulabilirsiniz. Bir web tarayıcısı açın ve http:// gidin<em>adresi</em>burada <em>adresi</em> ; işlem öykünücüsü tarafından atanan IP adresi gibi `http://127.0.0.1:80`. OWIN Hoş Geldiniz sayfasını görmeniz gerekir:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adım için bir Azure hesabınızın olması gerekir. Zaten yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Microsoft Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Çözüm Gezgini'nde AzureApp projeye sağ tıklayın. Seçin **yayımlama**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Azure hesabınızda oturum açmadınız tıklatmak **oturum**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Oturum açtıktan sonra bir abonelik seçin ve tıklayın **sonraki**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Bulut hizmeti için bir ad girin ve bir bölge seçin. **Oluştur**'u tıklatın.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Tıklayın **yayımlama**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Azure Etkinlik Günlüğü penceresini dağıtımın ilerleme durumunu gösterir. Uygulama dağıtıldığında, Gözat `http://appname.cloudapp.net/`burada *appname* bulut hizmetinizin adı.

## <a name="additional-resources"></a>Ek Kaynaklar

- [Project Katana’ya Genel Bakış](an-overview-of-project-katana.md)
- [Github'da Katana proje](https://github.com/aspnet/AspNetKatana/)
