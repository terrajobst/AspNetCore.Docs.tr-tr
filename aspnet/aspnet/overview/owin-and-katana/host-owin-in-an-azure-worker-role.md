---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: "Konağı Azure çalışan rolünde OWIN | Microsoft Docs"
author: MikeWasson
description: "Bu öğretici, bir Microsoft Azure çalışan rolünde kendini OWIN barındırma gösterilmektedir. Açık Web arabirimi için .NET (OWIN) .NET web sunucusu arasında bir Özet tanımlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 647514ae5a92b9d729179327fb97bd8005b0a4b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="host-owin-in-an-azure-worker-role"></a>Konak bir Azure çalışan rolünde OWIN
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Bu öğretici, bir Microsoft Azure çalışan rolünde kendini OWIN barındırma gösterilmektedir.
> 
> [.NET için Web Arabirimi'ni açmak](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar. OWIN ayrıştırır OWIN IIS dışında kendi işleminde, bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucunun web uygulamasından – Örneğin, Azure çalışan rolüne içinde.
> 
> Bu öğreticide, bir Microsoft Azure çalışan rolü OWIN uygulamalarında kendini barındırma öğreneceksiniz. Çalışan rolleri hakkında daha fazla bilgi için bkz: [Azure yürütme modelleri](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [2.3 .NET için Azure SDK](https://azure.microsoft.com/en-us/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Bir Microsoft Azure projesi oluşturma

Visual Studio'yu yönetici ayrıcalıklarıyla çalıştırın. Azure işlem öykünücüsü kullanarak uygulama yerel olarak hata ayıklama için yönetici ayrıcalıkları gerekir.

Üzerinde **dosya** menüsünde tıklatın **yeni**, ardından **proje**. Gelen **yüklü şablonlar**, Visual C# altında tıklatın **bulut** ve ardından **Windows Azure bulut hizmeti**. "AzureApp" adını verin ve projeyi tıklatın **Tamam**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, çift **çalışan rolü**. Varsayılan adı ("WorkerRole1") bırakın. Bu adım, bir çalışan rolü çözüme ekler. **Tamam**'ı tıklatın.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Oluşturulan Visual Studio çözümü iki proje içerir:

- &quot;AzureApp&quot; Azure uygulaması için yapılandırma ve rolleri tanımlar.
- &quot;WorkerRole1&quot; çalışan rolü kodunu içerir.

Genel olarak, tek bir rol kullansa da Bu öğretici bir Azure uygulama birden çok rol içerebilir.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Kendini barındırma OWIN paketleri ekleme

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.

Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Bir HTTP uç nokta ekleyin

Çözüm Gezgini'nde AzureApp projenizi genişletin. Rolleri düğümünü genişletin, WorkerRole1 sağ tıklatın ve seçin **özellikleri**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Tıklatın **uç noktaları**ve ardından **uç nokta Ekle**.

İçinde **Protokolü** açılır listeden seçin "http". İçinde **genel bağlantı noktası** ve **özel bağlantı noktası**, 80 yazın. Bu bağlantı noktası numaralarını farklı olabilir. Role bir isteği gönderdiğinizde hangi istemcilerin kullandığı genel bağlantı noktasıdır.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>OWIN başlangıç sınıfı oluşturma

Çözüm Gezgini'nde, WorkerRole1 projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `Startup`.

Tüm Demirbaş kod aşağıdakiyle değiştirin:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage` Genişletme yöntemi, uygulamanızı site çalıştığını doğrulamak için basit bir HTML sayfası ekler.

## <a name="start-the-owin-host"></a>OWIN konak Başlat

WorkerRole.cs dosyasını açın. Bu sınıf, çalışan rolü başlatıldığında ve durdurulduğunda, çalışan bir kod tanımlar.

Aşağıdaki ekleme deyimini kullanarak:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Ekleme bir **IDisposable** üyesine `WorkerRole` sınıfı:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

İçinde `OnStart` yöntemi, konağı başlatmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start** yöntemi OWIN ana bilgisayarı başlatır. Adını `Startup` yöntemi için bir tür parametresi bir sınıftır. Kurala göre konak çağıracak `Configure` bu sınıfın yöntemi.

Geçersiz kılma `OnStop` elden için  *\_uygulama* örneği:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

WorkerRole.cs için tam kod aşağıdaki gibidir:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Çözümü derlemek ve Azure işlem öykünücüsü uygulamayı yerel olarak çalıştırmak için F5 tuşuna basın. Güvenlik Duvarı ayarlarınızı bağlı olarak, güvenlik duvarı üzerinden öykünücüsü izin gerekebilir.

İşlem öykünücüsü uç noktası için bir yerel IP adresi atar. IP adresi işlem öykünücüsü kullanıcı Arabiriminde görüntüleyerek bulabilirsiniz. Görev çubuğu bildirim alanında bulunan öykünücü simgesine sağ tıklatın ve seçin **Göster işlem öykünücüsü kullanıcı Arabiriminde**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

IP adresi hizmet dağıtımları, dağıtım [kimlik], hizmet ayrıntıları altında bulabilirsiniz. Bir web tarayıcısı açın ve http:// gidin*adresi*, burada *adresi* işlem öykünücüsü tarafından; atanan IP adresi gibi `http://127.0.0.1:80`. OWIN Karşılama sayfasını görmeniz gerekir:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Azure'a dağıtma

Bu adım için bir Azure hesabınızın olması gerekir. Zaten yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Microsoft Azure ücretsiz deneme sürümü](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).

Çözüm Gezgini'nde AzureApp projesine sağ tıklayın. Seçin **yayımlama**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Azure hesabınızda açmadınız tıklatmak **oturum**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Oturumunuz açıldıktan sonra bir abonelik seçin ve'ı tıklatın **sonraki**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Bulut hizmeti için bir ad girin ve bir bölge seçin. 
              **Oluştur**'u tıklatın.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Tıklatın **yayımlama**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Azure etkinlik günlüğü penceresi dağıtımının ilerlemesini gösterir. Uygulama dağıtıldığında, Gözat `http://appname.cloudapp.net/`, burada *appname* bulut hizmetiniz adıdır.

## <a name="additional-resources"></a>Ek Kaynaklar

- [Proje Katana genel bakış](an-overview-of-project-katana.md)
- [Github'da Katana proje](https://github.com/aspnet/AspNetKatana/)
