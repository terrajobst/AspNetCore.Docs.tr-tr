---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: MVC 4 ile OAuth sağlayıcıları kullanma | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, kullanıcıların Facebo gibi bir dış sağlayıcı kimlik bilgileriyle oturum sağlayan bir ASP.NET MVC 4 web uygulaması oluşturma işlemini göstermektedir...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: d0203b62c911056fc56ed103c1c42f67816cbbf0
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021761"
---
<a name="using-oauth-providers-with-mvc-4"></a>MVC 4 ile OAuth Sağlayıcıları Kullanma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici, kullanıcıların Facebook, Twitter, Microsoft veya Google gibi bir dış sağlayıcı kimlik bilgileriyle oturum ve bu sağlayıcılardan işlevlerinden bazıları tümleştirmek sağlar bir ASP.NET MVC 4 web uygulamasının nasıl oluşturulacağını gösterir, Web uygulaması. Kolaylık olması için Bu öğreticide, Facebook kimlik bilgileriyle çalışmaya odaklanır.
> 
> Bir ASP.NET MVC 5 web uygulaması Dış kimlik bilgilerini kullanmak için bkz. [Facebook ve Google OAuth2 ve Openıd oturum açma ile ASP.NET MVC 5 uygulaması oluşturma](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Hesapları bu dış sağlayıcıları ile milyonlarca kullanıcıya zaten olduğundan, web sitelerinizi, bu kimlik bilgileri sağlayarak bu önemli bir avantaj sunar. Bu kullanıcılar, yeni bir dizi kimlik bilgisi oluşturma ve olmadığı siteniz için kaydolmanız daha eilimli olabilir. Ayrıca, bir kullanıcı bu sağlayıcılardan biri oturum açtıktan sonra sosyal operations sağlayıcısından birleştirebilirsiniz.


## <a name="what-youll-build"></a>Derleme

Bu öğreticide iki ana hedefi vardır:

1. OAuth sağlayıcısı kimlik bilgileriyle oturum açmasına etkinleştirin.
2. Hesap bilgileri sağlayıcıdan almak ve bu bilgileri sitenizin hesap kaydı ile tümleştirin.

Facebook kimlik doğrulama sağlayıcısı olarak kullanarak bu öğreticideki örneklerde odaklanmak olsa da, herhangi bir sağlayıcı kullanmak için kodu değiştirebilirsiniz. Herhangi bir sağlayıcı adımlarını çok benzer adımlarla Bu öğreticide görürsünüz. Yalnızca ayarlamak sağlayıcının API'sine doğrudan çağrı yaparken önemli farklılıkları da göreceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) veya [Web için Microsoft Visual Studio Express 2012](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Veya

- Microsoft Visual Studio 2010 SP1 veya [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Ayrıca, bu konuda, ASP.NET MVC ve Visual Studio hakkında temel bilgilere sahip olduğunuz varsayılır. ASP.NET MVC 4 için bir tanıtım gerekirse bkz [ASP.NET MVC 4'e giriş](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'da yeni bir ASP.NET MVC 4 Web uygulaması oluşturun ve adlandırın &quot;OAuthMVC&quot;. .NET Framework 4.5 ya da 4 hedefleyebilir.

![Proje oluşturma](using-oauth-providers-with-mvc/_static/image1.png)

Yeni ASP.NET MVC 4 Proje penceresinde **Internet uygulaması** bırakıp **Razor** görüntüleme altyapısı olarak.

![Internet uygulaması seçin](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Bir sağlayıcı etkinleştir

Internet uygulaması şablonu kullanarak bir MVC 4 web uygulaması oluşturduğunuzda, projeyi AuthConfig.cs uygulamasında adlı bir dosya oluşturulur\_başlangıç klasörü.

![AuthConfig dosyası](using-oauth-providers-with-mvc/_static/image3.png)

Dış kimlik doğrulama sağlayıcıları için istemcilerini kaydetmek için kod AuthConfig dosya içerir. Dış sağlayıcılar hiçbiri etkin şekilde varsayılan olarak, bu kod, kılınmıştır.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Dış kimlik doğrulama istemcisini kullanmak için bu kod açıklamalarını kaldırın gerekir. Sitenizdeki dahil etmek istediğiniz sağlayıcıları yalnızca açıklama durumundan çıkarın. Bu öğretici için yalnızca Facebook kimlik olanak sağlar.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Yukarıdaki örnekte yöntemi boş dizeler için kayıt parametreleri içerdiğini unutmayın. Şimdi uygulamayı çalıştırmayı denerseniz, uygulama için parametreler boş dizeler izin verilmediğinden bir bağımsız değişken özel durum oluşturur. Geçerli değerler sağlamak için web sitenizi dış sağlayıcılarıyla sonraki bölümde gösterildiği gibi kaydetmeniz gerekir.

## <a name="registering-with-an-external-provider"></a>Dış sağlayıcı ile kaydetme

Dış sağlayıcıdan kimlik bilgileriyle kullanıcıların kimliğini doğrulamak için web sitenizi ve sağlayıcı ile kaydetmeniz gerekir. Sitenizi kaydolduğunuzda, istemci kaydı sırasında dahil etmek için parametreler (örneğin, anahtar veya kimliği ve gizli dizi) alırsınız. Kullanmak istediğiniz sağlayıcılarıyla bir hesabınızın olması gerekir.

Bu öğretici, bu sağlayıcıları ile kaydetmek için gerçekleştirmeniz gereken adımlar göstermez. Adımları genellikle zor değildir. Sitenizi başarıyla kaydetmek için bu sitelerdeki sağlanan yönergeleri izleyin. Sitenizi kaydetme ile çalışmaya başlamak için geliştirici sitesi için bkz:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Sitenizi Facebook ile kaydederken sağlayabilir &quot;localhost&quot; site etki alanı için ve `&quot; http://localhost/&quot;` aşağıdaki resimde gösterildiği gibi URL. Localhost kullanarak çoğu sağlayıcılarıyla birlikte çalışarak, ancak şu anda Microsoft sağlayıcı ile çalışmaz. Microsoft sağlayıcısı için geçerli bir web sitesi URL'si eklemeniz gerekir.

![sitesini kaydedin](using-oauth-providers-with-mvc/_static/image4.png)

Önceki görüntüde uygulama kimliği, uygulama gizli anahtarı ve ilgili kişi e-posta için değerler kaldırıldı. Sitenizi gerçekten kaydettiğinizde, bu değerleri mevcut olacaktır. Uygulama kimliği ve uygulama gizli anahtarı değerlerini dikkat edin, uygulamanıza eklemek çünkü isteyeceksiniz.

## <a name="creating-test-users"></a>Test kullanıcılarını oluşturma

Sitenizi test etmek için var olan bir Facebook hesabı kullanarak olmayacaksa bu bölümü atlayabilirsiniz.

Test kullanıcıları, uygulamanıza Facebook uygulaması Yönetimi sayfasındaki kolayca oluşturabilirsiniz. Bunları test hesapları, sitenize oturum açmak için. Tıklayarak test kullanıcıları Oluştur **rolleri** sol gezinti bölmesinde, tıklayarak bağlantı **Oluştur** bağlantı.

![Test kullanıcıları oluştur](using-oauth-providers-with-mvc/_static/image5.png)

Facebook site sayısı, istek test hesapları otomatik olarak oluşturur.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Uygulama kimliğini ve parolasını sağlayıcıdan ekleme

Facebook'tan kimliğini ve parolasını aldığınız, AuthConfig dosyasına geri dönün ve bunları parametre değerlerini ekleyin. Aşağıda gösterilen değerleri, gerçek değerleri değildir.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Dış kimlik bilgilerinizle oturum açın

Tüm dış kimlik bilgilerini sitenizde etkinleştirmek için yapmanız gereken budur. Uygulamanızı çalıştırın ve sağ üst köşedeki oturum açma bağlantıya tıklayın. Şablonu, Facebook sağlayıcısı olarak kayıtlı ve sağlayıcı için bir düğme içeren otomatik olarak tanır. Birden çok sağlayıcı kaydolursanız, her biri için bir düğme otomatik olarak dahil edilir.

![Dış oturum açma](using-oauth-providers-with-mvc/_static/image6.png)

Bu öğretici için dış sağlayıcıları düğmeleri günlüğünde özelleştirme kapsamaz. Bu bilgi için bkz: [OAuth/Openıd kullanırken oturum açma kullanıcı Arabirimi özelleştirme](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Facebook kimlik bilgileriyle oturum için Facebook düğmesine tıklayın. Dış sağlayıcılardan birini seçtiğinizde, o siteye yeniden yönlendirilen ve oturum açmak için bu hizmet tarafından istenir.

Aşağıdaki görüntüde için Facebook oturum açma ekranı gösterilmektedir. Bunu, Facebook hesabınıza oauthmvcexample adlı bir siteye oturum açmak için kullandığınız not alır.

![facebook kimlik doğrulaması](using-oauth-providers-with-mvc/_static/image7.png)

Facebook kimlik bilgileriyle oturum açtıktan sonra bir sayfa, sitenin temel bilgileri erişimi olmasını kullanıcıya bildirir.

![İstek izni](using-oauth-providers-with-mvc/_static/image8.png)

Seçtikten sonra **uygulamasına gidin**, site için kullanıcı kaydetmeniz gerekir. Facebook kimlik bilgileriyle bir kullanıcı oturumu sonra kayıt sayfasına aşağıdaki resimde gösterilmektedir. Genellikle önceden doldurulmuş sağlayıcısından bir ada sahip kullanıcı adıdır.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Tıklayın **kaydetme** kaydı tamamlamak için. Tarayıcıyı kapatın.

Yeni hesap, veritabanına eklendiğini görebilirsiniz. Sunucu Gezgini'nde açın **DefaultConnection** açın ve veritabanı **tabloları** klasör.

![veritabanı tabloları](using-oauth-providers-with-mvc/_static/image10.png)

Sağ **USERPROFILE** tablosunu seçip **tablo verilerini Göster**.

![verileri göster](using-oauth-providers-with-mvc/_static/image11.png)

Eklediğiniz yeni hesap görürsünüz. Verileri bakın **Web sayfası\_OAuthMembership** tablo. Yeni eklediğiniz hesabı için dış sağlayıcı ile ilgili daha fazla veri görürsünüz.

Dış kimlik doğrulamasını etkinleştirmek istiyorsanız, gerçekleştirilir. Ancak, daha fazla bilgi sağlayıcısından yeni kullanıcı kayıt işlemine aşağıdaki bölümlerde gösterildiği gibi tümleştirebilirsiniz.

## <a name="create-models-for-additional-user-information"></a>Ek kullanıcı bilgileri için model oluşturma

Önceki bölümlerde fark olarak çalışmak yerleşik hesap kaydı için herhangi bir ek bilgi almak gerekmez. Ancak, kullanıcı ile ilgili geri ek bilgileri en dış sağlayıcıları geçirin. Aşağıdaki bölümlerde, bu bilgileri korumak ve veritabanına kaydetme işlemi gösterilmektedir. Özellikle, kullanıcının tam adı, kullanıcının kişisel web sayfası URI değerlerini ve Facebook hesabı doğruladı olup olmadığını gösteren bir değer saklar.

Kullanacağınız [Code First Migrations](https://msdn.microsoft.com/data/jj591621) ek kullanıcı bilgileri depolamak için bir tablo eklemek için. Geçerli veritabanı anlık görüntüsünü oluşturmak önce gerekir böylece var olan bir veritabanı için tablo ekliyoruz. Geçerli veritabanı anlık görüntüsünü oluşturarak, daha sonra yalnızca yeni bir tablo içeren bir geçiş oluşturabilirsiniz. Geçerli veritabanı anlık görüntüsünü oluşturmak için:

1. Açık **Paket Yöneticisi Konsolu**
2. Komutunu çalıştırın **geçişleri etkinleştir**
3. Komutunu çalıştırın **ekleme geçiş ilk – IgnoreChanges**
4. Komutunu çalıştırın **veritabanını güncelleştir**

Şimdi, yeni özellikler ekleyeceksiniz. Modeller klasörü AccountModels.cs dosyasını açın ve RegisterExternalLoginModel sınıfı bulun. Kimlik doğrulama sağlayıcısı'ndan döndürülmesini değerleri RegisterExternalLoginModel sınıfı içerir. Aşağıda vurgulandığı gibi adlandırılmış tam adı ve bağlantı özellikleri ekleyin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Ayrıca AccountModels.cs içinde ExtraUserInformation adlı yeni bir sınıf ekleyin. Bu sınıf, veritabanında oluşturulan yeni tablo temsil eder.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

UsersContext sınıfta olan DB özelliği için yeni bir sınıf oluşturmak için aşağıdaki vurgulanmış kodu ekleyin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Yeni bir tablo oluşturmak artık hazırsınız. Paket Yöneticisi konsolu yeniden ve bu süre açın:

1. Komutunu çalıştırın **ekleme geçiş AddExtraUserInformation**
2. Komutunu çalıştırın **veritabanını güncelleştir**

Yeni tablo artık veritabanında bulunmuyor.

## <a name="retrieve-the-additional-data"></a>Ek veri alma

Ek kullanıcı verilerini almak için iki yolu vardır. Varsayılan olarak, kimlik doğrulama isteği sırasında geri geçirilen kullanıcı verilerini korumak için ilk yoludur bakın. Özellikle sağlayıcısı API çağrısı ve daha fazla bilgi istemek için ikinci yoludur bakın. Tam adı ve bağlantı için değerleri otomatik olarak geri Facebook tarafından geçirilir. Facebook hesabı doğruladı olup olmadığını gösteren bir değer Facebook API'sine yapılan bir çağrıyla alınır. İlk olarak, tam adı ve bağlantı için değerler doldurulur ve ardından daha sonra doğrulanmış değeri alacaktır.

Ek kullanıcı verileri almak üzere açmak **AccountController.cs** dosyası **denetleyicileri** klasör.

Bu dosya, günlüğe kaydetme, kaydetme ve hesapları yönetmek için mantığı içerir. Özellikle, çağrılan yöntemler fark **ExternalLoginCallback** ve **ExternalLoginConfirmation**. Bu yöntemler içinde uygulamanız için dış oturum açma işlemleri özelleştirmek için kod ekleyebilirsiniz. İlk satırı **ExternalLoginCallback** yöntem içerir:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Ek kullanıcı verileri geri iletilir **ExtraData** özelliği **AuthenticationResult** öğesinden döndürülen nesne **VerifyAuthentication** yöntemi. Facebook istemcisini aşağıdaki değerleri içeren **ExtraData** özelliği:

- kimlik
- name
- Bağlantı
- cinsiyet
- accesstoken

Diğer sağlayıcılar ExtraData özelliğinde benzer ancak biraz farklı verilere sahip olur.

Kullanıcı sitenize yeni tanışıyorsanız, bazı ek verileri almak ve bu verileri onay görünüme iletmek. Yalnızca kullanıcı sitenize yeni, son blok yöntemindeki kod çalıştırılır. Şu satırı değiştirin:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Bu satırla:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Bu değişiklik, yalnızca tam adı ve bağlantı özelliklerinin değerlerini içerir.

İçinde **ExternalLoginConfirmation** yöntemi, ek kullanıcı bilgileri kaydetmek için aşağıdaki kodu vurgulanmış olarak değiştirin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Görünüm ayarlama

Sağlayıcıdan almak ek kullanıcı verileri kayıt sayfasında görüntülenir.

İçinde **görünümleri**/**hesabı** açık klasör **ExternalLoginConfirmation.cshtml**. Kullanıcı adı için var olan alanın tam adı, bağlantı ve PictureLink alanları ekleyin.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Şimdi uygulamayı çalıştırın ve ek bilgileri kaydedildi yeni bir kullanıcı kaydetmek neredeyse hazırsınız demektir. Site zaten kayıtlı değil bir hesabınızın olması gerekir. Farklı test hesabı kullanın veya satırları silmek **USERPROFILE** ve **Web sayfalarını\_OAuthMembership** tabloları yeniden kullanmak istediğiniz hesap için. Satırları silerek, hesabı yeniden kaydedildiğini sağlayacaktır.

Uygulamayı çalıştırın ve yeni kullanıcı kaydedin. Bu süre, onay sayfasından daha fazla değer içerdiğine dikkat edin.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Kayıt tamamlandıktan sonra Tarayıcıyı kapatın. Yeni değerlere dikkat veritabanına bakın **ExtraUserInformation** tablo.

## <a name="install-nuget-package-for-facebook-api"></a>Facebook API'si için NuGet paketini yükle

Facebook sağlayan bir [API](https://developers.facebook.com/docs/reference/apis/) işlemleri gerçekleştirmek için çağırabilirsiniz. Facebook API'si, HTTP istekleri göndermek yönlendiren veya bu istekleri gönderirken kolaylaştıran bir NuGet paketini yükleme kullanarak çağırabilirsiniz. Bu öğreticide bir NuGet paketi kullanarak gösterilir, ancak yükleme NuGet paketi gerekli değil. Bu öğreticide, Facebook C# SDK'sını paketin nasıl kullanılacağı gösterilmektedir. Facebook API'si Çağırma ile yardımcı olan diğer NuGet paketleri vardır.

Gelen **NuGet paketlerini Yönet** windows, Facebook C# SDK paketini seçin.

![Paketi Yükle](using-oauth-providers-with-mvc/_static/image13.png)

Kullanıcı için erişim belirtecini gerektiren bir işlem çağırmak için Facebook C# SDK'sını kullanır. Sonraki bölümde, erişim belirteci almak gösterilmektedir.

## <a name="retrieve-access-token"></a>Erişim belirtecini alma

Kullanıcının kimlik bilgileri doğrulandıktan sonra en dış sağlayıcıları bir erişim belirteci yeniden geçirin. Bu erişim belirteci, yalnızca kimliği doğrulanmış kullanıcılara kullanılabilen işlemleri çağırmak sağladığından çok önemlidir. Bu nedenle, daha fazla işlevsellik sağlamak istediğinizde almaya ve depolamaya erişim belirteci gereklidir.

Harici sağlayıcıya bağlı olarak, erişim belirteci yalnızca sınırlı bir süre için geçerli olabilir. Geçerli bir erişim belirteciyle sahip olmasını sağlamak için kaydeder ve bir oturumu değeri olarak depolamak yerine kullanıcının veritabanına kaydetme her zaman alır.

İçinde **ExternalLoginCallback** yöntemi, erişim belirtecini da geçirilir geri **ExtraData** özelliği **AuthenticationResult** nesne. Vurgulanmış kodu ekleyin **ExternalLoginCallback** erişim belirtecinde kaydetmek için **oturumu** nesne. Bu kod, kullanıcı bir Facebook hesabıyla oturum açtığı her sefer çalıştırılır.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Bu örnek, bir erişim belirteci Facebook'tan alır ancak erişim belirtecini herhangi bir dış sağlayıcısından adlı tuşuyla alabilirsiniz &quot;accesstoken&quot;.

## <a name="logging-off"></a>Oturum kapatma

Varsayılan **kapatma** yöntemi dışında uygulamanızın kullanıcı oturumu, ancak dış sağlayıcı tarafından kullanıcının günlüğe kaydetmez. Facebook dışında kullanıcı oturum ve kullanıcı oturumu kapattıktan sonra kalıcı belirteç önlemek için aşağıdaki vurgulanmış kodu ekleyebilirsiniz **kapatma** AccountController yöntemi.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

İçinde sağladığınız değerin `next` dışında Facebook kullanıcı oturumu sonra kullanılacak URL parametredir. Yerel bilgisayarınızda test ederken, doğru bağlantı noktası numarası için yerel sitenize sağlar. Üretim ortamında, contoso.com gibi varsayılan bir sayfası sunar.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Erişim belirteci gerektiren kullanıcı bilgilerini alma

Erişim belirteci depolanan ve Facebook C# SDK'sı paketinin yüklü olduğunu, bunları birlikte ek kullanıcı bilgileri Facebook'tan istemek için kullanabilirsiniz. İçinde **ExternalLoginConfirmation** yöntemi, bir örneğini oluşturmak **FacebookClient** erişim belirteci değerini geçirerek sınıf. Değeri istek **doğrulandı** geçerli, kimliği doğrulanmış kullanıcı için özellik. **Doğrulandı** özelliği, Facebook hesabıyla bir cep telefonuna bir ileti gönderme gibi bazı diğer araçlar, doğrulanmış olup olmadığını gösterir. Bu değer, veritabanında kaydedin.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Yeniden kullanıcı veritabanındaki kayıtları silme veya farklı bir Facebook hesabı kullanmanız gerekir.

Uygulamayı çalıştırın ve yeni kullanıcı kaydedin. Bakmak **ExtraUserInformation** doğrulandı özelliğinin değeri görmek için tabloyu.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, kullanıcı kimlik doğrulaması ve kayıt verisi için Facebook ile tümleşik bir site oluşturulur. MVC 4 web uygulaması ve bu varsayılan davranışı özelleştirme için ayarlanmış varsayılan davranışı hakkında bilgi edindiniz.

## <a name="related-topics"></a>İlgili konular

- [Kimlik doğrulaması ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
