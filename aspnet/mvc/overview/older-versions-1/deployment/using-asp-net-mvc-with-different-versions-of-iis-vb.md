---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: "ASP.NET MVC farklı sürümleri IIS (VB) ile kullanma | Microsoft Docs"
author: microsoft
description: "Bu öğreticide, ASP.NET MVC ve URL yönlendirme, Internet Information Services farklı sürümlerini kullanmayı öğrenin. Farklı stratejileri öğrenin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c9c3bf004b13677728c7c6bf2f5adf6a264dc49
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>ASP.NET MVC farklı sürümleri IIS (VB) ile kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu öğreticide, ASP.NET MVC ve URL yönlendirme, Internet Information Services farklı sürümlerini kullanmayı öğrenin. IIS 7.0 (Klasik mod), IIS 6.0 ve IIS önceki sürümleri ile ASP.NET MVC kullanarak farklı stratejileri öğrenin.


ASP.NET MVC çerçevesi, denetleyici eylemleri için rota tarayıcı isteklerine ASP.NET yönlendirme bağlıdır. ASP.NET yönlendirme avantajlarından yararlanmak için web sunucunuz üzerinde ek yapılandırma adımları gerçekleştirmeniz gerekebilir. Tüm Internet Information Services (IIS) ve işlem modu, uygulamanız için istek sürümüne bağlıdır.

IIS farklı sürümlerini bir özeti aşağıda verilmiştir:

- IIS 7.0 (tümleşik mod) - ASP.NET yönlendirmeyi kullanmak için gereken özel yapılandırma.
- IIS 7.0 (Klasik mod) - ASP.NET yönlendirmeyi kullanmak için özel yapılandırma gerçekleştirmeniz gerekir.
- IIS 6.0 veya aşağıda - ASP.NET yönlendirmeyi kullanmak için özel yapılandırma gerçekleştirmeniz gerekir.

En son IIS sürüm 7.5 (Win7) sürümüdür. IIS 7'ın IIS ile Windows Server 2008 ve VISTA/SP1 dahil ve daha yüksek. IIS 7.0 Vista işletim sisteminin Home Basic dışında herhangi bir sürümü yükleyebilirsiniz (bkz [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7.0 istekleri işlemek için iki modunu destekler. Tümleşik mod veya Klasik modda kullanabilirsiniz. IIS 7.0 tümleşik modunda kullanırken hiçbir özel yapılandırma adımlarını gerçekleştirmeniz gerekmez. Ancak, IIS 7.0 Klasik modda kullanılırken ek yapılandırma gerçekleştirmeniz gerekir.

Microsoft Windows Server 2003, IIS 6.0 içerir. Windows Server 2003 işletim sistemi kullanırken, IIS 6.0 IIS 7.0 için yükseltemezsiniz. IIS 6.0 kullanırken ek yapılandırma adımları gerçekleştirmeniz gerekir.

Microsoft Windows XP Professional IIS 5.1 içerir. IIS 5.1 kullanırken ek yapılandırma adımları gerçekleştirmeniz gerekir.

Son olarak, Microsoft Windows 2000 ve Microsoft Windows 2000 Professional, IIS 5.0 içerir. IIS 5.0 kullanırken ek yapılandırma adımları gerçekleştirmeniz gerekir.

## <a name="integrated-versus-classic-mode"></a>Klasik mod ile tümleşik

IIS 7.0, iki farklı istek işleme modunu kullanarak isteklerini işleyebilir: tümleşik ve klasik. Tümleşik mod, daha iyi performans ve daha fazla özellik sağlar. Klasik mod dahil için geriye dönük uyumluluk önceki IIS sürümleriyle.

İstek işleme modunu uygulama havuzu tarafından belirlenir. Uygulama ile ilişkilendirilmiş uygulama havuzunu belirleyerek, hangi işleme modunu belirli web uygulaması tarafından kullanılıyor belirleyebilirsiniz. Aşağıdaki adımları uygulayın:

1. Internet Information Services Yöneticisi başlatma
2. Bağlantıları penceresinde, bir uygulama seçin
3. Eylemler penceresinde **temel ayarları** uygulamasını Düzenle iletişim kutusunu açmak için bağlantı (bkz: Şekil 1) kutusu
4. Seçili uygulama havuzunu not edin.

Varsayılan olarak, iki uygulama havuzları desteklemek için IIS yapılandırılır: **DefaultAppPool** ve **Classic .NET AppPool**. DefaultAppPool seçtiyseniz, uygulamanızın tümleşik isteği işleme modunda çalışıyor. Classic .NET AppPool seçtiyseniz, uygulamanızın Klasik isteği işleme modunda çalışıyor.


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Şekil 1**: istek işleme modunu algılama ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))


İstek işleme modunu uygulamasını Düzenle iletişim kutusunda değiştirebilirsiniz dikkat edin. Seç düğmesini tıklayın ve uygulama ile ilişkili uygulama havuzunu değiştirmek. Bir ASP.NET uygulaması için tümleşik mod Klasikten değiştirirken uyumluluk sorunları olduğunu unutmayın. Daha fazla bilgi için aşağıdaki makalelere bakın:

- IIS 7.0 Windows Vista ve Windows Server 2008--ASP.NET 1.1 yükseltme [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- ASP.NET ve IIS 7.0 - tümleşmesi [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)


Bir ASP.NET uygulaması DefaultAppPool kullanıyorsanız, ASP.NET yönlendirme (ve bu nedenle ASP.NET MVC) çalışmaya almak için ek adımlar gerçekleştirmeniz gerekmez. Ancak, ASP.NET uygulaması Classic .NET AppPool kullanın sonra okuma korumak için yapılandırılmışsa, yapmak için daha fazla iş sahip.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>ASP.NET MVC eski sürümleri IIS ile kullanma

Ardından, IIS 7.0 IIS daha eski bir sürüm ile ASP.NET MVC kullanmanız gerekebilir veya IIS 7.0 Klasik modda kullanmak gerekirse, iki seçeneğiniz vardır. İlk olarak, dosya uzantıları kullanmak için yol tablosu değiştirebilirsiniz. Örneğin, bir URL /Store/Details gibi isteyen yerine /Store.aspx/Details gibi bir URL'nı isteyin.

İkinci seçenek olarak adlandırılan bir şeyler oluşturun, bir *joker karakter betik eşlemesi*. Joker karakter betik eşlemesi her istek ASP.NET Framework'e eşleme sağlar.

Web sunucunuza (örneğin, uygulama bir Internet servis sağlayıcısı tarafından barındırılan, ASP.NET MVC) erişimi yoksa ilk seçeneğini kullanmanız gerekir. URL'nizde görünümünü değiştirmek istemediğiniz ve web sunucunuz erişiminiz varsa, ikinci seçeneği kullanabilirsiniz.

Biz her seçenek aşağıdaki bölümlerde ayrıntılı olarak inceleyin.

## <a name="adding-extensions-to-the-route-table"></a>Yol tablosu uzantılar ekleme

ASP.NET IIS eski sürümleri ile çalışmak için yönlendirme almak için en kolay yolu, Global.asax dosyası, rota tablosunda değiştirmektir. Varsayılan ve listeleme 1 değiştirilmemiş Global.asax dosyasında varsayılan yol adlı bir rota yapılandırır.

**1 - (değiştirilmemiş) Global.asax listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

Listeleme 1'de yapılandırılmış varsayılan yol şuna rota URL'lere sağlar:

/ Home/Index

/ Ürün/Ayrıntılar/3

/ Ürün

Ne yazık ki, eski sürümleri IIS, ASP.NET framework bu isteklerini iletmek olmaz. Bu nedenle, bu istekleri bir denetleyiciye yönlendirilmiş olmaz. URL /Home/dizini için bir tarayıcı isteğini yaparsanız, örneğin, ardından hata sayfası Şekil 2'de elde edersiniz.


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Şekil 2**: 404 bulunamadı hatası alma ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))


IIS eski sürümleri, ASP.NET framework yalnızca belirli isteklerini eşler. İstek doğru dosya uzantısına sahip bir URL olması gerekir. Örneğin, bir istek /SomePage.aspx için ASP.NET framework eşlenmiş. Ancak, bir istek /SomePage.htm için desteklemez.

ASP.NET framework eşlenen bir dosya uzantısı içeren bu nedenle, ASP.NET yönlendirmesinin çalışması için almak için biz varsayılan yol değiştirmeniz gerekir.

Bu yapılır adlı bir komut dosyası kullanarak `registermvc.wsf`. ASP.NET MVC 1 sürümde bulunan `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ancak ASP.NET 2 itibariyle bu betiği adresinde ASP.NET Futures taşındı [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

Bu betiği yürüten yeni .mvc uzantısı IIS ile kaydeder. .Mvc uzantısı kaydettikten sonra böylece yolları .mvc bu uzantıyı kullanan yollarınızı Global.asax dosyasında değiştirebilirsiniz.

Listeleme 2 değiştirilmiş Global.asax dosyasında IIS eski sürümleri ile çalışır.

**2 - (uzantılarıyla değiştirilmiş) Global.asax listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


Önemli: ASP.NET MVC Global.asax dosyası değiştirdikten sonra uygulamanızı unutmayın.


Listeleme 2 Global.asax dosyasında yapılan iki önemli değişiklik vardır. Şimdi Global.asax dosyasında tanımlanmış iki yol vardır. Varsayılan yol, ilk rota için URL deseni şimdi şuna benzer:

{controller}.mvc/{action}/{id}

.Mvc uzantısı eklenmesi ASP.NET yönlendirme modülü karşılar dosyaları türünü değiştirir. Bu değişiklikle, ASP.NET MVC uygulamayı şimdi aşağıdaki gibi isteklerini yönlendirir:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

İkinci yol, kök yolu, yeni bir özelliktir. Bu URL deseni kök yolu için boş bir dizedir. Bu yol, uygulamanızın kök karşı yapılan istekleri eşleştirmek için gereklidir. Örneğin, kök yol şuna benzer bir istek eşleşir:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Yol tablosu için bu değişiklikleri yaptıktan sonra tüm bağlantılar, uygulamanızda bu yeni URL modellerle uyumlu olduğundan emin olmak gerekir. Diğer bir deyişle, tüm bağlantılarınızı .mvc uzantısı eklediğinizden emin olun. Ardından, bağlantılar oluşturmak için Html.ActionLink() yardımcı yöntemi kullanırsanız, herhangi bir değişiklik yapmak gerekmez.


Registermvc.wcf betik kullanmak yerine, ASP.NET framework el ile eşlenen IIS için yeni bir uzantı ekleyebilirsiniz. Yeni bir uzantı kendiniz eklerken, onay kutusunu etiketli emin olun **dosyanın var olduğunu doğrula** işaretli.


## <a name="hosted-server"></a>Barındırılan sunucusu

Her zaman, web sunucunuza erişim yok. Bir Internet barındırma sağlayıcısı kullanarak ASP.NET MVC uygulamanızın barındırıyorsanız, örneğin, ardından mutlaka IIS erişiminiz olmaz.

Bu durumda, ASP.NET framework eşlenen varolan dosya uzantılarını birini kullanmanız gerekir. .Aspx, .axd ve .ashx uzantıları ASP.NET ile eşlenmiş dosya uzantılarını örneklerindendir.

Örneğin, listeleme 3 değiştirilmiş Global.asax dosyasında .aspx uzantısı yerine .mvc uzantısını kullanır.

**3 - (.aspx uzantılarıyla değiştirilmiş) Global.asax listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

Listeleme 3 Global.asax dosyasında tam olarak bu .mvc uzantısı yerine .aspx uzantısı kullanır olgu dışında önceki Global.asax dosyası ile aynıdır. Herhangi bir ayar .aspx uzantıyı kullanmak için Uzak web sunucunuzda gerçekleştirmek zorunda değilsiniz.

## <a name="creating-a-wildcard-script-map"></a>Joker karakter betik eşlemesi oluşturma

Ardından, ASP.NET MVC uygulamanızın URL'lerini değiştirmek istediğiniz yok ve web sunucunuz erişiminiz varsa, ek bir seçeneğiniz vardır. ASP.NET framework web sunucusuna tüm istekleri eşleyen bir joker karakter betik eşlemesi oluşturabilirsiniz. Bu şekilde, varsayılan ASP.NET MVC yol tablosu (Klasik modda) IIS 7.0 veya IIS 6.0 ile kullanabilirsiniz.

Bu seçenek IIS web sunucusuna karşı yapılan her isteği müdahale neden olduğunu unutmayın. Bu görüntü, klasik ASP sayfalarını ve HTML sayfaları için istekleri içerir. Bu nedenle, etkinleştirme bir joker karakter betik eşlemesi ASP.NET için performans etkileri sahip.

IIS 7.0 için joker karakter betik eşlemesi etkinleştirmek nasıl aşağıda verilmiştir:

1. Bağlantıları penceresinde uygulamanızı seçin
2. Olduğundan emin olun **özellikleri** Görünüm Seçili
3. Çift **işleyici eşlemeleri** düğmesi
4. Tıklatın **joker karakter betik Eşlemesi Ekle** (bkz: Şekil 3) bağlantı
5. Aspnet yolunu girin\_isapi.dll dosyası (kopyalayabilirsiniz bu yol PageHandlerFactory komut dosyası eşleme)
6. MVC adını girin
7. Tıklatın **Tamam** düğmesi


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Şekil 3**: IIS 7. 0'ile bir joker karakter betik eşlemesi oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))


IIS 6.0 ile bir joker karakter betik eşlemesi oluşturmak için aşağıdaki adımları izleyin:

1. Bir Web sitesini sağ tıklatıp Özellikler'i seçin
2. Seçin **giriş dizini** sekmesi
3. Tıklatın **yapılandırma** düğmesi
4. Seçin **eşlemeleri** sekmesi
5. Tıklatın **Ekle** (bkz: Şekil 4) düğmesine
6. Aspnet yoluna Yapıştır\_(kopyalayabilirsiniz bu yolu .aspx dosyaları için komut dosyası eşlemesinden) yürütülebilir alanına isapi.dll
7. Etiketli onay kutusunun işaretini **dosyanın var olduğunu doğrula**
8. Tıklatın **Tamam** düğmesi


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Şekil 4**: IIS 6.0 ile bir joker karakter betik eşlemesi oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))


Joker karakter betik eşlemeleri etkinleştirdikten sonra bir kök yolu içeren Global.asax dosyasında yol tablosu değiştirmeniz gerekir. Aksi takdirde, uygulamanızın kök sayfa isteğinde bulunduğunda Şekil 5'te hata sayfası elde edersiniz. Listeleme 4 değiştirilmiş Global.asax dosyasında kullanabilirsiniz.


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Şekil 5**: eksik kök yolu hata ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))


**4 - (kök rotası değiştirilmiş) Global.asax listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

IIS 7.0 veya IIS 6.0 için joker karakter betik eşlemesi etkinleştirdikten sonra şuna varsayılan rota tablosu ile iş isteklerini yapabilirsiniz:

/

/ Home/Index

/ Ürün/Ayrıntılar/3

/ Ürün

## <a name="summary"></a>Özet

Bu öğreticinin amacı, IIS (veya IIS 7. 0'Klasik modda) eski bir sürümünü kullanırken, ASP.NET MVC nasıl kullanabileceğinizi açıklamak için oluştu. Biz IIS eski sürümleri ile çalışmak için ASP.NET yönlendirme alma iki yolu ele alınan: varsayılan rota tablosu değiştirin veya bir joker karakter betik eşlemesi oluşturun.

İlk seçenek, ASP.NET MVC uygulamanızda kullanılan URL'leri değiştirmeniz gerekir. Bu ilk seçeneği çok önemli bir avantajı, yol tablosu değiştirmek için bir web sunucusuna erişim gerekmez ' dir. Bu ilk seçeneği bile, ASP.NET MVC uygulamanızın bir Internet ile barındırdığında kullanabileceğiniz Şirket barındırma anlamına gelir.

İkinci seçenek, bir joker karakter betik eşlemesi oluşturmaktır. Bu ikinci seçeneği avantajlarından URL'nizde değişiklik gerekmez ' dir. Bu ikinci seçeneği bir dezavantajı, ASP.NET MVC uygulamanızın performansını etkileyebilir olmasıdır.

>[!div class="step-by-step"]
[Önceki](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
