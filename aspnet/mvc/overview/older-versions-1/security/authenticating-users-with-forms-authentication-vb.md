---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Forms kimlik doğrulama (VB) ile kullanıcıların kimliğini doğrulama | Microsoft Docs
author: microsoft
description: '[Authorize] özniteliği kullanmayı öğrenin MVC uygulamanızda belirli sayfalara parolanızı koruyun. Web sitesi yönetim çok kullanmayı öğrenin...'
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: a9d55df3132a5b5ceeb49ed6d0b83b847f2f1e3b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828860"
---
<a name="authenticating-users-with-forms-authentication-vb"></a>Form kimlik doğrulaması (VB) ile kullanıcıların kimliğini doğrulama
====================
tarafından [Microsoft](https://github.com/microsoft)

> [Authorize] özniteliği kullanmayı öğrenin MVC uygulamanızda belirli sayfalara parolanızı koruyun. Kullanıcılar ve roller oluşturmak ve yönetmek için Web Sitesi Yönetim Aracı'nı kullanmayı öğrenin. Ayrıca kullanıcı hesabı ve rol bilgilerini depolandığı yapılandırmayı öğrenin.


Bu öğreticide formların nasıl kullanabileceğiniz açıklayacak şekilde hedefidir parola kimlik doğrulaması, ASP.NET MVC uygulamaları görünümlerde koruyun. Kullanıcıları ve rolleri oluşturmak için Web Sitesi Yönetim Aracı'nı kullanmayı öğrenin. Ayrıca yetkisiz kullanıcıların denetleyici eylemleri yürütmesini engelleme öğrenin. Son olarak, kullanıcı adları ve parolaların depolandığı yapılandırma konusunda bilgi edinin.

#### <a name="using-the-web-site-administration-tool"></a>Web Sitesi Yönetim Aracı'nı kullanma

Bunu başka bir şey yapmadan önce ki bazı kullanıcılar ve roller oluşturarak başlamanız gerekir. Visual Studio 2008 Web Sitesi Yönetim Aracı'nı yararlanmak için yeni kullanıcılar ve roller oluşturmak için en kolay yolu olan. Menü seçeneğini belirleyerek bu araç başlatabilirsiniz **proje, ASP.NET yapılandırma**. Alternatif olarak, Çözüm Gezgini penceresinin en üstünde görünen dünyanın ulaşmaktan Çekiç (biraz scary) simgesini tıklayarak Web Sitesi Yönetim Aracı'nı çalıştırmanızı sağlayan (bkz. Şekil 1).

**Şekil 1 – Web Sitesi Yönetim Aracı'nı başlatma**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Web Sitesi Yönetim Aracı'içinde yeni kullanıcılar ve roller Güvenlik sekmesini seçerek oluşturun. Tıklayın **kullanıcı oluşturma** Stephen adlı yeni bir kullanıcı oluşturmak için bağlantı (bkz: Şekil 2). Stephen kullanıcı istediğiniz herhangi bir parolayla sağlayın (örneğin, *gizli*).

**Şekil 2-yeni kullanıcı oluşturma**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

İlk rol etkinleştirme ve bir veya daha fazla rollerini tanımlama yeni roller oluşturun. Rolleri etkinleştirmek **rolleri etkinleştir** bağlantı. Ardından, adlı bir rol oluşturun *Yöneticiler* tıklayarak **oluşturabilir veya rolleri yönetme** (bkz: Şekil 3) bağlayın.

**Şekil 3 – yeni rol oluşturma**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Son olarak, Sally adlı yeni bir kullanıcı oluşturun ve sally'nin satış Create User bağlantıya tıklamak ve sally'nin satış oluştururken seçmek, yöneticilerin yalnızca Yöneticiler rolü ile ilişkilendir (bkz: Şekil 4).

**Şekil 4 – bir kullanıcı bir role ekleniyor**

![clip_image008 [4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Tüm belirtti ve bitti Stephen ve Sally adlı iki yeni kullanıcı olması gerekir. Ayrıca, yöneticilerin adlı yeni bir rolü de sahip olmalıdır. Sally Yöneticiler rolünün bir üyesidir ve Stephen değildir.

#### <a name="requiring-authorization"></a>Yetkilendirme gerektiren

Kullanıcı eylemi [Authorize] özniteliği ekleyerek bir denetleyici eylemi çağırır. önce doğrulanması bir kullanıcı gerektirebilir. [Authorize] özniteliği için ayrı ayrı denetleyicisinin eylem uygulayabilir veya bu öznitelik için bir tüm denetleyicinin sınıf uygulayabilirsiniz.

Örneğin, denetleyici 1 listeleme CompanySecrets() adlı bir eylem kullanıma sunar. Bu eylem [Authorize] özniteliği ile donatılmış olduğundan, bir kullanıcının kimlik doğrulamasını sürece bu eylem çağrılamaz.

**1 – Controllers\HomeController.vb listeleme**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Tarayıcınızın adres çubuğuna URL'yi /Home/CompanySecrets girerek CompanySecrets() eylemin çağırması ve kimliği doğrulanmış bir kullanıcı olmayan varsa, otomatik olarak oturum açma görünümüne yönlendirilir (bkz: Şekil 5).

**Şekil 5-oturum açma görünümü**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Kullanıcı adınızı ve parolanızı girmek için oturum açma görünümü kullanabilirsiniz. Kayıtlı kullanıcı olmayan sonra tıklayabilirsiniz **kaydetme** kasaya gidilecek bağlantıya (bkz. Şekil 6) görüntüleyin. Kayıt görünümü, yeni bir kullanıcı hesabı oluşturmak için kullanabilirsiniz.

**Şekil 6: Register görüntüle**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Başarıyla oturum açtıktan sonra (bkz. Şekil 7) görüntüleme CompanySecrets görebilirsiniz. Varsayılan olarak, tarayıcı penceresini kapatana kadar oturum açma devam eder.

**Şekil 7 – CompanySecrets görüntüle**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Kullanıcı adı veya kullanıcı rolü tarafından yetkilendirme

[Authorize] özniteliği bir denetleyici eylemi kullanıcı ya da belirli bir kullanıcı rolleri kümesi için belirli bir veri kümesine erişimi kısıtlamak için kullanabilirsiniz. Örneğin, 2 listeleme değiştirilmiş giriş denetleyicisine StephenSecrets() ve AdministratorSecrets() adlı iki yeni eylemler içerir.

**2 – Controllers\HomeController.vb listeleme**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Yalnızca bir kullanıcı kullanıcı adıyla Stephen StephenSecrets() eylemini çağırabilirsiniz. Diğer tüm kullanıcılar oturum açma görünümüne yeniden yönlendirilen. Kullanıcıların özelliği, kullanıcı hesabı adlarını virgülle ayrılmış listesini kabul eder.

Yalnızca Yöneticiler rolündeki kullanıcılar AdministratorSecrets() eylemini çağırabilirsiniz. Örneğin, Filiz sally'nin satış yöneticileri grubunun bir üyesi olduğundan, AdministratorSecrets() eylemini çağırabilirsiniz. Diğer tüm kullanıcılar oturum açma görünümüne yeniden yönlendirilen. Roller özelliği Rol adlarının virgülle ayrılmış listesini kabul eder.

#### <a name="configuring-authentication"></a>Kimlik doğrulamasını yapılandırma

Bu noktada, kullanıcı hesabı ve rol bilgilerini nerede depolandığını merak ediyor olabilirsiniz. Varsayılan olarak, ilgili bilgiler ve adlandırılmış ASPNETDB.mdf MVC uygulamanızın uygulamada bulunan (nedeniyle RANU) SQL Express veritabanında depolanır\_veri klasörü. Bu veritabanı, üyelik kullanarak başlattığınızda ASP.NET çerçevesi tarafından otomatik olarak oluşturulur.

Çözüm Gezgini penceresinde ASPNETDB.mdf veritabanı görmek için ilk menü seçeneği projenin tüm dosyaları göster seçmeniz gerekir.

Varsayılan SQL Express veritabanı ile bir uygulama geliştirirken uygundur. Büyük olasılıkla, ancak bir üretim uygulaması için varsayılan ASPNETDB.mdf veritabanı kullanmak istemezsiniz. Bu durumda, aşağıdaki iki adımı izleyerek kullanıcı hesabı bilgilerini nerede depolandığını değiştirebilirsiniz:

1. Uygulama Hizmetleri veritabanı nesnelerini üretim veritabanınız için add - uygulama bağlantı dizenizi üretim veritabanına işaret edecek şekilde değiştirin

İlk adım üretim veritabanınız için tüm gerekli veritabanı nesnelerini (tablolarına ve depolanmış yordamlarına) eklemektir. ASP.NET SQL Sunucusu Kurulum Sihirbazı'nın avantajlarından yararlanmak için yeni bir veritabanı için bu nesneleri eklemek için en kolay yolu olan (bkz. Şekil 8). Bu araç, Microsoft Visual Studio 2008 program grubundan Visual Studio 2008 Komut İstemi'ni açarak ve komut isteminden aşağıdaki komutu yürütülürken başlatabilirsiniz:

ASP.NET\_regsql

**8 – ASP.NET SQL Server Kurulum Sihirbazı'nı Şekil**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

ASP.NET SQL Sunucusu Kurulum Sihirbazı, ağınızdaki bir SQL Server veritabanını seçin ve ASP.NET uygulama hizmetleri tarafından gerekli veritabanı nesnelerini yüklemeniz sağlar. Veritabanı sunucusu yerel makinenizde bulunması gerekli değildir.

> [!NOTE]
> ASP.NET SQL Server Kurulum Sihirbazı kullanmak istemiyorsanız aşağıdaki klasörde uygulama Hizmetleri veritabanı nesneleri eklemek için SQL komut dosyaları bulabilirsiniz:
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727


Gerekli veritabanı nesnelerini oluşturduktan sonra MVC uygulamanız tarafından kullanılan veritabanı bağlantısını değiştirmeniz gerekir. Üretim veritabanına işaret eden web (web.config) yapılandırma dosyanızdaki ApplicationServices bağlantı dizesini değiştirin. Örneğin, listeleme 3'te değiştirilmiş bağlantı MyProductionDB (özgün ApplicationServices bağlantı dizesini yorum olarak belirtilmiştir) adlı bir veritabanına işaret eder.

**3 – Web.config listeleme**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Veritabanı izinlerini yapılandırma

Veritabanınıza bağlanmak için tümleşik güvenlik kullanırsanız doğru Windows kullanıcı hesabı olarak oturum açın, veritabanına eklemek gerekir. Doğru hesap olup olmadığını ASP.NET geliştirme sunucusu ya da Internet Information Services, web sunucusu olarak kullandığınız bağlıdır. Doğru kullanıcı hesabı Ayrıca, işletim sistemine bağlıdır.

ASP.NET Geliştirme Sunucusu (Visual Studio tarafından kullanılan varsayılan web sunucusu) kullanıyorsanız, uygulamanızı Windows kullanıcı hesabınızın bağlamında yürütür. Bu durumda, Windows kullanıcı hesabınızın veritabanı sunucusu oturum açma eklemeniz gerekir.

Internet Information Services kullanıyorsanız alternatif olarak, daha sonra ASP.NET hesabı veya NT AUTHORITY/NETWORK SERVICE hesabı veritabanı sunucusu oturum açma eklemeniz gerekir. Windows XP kullanıyorsanız hesabından veritabanınız için bir oturum olarak ekleyin. Windows Vista veya Windows Server 2008 gibi daha yeni bir işletim sistemi kullanıyorsanız, NT Yetkilisi/ağ hizmeti hesabı veritabanı oturumu ekleyin.

Microsoft SQL Server Management Studio'yu kullanarak veritabanına yeni bir kullanıcı hesabı ekleyebilirsiniz (bkz. Şekil 9).

**Şekil 9 – yeni bir Microsoft SQL Server oturum açma oluşturma**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Gerekli oturum açma oluşturduktan sonra doğru veritabanı rollerine sahip bir veritabanı kullanıcısı için oturum açma eşlemeniz gerekir. Oturum açma çift tıklayın ve kullanıcı eşleme sekmesini seçin. Bir veya daha fazla uygulama Hizmetleri veritabanı rolleri seçin. Örneğin, kullanıcıların kimliklerini doğrulamak için aspnet sağlamak gerekir\_üyelik\_BasicAccess veritabanı rolü. Yeni kullanıcılar oluşturmak için ASP.NET etkinleştirmeniz gerektiğini\_üyelik\_FullAccess veritabanı rolü (bkz. Şekil 10).

**Şekil 10 – uygulama Hizmetleri veritabanı rolleri ekleme**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Özet

Bu öğreticide bir ASP.NET MVC uygulaması oluştururken form kimlik doğrulaması kullanmayı öğrendiniz. İlk olarak, yeni kullanıcılar ve roller Web Sitesi Yönetim Aracı'nı avantajlarından yararlanarak oluşturmayı öğrendiniz. Ardından, yetkisiz kullanıcıların denetleyici eylemleri yürütmesini önlemek için [Authorize] özniteliği kullanmayı öğrendiniz. Son olarak, bir üretim veritabanında kullanıcı ve rol bilgilerini depolamak için MVC uygulamasının nasıl yapılandırılacağını öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](preventing-javascript-injection-attacks-cs.md)
> [İleri](authenticating-users-with-windows-authentication-vb.md)
