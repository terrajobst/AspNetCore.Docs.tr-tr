---
title: "Visual Studio ve Git ile Azure için sürekli dağıtım"
author: rick-anderson
description: "Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Visual Studio ve Git ile ASP.NET Core için Azure için sürekli dağıtım

Tarafından [Erik Reitan](https://github.com/Erikre)

Bu öğretici sürekli dağıtımı kullanarak Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturma ve Visual Studio'dan Azure App Service'e dağıtma gösterir. 

Ayrıca bkz. [oluşturma ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için kullanım VSTS](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), kesintisiz teslim (CD) iş akışı için yapılandırma gösterilir [Azure uygulama hizmeti](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) Visual Studio Team kullanarak Hizmetler. Azure sürekli teslim Team Services, Azure App Service uygulamanız için güncelleştirmeleri yayımlamak için sağlam dağıtım ardışık düzen ayarlama basitleştirir. Ardışık düzen oluşturmak, testleri çalıştırmak, bir hazırlama yuvasını dağıtmak ve üretime dağıtmak için Azure portalından yapılandırılabilir.

> [!NOTE]
> Bu öğreticiyi tamamlamak için Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, şunları yapabilirsiniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici zaten aşağıdaki yüklemiş olduğunuz varsayılır:

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (çalışma zamanı ve araçları)

* [Git](https://git-scm.com/downloads) Windows için

## <a name="create-an-aspnet-core-web-app"></a>Bir ASP.NET Core web uygulaması oluşturma

1. Visual Studio'yu başlatın.

2. Gelen **dosya** menüsünde, select **yeni** > **proje**.

3. Seçin **ASP.NET Web uygulaması** proje şablonu. Altında görünür **yüklü** > **şablonları** > **Visual C#** > **Web**. Proje adı `SampleWebAppDemo`. Seçin **yeni Git deposunun oluşturma** seçeneğini ve tıklayın **Tamam**.

   ![Yeni Proje iletişim kutusu](azure-continuous-deployment/_static/01-new-project.png)

4. İçinde **yeni ASP.NET projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu, ardından **Tamam**.

   ![Yeni ASP.NET projesi iletişim kutusu](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a>Web uygulamasını yerel olarak çalıştırma

1. Visual Studio uygulama oluşturduktan sonra uygulama seçerek çalıştırma **hata ayıklama** -> **hata ayıklamayı Başlat**. Alternatif olarak, basabilirsiniz **F5**.

   Visual Studio ve yeni uygulamayı başlatmak için saat sürebilir. Tamamlandığında, tarayıcı çalışan uygulamanın gösterir.

   !['Hello World!' görüntüler uygulamayı çalıştıran gösteren tarayıcı penceresi](azure-continuous-deployment/_static/04-browser-runapp.png)

2. Çalışan Web uygulaması inceledikten sonra Tarayıcıyı kapatın ve uygulama durdurmak için Visual Studio'nun araç çubuğundaki "Hata ayıklamayı Durdur" simgesine tıklayın.

## <a name="create-a-web-app-in-the-azure-portal"></a>Azure Portalı'nda bir web uygulaması oluşturma

Aşağıdaki adımlar bir web uygulamasını Azure Portalı'nda oluşturmada size yol gösterecektir.

1. Oturum [Azure portalı](https://portal.azure.com)
2. DOKUNUN **yeni** en üstünde portalın sol
3. DOKUNUN **Web + mobil** > **Web uygulaması**

    ![Microsoft Azure Portal: Yeni düğmesi: Web + mobil Market altında: Web uygulaması düğmesi altında öne çıkan uygulamalar](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  İçinde **Web uygulaması** dikey penceresinde için benzersiz bir değer girin **uygulama hizmet adı**.

    ![Web uygulaması dikey](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >**Uygulama hizmeti adını** adının benzersiz olması gerekir. Adı girmek çalıştığınızda portal bu kural zorlar. Farklı bir değer girdikten sonra her örneği için bu değeri yerine gerekir **SampleWebAppDemo** Bu öğreticide gördüğünüz.

    &nbsp;
    
    Ayrıca, **Web uygulaması** dikey penceresinde, var olan seçin **App Service planı/konumu** veya yeni bir tane oluşturun. Yeni bir plan oluşturursanız, fiyatlandırma katmanı, konum ve diğer seçenekleri seçin. Uygulama hizmeti planları hakkında daha fazla bilgi için [Azure App Service planlarına ayrıntılı genel bakış](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).

5.  
              **Oluştur**'u tıklatın. Azure sağlamak ve web uygulaması başlatın.

    ![Azure Portal: Örnek Web uygulamasını Demo 01 Essentials dikey penceresi](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Yeni web uygulaması için Git yayımlamayı etkinleştirme

Git Azure App Service web uygulamanızı dağıtmak için kullanabileceğiniz bir dağıtılmış sürüm denetim sistemidir. Yerel bir Git deposu içinde web uygulamanız için yazdığınız kodu depolayacak ve kodunuzu uzak depoya ileterek Azure'a dağıtacaksınız.

1. İçine oturum [Azure Portal](https://portal.azure.com), zaten oturum açtınız.

2. Tıklatın **Gözat**, Gezinti bölmesinin konumunda bulunur.

3. Tıklatın **Web Apps** Azure aboneliğinizle ilişkili web uygulamalarının listesini görüntülemek için.

4. Bu öğreticinin önceki bölümde oluşturduğunuz web uygulaması seçin.

5. Varsa **ayarları** dikey gösterilmez, seçin **ayarları** içinde **Web uygulaması** dikey.

6. İçinde **ayarları** dikey penceresinde, select **dağıtım kaynağı** > **Kaynağı Seç** > **yerel Git deposu**.

   ![Ayarlar dikey: dağıtım kaynak dikey: kaynak dikey seçin](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. **Tamam**'ı tıklatın.

8. Daha önce bir web uygulaması veya diğer App Service uygulama yayımlamak için dağıtım kimlik bilgileri ayarlamadıysanız, bunları şimdi kurun:

   * Tıklatın **ayarları** > **dağıtım kimlik bilgileri**. **Dağıtım kimlik bilgilerini ayarla** dikey penceresinde görüntülenir.

   * Bir kullanıcı adı ve parola oluşturun.  Git ayarlarken daha sonra bu parola gerekir.

   * **Kaydet**'e tıklayın.

9. İçinde **Web uygulaması** dikey penceresinde tıklatın **ayarları** > **özellikleri**. İçin dağıtacaksınız uzak Git deposu URL'sini altında gösterilen **GIT URL'si**.

10. Kopya **GIT URL'si** öğreticide daha sonra kullanmak için değer.

   ![Azure Portal: uygulama özellikleri dikey penceresi](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Web uygulamanızı Azure App Service'te yayımlama

Bu bölümde, Visual Studio ve anında iletme bu depodan web uygulamanızı dağıtmak için Azure'a kullanarak yerel bir Git deposu oluşturur. Adımlar şunlardır:

   * Azure için yerel deponuza dağıtabilmek için GIT URL'si değerini kullanarak uzak depo ayarı ekleyin.

   * Proje değişiklikleri uygulayın.

   * Proje değişikliklerinizi yerel depodan Azure'da uzak deponuza iletin.

&nbsp;
   
1.  İçinde **Çözüm Gezgini** sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**. **Takım Gezgini** görüntülenir.

    ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/10-team-explorer.png)

2.  İçinde **Takım Gezgini**seçin **ev** (ev simgesi) > **ayarları** > **deposu ayarları**.

3.  İçinde **uzaktan kumandalar** bölümünü **deposu ayarları** seçin **Ekle**. **Eklemek uzak** iletişim kutusu görüntülenir.

4.  Ayarlama **adı** uzak **Azure ÖrnekUyg**.

5.  Değeri ayarlanamıyor **Fetch** için **Git URL'si** Azure'dan Bu öğreticide daha önce kopyaladığınız. Bu ile biten URL olduğuna dikkat edin **.git**.

    ![Uzaktan iletişim Düzenle](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >Alternatif olarak, uzak depodan belirtebilirsiniz **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve komutunu girerek. Örneğin:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  Seçin **ev** (ev simgesi) > **ayarları** > **genel ayarları**. Adınızı ve e-posta adresinizi ayarlanmış olduğundan emin olun. Seçmeniz gerekebilir **güncelleştirme**.

7.  Seçin **giriş** > **değişiklikleri** geri dönmek için **değişiklikleri** görünümü.

8.  Bir kaydetme iletisi gibi girin **ilk anında #1** tıklatıp **tamamlama**. Bu eylem oluşturacak bir *tamamlama* yerel olarak. Sonra *eşitleme* Azure ile.

    ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >Alternatif olarak, değişikliklerinizi gelen onaylayabilirsiniz **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutları girerek. Örneğin:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  Seçin **giriş** > **eşitleme** > **Eylemler** > **komut istemi açın**. Komut isteminde, proje dizinine açılır.

10.  Komut penceresinde aşağıdaki komutu girin:

    `git push -u Azure-SampleApp master`

11.  Azure girin **dağıtım kimlik bilgileri** Azure'da daha önce oluşturulan parola.

    >[!NOTE]
    >Girdiğiniz parola görünür olmaz.

    Bu komut, yerel proje dosyalarınızın Azure'a dağıtmaya işlemi başlar. Yukarıdaki komut çıktısı dağıtımının başarılı bir ileti ile sona erer.
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > Bir proje üzerinde işbirliği yapmak gereksinim duyarsanız için itme düşünmelisiniz [GitHub](https://github.com) arasında Azure'a dağıtmaya.
 
### <a name="verify-the-active-deployment"></a>Etkin dağıtımı doğrulama

Başarılı bir şekilde web uygulamasını yerel ortamınızdan Azure'a aktarılan olduğunu doğrulayabilirsiniz. Listelenen başarılı dağıtım görürsünüz.

1. İçinde [Azure Portal](https://portal.azure.com), web uygulamanızı seçin. Ardından, seçin **ayarları** > **sürekli dağıtım**.

   ![Azure Portal: Ayarlar dikey: dağıtımları dikey penceresi başarılı dağıtımı gösterme](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Uygulamayı Azure'da çalıştırma

Web uygulamanız için Azure dağıttıysanız, uygulamayı çalıştırabilirsiniz.

Bu iki yolla yapılabilir:

* Azure portalında web uygulamanız için web uygulaması dikey bulun ve tıklatın **Gözat** varsayılan tarayıcınızda uygulamanızı görüntülemek için.

* Bir tarayıcı açın ve web uygulamanız için URL'yi girin. Örneğin:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Web uygulamanızı güncelleştirin ve yeniden yayımlamanız

Yerel koda değişiklikleri yaptıktan sonra yayımlayabilirsiniz.

1.  İçinde **Çözüm Gezgini** Visual Studio açık *haline* dosya.

2.  İçinde `Configure` yöntemini değiştirme `Response.WriteAsync` şekilde aşağıdaki gibi görünmesi yöntemi:

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  Değişiklikleri kaydedilsin *haline*.

4.  İçinde **Çözüm Gezgini**, sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**. **Takım Gezgini** görüntülenir.

5.  Bir kaydetme iletisi gibi girin:

    ```none
    Update #2
    ```

6.  Tuşuna **tamamlama** proje değişiklikleri yürütmek için düğmesi.

7.  Seçin **giriş** > **eşitleme** > **Eylemler** > **anında**.

>[!NOTE]
>Alternatif olarak, değişikliklerden gönderebilir **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutunu girerek. Örneğin:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Görünüm Azure güncelleştirilmiş web uygulaması

Güncelleştirilen web uygulamanızı seçerek görüntülemek **Gözat** Azure Portalı'nda veya bir tarayıcı açıp web uygulamanız için URL girerek web uygulaması dikey penceresinden. Örneğin:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Ek Kaynaklar

* [Yayımlama ve dağıtım](index.md)

* [Proje Kudu](https://github.com/projectkudu/kudu/wiki)
