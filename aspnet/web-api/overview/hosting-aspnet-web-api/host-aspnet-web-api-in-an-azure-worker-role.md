---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: ASP.NET Web API 2 Azure çalışan rolünde konak | Microsoft Docs
author: MikeWasson
description: Bu öğretici bir Azure çalışan rolünde ASP.NET Web API barındırmak OWIN Web API çerçevesi Self barındırmak için kullanma gösterilmektedir. Web arabirimi için .NET (OWIN) de Aç...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 7ba1dc850e2f9d9c88e6ddf263a796e1867a98be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873657"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>ASP.NET Web API 2 Azure çalışan rolünde ana bilgisayar
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Bu öğretici bir Azure çalışan rolünde ASP.NET Web API barındırmak OWIN Web API çerçevesi Self barındırmak için kullanma gösterilmektedir.
> 
> [.NET için Web Arabirimi'ni açmak](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar. OWIN ayrıştırır OWIN IIS dışında kendi işleminde, bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucunun web uygulamasından – Örneğin, Azure çalışan rolüne içinde.
> 
> Bu öğreticide, Microsoft.Owin.Host.HttpListener paket kullanacağınız bir HTTP sunucusu, sağlayan OWIN uygulamaları kendi kendine barındırmak için kullanılır.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - [2.3 .NET için Azure SDK](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Bir Microsoft Azure projesi oluşturma

Visual Studio'yu yönetici ayrıcalıklarıyla çalıştırın. Azure işlem öykünücüsü kullanarak uygulama yerel olarak hata ayıklama için yönetici ayrıcalıkları gerekir.

Üzerinde **dosya** menüsünde tıklatın **yeni**, ardından **proje**. Gelen **yüklü şablonlar**, Visual C# altında tıklatın **bulut** ve ardından **Windows Azure bulut hizmeti**. "AzureApp" adını verin ve projeyi tıklatın **Tamam**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, çift **çalışan rolü**. Varsayılan adı ("WorkerRole1") bırakın. Bu adım, bir çalışan rolü çözüme ekler. **Tamam**'ı tıklatın.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Oluşturulan Visual Studio çözümü iki proje içerir:

- &quot;AzureApp&quot; Azure uygulaması için yapılandırma ve rolleri tanımlar.
- &quot;WorkerRole1&quot; çalışan rolü kodunu içerir.

Genel olarak, tek bir rol kullansa da Bu öğretici bir Azure uygulama birden çok rol içerebilir.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API ve OWIN paketleri ekleme

Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.

Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Bir HTTP uç nokta ekleyin

Çözüm Gezgini'nde AzureApp projenizi genişletin. Rolleri düğümünü genişletin, WorkerRole1 sağ tıklatın ve seçin **özellikleri**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Tıklatın **uç noktaları**ve ardından **uç nokta Ekle**.

İçinde **Protokolü** açılır listeden seçin "http". İçinde **genel bağlantı noktası** ve **özel bağlantı noktası**, 80 yazın. Bu bağlantı noktası numaralarını farklı olabilir. Role bir isteği gönderdiğinizde hangi istemcilerin kullandığı genel bağlantı noktasıdır.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Web API için yapılandırma kendini barındırma

Çözüm Gezgini'nde, WorkerRole1 projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için. Sınıf adını `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Bu dosyadaki Demirbaş kod tümünün aşağıdakiyle değiştirin:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Bir Web API denetleyicisi ekleme

Ardından, bir Web API denetleyicisi sınıfı ekleyin. WorkerRole1 projesine sağ tıklatın ve **Ekle** / **sınıfı**. TestController sınıfı adı. Bu dosyadaki Demirbaş kod tümünün aşağıdakiyle değiştirin:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Kolaylık olması için bu denetleyici yalnızca düz metin dönüş iki GET yöntemleri tanımlar.

## <a name="start-the-owin-host"></a>OWIN konak Başlat

WorkerRole.cs dosyasını açın. Bu sınıf, çalışan rolü başlatıldığında ve durdurulduğunda, çalışan bir kod tanımlar.

Aşağıdaki ekleme deyimini kullanarak:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Ekleme bir **IDisposable** üyesine `WorkerRole` sınıfı:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

İçinde `OnStart` yöntemi, konağı başlatmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start** yöntemi OWIN ana bilgisayarı başlatır. Adını `Startup` yöntemi için bir tür parametresi bir sınıftır. Kurala göre konak çağıracak `Configure` bu sınıfın yöntemi.

Geçersiz kılma `OnStop` elden için  *\_uygulama* örneği:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

WorkerRole.cs için tam kod aşağıdaki gibidir:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Çözümü derlemek ve Azure işlem öykünücüsü uygulamayı yerel olarak çalıştırmak için F5 tuşuna basın. Güvenlik Duvarı ayarlarınızı bağlı olarak, güvenlik duvarı üzerinden öykünücüsü izin gerekebilir.

> [!NOTE]
> Aşağıdaki gibi bir özel durum almaya devam ediyorsanız lütfen bkz [bu blog gönderisine](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) geçici bir çözüm için. "Dosya veya derleme yüklenemedi ' Microsoft.Owin, sürüm 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' ya da bağımlılıklarından biri. Konumlandırılan derlemenin bildirim tanımı derleme başvurusuyla eşleşmiyor. (HRESULT özel durumu: 0x80131040) "


İşlem öykünücüsü uç noktası için bir yerel IP adresi atar. IP adresi işlem öykünücüsü kullanıcı Arabiriminde görüntüleyerek bulabilirsiniz. Görev çubuğu bildirim alanında bulunan öykünücü simgesine sağ tıklatın ve seçin **Göster işlem öykünücüsü kullanıcı Arabiriminde**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

IP adresi hizmet dağıtımları, dağıtım [kimlik], hizmet ayrıntıları altında bulabilirsiniz. Bir web tarayıcısı açın ve http:// gidin<em>adresi</em>/test/1, burada <em>adresi</em> işlem öykünücüsü tarafından; atanan IP adresidir Örneğin, `http://127.0.0.1:80/test/1`. Web API denetleyicisi yanıtı görmeniz gerekir:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Azure'a dağıtma

Bu adım için bir Azure hesabınızın olması gerekir. Zaten yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Microsoft Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Çözüm Gezgini'nde AzureApp projesine sağ tıklayın. Seçin **yayımlama**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Azure hesabınızda açmadınız tıklatmak **oturum**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Oturumunuz açıldıktan sonra bir abonelik seçin ve'ı tıklatın **sonraki**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Bulut hizmeti için bir ad girin ve bir bölge seçin. **Oluştur**'u tıklatın.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Tıklatın **yayımlama**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Azure etkinlik günlüğü penceresi dağıtımının ilerlemesini gösterir. Uygulama dağıtıldığında, Gözat http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [Project Katana’ya Genel Bakış](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Github'da Katana proje](https://github.com/aspnet/AspNetKatana)
