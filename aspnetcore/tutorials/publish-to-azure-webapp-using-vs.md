---
title: "Visual Studio kullanarak Azure ASP.NET Core uygulama yayımlama"
author: rick-anderson
description: "Visual Studio kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin."
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: d0e64c967ff332365981338809a47faf35d499ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), ve [Rachel Appel](https://twitter.com/rachelappel)

Bkz: [Mac için Visual Studio'dan Azure Yayımla](https://blog.xamarin.com/publish-azure-visual-studio-mac/) Mac'te çalışıyorsanız

## <a name="set-up"></a>Ayarlama

* Açık bir [ücretsiz Azure hesabı](https://aka.ms/K5y5yh) , yoksa. 

## <a name="create-a-web-app"></a>Bir web uygulaması oluşturma

Visual Studio Başlangıç sayfasında, seçin **Dosya > Yeni > Proje...**

![Dosya menüsü](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

Tamamlamak **yeni proje** iletişim:

* Sol bölmede seçin **.NET Core**.
* Orta bölmede seçin **ASP.NET çekirdek Web uygulaması**.
* Seçin **Tamam**.

![Yeni Proje iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

İçinde **çekirdek yeni bir ASP.NET Web uygulaması** iletişim:

* Seçin **Web uygulaması**.
* Seçin **değiştirmek kimlik doğrulama**.

![Yeni Proje iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

**Kimlik doğrulamayı Değiştir** iletişim kutusu görüntülenir. 

* Seçin **bireysel kullanıcı hesapları**.
* Seçin **Tamam** geri dönmek için **çekirdek yeni bir ASP.NET Web uygulaması**seçeneğini belirleyip **Tamam** yeniden.

![Yeni ASP.NET çekirdek Web kimlik doğrulaması iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio çözümü oluşturur.

## <a name="run-the-app"></a>Uygulama Çalıştırma

* Projeyi çalıştırmak için CTRL + F5 tuşuna basın.
* Test **hakkında** ve **kişi** bağlantılar.

![Web uygulamasını localhost üzerinde Microsoft Edge'de açın](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a>Bir kullanıcı kaydı

* Seçin **kaydetmek** ve yeni bir kullanıcı kaydedin. Kurgusal bir e-posta adresini kullanabilirsiniz. Gönderdiğinizde, aşağıdaki hata sayfasını görüntüler:

    *"Dahili Sunucu hatası: veritabanı işlemi başarısız oldu istek işlenirken. SQL özel durumu: veritabanı açılamıyor. Uygulama DB bağlamı için varolan geçişler uygulama bu sorun çözülebilir."*
* Seçin **uygulamak geçişler** ve Sayfa güncelleştirmelerini Yenile sonra sayfa.

![İç sunucu hatası: İstek işlenirken bir veritabanı işlemi başarısız oldu. SQL özel durumu: veritabanı açılamıyor. Uygulama DB bağlamı için varolan geçişler uygulama bu sorun çözülebilir.](publish-to-azure-webapp-using-vs/_static/mig.png)

Yeni kullanıcı kaydetmek için kullanılan e-posta uygulaması görüntüler ve **oturumunuzu** bağlantı.

![Web uygulaması Microsoft Edge'de açın. YAZMAÇ bağlantı Hello metni değiştirilir email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Uygulamayı Azure'a dağıtma

Çözüm Gezgini'nde projeye sağ tıklayın ve **Yayımla...** .

![Bağlamsal menü vurgulanmış Yayımla bağlantısıyla Aç](publish-to-azure-webapp-using-vs/_static/pub.png)

İçinde **Yayımla** iletişim:

* Seçin **Microsoft Azure uygulama hizmeti**.
* Dişli simgesini seçin ve ardından **profili oluştur**.
* Seçin **profili oluşturma**.

![Yayımlama iletişim kutusu](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a>Azure kaynakları oluşturun

**App Service Oluştur** iletişim kutusu görüntülenir:

* Aboneliğinizi girin.
* **App Name**, **kaynak grubu**, ve **App Service planı** giriş alanları doldurulur. Bu adları tutun veya onları değiştirebilirsiniz.

![App Service iletişim](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Seçin **Hizmetleri** sekmesinde yeni bir veritabanı oluşturun.

* Yeşil seçin  **+**  yeni bir SQL veritabanı oluşturmak için simgesi

![Yeni SQL veritabanı](publish-to-azure-webapp-using-vs/_static/sql.png)

* Seçin **yeni...**  üzerinde **SQL veritabanını yapılandırma** yeni bir veritabanı oluşturmak için iletişim kutusu.

![Yeni SQL veritabanı ve sunucu](publish-to-azure-webapp-using-vs/_static/conf.png)

**SQL Server'ı Yapılandır** iletişim kutusu görüntülenir.

* Bir yönetici kullanıcı adı ve parola girin ve ardından **Tamam**. Varsayılan tutabilirsiniz **sunucu adı**. 

> [!NOTE]
> "Yönetici" Yönetici kullanıcı adı olarak izin verilmiyor.

![SQL Server iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Seçin **Tamam**.

Visual Studio döner **App Service Oluştur** iletişim.

* Seçin **oluşturma** üzerinde **App Service Oluştur** iletişim.

![SQL veritabanı iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/conf_final.png)

Visual Studio, Azure üzerinde SQL Server ve Web uygulaması oluşturur. Bu adım birkaç dakika sürebilir. Oluşturulan kaynakları hakkında daha fazla bilgi için bkz: [cihazıyla kaynakları](#additonal-resources).

Dağıtım tamamlandığında seçin **ayarları**:

![SQL Server iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/set.png)

Üzerinde **ayarları** sayfasında **Yayımla** iletişim:

  * Genişletme **veritabanları** ve denetleme **çalışma zamanında Bu bağlantı dizesini kullan**.
  * Genişletme **Entity Framework geçişler** ve denetleme **bu geçiş yayımlama Uygula**.

* Seçin **kaydetmek**. Visual Studio döner **Yayımla** iletişim. 

![Yayımlama iletişim kutusu: ayarlar paneli](publish-to-azure-webapp-using-vs/_static/pubs.png)

Tıklatın **yayımlama**. Visual Studio publishs uygulamanızı azure'a. Depoyment tamamlandığında uygulamayı tarayıcıda açılır.

### <a name="test-your-app-in-azure"></a>Azure'da uygulamanızı test etme

* Test **hakkında** ve **kişi** bağlantılar

* Yeni bir kullanıcı kaydı

![Web uygulamasını Azure App Service'te Microsoft Edge açılır](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Uygulamayı güncelleştirme

* Düzen *Pages/About.cshtml* Razor sayfasında ve içeriğini değiştirin. Örneğin, "Merhaba ASP.NET Core!" söylemek için paragraf değiştirebilirsiniz:[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* Sağ tıklatın ve proje **Yayımla...**  yeniden.

![Bağlamsal menü vurgulanmış Yayımla bağlantısıyla Aç](publish-to-azure-webapp-using-vs/_static/pub.png)

* Uygulama yayımlandıktan sonra yaptığınız değişiklikleri Azure üzerinde kullanılabilir olduğundan emin olun.

![Görev tamamlandığında doğrulayın](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>Temizleme

Uygulamayı test etme işlemini tamamladığınızda, Git [Azure portal](https://portal.azure.com/) ve uygulamayı silin.

* Seçin **kaynak grupları**, oluşturduğunuz kaynak grubunu seçin.

![Azure Portal: Kaynak gruplarını kenar menüsü](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* İçinde **kaynak grupları** sayfasında, **silmek**.

![Azure Portal: Kaynak grupları sayfası](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Kaynak grubunun adını girin ve **silmek**. Artık uygulamanızı ve Bu öğreticide oluşturduğunuz diğer tüm kaynaklar Azure'dan silinir.

### <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio ve Git ile Azure için sürekli dağıtım](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a>Diğer kaynaklar

* [Azure uygulama hizmeti](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [Azure kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/)
