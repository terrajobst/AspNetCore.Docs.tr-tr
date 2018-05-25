---
uid: web-api/overview/security/external-authentication-services
title: ASP.NET Web API ile dış kimlik doğrulama hizmeti (C#) | Microsoft Docs
author: rmcmurray
description: ASP.NET Web API'de Dış kimlik doğrulama hizmetleri kullanmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 406a85db7055910cb7a4e15fec8ef68dff5a19dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>ASP.NET Web API ile dış kimlik doğrulama hizmeti (C#)
====================
tarafından [Robert McMurray '](https://github.com/rmcmurray)

Visual Studio 2013 ve ASP.NET 4.5.1 genişletmek için güvenlik seçenekleri [tek sayfa uygulamaları](../../../single-page-application/index.md) (SPA) ve [Web API](../../index.md) birkaç dahil Dış kimlik doğrulama hizmetleri ile tümleştirme hizmetleri OAuth/Openıd ve sosyal medya kimlik doğrulama hizmetleri: Microsoft Accounts, Twitter, Facebook ve Google.

### <a name="in-this-walkthrough"></a>Bu kılavuzda

- [Dış kimlik doğrulama hizmetleri kullanma](#USING)
- [Örnek Web uygulaması oluşturma](#SAMPLE)
- [Facebook kimlik doğrulamasını etkinleştirme](#FACEBOOK)
- [Google kimlik doğrulamasını etkinleştirme](#GOOGLE)
- [Microsoft kimlik doğrulamasını etkinleştirme](#MICROSOFT)
- [Twitter kimlik doğrulamasını etkinleştirme](#TWITTER)
- [Ek bilgi](#MOREINFO)

    - [Dış kimlik doğrulama hizmeti birleştirme](#COMBINE)
    - [Tam etki alanı adı kullanmak için IIS Express yapılandırma](#FQDN)
    - [Microsoft kimlik doğrulaması için uygulama ayarlarınızı edinme](#OBTAIN)
    - [İsteğe bağlı: Yerel kaydını devre dışı](#DISABLE)

### <a name="prerequisites"></a>Önkoşullar

Bu kılavuzda örnekleri izlemek için aşağıdakilere sahip olmanız gerekir:

- Visual Studio 2013
- Bir hesap için en az bir aşağıdaki Dış kimlik doğrulama hizmeti:

    - Bir Google kullanıcı hesabı
    - Bir geliştirici hesabını aşağıdaki sosyal medya kimlik doğrulaması hizmetlerden biri için gizli anahtar ve uygulama tanımlayıcısı ile:

        - Microsoft hesapları ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Dış kimlik doğrulama hizmetleri kullanma

Bol miktarda geliştirme azaltmak için web geliştiricileri Yardım şu anda kullanılabilir Dış kimlik doğrulama hizmetleri zaman yeni web uygulamaları oluştururken. Bir dış web hizmeti veya sosyal medya Web sitesi kimlik doğrulama hizmetleri web uygulaması uygular, geliştirme zamanı, kaydettiğinde web kullanıcıları genellikle birkaç var olan hesapları popüler web hizmetleri ve sosyal medya Web siteleri, bu nedenle sahip kimlik doğrulama uygulaması oluşturma harcadığınız. Bir dış kimlik doğrulama hizmeti kullanan son kullanıcılar web uygulamanız için başka bir hesap oluşturmak sahip ve aynı zamanda başka bir kullanıcı adı ve parola anımsamak kaydeder.

Geçmişte, geliştiricilerin iki seçenek beklendiğinden: kendi kimlik doğrulama uygulaması oluşturun veya bir dış kimlik doğrulama hizmeti uygulamalarına tümleştirmeleri öğrenin. En temel düzeyde, aşağıdaki diyagramda bir dış kimlik doğrulama hizmeti kullanmak üzere yapılandırılmış bir web uygulamasından bilgilerini isteyen bir kullanıcı aracısı (web tarayıcısı) için bir basit istek akışı gösterilmektedir:

[![](external-authentication-services/_static/image2.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image1.png)

Yukarıdaki şemada, kullanıcı aracısı (veya web tarayıcısı bu örnekte) bir web tarayıcısı bir dış kimlik doğrulama hizmeti yeniden yönlendiren bir web uygulaması için istekte bulunur. Kullanıcı Aracısı, kimlik bilgileri Dış kimlik doğrulama hizmetine gönderir ve kullanıcı aracısı başarıyla doğrulaması, dış kimlik doğrulama hizmeti kullanıcı aracısı çeşit özgün web uygulamasıyla yönlendirilecek hangi belirteci Kullanıcı Aracısı web uygulamasına gönderir. Web uygulaması, kullanıcı aracısı Dış kimlik doğrulama hizmeti tarafından başarıyla doğrulandı ve web uygulaması kullanıcı aracısı hakkında daha fazla bilgi toplamak için belirteci kullanabilir doğrulamak için belirteç kullanır. Uygulama yaptıktan sonra kullanıcı aracısının bilgilerini işleme, web uygulamasının yetkilendirme ayarlarına bağlı kullanıcı aracısına uygun yanıtı döndürür.

Bu ikinci örnekte, kullanıcı aracısı dış yetkilendirme sunucusu ve web uygulaması ile görüşür ve web uygulaması kullanıcı hakkında ek bilgi almak için dış yetkilendirme sunucusu ile ek iletişim gerçekleştirir aracı:

[![](external-authentication-services/_static/image4.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image3.png)

Visual Studio 2013 ve ASP.NET 4.5.1 dış kimlik doğrulama hizmetleri ile tümleştirme geliştiriciler için aşağıdaki kimlik doğrulama hizmetleri için yerleşik tümleştirme sağlayarak kolaylaştırmak:

- Facebook
- Google
- Microsoft Accounts (Windows Live ID hesapları)
- Twitter

Bu kılavuzda örnekler desteklenen Dış kimlik doğrulama hizmetlerinin her biri Visual Studio 2013 ile birlikte gelen yeni ASP.NET Web uygulaması şablonu kullanarak yapılandırma gösterilmektedir.

> [!NOTE]
> Gerekirse, ayarları, dış kimlik doğrulama hizmeti için FQDN değerinizi eklemeniz gerekebilir. Bu gereksinim, istemciler tarafından kullanılan FQDN eşleştirmek, uygulamanızın ayarlarını FQDN gerektiren bazı dış kimlik doğrulama hizmetleri için güvenlik kısıtlamaları temel alır. (Bu adımları her dış kimlik doğrulama hizmeti için büyük ölçüde farklılık gösterir; bu gerekli olup olmadığını görmek için her dış kimlik doğrulama hizmeti ve bu ayarların nasıl yapılandırılacağı yönelik belgelere gerekecek.) Bu ortam test etmek için bir FQDN kullanmak için bkz. IIS Express'i yapılandırmak gerekirse [yapılandırma tam etki alanı adı kullanmak için IIS Express](#FQDN) bu kılavuzda daha sonra bölüm.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Örnek Web uygulaması oluşturma

Aşağıdaki adımları ASP.NET Web uygulaması şablonu kullanarak örnek bir uygulama oluşturmada size yol göstereceğiz ve bu kılavuzda daha sonra Dış kimlik doğrulama hizmetlerinin her biri için bu örnek uygulama kullanır.

Visual Studio 2013 seçin Başlat **yeni proje** başlangıç sayfasından. Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.

[![](external-authentication-services/_static/image6.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image5.png)

Zaman **yeni proje** iletişim kutusu görüntülenir, seçin **yüklü** **şablonları** ve genişletin **Visual C#**. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET Web uygulaması**. Projeniz için bir ad girin ve tıklayın **Tamam**.

[![](external-authentication-services/_static/image8.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image7.png)

Zaman **yeni ASP.NET projesi** görüntülenen ise select **SPA** şablonu ve tıklatın **proje oluştur**.

[![](external-authentication-services/_static/image10.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image9.png)

Visual Studio 2013 bekleyin, projenizi oluşturur.

[![](external-authentication-services/_static/image12.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image11.png)

Visual Studio 2013 projenizi oluşturma tamamlandığında, açık *Startup.Auth.cs* bulunan dosya **uygulama\_Başlat** klasör.

[![](external-authentication-services/_static/image14.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image13.png)

İlk proje oluşturduğunuzda, dış kimlik doğrulama hizmeti hiçbiri etkinleştirilmiş olan *Startup.Auth.cs* dosya; kodunuzu şuna benzeyebilir aşağıdaki gösterilmektedir, where için vurgulanan bölümleri ile etkinleştirmeniz bir dış kimlik doğrulama hizmeti ve ASP.NET uygulamanızın Microsoft Accounts, Twitter, Facebook veya Google kimlik doğrulaması kullanmak için tüm ilgili ayarları:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Derleme ve web uygulamanızda hata ayıklama için F5 tuşuna bastığınızda, burada bir dış kimlik doğrulama hizmeti tanımlanmadı göreceğiniz bir oturum açma ekranı görüntülenir.

[![](external-authentication-services/_static/image16.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image15.png)

Aşağıdaki bölümlerde, Visual Studio 2013'te ASP.NET ile sağlanan Dış kimlik doğrulama hizmetlerinin her biri etkinleştirme hakkında bilgi edineceksiniz.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Facebook kimlik doğrulamasını etkinleştirme

Facebook kullanarak kimlik doğrulaması Facebook developer hesabı oluşturma gerektirir ve projenizin bir uygulama kimliği ile gizli anahtarını facebook'taki çalışması için gerektirir. Bir Facebook developer hesabı oluşturma ve uygulama kimliği ve gizli anahtar alma hakkında daha fazla bilgi için bkz: [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Uygulama kimliği ile gizli anahtarını kez almış, web uygulamanız için Facebook kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Visual Studio 2013'te projeniz açıktır açtığınızda *Startup.Auth.cs* dosyası:

    [![](external-authentication-services/_static/image18.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image17.png)
2. Vurgulanmış kodu bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Kaldırma &quot; // &quot; karakterleri vurgulanan satırlar kodunun açıklamasını kaldırın ve sonra uygulama kimliği ve gizli anahtar ekleyin. Bu parametreler ekledikten sonra projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Web uygulamanızı web tarayıcısında açmak için F5 tuşuna bastığınızda, Facebook bir dış kimlik doğrulama hizmeti olarak tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image20.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image19.png)
5. Tıkladığınızda **Facebook** düğmesi, tarayıcınızın Facebook oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image22.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image21.png)
6. Facebook kimlik bilgilerinizi girdikten'ı tıklatın **oturum**, web tarayıcınızın geri web uygulamanıza yönlendirilir ve hangi için ister **kullanıcı adı** ile ilişkilendirmek istediğiniz, Facebook hesabı:

    [![](external-authentication-services/_static/image24.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image23.png)
7. Girdiğiniz kullanıcı adı ve tıklandığında sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Facebook hesabınız için:

    [![](external-authentication-services/_static/image26.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Google kimlik doğrulamasını etkinleştirme

Google ile ne kadar kolay bir geliştirici hesabını gerektirmeyen veya uygulama kimliği veya gizli anahtarı gibi ek bilgileri bir dış kimlik doğrulama hizmeti olarak ihtiyacı yoktur çünkü etkinleştirmek için Dış kimlik doğrulama hizmetleri olduğu gerekli.

Web uygulamanızın Google kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Visual Studio 2013'te projeniz açıktır açtığınızda *Startup.Auth.cs* dosyası:

    [![](external-authentication-services/_static/image28.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image27.png)
2. Vurgulanmış kodu bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Kaldırma &quot; // &quot; karakterleri vurgulanan kod satırını açıklamadan çıkarın ve projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Web uygulamanızı web tarayıcısında açmak için F5 tuşuna bastığınızda, Google bir dış kimlik doğrulama hizmeti olarak tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image30.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image29.png)
5. Tıkladığınızda **Google** düğmesi, tarayıcınızın Google oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image32.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image31.png)
6. Google kimlik bilgilerinizi girin ve tıklayın sonra **oturum**, Google, web uygulamanızın Google hesabınız erişim izni olduğunu doğrulamanızı ister:

    [![](external-authentication-services/_static/image34.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image33.png)
7. Tıkladığınızda **kabul**, web tarayıcınızın geri web uygulamanıza yönlendirilir ve hangi için ister **kullanıcı adı** Google hesabınızla ilişkilendirmek istediğiniz:

    [![](external-authentication-services/_static/image36.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image35.png)
8. Girdiğiniz kullanıcı adı ve tıklandığında sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Google hesabınız için:

    [![](external-authentication-services/_static/image38.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Microsoft kimlik doğrulamasını etkinleştirme

Microsoft kimlik doğrulaması bir geliştirici hesabı oluşturmanızı gerektirir ve çalışmak için bir istemci kimliği ve istemci gizli anahtarı gerektirir. Microsoft developer hesabı oluşturma ve istemci kimliği ve istemci gizli anahtarı edinme hakkında daha fazla bilgi için bkz: [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Bir kez tüketici anahtarı ve tüketici gizli aldıysanız, web uygulamanızı Microsoft kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Visual Studio 2013'te projeniz açıktır açtığınızda *Startup.Auth.cs* dosyası:

    [![](external-authentication-services/_static/image40.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image39.png)
2. Vurgulanmış kodu bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Kaldırma &quot; // &quot; karakterleri vurgulanan satırlar kodunun açıklamasını kaldırın ve sonra istemci Kimliğini ve istemci gizli anahtarı ekleyin. Bu parametreler ekledikten sonra projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Web uygulamanızı web tarayıcısında açmak için F5 tuşuna bastığınızda, Microsoft bir dış kimlik doğrulama hizmeti olarak tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image42.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image41.png)
5. Tıkladığınızda **Microsoft** düğmesi, tarayıcınızın Microsoft oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image44.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image43.png)
6. Microsoft kimlik bilgilerinizi girin ve tıklayın sonra **oturum**, web uygulamanızı Microsoft hesabınıza erişmek için izinlere sahip olduğunu doğrulayın istenir:

    [![](external-authentication-services/_static/image46.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image45.png)
7. Tıkladığınızda **Evet**, web tarayıcınızın geri web uygulamanıza yönlendirilir ve hangi için ister **kullanıcı adı** Microsoft hesabınızla ilişkilendirmek istediğiniz:

    [![](external-authentication-services/_static/image48.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image47.png)
8. Girdiğiniz kullanıcı adı ve tıklandığında sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Microsoft hesabınız için:

    [![](external-authentication-services/_static/image50.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Twitter kimlik doğrulamasını etkinleştirme

Twitter kimlik doğrulaması, bir geliştirici hesabı oluşturmak gerektirir ve çalışmak için bir tüketici anahtarı ve tüketici gizli anahtarı gerektirir. Bir Twitter developer hesabı oluşturma ve tüketici anahtarı ve tüketici gizli anahtarı edinme hakkında daha fazla bilgi için bkz: [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Bir kez tüketici anahtarı ve tüketici gizli aldıysanız, web uygulamanızı Twitter kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Visual Studio 2013'te projeniz açıktır açtığınızda *Startup.Auth.cs* dosyası:

    [![](external-authentication-services/_static/image52.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image51.png)
2. Vurgulanmış kodu bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Kaldırma &quot; // &quot; karakterleri vurgulanan satırlar kodunun açıklamasını kaldırın ve sonra tüketici anahtarı ve tüketici gizli anahtarı ekleyin. Bu parametreler ekledikten sonra projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Web uygulamanızı web tarayıcısında açmak için F5 tuşuna bastığınızda, Twitter bir dış kimlik doğrulama hizmeti olarak tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image54.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image53.png)
5. Tıkladığınızda **Twitter** düğmesi, tarayıcınızı Twitter oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image56.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image55.png)
6. Twitter kimlik bilgilerinizi girdikten'ı tıklatın **Authorize uygulama**, web tarayıcınızın geri web uygulamanıza yönlendirilir ve hangi için ister **kullanıcı adı** ile ilişkilendirmek istediğiniz Twitter hesabınız:

    [![](external-authentication-services/_static/image58.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image57.png)
7. Girdiğiniz kullanıcı adı ve tıklandığında sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Twitter hesabınız için:

    [![](external-authentication-services/_static/image60.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Ek Bilgiler

OAuth ve Openıd kullanan uygulamaları oluşturma hakkında ek bilgi için aşağıdaki URL'ler bakın:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Dış kimlik doğrulama hizmeti birleştirme

Daha fazla esneklik için aynı anda birden çok dış kimlik doğrulama hizmeti tanımlayabilirsiniz - bu web uygulamasının kullanıcıların etkin Dış kimlik doğrulama hizmetlerini bir hesaptan kullanmasına izin verir:

[![](external-authentication-services/_static/image62.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Tam etki alanı adı kullanmak için IIS Express yapılandırma

Bazı dış kimlik doğrulama sağlayıcıları gibi bir HTTP adresi kullanarak uygulamanızı test etme desteklemeyen `http://localhost:port/`. Bu sorunu çözmek için statik bir tam etki alanı adı (FQDN) eşleme HOSTS dosyanıza ekleyip test/hata ayıklama için FQDN kullanmak için Visual Studio 2013'te, proje seçenekleri yapılandırabilirsiniz. Bunu yapmak için aşağıdaki adımları kullanın:

- HOSTS dosyasını eşleme statik bir FQDN ekleyin:

  1. Windows'da yükseltilmiş bir komut istemi açın.
  2. Şu komutu yazın:

      <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
  3. HOSTS dosyasına bir giriş aşağıdaki gibi ekleyin:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. HOSTS dosyasını kaydedip kapatın.

- Visual Studio projenizi FQDN'si kullanacak şekilde yapılandırın:

  1. Projeniz Visual Studio 2013'te açık olduğunda tıklatın **proje** menüsünde ve projenizin Özellikler'i seçin. Örneğin, seçebilirsiniz **WebApplication1 özellikleri**.
  2. Seçin **Web** sekmesi.
  3. İçin FQDN değerinizi girin <strong>proje URL'si</strong>. Örneğin, girersiniz <kbd> <http://www.wingtiptoys.com> </kbd> HOSTS dosyasına eklenen FQDN eşleme olup olmadığı.

- Uygulamanız için FQDN kullanmak için IIS Express yapılandırın:

    1. Windows'da yükseltilmiş bir komut istemi açın.
    2. IIS Express klasörünüzü değiştirmek için aşağıdaki komutu yazın:

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. FQDN uygulamanıza eklemek için aşağıdaki komutu yazın:

        <kbd>appcmd.exe set config-section:system.applicationHost/sites /+&quot;[ad='WebApplication1'].bindings.[Protokol='http',Bindingınformation='*:80:www.wingtiptoys.com']&quot; /Commit:APPHOST</kbd>

  Burada **WebApplication1** projenizi adıdır ve **Bindingınformation** , test için kullanmak istediğiniz bağlantı noktası numarası ve FQDN içerir.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Microsoft kimlik doğrulaması için uygulama ayarlarınızı edinme

Windows Live Microsoft Authentication için uygulamaya bağlama basit bir işlemdir. Windows Live uygulamaya bağlanmamış değilse, aşağıdaki adımları kullanabilirsiniz:

1. Gözat [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) ve Microsoft hesap adınızı ve istendiğinde parolayı girin ve ardından **oturum**:

    [![](external-authentication-services/_static/image64.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image63.png)
2. İstendiğinde, uygulamanızın dil ve adını girin ve ardından **kabul ediyorum**:

    [![](external-authentication-services/_static/image66.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image65.png)
3. Üzerinde **API ayarları** sayfasında uygulamanız için uygulama ve kopyalama için yeniden yönlendirme etki alanını girin **istemci kimliği** ve **gizli** , projeniz için ve ardından tıklatın **kaydetmek**:

    [![](external-authentication-services/_static/image68.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>İsteğe bağlı: Yerel kaydını devre dışı

Geçerli ASP.NET yerel kayıt işlevi otomatik programları (aracılarını) hesapları üye oluşturma engellemez; Örneğin, bir bot önleme ve doğrulama teknoloji gibi kullanarak tarafından [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Bu nedenle, oturum açma sayfasındaki yerel oturum açma form ve kayıt bağlantıyı kaldırmanız gerekir. Bunu yapmak için açık  *\_Login.cshtml* projenizde sayfa açıklamadan çıkarın ve ardından satırları yerel oturum açma paneli ve kayıt bağlantısı için. Sonuçta elde edilen sayfa, aşağıdaki kod örneği gibi görünmelidir:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Yerel oturum açma paneli ve kayıt bağlantı devre dışı bıraktıktan sonra oturum açma sayfanız yalnızca etkinleştirdiğiniz Dış kimlik doğrulama sağlayıcıları görüntüler:

[![](external-authentication-services/_static/image70.png "Görüntü genişletmek için tıklatın")](external-authentication-services/_static/image69.png)
