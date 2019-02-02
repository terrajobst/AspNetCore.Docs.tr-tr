---
uid: web-api/overview/security/external-authentication-services
title: ASP.NET Web API ile dış kimlik doğrulama hizmeti (C#) | Microsoft Docs
author: rmcmurray
description: Dış kimlik doğrulama hizmetlerini ASP.NET Web API'sini kullanmayı açıklar.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: de9b64e6c582059ec66ab352f60773f50af7b1ff
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667862"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a>ASP.NET Web API ile dış kimlik doğrulama hizmeti (C#)

Visual Studio 2017 ve ASP.NET 4.7.2 güvenlik seçeneklerini genişletin [tek sayfalık uygulamalar](../../../single-page-application/index.md) (SPA) ve [Web API](../../index.md) birkaç dahil Dış kimlik doğrulama hizmetleri ile tümleştirme hizmetleri OAuth/Openıd ve sosyal medya kimlik doğrulama hizmetleri: Microsoft hesapları, Twitter, Facebook ve Google.  

### <a name="in-this-walkthrough"></a>Bu izlenecek yolda

- [Dış kimlik doğrulama hizmetlerini kullanma](#USING)
- [Örnek Web uygulaması oluşturma](#SAMPLE)
- [Facebook kimlik doğrulamasını etkinleştirme](#FACEBOOK)
- [Google kimlik doğrulamasını etkinleştirme](#GOOGLE)
- [Microsoft kimlik doğrulamasını etkinleştirme](#MICROSOFT)
- [Twitter kimlik doğrulamasını etkinleştirme](#TWITTER)
- [Ek bilgi](#MOREINFO)

    - [Dış kimlik doğrulama hizmetlerini birleştirme](#COMBINE)
    - [Tam etki alanı adı kullanmak için IIS Express yapılandırması](#FQDN)
    - [Microsoft kimlik doğrulaması için uygulama ayarlarınızı edinme](#OBTAIN)
    - [İsteğe bağlı: Yerel kayıt devre dışı bırak](#DISABLE)

### <a name="prerequisites"></a>Önkoşullar

Bu kılavuzdaki örnekleri izlemek için aşağıdakilere sahip olmanız gerekir:

- Visual Studio 2017
- Uygulama tanımlayıcısı ve aşağıdaki sosyal medya kimlik doğrulaması hizmetlerden biri için gizli anahtar içeren bir geliştirici hesabı:

  - Microsoft hesapları ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
  - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
  - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))
  - Google ([https://developers.google.com/](https://developers.google.com))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Dış kimlik doğrulama hizmetlerini kullanma

Geliştirme azaltmak için web geliştiricilerin Yardım için şu anda kullanılabilir olan dış kimlik doğrulama hizmetlerinin yapmanıza yeni web uygulamaları oluştururken zaman. Kimlik doğrulaması bir dış web hizmetini veya sosyal medya Web sitesi Hizmetleri bir web uygulaması uygular, geliştirme süresini kaydettiği web kullanıcıların genellikle birkaç mevcut hesapları için popüler web hizmetleri ve sosyal Web siteleri, bu nedenle olması kimlik doğrulama uygulaması oluşturma harcanmış olması. Son kullanıcılar, bir dış kimlik doğrulama hizmetini kullanarak web uygulamanız için başka bir hesap oluşturmak zorunda ve başka bir kullanıcı adı ve parolayı unutmayın zorunda kaydeder.

Geçmişte, iki seçeneğiniz geliştiriciler çalıştırılmış: kendi kimlik doğrulama uygulaması oluşturabilir veya bir dış kimlik doğrulama hizmeti uygulamalarına tümleştirmeyi öğrenin. En temel düzeyde bir dış kimlik doğrulama hizmetini kullanmak üzere yapılandırılmış bir web uygulamasından bilgi isteyen bir kullanıcı aracısı (web tarayıcısı) için bir basit isteği akışı aşağıdaki diyagramda gösterilmektedir:

[![](external-authentication-services/_static/image2.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image1.png)

Yukarıdaki diyagramda, kullanıcı aracısı (veya bir web tarayıcısı bu örnekte), web tarayıcısı bir dış kimlik doğrulama hizmetine yönlendiren bir web uygulaması için bir istek gönderir. Kullanıcı Aracısı, kimlik bilgileri Dış kimlik doğrulama hizmetine gönderir ve kullanıcı aracısı başarıyla yapıldığını, dış kimlik doğrulama hizmeti kullanıcı aracısı çeşit özgün web uygulamasıyla yönlendirilecek, simge Kullanıcı Aracısı, web uygulamasına gönderir. Web uygulaması, kullanıcı aracısı Dış kimlik doğrulama hizmeti tarafından başarıyla doğrulandıktan ve web uygulaması kullanıcı aracısı hakkında daha fazla bilgi toplamak için belirteci kullanabilir doğrulamak için belirteci kullanır. Uygulama tamamlandıktan sonra kullanıcı aracısının bilgi işleme, web uygulaması kullanıcı aracısına yetkilendirme ayarlarına göre uygun yanıtı döndürür.

Bu ikinci örnekte, kullanıcı aracısı web uygulama ve dış yetkilendirme sunucusu ile görüşür ve web uygulaması kullanıcı hakkında ek bilgi almak için dış yetkilendirme sunucusu ile ek iletişimi gerçekleştirir. aracı:

[![](external-authentication-services/_static/image4.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image3.png)

Visual Studio 2017 ve ASP.NET 4.7.2 dış kimlik doğrulama hizmetleri ile tümleştirme geliştiriciler için yerleşik tümleştirmesi için aşağıdaki kimlik doğrulama hizmetleri sağlayarak kolaylaştırır:

- Facebook
- Google
- Microsoft Accounts (Windows Live ID hesabı)
- Twitter

Desteklenen Dış kimlik doğrulama hizmetlerinin her biri Visual Studio 2017'yle birlikte gelen yeni ASP.NET Web uygulaması şablonunu kullanarak yapılandırma bu kılavuzdaki örnekler gösterilecektir.

> [!NOTE]
> Gerekirse, dış kimlik doğrulama hizmeti ayarlarını, FQDN eklemek gerekebilir. Bu gereksinim, istemciler tarafından kullanılan FQDN ile eşleşecek şekilde uygulama ayarlarınızı FQDN gerektiren bazı dış kimlik doğrulama hizmetleri için güvenlik kısıtlamaları temel alır. (Bu adımlar, her bir dış kimlik doğrulama hizmeti için büyük ölçüde değişir; bunun gerekli olup olmadığını görmek için her bir dış kimlik doğrulama hizmeti ve bu ayarların nasıl yapılandırılacağı belgelerine başvurmanız gerekir.) Bu ortamın test etmek için bir FQDN kullanmak üzere IIS Express'i yapılandırmak gerekirse [yapılandırma tam etki alanı adı kullanmak için IIS Express](#FQDN) bu kılavuzda daha sonra bölüm.


<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a>Örnek Web uygulaması oluşturma

Aşağıdaki adımları ASP.NET Web uygulaması şablonu kullanılarak bir örnek uygulama oluşturmada size yol göstereceğiz ve bu kılavuzda daha sonra Dış kimlik doğrulama hizmetlerinin her biri için bu örnek uygulama kullanın.

Visual Studio 2017'yi başlatın ve seçin **yeni proje** başlangıç sayfasından. Veya **dosya** menüsünde **yeni** ardından **proje**.

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

Zaman **yeni proje** iletişim kutusu görüntülenirse, seçin **yüklü** genişletin **Visual C#** . Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET Web uygulaması (.Net Framework)**. Projeniz için bir ad girin ve tıklayın **Tamam**.

[![](external-authentication-services/_static/image71.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image71.png)

Zaman **yeni ASP.NET projesi** olduğundan görüntülenen, select **tek sayfalı uygulama** şablonu ve tıklatın **proje oluştur**.

[![](external-authentication-services/_static/image72.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image72.png)

Bekleme Visual Studio 2017, projeniz oluşturur.

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

Visual Studio 2017, projeniz oluşturulurken tamamlandığında açın *Startup.Auth.cs* bulunan dosya **uygulama\_Başlat** klasör.

İlk proje oluşturduğunuzda, dış kimlik doğrulama hizmeti hiçbiri olarak etkinleştirilen *Startup.Auth.cs* dosya; kodunuzu neye benzeyebileceğini aşağıdaki gösterir, nerede için vurgulanan bölümler ile etkinleştirmeniz bir dış kimlik doğrulama hizmetini ve ilgili ayarları Microsoft Accounts, Twitter, Facebook veya Google kimlik doğrulaması ile ASP.NET uygulamanızı kullanmak için:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Derleme ve web uygulamanızda hata ayıklamak için F5 tuşuna bastığınızda, dış kimlik doğrulama hizmeti yok tanımlanmış göreceğiniz bir oturum açma ekranı görüntüler.

[![](external-authentication-services/_static/image73.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image73.png)

Aşağıdaki bölümlerde, Visual Studio 2017'de ASP.NET ile sağlanan Dış kimlik doğrulama hizmetlerinin her biri etkinleştirmek öğreneceksiniz.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Facebook kimlik doğrulamasını etkinleştirme

Facebook ile kimlik doğrulaması Facebook Geliştirici hesabı oluşturmanızı gerektirir ve projeniz bir uygulama kimliği ve gizli anahtar facebook'taki işlevi gerektirir. Bir Facebook developer hesabı oluşturma ve uygulama kimliği ve gizli anahtar alma hakkında daha fazla bilgi için bkz: [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Uygulama kimliği ve gizli anahtarı bir kez almış olan, web uygulamanız için Facebook kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Projenizi Visual Studio 2017'de açık olduğunda, açma *Startup.Auth.cs* dosya.

2. Kod Facebook kimlik doğrulaması bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Kaldırma &quot; // &quot; karakter vurgulanan kod satırlarındaki ve uygulama kimliği ve gizli anahtarı ekleyin. Bu parametreleri ekledikten sonra projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Web uygulamanızı web tarayıcınızda açmak için F5 tuşuna bastığınızda, Facebook bir dış kimlik doğrulama hizmeti olarak tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image74.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image74.png)
5. Tıkladığınızda **Facebook** düğmesi, tarayıcınız Facebook oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image22.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image21.png)
6. Facebook kimlik bilgilerinizi girin ve tıklayın sonra **oturum**, web tarayıcınızda yeniden web uygulamanıza yönlendirilir, sizin için ister **kullanıcı adı** ilişkilendirmek istediğiniz, Facebook hesabı:

    [![](external-authentication-services/_static/image24.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image23.png)
7. Kullanıcı adınızı girdikten ve tıklanan sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Facebook hesabınız için:

    [![](external-authentication-services/_static/image26.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Google kimlik doğrulamasını etkinleştirme

Google'ı kullanarak kimlik doğrulaması gerektiren bir Google developer hesabı oluşturma ve projenize bir uygulama kimliği ve gizli anahtar google'dan işlevi gerektirir. Bir Google developer hesabı oluşturma ve uygulama kimliği ve gizli anahtar alma hakkında daha fazla bilgi için bkz: [ https://developers.google.com ](https://developers.google.com).


Web uygulamanızı Google kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Projenizi Visual Studio 2017'de açık olduğunda, açma *Startup.Auth.cs* dosya.

2. Kod Google kimlik doğrulaması bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Kaldırma &quot; // &quot; karakter vurgulanan kod satırlarındaki ve uygulama kimliği ve gizli anahtarı ekleyin. Bu parametreleri ekledikten sonra projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Web uygulamanızı web tarayıcınızda açmak için F5 tuşuna bastığınızda, Google bir dış kimlik doğrulama hizmeti olarak tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image75.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image75.png)
5. Tıkladığınızda **Google** düğmesi, tarayıcınız Google oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image32.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image31.png)
6. Google kimlik bilgilerinizi girin ve tıklayın sonra **oturum**, Google, web uygulamanızın Google hesabınıza erişmek için izinlere sahip olduğunu doğrulamanızı ister:

    [![](external-authentication-services/_static/image34.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image33.png)
7. Tıkladığınızda **kabul**, web tarayıcınızda yeniden web uygulamanıza yönlendirilir, sizin için ister **kullanıcı adı** Google hesabınızla ilişkilendirmek istediğiniz:

    [![](external-authentication-services/_static/image36.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image35.png)
8. Kullanıcı adınızı girdikten ve tıklanan sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Google hesabınız için:

    [![](external-authentication-services/_static/image38.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Microsoft kimlik doğrulamasını etkinleştirme

Bir geliştirici hesabı oluşturmak Microsoft kimlik doğrulaması gerektiriyor ve çalışabilmesi için istemci kimliği ve gizli anahtar gerekli. Microsoft developer hesabı oluşturma ve istemci kimliği ve gizli anahtar alma hakkında daha fazla bilgi için bkz: [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Tüketici anahtarı ve tüketici gizli bir kez almış olan, web uygulamanız için Microsoft kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Projenizi Visual Studio 2017'de açık olduğunda, açma *Startup.Auth.cs* dosya.

2. Kod Microsoft kimlik doğrulaması bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Kaldırma &quot; // &quot; karakter vurgulanan kod satırlarındaki ve istemci Kimliğini ve istemci gizli anahtarını ekleyin. Bu parametreleri ekledikten sonra projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Web uygulamanızı web tarayıcınızda açmak için F5 tuşuna bastığınızda, bir dış kimlik doğrulama hizmeti olarak Microsoft tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image42.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image41.png)
5. Tıkladığınızda **Microsoft** düğmesi, tarayıcınız Microsoft oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image44.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image43.png)
6. Microsoft kimlik bilgilerinizi girin ve tıklayın sonra **oturum**, web uygulamanızı Microsoft hesabınıza erişmek için izinlere sahip olduğunu doğrulayın istenir:

    [![](external-authentication-services/_static/image46.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image45.png)
7. Tıkladığınızda **Evet**, web tarayıcınızda yeniden web uygulamanıza yönlendirilir, sizin için ister **kullanıcı adı** Microsoft hesabınızla ilişkilendirmek istediğiniz:

    [![](external-authentication-services/_static/image48.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image47.png)
8. Kullanıcı adınızı girdikten ve tıklanan sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Microsoft hesabınız için:

    [![](external-authentication-services/_static/image50.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Twitter kimlik doğrulamasını etkinleştirme

Kimlik doğrulaması, bir geliştirici hesabı oluşturmanızı gerektirir ve çalışabilmesi için bir tüketici anahtarı ve tüketici gizli gerekli twitter. Bir Twitter developer hesabı oluşturma ve tüketici anahtarı ve tüketici gizli anahtarı alma hakkında daha fazla bilgi için bkz: [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Tüketici anahtarı ve tüketici gizli bir kez almış olan, web uygulamanız için Twitter kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları kullanın:

1. Projenizi Visual Studio 2017'de açık olduğunda, açma *Startup.Auth.cs* dosya.

2. Kod Twitter kimlik doğrulaması bölümünü bulun:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Kaldırma &quot; // &quot; karakter vurgulanan kod satırlarındaki ve tüketici anahtarı ve tüketici gizli anahtarı ekleyin. Bu parametreleri ekledikten sonra projenizi yeniden derleyin:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Web uygulamanızı web tarayıcınızda açmak için F5 tuşuna bastığınızda, Twitter bir dış kimlik doğrulama hizmeti olarak tanımlanmış görürsünüz:

    [![](external-authentication-services/_static/image54.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image53.png)
5. Tıkladığınızda **Twitter** düğmesi, tarayıcınız Twitter oturum açma sayfasına yönlendirilir:

    [![](external-authentication-services/_static/image56.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image55.png)
6. Twitter kimlik bilgilerinizi girin ve tıklayın sonra **uygulamayı Yetkilendir**, web tarayıcınızda yeniden web uygulamanıza yönlendirilir, sizin için ister **kullanıcı adı** ilişkilendirmek istediğiniz Twitter hesabınızı:

    [![](external-authentication-services/_static/image58.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image57.png)
7. Kullanıcı adınızı girdikten ve tıklanan sonra **kaydolun** düğmesi, web uygulamanızın varsayılan görüntüler **giriş sayfası** Twitter hesabınız için:

    [![](external-authentication-services/_static/image60.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Ek Bilgiler

OAuth ve Openıd kullanan uygulamalar oluşturma hakkında ek bilgi için aşağıdaki URL'ler bakın:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Dış kimlik doğrulama hizmetlerini birleştirme

Daha fazla esneklik için aynı anda birden fazla dış kimlik doğrulama hizmeti tanımlayabilirsiniz - bu uygulamanın kullanıcıları, etkin Dış kimlik doğrulama hizmetlerinin herhangi bir hesabı kullanacak şekilde web sağlar:

[![](external-authentication-services/_static/image62.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a>Bir tam etki alanı adını kullanmak için IIS Express'i yapılandırmak

Bazı dış kimlik doğrulama sağlayıcıları gibi bir HTTP adresi kullanarak uygulamanızı test etme desteklemeyen `http://localhost:port/`. Bu sorunu geçici olarak çözmek için statik bir tam etki alanı adı (FQDN) eşleme KONAKLARI dosyanıza ekleyin ve FQDN test/hata ayıklama için kullanılacak Visual Studio 2017'de, proje seçenekleri yapılandırın. Bunu yapmak için aşağıdaki adımları kullanın:

- Eşleme, ana bilgisayarlar dosyası statik bir FQDN ekleyin:

  1. Windows yükseltilmiş bir komut istemi açın.
  2. Şu komutu yazın:

      <kbd>Not Defteri'ni %WinDir%\system32\drivers\etc\hosts</kbd>
  3. Aşağıdaki gibi bir giriş HOSTS dosyasına ekleyin:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. KONAKLARI dosyasını kaydedip kapatın.

- Visual Studio projenizi FQDN kullanmak üzere yapılandırın:

  1. Projenizi Visual Studio 2017'de açık olduğunda tıklayın **proje** menüsünde ve projenizin Özellikler'i seçin. Örneğin, seçtiğiniz **WebApplication1 özellikleri**.
  2. Seçin **Web** sekmesi.
  3. İçin bir FQDN değerinizi girin <strong>proje URL'si</strong>. Örneğin, şunu yazarsınız: <kbd> <http://www.wingtiptoys.com> </kbd> , HOSTS dosyasına eklendi FQDN eşleme olup olmadığını.

- Uygulamanız için bir FQDN kullanmak için IIS Express yapılandırın:

    1. Windows yükseltilmiş bir komut istemi açın.
    2. IIS Express klasör olarak değiştirmek için aşağıdaki komutu yazın:

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. FQDN uygulamanıza eklemek için aşağıdaki komutu yazın:

        <kbd>appcmd.exe set config-section:system.applicationHost/sites /+&quot;[ad='WebApplication1'].bindings.[Protokol='http',Bindingınformation='*:80:www.wingtiptoys.com']&quot; /Commit:APPHOST</kbd>

  Burada **WebApplication1** projenizin adıdır ve **Bindingınformation** testiniz için kullanmak istediğiniz bağlantı noktası numarası ve FQDN içerir.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Microsoft kimlik doğrulaması için uygulama ayarlarınızı edinme

Microsoft Authentication Windows Live uygulamaya bağlama basit bir işlemdir. Zaten bir Windows Live uygulamaya bağlantı oluşturmadınız varsa aşağıdaki adımları kullanabilirsiniz:

1. Gözat [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) ve Microsoft hesap adınızı ve istendiğinde parolayı girin ve ardından tıklayın **oturum**:

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. Seçin **uygulama ekleme** istendiğinde uygulamanızın adını girin ve ardından **Oluştur**:

    [![](external-authentication-services/_static/image79.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image79.png)
3. Uygulamanızı altında seçin **adı** ve kendi uygulama özellikleri sayfasında görüntülenir.

4. Uygulamanız için yeniden yönlendirme etki alanını girin. Kopyalama **uygulama kimliği** ve altında **uygulama gizli dizilerini**seçin **oluşturmak parola**. Görüntülenen parola kopyalayın. Uygulama kimliği ve parolası, istemci Kimliğini ve istemci gizli anahtarı olan. Seçin **Tamam** ardından **Kaydet**.

    [![](external-authentication-services/_static/image77.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>İsteğe bağlı: Yerel kayıt devre dışı bırak

Geçerli ASP.NET yerel kayıt işlevi otomatik programların (botlar) üyesi hesapları oluşturmanızı engellemez; gibi bir bot önleme ve doğrulama teknolojisini kullanarak örneğin, [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Bu nedenle, oturum açma sayfasındaki yerel oturum açma formu ve kayıt bağlantısını kaldırmanız gerekir. Bunu yapmak için açık  *\_Login.cshtml* sayfasında projenizde ve yerel oturum açma paneli ve kayıt bağlantısını için çizgilerin ardından açıklama. Sonuçta elde edilen sayfa, aşağıdaki kod örneği gibi görünmelidir:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Yerel oturum açma paneli ve kayıt bağlantısını devre dışı bıraktıktan sonra oturum açma sayfanız yalnızca etkinleştirdiğiniz Dış kimlik doğrulama sağlayıcıları görüntüler:

[![](external-authentication-services/_static/image70.png "Görüntü genişletmek için tıklayın")](external-authentication-services/_static/image69.png)
