---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: "Web için bir veritabanı sunucusunu yapılandırma dağıtımı yayımlamadan | Microsoft Docs"
author: jrjlee
description: "Bu konuda, web dağıtımı ve yayımlama desteklemek için SQL Server 2008 R2 veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır. Bu konuda açıklanan ortak görevlerdir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: b225d9911246b3e2be1679b73a9f31d9f8577ba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Web dağıtımı yayımlama için veritabanı sunucusunu yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, web dağıtımı ve yayımlama desteklemek için SQL Server 2008 R2 veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.
> 
> Bu konuda açıklanan görevleri her dağıtım senaryosu & #x 2014 ortak; web sunucularınızın IIS Web Dağıtım Aracı (Web dağıtımı) uzaktan Aracı hizmeti, Web dağıtımı işleyicisi veya çevrimdışı dağıtımı kullanmak üzere yapılandırılmış olup olmadığını önemli değildir veya Uygulamanızı bir tek bir web sunucusu veya sunucu grubunda çalışıyor. Veritabanı dağıtma şeklinizi güvenlik gereksinimleri ve diğer konular göre değişebilir. Örneğin, veritabanı veya örnek verileri kaydetmeden dağıtabileceğinizi ve kullanıcı rolü eşlemeleri dağıtma veya dağıtımdan sonra bunları el ile yapılandırmanız olabilir. Ancak, veritabanı sunucusu yapılandırma biçiminizi aynı kalır.


Web dağıtımını desteklemek için bir veritabanı sunucusu yapılandırmak için ek ürün veya araçları yüklemeniz gerekmez. Veritabanı sunucunuz ve web sunucunuz farklı makinelerde çalıştırma varsayılarak, yalnızca yapmanız gerekir:

- SQL Server'ın TCP/IP'yi kullanarak iletişim kurmasına izin verir.
- Herhangi bir güvenlik duvarı üzerinden SQL Server trafiğine izin verin.
- Web sunucusu makine hesabının SQL Server oturum açma verin.
- Makine hesabı oturum açma için herhangi bir gereken veritabanı rol eşleyin.
- Dağıtım bir SQL Server oturum açma ve veritabanı oluşturan izinlerini çalışacak hesabı verin.
- Yineleme dağıtımlarını desteklemek için dağıtım hesabı oturum açma eşleme **db\_sahibi** veritabanı rolü.

Bu konuda her bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir. Görevleri ve bu konudaki yönergeler bir varsayılan örneği SQL Server 2008 R2'in Windows Server 2008 R2 üzerinde çalışan başlatıyorsanız varsayalım. Devam etmeden önce emin olun:

- Windows Server 2008 R2 Service Pack 1 ve kullanılabilir tüm güncelleştirmeleri yüklenir.
- Etki alanına katılmış sunucusudur.
- Sunucunun bir statik IP adresi vardır.
- SQL Server 2008 R2 Service Pack 1 ve kullanılabilir tüm güncelleştirmeleri yüklenir.

SQL Server örneği yalnızca içermesi gerekir **veritabanı altyapısı Hizmetleri** otomatik olarak tüm SQL Server yüklemesine dahil rol. Ancak, yapılandırma ve Bakım kolaylaştırmak için dahil öneririz **Yönetim Araçları – temel** ve **Yönetim Araçları – tam** sunucu rolleri.

> [!NOTE]
> Bilgisayarlar bir etki alanına katılma ile ilgili daha fazla bilgi için bkz: [katılan bilgisayarların etki alanı ve günlüğü üzerinde](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx). Statik IP adreslerini yapılandırma hakkında daha fazla bilgi için bkz: [bir statik IP adresi yapılandırın](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx). SQL Server yükleme hakkında daha fazla bilgi için bkz: [SQL Server 2008 R2 yükleme](https://technet.microsoft.com/en-us/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>SQL Server için uzaktan erişimi etkinleştirin

SQL Server, uzak bilgisayarlarla iletişim kurmak için TCP/IP'yi kullanır. Farklı makinelerde veritabanı sunucunuz ve web sunucunuz varsa, için gerekir:

- SQL Server Ağ ayarları TCP/IP üzerinden iletişime izin verecek şekilde yapılandırın.
- TCP trafiğine izin ver (ve bazı durumlarda kullanıcı veri birimi Protokolü (UDP) trafiği için), SQL Server örneğinin kullandığı bağlantı noktalarındaki tüm donanım veya yazılım güvenlik duvarlarını yapılandırın.

SQL Server'ın TCP/IP üzerinden iletişim kurmasını sağlamak için SQL Server örneği için ağ yapılandırmasını değiştirmek için SQL Server Configuration Manager kullanın.

**SQL Server'ın TCP/IP'yi kullanarak iletişim kurmasını sağlamak için**

1. Üzerinde **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft SQL Server 2008 R2**, tıklatın **yapılandırma araçları**ve ardından **SQL Server Configuration Manager**.
2. Ağaç görünümü bölmesinde **SQL Server Ağ Yapılandırması**ve ardından **MSSQLSERVER protokolleri**.

    > [!NOTE]
    > SQL Server'ın birden çok örneği yüklüyse göreceğiniz bir **için protokolleri***[örnek adı]* her örneği için öğesi. Bir örnek tarafından örneği bazında ağ ayarlarını yapılandırmanız gerekir.
3. Ayrıntılar bölmesinde, **TCP/IP'yi** satır ve ardından **etkinleştirmek**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. İçinde **uyarı** iletişim kutusu, tıklatın **Tamam**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Yeni ağ yapılandırmanızı etkinleşmeden önce MSSQLSERVER hizmetini yeniden başlatmanız gerekir. Bunu, bir komut isteminde, Hizmetler konsolunu veya SQL Server Management Studio yapabilirsiniz. Bu yordamda, SQL Server Management Studio'yu kullanırsınız.
6. SQL Server Yapılandırma Yöneticisi'ni kapatın.
7. Üzerinde **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft SQL Server 2008 R2**ve ardından **SQL Server Management Studio**.
8. İçinde **sunucuya Bağlan** iletişim kutusunda **sunucu adı** kutusunda, veritabanı sunucusunun adını yazın ve ardından **Bağlan**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. İçinde **Object Explorer** bölmesinde, üst sunucu düğümüne sağ tıklayın (örneğin, **TESTDB1**) ve ardından **yeniden**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. İçinde **Microsoft SQL Server Management Studio** iletişim kutusu, tıklatın **Evet**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Hizmet yeniden başlatıldığında SQL Server Management Studio'yu kapatın.

Bir güvenlik duvarı üzerinden SQL Server trafiğe izin vermek için önce SQL Server örneği kullanarak hangi bağlantı noktalarının bilmeniz gerekir. Bu nasıl SQL Server örneği oluşturulan yapılandırılmış ve değişir:

- A *varsayılan örnek* SQL Server'ın dinlediği için (ve yanıtlar) TCP bağlantı noktası 1433'ü istekler.
- A *adlandırılmış örneği* SQL Server'ın dinlediği için (ve yanıtlar) dinamik olarak atanmış bir TCP bağlantı noktası isteklerini.
- SQL Server Browser hizmetini etkinleştirilirse, istemcilerin UDP bağlantı noktası 1434 belirli bir SQL Server örneği için kullanılacak TCP bağlantı noktasını öğrenmek için hizmet sorgulayabilirsiniz. Ancak, bu hizmeti güvenlik nedenleriyle genellikle devre dışıdır.

SQL Server'ın varsayılan örneğinin kullanmakta olduğunuz varsayılarak trafiğine izin vermek için güvenlik duvarını yapılandırmanız gerekir.

| Yön | Bağlantı noktasından | Bağlantı noktası | Bağlantı noktası türü |
| --- | --- | --- | --- |
| Gelen | tüm | 1433 | TCP |
| Giden | 1433 | tüm | TCP |
  

> [!NOTE]
> Teknik olarak, bir istemci bilgisayar SQL Server ile iletişim kurmak için 1024 ile 5000 arasında rastgele atanan bir TCP bağlantı noktası kullanır ve buna göre güvenlik duvarı kuralları kısıtlayabilirsiniz. SQL Server bağlantı noktaları ve güvenlik duvarları hakkında daha fazla bilgi için bkz: [SQL için bir güvenlik duvarı iletişim kurmak için gereken TCP/IP bağlantı noktası numaralarını](https://go.microsoft.com/?linkid=9805125) ve [nasıl yapılır: belirli bir TCP bağlantı noktası üzerinde (SQL Server yapılandırma dinlemek üzere yapılandırmak Yöneticisi)](https://msdn.microsoft.com/en-us/library/ms177440.aspx).


Çoğu Windows Server ortamlarda, büyük olasılıkla veritabanı sunucusunda Windows Güvenlik duvarını yapılandırmanız gerekecektir. Bir kural özellikle engellediği sürece varsayılan olarak, Windows Güvenlik Duvarı tüm giden trafiğe izin verir. Veritabanınıza erişmek için web sunucunuzu etkinleştirmek için SQL Server örneğinin kullandığı bağlantı noktası numarasını TCP trafiğine izin veren bir gelen kuralı yapılandırmanız gerekir. SQL Server varsayılan örneğini kullanıyorsanız, bu kuralı yapılandırmak için bir sonraki yordamı kullanabilirsiniz.

**Windows Güvenlik Duvarı'nı varsayılan SQL Server örneği ile iletişime izin verecek şekilde yapılandırmak için**

1. Veritabanı sunucusunda üzerinde **Başlat** menüsündeki **Yönetimsel Araçlar**ve ardından **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
2. Ağaç görünümü bölmesinde **gelen kuralları**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. İçinde **Eylemler** bölmesi altında **gelen kuralları**, tıklatın **yeni kural**.
4. Yeni gelen kuralı sihirbazında, üzerinde **kural türü** sayfasında, **bağlantı noktası**ve ardından **sonraki**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Üzerinde **protokol ve bağlantı noktaları** sayfasında **TCP** seçili ve **belirli yerel bağlantı noktaları** kutusuna **1433**ve ardından **Sonraki**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Üzerinde **eylem** sayfasında, bırakın **bağlantıya izin** 'ı tıklatın ve seçili **sonraki**.
7. Üzerinde **profil** sayfasında, bırakın **etki alanı** seçili temizleyin **özel** ve **ortak** onay kutularını işaretleyin ve ardından  **Sonraki**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Üzerinde **adı** sayfasında, kuralın düzgün açıklayıcı bir ad girin (örneğin, **SQL Server varsayılan örneğini – ağ erişimi**) ve ardından **son**.

Özellikle gerekiyorsa SQL Server ile standart olmayan veya dinamik bağlantı noktaları üzerinden iletişim kurmak için bkz: SQL Server için Windows Güvenlik duvarını yapılandırma hakkında daha fazla bilgi için [nasıl yapılır: bir Windows Güvenlik Duvarı veritabanı altyapısı erişimi için yapılandırma](https://technet.microsoft.com/en-us/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Oturumu yapılandırmak ve veritabanı izinleri

Bir web uygulaması için Internet Information Services (IIS) dağıttığınızda, uygulamanın uygulama havuzunun kimliğini kullanarak çalışır. Uygulama havuzu kimliklerini bir etki alanı ortamında ağ kaynaklarına erişmek için çalıştırdıkları sunucusu makine hesabının kullanın. Makine hesapları alın formun *[etki alanı adı]***\***[makine adı]***$**&#x2014;Örneğin, **FABRIKAM\ TESTWEB1$**. Ağ üzerinden bir veritabanına erişmek, web uygulamanızın izin vermek için gerekir:

- Web sunucusunun makine hesabı için oturum açma SQL Server örneğine ekleyin.
- Makine hesabı oturum açma için herhangi bir gereken veritabanı rol eşleme (genellikle **db\_datareader** ve **db\_datawriter**).

Web uygulamanızın tek bir sunucu yerine bir sunucu grubu üzerinde çalışıyorsa, sunucu grubundaki her web sunucusu için bu yordamları yineleyin gerekir.

> [!NOTE]
> Uygulama havuzu kimlikleri ve ağ kaynaklarına erişimini hakkında daha fazla bilgi için bkz: [uygulama havuzu kimlikleri](https://go.microsoft.com/?linkid=9805123).


Bu görevleri çeşitli şekillerde yaklaşımını. Oturum oluşturmak için şunlardan birini yapabilirsiniz:

- Oturum açma Transact-SQL veya SQL Server Management Studio kullanarak veritabanı sunucusunda el ile oluşturun.
- Bir SQL Server 2008 sunucu projesi oluşturun ve oturum açma dağıtmak için Visual Studio'da kullanın.

Dağıtmak istediğiniz veritabanını bağımlı değil bir SQL Server oturumu veritabanı düzeyinde bir nesne yerine sunucu düzeyinde bir nesne olduğundan. Bu nedenle, herhangi bir noktada oturum açma oluşturabilirsiniz ve en kolay yaklaşım genellikle veritabanları dağıtmaya başlamadan önce oturum açma el ile veritabanı sunucusunda oluşturmaktır. SQL Server Management Studio'da bir oturumu oluşturmak için bir sonraki yordamı kullanabilirsiniz.

**Web sunucusunun makine hesabı için bir SQL Server oturumu oluşturmak için**

1. Veritabanı sunucusunda üzerinde **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft SQL Server 2008 R2**ve ardından **SQL Server Management Studio** .
2. İçinde **sunucuya Bağlan** iletişim kutusunda **sunucu adı** kutusunda, veritabanı sunucusunun adını yazın ve ardından **Bağlan**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. İçinde **Object Explorer** bölmesinde sağ **güvenlik**, işaret **yeni**ve ardından **oturum açma**.
4. İçinde **oturum açma – yeni** iletişim kutusunda **oturum açma adı** , web sunucusu makine hesabının adını yazın (örneğin, **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. **Tamam**'ı tıklatın.

Bu noktada, veritabanı sunucunuzun Web dağıtımı yayımlama için hazırdır. Ancak, dağıttığınız çözümleri makine hesabı oturum açma için gerekli veritabanı rolleri eşleme kadar çalışmaz. Veritabanı rolleri için oturum açma eşleme daha zorlayıcı çok gerektirir, siz çalıştırdıktan sonra eşleme rolleri kadar veritabanı dağıttıktan sonra olamaz. Makine hesabı oturum açma için gerekli veritabanı rolleri eşlemek için şunlardan birini yapabilirsiniz:

- İlk kez veritabanı dağıtıldıktan sonra veritabanı rolleri oturum açma el ile atayın.
- Oturum açma veritabanı rolleri atamak için bir dağıtım sonrası komut dosyası kullanın.

Oturum açma ve veritabanı rolü eşlemeleri oluşturulmasını otomatik hale getirme ile ilgili daha fazla bilgi için bkz: [Test ortamları için veritabanı rol üyeliklerini dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Alternatif olarak, makine hesabı oturum açma için gerekli veritabanı rolleri el ile eşlemek için bir sonraki yordamı kullanın. Kadar bu yordam çalıştırılamaz unutmayın *sonra* veritabanı dağıttıktan sonra.

**Web sunucusunun makine hesabı oturum açma veritabanı rolleri eşlemek için**

1. Önceki SQL Server Management Studio'yu açın.
2. İçinde **Nesne Gezgini** bölmesinde genişletin **güvenlik** düğümü genişletin **oturumları** düğümü, makine hesabı oturum açma çift tıklayın ve ardından (örneğin,  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. İçinde **oturum açma özellikleri** iletişim kutusu, tıklatın **kullanıcı eşleme**.
4. İçinde **bu oturum açmaya eşlenen kullanıcılar** tablo, veritabanınızın adını seçin (örneğin, **ContactManager**).
5. İçinde **için veritabanı rolü üyeliği:** *[veritabanı adı]* listesinde, gerekli izinleri seçin. Contact Manager örnek çözümü söz konusu olduğunda, seçmelisiniz **db\_datareader** ve **db\_datawriter** rolleri.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. **Tamam**'ı tıklatın.

Veritabanı rolleri el ile eşleme genellikle birden çok test ortamları için yeterli olmakla birlikte, hazırlık veya üretim ortamları için otomatik olarak veya tek tıklatmayla dağıtımları için daha az tercih edilir. Bu tür bir dağıtım sonrası komut dosyalarında kullanarak görev otomatikleştirme hakkında daha fazla bilgi bulabilirsiniz [Test ortamları için veritabanı rol üyeliklerini dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Sunucu projeleri ve veritabanı projeler hakkında daha fazla bilgi için bkz: [Visual Studio 2010 SQL Server veritabanı projeleri](https://msdn.microsoft.com/en-us/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Dağıtım hesap izinlerini yapılandırma

Dağıtımını çalıştırmak için kullandığınız hesabın SQL Server Yöneticisi değilse, bu hesap için bir oturum oluşturmanız gerekir. Veritabanını oluşturmak için hesap bir üyesi olmalıdır **dbcreator** sunucu rolü veya eşdeğer izinlere sahip.

> [!NOTE]
> Veritabanını dağıtmak için Web dağıtımı veya VSDBCMD kullandığınızda (SQL Server örneğinizi karma mod kimlik doğrulamayı destekleyecek şekilde yapılandırılmışsa), Windows kimlik bilgileri veya SQL Server kimlik bilgilerini kullanabilirsiniz. Sonraki yordam Windows kimlik bilgileri kullanmak istiyorsanız ancak hiçbir şey dağıtım yapılandırdığınızda, bir SQL Server kullanıcı adı ve parola bağlantı dizenizi belirleyerek durdurma varsayar.


**Dağıtım hesap izinlerini ayarlamak için**

1. Önceki SQL Server Management Studio'yu açın.
2. İçinde **Object Explorer** bölmesinde sağ **güvenlik**, işaret **yeni**ve ardından **oturum açma**.
3. İçinde **oturum açma – yeni** iletişim kutusunda **oturum açma adı** dağıtım hesabınızın adını yazın (örneğin, **FABRIKAM\matt**).
4. İçinde **sayfa Seç** bölmesinde tıklatın **sunucu rolleri**.
5. Seçin **dbcreator**ve ardından **Tamam**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Sonraki dağıtımlarını desteklemek için aynı zamanda dağıtma hesabına eklemeniz gerekir **db\_sahibi** ilk dağıtımdan sonra veritabanı rolü. Sonraki dağıtımlarında var olan bir veritabanı şemasını değiştirme yerine, yeni bir veritabanı oluşturmak olmasıdır. Veritabanı belirgin nedenlerle oluşturuncaya kadar önceki bölümde açıklandığı gibi bir veritabanı rolüne bir kullanıcı ekleyemezsiniz.

**Dağıtım hesabı oturum açma için db eşlemek için\_sahip veritabanı rolü**

1. Önceki SQL Server Management Studio'yu açın.
2. İçinde **Nesne Gezgini** penceresinde genişletin **güvenlik** düğümü genişletin **oturumları** düğümü, makine hesabı oturum açma çift tıklayın ve ardından (örneğin,  **FABRIKAM\matt**).
3. İçinde **oturum açma özellikleri** iletişim kutusu, tıklatın **kullanıcı eşleme**.
4. İçinde **bu oturum açmaya eşlenen kullanıcılar** tablo, veritabanınızın adını seçin (örneğin, **ContactManager**).
5. İçinde **için veritabanı rolü üyeliği:** *[veritabanı adı]* listesinde **db\_sahibi** rol.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. **Tamam**'ı tıklatın.

## <a name="conclusion"></a>Sonuç

Veritabanı sunucunuz artık veritabanlarınızı erişmek Uzak IIS web sunucularının izin veren ve Uzak veritabanı dağıtımları kabul etmek için hazır olması gerekir. Dağıtma ve veritabanları kullanma girişiminde bulunmadan önce aşağıdaki önemli noktaları denetlemek isteyebilirsiniz:

- SQL Server'ın Uzak TCP/IP bağlantılarını kabul edecek şekilde yapılandırdınız mı?
- SQL Server trafiğe izin vermek için herhangi bir güvenlik duvarı yapılandırdınız mı?
- SQL Server erişecek her web sunucusunun makine hesabı oturum açma oluşturduğunuz?
- Veritabanı dağıtımınızı kullanıcı rolü eşlemeleri oluşturmak için bir komut dosyası içermez veya veritabanı ilk kez dağıttıktan sonra bunları el ile oluşturmanız gerekiyor mu?
- Dağıtım hesabı için bir oturum oluşturduktan ve kendisine eklenmiş **dbcreator** sunucu rolü?

## <a name="further-reading"></a>Daha Fazla Bilgi

Veritabanı projeleri dağıtma ile ilgili yönergeler için bkz: [dağıtma veritabanı projeleri](../web-deployment-in-the-enterprise/deploying-database-projects.md). Bir dağıtım sonrası komut dosyası çalıştırarak rol üyeliklerini veritabanı oluşturma ile ilgili yönergeler için bkz: [Test ortamları için veritabanı rol üyeliklerini dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Üyelik veritabanları teşkil benzersiz bir dağıtım zorlayan konusunda yönergeler için bkz [üyelik veritabanına dağıtma kurumsal ortamlarda](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

>[!div class="step-by-step"]
[Önceki](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
[sonraki](creating-a-server-farm-with-the-web-farm-framework.md)
