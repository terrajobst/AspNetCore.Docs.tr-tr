---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: "OAuth sağlayıcılarıyla MVC 4 ile kullanma | Microsoft Docs"
author: tfitzmac
description: "Bu öğretici Facebo gibi harici bir sağlayıcıdan kimlik bilgilerinizle oturum açmalarını sağlar bir ASP.NET MVC 4 web uygulamasının nasıl oluşturulacağını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: f0d053cecbf9a59f258470ee370852e3f112908c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-oauth-providers-with-mvc-4"></a>OAuth sağlayıcılarıyla MVC 4 ile kullanma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici, kullanıcıların Facebook, Twitter, Microsoft veya Google, gibi harici bir sağlayıcıdan kimlik bilgilerinizle oturum açın ve bazı işlevlerini bu sağlayıcılardan tümleştirmenize olanak sağlayan bir ASP.NET MVC 4 web uygulamasının nasıl oluşturulacağını gösterir, Web uygulaması. Kolaylık olması için Bu öğretici, Facebook kimlik bilgileri ile çalışma hakkında odaklanır.
> 
> Bir ASP.NET MVC 5 web uygulaması Dış kimlik bilgilerini kullanmak için bkz: [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturmak](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Milyonlarca kullanıcıya bu dış sağlayıcıları hesaplarıyla olduğundan bu kimlik bilgileri, web sitelerindeki etkinleştirilmesi önemli bir avantajı sağlar. Bu kullanıcılar oluşturun ve yeni bir kimlik bilgileri kümesi unutmayın gerek yoktur, siteniz için kaydolmanız daha eilimli olabilir. Ayrıca, bir kullanıcı bu sağlayıcılardan biri oturum açtıktan sonra sağlayıcının sosyal işlemlerinden dahil edebilirsiniz.


## <a name="what-youll-build"></a>Yapı

Bu öğreticide iki ana amacı vardır:

1. Bir kullanıcının bir OAuth sağlayıcısından kimlik bilgilerinizle oturum etkinleştirin.
2. Hesap bilgileri sağlayıcıdan almak ve bu bilgileri siteniz için hesap kayıt ile tümleştirebilirsiniz.

Facebook kimlik doğrulama sağlayıcısı olarak kullanarak bu öğreticinin örneklerde odaklanmak rağmen sağlayıcılardan herhangi birinin kullanmak için kodu değiştirebilirsiniz. Herhangi bir sağlayıcıyı uygulamaya yönelik adımlar, bu öğreticide görürsünüz adımları çok benzer. Yalnızca ayarlamak doğrudan sağlayıcının API çağrıları yapılırken önemli farklılıklar fark edeceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) veya [Web için Microsoft Visual Studio Express 2012](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Veya

- Microsoft Visual Studio 2010 SP1 veya [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Ayrıca, bu konu, ASP.NET MVC ve Visual Studio hakkında temel bilgilere sahip varsayar. ASP.NET MVC 4 giriş gerekirse bkz [ASP.NET MVC 4 giriş](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'da yeni bir ASP.NET MVC 4 Web uygulaması oluşturun ve adlandırın &quot;OAuthMVC&quot;. .NET Framework 4.5 veya 4 hedefleyebilirsiniz.

![Proje oluşturma](using-oauth-providers-with-mvc/_static/image1.png)

Yeni ASP.NET MVC 4 Proje penceresinde seçin **Internet uygulama** bırakıp **Razor** görüntüleme altyapısı olarak.

![Internet uygulamasını seçin](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Bir sağlayıcı etkinleştir

Internet uygulaması şablonu kullanarak bir MVC 4 web uygulaması oluşturduğunuzda, Proje uygulama AuthConfig.cs adlı bir dosya oluşturulur\_başlangıç klasörü.

![AuthConfig dosyası](using-oauth-providers-with-mvc/_static/image3.png)

AuthConfig dosya Dış kimlik doğrulama sağlayıcıları için istemcilerini kaydetmek için kod içerir. Dış sağlayıcılardan hiçbiri etkin şekilde varsayılan olarak, bu kod, kılınmıştır.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Dış kimlik doğrulama istemcisi kullanmak için bu kodu açıklama durumundan çıkarmanız gerekir. Yalnızca, sitenize dahil etmek istediğiniz sağlayıcıları açıklamadan çıkarın. Bu öğretici için Facebook kimlik bilgileri yalnızca olanak sağlar.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Kayıt parametreleri için boş dizeler includes yöntemi yukarıdaki örnekte dikkat edin. Uygulamayı şimdi çalıştırmayı denerseniz, uygulama için parametreler boş dizeler izin verilmediğinden bir bağımsız değişken özel durum oluşturur. Geçerli değerler sağlamak için web sitenizi dış sağlayıcılarıyla sonraki bölümde gösterildiği gibi kaydetmeniz gerekir.

## <a name="registering-with-an-external-provider"></a>Dış sağlayıcı ile kaydetme

Dış sağlayıcıdan kimlik bilgilerine sahip kullanıcıların kimliğini doğrulamak için web sitenizi sağlayıcı ile kaydetmeniz gerekir. Sitenizi kaydederken istemci kaydedilirken dahil etmek için parametreler (örneğin, anahtar veya kimliğini ve parolasını) alırsınız. Kullanmak istediğiniz sağlayıcıları ile bir hesabınızın olması gerekir.

Bu öğretici, tüm bu sağlayıcıları ile kaydetmek için gerçekleştirmeniz gereken adımlar göstermez. Adımlar genellikle zor değildir. Siteniz başarıyla kaydetmek için bu sitelerdeki sağlanan yönergeleri izleyin. Sitenizi kaydetme ile çalışmaya başlamak için geliştirici site için bkz:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Sitenizi Facebook ile kaydedilirken sağlanabilen &quot;localhost&quot; site, etki alanı için ve `&quot;http://localhost/&quot;` aşağıdaki resimde gösterildiği gibi URL. Localhost kullanarak çoğu sağlayıcıları ile çalışır, ancak şu anda Microsoft sağlayıcı ile çalışmaz. Microsoft sağlayıcısı için geçerli bir web sitesi URL'si eklemeniz gerekir.

![Site Kaydet](using-oauth-providers-with-mvc/_static/image4.png)

Önceki görüntüde uygulama kimliği, uygulama gizli anahtarı ve ilgili kişi e-posta değerlerini kaldırılmıştır. Sitenizi gerçekte kaydettiğinizde, bu değerleri mevcut olacaktır. Uygulamanıza ekleyeceksiniz çünkü uygulama kimliği ve uygulama gizli anahtarı değerlerini not isteyeceksiniz.

## <a name="creating-test-users"></a>Test kullanıcıları oluşturma

Sitenizi test etmek için var olan bir Facebook hesabını kullanarak olmayacaksa bu bölümü atlayabilirsiniz.

Facebook Uygulama Yönetimi sayfasında uygulamanız için test kullanıcıları kolayca oluşturabilirsiniz. Bunları test sitenize oturum açmak için hesaplar. Tıklayarak test kullanıcıları Oluştur **rolleri** sol gezinti bölmesinde ve tıklayarak bağlantıyı **oluşturma** bağlantı.

![Test kullanıcıları oluştur](using-oauth-providers-with-mvc/_static/image5.png)

Facebook site sayısı, istek test hesapları, otomatik olarak oluşturur.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Uygulama kimliğini ve parolasını sağlayıcıdan ekleme

Facebook kimliği ve parolası aldığınız, AuthConfig dosyasına geri dönün ve parametre değerlerini ekleyin. Aşağıda gösterilen değerleri gerçek değerleri değildir.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Dış kimlik bilgileriyle oturum açın

Tüm sitenizdeki Dış kimlik bilgilerini sağlamak için yapmanız gereken budur. Uygulamanızı çalıştırın ve sağ üst köşesindeki oturum açma bağlantıya tıklayın. Şablonu, Facebook sağlayıcısı olarak kayıtlı ve sağlayıcısı için bir düğmeyi içeren otomatik olarak tanır. Birden çok sağlayıcı kaydolursanız, her biri için bir düğme otomatik olarak dahil edilir.

![Dış oturum açma](using-oauth-providers-with-mvc/_static/image6.png)

Bu öğretici, dış sağlayıcıları için düğmeler günlüğüne özelleştirmek nasıl ele alınmamaktadır. Bu bilgi için bkz: [OAuth/Openıd kullanırken oturum açma kullanıcı Arabirimi özelleştirme](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Facebook kimlik bilgileriyle oturum Facebook düğmesine tıklayın. Dış sağlayıcılardan biri seçtiğinizde, o siteye yeniden yönlendirilen ve oturum açmak için bu hizmet tarafından istenir.

Aşağıdaki resimde, Facebook için oturum açma ekranı gösterir. Bunu, Facebook hesabınızı oauthmvcexample adlı bir sitede oturum açmak için kullandığınız not alır.

![facebook kimlik doğrulaması](using-oauth-providers-with-mvc/_static/image7.png)

Facebook kimlik bilgilerinizle oturum açtıktan sonra bir sayfa sitenin temel bilgilere erişimi olduğunu kullanıcıya bildirir.

![İstek izni](using-oauth-providers-with-mvc/_static/image8.png)

Seçtikten sonra **uygulamasına gidin**, kullanıcı site için kaydetmeniz gerekir. Facebook kimlik bilgileriyle bir kullanıcı oturum sonra aşağıdaki görüntüde kayıt sayfası gösterilir. Genellikle önceden doldurulmuş sağlayıcıdan bir ada sahip kullanıcı adıdır.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Tıklatın **kaydetmek** kayıt işlemini tamamlamak için. Tarayıcıyı kapatın.

Yeni hesap, veritabanına eklenmiş olduğunu görebilirsiniz. Server Explorer'da açın **DefaultConnection** veritabanı ve açık **tabloları** klasör.

![veritabanı tabloları](using-oauth-providers-with-mvc/_static/image10.png)

Sağ **USERPROFILE** tablo ve seçin **Show Table Data**.

![Verileri göster](using-oauth-providers-with-mvc/_static/image11.png)

Eklediğiniz yeni hesap görürsünüz. Verileri bakabilir **Web sayfası\_OAuthMembership** tablo. Yeni eklediğiniz hesabı için dış sağlayıcı ile ilgili daha fazla veri görürsünüz.

Dış kimlik doğrulamasını etkinleştirmek istiyorsanız, yapılır. Ancak, daha fazla bilgi sağlayıcısından yeni kullanıcı kayıt işlemine aşağıdaki bölümlerde gösterildiği gibi tümleştirebilirsiniz.

## <a name="create-models-for-additional-user-information"></a>Ek kullanıcı bilgilerini için model oluşturma

Önceki bölümlerde fark olarak çalışmak yerleşik hesap kaydı için herhangi bir ek bilgi almak gerekmez. Ancak, en dış sağlayıcıları kullanıcı hakkındaki geri ek bilgileri geçirin. Aşağıdaki bölümlerde, bu bilgileri korumak ve veritabanına kaydetme gösterilmektedir. Özellikle, kullanıcının tam adını, kullanıcının kişisel web sayfası URI değerleri ve Facebook hesap doğruladı olup olmadığını belirten bir değer saklar.

Kullanacağınız [Code First Migrations](https://msdn.microsoft.com/data/jj591621) ek kullanıcı bilgilerini depolamak için bir tablo eklemek için. Geçerli veritabanının anlık görüntüsünü oluşturmak öncelikle gerekir, tablo mevcut bir veritabanına eklersiniz. Geçerli veritabanının anlık oluşturarak, yalnızca yeni bir tablo içeren bir geçiş sonrası oluşturabilirsiniz. Geçerli veritabanının anlık görüntüsünü oluşturmak için:

1. Açık **Paket Yöneticisi Konsolu**
2. Komutu çalıştırın **enable-migrations**
3. Komutu çalıştırın **add-migration ilk – IgnoreChanges**
4. Komutu çalıştırın **güncelleştirme veritabanı**

Şimdi, yeni özellikler ekleyeceksiniz. Modeller klasörü AccountModels.cs dosyasını açın ve RegisterExternalLoginModel sınıfı bulunamadı. RegisterExternalLoginModel sınıf, kimlik doğrulama sağlayıcısı'ndan döndürülmesini değerleri içerir. Aşağıda vurgulandığı gibi FullName ve bağlantı adlı özellikleri ekleyin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Ayrıca AccountModels.cs içinde ExtraUserInformation adlı yeni bir sınıf ekleyin. Bu sınıf, veritabanında oluşturulan yeni tablo temsil eder.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

UsersContext sınıfında yeni sınıf için bir DbSet özellik oluşturmak için aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Şimdi yeni bir tablo oluşturmak hazır olursunuz. Paket Yöneticisi konsolu yeniden ve bu süre açın:

1. Komutu çalıştırın **add-migration AddExtraUserInformation**
2. Komutu çalıştırın **güncelleştirme veritabanı**

Yeni tablo artık veritabanında yok.

## <a name="retrieve-the-additional-data"></a>Ek veri alma

Ek kullanıcı verilerini almak için iki yolu vardır. Varsayılan olarak, kimlik doğrulama isteği sırasında geri geçirilen kullanıcı verilerini korumak için ilk yoludur bakın. İkinci sağlayıcısı API çağrısı ve daha fazla bilgi istemek için özellikle yoludur. FullName ve bağlantı için değerleri otomatik olarak geri Facebook tarafından geçirilir. Facebook hesabına doğruladı olup olmadığını belirten bir değer, Facebook API'si çağrısıyla alınır. İlk olarak, FullName ve bağlantı için değerleri doldurmak ve ardından daha sonra doğrulanmış değeri alırsınız.

Ek kullanıcı verilerini almak için açık **AccountController.cs** dosyasını **denetleyicileri** klasör.

Bu dosya için günlüğe kaydetme, kaydetme ve hesaplarını yönetme mantığını içerir. Özellikle, çağrılan yöntemler fark **ExternalLoginCallback** ve **ExternalLoginConfirmation**. Bu yöntemler içinde dış oturum açma işlemleri, uygulamanız için özelleştirmek için kod ekleyebilirsiniz. İlk satırı **ExternalLoginCallback** yöntem içerir:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Ek kullanıcı verilerini geri geçirilir **ExtraData** özelliği **yazılımdan AuthenticationResult** sunucudan döndürülen nesne **VerifyAuthentication** yöntemi. Facebook istemcisini aşağıdaki değerleri içeren **ExtraData** özelliği:

- kimlik
- name
- Bağlantı
- Cinsiyeti
- accesstoken

Diğer sağlayıcıları ExtraData özelliğinde benzer ancak biraz daha farklı verilere sahip olur.

Kullanıcı sitenize yeni ise, bazı ek verileri alabilir ve bu verileri onay görünüme iletmek. Yalnızca kullanıcı sitenize yeni ise son yöntemindeki kod bloğunu çalıştırılır. Aşağıdaki satırı değiştirin:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Bu satırla:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Bu değişikliğin yalnızca FullName ve bağlantı özelliklerine ilişkin değerleri içerir.

İçinde **ExternalLoginConfirmation** yöntemi, ek kullanıcı bilgileri kaydetmek için aşağıdaki kodu vurgulandığı gibi değiştirin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Görünüm ayarlama

Sağlayıcıdan almak ek kullanıcı verilerini kayıt sayfası görüntülenir.

İçinde **görünümleri**/**hesap** klasörü, açık **ExternalLoginConfirmation.cshtml**. Kullanıcı adı için var olan alanın FullName, bağlantı ve PictureLink alanları ekleyin.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Şimdi uygulamayı çalıştırın ve kaydedilmiş ek bilgiler içeren yeni bir kullanıcı kaydetmek neredeyse hazırsınız demektir. Site ile kayıtlı olmayan bir hesabınızın olması gerekir. Farklı bir test hesabınız veya satırları silme **USERPROFILE** ve **Web sayfalarını\_OAuthMembership** tablolar yeniden kullanmak istediğiniz hesap için. Bu satırları silme tarafından hesabın yeniden kayıtlı olduğunu güvence altına alır.

Uygulamayı çalıştırın ve yeni kullanıcı kaydedin. Bu süre daha fazla değer onay sayfasında içerdiğine dikkat edin.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Kayıt işlemini tamamladıktan sonra Tarayıcıyı kapatın. Yeni değerleri fark veritabanına bakın **ExtraUserInformation** tablo.

## <a name="install-nuget-package-for-facebook-api"></a>Facebook API'si için NuGet paket yüklemesi

Facebook sağlayan bir [API](https://developers.facebook.com/docs/reference/apis/) işlemleri gerçekleştirmek için çağırabilirsiniz. HTTP istekleri göndermek yönlendirerek tarafından ya da bu istekleri gönderme kolaylaştıran bir NuGet paketi yükleniyor kullanarak Facebook API'si çağırabilirsiniz. Bu öğreticide gösterilen bir NuGet paketi kullanarak, ancak NuGet yükleme paketi gerekli değil. Bu öğretici Facebook C# SDK paketin nasıl kullanılacağı gösterilir. Facebook API'si Çağırma ile yardımcı diğer NuGet paketleri vardır.

Gelen **NuGet paketlerini Yönet** windows, Facebook C# SDK paketi seçin.

![Paketi Yükle](using-oauth-providers-with-mvc/_static/image13.png)

Kullanıcı için erişim belirteci gerektiren bir işlem çağırmak için Facebook C# SDK'sını kullanır. Sonraki bölümde, bir erişim belirteci edinmek gösterilmektedir.

## <a name="retrieve-access-token"></a>Erişim belirteci alın

Kullanıcının kimlik bilgileri doğrulandıktan sonra en dış sağlayıcıları bir erişim belirteci geri geçirin. Sizin, yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilir olan işlemleri çağırmanıza olanak sağladığından, bu erişim belirteci çok önemlidir. Bu nedenle, daha fazla işlevsellik sağlamak istediğinizde erişim belirteci saklama ve alma gereklidir.

Dış sağlayıcıya bağlı olarak, erişim belirteci yalnızca bir sınırlı bir süre için geçerli olabilir. Geçerli erişim belirteci olmasını sağlamak için kaydeder ve bir oturum değeri olarak depolamak yerine kullanıcının veritabanına kaydetme her zaman alır.

İçinde **ExternalLoginCallback** yöntemi, erişim belirteci de geçirilir geri **ExtraData** özelliği **yazılımdan AuthenticationResult** nesnesi. Vurgulanmış kodu ekleyin **ExternalLoginCallback** erişim belirtecinde yer kaydetmek için **oturum** nesnesi. Kullanıcı bir Facebook hesabıyla oturum açtığı her sefer bu kodu çalıştırılır.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Bu örnek, Facebook bir erişim belirteci alır ancak adlı tuşuyla erişim belirteci tüm dış sağlayıcıdan alabilir &quot;accesstoken&quot;.

## <a name="logging-off"></a>Oturum kapatma

Varsayılan **kapatma** yöntemi, uygulamanızın dışında kullanıcı oturumu, ancak dış sağlayıcı dışında kullanıcı oturum açılamıyor. Facebook dışında kullanıcı oturum ve kullanıcı oturumu kapattıktan sonra kalıcı belirteç önlemek için aşağıdaki vurgulanmış kodu ekleyebilirsiniz **kapatma** AccountController yöntemi.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Sağladığınız içinde değeri `next` parametredir dışında Facebook kullanıcı oturumu sonra kullanılacak URL. Yerel bilgisayarınızda test edilirken yerel siteniz için doğru bağlantı noktası numarasını sağlamak. Üretimde contoso.com gibi bir varsayılan sayfa sağlamak.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Erişim belirteci gerektiren kullanıcı bilgilerini al

Erişim belirteci depolanır ve Facebook C# SDK'sı paketinin yüklü göre bunları birlikte Facebook ek kullanıcı bilgi istemek için kullanabilirsiniz. İçinde **ExternalLoginConfirmation** yöntemi, bir örneğini oluşturmak **FacebookClient** erişim belirteci değerini geçirerek sınıfı. Değeri, istek **doğrulandı** özelliği için geçerli, kimliği doğrulanmış kullanıcı. **Doğrulandı** özelliği, Facebook bir cep telefonu ileti gönderme gibi bazı başka bir yöntemle hesabıyla doğruladı olup olmadığını belirtir. Bu değer veritabanında kaydedin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Ayrıca, kullanıcı için veritabanındaki kayıtları silme veya farklı bir Facebook hesabıyla kullanmak yeniden gerekir.

Uygulamayı çalıştırın ve yeni kullanıcı kaydedin. Bakmak **ExtraUserInformation** doğrulandı özelliğinin değeri görmek için tablo.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, Facebook ile kullanıcı kimlik doğrulaması ve kayıt verileri için tümleşik bir site oluşturuldu. MVC 4 web uygulaması ve bu varsayılan davranışı özelleştirmek nasıl ayarlanır varsayılan davranış hakkında öğrendiniz.

## <a name="related-topics"></a>İlgili konular

- [Kimlik doğrulama ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
