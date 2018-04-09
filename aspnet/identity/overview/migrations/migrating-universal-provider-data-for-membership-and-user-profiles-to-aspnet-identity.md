---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Üyelik ve ASP.NET Identity (C#) için kullanıcı profilleri için evrensel sağlayıcı veri geçirme | Microsoft Docs
author: rustd
description: Bu öğretici, kullanıcı ve rol verileri ve mevcut uygulama Evrensel sağlayıcıları kullanılarak oluşturulan kullanıcı profili verilerini geçirmek için gerekli olan adımları açıklanmıştır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f65f93b20543d06ea70a9009b6921e297477c99e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Üyelik ve ASP.NET Identity (C#) için kullanıcı profilleri için evrensel sağlayıcısı verileri geçirme
====================
tarafından [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Bu öğretici, kullanıcı ve rol verileri ve Evrensel Sağlayıcılar, ASP.NET Identity modeli varolan bir uygulamanın kullanılarak oluşturulan kullanıcı profili verilerini geçirmek gereken adımları açıklar. Bir yaklaşım, kullanıcı profili verilerini geçirmek için SQL üyeliğe sahip bir uygulamada kullanılabilir burada sözü edilen.


Visual Studio 2013 sürümüyle, yeni bir ASP.NET Identity sistemi ASP.NET takım sunulan ve daha fazla bilgiyi bu sürüm hakkında [burada](../../index.md). Web uygulamalarından geçirmek için makale üzerinde takip [yeni kimlik sistemi SQL üyelik](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), bu makalede kullanıcı ve rol yönetimi için sağlayıcı modelini izler mevcut uygulamalarını taşımak için adımları gösterir Yeni kimlik modeline. Bu öğreticinin odak öncelikle sorunsuz bir şekilde yeni sisteme bağlanmanıza kullanıcı profili verilerini geçirme olacaktır. Kullanıcı ve rol bilgilerini geçirme için SQL üyeliği benzer. Profil verilerini geçirmek ve ardından yaklaşım SQL üyeliğe sahip bir uygulamada kullanılabilir.

Örnek olarak, biz sağlayıcıları modeli kullanan Visual Studio 2012 kullanılarak oluşturulan bir web uygulaması ile başlar. Biz sonra profil yönetimi için kodu ekleyin, bir kullanıcı kaydı, kullanıcılar için profil verileri eklemek, veritabanı şemasını geçirmek ve sonra uygulamayı kimlik sistemi için kullanıcı ve rol yönetimi kullanacak şekilde değiştirin. Bir test geçiş olarak Evrensel sağlayıcıları kullanılarak oluşturulan kullanıcılar oturum açamaz ve yeni kullanıcılar kaydetmeniz gerekir.

> [!NOTE]
> Tam örneğini şurada bulabilirsiniz [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Profil verileri geçiş özeti

İle geçişlerin başlamadan önce bize sağlayıcıları modelinde profil verileri depolamak deneyimi bakın. Kullanıcıların birden çok yolla depolanabilir uygulama için profil verileri, aralarındaki en yaygın Evrensel sağlayıcıları ile birlikte gönderilen yerleşik Profil sağlayıcıları kullanarak. Adımları içerir.

1. Profil verilerini depolamak için kullanılan özelliklere sahip bir sınıf ekleyin.
2. 'ProfileBase' genişleten ve kullanıcı için yukarıdaki profil verileri almak için yöntemleri uygulayan bir sınıf ekleyin.
3. Varsayılan Profil sağlayıcıları kullanarak etkinleştirin *web.config* dosya ve profil bilgilerini erişirken kullanılacak #2. adımda bildirilen sınıfı tanımlayabilirsiniz.

Profil bilgilerini serileştirilmiş xml ve veritabanı 'Profilleri' tablosunda ikili veri olarak depolanır.

Yeni ASP.NET kimlik sistemi kullanmak için uygulamayı taşıdıktan sonra profil bilgileri serisi ve kullanıcı sınıfındaki özellikleri olarak depolanır. Her bir özellik, kullanıcı tablodaki sütunlar üzerine sonra eşlenebilir. Avantajı burada özellikleri serileştirmek/veri bilgilerini seri durumdan zorunluluğunu yanı sıra kullanıcı sınıfı kullanarak doğrudan çalıştığından emin olup bu erişirken zaman.

## <a name="getting-started"></a>Başlarken

1. Visual Studio 2012'de yeni bir ASP.NET 4.5 Web formları uygulaması oluşturun. Geçerli örnek Web Forms şablonunu kullanır, ancak MVC uygulaması de kullanabilirsiniz.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Profil bilgilerini depolamak için ' modeller' yeni bir klasör oluşturun  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Örnek olarak, doğum tarihi, şehir, yükseklik ve Ağırlık kullanıcının profilinde bize depolar. Yükseklik ve Ağırlık 'PersonalStats' adlı özel bir sınıf depolanır. Depolamak ve profil almak için 'ProfileBase' genişleten bir sınıfı gerekiyor. Yeni bir sınıf profil bilgilerini depolamak ve almak için ' AppProfile' oluşturalım.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Profilinde etkinleştirmek *web.config* dosya. Depolama / #3. adımda oluşturduğunuz kullanıcı bilgilerini almak için kullanılan sınıf adı girin.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Web forms sayfası kullanıcı profil verileri almak ve depolamak için 'Account' klasöründe ekleyin. Projeye sağ tıklayın ve 'Yeni Öğe Ekle' seçin. Ana Sayfa 'AddProfileData.aspx' ile yeni bir webforms sayfa ekleyin. Aşağıdaki 'MainContent' bölümünde kopyalayın:

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Arkasındaki kodda aşağıdaki kodu ekleyin:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Hangi AppProfile altında sınıfı derleme hataları kaldırmak için tanımlanmış bir ad ekleyin.
6. Uygulamayı çalıştırın ve kullanıcı adı olan yeni bir kullanıcı oluşturmak '**olduser'.** 'AddProfileData' sayfasına gidin ve kullanıcı için profil bilgilerini ekleyin.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Veriler Sunucu Gezgini penceresini kullanarak 'Profilleri' tablosundaki serileştirilmiş xml olarak depolanır doğrulayabilirsiniz. Visual Studio'da 'Görüntüle' menüsünden 'Sunucu Gezgini' seçin. Bir veri bağlantısı için tanımlanan veritabanı olmalıdır *web.config* dosya. Veri bağlantısı tıklayarak farklı alt kategorileri gösterir. Veritabanınızda, farklı tablolara Göster sonra 'Profilleri' sağ tıklatın ve 'Show Table profilleri tablosunda depolanan profil verilerini görüntülemek için Data' seçin ' tabloları genişletin.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Geçirme veritabanı şeması

Kimlik sistemi ile çalışmak için varolan veritabanı yapmak için şu kimlik veritabanını özgün veritabanına eklediğimiz alanlarını destekleyecek biçimde şemada güncelleştirmeniz gerekir. Bu yapılabilir SQL komut dosyalarını kullanarak yeni tablolar oluşturma ve varolan bilgileri kopyalayın. 'Sunucu Gezgini' penceresinde 'tablolarını görüntülemek için DefaultConnection' genişletin. Tabloları sağ tıklayın ve 'Yeni sorgu' yi seçin

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

SQL komut dosyasından Yapıştır [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) ve çalıştırın. 'DefaultConnection' yenilenirse, yeni tablolar eklenen görebiliriz. İçindeki bilgileri geçirilmiş görmek için tablo verileri kontrol edebilirsiniz.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>ASP.NET kimlik kullanmak üzere uygulamayı geçirme

1. ASP.NET kimliği için gereken Nuget paketlerini yükleyin:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Nuget paketlerini yönetme hakkında daha fazla bilgi bulunabilir [burada](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Tabloda varolan veriler çalışmak için size geri tablolara harita ve kimlik sistemi takma modeli sınıflarını oluşturmanız gerekir. Kimlik sözleşmesinin bir parçası olarak modeli sınıfları Identity.Core DLL'de tanımlanan arabirimleri ya da uygulamanız gerekir veya bu arabirimleri Microsoft.ASPNET.Identity.entityframework içinde kullanılabilir varolan uyarlamasını genişletebilirsiniz. Biz mevcut sınıflarını rolü, kullanıcı oturumu ve kullanıcı talepleri için kullanır. Bizim örnek için özel bir kullanıcı kullanmamız gerekiyor. Projeye sağ tıklayın ve 'IdentityModels' yeni bir klasör oluşturun. Yeni bir 'User' sınıf aşağıda gösterildiği gibi ekleyin:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   'ProfileInfo' artık kullanıcı sınıfı bir özellik olduğuna dikkat edin. Bu nedenle biz doğrudan profili verilerle çalışmak için kullanıcı sınıfını kullanabilirsiniz.

Dosyaları kopyalama **IdentityModels** ve **IdentityAccount** klasörleri yükleme kaynağından ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Bunlar, kalan modeli sınıfları ve kullanıcı ve rol yönetimi ASP.NET Identity API'lerini kullanarak için gereken yeni sayfalar içerir. Kullanılan yaklaşımı SQL üyelik benzer ve ayrıntılı açıklama bulunabilir [burada](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

## <a name="copying-profile-data-to-the-new-tables"></a>Yeni tablolar için profil verileri kopyalama

Daha önce belirtildiği gibi biz profilleri tablolarındaki xml verileri seri durumdan ve AspNetUsers tablosunun sütunları depolamak gerekir. Bu sütunları gerekli verileri ile doldurmak için kalan tüm olacak şekilde yeni sütunlar önceki adımda kullanıcılar tablosundaki oluşturuldu. Bunu yapmak için kullanıcılar tablosunun yeni oluşturulan sütunları doldurmak için bir kez çalışan bir konsol uygulaması kullanacağız.

1. Mevcut çözüme yeni bir konsol uygulaması oluşturun.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Entity Framework paketinin en son sürümünü yükleyin.
3. Konsol uygulaması bir başvuru olarak yukarıda oluşturduğunuz web uygulaması ekleyin. Yapmak için bu sağ projede ve ardından 'Başvuru Ekle', daha sonra çözüm, ardından projeye tıklayın ve Tamam'ı tıklatın.
4. Kopya kodu Program.cs sınıfında aşağıda. Bu mantık her kullanıcı için profil verileri okur, 'ProfileInfo' nesnesi olarak serileştirir ve veritabanına depolar.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Karşılık gelen ad alanları içermelidir şekilde kullanılan modellerin bazıları web uygulama projesi 'IdentityModels' klasöründe tanımlanır.
5. Yukarıdaki kod uygulama veritabanı dosyasında çalışır\_web uygulaması projesi veri klasörü önceki adımlarda oluşturulacak. Başvuran için web uygulamasının web.config dosyasında bağlantı dizesiyle konsol uygulamasındaki app.config dosyasında bağlantı dizesini güncelleştirin. Ayrıca 'AttachDbFilename' özelliğindeki tam fiziksel yolunu belirtin.
6. Bir komut istemi açın ve yukarıdaki konsol uygulaması bin klasörüne gidin. Yürütülebilir dosyayı çalıştırmak ve aşağıdaki görüntüde gösterildiği gibi günlük çıkışı gözden geçirin.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Server Explorer'da 'AspNetUsers' tablo açın ve yeni sütunlardaki özellikleri tutmak verileri doğrulayın. İle ilgili özellik değerlerini güncelleştirilmelidir.

## <a name="verify-functionality"></a>İşlevselliğini doğrulayın

Eski veritabanından bir kullanıcı oturum açma ASP.NET Identity kullanılarak uygulanan yeni eklenen üyelik sayfalarını kullanın. Kullanıcı kimlik bilgilerini kullanarak oturum açması görebilmeniz gerekir. Rol, ekleme, parola değiştirme, yeni bir kullanıcı oluşturma OAuth ekleme gibi diğer işlevlerini deneyin kullanıcıları ekleyin rolleri, vb. için.

Eski kullanıcı ve yeni kullanıcılar için profil verileri alınır ve kullanıcıların tablosunda depolanır. Eski tablo artık başvurulmayan.

## <a name="conclusion"></a>Sonuç

Makaleyi sağlayıcı modeline üyelik ASP.NET kimliği için kullanılan web uygulamalarını geçirme işlemi açıklanmaktadır. Makale ayrıca kullanıcıların kimlik sisteme sayfaya profil verileri geçirme özetlenen. Sorular ve uygulamanızı geçirirken karşılaşılan sorunları için aşağıda yorum bırakın.

*Rick Anderson ve Robert McMurray'nın makaleyi gözden geçirme için teşekkür ederiz.*
