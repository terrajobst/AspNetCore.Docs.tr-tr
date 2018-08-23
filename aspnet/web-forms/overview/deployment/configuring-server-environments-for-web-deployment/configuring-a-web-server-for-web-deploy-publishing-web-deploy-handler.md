---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Bir Web sunucusunu yapılandırma için Web dağıtımı yayımlama (Web dağıtımı işleyicisi) | Microsoft Docs
author: jrjlee
description: Bu konu, Web'de yayımlama ve dağıtma IIS Web Han kullanarak dağıtımını desteklemek için bir Internet Information Services (IIS) web sunucusu yapılandırmanız açıklar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 13e4fdf77daf26abe837a90db9c11ecbe1957823
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752120"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Bir Web sunucusunu yapılandırma için Web dağıtımı yayımlama (Web dağıtımı işleyicisi)
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, Web'de yayımlama ve IIS Web dağıtımı işleyicisi kullanarak dağıtımını desteklemek için bir Internet Information Services (IIS) web sunucusu yapılandırma konusunda açıklanmaktadır.
> 
> Web dağıtımı 2.0 veya üzeri çalışırken, uygulamaları veya web sunucusuna siteleri almak için kullanabileceğiniz üç ana yaklaşım vardır. Şunları yapabilirsiniz:
> 
> - Kullanım *Web dağıtımı aracı uzak hizmet*. Bu yaklaşım web sunucusunun daha az yapılandırma gerektirir, ancak hiçbir şey sunucuya dağıtmak için bir yerel sunucu yöneticisinin kimlik bilgilerini sağlamanız gerekir.
> - Kullanım *Web dağıtımı işleyicisi*. Bu yaklaşım, çok daha karmaşıktır ve web sunucusu kurmak için ilk daha fazla çaba gerektirir. Ancak, bu yaklaşımı kullandığınızda, dağıtım gerçekleştirmek yönetici olmayan kullanıcıların IIS yapılandırabilirsiniz. Web dağıtımı işleyicisi, yalnızca IIS 7 veya sonraki bir sürümü kullanılabilir.
> - Kullanım *çevrimdışı dağıtım*. Bu yaklaşım web sunucusunun en az yapılandırma gerektirir, ancak sunucu yönetici el ile web paketi sunucuya kopyalayın ve IIS Yöneticisi'yle içe aktarın.
> 
> Anahtar özellikler, avantajları ve dezavantajları bu yaklaşımların hakkında daha fazla bilgi için bkz. [Web dağıtımı için doğru yaklaşımı seçme](choosing-the-right-approach-to-web-deployment.md).


Evet, yönetici olmayan kullanıcılar için belirli IIS Web siteleri içerik dağıtmak izin vermek istiyorsanız. Bu yaklaşım genellikle bu senaryoları türlerinde tercih edilir:

- Burada uzak dağıtım tetikler kişi ya da hizmet hesabı, Sunucu Yöneticisi kimlik bilgilerine erişiminiz olması beklenmez hazırlık veya üretim ortamları.
- Barındırılan ortamlarda, uzak kullanıcıların kendi Web siteleri, web sunucuları (veya başkasını Web sitelerine erişim) üzerinde tam denetim vermeden güncelleştirme olanağı vermek istediğiniz.

Geliştirme ve test senaryolarında veya daha küçük kuruluşlar, Sunucu Yöneticisi kimlik bilgilerini kullanarak dağıtma içeriği genellikle küçük contentious. Bu senaryolarda, web sunucularınızın dağıtımını kullanarak destekleyecek şekilde yapılandırma [Web Uzak Aracı hizmeti Dağıt](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) daha basit bir yaklaşım sunar.

## <a name="task-overview"></a>Görev genel bakış

Kabul etmek ve Web dağıtımı işleyicisi yaklaşımı kullanarak uzak bir bilgisayardan web paketlerini dağıtmak için web sunucusunu yapılandırmak için yapmanız:

- Oluşturun veya seçin, kimlik bilgileri, dağıtımları gerçekleştirmek için kullanacağınız bir etki alanı kullanıcı hesabını ("yönetici olmayan kullanıcı").
- IIS 7.5, Web yönetimi hizmeti ve temel kimlik doğrulama modülü gibi yükleyin.
- Web dağıtımı 2.1 veya üzerini yükleyin.
- Web yönetimi hizmeti uzak bağlantılara izin verecek şekilde yapılandırın ve hizmeti başlatın.
- Dağıtılan içerik barındırmak için bir IIS Web sitesi oluşturun.
- IIS Yöneticisi'nde Web sitenize, yönetici olmayan kullanıcı izinleri verin.
- Web yönetimi hizmeti temsilci kuralları ekleme ve yönetici olmayan kullanıcı hesabınızı kullanarak Web sitesi içeriğini değiştirmek için hizmeti izin emin olun.
- Tüm güvenlik duvarlarının 8172 numaralı bağlantı noktasında gelen bağlantılara izin verecek şekilde yapılandırın.

Özellikle ContactManager örnek çözüm barındırmak için için gerekir:

- .NET Framework 4. 0'ı yükleyin.
- ASP.NET MVC 3 yükleyin.

Bu konuda, bu yordamların her biri gerçekleştirme gösterilmektedir. Bu konudaki yönergeler ve görevleri ile birlikte Windows Server 2008 R2 çalıştıran bir temiz sunucusu derleme başlatılıyor varsayılır. Devam etmeden önce şunlardan emin olun:

- Windows Server 2008 R2 Service Pack 1 ve tüm kullanılabilir güncelleştirmeler yüklenir.
- Etki alanına katılmış sunucusudur.
- Sunucuda bir statik IP adresi var.

> [!NOTE]
> Bilgisayarlarının bir etki alanına katılmasını sağlama hakkında daha fazla bilgi için bkz: [katılan bilgisayarların etki alanı ve günlüğe kaydetme üzerinde](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Statik IP adreslerini yapılandırma hakkında daha fazla bilgi için bkz. [statik bir IP adresi yapılandırın](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Ürünler ve bileşenlerini yükleme

Bu bölümde, bileşenleri ve gerekli ürün web sunucusunda yüklenmesinde size kılavuzluk eder. Başlamadan önce iyi sunucunuzun tam olarak güncel olduğundan emin olmak için Windows Update çalıştırmaktır.

Bu durumda, bunları yüklemeniz gerekir:

- **IIS 7 önerilen Yapılandırması**. Böylece **Web sunucusu (IIS)** , web sunucusu rolü ve IIS modüllerini ve bir ASP.NET uygulamasını barındırmak için gereken bileşenleri yükler.
- **IIS: Yönetim hizmeti**. Bu, IIS Web Yönetimi Hizmeti (WMSvc) yükler. Bu hizmet, IIS Web siteleri uzaktan yönetilmesini sağlar ve istemcilere Web dağıtımı işleyicisi uç noktasını kullanıma sunar.
- **IIS: Temel kimlik doğrulaması**. Bu, IIS temel kimlik doğrulama modülünü yükler. Bu sayede Web Yönetimi Hizmeti (WMSvc) sağladığınız kimlik.
- **Web dağıtım aracı 2.1 veya üzeri**. Bu seçenek, Web dağıtımı (ve temel alınan çalıştırılabilir, MSDeploy.exe) sunucunuzda yükler. Bu işlemin bir parçası olarak, Web dağıtımı işleyicisi yükler ve Web yönetimi hizmeti ile tümleştirilir.
- **.NET framework 4.0**. Bu, .NET Framework'ün bu sürümünde oluşturulmuş uygulamaları çalıştırmak için gereklidir.
- **ASP.NET MVC 3**. Bu, MVC 3 uygulamaları çalıştırmak için gereken bütünleştirilmiş kodları yükler.

> [!NOTE]
> Bu izlenecek yol çeşitli bileşenlerini yükleme ve yapılandırma için Web Platformu yükleyicisi kullanımını açıklar. Web Platformu Yükleyicisi'ni kullanmanız gerekmez ancak otomatik olarak bağımlılıkları algılamasını ve her zaman en son ürün sürümlerini alma sağlayarak yükleme işlemini basitleştirir. Daha fazla bilgi için [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/?linkid=9805118).


**Gerekli ürün ve bileşenlerini yüklemek için**

1. İndirme ve yükleme [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118).
2. Web Platformu yükleyicisi, yükleme tamamlandıktan sonra otomatik olarak başlatılır.

    > [!NOTE]
    > Şimdi Web Platformu yükleyicisi diledikleri zaman başlatabilirsiniz **Başlat** menüsü. Bunu yapmak için **Başlat** menüsünü tıklatın **tüm programlar**ve ardından **Microsoft Web Platformu yükleyicisi**.
3. Üst kısmındaki **Web Platformu yükleyicisi 3.0** penceresinde tıklayın **ürünleri**.
4. Gezinti bölmesinde, pencerenin sol tarafındaki tıklayarak **çerçeveleri**.
5. İçinde **Microsoft .NET Framework 4** .NET Framework zaten yüklü değilse, satırı tıklatın **Ekle**.

    > [!NOTE]
    > Windows Update aracılığıyla .NET Framework 4.0 zaten yüklü olabilir. Bir ürün veya bileşeni zaten yüklüyse, Web Platformu yükleyicisi bu değiştirerek gösterecektir **Ekle** metnini içeren düğmeye **yüklü**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. İçinde **ASP.NET MVC 3 (Visual Studio 2010)** satır, tıklayın **Ekle**.
7. Gezinti bölmesinde **sunucu**.
8. İçinde **IIS 7 önerilen Yapılandırması** satır, tıklayın **Ekle**.
9. İçinde **Web dağıtım aracı 2.1** satır, tıklayın **Ekle**.
10. İçinde **IIS: temel kimlik doğrulaması** satır, tıklayın **Ekle**.
11. İçinde **IIS: Yönetim hizmeti** satır, tıklayın **Ekle**.
12. **Yükle**'ye tıklatın. Web Platformu yükleyicisi ürünleri &#x2014; herhangi bir ilişkili bağımlılıkları &#x2014; yüklenecek birlikte listesini gösterir ve lisans koşullarını kabul isteyip istemediğinizi sorar.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Lisans koşullarını gözden geçirin ve koşulları kabul ediyorsa **kabul ediyorum**.
14. Yükleme tamamlandığında, tıklayın **son**ve ardından kapatın **Web Platformu yükleyicisi 3.0** penceresi.

IIS yüklemeden önce .NET Framework 4.0 yüklü değilse, çalıştırmanız gerekir [ASP.NET IIS Kayıt Aracı](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) IIS ile ASP.NET en son sürümünü kaydetmek için. Bunu yapmazsanız, herhangi bir sorun IIS (HTML dosyaları gibi) statik içerik sunmak bulabilirsiniz, ancak onu döndürür **HTTP Hatası 404.0 – bulunamadı** çalıştığınızda ASP.NET içeriği gidin. ASP.NET 4.0 kayıtlı olduğundan emin olmak için bir sonraki yordamı kullanın.

**ASP.NET 4. 0'ı IIS ile kaydetmeniz için**

1. Tıklayın **Başlat**, Anahtar'a tıklayın ve **komut istemi**.
2. Arama sonuçlarında sağ **komut istemi**ve ardından **yönetici olarak çalıştır**.
3. Komut İstemi penceresinde gidin **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** dizin.
4. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Herhangi bir noktada 64-bit web uygulamalarını barındırmak için planlıyorsanız, ASP.NET 64-bit sürümü de IIS ile kaydetmeniz. Komut İstemi penceresinde bunu yapmak için gidin **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** dizin.
6. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

İyi bir yöntem olarak, Windows Update yeniden bu noktada yeni ürünler ve yüklediğiniz bileşenler için güncelleştirmeleri karşıdan yüklenip kurulacak kullanın.

## <a name="configure-the-web-management-service"></a>Web yönetimi hizmeti yapılandırın

İhtiyacınız olan her şey yükledikten sonra sonraki adımda Web yönetimi hizmeti, IIS'de sağlamaktır. Yüksek düzeyde, bu görevleri tamamlamak gerekir:

- Sunucu düzeyinde temel kimlik doğrulamasını etkinleştirin.
- Web yönetimi hizmeti uzak bağlantıları kabul edecek şekilde yapılandırın.
- Web yönetimi hizmeti başlatın.
- Gerekli Web Yönetim hizmeti temsilci kuralları yerinde olduğundan emin olun.

**Web yönetimi hizmeti yapılandırmak için**

1. Üzerinde **Başlat** menüsünde **Yönetimsel Araçlar**ve ardından **Internet Information Services (IIS) Yöneticisi'ni**.
2. IIS Yöneticisi'nde, **bağlantıları** bölmesinde sunucu düğümünü tıklatın (örneğin, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Orta bölmede altında **IIS**, çift **kimlik doğrulaması**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. Sağ **temel kimlik doğrulaması**ve ardından **etkinleştirme**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. İçinde **bağlantıları** bölmesinde tekrar en üst düzey ayarlara dönmek için sunucu düğümünü tıklatın.
6. Orta bölmede altında **Yönetim**, çift **yönetim hizmeti**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Orta bölmede seçin **uzak bağlantıları etkinleştir**.

    > [!NOTE]
    > Web yönetimi hizmeti zaten çalışıyorsa, önce durdurmanız gerekir.
8. İçinde **eylemleri** bölmesinde tıklayın **Başlat** Web yönetimi hizmeti başlatılamıyor.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Ayarlarınızı kaydetmek için istenirse, tıklayın **Evet**.

    > [!NOTE]
    > Hizmetini otomatik olarak başlayacak şekilde yapılandırmak isteyebilirsiniz. Bunu yapmak için Hizmetler konsolunu açın, sağ **Web yönetimi hizmeti**ve ardından **özellikleri**. İçinde **başlangıç türü** açılan listesinden **otomatik**ve ardından **Tamam**.
10. İçinde **bağlantıları** bölmesinde tekrar en üst düzey ayarlara dönmek için sunucu düğümünü tıklatın.
11. Orta bölmede altında **Yönetim**, çift **yönetim hizmeti temsilci**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Orta bölmede bir kural kümesi içerdiğini doğrulayın.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Bu kurallar, çeşitli Web dağıtımı sağlayıcıları kullanan yetkili Web yönetimi hizmeti kullanıcıların izin verin. Örneğin, Web dağıtımı işleyicisi IIS web uygulamaları ve içerik dağıtmak için olmalıdır tüm Web yönetimi hizmeti kullanıcıların kimlik doğrulaması izin veren bir temsilci kuralı **contentPath** ve **Iisapp**  sağlayıcıları (ekran görüntüsünde görebilirsiniz son kuralı).

    Bu konuda açıklanan siparişteki ürünleri ve bileşenleri yüklü değilse, Web Dağıtımı'nın en son sürümünü Web yönetimi hizmeti için tüm gerekli temsilci kuralları otomatik olarak eklemelisiniz. Yönetim hizmeti temsilci sayfasına, tüm kuralları göstermez kendiniz oluşturmanız gerekir. Bunun nasıl yapılacağı hakkında yönergeler için bkz [Web dağıtımı işleyicisi yapılandırma](https://go.microsoft.com/?linkid=9805124).
13. İçinde **bağlantıları** bölmesinde tekrar en üst düzey ayarlara dönmek için sunucu düğümünü tıklatın.

## <a name="create-and-configure-an-iis-website"></a>Oluşturma ve bir IIS Web sitesi yapılandırma

Sunucunuza web içeriği dağıtmadan önce oluşturulup bir IIS Web sitesi içeriğini barındırmak için yapılandırılması gerekir. Web dağıtımı, web paketleri yalnızca var olan bir IIS Web sitesine dağıtabilirsiniz; Bu Web sitesi sizin için oluşturulamıyor. Ayrıca yönetici olmayan hesabınızın içeriğini uzaktan dağıtmak izin vermek için küçük bir ek yapılandırma yapmanız gerekir. Yüksek düzeyde, bu görevleri tamamlamak gerekir:

- İçeriğinizi barındırmak için dosya sisteminde bir klasör oluşturun.
- İçerik sunmak için bir IIS Web sitesi oluşturma ve yerel klasör ile ilişkilendirebilirsiniz.
- Uygulama havuzu kimliği yerel klasör izinlerini okuma izni ver.
- Gerekli IIS web uygulamanızı dağıtacağınız etki alanı hesabı için izinler verir.

Bu yaklaşım olmasa da nothing IIS'de varsayılan Web sitesine içerik dağıtımını durdurmak, test ya da gösterim senaryoları dışında her şey için önerilmez. Bir üretim ortamının benzetimini yapmak için uygulamanızın gereksinimleri için belirli ayarlarla yeni bir IIS Web sitesi oluşturmanız gerekir.

**Bir IIS Web sitesi oluşturmak için**

1. Yerel dosya sisteminde içeriğinizi depolamak için bir klasör oluşturun (örneğin, **C:\DemoSite**).
2. Üzerinde **Başlat** menüsünde **Yönetimsel Araçlar**ve ardından **Internet Information Services (IIS) Yöneticisi'ni**.
3. IIS Yöneticisi'nde, **bağlantıları** bölmesinde sunucu düğümünü genişletin (örneğin, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Sağ **siteleri** düğümünü ve ardından **Web sitesi Ekle**.
5. İçinde **Site adı** IIS Web sitesi için bir ad yazın (örneğin, **DemoSite**).
6. İçinde **fiziksel yolu** kutusuna yazın (veya göz atın), yerel bir klasör yolu (örneğin, **C:\DemoSite**).
7. İçinde **bağlantı noktası** istediğiniz Web sitesini barındırmak bağlantı noktası numarasını yazın (örneğin, **85**).

    > [!NOTE]
    > Standart bağlantı noktası numaralarını, HTTP için 80 ve HTTPS için 443 ' dir. Ancak, bu Web sitesi bağlantı noktası 80 üzerinde barındırıyorsanız, sitenizi erişebilmeniz için önce varsayılan Web sitesini Durdur gerekir.
8. Bırakın **ana bilgisayar adı** kutusunu boş, Web sitesi için bir etki alanı adı sistemi (DNS) kaydı yapılandırın ve ardından istediğiniz sürece **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > Bir üretim ortamında, büyük olasılıkla, Web sitesi bağlantı noktası 80 üzerinde barındırmak ve DNS kayıtlarını eşleşen birlikte bir konak üstbilgisi yapılandırma isteyeceksiniz. IIS 7'de konak üstbilgileri yapılandırma hakkında daha fazla bilgi için bkz. [bir Web sitesi (IIS 7) için bir konak üstbilgisi yapılandırma](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Windows Server 2008 R2'de DNS sunucusu rolü hakkında daha fazla bilgi için bkz. [DNS sunucusuna genel bakış](https://technet.microsoft.com/en-gb/library/cc770392.aspx) ve [DNS sunucusu](https://technet.microsoft.com/windowsserver/dd448607).
9. **Eylemler** bölmesinde, **Site Düzenle**altında, **Bağlamalar**'ı tıklatın.
10. İçinde **Site bağlamaları** iletişim kutusu, tıklayın **Ekle**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. İçinde **Site bağlaması Ekle** iletişim kutusu, kümesi **IP adresi** ve **bağlantı noktası** var olan site yapılandırmanızla eşleşecek şekilde.
12. İçinde **ana bilgisayar adı** web sunucunuzun adını yazın (örneğin, **STAGEWEB1**) ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > İlk site bağlaması IP adresi ve bağlantı noktasını kullanarak yerel olarak site erişmenize olanak sağlayan veya `http://localhost:85`. Makine adını kullanarak etki alanındaki diğer bilgisayarlardan siteye erişmek ikinci bir site bağlaması sağlar (örneğin, http://stageweb1:85).
13. İçinde **Site bağlamaları** iletişim kutusu, tıklayın **Kapat**.
14. İçinde **bağlantıları** bölmesinde tıklayın **uygulama havuzları**.
15. İçinde **uygulama havuzları** bölmesinde, uygulama havuzunuzu adına sağ tıklayın ve ardından **temel ayarları**. Varsayılan olarak, Web sitenizin adı, uygulama havuzunun adı eşleşir (örneğin, **DemoSite**).
16. İçinde **.NET Framework sürümünü** listesinden **.NET Framework v4.0.30319**ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > Örnek çözüm, .NET Framework 4.0 gerektirir. Bu bir gereksinim Web dağıtımı için genel olarak geçerli değildir.

Sırayla içerik sunmak bir Web siteniz için uygulama havuzu kimliği içeriği depolayan bir yerel klasör izinlerini okuma olmalıdır. IIS 7.5 uygulama havuzları (aksine, IIS, burada ağ hizmeti hesabını kullanarak genellikle uygulama havuzlarında çalışır önceki sürümleri için) varsayılan olarak benzersiz bir uygulama havuzu kimliği ile çalıştırın. Uygulama havuzu kimliği gerçek kullanıcı hesabı değil ve tüm kullanıcılar veya gruplar &#x2014 listelerde görünmüyor; uygulama havuzunu başlatıldığında, bunun yerine, bunu dinamik olarak oluşturulur. Her uygulama havuzu kimliği için yerel eklenir **IIS\_IUSRS** gizli öğe olarak güvenlik grubu.

Bir uygulama havuzu kimliği bir dosya veya klasör izinleri vermek için iki seçeneğiniz vardır:

- İzinleri aşağıdaki biçimi kullanarak uygulama havuzu kimliği için doğrudan atayın. <strong>IIS uygulama havuzu\</ strong ><em>[uygulama havuzu adı]</em>(örneğin, <strong>IIS AppPool\DemoSite</strong>).
- Atama izinleri **IIS\_IUSRS** grubu.

Yerel izinler atamak için en yaygın yaklaşımdır **IIS\_IUSRS** grubunda olduğundan bu yaklaşım uygulama havuzları, dosya sistemi izinleri yapılandırmadan değiştirmenize olanak tanır. Sonraki yordam, bu grup tabanlı bir yaklaşım kullanır.

> [!NOTE]
> IIS 7.5, uygulama havuzu kimlikleri hakkında daha fazla bilgi için bkz. [uygulama havuzu kimlikleri](https://go.microsoft.com/?linkid=9805123).


**Bir IIS Web sitesi için klasör izinlerini yapılandırmak için**

1. Windows Gezgini'nde yerel klasörünüz konumuna göz atın.
2. Klasörü sağ tıklatın ve ardından **özellikleri**.
3. Üzerinde **güvenlik** sekmesinde **Düzenle**ve ardından **Ekle**.
4. Tıklayın **konumları**. İçinde **konumları** iletişim kutusunda, yerel sunucuyu seçin ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. İçinde **kullanıcıları veya Grupları Seç** iletişim kutusuna **IIS\_IUSRS**, tıklayın **Adları Denetle**ve ardından **Tamam**.
6. İçinde <strong>izinlerini</strong><em>[klasör adı]</em> iletişim kutusunda, yeni gruba atanmış olan bildirim <strong>okuma &amp; yürütme</strong>, <strong>klasörü Listele içeriği</strong>, ve <strong>okuma</strong> varsayılan izinleri. Bu değiştirmeden bırakın ve tıklayın <strong>Tamam</strong>.
7. Tıklayın <strong>Tamam</strong> kapatmak için <em>[klasör adı]</em><strong>özellikleri</strong> iletişim kutusu.

Son bir görev olarak için yönetici olmayan kullanıcı kimlik bilgileri, içerik dağıtmak için kullanacağınız uygun izinleri vermeniz gerekir. Bu kullanıcı içerik uzaktan Web sitenize dağıtmak için izinler gerektirir.

**Yönetici olmayan etki alanı kullanıcısı için IIS Web sitesi izinlerini yapılandırmak için**

1. IIS Yöneticisi'nde içinde **bağlantıları** bölmesinde, Web sitesi düğümüne sağ tıklayın (örneğin, **DemoSite**), işaret **Dağıt**ve ardından **Web yapılandırma Yayımlama dağıtma**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. İçinde **yapılandırma Web dağıtımı yayımlama** sağındaki iletişim kutusu **yayımlama izin vermek için bir kullanıcı seçin** listesinde, üç nokta düğmesine tıklayın.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. İçinde **kullanıcıya izin ver** iletişim kutusunda, içerik dağıtma ve ardından kullanmak istediğiniz hesabın etki alanı ve kullanıcı adını yazın **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. İçinde **yapılandırma Web dağıtımı yayımlama** iletişim kutusu, tıklayın **Kurulum**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Bu işlem, tek bir adımda iki önemli işlevler gerçekleştirir. İlk olarak, önceki bölümde incelenirken temsilci kurallara göre Web sitesi Web yönetimi hizmeti üzerinden uzaktan değiştirme izni verir. İkinci olarak, kaynak klasörünün eklemek, değiştirmek ve Web sitesi içeriğini izinleri izin verir Web sitesi için kullanıcıya tam denetim verir.
5. İçinde **yapılandırma Web dağıtımı yayımlama** iletişim kutusu, tıklayın **Kapat**.

## <a name="configure-firewall-exceptions"></a>Güvenlik Duvarı özel durumlarını yapılandırma

Varsayılan olarak, IIS Web Yönetim Hizmeti'nin 8172 numaralı TCP bağlantı noktasını dinler. Web sunucunuz üzerinde Windows Güvenlik Duvarı etkinse, (tüm giden trafiğe varsayılan olarak Windows Güvenlik Duvarı'nda izin verilir) 8172 numaralı bağlantı noktasındaki TCP trafiğine izin veren yeni bir gelen kuralı oluşturmak gerekir. Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, trafiğine izin veren kurallar oluşturmak gerekir.

| Yön | Bağlantı noktasından | Bağlantı noktası | Bağlantı noktası türü |
| --- | --- | --- | --- |
| Gelen | Tüm | 8172 | TCP |
| Giden | 8172 | Tüm | TCP |
  

Windows Güvenlik duvarı kuralları yapılandırma hakkında daha fazla bilgi için bkz. [güvenlik duvarı kurallarını yapılandırma](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Üçüncü taraf güvenlik duvarları için lütfen ürün belgelerine bakın.

## <a name="conclusion"></a>Sonuç

Web sunucunuz artık Web Yönetim hizmeti aracılığıyla Web dağıtımı işleyicisi uzak dağıtımlara kabul etmeye hazır olmalıdır. Sunucuya bir web uygulaması dağıtma girişiminde bulunmadan önce aşağıdaki önemli noktalara denetlemek isteyebilirsiniz:

- Temel kimlik doğrulaması IIS'de sunucu düzeyinde etkinleştirdiniz mi?
- Web yönetimi hizmeti için Uzak bağlantıları etkinleştirdiniz mi?
- Web yönetimi hizmeti başladınız mı?
- Yönetim hizmeti temsilci kuralları yerinde var mı?
- Uygulama havuzu kimliği, Web siteniz için kaynak klasöre okuma erişimi yok?
- Yönetici olmayan kullanıcı hesabı IIS'de site düzeyinde izinlere sahip?
- Güvenlik duvarınızı, TCP bağlantı noktası 8172 sunucusunda gelen bağlantılara izin veriyor mu?

## <a name="further-reading"></a>Daha Fazla Bilgi

Web dağıtımı işleyicisi için web paketleri dağıtmak için özel Microsoft Build Engine (MSBuild) proje dosyalarının nasıl yapılandırılacağı hakkında yönergeler için bkz. [bir hedef ortam için dağıtım özellikleri yapılandırma](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Önceki](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [İleri](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
