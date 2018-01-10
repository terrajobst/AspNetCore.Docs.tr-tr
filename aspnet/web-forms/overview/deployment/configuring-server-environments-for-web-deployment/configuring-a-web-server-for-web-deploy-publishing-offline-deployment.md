---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: "Web için bir Web sunucusu yapılandırma dağıtımı yayımlamadan (çevrimdışı dağıtımı) | Microsoft Docs"
author: jrjlee
description: "Bu konuda çevrimdışı web yayımlama ve dağıtım desteklemek için bir IIS web sunucusunun nasıl yapılandırılacağı açıklanmaktadır. Internet Information Services ile çalışırken (t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: cd3343f58cbb9bb868d15a91152f07444c2bd68e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Web dağıtımı için yayımlama (çevrimdışı dağıtımı) bir Web sunucusu yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda çevrimdışı web yayımlama ve dağıtım desteklemek için bir IIS web sunucusunun nasıl yapılandırılacağı açıklanmaktadır.
> 
> Internet Information Services (IIS) Web dağıtımı aracı ile (Web dağıtımı) 2.0 veya üstü çalışırken, uygulamaları veya bir web sunucusuna siteleri almak için kullanabileceğiniz üç ana yaklaşım vardır. Şunları yapabilirsiniz:
> 
> - Kullanım *Web dağıtımı uzak aracının*. Bu yaklaşım web sunucusunun daha az yapılandırma gerektirir, ancak hiçbir şey sunucuya dağıtmak için bir yerel sunucu yöneticisinin kimlik bilgilerini sağlamanız gerekir.
> - Kullanım *Web dağıtımı işleyici*. Bu yaklaşım, çok daha karmaşıktır ve web sunucusu kurmak için daha fazla ilk çaba gerektirir. Ancak, bu yaklaşım kullandığınızda, dağıtım gerçekleştirmek yönetici olmayan kullanıcıların izin vermek için IIS yapılandırabilirsiniz. Web dağıtımı işleyicisi yalnızca IIS 7 veya sonraki bir sürümü kullanılabilir.
> - Kullanım *çevrimdışı dağıtım*. Bu yaklaşım web sunucusunun en az yapılandırma gerektirir, ancak sunucu yönetici el ile web paketini sunucuya kopyalayın ve IIS Yöneticisi'yle alma.
> 
> Anahtar özellikler, olumlu ve olumsuz yönlerini Bu yaklaşımlar hakkında daha fazla bilgi için bkz: [Web dağıtımı sağ yaklaşımı seçme](choosing-the-right-approach-to-web-deployment.md).


Evet, ağ altyapısı veya güvenlik kısıtlamaları uzaktan dağıtım engelliyorsa. Bu web sunucularının olduğu yalıtılmış Internet'e üretim ortamlarında &#x2014; durumda en büyük olasılıkla ya da fiziksel olarak veya güvenlik duvarları ve alt ağları &#x2014 tarafından; sunucu altyapınızı geri kalanından.

Belli ki, bu yaklaşım, web uygulamalarınızı düzenli olarak güncelleştirilirse daha az tercih haline gelir. Altyapınızı izin veriyorsa, uzak dağıtım etkinleştirmeyi düşünün dağıtmak Web işleyicisi veya Web dağıtımı uzak aracı hizmetini kullanarak isteyebilirsiniz.

## <a name="task-overview"></a>Görev genel bakış

Çevrimdışı içe ve web paketleri dağıtımını desteklemek için web sunucusunu yapılandırmak için yapmanız:

- IIS 7.5 ve IIS 7 önerilen yapılandırması yükleyin.
- Web dağıtımı 2.1 veya üstünü yükleyin.
- Dağıtılan içeriğini barındırmak için bir IIS Web sitesi oluşturun.
- Web dağıtım aracı hizmetini devre dışı bırakın.

Örnek çözümü özel olarak barındırmak için için gerekir:

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
- **.NET framework 4.0**. Bu, .NET Framework'ün bu sürümünde oluşturulan uygulamaları çalıştırmak için gereklidir.
- **Web dağıtım aracı 2.1 veya üzeri**. Bu, sunucunuzda Web dağıtımı (ve onun temel yürütülebilir MSDeploy.exe) yükler. Web dağıtımı IIS ile tümleşir ve içeri ve dışarı aktarma web paketleri olanak tanır.
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

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. İçinde **ASP.NET MVC 3 (Visual Studio 2010)** satır, tıklatın **Ekle**.
7. Gezinti bölmesinde **Server**.
8. İçinde **IIS 7 önerilen Yapılandırması** satır, tıklatın **Ekle**.
9. İçinde **Web dağıtım aracı 2.1** satır, tıklatın **Ekle**.
10. **Yükle**'ye tıklatın. Web Platformu yükleyicisi ürünleri &#x2014; herhangi bir ilişkili bağımlılıkları &#x2014; yüklenecek birlikte listesini gösterir ve lisans koşullarını kabul isteyip istemediğinizi sorar.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Lisans koşullarını gözden geçirin ve koşullarını kabul ederseniz tıklayın **kabul ediyorum**.
12. Yükleme tamamlandığında, tıklatın **son**ve ardından kapatın **Web Platformu yükleyicisi 3.0** penceresi.

IIS yüklemeden önce .NET Framework 4.0 yüklü değilse, çalıştırmanız gerekir [ASP.NET IIS Kayıt Aracı](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) IIS ile ASP.NET en son sürümünü kaydetmek için. Bunu yapmazsanız, sorunsuz IIS (HTML dosyaları gibi) statik içerik sunmanızı bulabilirsiniz, ancak onu döndürür **HTTP Hatası 404.0 – bulunamadı** ASP.NET içeriğe göz atmak çalıştığınızda. ASP.NET 4.0 kayıtlı olduğundan emin olmak için bir sonraki yordamı kullanın.

**IIS ile ASP.NET 4.0 kaydetmek için**

1. Tıklatın **Başlat**ve ardından **komut istemi**.
2. Arama sonuçlarında sağ **komut istemi**ve ardından **yönetici olarak çalıştır**.
3. Komut İstemi penceresine gidin **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** dizini.
4. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Ana bilgisayar 64-bit web uygulamaları herhangi bir noktada planlıyorsanız, ASP.NET 64-bit sürümünü de IIS ile kaydetmelisiniz. Bunu komut istemi penceresine gidin **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** dizini.
6. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

İyi bir uygulama, Windows Update'i yeniden bu noktada yeni ürünler ve yüklemiş olduğunuz bileşenler için güncelleştirmeleri karşıdan yükleyip kullanın.

## <a name="configure-the-iis-website"></a>IIS Web sitesi yapılandırma

Sunucunuza web içeriği dağıtmadan önce oluşturma ve bir IIS Web sitesi içeriğini barındırmak için yapılandırma gerekir. Web dağıtımı, yalnızca mevcut bir IIS Web sitesine web paketleri dağıtabilirsiniz; Web sitesi sizin için oluşturulamaz. Yüksek düzeyde, bu görevleri tamamlamak gerekir:

- İçeriğinizi barındırmak üzere dosya sisteminde bir klasör oluşturun.
- İçerik sunmak için bir IIS Web sitesi oluşturma ve yerel klasör ile ilişkilendirebilirsiniz.
- Uygulama havuzu kimliği yerel klasör üzerindeki izinleri okuma verme.

Olmasına karşın bir şey IIS'de varsayılan Web sitesi için içerik dağıtma durdurma test ya da gösterim senaryoları dışında her şey için bu yaklaşım önerilmez. Bir üretim ortamının benzetimini yapmak için uygulamanızın gereksinimlerine belirli ayarlarla yeni bir IIS Web sitesi oluşturmanız gerekir.

**Oluşturma ve bir IIS Web sitesi yapılandırma**

1. Yerel dosya sisteminde içeriğinizi depolamak için bir klasör oluşturun (örneğin, **C:\DemoSite**).
2. Üzerinde **Başlat** menüsündeki **Yönetimsel Araçlar**ve ardından **Internet Information Services (IIS) Yöneticisi'ni**.
3. IIS Yöneticisi ' nde, **bağlantıları** bölmesinde sunucu düğümünü genişletin (örneğin, **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Sağ **siteleri** düğümünü ve ardından **Web sitesi Ekle**.
5. İçinde **Site adı** IIS Web sitesi için bir ad yazın (örneğin, **DemoSite**).
6. İçinde **fiziksel yolu** kutusuna yazın (veya göz atın), yerel klasör yolu (örneğin, **C:\DemoSite**).
7. İçinde **bağlantı noktası** kutusunda, istediğiniz Web sitesini barındırmak bağlantı noktası numarasını yazın (örneğin, **85**).

    > [!NOTE]
    > Standart bağlantı noktası numaralarını, HTTP için 80 ve HTTPS için 443 ' dir. Ancak, bu Web sitesi bağlantı noktası 80 üzerinde barındırıyorsanız, sitenizi erişebilmeniz için önce varsayılan Web sitesini Durdur gerekir.
8. Bırakın **ana bilgisayar adı** kutusuna Web sitesi için bir etki alanı adı sistemi (DNS) kaydını yapılandırın ve ardından istemediğiniz sürece boş **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > Bir üretim ortamında, büyük olasılıkla, Web sitesi bağlantı noktası 80 üzerinde barındırmak ve DNS kayıtlarını eşleşen birlikte bir konak üstbilgisi yapılandırma istersiniz. IIS 7'de konak üstbilgileri yapılandırma hakkında daha fazla bilgi için bkz: [bir Web sitesi (IIS 7) için bir konak üstbilgisi yapılandırma](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx). Windows Server 2008 R2'de DNS sunucusu rolü hakkında daha fazla bilgi için bkz: [DNS sunucusuna genel bakış](https://technet.microsoft.com/en-gb/library/cc770392.aspx) ve [DNS sunucusu](https://technet.microsoft.com/en-us/windowsserver/dd448607).
9. **Eylemler** bölmesinde, **Site Düzenle**altında, **Bağlamalar**'ı tıklatın.
10. İçinde **Site bağlamaları** iletişim kutusu, tıklatın **Ekle**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. İçinde **Site bağlaması Ekle** iletişim kutusu, kümesi **IP adresi** ve **bağlantı noktası** var olan site yapılandırmanızı eşleşecek şekilde.
12. İçinde **ana bilgisayar adı** web sunucunuzun adını yazın (örneğin, **PROWEB1**) ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > İlk site bağlaması IP adresi ve bağlantı noktasını kullanarak yerel olarak site erişmenize olanak sağlayan veya `http://localhost:85`. İkinci site bağlaması makine adı (örneğin, http://proweb1:85) kullanarak etki alanındaki diğer bilgisayarlardan site erişmenize olanak tanır.
13. İçinde **Site bağlamaları** iletişim kutusu, tıklatın **Kapat**.
14. İçinde **bağlantıları** bölmesinde tıklatın **uygulama havuzları**.
15. İçinde **uygulama havuzları** bölmesinde, uygulama havuzunun adını sağ tıklatın ve ardından **temel ayarları**. Varsayılan olarak, Web sitenizin adı, uygulama havuzu adı ile eşleşir (örneğin, **DemoSite**).
16. İçinde **.NET Framework sürüm** listesinde **.NET Framework v4.0. 30319**ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > Örnek çözümü .NET Framework 4.0 gerektirir. Bu gereksinim Web dağıtımı için genel değil.

İçerik sunmak Web siteniz için sırada uygulama havuzu kimliği içeriği depolayan yerel klasör üzerinde Okuma izinleriniz olmalıdır. IIS 7.5 uygulama havuzları (aksine, IIS uygulama havuzları genellikle ağ hizmeti hesabını kullanarak çalıştırdığı, önceki sürümler) varsayılan olarak benzersiz uygulama havuzu kimliği ile çalıştırın. Uygulama havuzu kimliği gerçek kullanıcı hesabı değil ve tüm kullanıcılar veya gruplar &#x2014 listelerde görünmüyor; uygulama havuzunu başlatıldığında, bunun yerine, bunu dinamik olarak oluşturulur. Her uygulama havuzu kimliği yerel eklenir **IIS\_IUSRS** gizli bir öğe olarak güvenlik grubu.

Bir uygulama havuzu kimliği bir dosya veya klasör izinleri vermek için iki seçeneğiniz vardır:

- İzinleri biçimi kullanarak uygulama havuzu kimliği için doğrudan atayın. **IIS AppPool\***[uygulama havuzu adı]*(örneğin, **IIS AppPool\DemoSite**).
- İzinleri atayın **IIS\_IUSRS** grubu.

Yerel izinler atamak için en yaygın yaklaşımdır **IIS\_IUSRS** grubunda olduğundan bu yaklaşım, dosya sistemi izinleri yapılandırmadan uygulama havuzları değiştirmenize olanak sağlar. Sonraki yordamda bu grup tabanlı yaklaşımı kullanır.

> [!NOTE]
> IIS 7.5, uygulama havuzu kimlikleri hakkında daha fazla bilgi için bkz: [uygulama havuzu kimlikleri](https://go.microsoft.com/?linkid=9805123).


**Bir IIS Web sitesi için klasör izinlerini yapılandırmak için**

1. Windows Gezgini'nde yerel klasör konumuna gidin.
2. Klasöre sağ tıklayın ve ardından **özellikleri**.
3. Üzerinde **güvenlik** sekmesini tıklatın, **Düzenle**ve ardından **Ekle**.
4. Tıklatın **konumları**. İçinde **konumları** iletişim kutusunda, yerel sunucuyu seçin ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. İçinde **kullanıcıları veya Grupları Seç** iletişim kutusuna **IIS\_IUSRS**, tıklatın **Adları Denetle**ve ardından **Tamam**.
6. İçinde **izinlerini***[klasör adı]* iletişim kutusunda, yeni Grup atanan bildirim **okuma &amp; yürütme**, **liste klasörü içeriği**, ve **okuma** varsayılan izinleri. Bu değiştirmeden bırakın ve tıklatın **Tamam**.
7. Tıklatın **Tamam** kapatmak için *[klasör adı]***özellikleri** iletişim kutusu.

## <a name="disable-the-remote-agent-service"></a>Uzak Aracı hizmetini devre dışı bırakma

Web dağıtımı yüklediğinizde, Web dağıtım aracı hizmetinin yüklü ve otomatik olarak başlatılır. Bu hizmet, dağıtma ve uzak bir konumdan web paketleri yayımlama olanak sağlar. Durdurun ve hizmetin devre dışı bırakmak için uzaktan dağıtım özelliği bu senaryoda, kullanarak olmaz.

> [!NOTE]
> İçeri aktarma ve web paketini el ile dağıtmak için Uzak Aracı hizmetini durdurmak gerekmez. Ancak, durdurmak ve bunu kullanmayı planlamıyorsanız hizmetini devre dışı bırakmak için iyi bir uygulama olur.


Durdur ve çeşitli komut satırı yardımcı programları veya Windows PowerShell cmdlet'lerini kullanarak bir hizmet birden çok yolla devre dışı bırakabilirsiniz. Bu yordam, UI tabanlı basit bir yaklaşım açıklar.

**Durdurmak ve Uzak Aracı'nı devre dışı bırakmak için**

1. **Başlat** menüsünde, **Yönetim Araçları**'na gelin ve ardından **Hizmetler**'e tıklayın.
2. Hizmetler konsolunda bulun **Web Dağıtım Aracı hizmeti** satır.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Sağ **Web Dağıtım Aracı hizmeti**ve ardından **özellikleri**.
4. İçinde **Web Dağıtım Aracı hizmeti özellikleri** iletişim kutusu, tıklatın **durdurmak**.
5. İçinde **başlangıç türü** listesinde **devre dışı**ve ardından **Tamam**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Sonuç

Bu noktada, web sunucunuz çevrimdışı web paket dağıtımı için hazırdır. Bir IIS Web sitesini web paketleri almaya çalışmadan önce bu önemli noktaları denetlemek isteyebilirsiniz:

- IIS ile ASP.NET 4.0 kaydettiğiniz?
- Uygulama havuzu kimliğini Web siteniz için kaynak klasöre okuma erişimi var mı?
- Web Dağıtım Aracı hizmeti durdu mu?

>[!div class="step-by-step"]
[Önceki](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[sonraki](configuring-a-database-server-for-web-deploy-publishing.md)
