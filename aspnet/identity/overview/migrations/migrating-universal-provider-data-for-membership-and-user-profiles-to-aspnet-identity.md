---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Üyelik ve ASP.NET ıdentity'ye (C#) kullanıcı profilleri için evrensel sağlayıcı verilerini geçirme | Microsoft Docs
author: rustd
description: Bu öğreticide, kullanıcı ve rol verileri ve mevcut bir uygulamanın Evrensel sağlayıcıları kullanılarak oluşturulan kullanıcı profili verileri geçirmek gerekli olan adımları açıklanmaktadır...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 6115b2a6caca05659f1c35ce97954807a6fb01ae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753958"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Üyelik ve ASP.NET ıdentity'ye (C#) kullanıcı profilleri için evrensel sağlayıcı verilerini geçirme
====================
tarafından [Pranav Rastogi'nin](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Bu öğretici, kullanıcı ve rol verileri ve evrensel sağlayıcıları ASP.NET kimlik modeli için mevcut bir uygulamayla kullanılarak oluşturulan kullanıcı profili verileri geçirmek gereken adımları açıklar. Kullanıcı profili verilerini taşımak için SQL üyeliğe sahip bir uygulamada kullanılabilir yaklaşım burada bahsedilen.


Visual Studio 2013, ASP.NET takımı yeni bir ASP.NET kimlik sistemi sürümlerine ve daha fazla bilgi bu sürüm hakkında [burada](../../index.md). Web uygulamalarından geçirmek için makaleyi takip [SQL üyelik için yeni kimlik sistemi](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), bu makalede kullanıcı ve rol yönetimi sağlayıcıları modelini izleyen mevcut uygulamalarını taşımak üzere adımları gösterilmektedir. Yeni kimlik modeli için. Odak noktası, Bu öğretici, kullanıcı profili verileri sorunsuz bir şekilde, yeni sisteme yeteneklerinizi öncelikle geçirilmesi olacaktır. SQL üyelik için kullanıcı ve rol bilgilerini geçirme benzerdir. Profil verileri geçirmek için izlenen bir yaklaşım, SQL üyeliğe sahip bir uygulamada kullanılabilir.

Örneğin, sağlayıcı modeli kullanan Visual Studio 2012 kullanılarak oluşturulan bir web uygulaması ile başlayacağız. Biz sonra profil yönetimi için kod ekleyin, bir kullanıcı kaydı, kullanıcıların profil verileri ekleme, veritabanı şemasını geçirme ve sonra uygulamayı kimlik sistemi için kullanıcı ve rol yönetimi kullanacak şekilde değiştirin. Test geçiş olarak Evrensel sağlayıcıları kullanılarak oluşturulan kullanıcılar oturum açamaz ve yeni kullanıcılar kaydetme görüyor olmalısınız.

> [!NOTE]
> Tam örneğini şurada bulabilirsiniz [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Profil verileri geçiş özeti

Migrations ile başlatmadan önce bize sağlayıcıları modelde profil verileri depolama deneyimi bakın. Kullanıcılar birden çok yolla depolanabilir uygulaması için profil verileri, aralarında en yaygın kullanarak evrensel sağlayıcıları ile birlikte sevk yerleşik Profil sağlayıcıları. Adımları içerir

1. Profil verilerini depolamak için kullanılan özelliklere sahip bir sınıf ekleyin.
2. 'ProfileBase' genişleten ve kullanıcı için yukarıdaki profil verilerini almak için yöntemleri uygulayan bir sınıf ekleyin.
3. Varsayılan Profil sağlayıcıları kullanarak etkinleştirin *web.config* dosya ve profil bilgilerini erişirken kullanılacak #2. adımda bildirilen sınıf tanımlayın.

Profil bilgileri, serileştirilmiş xml ve veritabanı 'Profilleri' tablosunda ikili veri olarak depolanır.

Yeni ASP.NET kimlik sistemi kullanmak için uygulamayı geçirdikten sonra profil bilgileri seri durumdan ve kullanıcı sınıfındaki özellikleri olarak depolanır. Her bir özellik, ardından kullanıcı tablosunda sütun üzerine eşlenebilir. Burada özellikleri serileştirmek/veri bilgilerini seri durumdan çıkarılacak olmaması yanı sıra kullanıcı sınıfı kullanarak doğrudan çalışılabilmesi avantajlarındandır bunu erişirken zaman.

## <a name="getting-started"></a>Başlarken

1. Visual Studio 2012'de yeni bir ASP.NET 4.5 Web Forms uygulaması oluşturun. Geçerli örnek Web Forms şablonu kullanan, ancak MVC uygulaması de kullanabilirsiniz.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. 'Model' profil bilgilerini depolamak için yeni bir klasör oluşturun  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Örneğin, doğum tarihi, şehir, yükseklik ve Ağırlık kullanıcının profilinde bize depolayın. Yükseklik ve Ağırlık 'PersonalStats' adlı özel bir sınıf depolanır. Profil almak ve depolamak için 'ProfileBase' genişleten bir sınıfı ihtiyacımız var. Yeni bir sınıf profil bilgilerini depolamak ve almak için ' AppProfile oluşturalım.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Profili etkinleştir *web.config* dosya. Depolama / #3. adımda oluşturduğunuz kullanıcı bilgilerini almak için kullanılan sınıf adı girin.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Kullanıcı profili verileri almak ve depolamak için 'Account' klasöründe bir web forms sayfası ekleyin. Projeye sağ tıklayın ve 'Yeni Öğe Ekle' seçin. Ana Sayfa 'AddProfileData.aspx' ile yeni bir webforms sayfası ekleyin. 'MainContent' bölümünde aşağıdakileri kopyalayın:

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Arka plan kod aşağıdaki kodu ekleyin:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Hangi AppProfile altında sınıfı derleme hataları kaldırmak için tanımlanan ad alanı ekleyin.
6. Uygulamayı çalıştırın ve yeni kullanıcı oluşturma kullanıcı adıyla '**olduser'.** 'AddProfileData' sayfasına gidin ve kullanıcı profili bilgilerini ekleyin.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Sunucu Gezgini penceresini kullanarak 'Profilleri' tablosunda serileştirilmiş xml olarak verilerin depolandığı doğrulayabilirsiniz. Visual Studio'da 'Görünüm' menüsünden 'Sunucu Gezgini' seçin. Bir veri bağlantısı için tanımlanan veritabanı olmalıdır *web.config* dosya. Veri bağlantısına tıklayarak farklı alt kategorileri gösterir. Veritabanınızda, farklı tabloları göster sonra 'Profillerde' sağ tıklayın ve 'Tablo verilerini profilleri tablosunda depolanan profil verilerini görüntülemek için Göster' seçin ' tablolar ''ı genişletin.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Veritabanı şema geçişi

Var olan veritabanını kimlik sistemine iş yapmak için özgün veritabanına ekledik alanlarını desteklemek için kimlik veritabanı şemasını güncelleştirmek ihtiyacımız var. Bu yapılabilir SQL betiklerini kullanarak yeni tablolar oluşturun ve mevcut bilgileri kopyalayın. 'Sunucu Gezgini' penceresinde 'tabloları görüntülenecek DefaultConnection' genişletin. Tabloları sağ tıklayın ve 'Yeni sorgu' yi seçin

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

SQL betiği yapıştırın [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) ve çalıştırın. 'DefaultConnection' yenilenmiş olması durumunda, yeni tablolar eklenen görebiliriz. Bilgi geçirildiğini görmek için tabloları içindeki verilerin kontrol edebilirsiniz.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>ASP.NET Identity kullanmak için uygulamayı geçirme

1. ASP.NET kimliği için gerekli Nuget paketlerini yükleyin:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Nuget paketlerini yönetme hakkında daha fazla bilgi bulunabilir [burada](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Tablodaki mevcut verilerle çalışmak için size geri tablolara eşleme ve kimlik sistemi içinde takma model sınıfları oluşturmanız gerekir. Kimlik sözleşmesinin bir parçası olarak model sınıfları Identity.Core dll içinde tanımlanan arabirimleri ya da uygulamanız gerekir veya Microsoft.ASPNET.Identity.entityframework içinde kullanılabilen bu arabirimlerin mevcut uygulamasına genişletebilirsiniz. Biz varolan sınıfları rolünü, kullanıcı oturum açma bilgileri ve kullanıcı taleplerini kullanacaklardır. Bizim Örneğimiz için özel bir kullanıcı kullanılacak ihtiyacımız var. Projeye sağ tıklayın ve 'IdentityModels' yeni bir klasör oluşturun. Yeni bir 'User' sınıfı, aşağıda gösterildiği gibi ekleyin:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Kullanıcı sınıfı bir özellikte 'ProfileInfo' artık olduğuna dikkat edin. Bu nedenle doğrudan profili verilerle çalışmak için kullanıcı sınıfı kullanabiliriz.

Dosyaları kopyalama **IdentityModels** ve **IdentityAccount** klasörleri yükleme kaynağından ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Bu, kalan model sınıfları ve kullanıcı ve ASP.NET Identity API'lerini kullanarak rol yönetimi için gereken yeni sayfa vardır. SQL üyelik için kullanılan yaklaşımı benzer ve ayrıntılı bir açıklama bulunabilir [burada](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Yeni tablolar için profil verileri kopyalama

Daha önce bahsedildiği gibi profilleri tablolarındaki xml verileri seri durumdan ve AspNetUsers tablonun sütunlarında depolamak ihtiyacımız var. Kaldı için bu sütunların gerekli verileri ile doldurmak için önceki adımda kullanıcılar tablosundaki yeni sütunlar oluşturulur. Bunu yapmak için kullanıcılar yeni oluşturulan sütun doldurmak için bir kez çalıştırılan bir konsol uygulaması kullanacağız.

1. Varolan çözüme yeni bir konsol uygulaması oluşturun.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Entity Framework paketini en son sürümünü yükleyin.
3. Konsol uygulaması için başvuru olarak yukarıda oluşturduğunuz web uygulamasına ekleyin. Yapmak için bu sağ tıklatın projede ve 'Add References', sonra çözüm sonra projeye tıklayın ve Tamam'ı tıklatın.
4. Kopyalama kodu Program.cs sınıfında aşağıda. Bu mantık, her kullanıcı için profil verileri okur, 'ProfileInfo' nesnesi olarak serileştirir ve veritabanına depolar.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   İlgili ad alanlarını içermelidir. Bu nedenle bazı kullanılan modeller web uygulaması projesi 'IdentityModels' klasöründe tanımlanır.
5. Yukarıdaki kod, uygulama veritabanı dosyası üzerinde çalışır\_web uygulama projesinin veri klasörü önceki adımlarda oluşturulan. Konsol uygulamasının app.config dosyasında bağlantı dizesi, başvurmak için web uygulamasının web.config içindeki bağlantı dizesiyle güncelleştirin. Ayrıca 'AttachDbFilename' özelliğinde tam fiziksel yolunu belirtin.
6. Bir komut istemi açın ve yukarıdaki konsol uygulamasının bin klasörüne gidin. Yürütülebilir dosyayı çalıştırın ve aşağıdaki görüntüde gösterildiği gibi günlük çıktısını gözden geçirin.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Sunucu Gezgini'nde 'AspNetUsers' tablosunu açın ve özellikleri yeni sütunlardaki verileri doğrulayın. İle ilgili özellik değerlerini güncelleştirilmelidir.

## <a name="verify-functionality"></a>İşlevselliğini doğrulayın

Eski veritabanından bir kullanıcı oturum açma ASP.NET Identity kullanılarak uygulanan yeni eklenen üyelik sayfalarını kullanın. Kullanıcı kimlik bilgilerini kullanarak oturum açması görebilmeniz gerekir. Rol, ekleme, parola değiştirme, yeni bir kullanıcı oluşturma OAuth ekleme gibi diğer işlevlerini deneyin kullanıcı ekleme vb. rolleri için.

Eski kullanıcı ve yeni kullanıcıların profil verileri alınır ve kullanıcı tablosunda depolanan. Eski tablo artık başvurulması gerekir.

## <a name="conclusion"></a>Sonuç

Makaleyi ASP.NET ıdentity'ye üyelik için sağlayıcı modeli kullandığınız web uygulamalarını geçirme işlemi açıklanmaktadır. Makale ayrıca kullanıcıların kimlik sistemine aşılayın profil veri geçiriyorsanız özetlenen. Lütfen soru ve uygulamanızı geçişi sırasında karşılaştığınız sorunları için aşağıda yorum bırakın.

*Rick Anderson ve Robert McMurray'nın makaleyi gözden geçirme için teşekkür ederiz.*
