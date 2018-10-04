---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Azure Active Directory ile ASP.NET uygulamaları geliştirme | Microsoft Docs
author: Rick-Anderson
description: Azure Active Directory için Microsoft ASP.NET araçları kolaylaştırır Azure üzerinde barındırılan web uygulamaları için kimlik doğrulamasını etkinleştirin. Azure kimlik Doğr kullanabileceğiniz...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 96a6b8d1c258e742907b649b631f8a6c2c4bbf98
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577476"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Azure Active Directory ile ASP.NET uygulamaları geliştirme
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

Microsoft ASP.NET araçları etkinleştirme kimlik doğrulaması barındırılan web uygulamaları için Azure Active Directory kolaylaştırır [Azure](https://www.windowsazure.com/home/features/web-sites/). Kuruluşunuz, şirket içi Active Directory'nizden eşitlenen Kurumsal hesaplara veya kendi özel bir Azure Active Directory etki alanında oluşturulan kullanıcılar Office 365 kullanıcıların kimliğini doğrulamak için Azure kimlik doğrulaması'nı kullanabilirsiniz. Windows Azure kimlik doğrulamasını etkinleştirme, tek bir kullanan kullanıcıların kimliğini doğrulamak için uygulamanıza yapılandırır [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Kiracı.

Bu öğreticide ile oturum açma için yapılandırılmış bir ASP.NET uygulaması oluşturma işlemini gösterilecek [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Ayrıca, şu anda oturum açmış kullanıcı hakkında bilgi almak için Graph API'nin nasıl çağrılacağını ve uygulamayı azure'a dağıtmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

1. [Web için Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) veya [Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 veya üstü gereklidir.
3. Bir Azure hesabı. [Buraya](https://azure.microsoft.com/pricing/free-trial/) bir hesap zaten yoksa ücretsiz bir deneme sürümü için.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Genel yönetici, Active Directory'ye ekleme

1. Oturum [Azure Yönetim Portalı](https://manage.windowsazure.com/).
2. Tüm Azure hesaplarını içeren bir **varsayılan dizin** --tıklayın, ardından tıklatın **kullanıcılar** sayfanın üst kısmındaki sekme (aşağıdaki resme bakın).
3. Kullanıcı Ekle seçeneğine tıklayın.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. İle yeni bir kullanıcı oluşturmak **genel yönetici** rol. Tıklayın **kullanıcılar** üst menü ve ardından **Kullanıcı Ekle** komut çubuğunda düğme.
5. İçinde **Kullanıcı Ekle** iletişim kutusunda, yeni kullanıcı için bir ad girin ve ardından sağ oka tıklayın.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Kullanıcı adı girin ve rol kümesine **genel yönetici**. Genel Yöneticiler, parola kurtarma amacıyla bir alternatif e-posta adresi gerektirir. Bitirdikten sonra sağ oka tıklayın.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. İletişim kutusunun bir sonraki sayfada **Oluştur**. Geçici bir parola yeni kullanıcı için oluşturulan ve iletişim kutusunda görüntülenir.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Parola kaydetmeye ilk oturum açtığında sonra parolayı değiştirmek için gerekli olacaktır. Aşağıdaki görüntüde, yeni yönetici hesabı gösterilmektedir. Ayrıca bu sayfada gösterilen Microsoft hesabı değil, uygulama oturum açmak için Azure Active Directory kullanmanız gerekir.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>ASP.NET uygulaması oluşturma

Aşağıdaki adımları kullanın [Visual Studio Express 2013 Web](https://www.microsoft.com/download/details.aspx?id=40747)ve gerektiren [Visual Studio 2013 güncelleştirme 3'ü](https://www.microsoft.com/download/details.aspx?id=43721).

1. Visual Studio'da **dosya** ardından **yeni proje**. Üzerinde **yeni proje** iletişim kutusunda, seçin Visual C# Web projesinde sol menüden ve **Tamam**. İşaretini de isteyebilirsiniz **projeye Application Insights Ekle** uygulamanızın işlevselliğini istemiyorsanız.
2. İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC**ve ardından **kimlik doğrulamayı Değiştir**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Üzerinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **Kurumsal hesaplar**. Bu seçeneklerin yanı sıra otomatik olarak uygulamanızı Azure AD'ye kaydetme, Azure AD ile tümleştirmek için uygulamanızı otomatik olarak yapılandırmak için kullanılabilir. Kullanmak zorunda değilsiniz **kimlik doğrulamayı Değiştir** kaydetme ve uygulamanızı, ancak bu yapılandırma için iletişim kutusu sağlar, çok daha kolay. Örneğin Visual Studio 2012 kullanıyorsanız, yine de el ile Azure Yönetim Portalı'nda uygulamayı kaydetme ve Azure AD ile tümleştirmek için yapılandırmasını güncelleştirin.  
   Açılan menülerde seçin **bulut - tek kurum** ve **çoklu oturum açma, dizin verilerini okuma**. Azure AD dizininizde, örneğin (aşağıdaki görüntülerin) için etki alanını girin *aricka0yahoo.onmicrosoft.com*ve ardından **Tamam**. Etki alanı adını azure portalında varsayılan dizin için etki alanları sekmesinden alabilirsiniz (sıradaki resimde, aşağıya bakın).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   Aşağıdaki görüntüde, etki alanı adını Azure portalından gösterilmektedir.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Uygulama Kimliği URI'si, Azure AD'de tıklayarak kaydedilecek isteğe bağlı olarak yapılandırabileceğiniz **diğer seçenekler**. Uygulama Kimliği URI'si Azure AD'de kayıtlı olan ve Azure AD ile iletişim kurarken kendisini tanımlamak için uygulama tarafından kullanılan bir uygulama için benzersiz tanımlayıcısıdır. Uygulama Kimliği URI'si ve diğer özellikleri kayıtlı uygulamalar hakkında daha fazla bilgi için bkz. [bu konuda](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Uygulama Kimliği URI'si alanının altında onay kutusuna tıklayarak, ayrıca var olan bir kaydı aynı uygulama kimliği URI'si kullanan Azure AD'de üzerine seçebilirsiniz.
4. ' I tıklattıktan sonra **Tamam**, bir oturum açma iletişim kutusu görünür ve bir genel yönetici hesabını (aboneliğinizle ilişkili Microsoft hesabı değil) kullanarak oturum açmanız gerekir. Daha önce oluşturduğunuz yeni bir yönetici hesabı, parolayı değiştirin ve ardından yeni bir parola kullanarak tekrar oturum gerekecek.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Başarıyla kimlik doğrulaması yaptınız sonra **yeni ASP.NET projesi** iletişim, kimlik doğrulaması seçeneği gösterilir (**kuruluş** ) ve yeni uygulama burada olacaktır dizini (kayıtlı*aricka0yahoo.onmicrosoft.com* aşağıdaki görüntüde). Bu bilgiler etiketli onay kutusunu işaretleyin **bulutta Barındır**. Bu onay kutusunu seçtiyseniz, proje bir Azure web uygulaması olarak sağlanır ve daha sonra kolayca yayımlamak için etkin olacaktır. **Tamam**'ı tıklatın.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Azure Web sitesini yapılandırın** iletişim kutusu görünür ve bir site otomatik olarak oluşturulan adı ve bölge kullanma. Ayrıca, içine iletişim kutusunda oturum açmış olduğunuz hesap unutmayın. Bu hesap bir Azure aboneliğiniz için bağlı olduğundan emin olmak istediğiniz genellikle bir Microsoft hesabı.

    > [!NOTE]
    > Bu proje bir veritabanı gerektirir. Mevcut veritabanlarınızın seçin veya yeni bir tane oluşturmak için ihtiyacınız. Bir veritabanı projesi az miktarda bir kimlik doğrulaması yapılandırma verilerini depolamak için bir yerel veritabanı dosyası zaten kullandığı için gereklidir. Uygulamayı Azure Web Sitesi'ne dağıttığınızda, bu veritabanı bulutta erişilebilir olan birini seçin gerek dağıtımla paketlenmiş değil. **Tamam**'ı tıklatın.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Proje oluşturulur ve kimlik doğrulama seçenekleri ve web uygulama seçenekleri projeyle otomatik olarak yapılandırılır. Bu işlem tamamlandıktan sonra yerel olarak tuşuna basarak projeyi çalıştırın **^ F5**. Kuruluş hesabınızı kullanarak oturum açmanız gerekir. Daha önce oluşturduğunuz hesap için kullanıcı adı ve parola sağlayın ve tıklayın **oturum**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Başarılı oturum açma işleminden sonra sayfanın sağ üst köşesindeki kullanıcı adını görüntüleyerek kimliğinin ASP.NET sitesi gösterilir.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Hatası alırsanız:  
   Değer null veya boş olamaz. Parametre adı: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   bkz: [hata ayıklama](#dbg) öğreticinin sonunda bölüm.

## <a name="basics-of-the-graph-api"></a>Graph API ile ilgili temel bilgiler

[Graph API'sini](https://msdn.microsoft.com/library/azure/hh974476.aspx) Azure AD dizininizde CRUD ve nesneler üzerinde başka işlemler gerçekleştirmek için kullanılan programlama arabirimi. Visual Studio 2013'te yeni bir proje oluştururken, kimlik doğrulaması için bir kuruluş hesabı seçeneğini belirlerseniz, uygulamanız zaten Graph API'sini çağırmak için yapılandırılır. Bu bölümde kısaca, Graph API'sini nasıl çalıştığı gösterilmektedir.

1. En üstünde oturum açmış kullanıcı adına çalışan uygulamanızda tıklatın doğru sayfanın. Bu eylemi giriş denetleyicisine kullanıcı profili sayfasına yönlendirilirsiniz. Tablo yönetici hesabı hakkında daha önce oluşturduğunuz kullanıcı bilgilerini içerdiğini fark edersiniz. Bu bilgiler, dizinde depolanır ve Graph API'si sayfa yüklendiğinde bu bilgileri almak için çağrılır.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Visual Studio'ya geri dönün ve genişletin **denetleyicileri** klasörünü ve ardından açık **HomeController.cs** dosya. Gördüğünüz bir **UserProfile()** içeren bir belirteç almak ve sonra Graph API'sini çağırmak için kod eylemi. Bu kod, aşağıda yineleniyor: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Graph API'sini çağırmak için önce bir belirteç almak gerekir. Belirteç alındığında, dize değeri tüm istekler için Graph API için yetkilendirme üst bilgisinde eklenmesi gereken. Yukarıdaki kodu çoğunu bir belirteç almak üzere Azure AD kimlik doğrulaması, Graph API'sine çağrı yapmak için belirteci kullanarak ve böylece Görünümü'nde sunulan yanıt sonra dönüştürme işlemlerinin ayrıntılarını ele alır.

    En ilgili tartışma için aşağıdaki vurgulanan satırın bölümüdür: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Bu satırı görünümde sunulur ve JSON yanıtı seri durumdan kullanıcının adını temsil eder.

    HttpClient kullanan Graph API'sini çağırmak ve kendiniz ham verileri işlemek, ancak daha kolay bir yolu kullanmaktır [Graph istemci kitaplığı NuGet aracılığıyla kullanılabilir olan](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). İstemci Kitaplığı, ham HTTP isteklerini ve döndürülen veri dönüşümü, işler ve .NET ortamını Graph API'si ile çalışmak çok daha kolay hale getirir. İlgili Graph API kod örnekleri görmek [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Uygulamayı azure'a dağıtma

Aşağıdaki adımlar, uygulamayı azure'a dağıtma gösterilmektedir. Yalnızca birkaç adımda yayımlanmaya hazır olacak şekilde önceki adımlarda, yeni projenizin, azure'da bir web uygulaması ile bağlı.

1. Visual Studio'da projeye sağ tıklatın **Yayımla**. **Web'i Yayımla** her bir ayar zaten yapılandırılmış iletişim kutusu görüntülenir. Tıklayarak **sonraki** düğmesine gitmek için **ayarları** sayfası. Kimlik doğrulaması istenebilir; Azure abonelik hesabınızı (genellikle bir Microsoft hesabı) ve daha önce oluşturduğunuz Kurumsal hesap kullanarak kimlik doğrulaması emin olun.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Denetleme **kuruluş kimlik doğrulamasını etkinleştir** seçeneği. İçinde **etki alanı** dizininiz için etki alanı girin. Gelen **erişim düzeyi** açılan listesinde, select **çoklu oturum açma, dizin verilerini okuma**. İçinde kullandığınız önceki veritabanı zaten doldurulmuş fark edeceksiniz **veritabanları** bölümü. Tıklayın **yayımlama**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio Web dağıtımı başlar ve ardından yeni bir tarayıcı penceresi görünür. Dizininiz için yeniden kimlik doğrulaması istenebilir. Kimlik doğrulaması yaptınız sonra azure'da yeni yayımlanan Web sitenize yönlendirilirsiniz.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Uygulamayı hata ayıklama

Şu hatayı alırsanız:   
 Değer null veya boş olamaz. Parametre adı: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Değiştirin *görünümler/paylaşılan\\_LoginPartial.cshtml* aşağıdaki dosya:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Uygulamayı çalıştırdıktan sonra oturum açmış kullanıcı, "Null kullanıcı" gösteriyorsa, oturumu kapatın ve yeniden daha önce oluşturduğunuz Active Directory hesabıyla oturum açın.

Rick Rainey'nın izlemek için mükemmel bir öğretici olduğu [yakından bakış: Azure Web siteleri ve Azure AD kullanarak bir Kurumsal kimlik doğrulaması](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Daha fazla bilgi

- [Yakından bakış: Azure Web siteleri ve Azure AD kullanarak bir Kurumsal kimlik doğrulaması](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Azure AD Graph API'sine genel bakış](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD'de kimlik doğrulama senaryoları](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Github'da Azure AD kod örnekleri](https://github.com/AzureADSamples)
