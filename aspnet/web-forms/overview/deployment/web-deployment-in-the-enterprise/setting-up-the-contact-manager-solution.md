---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: "İlgili Kişi Yöneticisi çözümü ayarladıktan | Microsoft Docs"
author: jrjlee
description: "Bu konu, indirin ve bir geliştirici iş istasyonuna yerel olarak çalıştırmak için Kişi Yöneticisi çözümünüzü yapılandırma açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 85468949ee61504d6076a191b70a96e8018c67aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="setting-up-the-contact-manager-solution"></a>İlgili Kişi Yöneticisi çözüm ayarlama
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, indirin ve bir geliştirici iş istasyonuna yerel olarak çalıştırmak için Kişi Yöneticisi çözümünüzü yapılandırma açıklar.


## <a name="system-requirements"></a>Sistem Gereksinimleri

Contact Manager çözümü yerel olarak çalıştırın ve Bu öğreticide açıklanan diğer görevleri gerçekleştirmek için bu yazılım geliştirici iş istasyonunuza yüklemeniz gerekir:

- Visual Studio 2010 Service Pack 1, Premium veya Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS Web Dağıtım Aracı (Web dağıtımı) 2.1 veya üzeri
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1 

Visual Studio 2010 dışında indirin ve tüm bu ürünleri ve bileşenleri aracılığıyla en son sürümlerini yüklemek [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Karşıdan yüklemek ve çözüm ayıklamak

MSDN kod Galerisi'nden Contact Manager örnek uygulama indirebilirsiniz [burada](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Yapılandırma ve çözüm çalıştırma

Yapılandırmak ve yerel makinenizde Contact Manager çözümü çalıştırmak için üst düzey adımları gerçekleştirmeniz gerekir:

1. Zaten yoksa, üyelik ve rol yönetimi özellikleri etkin yerel bir ASP.NET uygulama Hizmetleri veritabanı oluşturun.
2. Bağlantı dizeleri düzenleme *web.config* dosyaları, yerel SQL Server Express örneğini gösterme.
3. Visual Studio 2010'dan çözümü çalıştırın.

Bu bölüm geri kalan her bu görevleri tamamlamak hakkında daha fazla kılavuzu sağlar.

**Uygulama Hizmetleri veritabanı oluşturmak için**

1. Visual Studio 2010 Komut istemi açın. Bunu yapmak için **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft Visual Studio 2010**, tıklatın **Visual Studio Araçları**ve ardından tıklatın **Visual Studio komut istemi (2010)**.
2. Komut isteminde aşağıdaki komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Kullanım **– C** anahtar veritabanı sunucunuz için bağlantı dizesini belirtin.
    2. Kullanım **– A** anahtar uygulama hizmetleri veritabanına eklemek istediğiniz özellikleri belirtin. Bu durumda, **m** üyelik sağlayıcısı için destek eklemek istediğiniz gösterir ve **r** Rol Yöneticisi için destek eklemek istediğiniz gösterir.
    3. Kullanım **– d** anahtar uygulama hizmetleri veritabanınız için bir ad belirtin. Bu anahtarı atlarsanız, yardımcı programı varsayılan adı ile bir veritabanı oluşturmak **aspnetdb**.
3. Veritabanı başarıyla oluşturuldu, komut istemini bir onay gösterir.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Aspnet hakkında daha fazla bilgi için\_regsql yardımcı programı, bkz: [ASP.NET SQL Server Kayıt Aracı (Aspnet\_regsql.exe)](https://msdn.microsoft.com/en-us/library/ms229862(v=vs.100).aspx).


Bağlantı dizeleri kişinin Yöneticisi çözümde, yerel SQL Server Express örneğini için işaret ettiğinden emin olmak için sonraki adımdır bakın.

**Bağlantı dizelerini güncelleştirmek için**

1. Contact Manager çözümü Visual Studio 2010'da açın.
2. İçinde **Çözüm Gezgini** penceresinde genişletin **ContactManager.Mvc** proje, çift tıklayın ve ardından **Web.config** düğümü.

    > [!NOTE]
    > ContactManager.Mvc proje iki içeriyor *web.config* dosyaları. Proje düzeyi dosyasını düzenlemeniz gerekir.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. İçinde **connectionStrings** öğesi adlı bağlantı dizesini doğrulayın **ApplicationServices** , yerel ASP.NET uygulama hizmetleri veritabanına işaret eder.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. İçinde **Çözüm Gezgini** penceresinde genişletin **ContactManager.Service** proje, çift tıklayın ve ardından **Web.config** düğümü.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. İçinde **connectionStrings** öğesinde adlı bağlantı dizesi **ContactManagerContext**, doğrulayın **veri kaynağı** özelliği, yerel bir SQL örneğine ayarlanmış Server Express. Bağlantı dizesinde başka bir şey değiştirmeniz gerekmez.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Tüm açık dosyalar kaydedin.

Artık yerel makinenizde Contact Manager çözümü çalıştırmaya hazır olması gerekir.

> [!NOTE]
> Uygulama Hizmetleri veritabanına oluşturmadan bu adımları izlerseniz, ASP.NET kullanıcı oluşturma denemesi ilk kez bir veritabanı oluşturun. Ancak, el ile veritabanı oluşturma, desteklemek istediğiniz uygulama hizmetleri özellik kümesi üzerinde çok daha fazla denetim sağlar.


**Contact Manager çözümü çalıştırmak için**

1. Visual Studio 2010'da F5 tuşuna basın.
2. Internet Explorer başlar ve ilgili kişi yöneticisi ASP.NET MVC 3 uygulamasının URL'si ister. Varsayılan olarak, uygulama görüntüler **tüm kişileri** sayfası.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Birkaç kişi ekleyin ve ardından uygulama beklendiği gibi çalıştığını doğrulayın.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Gözat `http://localhost:50114/Account/Register` (uygulaması farklı bir bağlantı noktası üzerinde koyduysanız URL'sini ayarlayın). Bir kullanıcı adı, e-posta adresi ve parola eklemek ve bir hesap başarıyla kaydettirebilir doğrulayın.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Gözat `http://localhost:50114/Account/LogOn` (uygulaması farklı bir bağlantı noktası üzerinde koyduysanız URL'sini ayarlayın). Yeni oluşturduğunuz hesabı kullanarak oturum açması doğrulayın.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Hata ayıklamayı durdurmak için Internet Explorer'ı kapatın.

## <a name="conclusion"></a>Sonuç

Bu noktada, kişinin Yöneticisi çözümü tam olarak yerel makinenizde çalıştırmak için yapılandırılması gerekir. Bu öğreticide diğer konulara çalışırken çözüm bir başvuru olarak kullanabilirsiniz.

Sonraki konuyu [proje dosyası anlama](understanding-the-project-file.md), dağıtım işlemi denetlemek için kişinin Yöneticisi çözüm içinde özel Microsoft Build Engine (MSBuild) proje dosyalarını nasıl kullanabileceğiniz açıklanır.

>[!div class="step-by-step"]
[Önceki](the-contact-manager-solution.md)
[sonraki](understanding-the-project-file.md)
