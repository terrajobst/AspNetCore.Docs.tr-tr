---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Kişi Yöneticisi çözümünü ayarlama | Microsoft Docs
author: jrjlee
description: Bu konuda, indirin ve yerel olarak bir geliştirici iş istasyonunda çalıştırmak için Kişi Yöneticisi çözümünü yapılandırma açıklanmaktadır.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 479dbb8d2edbe9fb953ea9e1312ffb8fdbd3e2fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802137"
---
<a name="setting-up-the-contact-manager-solution"></a>Kişi Yöneticisi çözümünü ayarlama
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, indirin ve yerel olarak bir geliştirici iş istasyonunda çalıştırmak için Kişi Yöneticisi çözümünü yapılandırma açıklanmaktadır.


## <a name="system-requirements"></a>Sistem Gereksinimleri

Kişi Yöneticisi çözümü yerel olarak çalıştırın ve Bu öğreticide açıklanan diğer görevleri gerçekleştirmek için geliştirici iş istasyonunuzda bu yazılımı yüklemeniz gerekir:

- Visual Studio 2010 Service Pack 1, Premium veya Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS Web Dağıtım Aracı (Web dağıtımı) 2.1 veya üzeri
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1 

Visual Studio 2010 dışında indirin ve tüm bu ürünleri ve bileşenleriyle en son sürümlerini yüklemek [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>İndirin ve çözüm ayıklayın

Kişi Yöneticisi örnek uygulamayı MSDN kod Galerisi'nden indirebilirsiniz [burada](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Yapılandırma ve çözümü çalıştırın

Yapılandırma ve Kişi Yöneticisi çözümünü yerel makinenizde çalıştırma için üst düzey adımları gerçekleştirmeniz gerekir:

1. Yerel bir ASP.NET uygulama hizmetleri veritabanına, zaten yoksa, üyelik ve rol yönetimi özellikleriyle birlikte oluşturun.
2. Bağlantı dizelerini düzenlemek *web.config* dosyaları, yerel SQL Server Express örneğine işaret edecek şekilde.
3. Visual Studio 2010'dan çözümü çalıştırın.

Bu bölümün geri kalanında bu görevlerin her birini tamamlamak hakkında daha fazla rehberlik sağlar.

**Uygulama Hizmetleri veritabanı oluşturmak için**

1. Visual Studio 2010 Komut istemi açın. Bunu yapmak için **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft Visual Studio 2010**, tıklayın **Visual Studio Araçları**ve ardından tıklayın **Visual Studio komut istemi (2010)**.
2. Komut isteminde bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Kullanım **– C** geçme, veritabanı sunucusu için bağlantı dizesini belirtin.
    2. Kullanım **–A** uygulama hizmetleri veritabanına eklemek istediğiniz özellikleri belirlemek için anahtarı. Bu durumda, **m** üyelik sağlayıcısı için destek eklemek istediğiniz gösterir ve **r** Rol Yöneticisi için destek eklemek istediğiniz gösterir.
    3. Kullanım **– d** geçme uygulama hizmetleri veritabanınız için bir ad belirtin. Bu anahtar atlarsanız, yardımcı programı varsayılan adı ile bir veritabanı oluşturma **aspnetdb**.
3. Veritabanı başarıyla oluşturuldu, komut istemini bir onay gösterir.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> ASP.NET hakkında daha fazla bilgi için\_regsql yardımcı programı, bkz: [ASP.NET SQL Server Kayıt Aracı (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Sonraki adım, kişi yöneticisi çözümünü bağlantı dizelerini, SQL Server Express'in yerel örneğine işaret ettiğinden emin olmaktır.

**Bağlantı dizelerini güncelleştirmek için**

1. Kişi Yöneticisi çözümü Visual Studio 2010'da açın.
2. İçinde **Çözüm Gezgini** penceresini genişletin **ContactManager.Mvc** proje ve çift tıklatarak **Web.config** düğümü.

    > [!NOTE]
    > İki ContactManager.Mvc projeyi içeren *web.config* dosyaları. Proje düzeyi dosyasını düzenlemeniz gerekir.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. İçinde **connectionStrings** öğesi adlı bağlantı dizesini doğrulayın **ApplicationServices** yerel ASP.NET uygulama hizmetleri veritabanına işaret eder.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. İçinde **Çözüm Gezgini** penceresini genişletin **ContactManager.Service** proje ve çift tıklatarak **Web.config** düğümü.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. İçinde **connectionStrings** öğesinde adlı bağlantı dizesi **ContactManagerContext**, doğrulayın **veri kaynağı** özelliği, yerel bir SQL örneğine ayarlanır Server Express. Bağlantı dizesinin başka bir şey değiştirmeniz gerekmez.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Açık olan tüm dosyaları kaydedin.

Şimdi yerel makinenizde Kişi Yöneticisi çözümü çalıştırmak hazır olmanız gerekir.

> [!NOTE]
> Uygulama Hizmetleri veritabanına oluşturmadan bu adımları izlerseniz, ASP.NET bir kullanıcı oluşturmaya ilk kez bir veritabanı oluşturun. Ancak, el ile veritabanı oluşturma, desteklemek istediğiniz uygulama hizmetleri özellik kümesi üzerinde çok daha fazla denetim sağlar.


**Kişi Yöneticisi çözümü çalıştırmak için**

1. Visual Studio 2010'da, F5 tuşuna basın.
2. Internet Explorer başlar ve Kişi Yöneticisi ASP.NET MVC 3 uygulama URL'sini ister. Varsayılan olarak, uygulama görüntüler **tüm kişileri** sayfası.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Bazı kişiler ekleyin ve ardından uygulamanın beklendiği gibi çalıştığını doğrulayın.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Gözat `http://localhost:50114/Account/Register` (uygulama farklı bir bağlantı noktası üzerinde koyduysanız URL'sini ayarlayın). Bir kullanıcı adı, e-posta adresi ve parola ekleyin ve hesap başarıyla kaydı mümkün olmadığını doğrulayın.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Gözat `http://localhost:50114/Account/LogOn` (uygulama farklı bir bağlantı noktası üzerinde koyduysanız URL'sini ayarlayın). Yeni oluşturduğunuz hesapla oturum açabilmesi doğrulayın.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Hata ayıklamayı durdurmak için Internet Explorer'ı kapatın.

## <a name="conclusion"></a>Sonuç

Bu noktada, kişi yöneticisi çözümünü yerel makinenizde çalıştırmak için tam olarak yapılandırılmalıdır. Bu öğreticide diğer konu başlıklarını çalışırken, çözüm başvuru olarak kullanabilirsiniz.

Bir sonraki konu [proje dosyasını anlama](understanding-the-project-file.md), dağıtım işlemini denetlemek için özel Microsoft Build Engine (MSBuild) proje dosyaları içinde Kişi Yöneticisi çözümünü nasıl kullanabileceğinizi açıklar.

> [!div class="step-by-step"]
> [Önceki](the-contact-manager-solution.md)
> [İleri](understanding-the-project-file.md)
