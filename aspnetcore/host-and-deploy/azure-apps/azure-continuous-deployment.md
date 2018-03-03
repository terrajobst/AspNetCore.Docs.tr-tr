---
title: "Visual Studio ve Git ile Azure için sürekli dağıtım"
author: rick-anderson
description: "Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: ea4788b5daead9e355e13b963c025dd110eb2bff
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Visual Studio ve Git ile ASP.NET Core için Azure için sürekli dağıtım

Tarafından [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Bu öğretici, sürekli dağıtımı kullanarak Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturma ve Visual Studio'dan Azure App Service'e dağıtma gösterir.

Ayrıca bkz. [oluşturma ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için kullanım VSTS](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), kesintisiz teslim (CD) iş akışı için yapılandırma gösterilir [Azure uygulama hizmeti](/azure/app-service/app-service-web-overview) Visual Studio Team kullanarak Hizmetler. Azure sürekli teslim Team Services, Azure App Service içinde barındırılan uygulamalar için güncelleştirmeleri yayımlamak için sağlam dağıtım ardışık düzen ayarlama basitleştirir. Ardışık düzen oluşturmak, testleri çalıştırmak, bir hazırlama yuvasını dağıtmak ve üretime dağıtmak için Azure portalından yapılandırılabilir.

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Microsoft Azure hesabı gereklidir. Bir hesap almak için [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici aşağıdaki yazılımın yüklü olduğu varsayılır:

* [Visual Studio](https://www.visualstudio.com)
* [.NET core SDK](https://www.microsoft.com/net/download/core) (çalışma zamanı ve araçları)
* [Git](https://git-scm.com/downloads) Windows için

## <a name="create-an-aspnet-core-web-app"></a>Bir ASP.NET Core web uygulaması oluşturma

1. Visual Studio'yu başlatın.

1. Gelen **dosya** menüsünde, select **yeni** > **proje**.

1. Seçin **ASP.NET çekirdek Web uygulaması** proje şablonu. Altında görünür **yüklü** > **şablonları** > **Visual C#** > **.NET Core**. Proje adı `SampleWebAppDemo`. Seçin **yeni Git deposunun oluşturma** seçeneğini ve tıklayın **Tamam**.

   ![Yeni Proje iletişim kutusu](azure-continuous-deployment/_static/01-new-project.png)

1. İçinde **yeni ASP.NET Core projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu, ardından **Tamam**.

   ![Yeni ASP.NET projesi iletişim kutusu](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> En son .NET Core 2.0 sürümüdür.

### <a name="running-the-web-app-locally"></a>Web uygulamasını yerel olarak çalıştırma

1. Visual Studio uygulama oluşturduktan sonra uygulama seçerek çalıştırma **hata ayıklama** > **hata ayıklamayı Başlat**. Alternatif olarak, basın **F5**.

   Visual Studio ve yeni uygulamayı başlatmak için saat sürebilir. Tamamlandığında, tarayıcı çalışan uygulamanın gösterir.

   !['Hello World!' görüntüler uygulamayı çalıştıran gösteren tarayıcı penceresi](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Çalışan Web uygulaması inceledikten sonra Tarayıcıyı kapatın ve uygulama durdurmak için Visual Studio araç çubuğunda "Hata ayıklamayı Durdur" simgesini seçin.

## <a name="create-a-web-app-in-the-azure-portal"></a>Azure Portalı'nda bir web uygulaması oluşturma

Aşağıdaki adımlar, Azure Portalı'nda bir web uygulaması oluşturun:

1. Oturum [Azure Portal](https://portal.azure.com).

1. Seçin **yeni** en sol üst portal arabirimi.

1. Seçin **Web + mobil** > **Web uygulaması**.

   ![Microsoft Azure Portal: Yeni düğmesi: Web + mobil Market altında: Web uygulaması düğmesi altında öne çıkan uygulamalar](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. İçinde **Web uygulaması** dikey penceresinde için benzersiz bir değer girin **uygulama hizmet adı**.

   ![Web uygulaması dikey](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **Uygulama hizmeti adını** adının benzersiz olması gerekir. Adı sağlandığında portal bu kural zorlar. Farklı bir değer sağlayarak, her örneği için bu değeri yerine **SampleWebAppDemo** bu öğreticideki.

   Ayrıca, **Web uygulaması** dikey penceresinde, var olan seçin **App Service planı/konumu** veya yeni bir tane oluşturun. Yeni bir plan oluşturuyorsanız, fiyatlandırma katmanı, konum ve diğer seçenekleri seçin. Uygulama hizmeti planları hakkında daha fazla bilgi için bkz: [Azure App Service planlarına ayrıntılı genel bakış](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Seçin **oluşturma**. Azure sağlamak ve web uygulaması başlatın.

   ![Azure Portal: Örnek Web uygulamasını Demo 01 Essentials dikey penceresi](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Yeni web uygulaması için Git yayımlamayı etkinleştirme

Git Azure App Service web uygulama dağıtmak için kullanılan bir dağıtılmış sürüm denetim sistemidir. Web uygulama kodu yerel bir Git deposunda depolanır ve kod uzak depoya ileterek Azure'a dağıtılır.

1. İçine oturum [Azure Portal](https://portal.azure.com).

1. Seçin **uygulama hizmetleri** Azure aboneliği ile ilişkili uygulama hizmetleri listesini görüntülemek için.

1. Bu öğreticinin önceki bölümde oluşturduğunuz web uygulaması seçin.

1. İçinde **dağıtım** dikey penceresinde, select **dağıtım seçenekleri** > **Kaynağı Seç** > **yerel Git deposu**.

   ![Ayarlar dikey: dağıtım kaynak dikey: kaynak dikey seçin](azure-continuous-deployment/_static/deployment-options.png)

1. Seçin **Tamam**.

1. Bir web uygulaması veya diğer App Service uygulama yayımlamak için dağıtım kimlik bilgileri önceden ayarlanmış yüklemediyseniz, bunları şimdi kurun:

   * Seçin **ayarları** > **dağıtım kimlik bilgileri**. **Dağıtım kimlik bilgilerini ayarla** dikey penceresi görüntülenir.
   * Bir kullanıcı adı ve parola oluşturun. Daha sonra kullanmak için parola Git ayarlarken kaydedin.
   * Seçin **kaydetmek**.

1. İçinde **Web uygulaması** dikey penceresinde, select **ayarları** > **özellikleri**. Dağıtmak için Uzak Git deposu URL'sini altında gösterilen **GIT URL'si**.

1. Kopya **GIT URL'si** öğreticide daha sonra kullanmak için değer.

   ![Azure Portal: uygulama özellikleri dikey penceresi](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Azure App Service'te web uygulaması yayımlama

Bu bölümde, Visual Studio ve anında iletme bu depodan web uygulaması dağıtma için Azure kullanarak yerel bir Git deposu oluşturursunuz. Adımlar şunlardır:

* Yerel deposu Azure'da dağıtılabilmesi amacıyla GIT URL'si değerini kullanarak uzak depo ayarı ekleyin.
* Proje değişiklikleri uygulayın.
* Proje değişiklikleri yerel depodan Azure'da uzak deponuza iletin.

1. İçinde **Çözüm Gezgini** sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**. **Takım Gezgini** görüntülenir.

   ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/10-team-explorer.png)

1. İçinde **Takım Gezgini**seçin **ev** (ev simgesi) > **ayarları** > **deposu ayarları**.

1. İçinde **uzaktan kumandalar** bölümünü **deposu ayarları**seçin **Ekle**. **Eklemek uzak** iletişim kutusu görüntülenir.

1. Ayarlama **adı** uzak **Azure ÖrnekUyg**.

1. Değeri ayarlanamıyor **Fetch** için **Git URL'si** Bu öğreticide daha önce Azure'dan kopyalanır. Bu ile biten URL olduğuna dikkat edin **.git**.

   ![Uzaktan iletişim Düzenle](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Alternatif olarak, uzak depodan belirtin **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve komutunu girerek. Örnek:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Seçin **ev** (ev simgesi) > **ayarları** > **genel ayarları**. Ad ve e-posta adresi olarak ayarlanmadığından onaylayın. Seçin **güncelleştirme** gerekiyorsa.

1. Seçin **giriş** > **değişiklikleri** geri dönmek için **değişiklikleri** görünümü.

1. Bir kaydetme iletisi gibi girin **ilk anında #1** seçip **tamamlama**. Bu eylem oluşturur bir *tamamlama* yerel olarak.

   ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Kaydetme işleminin alternatifi olarak, değişiklikleri **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutları girerek. Örnek:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Seçin **giriş** > **eşitleme** > **Eylemler** > **komut istemi açın**. Komut istemi proje dizinine açılır.

1. Komut penceresinde aşağıdaki komutu girin:

   `git push -u Azure-SampleApp master`

1. Azure girin **dağıtım kimlik bilgileri** Azure'da daha önce oluşturulan parola.

   Bu komut, yerel proje dosyalarını Azure'a dağıtmaya işlemi başlatır. Yukarıdaki komut çıktısı dağıtımının başarılı bir ileti ile sona erer.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Projede işbirliği gerekiyorsa, için itme göz önünde bulundurun [GitHub](https://github.com) Azure'a dağıtmaya önce.
 
### <a name="verify-the-active-deployment"></a>Etkin dağıtımı doğrulama

Azure web uygulaması aktarımı yerel ortamından başarılı olduğunu doğrulayın.

İçinde [Azure Portal](https://portal.azure.com), web uygulamasını seçin. Seçin **dağıtım** > **dağıtım seçenekleri**.

![Azure Portal: Ayarlar dikey: dağıtımları dikey penceresi başarılı dağıtımı gösterme](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Uygulamayı Azure'da çalıştırma

Web uygulaması için Azure dağıtılır, uygulamayı çalıştırın.

Bu iki yolla gerçekleştirilebilir:

* Azure Portalı'nda, web uygulaması için web uygulaması dikey bulun. Seçin **Gözat** uygulamayı varsayılan tarayıcıda görüntülemek için.
* Bir tarayıcı açın ve web uygulaması için URL'yi girin. Örnek: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Web uygulaması güncelleştirin ve yeniden yayımlamanız

Yerel kod değişiklikleri yaptıktan sonra yeniden yayımlayın:

1. İçinde **Çözüm Gezgini** Visual Studio açık *haline* dosya.

1. İçinde `Configure` yöntemini değiştirme `Response.WriteAsync` şekilde aşağıdaki gibi görünmesi yöntemi:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Değişiklikleri kaydetmek *haline*.

1. İçinde **Çözüm Gezgini**, sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**. **Takım Gezgini** görüntülenir.

1. Bir kaydetme iletisi gibi girin `Update #2`.

1. Tuşuna **tamamlama** proje değişiklikleri yürütmek için düğmesi.

1. Seçin **giriş** > **eşitleme** > **Eylemler** > **anında**.

> [!NOTE]
> Alternatif olarak, değişiklikleri anında **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutunu girerek. Örnek:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Görünüm Azure güncelleştirilmiş web uygulaması

Güncelleştirilmiş web uygulaması seçerek görüntülemek **Gözat** Azure Portalı'nda veya bir tarayıcı açarak ve web uygulaması için URL girerek web uygulaması dikey penceresinden. Örnek: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Ek kaynaklar

* [Derleme ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için VSTS kullanın](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)
