---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Form kimlik doğrulamasını (C#) ile kullanıcıların kimlik doğrulaması | Microsoft Docs
author: microsoft
description: '[Authorize] özniteliği kullanmayı öğrenin MVC uygulamanızda belirli sayfaları için parola koruması. Web sitesi yönetim çok kullanmayı öğrenin...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: e1def84bbf48847339e89b239b026d053640b935
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-c"></a>Form kimlik doğrulaması (C#) ile kullanıcıların kimlik doğrulaması
====================
tarafından [Microsoft](https://github.com/microsoft)

> [Authorize] özniteliği kullanmayı öğrenin MVC uygulamanızda belirli sayfaları için parola koruması. Kullanıcılar ve roller oluşturmak ve yönetmek için Web Sitesi Yönetim Aracı'nı kullanmayı öğrenin. Ayrıca kullanıcı hesabı ve rol bilgilerini depolandığı yapılandırmayı öğrenin.


Bu öğreticinin amacı formlar nasıl kullanabileceğinizi açıklar etmektir parola kimlik doğrulaması, ASP.NET MVC uygulamalarınızı görünümlerde koruma. Kullanıcılar ve roller oluşturmak için Web Sitesi Yönetim Aracı'nı kullanmayı öğrenin. Ayrıca yetkisiz kullanıcıların denetleyici eylemleri yürütmesini engelleme öğrenin. Son olarak, kullanıcı adları ve parolalar depolandığı yapılandırma konusunda bilgi edinin.

#### <a name="using-the-web-site-administration-tool"></a>Web Sitesi Yönetim Aracı'nı kullanma

Bunu başka bir şey yapmadan önce biz bazı kullanıcılar ve roller oluşturarak başlamanız gerekir. Visual Studio 2008 Web Sitesi Yönetim Aracı'nı yararlanmak için yeni kullanıcılar ve roller oluşturmak için en kolay yolu değil. Menü seçeneğini seçerek, bu aracı başlatabilirsiniz **proje, ASP.NET yapılandırma**. Alternatif olarak, Çözüm Gezgini penceresinin en üstünde görünür world basarsa Çekiç (biraz korkutucu) simgesini tıklatarak Web Sitesi Yönetim Aracı'nı başlatabilirsiniz (bkz: Şekil 1).

**Şekil 1 – Web Sitesi Yönetim Aracı'nı başlatma**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

Web sitesi yönetim aracı içinde yeni kullanıcılar ve roller Güvenlik sekmesini seçerek oluşturun. Tıklatın **kullanıcı oluşturma** Stephen adlı yeni bir kullanıcı oluşturmak için bağlantı (bkz: Şekil 2). İstediğiniz herhangi bir parolayla Stephen kullanıcı sağlayın (örneğin, *gizli*).

**Şekil 2 – yeni bir kullanıcı oluşturma**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

İlk rol etkinleştirme ve bir veya daha fazla rollerini tanımlama yeni rolleri oluşturun. Tıklayarak rolleri etkinleştir **rolleri etkinleştir** bağlantı. Ardından, adlı bir rol oluşturmak *Yöneticiler* tıklayarak **roller oluştur veya Yönet** (bkz: Şekil 3) bağlantı.

**Şekil 3 – yeni rol oluşturma**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Son olarak, değiştirmemesi adlı yeni bir kullanıcı oluşturmak ve kullanıcı oluştur bağlantıyı tıklatarak ve yöneticiler değiştirmemesi oluştururken seçerek değiştirmemesi yöneticileri rolü ile ilişkilendir (Şekil 4'e bakın).

**Şekil 4 – bir role kullanıcı ekleme**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Tüm denirse ve bitti Stephen ve değiştirmemesi adlı iki yeni kullanıcılar olması gerekir. Ayrıca Yöneticiler adlı yeni bir role sahip olmalıdır. Değiştirmemesi Yöneticileri rolünün üyesi ve Stephen değil.

#### <a name="requiring-authorization"></a>Yetkilendirme gerektirme

Bir kullanıcının kullanıcı eylemine [Authorize] özniteliğini ekleyerek bir denetleyici eylemi çağırır önce doğrulanması için gerektirebilir. [Authorize] özniteliği için tek tek denetleyici eylem uygulayabilir veya bu öznitelik bir tüm denetleyicinin sınıf uygulayabilirsiniz.

Örneğin, denetleyici listeleme 1 CompanySecrets() adlı bir eylem kullanıma sunar. Bu eylem [Authorize] özniteliği ile donatılmış olduğundan, bir kullanıcının kimliği doğrulanır sürece bu eylem çağrılamaz.

**1 – Controllers\HomeController.cs listeleme**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Tarayıcınızın adres çubuğuna URL'yi /Home/CompanySecrets girerek CompanySecrets() eylemi çağırmak ve kimliği doğrulanmış bir kullanıcı olmayan varsa, otomatik olarak oturum açma görünümüne yönlendirilecek (bkz. Şekil 5).

**Şekil 5 – oturum açma görünümü**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Kullanıcı adı ve parola belirtmek için oturum açma görünümü kullanabilirsiniz. Kayıtlı kullanıcı olmayan sonra tıklayabilirsiniz **kaydetmek** kasaya gitmek için bağlantı (bkz. Şekil 6) görüntüleyin. Kayıt görünümü, yeni bir kullanıcı hesabı oluşturmak için kullanabilirsiniz.

**Şekil 6 – kayıt görünümü**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Başarıyla oturum açtıktan sonra (bkz. Şekil 7) görüntülemek CompanySecrets görebilirsiniz. Varsayılan olarak, tarayıcı penceresini kapatana kadar oturum açmanız devam eder.

**Şekil 7 – CompanySecrets görünümü**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Kullanıcı adı veya kullanıcı rolü tarafından yetkilendirme

Bir denetleyici eylemi fazla kullanıcı ya da belirli bir kullanıcı rolleri kümesi için belirli bir küme için erişimi kısıtlamak için [Authorize] özniteliğini kullanabilirsiniz. Örneğin, değiştirilmiş giriş denetleyicisi listeleme 2'deki StephenSecrets() ve AdministratorSecrets() adlı iki yeni eylemleri içerir.

**2 – Controllers\HomeController.cs listeleme**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Yalnızca bir kullanıcı kullanıcı adıyla Stephen StephenSecrets() eylem çağırabilirsiniz. Diğer tüm kullanıcılar için oturum açma görünümü yönlendirilirsiniz. Kullanıcıların özelliği kullanıcı hesabı adlarının virgülle ayrılmış listesini kabul eder.

Yalnızca Yöneticiler rolündeki kullanıcılar AdministratorSecrets() eylem çağırabilirsiniz. Örneğin, değiştirmemesi Yöneticiler grubunun bir üyesi olduğundan, kendisi AdministratorSecrets() eylem çağırabilirsiniz. Diğer tüm kullanıcılar için oturum açma görünümü yönlendirilirsiniz. Roller özelliği Rol adlarının virgülle ayrılmış listesini kabul eder.

#### <a name="configuring-authentication"></a>Kimlik doğrulamasını yapılandırma

Bu noktada, kullanıcı hesabı ve rol bilgilerini nerede depolanıyor merak ediyor olabilirsiniz. Varsayılan olarak, bilgileri (nedeniyle RANU) SQL Express adlandırılmış ASPNETDB.mdf MVC uygulamanızın uygulamada bulunan bir veritabanında depolanan\_veri klasörü. Bu veritabanı üyelik kullanmaya başladığınızda, ASP.NET framework tarafından otomatik olarak oluşturulur.

Solution Explorer penceresi ASPNETDB.mdf veritabanında görmek için önce tüm dosyaları göster menü seçeneğini projenin seçmeniz gerekir.

SQL Express varsayılan veritabanı kullanan bir uygulama geliştirirken uygundur. Büyük olasılıkla, ancak bir üretim uygulaması için varsayılan ASPNETDB.mdf veritabanını kullanmak istemezsiniz. Bu durumda, kullanıcı hesabı bilgilerini aşağıdaki iki adımları tamamlayarak depolandığı değiştirebilirsiniz:

1. Uygulama Hizmetleri veritabanı nesnelerini üretim veritabanınız için add - üretim veritabanınız işaret etmek için uygulama bağlantı dizesini değiştirin

İlk adım üretim veritabanınız için tüm gerekli veritabanı nesnelerini (tabloları ve saklı yordamlar) eklemektir. Bu nesne için yeni bir veritabanı eklemek için en kolay ASP.NET SQL Sunucusu Kurulum Sihirbazı'nı yararlanmak için yoludur (bkz. Şekil 8). Bu araç, Microsoft Visual Studio 2008 program grubundan Visual Studio 2008 komut istemi açarak ve komut isteminden aşağıdaki komutu yürütülürken başlatabilirsiniz:

aspnet\_regsql

**ASP.NET SQL Sunucusu Kurulum Sihirbazı'nı 8 – Şekil**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

ASP.NET SQL Server Kurulum Sihirbazı, ağınızdaki bir SQL Server veritabanını seçin ve tüm ASP.NET uygulama hizmetleri tarafından gerekli veritabanı nesnelerini yüklemenize olanak sağlar. Veritabanı sunucusunun yerel makinenizde bulunması gerekli değildir.

> [!NOTE] 
> 
> ASP.NET SQL Sunucusu Kurulum Sihirbazı'nı kullanmak istemiyorsanız, aşağıdaki klasörde uygulama Hizmetleri veritabanı nesnelerini eklemek için SQL komut dosyaları bulabilirsiniz:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


Gerekli veritabanı nesnelerini oluşturduktan sonra MVC uygulamanız tarafından kullanılan veritabanı bağlantısı değiştirmeniz gerekir. Üretim veritabanına işaret eden web (web.config) yapılandırma dosyanızda ApplicationServices bağlantı dizesini değiştirin. Örneğin, listeleme 3'te değiştirilmiş bağlantı MyProductionDB (özgün ApplicationServices bağlantı dizesi yorum olarak belirtilmiştir) adlı bir veritabanı işaret eder.

**3 – Web.config listeleme**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Veritabanı izinleri yapılandırma

Veritabanınıza bağlanmak için tümleşik güvenliği kullanırsanız, doğru Windows kullanıcı hesabı olarak oturum açın, veritabanına eklemeniz gerekir. ASP.NET geliştirme sunucusu veya Internet Information Services web sunucunuz olarak kullanıp üzerinde doğru hesap bağlıdır. Doğru kullanıcı hesabı Ayrıca, işletim sistemine bağlıdır.

ASP.NET Geliştirme Sunucusu (Visual Studio tarafından kullanılan varsayılan web sunucusu) kullanıyorsanız, uygulamanızı Windows kullanıcı hesabı bağlamında yürütür. Bu durumda, veritabanı sunucusu oturumu açma, Windows kullanıcı hesabı eklemeniz gerekir.

Internet Information Services kullanıyorsanız, alternatif olarak, daha sonra ASP.NET hesabından veya NT Yetkilisi/ağ hizmeti hesabı veritabanı server oturumu açma eklemeniz gerekir. Windows XP kullanıyorsanız, ASP.NET hesabından veritabanınız için bir oturum olarak ekleyin. Windows Vista veya Windows Server 2008 gibi daha yeni bir işletim sistemi kullanıyorsanız, NT Yetkilisi/ağ hizmeti hesabı veritabanı oturumu olarak ekleyin.

Microsoft SQL Server Management Studio'yu kullanarak, veritabanına yeni bir kullanıcı hesabı ekleyebilirsiniz (bkz. Şekil 9).

**Şekil 9 – yeni bir Microsoft SQL Server oturum açma oluşturma**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Gerekli oturum açma oluşturduktan sonra doğru veritabanı rolleri ile bir veritabanı kullanıcısı için oturum açma eşlemeniz gerekir. Oturum açma çift tıklayın ve kullanıcı eşleme sekmesini seçin. Bir veya daha fazla uygulama Hizmetleri veritabanı rolleri seçin. Örneğin, kullanıcıların kimliğini doğrulamak için aspnet etkinleştirmeniz gerekiyor\_üyelik\_BasicAccess veritabanı rolü. Yeni kullanıcılar oluşturmak için ASP.NET etkinleştirmeniz gerekir\_üyelik\_FullAccess veritabanı rolü (bkz. Şekil 10).

**Şekil 10 – uygulama Hizmetleri veritabanı rolleri ekleme**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Özet

Bu öğreticide, bir ASP.NET MVC uygulaması oluştururken, Forms kimlik doğrulaması kullanan öğrendiniz. İlk olarak, Web Sitesi Yönetim Aracı'nı yararlanarak yeni kullanıcılar ve roller oluşturma hakkında bilgi edindiniz. Ardından, [Authorize] özniteliği denetleyici eylemleri çağırma yetkisiz kullanıcıların önlemek için nasıl kullanılacağı hakkında bilgi edindiniz. Son olarak, bir üretim veritabanında kullanıcı ve rol bilgilerini depolamak için MVC uygulamasının nasıl yapılandırılacağını öğrendiniz.

> [!div class="step-by-step"]
> [Next](authenticating-users-with-windows-authentication-cs.md)
