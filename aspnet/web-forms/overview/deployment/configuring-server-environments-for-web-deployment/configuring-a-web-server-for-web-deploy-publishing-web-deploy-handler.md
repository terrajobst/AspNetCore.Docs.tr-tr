---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: "Web için bir Web sunucusu yapılandırma dağıtımı yayımlamadan (Web dağıtımı işleyici) | Microsoft Docs"
author: jrjlee
description: "Bu konuda Web'de yayımlama ve IIS Web dağıtımı Han kullanarak dağıtımını desteklemek için Internet Information Services (IIS) web sunucusunun nasıl yapılandırılacağı açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 2127a98a0abf2c94e32b907d945c9b4d36fb2360
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Web için bir Web sunucusu yapılandırma dağıtımı yayımlamadan (Web dağıtımı işleyici)
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, web yayımlama ve IIS Web dağıtımı işleyicisi kullanarak dağıtım desteklemek için Internet Information Services (IIS) web sunucusunun nasıl yapılandırılacağı açıklanmaktadır.
> 
> Web dağıtımı 2.0 veya üstü çalışırken, uygulamaları veya bir web sunucusuna siteleri almak için kullanabileceğiniz üç ana yaklaşım vardır. Şunları yapabilirsiniz:
> 
> - Kullanım *Web dağıtımı uzak aracının*. Bu yaklaşım web sunucusunun daha az yapılandırma gerektirir, ancak hiçbir şey sunucuya dağıtmak için bir yerel sunucu yöneticisinin kimlik bilgilerini sağlamanız gerekir.
> - Kullanım *Web dağıtımı işleyici*. Bu yaklaşım, çok daha karmaşıktır ve web sunucusu kurmak için daha fazla ilk çaba gerektirir. Ancak, bu yaklaşım kullandığınızda, dağıtım gerçekleştirmek yönetici olmayan kullanıcıların izin vermek için IIS yapılandırabilirsiniz. Web dağıtımı işleyicisi yalnızca IIS 7 veya sonraki bir sürümü kullanılabilir.
> - Kullanım *çevrimdışı dağıtım*. Bu yaklaşım web sunucusunun en az yapılandırma gerektirir, ancak sunucu yönetici el ile web paketini sunucuya kopyalayın ve IIS Yöneticisi'yle alma.
> 
> Anahtar özellikler, olumlu ve olumsuz yönlerini Bu yaklaşımlar hakkında daha fazla bilgi için bkz: [Web dağıtımı sağ yaklaşımı seçme](choosing-the-right-approach-to-web-deployment.md).


Evet, yönetici olmayan kullanıcılar için belirli IIS Web siteleri içerik dağıtmak üzere izin vermek istiyorsanız. Bu yaklaşım, genellikle bu senaryoları türlerinde tercih edilir:

- Burada uzaktan dağıtım tetikler kişi ya da hizmet hesabı bir sunucu yöneticisinin kimlik bilgilerine erişiminiz olması beklenmez hazırlık veya üretim ortamları.
- Barındırılan ortamlarda, uzak kullanıcıların kendi Web siteleri, web sunucusu (veya başka birinin Web sitelerine erişim) üzerinde tam denetimi vermeden güncelleştirme vermek istediğiniz.

Geliştirme veya test senaryoları veya daha küçük kuruluşlar server Yöneticisi kimlik bilgilerini kullanarak dağıtma içerik genellikle küçük contentious. Bu senaryolarda, dağıtım kullanarak desteklemek için web sunucuları yapılandırma [Web Uzak Aracı hizmeti Dağıt](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) daha kolay bir yaklaşım sunmaktadır.

## <a name="task-overview"></a>Görev genel bakış

Kabul etmek ve Web dağıtımı işleyicisi yaklaşımı kullanarak uzak bir bilgisayardan web paketleri dağıtmak için web sunucusunu yapılandırmak için yapmanız:

- Oluşturun veya seçin, kimlik bilgileri dağıtımları gerçekleştirmek için kullanacağınız bir etki alanı kullanıcı hesabı ("yönetici olmayan kullanıcı").
- IIS 7.5, Web yönetimi hizmeti ve temel kimlik doğrulama modülü gibi yükleyin.
- Web dağıtımı 2.1 veya üstünü yükleyin.
- Uzak bağlantılara izin vermek için Web Yönetim hizmeti yapılandırın ve hizmeti başlatın.
- Dağıtılan içeriğini barındırmak için bir IIS Web sitesi oluşturun.
- IIS Yöneticisi'nde Web sitenizi, yönetici olmayan kullanıcı izinlerini verin.
- Web Yönetim hizmeti temsilci kuralları ekleyin ve yönetici olmayan kullanıcı hesabınızı kullanarak Web sitesi içeriğini değiştirmek için hizmeti izin olun.
- Bağlantı noktası 8172 gelen bağlantılara izin vermek için tüm güvenlik duvarlarını yapılandırın.

ContactManager örnek çözümü özel olarak barındırmak için için gerekir:

- .NET Framework 4.0 yükleyin.
- ASP.NET MVC 3 yükleyin.

Bu konuda her bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir. Görevleri ve bu konudaki yönergeler Windows Server 2008 R2 çalıştıran bir temiz server yapı ile başlatıyorsanız varsayalım. Devam etmeden önce emin olun:

- Windows Server 2008 R2 Service Pack 1 ve kullanılabilir tüm güncelleştirmeleri yüklenir.
- Etki alanına katılmış sunucusudur.
- Sunucunun bir statik IP adresi vardır.

> [!NOTE]
> Bilgisayarlar bir etki alanına katılma ile ilgili daha fazla bilgi için bkz: [katılan bilgisayarların etki alanı ve günlüğü üzerinde](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx). Statik IP adreslerini yapılandırma hakkında daha fazla bilgi için bkz: [bir statik IP adresi yapılandırın](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Ürünler ve bileşenlerini yükler

Bu bölümde, bileşenleri ve gerekli ürün web sunucusunda yüklenmesinde size kılavuzluk eder. Başlamadan önce sunucunuzun tam olarak güncel olduğundan emin olmak için Windows Update'i çalıştırın için iyi bir uygulama olur.

Bu durumda, bunları yüklemeniz gerekir:

- **IIS 7 önerilen Yapılandırması**. Böylece **Web sunucusu (IIS)** web sunucunuz üzerinde rol ve IIS modüllerini ve ASP.NET uygulamasını barındırmak için gereken bileşenler kümesi yükler.
- **IIS: Yönetim hizmeti**. Bu, IIS'de Web Yönetimi Hizmeti (WMSvc) yükler. Bu hizmet, IIS Web siteleri uzaktan yönetilmesini sağlar ve istemcilere Web dağıtımı işleyicisi uç noktasını kullanıma sunar.
- **IIS: Temel kimlik doğrulaması**. IIS temel kimlik doğrulama modülünü yükler. Bu Web Yönetimi Hizmeti (WMSvc) sağlar, verdiğiniz kimlik bilgileri kimlik doğrulaması.
- **Web dağıtım aracı 2.1 veya üzeri**. Bu, sunucunuzda Web dağıtımı (ve onun temel yürütülebilir MSDeploy.exe) yükler. Bu işlemin bir parçası olarak bu Web dağıtımı işleyicisi yükler ve Web yönetimi hizmeti ile tümleşir.
- **.NET framework 4.0**. Bu, .NET Framework'ün bu sürümünde oluşturulan uygulamaları çalıştırmak için gereklidir.
- **ASP.NET MVC 3**. Bu MVC 3 uygulamaları çalıştırmak için gereken derlemeler yükler.

> [!NOTE]
> Bu kılavuzda yüklemek ve çeşitli bileşenleri yapılandırmak için Web Platformu yükleyicisi kullanımını açıklar. Web Platformu yükleyicisi kullanmak zorunda değilsiniz rağmen otomatik olarak bağımlılıkları algılamasını ve her zaman en son ürün sürümleri alma sağlayarak yükleme işlemini basitleştirir. Daha fazla bilgi için bkz: [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/?linkid=9805118).


**Gerekli ürün ve bileşenlerini yüklemek için**

1. İndirme ve yükleme [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118).
2. Yükleme tamamlandığında, Web Platformu yükleyicisi otomatik olarak başlatır.

    > [!NOTE]
    > Web Platformu yükleyicisi şimdi istediğiniz zaman başlatabilir **Başlat** menüsü. Bunu yapmak için **Başlat** menüsünde tıklatın **tüm programlar**ve ardından **Microsoft Web Platformu yükleyicisi**.
3. Üstündeki **Web Platformu yükleyicisi 3.0** penceresinde tıklatın **ürünleri**.
4. Gezinti bölmesinde, pencerenin sol tarafındaki tıklatın **çerçeveler**.
5. İçinde **Microsoft .NET Framework 4** .NET Framework zaten yüklü değilse, satır tıklatın **Ekle**.

    > [!NOTE]
    > Windows Update aracılığıyla .NET Framework 4.0 zaten yüklenmiş. Bir ürün veya bileşenin zaten yüklüyse, Web Platformu yükleyicisi bu değiştirerek gösterecektir **Ekle** düğmesi metni ile **yüklü**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. İçinde **ASP.NET MVC 3 (Visual Studio 2010)** satır, tıklatın **Ekle**.
7. Gezinti bölmesinde **Server**.
8. İçinde **IIS 7 önerilen Yapılandırması** satır, tıklatın **Ekle**.
9. İçinde **Web dağıtım aracı 2.1** satır, tıklatın **Ekle**.
10. İçinde **IIS: temel kimlik doğrulaması** satır, tıklatın **Ekle**.
11. İçinde **IIS: Yönetim hizmeti** satır, tıklatın **Ekle**.
12. **Yükle**'ye tıklatın. Web Platformu yükleyicisi ürünleri & #x 2014; herhangi bir ilişkili bağımlılıkları & #x 2014; yüklenecek birlikte listesini gösterir ve lisans koşullarını kabul isteyip istemediğinizi sorar.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Lisans koşullarını gözden geçirin ve koşullarını kabul ederseniz tıklayın **kabul ediyorum**.
14. Yükleme tamamlandığında, tıklatın **son**ve ardından kapatın **Web Platformu yükleyicisi 3.0** penceresi.

IIS yüklemeden önce .NET Framework 4.0 yüklü değilse, çalıştırmanız gerekir [ASP.NET IIS Kayıt Aracı](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) IIS ile ASP.NET en son sürümünü kaydetmek için. Bunu yapmazsanız, sorunsuz IIS (HTML dosyaları gibi) statik içerik sunmanızı bulabilirsiniz, ancak onu döndürür **HTTP Hatası 404.0 – bulunamadı** ASP.NET içeriğe göz atmak çalıştığınızda. ASP.NET 4.0 kayıtlı olduğundan emin olmak için bir sonraki yordamı kullanın.

**IIS ile ASP.NET 4.0 kaydetmek için**

1. Tıklatın **Başlat**ve ardından **komut istemi**.
2. Arama sonuçlarında sağ **komut istemi**ve ardından **yönetici olarak çalıştır**.
3. Komut İstemi penceresine gidin **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** dizini.
4. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Ana bilgisayar 64-bit web uygulamaları herhangi bir noktada planlıyorsanız, ASP.NET 64-bit sürümünü de IIS ile kaydetmelisiniz. Bunu komut istemi penceresine gidin **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** dizini.
6. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

İyi bir uygulama, Windows Update'i yeniden bu noktada yeni ürünler ve yüklemiş olduğunuz bileşenler için güncelleştirmeleri karşıdan yükleyip kullanın.

## <a name="configure-the-web-management-service"></a>Web yönetimi hizmeti yapılandırın

Gereksinim duyduğunuz her şeyi yüklediniz, sonraki adıma IIS'de Web yönetimi hizmeti yapılandırmaktır. Yüksek düzeyde, bu görevleri tamamlamak gerekir:

- Sunucu düzeyinde temel kimlik doğrulamasını etkinleştirin.
- Uzak bağlantıları kabul etmek için Web Yönetim hizmetini yapılandırın.
- Web yönetimi hizmeti başlatın.
- Gerekli Web Yönetim hizmeti temsilci kuralları yerinde olduğundan emin olun.

**Web yönetimi hizmeti yapılandırmak için**

1. Üzerinde **Başlat** menüsündeki **Yönetimsel Araçlar**ve ardından **Internet Information Services (IIS) Yöneticisi'ni**.
2. IIS Yöneticisi ' nde, **bağlantıları** bölmesinde sunucu düğümünü tıklatın (örneğin, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Orta bölmede altında **IIS**, çift **kimlik doğrulama**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. Sağ **temel kimlik doğrulaması**ve ardından **etkinleştirmek**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. İçinde **bağlantıları** bölmesinde tekrar en üst düzey ayarlara dönmek için sunucu düğümünü tıklatın.
6. Orta bölmede altında **Yönetim**, çift **yönetim hizmeti**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Orta bölmede seçin **uzak bağlantıları etkinleştirmek**.

    > [!NOTE]
    > Web yönetimi hizmeti zaten çalışıyorsa, önce durdurmanız gerekir.
8. İçinde **Eylemler** bölmesinde tıklatın **Başlat** Web yönetimi hizmeti başlatmak için.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Ayarlarınızı kaydetmek için istenirse tıklatın **Evet**.

    > [!NOTE]
    > Hizmeti otomatik olarak başlayacak şekilde yapılandırmak isteyebilirsiniz. Bunu yapmak için Hizmetler konsolunu açın, sağ **Web Yönetim hizmeti**ve ardından **özellikleri**. İçinde **başlangıç türü** açılır listesinden, **otomatik**ve ardından **Tamam**.
10. İçinde **bağlantıları** bölmesinde tekrar en üst düzey ayarlara dönmek için sunucu düğümünü tıklatın.
11. Orta bölmede altında **Yönetim**, çift **yönetim hizmeti temsilci**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Orta bölmede bir kural kümesi içerdiğini doğrulayın.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Bu kurallar, çeşitli Web dağıtımı sağlayıcıları kullanmak yetkili Web yönetimi hizmeti kullanıcıların sağlar. Örneğin, IIS Web dağıtımı işleyicisi üzerinden web uygulamaları ve içerik dağıtmak için koyulmalıdır kimliği doğrulanmış tüm kullanmak için Web Yönetim hizmeti kullanıcılara izin veren bir temsilci kuralı **contentPath** ve **Iisapp**  sağlayıcıları (ekran görüntüsünde görebilirsiniz son kuralı).

    Bu konuda açıklanan sırayla ürünlerini ve bileşenleri yüklediyseniz, Web Dağıtımı'nın en son sürümünü otomatik olarak tüm gerekli temsilci kuralları için Web Yönetim hizmeti eklemeniz gerekir. Yönetim hizmeti temsilci sayfasına herhangi bir kuralın görünmüyorsa, bunları kendiniz oluşturmanız gerekir. Bunun nasıl yapılacağı hakkında yönergeler için bkz [Web dağıtım işleyicisi yapılandırma](https://go.microsoft.com/?linkid=9805124).
13. İçinde **bağlantıları** bölmesinde tekrar en üst düzey ayarlara dönmek için sunucu düğümünü tıklatın.

## <a name="create-and-configure-an-iis-website"></a>Oluşturma ve bir IIS Web sitesi yapılandırma

Sunucunuza web içeriği dağıtmadan önce oluşturma ve bir IIS Web sitesi içeriğini barındırmak için yapılandırma gerekir. Web dağıtımı, yalnızca mevcut bir IIS Web sitesine web paketleri dağıtabilirsiniz; Web sitesi sizin için oluşturulamaz. Ayrıca yönetici olmayan hesabınızı uzaktan içerik dağıtmak üzere izin vermek için çok az bir ek yapılandırma yapmanız gerekir. Yüksek düzeyde, bu görevleri tamamlamak gerekir:

- İçeriğinizi barındırmak üzere dosya sisteminde bir klasör oluşturun.
- İçerik sunmak için bir IIS Web sitesi oluşturma ve yerel klasör ile ilişkilendirebilirsiniz.
- Uygulama havuzu kimliği yerel klasör üzerindeki izinleri okuma verme.
- Web uygulamanızı dağıtacağınız etki alanı hesabı için gereken IIS izinleri verin.

Olmasına karşın bir şey IIS'de varsayılan Web sitesi için içerik dağıtma durdurma test ya da gösterim senaryoları dışında her şey için bu yaklaşım önerilmez. Bir üretim ortamının benzetimini yapmak için uygulamanızın gereksinimlerine belirli ayarlarla yeni bir IIS Web sitesi oluşturmanız gerekir.

**Bir IIS Web sitesi oluşturmak için**

1. Yerel dosya sisteminde içeriğinizi depolamak için bir klasör oluşturun (örneğin, **C:\DemoSite**).
2. Üzerinde **Başlat** menüsündeki **Yönetimsel Araçlar**ve ardından **Internet Information Services (IIS) Yöneticisi'ni**.
3. IIS Yöneticisi ' nde, **bağlantıları** bölmesinde sunucu düğümünü genişletin (örneğin, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Sağ **siteleri** düğümünü ve ardından **Web sitesi Ekle**.
5. İçinde **Site adı** IIS Web sitesi için bir ad yazın (örneğin, **DemoSite**).
6. İçinde **fiziksel yolu** kutusuna yazın (veya göz atın), yerel klasör yolu (örneğin, **C:\DemoSite**).
7. İçinde **bağlantı noktası** kutusunda, istediğiniz Web sitesini barındırmak bağlantı noktası numarasını yazın (örneğin, **85**).

    > [!NOTE]
    > Standart bağlantı noktası numaralarını, HTTP için 80 ve HTTPS için 443 ' dir. Ancak, bu Web sitesi bağlantı noktası 80 üzerinde barındırıyorsanız, sitenizi erişebilmeniz için önce varsayılan Web sitesini Durdur gerekir.
8. Bırakın **ana bilgisayar adı** kutusuna Web sitesi için bir etki alanı adı sistemi (DNS) kaydını yapılandırın ve ardından istemediğiniz sürece boş **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > Bir üretim ortamında, büyük olasılıkla, Web sitesi bağlantı noktası 80 üzerinde barındırmak ve DNS kayıtlarını eşleşen birlikte bir konak üstbilgisi yapılandırma istersiniz. IIS 7'de konak üstbilgileri yapılandırma hakkında daha fazla bilgi için bkz: [bir Web sitesi (IIS 7) için bir konak üstbilgisi yapılandırma](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx). Windows Server 2008 R2'de DNS sunucusu rolü hakkında daha fazla bilgi için bkz: [DNS sunucusuna genel bakış](https://technet.microsoft.com/en-gb/library/cc770392.aspx) ve [DNS sunucusu](https://technet.microsoft.com/en-us/windowsserver/dd448607).
9. **Eylemler** bölmesinde, **Site Düzenle**altında, **Bağlamalar**'ı tıklatın.
10. İçinde **Site bağlamaları** iletişim kutusu, tıklatın **Ekle**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. İçinde **Site bağlaması Ekle** iletişim kutusu, kümesi **IP adresi** ve **bağlantı noktası** var olan site yapılandırmanızı eşleşecek şekilde.
12. İçinde **ana bilgisayar adı** web sunucunuzun adını yazın (örneğin, **STAGEWEB1**) ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > İlk site bağlaması IP adresi ve bağlantı noktasını kullanarak yerel olarak site erişmenize olanak sağlayan veya `http://localhost:85`. İkinci site bağlaması makine adı (örneğin, http://stageweb1:85) kullanarak etki alanındaki diğer bilgisayarlardan site erişmenize olanak tanır.
13. İçinde **Site bağlamaları** iletişim kutusu, tıklatın **Kapat**.
14. İçinde **bağlantıları** bölmesinde tıklatın **uygulama havuzları**.
15. İçinde **uygulama havuzları** bölmesinde, uygulama havuzunun adını sağ tıklatın ve ardından **temel ayarları**. Varsayılan olarak, Web sitenizin adı, uygulama havuzu adı ile eşleşir (örneğin, **DemoSite**).
16. İçinde **.NET Framework sürüm** listesinde **.NET Framework v4.0. 30319**ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > Örnek çözümü .NET Framework 4.0 gerektirir. Bu gereksinim Web dağıtımı için genel değil.

İçerik sunmak Web siteniz için sırada uygulama havuzu kimliği içeriği depolayan yerel klasör üzerinde Okuma izinleriniz olmalıdır. IIS 7.5 uygulama havuzları (aksine, IIS uygulama havuzları genellikle ağ hizmeti hesabını kullanarak çalıştırdığı, önceki sürümler) varsayılan olarak benzersiz uygulama havuzu kimliği ile çalıştırın. Uygulama havuzu kimliği gerçek kullanıcı hesabı değil ve tüm kullanıcılar veya gruplar & #x 2014 listelerde görünmüyor; uygulama havuzunu başlatıldığında, bunun yerine, bunu dinamik olarak oluşturulur. Her uygulama havuzu kimliği yerel eklenir **IIS\_IUSRS** gizli bir öğe olarak güvenlik grubu.

Bir uygulama havuzu kimliği bir dosya veya klasör izinleri vermek için iki seçeneğiniz vardır:

- İzinleri biçimi kullanarak uygulama havuzu kimliği için doğrudan atayın. **IIS AppPool\***[uygulama havuzu adı] * (örneğin, **IIS AppPool\DemoSite**).
- İzinleri atayın **IIS\_IUSRS** grubu.

Yerel izinler atamak için en yaygın yaklaşımdır **IIS\_IUSRS** grubunda olduğundan bu yaklaşım, dosya sistemi izinleri yapılandırmadan uygulama havuzları değiştirmenize olanak sağlar. Sonraki yordamda bu grup tabanlı yaklaşımı kullanır.

> [!NOTE]
> IIS 7.5, uygulama havuzu kimlikleri hakkında daha fazla bilgi için bkz: [uygulama havuzu kimlikleri](https://go.microsoft.com/?linkid=9805123).


**Bir IIS Web sitesi için klasör izinlerini yapılandırmak için**

1. Windows Gezgini'nde yerel klasör konumuna gidin.
2. Klasöre sağ tıklayın ve ardından **özellikleri**.
3. Üzerinde **güvenlik** sekmesini tıklatın, **Düzenle**ve ardından **Ekle**.
4. Tıklatın **konumları**. İçinde **konumları** iletişim kutusunda, yerel sunucuyu seçin ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. İçinde **kullanıcıları veya Grupları Seç** iletişim kutusuna **IIS\_IUSRS**, tıklatın **Adları Denetle**ve ardından **Tamam**.
6. İçinde **izinlerini***[klasör adı]* iletişim kutusunda, yeni Grup atanan bildirim **okuma &amp; yürütme**, **liste klasörü içeriği**, ve **okuma** varsayılan izinleri. Bu değiştirmeden bırakın ve tıklatın **Tamam**.
7. Tıklatın **Tamam** kapatmak için *[klasör adı]***özellikleri** iletişim kutusu.

Son bir görev olarak kimlik içerik dağıtmak için kullanacağınız yönetici olmayan kullanıcı uygun izinleri vermeniz gerekir. Bu kullanıcı içerik uzaktan Web sitenize dağıtmak için izinler gerektirir.

**Yönetici olmayan etki alanı kullanıcısı için IIS Web sitesi izinlerini yapılandırmak için**

1. IIS Yöneticisi ' nde içinde **bağlantıları** bölmesinde, Web sitesi düğümüne sağ tıklayın (örneğin, **DemoSite**), işaret **dağıtma**ve ardından **Web yapılandırma Yayımlama dağıtmak**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. İçinde **yapılandırma Web dağıtımı yayımlama** iletişim kutusu, sağındaki **yayımlama izin vermek için bir kullanıcı seçin** listesinde, üç nokta düğmesini tıklatın.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. İçinde **kullanıcıya izin ver** iletişim kutusunda, etki alanı ve kullanıcı içerik dağıtmak ve ardından kullanmak istediğiniz hesabın adını yazın **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. İçinde **yapılandırma Web dağıtımı yayımlama** iletişim kutusu, tıklatın **Kurulum**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Bu işlem tek bir adımda iki önemli işlevleri gerçekleştirir. İlk olarak, önceki bölümde incelenmesi temsilci kurallarına göre kullanıcı Web sitesi Web yönetimi hizmeti üzerinden uzaktan değiştirme izni verir. İkinci olarak, kaynak klasörün eklemek, değiştirmek ve Web sitesi içeriğini izinleri olanak tanır. Web sitesi için kullanıcı tam denetim verir.
5. İçinde **yapılandırma Web dağıtımı yayımlama** iletişim kutusu, tıklatın **Kapat**.

## <a name="configure-firewall-exceptions"></a>Güvenlik Duvarı özel durumlarını yapılandırın

Varsayılan olarak, IIS Web Yönetim hizmeti 8172 numaralı TCP bağlantı noktasını dinler. Web sunucunuz üzerinde Windows Güvenlik Duvarı etkinse (giden tüm trafiği Windows Güvenlik Duvarı varsayılan olarak izin verilir) 8172 numaralı bağlantı noktasındaki TCP trafiğine izin veren yeni bir gelen kuralı oluşturmanız gerekir. Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, trafiğine izin verme kuralları oluşturmanız gerekir.

| Yön | Bağlantı noktasından | Bağlantı noktası | Bağlantı noktası türü |
| --- | --- | --- | --- |
| Gelen | tüm | 8172 | TCP |
| Giden | 8172 | tüm | TCP |
  

Windows Güvenlik duvarı kuralları yapılandırma hakkında daha fazla bilgi için bkz: [güvenlik duvarı kurallarını yapılandırma](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx). Üçüncü taraf güvenlik duvarları için lütfen ürün belgelerine bakın.

## <a name="conclusion"></a>Sonuç

Web sunucunuzun artık Web dağıtımı işleyicisine Web Yönetim hizmeti aracılığıyla uzak dağıtımları kabul etmeye hazır olması gerekir. Sunucu bir web uygulamasına dağıtmayı denemeden önce aşağıdaki önemli noktaları denetlemek isteyebilirsiniz:

- IIS'de sunucu düzeyinde temel kimlik doğrulaması etkin?
- Web yönetimi hizmeti için Uzak bağlantılar etkin mi?
- Web yönetimi hizmeti başlattığınız?
- Yönetim hizmeti temsilci kuralları yerinde var mı?
- Uygulama havuzu kimliğini Web siteniz için kaynak klasöre okuma erişimi var mı?
- Yönetici olmayan bir kullanıcı hesabını site düzeyinde izinler IIS'de var mı?
- Güvenlik duvarının TCP bağlantı noktası 8172 sunucuda gelen bağlantılara izin veriyor mu?

## <a name="further-reading"></a>Daha Fazla Bilgi

Web dağıtımı işleyicisine web paketleri dağıtmak için özel Microsoft Build Engine (MSBuild) proje dosyalarını yapılandırma hakkında yönergeler için bkz [hedef ortam için dağıtım özellikleri yapılandırma](configuring-deployment-properties-for-a-target-environment.md).

>[!div class="step-by-step"]
[Önceki](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[sonraki](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
