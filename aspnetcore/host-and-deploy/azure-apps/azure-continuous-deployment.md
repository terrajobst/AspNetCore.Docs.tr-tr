---
title: ASP.NET Core ile Git ve Visual Studio ile Azure'a sürekli dağıtım
author: rick-anderson
description: Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 3470d9278574a95115b14f25b90a0a93bb3b8a67
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756616"
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a>ASP.NET Core ile Git ve Visual Studio ile Azure'a sürekli dağıtım

tarafından [Erik Reitan](https://github.com/Erikre)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Bu öğreticide, sürekli dağıtım kullanarak, Visual Studio kullanarak ASP.NET Core web uygulaması oluşturma ve bunu Visual Studio'dan Azure App Service'e dağıtma gösterilir.

Ayrıca bkz [kullanım oluşturun ve sürekli dağıtım ile bir Azure Web uygulamasına yayımlamak için VSTS](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), nasıl bir sürekli teslim (CD) iş akışı yapılandırma [Azure App Service](/azure/app-service/app-service-web-overview) kullanarak Visual Studio Takım Hizmetler. Azure, Team Services'da sürekli teslim, Azure App Service'te barındırılan uygulamalar için güncelleştirmeleri yayımlamak için sağlam bir dağıtım işlem hattı ayarlamayı basitleştirir. İşlem hattı, Azure portalından oluşturmak, testleri çalıştırmak, hazırlama yuvasına dağıtın ve sonra üretim ortamına dağıtmak için yapılandırılabilir.

> [!NOTE]
> Bu öğreticiyi tamamlamak için Microsoft Azure hesabı gereklidir. Bir hesap almak için [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, aşağıdaki yazılımın yüklü olduğu varsayılır:

* [Visual Studio](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://git-scm.com/downloads) Windows için

## <a name="create-an-aspnet-core-web-app"></a>Bir ASP.NET Core web uygulaması oluşturma

1. Visual Studio'yu başlatın.

1. Gelen **dosya** menüsünde **yeni** > **proje**.

1. Seçin **ASP.NET Core Web uygulaması** proje şablonu. Altında göründüğü **yüklü** > **şablonları** > **Visual C#** > **.NET Core**. Projeyi adlandırın `SampleWebAppDemo`. Seçin **yeni Git deposu Oluştur** seçeneğini ve tıklayın **Tamam**.

   ![Yeni Proje iletişim kutusu](azure-continuous-deployment/_static/01-new-project.png)

1. İçinde **yeni ASP.NET Core projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu, ardından **Tamam**.

   ![Yeni ASP.NET Core projesi iletişim kutusu](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> En son .NET Core 2.0 sürümüdür.

### <a name="running-the-web-app-locally"></a>Web uygulamasını yerel olarak çalıştırma

1. Visual Studio uygulamayı oluşturmayı tamamladığında, uygulamayı seçerek çalıştırma **hata ayıklama** > **hata ayıklamayı Başlat**. Alternatif olarak, basın **F5**.

   Bu, Visual Studio ve yeni uygulamayı başlatmak için zaman alabilir. Tamamlandığında, tarayıcıyı çalışmakta olan uygulamayla gösterir.

   !['Hello World!' görüntüler uygulama çalıştıran gösteren tarayıcı penceresi](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Çalışan Web uygulamasına inceledikten sonra Tarayıcıyı kapatın ve uygulamanın durdurmak için Visual Studio araç çubuğunda "Hata ayıklamayı Durdur" simgesini seçin.

## <a name="create-a-web-app-in-the-azure-portal"></a>Azure Portalı'nda bir web uygulaması oluşturma

Aşağıdaki adımlar, Azure Portalı'nda bir web uygulaması oluşturur:

1. Oturum [Azure portalında](https://portal.azure.com).

1. Seçin **yeni** en sol üst köşesindeki portal arabirimi.

1. Seçin **Web + mobil** > **Web uygulaması**.

   ![Microsoft Azure portalı: Yeni düğme: Web + mobil Market altında: Web uygulaması düğmesi altında öne çıkan uygulamalar](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. İçinde **Web uygulaması** dikey penceresinde için benzersiz bir değer girin **uygulama hizmeti adı**.

   ![Web uygulaması dikey](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **Uygulama hizmeti adı** adı benzersiz olmalıdır. Adı sağlandığında portal bu kuralı uygular. Bu değer her oluşumu için farklı bir değer sağlayan, yerine **SampleWebAppDemo** bu öğreticideki.

   Ayrıca **Web uygulaması** dikey penceresinde, mevcut bir seçin **App Service planı/konumu** veya yeni bir tane oluşturun. Yeni bir plan oluşturuyorsanız, fiyatlandırma katmanını, konum ve diğer seçenekleri'ni seçin. App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına ayrıntılı genel bakış](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Seçin **oluşturma**. Azure, sağlama ve web uygulaması başlatın.

   ![Azure portalı: Örnek Web uygulamasını tanıtım 01 hakkında temel bilgiler dikey penceresi](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Yeni web uygulaması için Git yayımlamayı etkinleştirme

Git, bir Azure App Service web uygulaması dağıtmak için kullanılan bir dağıtılmış sürüm denetim sistemidir. Web uygulama kodu yerel bir Git deposunda depolanır ve kodu uzak depoya ileterek Azure'a dağıtılır.

1. Oturum [Azure portalında](https://portal.azure.com).

1. Seçin **uygulama hizmetleri** Azure aboneliği ile ilişkili uygulama hizmetlerin bir listesini görüntülemek için.

1. Bu öğreticinin önceki bölümünde oluşturduğunuz web uygulamasını seçin.

1. İçinde **dağıtım** dikey penceresinde **dağıtım seçenekleri** > **Kaynağı Seç** > **yerel Git deposu**.

   ![Ayarlar dikey penceresi: dağıtım kaynağı dikey penceresi: kaynak dikey penceresini seçin](azure-continuous-deployment/_static/deployment-options.png)

1. Seçin **Tamam**.

1. Dağıtım kimlik bilgileri, bir web uygulaması veya diğer App Service uygulaması yayımlamak için önceden kurulmuş yüklemediyseniz, bunları şimdi ayarlayın:

   * Seçin **ayarları** > **dağıtım kimlik bilgileri**. **Dağıtım kimlik bilgilerini ayarla** dikey penceresi görüntülenir.
   * Bir kullanıcı adı ve parola oluşturun. Git'i kurma ayarlarken daha sonra kullanmak için parola kaydedin.
   * Seçin **Kaydet**.

1. İçinde **Web uygulaması** dikey penceresinde **ayarları** > **özellikleri**. Dağıtmak için Uzak Git deposunun URL'sini altında gösterilen **GIT URL'si**.

1. Kopyalama **GIT URL'si** öğreticide daha sonra kullanmak için değer.

   ![Azure portalı: uygulama özellikleri dikey penceresi](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Web uygulamasını Azure App Service'e yayımlama

Bu bölümde, Visual Studio ve anında iletme bu depodan web uygulamasına dağıtmak için Azure'da kullanarak yerel bir Git deposu oluşturun. Adımlar şunlardır:

* GIT URL değeri, yerel depoya Azure'a dağıtılacak şekilde kullanarak uzak depo ayarı ekleyin.
* Proje değişiklikleri uygulayın.
* Proje değişiklikleri yerel depodan Azure'da uzak depoya gönderin.

1. İçinde **Çözüm Gezgini** sağ **çözüm 'SampleWebAppDemo'** seçip **işleme**. **Takım Gezgini** görüntülenir.

   ![Takım Gezgini Bağlan sekmesinde](azure-continuous-deployment/_static/10-team-explorer.png)

1. İçinde **Takım Gezgini**seçin **giriş** (giriş simgesi) > **ayarları** > **depo ayarları**.

1. İçinde **uzaktan kumandalar** bölümünü **depo ayarları**seçin **Ekle**. **Ekleme uzak** iletişim kutusu görüntülenir.

1. Ayarlama **adı** için uzaktan **Azure örnek uygulaması**.

1. Değerini **Fetch** için **Git URL'si** Bu öğreticide daha önce Azure'dan kopyalanır. İle biten URL olduğunu unutmayın **.git**.

   ![Uzaktan iletişim Düzenle](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Alternatif olarak, uzak depodan belirtin **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve komutunu girerek. Örnek:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Seçin **giriş** (giriş simgesi) > **ayarları** > **genel ayarlar**. Ad ve e-posta adresinin ayarlandığını onaylayın. Seçin **güncelleştirme** gerekirse.

1. Seçin **giriş** > **değişiklikleri** dönmek için **değişiklikleri** görünümü.

1. Bir işleme iletisi girin **ilk anında iletme #1** seçip **işleme**. Bu eylem, oluşturur bir *işleme* yerel olarak.

   ![Takım Gezgini Bağlan sekmesinde](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Alternatif olarak, işleme değişir **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutları girerek. Örnek:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Seçin **giriş** > **eşitleme** > **eylemleri** > **komut istemi açın**. Komut istemi proje dizinine açılır.

1. Komut penceresinde aşağıdaki komutu girin:

   `git push -u Azure-SampleApp master`

1. Azure girin **dağıtım kimlik bilgileri** Azure'da daha önce oluşturulan parola.

   Bu komut, yerel proje dosyalarını Azure'a gönderme işlemini başlatır. Yukarıdaki komut çıktısı, dağıtım başarılı bir ileti ile sona erer.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Projede işbirliği gerekiyorsa, dala gönderim yapmasını istemeyiz göz önünde bulundurun [GitHub](https://github.com) Azure'a göndermeden önce.
 
### <a name="verify-the-active-deployment"></a>Etkin dağıtımı doğrulama

Azure web app aktarımı yerel bir ortamdan başarılı olduğunu doğrulayın.

İçinde [Azure portalı](https://portal.azure.com), web uygulamasını seçin. Seçin **dağıtım** > **dağıtım seçenekleri**.

![Azure portalı: Ayarlar dikey penceresi: başarılı dağıtımı gösteren dağıtımları dikey penceresi](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Uygulamayı Azure'da çalıştırın

Web uygulamasını Azure'a dağıtıldığına göre uygulamayı çalıştırın.

Bu iki şekilde gerçekleştirilebilir:

* Azure Portalı'nda web uygulaması için web uygulaması dikey bulun. Seçin **Gözat** uygulamayı varsayılan tarayıcıda görüntülemek için.
* Bir tarayıcı açın ve web uygulamasının URL'sini girin. Örnek: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Web uygulamasını güncelleştirme ve yeniden yayımlama

Yerel koda değişiklikleri yaptıktan sonra yeniden yayımlayın:

1. İçinde **Çözüm Gezgini** Visual Studio açık *Startup.cs* dosya.

1. İçinde `Configure` yöntemini, değiştirme `Response.WriteAsync` şekilde aşağıdaki gibi görünecek şekilde yöntemi:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Değişiklikleri kaydetmek *Startup.cs*.

1. İçinde **Çözüm Gezgini**, sağ **çözüm 'SampleWebAppDemo'** seçip **işleme**. **Takım Gezgini** görüntülenir.

1. Bir işleme iletisi girin `Update #2`.

1. Tuşuna **işleme** proje değişiklikleri kaydetmek için düğme.

1. Seçin **giriş** > **eşitleme** > **eylemleri** > **anında iletme**.

> [!NOTE]
> Alternatif olarak, değişiklikleri anında iletme **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve bir git komutu girmeyi. Örnek:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Güncelleştirilen web uygulamasını Azure'da görüntüleme

Güncelleştirilen web uygulamasını seçerek görüntüleme **Gözat** bir tarayıcıyı açarak ve web uygulaması için URL girilerek veya Azure Portal'da web uygulaması dikey penceresinden. Örnek: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Ek kaynaklar

* [Derleme ve sürekli dağıtım ile bir Azure Web uygulamasına yayımlamak için VSTS kullanın](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Kudu projesi](https://github.com/projectkudu/kudu/wiki)
