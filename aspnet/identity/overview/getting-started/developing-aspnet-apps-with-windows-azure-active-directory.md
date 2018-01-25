---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: "Azure Active Directory ile ASP.NET uygulama geliştirme | Microsoft Docs"
author: Rick-Anderson
description: "Azure Active Directory için Microsoft ASP.NET araçları, kolaylaştırır Azure üzerinde barındırılan web uygulamaları için kimlik doğrulamasını etkinleştirin. Azure Authenti kullanabilirsiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 1ef0468d5f5c17480b23ac88983f30fe6f4979c0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Azure Active Directory ile ASP.NET uygulama geliştirme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET araçları Azure Active Directory üzerinde barındırılan web uygulamaları için kimlik doğrulamasını etkinleştirmek basit yapar için [Azure](https://www.windowsazure.com/home/features/web-sites/). Kuruluşunuz, şirket içi Active Directory'nizden eşitlenen Kurumsal hesaplara veya kendi özel Azure Active Directory etki alanında oluşturulan kullanıcıların Office 365 kullanıcıların kimliğini doğrulamak için Azure kimlik doğrulaması kullanabilirsiniz. Windows Azure kimlik doğrulamasını etkinleştirme yapılandırır tek bir kullanarak kullanıcıların kimliklerini doğrulamak için uygulamanızın [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Kiracı.
> 
>  Bu öğretici Rick Anderson tarafından yazıldı[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


Bu öğretici ile oturum açma için yapılandırılmış bir ASP.NET uygulamasının nasıl oluşturulacağını gösterir [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Ayrıca, şu anda oturum açmış kullanıcı hakkında bilgi almak için grafik API'sini çağırmak nasıl ve uygulamayı Azure'a dağıtmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

1. [Web için Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) veya [Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 güncelleştirme 4](https://www.microsoft.com/download/details.aspx?id=44921) -güncelleştirme 3 veya üstü gereklidir.
3. Bir Azure hesabı. [Burayı](https://azure.microsoft.com/pricing/free-trial/) bir hesap zaten yoksa ücretsiz bir deneme sürümü için.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Genel yönetici, Active Directory'ye ekleme

1. Oturum [Azure Yönetim Portalı](https://manage.windowsazure.com/).
2. Tüm Azure hesaplarını içeren bir **varsayılan dizin** --tıklayın ve ardından **kullanıcılar** sayfanın üst kısmındaki sekme (aşağıdaki görüntü bakın).
3. Kullanıcı Ekle'yi tıklatın.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Yeni bir kullanıcıyla oluşturma **genel yönetici** rol. Tıklatın **kullanıcılar** üst menüsünden ve ardından **Kullanıcı Ekle** komut çubuğundan düğme.
5. İçinde **Kullanıcı Ekle** iletişim kutusunda, yeni kullanıcı için bir ad girin ve ardından sağ oka tıklayın.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Kullanıcı adı girin ve rol kümesine **genel yönetici**. Genel Yöneticiler parola kurtarma amacıyla bir alternatif e-posta adresi gerektirir. Bitirdiğinizde, sağ oka tıklayın.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. İletişim kutusunda sonraki sayfasında **oluşturma**. Geçici bir parola için yeni kullanıcı oluşturulur ve iletişim kutusunda görüntülenir.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
 Parola kaydetmeye ilk oturum açtığında sonra parolayı değiştirmesi gerekir. Aşağıdaki resimde, yeni yönetici hesabı gösterilmiştir. Ayrıca bu sayfada gösterilen Microsoft hesabı değil uygulamanıza oturum için Azure Active Directory kullanmanız gerekir.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>ASP.NET uygulaması oluşturma

Aşağıdaki adımları kullanın [için Visual Studio Express 2013 Web](https://www.microsoft.com/download/details.aspx?id=40747)ve gerektirir [Visual Studio 2013 güncelleştirme 3'ü](https://www.microsoft.com/download/details.aspx?id=43721).

1. Visual Studio'da sırasıyla **dosya** ve ardından **yeni proje**. Üzerinde **yeni proje** iletişim, Visual C# Web projesi sol menüden ve tıklayın select **Tamam**. İşaretini isteyebilirsiniz **projeye Application Insights Ekle** uygulamanız için işlevselliği istemiyorsanız.
2. İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC**ve ardından **kimlik doğrulamayı Değiştir**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Üzerinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **Kurumsal hesaplar**. Bu seçeneklerin yanı sıra otomatik olarak Azure AD ile uygulamanızı kaydetme Azure AD ile tümleştirmek için uygulamanızı otomatik olarak yapılandırmak için kullanılabilir. Kullanmak zorunda değilsiniz **kimlik doğrulamayı Değiştir** kaydetmek ve uygulamanız, ancak yapılandırmak için iletişim kılar çok daha kolay. Visual Studio 2012 örneğin kullanıyorsanız, yine de el ile Azure Yönetim Portalı'nda uygulama kaydeder ve Azure AD ile tümleştirme yapılandırmasını güncelleştirin.  
 Aşağı açılan menüler seçin **bulut - tek bir kurumun** ve **çoklu oturum açma, dizin verilerini okuma**. Örneğin (görüntülerinde aşağıdaki) Azure AD dizininiz için etki alanını girin *aricka0yahoo.onmicrosoft.com*ve ardından **Tamam**. Azure portalındaki varsayılan dizin için etki alanları sekmesinden etki alanı adı alabilirsiniz (sonraki resim aşağıya bakın).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
 Aşağıdaki resim Azure portalından etki alanı adını gösterir.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > İsteğe bağlı olarak Azure AD içinde tıklayarak kaydedilecek uygulama kimliği URI'si yapılandırabilirsiniz **diğer seçenekler**. Uygulama Kimliği URI'si Azure AD'de kayıtlı ve Azure AD ile iletişim kurarken kendini tanıtmak için uygulama tarafından kullanılan bir uygulama için benzersiz tanıtıcıdır. Uygulama Kimliği URI'si ve diğer özellikleri kayıtlı uygulamalar hakkında daha fazla bilgi için bkz: [bu konuda](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Uygulama Kimliği URI'si alan altında onay kutusunu tıklatarak, ayrıca varolan bir kayıt aynı uygulama kimliği URI'si kullanan Azure AD'de üzerine yazmayı seçebilirsiniz.
4. ' I tıklattıktan sonra **Tamam**, bir oturum açma iletişim kutusu görünür ve bir genel yönetici hesabı (aboneliğinizle ilişkili Microsoft hesabı değil) kullanarak oturum açmanız gerekir. Daha önce oluşturduğunuz yeni bir yönetici hesabı, parolayı değiştirmek ve yeni parolayı kullanarak yeniden oturum gerekecek.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Başarıyla kimlik doğruladınız sonra **yeni ASP.NET projesi** iletişim kutusu, kimlik doğrulaması seçeneği Göster (**kuruluş** ) ve yeni uygulama burada olacaktır directory (kayıtlı*aricka0yahoo.onmicrosoft.com* aşağıdaki görüntüde). Bu bilgiler etiketli onay kutusunu işaretleyin **bulutta Barındır**. Bu onay kutusunu seçtiyseniz proje Azure web uygulaması sağlanacak ve daha sonra kolayca yayımlamak için etkinleştirilir. **Tamam**'ı tıklatın.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Azure Web sitesini yapılandırın** iletişim kutusu görünecektir, otomatik olarak oluşturulan site adı ve bölge kullanma. Ayrıca, şu anda içine iletişim kutusunda oturum açtığınız hesabı unutmayın. Bu hesap, Azure aboneliğinizin, bağlı olduğundan emin olmak istiyoruz genellikle bir Microsoft hesabı.

    > [!NOTE]
    > Bu proje bir veritabanı gerektirir. Mevcut veritabanlarından birini seçin veya yeni bir tane oluşturmak gerekir. Bir veritabanı proje bir yerel veritabanı dosyası zaten bir miktarda kimlik doğrulaması yapılandırma verilerini depolamak için kullandığı için gereklidir. Bir Azure Web sitesine uygulamayı dağıttığınızda, bu veritabanı bulutta erişilebilir olan bir seçim yapmanız için dağıtım ile paket değil. **Tamam**'ı tıklatın.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Proje oluşturulur ve kimlik doğrulama seçeneklerini ve web uygulama seçenekleri proje ile otomatik olarak yapılandırılır. Bu işlem tamamlandıktan sonra projeyi yerel olarak basarak çalıştırmak **^ F5**. Kuruluş hesabınızı kullanarak oturum açmanız gerekir. Daha önce oluşturduğunuz hesap için kullanıcı adı ve parola sağlayın ve tıklatın **oturum**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Sonra başarılı oturum açma sayfasının sağ üst köşedeki kullanıcı adı görüntüleyerek doğruladınız ASP.NET sitesi gösterilir.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
 Hatayı alırsanız:  
 Değer null veya boş olamaz. Parametre adı: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
 bkz: [hata ayıklama](#dbg) öğreticinin sonunda bölüm.

## <a name="basics-of-the-graph-api"></a>Grafik API'si temelleri

[Grafik API'si](https://msdn.microsoft.com/library/azure/hh974476.aspx) Azure AD dizininizi CRUD ve nesneler üzerinde başka işlemler gerçekleştirmek için kullanılan programlama arabirimi. Visual Studio 2013'te yeni bir proje oluşturma sırasında kimlik doğrulaması için bir kurumsal hesap seçeneği belirlerseniz, uygulamanız zaten grafik API'sini aramak üzere yapılandırılır. Bu bölümde kısaca, grafik API'sini nasıl çalıştığını gösterir.

1. Çalışan uygulamanızda en üstünde oturum açmış kullanıcının adına tıklayın sayfasının sağ. Bu sizi bir eylem giriş denetleyicisinde kullanıcı profili sayfasına götürür. Tablo yönetici hesabı hakkında daha önce oluşturduğunuz kullanıcı bilgilerini içeren fark edeceksiniz. Bu bilgiler dizininizde depolanır ve grafik API'si Sayfa yüklediğinde, bu bilgileri almak için çağrılır.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Visual Studio'ya geri dönün ve genişletin **denetleyicileri** klasörünü ve ardından açın **HomeController.cs** dosya. Göreceğiniz bir **UserProfile()** bir belirteç almak ve grafik API'sini çağırmak için kodu içeren eylem. Bu kod, aşağıda yineleniyor: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Grafik API'si yi çağırmak için önce bir belirteç almak gerekir. Belirteç alındığında, dize değeri grafik API'si için tüm sonraki istekleri için yetkilendirme üst eklenmesi gerekir. Yukarıdaki kodu çoğunu bir belirteç almak için Azure AD kimlik doğrulaması, grafik API'si için arama yapmak için belirteç kullanma ve böylece görünümde sunulan yanıt dönüştürme ayrıntılarını işler.

    Tartışma için en uygun bölümü aşağıdaki vurgulanan çizgidir: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Bu satırı JSON yanıtı serisi ve görünümde sunulan kullanıcı adını temsil eder.

    HttpClient kullanarak grafik API'sini aramak ve kendiniz ham verileri işlemek, ancak daha kolay bir yolu kullanmaktır [NuGet kullanılabilir olan grafik istemci Kitaplığı](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). İstemci Kitaplığı ham HTTP istekleri ve döndürülen veri dönüştürme, işler ve .NET ortamında grafik API'si ile çalışmak çok daha kolay hale getirir. İlişkili grafik API'si kod örnekleri görmek [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure'a dağıtma

Aşağıdaki adımları uygulamayı Azure'a dağıtmak nasıl yapacağınızı gösterir. Yalnızca birkaç adımda yayımlanmaya hazır olması için önceki adımlarda, yeni projenizi Azure üzerinde bir web uygulaması ile bağlı.

1. Visual Studio'da sağ tıklatın ve proje **Yayımla**. **Web'i Yayımla** iletişim kutusu zaten yapılandırılmış her ayarıyla görünecektir. Tıklayın **sonraki** gitmek için düğmesi **ayarları** sayfası. Kimlik doğrulaması istenebilir; Azure aboneliği hesabınızı (genellikle bir Microsoft hesabı) ve daha önce oluşturduğunuz Kurumsal hesap kullanarak kimlik doğrulaması emin olun.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Denetleme **kuruluş kimlik doğrulamasını etkinleştir** seçeneği. İçinde **etki alanı** alan, dizininiz için etki alanını girin. Gelen **erişim düzeyi** açılan listesinde, select **çoklu oturum açma, dizin verilerini okuma**. İçinde kullanılan önceki veritabanı zaten doldurulmuş fark edeceksiniz **veritabanları** bölümü. Tıklatın **yayımlama**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio Web sitenizi dağıtmaya başlar ve ardından yeni bir tarayıcı penceresi görüntülenir. Dizininiz için bir kez yeniden kimlik doğrulaması istenebilir. Kimlik doğruladınız sonra Azure ile ilgili yeni yayımlanan Web sitesine yönlendirildi.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Uygulama hata ayıklama

Şu hatayı alırsanız:   
 Değer null veya boş olamaz. Parametre adı: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Kodla *görünümler/paylaşılan\\_LoginPartial.cshtml* aşağıdaki dosyasıyla:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Uygulama çalıştırdıktan sonra oturum açan kullanıcının "Null kullanıcı" gösteriyorsa, oturumu kapatın ve yeniden daha önce oluşturduğunuz Active Directory hesabıyla oturum açın.

İzlemek için mükemmel bir öğretici Rick Rainey ait olduğu [derinlemesine bakış: Azure Web siteleri ve kuruluş Azure AD kullanarak kimlik doğrulaması](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Daha fazla bilgi

- [Derin Dalış: Azure Web siteleri ve kuruluş Azure AD kullanarak kimlik doğrulaması](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Azure AD Graph API genel bakış](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD kimlik doğrulama senaryoları](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Github'da Azure AD kod örnekleri](https://github.com/AzureADSamples)
