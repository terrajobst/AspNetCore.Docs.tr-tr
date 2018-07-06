---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Web için bir veritabanı sunucusunu yapılandırma dağıtımı yayımlama | Microsoft Docs
author: jrjlee
description: Bu konuda, web dağıtımı ve yayımlama desteklemek için SQL Server 2008 R2 veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır. Bu konuda açıklanan görevler ortak olan...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 9ec938298ea63609c276aed807a67722bf9bd66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841985"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Bir veritabanı sunucusunu Web dağıtımı yayımlama için yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, web dağıtımı ve yayımlama desteklemek için SQL Server 2008 R2 veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.
> 
> Bu konuda açıklanan görevlerin her dağıtım senaryosu için ortak olan&#x2014;IIS Web Dağıtım Aracı (Web dağıtımı) Uzak Aracı hizmeti, Web dağıtımı işleyicisi ya da çevrimdışı dağıtımı kullanmak için uygulamanızı web sunucularınızdan yapılandırılıp yapılandırılmadığını önemli değildir veya Uygulama, tek bir web sunucusu veya sunucu grubunda çalışıyor. Veritabanı dağıtma şeklinizi güvenlik gereksinimleri ve diğer konular göre değişebilir. Örneğin, veritabanı veya örnek verileri kaydetmeden dağıtabileceğinizi ve kullanıcı rolü eşlemeleri dağıtma veya dağıtımdan sonra bunları el ile yapılandırmanız. Ancak, veritabanı sunucusunu yapılandırma şekliniz aynı kalır.


Web dağıtımını destekleyen bir veritabanı sunucusunu yapılandırma için ek ürün veya araçları yüklemeniz gerekmez. Veritabanı sunucunuz ve web sunucunuz farklı makinelerde çalıştırın varsayılarak, yalnızca yapmanız gerekir:

- SQL Server'ı kullanarak TCP/IP iletişim kurmasına izin verir.
- Herhangi bir güvenlik duvarı üzerinden SQL Server trafiğine izin verin.
- Web sunucusu makine hesabının SQL Server oturum açma sağlar.
- Makine hesabı oturum açma, tüm gerekli veritabanı rolleriyle eşleyebileceğinizi.
- Dağıtım bir SQL Server oturum açma ve veritabanı oluşturan izinleri çalıştıracak hesabı verin.
- Yineleme dağıtımlarını desteklemek üzere dağıtım hesabı oturum açma harita **db\_sahibi** veritabanı rolü.

Bu konuda, bu yordamların her biri gerçekleştirme gösterilmektedir. Görevler ve bu konudaki yönergeler Windows Server 2008 R2 üzerinde çalışan SQL Server 2008 R2 varsayılan bir örnek ile başlatıyorsanız varsayılır. Devam etmeden önce şunlardan emin olun:

- Windows Server 2008 R2 Service Pack 1 ve tüm kullanılabilir güncelleştirmeler yüklenir.
- Etki alanına katılmış sunucusudur.
- Sunucuda bir statik IP adresi var.
- SQL Server 2008 R2 Service Pack 1 ve tüm kullanılabilir güncelleştirmeler yüklenir.

SQL Server örneği yalnızca eklemis **veritabanı altyapısı Hizmetleri** rol, tüm SQL Server yüklemesinde otomatik olarak eklenir. Ancak, için yapılandırma ve Bakım kolaylığı, eklediğinizden öneririz **Yönetim Araçları – temel** ve **Yönetim Araçları – tam** sunucu rolleri.

> [!NOTE]
> Bilgisayarlarının bir etki alanına katılmasını sağlama hakkında daha fazla bilgi için bkz: [katılan bilgisayarların etki alanı ve günlüğe kaydetme üzerinde](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Statik IP adreslerini yapılandırma hakkında daha fazla bilgi için bkz. [statik bir IP adresi yapılandırın](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). SQL Server'ı yükleme hakkında daha fazla bilgi için bkz. [SQL Server 2008 R2'yi yükleme](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>SQL Server'a uzaktan erişimi etkinleştirin

SQL Server uzak bilgisayarlarla iletişim kurmak için TCP/IP'yi kullanır. Farklı makinelerde veritabanı sunucunuz ve web sunucunuz varsa, için gerekir:

- TCP/IP üzerinden iletişime izin vermek üzere SQL sunucu ağ ayarlarını yapılandırın.
- TCP trafiğine izin ver (ve bazı durumlarda kullanıcı veri birimi Protokolü (UDP) trafiği için) SQL Server örneği tarafından kullanılan bağlantı noktaları üzerinde bir donanım veya yazılım güvenlik duvarlarını yapılandırın.

TCP/IP üzerinden iletişim kurmak SQL Server'ı etkinleştirmek için SQL Server örneği için ağ yapılandırmasını değiştirmek için SQL Server Yapılandırma Yöneticisi'ni kullanın.

**TCP/IP'yi kullanarak iletişim kurmak SQL Server'ı etkinleştirmek için**

1. Üzerinde **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft SQL Server 2008 R2**, tıklayın **yapılandırma araçları**ve ardından **SQL Server Yapılandırma Yöneticisi**.
2. Ağaç görünümü bölmesinde **SQL Server Ağ Yapılandırması**ve ardından **MSSQLSERVER protokolleri**.

   > [!NOTE]
   > Birden çok SQL Server örneğini yüklediyseniz, gördüğünüz bir <strong>için protokoller</strong><em>[örnek adı]</em> her örneği için öğesi. Bir örnek tarafından örneği bazında ağ ayarlarını yapılandırmak gerekir.
3. Ayrıntılar bölmesinde, **TCP/IP'yi** satır ve ardından **etkinleştirme**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. İçinde **uyarı** iletişim kutusu, tıklayın **Tamam**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. MSSQLSERVER hizmetinin yeni ağ yapılandırmanızı geçerlilik kazanmasından önce yeniden başlatmanız gerekir. Bunu bir komut isteminde, Hizmetler konsolunu veya SQL Server Management Studio yapabilirsiniz. Bu yordamda, SQL Server Management Studio kullanacaksınız.
6. SQL Server Yapılandırma Yöneticisi'ni kapatın.
7. Üzerinde **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft SQL Server 2008 R2**ve ardından **SQL Server Management Studio**.
8. İçinde **sunucuya Bağlan** iletişim kutusundaki **sunucu adı** kutusunda, veritabanı sunucusunun adını yazın ve ardından **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. İçinde **Nesne Gezgini** bölmesinde ana sunucu düğümüne sağ tıklayın (örneğin, **TESTDB1**) ve ardından **yeniden**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. İçinde **Microsoft SQL Server Management Studio** iletişim kutusu, tıklayın **Evet**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Hizmet yeniden başlatıldığında SQL Server Management Studio'yu kapatın.

SQL Server trafiğinin bir güvenlik duvarı üzerinden izin vermek için önce SQL Server örneği kullanarak hangi bağlantı noktalarının bilmeniz gerekir. Bu, nasıl SQL Server örneği oluşturulan yapılandırılmış ve şirket bağlıdır:

- A *varsayılan örnek* SQL Server'ın dinlediği için (ve yanıtlar) 1433 numaralı TCP bağlantı noktasını istekleri.
- A *adlandırılmış örnek* SQL Server'ın dinlediği için (ve yanıtlar) dinamik olarak atanan bir TCP bağlantı noktası isteklerini.
- SQL Server Browser hizmetini etkinleştirilirse, istemciler UDP bağlantı noktası 1434'ü belirli bir SQL Server örneği için kullanılacak TCP bağlantı noktasını bulmak için hizmette sorgulayabilir. Ancak, bu hizmet güvenlik nedenleriyle genellikle devre dışıdır.

SQL Server'ın varsayılan örneğinin kullanmakta olduğunuz varsayılarak, trafiğe izin vermek için güvenlik duvarını yapılandırmanız gerekir.

| Yön | Bağlantı noktasından | Bağlantı noktası | Bağlantı noktası türü |
| --- | --- | --- | --- |
| Gelen | Tüm | 1433 | TCP |
| Giden | 1433 | Tüm | TCP |
  

> [!NOTE]
> Teknik olarak, bir istemci bilgisayar SQL Server ile iletişim kurmak için 1024 ile 5000 arasında rastgele atanan bir TCP bağlantı noktası kullanır ve güvenlik duvarı kurallarınıza uygun şekilde kısıtlayabilirsiniz. SQL Server bağlantı noktaları ve güvenlik duvarları hakkında daha fazla bilgi için bkz. [SQL için bir güvenlik duvarı iletişim kurmak için gereken TCP/IP bağlantı noktası numaralarını](https://go.microsoft.com/?linkid=9805125) ve [nasıl yapılır: bir belirli TCP bağlantı noktasını (SQL Server yapılandırması dinlemek üzere yapılandırılması Yöneticisi)](https://msdn.microsoft.com/library/ms177440.aspx).


Çoğu Windows Server ortamlarda, büyük olasılıkla veritabanı sunucusunda Windows Güvenlik Duvarı'nı yapılandırmanız gerekecektir. Bir kural özellikle engellemediği sürece varsayılan olarak, Windows Güvenlik Duvarı tüm giden trafiği sağlar. Web sunucunuzun, veritabanınızın ulaşmak etkinleştirmek için SQL Server örneğinin kullandığı bağlantı noktası numarası TCP trafiğine izin veren bir gelen kuralı yapılandırmanız gerekir. SQL Server varsayılan örneğini kullanıyorsanız, bu kuralı yapılandırmak için sonraki yordamı kullanabilirsiniz.

**Windows Güvenlik Duvarı'nı varsayılan bir SQL Server örneği ile iletişime izin verecek şekilde yapılandırmak için**

1. Veritabanı sunucusunda bulunan **Başlat** menüsünde **Yönetimsel Araçlar**ve ardından **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
2. Ağaç görünümü bölmesinde **gelen kuralları**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. İçinde **eylemleri** bölmesi altında **gelen kuralları**, tıklayın **yeni kural**.
4. Yeni gelen kuralı sihirbazında, üzerinde **kural türü** sayfasında **bağlantı noktası**ve ardından **sonraki**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Üzerinde **protokol ve bağlantı noktaları** sayfasında **TCP** seçildiğinde ve **belirli yerel bağlantı noktaları** kutusuna **1433**ve ardından **Sonraki**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Üzerinde **eylem** sayfasında **bağlantıya izin** tıklatın ve seçili **sonraki**.
7. Üzerinde **profili** sayfasında **etki alanı** seçili Temizle **özel** ve **genel** onay kutularını işaretleyin ve ardından  **Sonraki**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Üzerinde **adı** sayfasında, kural uygun şekilde açıklayıcı bir ad verin (örneğin, **SQL Server varsayılan örneğini – ağ erişimi**) ve ardından **son**.

Özellikle ihtiyacınız varsa SQL Server ile standart veya dinamik bağlantı noktaları üzerinden iletişim kurmak için bkz: SQL Server için Windows Güvenlik Duvarı yapılandırma hakkında daha fazla bilgi için [nasıl yapılır: veritabanı altyapısı erişimi için Windows Güvenlik duvarını yapılandırma](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Oturumu yapılandırmak ve veritabanı izinleri

Internet Information Services (IIS) web uygulaması dağıttığınızda, uygulama havuzu kimliğini kullanarak uygulama çalışır. Bir etki alanı ortamında, uygulama havuzu kimlikleri ağ kaynaklarına erişmek için çalıştıkları sunucusunun makine hesabını kullanın. Makine hesapları şu yapıda <em>[etki alanı adı]</em><strong>\</ strong ><em>[makine adı]</em><strong>$</strong>&#x2014;Örneğin, <strong>FABRIKAM\TESTWEB1$</strong>. Web uygulamanızı bir veritabanına ağ üzerinden erişmek izin vermek için gerekir:

- Bir oturum açma web sunucusu makine hesabının SQL Server örneğine ekleyin.
- Makine hesabı oturum açma tüm gerekli veritabanı rolleriyle eşleyebileceğinizi (genellikle **db\_datareader** ve **db\_datawriter**).

Web uygulamanızın tek bir sunucu yerine bir sunucu grubu üzerinde çalışıyorsa, sunucu grubundaki her bir web sunucusu için bu yordamları yineleyin gerekecektir.

> [!NOTE]
> Uygulama havuzu kimlikleri ve ağ kaynaklarına erişimini hakkında daha fazla bilgi için bkz. [uygulama havuzu kimlikleri](https://go.microsoft.com/?linkid=9805123).


Bu görevler çeşitli şekillerde yaklaşımını. Oturum açma için şunlardan birini yapabilirsiniz:

- Oturum açma, Transact-SQL veya SQL Server Management Studio kullanarak veritabanı sunucusunda el ile oluşturun.
- Bir SQL Server 2008 sunucu projesi oluşturma ve oturum açma dağıtmak için Visual Studio kullanın.

Dağıtmak istediğiniz veritabanına bağımlı olmayan bir SQL Server oturum açma veritabanı düzeyinde bir nesne yerine, sunucu düzeyinde bir nesne olduğundan. Bu nedenle, herhangi bir noktada oturumu oluşturabilirsiniz ve en kolay yaklaşım genellikle, veritabanları dağıtmaya başlamadan önce oturum açma el ile veritabanı sunucusunda oluşturmaktır. Sonraki yordamda SQL Server Management Studio'da bir oturumu oluşturmak için kullanabilirsiniz.

**Bir web sunucusu makine hesabının SQL Server oturumu oluşturmak için**

1. Veritabanı sunucusunda üzerinde **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft SQL Server 2008 R2**ve ardından **SQL Server Management Studio** .
2. İçinde **sunucuya Bağlan** iletişim kutusundaki **sunucu adı** kutusunda, veritabanı sunucusunun adını yazın ve ardından **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. İçinde **Nesne Gezgini** bölmesinde sağ **güvenlik**, işaret **yeni**ve ardından **oturum açma**.
4. İçinde **oturum açma – yeni** iletişim kutusundaki **oturum açma adı** , web sunucusu makine hesabının adını yazın (örneğin, **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. **Tamam**'ı tıklatın.

Bu noktada, veritabanı sunucunuza Web dağıtımı yayımlama için hazırdır. Ancak, makine hesabı oturum açma gerekli veritabanı rolleriyle eşleyebileceğinizi kadar dağıttığınız herhangi bir çözüm işe yaramaz. Düşündüğünüz daha çok veritabanı rollerine oturum açma eşlemesi gerektirir, siz kadar harita rolleri, sonra veritabanını dağıttınız olamaz. Makine hesabı oturum açma gerekli veritabanı rolleriyle eşleyebileceğinizi için şunlardan birini yapabilirsiniz:

- İlk kez veritabanını dağıttıktan sonra veritabanı rolleri için oturum açma el ile atayın.
- Oturum açma veritabanı rolleri atamak için bir dağıtım sonrası betiği kullanın.

Oturum açma bilgileri ve veritabanı rolü eşlemeleri oluşturulmasını otomatik hale getirme konusunda daha fazla bilgi için bkz. [Test ortamlarına veritabanı rol üyelikleri dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Alternatif olarak, makine hesap oturum açma bilgilerini el ile gerekli veritabanı rolleriyle eşleyebileceğinizi sonraki yordamı kullanın. Unutmayın, bu yordamı kadar gerçekleştiremezsiniz *sonra* veritabanı dağıttınız.

**Veritabanı rolleri için web sunucusu makine hesabı oturum açma eşlemek için**

1. Önceki gibi SQL Server Management Studio'yu açın.
2. İçinde **Nesne Gezgini** bölmesinde, genişletin **güvenlik** düğümünü genişletin **oturumları** düğümünü ve ardından makine hesabı oturum açma çift tıklayın (örneğin,  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. İçinde **oturum açma özellikleri** iletişim kutusu, tıklayın **kullanıcı eşleme**.
4. İçinde **bu oturum açmaya eşlenen kullanıcılar** tablo, veritabanınızın adını seçin (örneğin, **ContactManager**).
5. İçinde **için veritabanı rolü üyeliği:** *[veritabanı adı]* listesinde, gerekli izinleri seçin. Kişi Yöneticisi örnek çözüm söz konusu olduğunda, seçmelisiniz **db\_datareader** ve **db\_datawriter** rolleri.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. **Tamam**'ı tıklatın.

Veritabanı rolleri el ile eşleme genellikle birden fazla test ortamları için yeterli olmakla birlikte, hazırlama veya üretim ortamları için otomatik olarak veya tek tıklamayla dağıtımlar için daha az istenen bir durumdur. Bu tür bir dağıtım sonrası betiklerini kullanarak görevi otomatik hale getirme konusunda daha fazla bilgi bulabilirsiniz [Test ortamlarına veritabanı rol üyelikleri dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Sunucu projeleri ve veritabanı projeleri hakkında daha fazla bilgi için bkz. [Visual Studio 2010 SQL Server veritabanı projeleri](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Dağıtım hesabının izinlerini yapılandırma

Dağıtım çalıştırmak için kullanacağınız hesabı SQL Server Yöneticisi değilse, bu hesap için bir oturum oluşturmanız gerekir. Veritabanını oluşturmak için hesabın üyesi olması gerekir **dbcreator** sunucu rolü veya eşdeğer izinlere sahip.

> [!NOTE]
> Bir veritabanını dağıtmak için Web dağıtımı veya VSDBCMD kullandığınızda (SQL Server Örneğinize karma mod kimlik doğrulamasını desteklemek üzere yapılandırılmışsa), Windows kimlik bilgileri veya SQL Server kimlik bilgilerini kullanabilirsiniz. Sonraki yordam, Windows kimlik bilgilerini kullanmak istediğiniz, ancak şey dağıtım yapılandırdığınızda, bir SQL Server kullanıcı adı ve parola bağlantı dizenizi belirtmelerini durdurma varsayar.


**Dağıtım hesabının izinlerini ayarlamak için**

1. Önceki gibi SQL Server Management Studio'yu açın.
2. İçinde **Nesne Gezgini** bölmesinde sağ **güvenlik**, işaret **yeni**ve ardından **oturum açma**.
3. İçinde **oturum açma – yeni** iletişim kutusundaki **oturum açma adı** dağıtım hesabınızın adını yazın (örneğin, **FABRIKAM\matt**).
4. İçinde **sayfa Seç** bölmesinde tıklayın **sunucu rolleri**.
5. Seçin **dbcreator**ve ardından **Tamam**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Sonraki dağıtımlarını desteklemek üzere, ayrıca dağıtma eklemeniz gerekecektir **db\_sahibi** ilk dağıtımdan sonra veritabanı rolü. Sonraki dağıtımlarda var olan bir veritabanının şemasını değiştirme yerine, yeni bir veritabanı oluşturma olmasıdır. Veritabanı bariz nedenlerle oluşturuncaya kadar önceki bölümde açıklandığı gibi bir veritabanı rolüne bir kullanıcı ekleyemezsiniz.

**Veritabanına dağıtım hesap oturum açma bilgilerini eşlemek için\_sahip veritabanı rolü**

1. Önceki gibi SQL Server Management Studio'yu açın.
2. İçinde **Nesne Gezgini** penceresinde genişletin **güvenlik** düğümünü genişletin **oturumları** düğümünü ve ardından makine hesabı oturum açma çift tıklayın (örneğin,  **FABRIKAM\matt**).
3. İçinde **oturum açma özellikleri** iletişim kutusu, tıklayın **kullanıcı eşleme**.
4. İçinde **bu oturum açmaya eşlenen kullanıcılar** tablo, veritabanınızın adını seçin (örneğin, **ContactManager**).
5. İçinde **için veritabanı rolü üyeliği:** *[veritabanı adı]* listesinden **db\_sahibi** rol.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. **Tamam**'ı tıklatın.

## <a name="conclusion"></a>Sonuç

Veritabanı sunucunuz artık uzak veritabanı dağıtımlarını kabul etmek ve Uzak IIS web sunucularının veritabanlarına izin vermek için hazır olmanız gerekir. Dağıtma ve veritabanlarını kullanma girişiminde bulunmadan önce aşağıdaki önemli noktalara denetlemek isteyebilirsiniz:

- SQL Server'ın Uzak TCP/IP bağlantılarını kabul edecek şekilde yapılandırdığınız?
- SQL Server trafiğe izin vermek için herhangi bir güvenlik duvarı yapılandırdığınız?
- SQL Server erişen her web sunucusunun makine hesap oturum açma bilgilerini oluşturdunuz mu?
- Kullanıcı rolü eşlemeleri oluşturmak için bir betik, veritabanı dağıtımı içermez veya ilk kez veritabanını dağıttıktan sonra bu el ile oluşturmanız gerekir?
- Sahip dağıtım hesabı için bir oturum açma oluşturduğunuz ve kendisine eklenmiş **dbcreator** sunucu rolü?

## <a name="further-reading"></a>Daha Fazla Bilgi

Veritabanı projeleri dağıtma hakkında yönergeler için bkz. [veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md). Bir dağıtım sonrası betiği çalıştırarak veritabanı rol üyelikleri oluşturma ile ilgili yönergeler için bkz. [Test ortamlarına veritabanı rol üyelikleri dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Üyelik veritabanları konusunda sizi uyarmayı benzersiz dağıtım zorluklarını karşılayacak şekilde nasıl ile ilgili yönergeler için bkz. [Kurumsal ortamlara üyelik veritabanları dağıtma](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Önceki](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [İleri](creating-a-server-farm-with-the-web-farm-framework.md)
