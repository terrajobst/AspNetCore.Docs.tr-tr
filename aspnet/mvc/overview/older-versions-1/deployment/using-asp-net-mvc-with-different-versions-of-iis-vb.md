---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: ASP.NET MVC farklı (VB) IIS sürümleriyle kullanma | Microsoft Docs
author: microsoft
description: Bu öğreticide, ASP.NET MVC ve URL yönlendirme, Internet Information Services'ın farklı sürümleriyle kullanmayı öğrenin. Farklı stratejiler şunların...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e997f3c7db124869a731b346a4fb3b04072fb0b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370051"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>ASP.NET MVC farklı (VB) IIS sürümleriyle kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu öğreticide, ASP.NET MVC ve URL yönlendirme, Internet Information Services'ın farklı sürümleriyle kullanmayı öğrenin. IIS 7. 0'ı (Klasik mod), IIS 6.0 ve önceki IIS sürümlerinde, ASP.NET MVC kullanarak için farklı stratejiler öğrenin.


ASP.NET MVC çerçevesi, denetleyici eylemleri için tarayıcı istekleri ASP.NET yönlendirmesi üzerinde bağlıdır. ASP.NET yönlendirme avantajlarından yararlanmak için web sunucunuzda ek yapılandırma adımları gerçekleştirmeniz gerekebilir. Internet Information Services (IIS) ve istek işleme modu, uygulamanızın sürümünün tüm bağlıdır.

IIS farklı sürümlerini bir özeti aşağıda verilmiştir:

- IIS 7.0 (tümleşik mod) - ASP.NET yönlendirmesi kullanmak için gerekli özel yapılandırma.
- IIS 7.0 (Klasik mod) - ASP.NET yönlendirmesi kullanan özel bir yapılandırma yapılması gerekir.
- IIS 6.0 veya aşağıda - ASP.NET yönlendirmesi kullanan özel bir yapılandırma için gerçekleştirmeniz gerekir.

En son IIS sürüm 7.5 (Win7) sürümüdür. IIS 7, IIS ile Windows Server 2008 ve VISTA/SP1 bulunan ve daha yüksek. IIS 7.0 üzerinde Vista işletim sistemini Home Basic dışında herhangi bir sürümünü yükleyebilirsiniz (bkz [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7.0, istekleri işlemek için iki modu destekler. Tümleşik modda veya Klasik modda kullanabilirsiniz. IIS 7.0 tümleşik modunda kullanırken hiçbir özel yapılandırma adımlarını gerçekleştirmeniz gerekmez. Bununla birlikte, IIS 7.0, Klasik modunda kullanırken ek yapılandırma gerçekleştirmeniz gerekir.

Microsoft Windows Server 2003, IIS 6.0 içerir. IIS 7.0, Windows Server 2003 işletim sistemi kullanırken IIS 6.0 yükseltemezsiniz. IIS 6. 0'ı kullanırken, ek yapılandırma adımlarını gerçekleştirmeniz gerekir.

Microsoft Windows XP Professional IIS 5.1 içerir. IIS 5.1 kullanırken ek yapılandırma adımlarını gerçekleştirmeniz gerekir.

Son olarak, Microsoft Windows 2000 ve Microsoft Windows 2000 Professional, IIS 5.0 içerir. IIS 5.0 kullanırken ek yapılandırma adımlarını gerçekleştirmeniz gerekir.

## <a name="integrated-versus-classic-mode"></a>Klasik mod tümleşik

IIS 7.0, iki farklı istek işleme modunu kullanarak istekleri işleyebilir: tümleşik ve klasik. Tümleşik modu, daha iyi performans ve daha fazla özellik sağlar. Klasik mod dahil için geriye dönük uyumluluk önceki IIS sürümleriyle.

İstek işleme modunu uygulama havuzu tarafından belirlenir. Uygulama ile ilişkili uygulama havuzunun belirleyerek, hangi işleme modunu belirli web uygulaması tarafından kullanılıyor belirleyebilirsiniz. Aşağıdaki adımları uygulayın:

1. Internet Information Services Manager'ı Başlat
2. Bağlantıları penceresinde bir uygulama seçin
3. Eylemler pencerede **temel ayarları** uygulamasını Düzenle iletişim kutusunu açmak için bağlantıyı (bkz. Şekil 1) kutusunda
4. Seçili uygulama havuzunu not alın.

Varsayılan olarak, iki uygulama havuzlarını desteklemek için IIS yapılandırılır: **DefaultAppPool** ve **Classic .NET AppPool**. Ardından DefaultAppPool seçtiyseniz, uygulamanızın tümleşik istek işleme modunda çalışıyor. Classic .NET AppPool seçtiyseniz, uygulamanızın Klasik istek işleme modunda çalışıyor.


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Şekil 1**: istek işleme modunu algılama ([tam boyutlu görüntüyü görmek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))


Uygulamayı Düzenle iletişim kutusu içinde istek işleme modunu değiştirebileceğiniz dikkat edin. Seç düğmesini tıklayın ve uygulama ile ilişkili uygulama havuzunu Değiştir. Bir ASP.NET uygulaması için tümleşik modu Klasikten değiştirilirken uyumluluk sorunları olduğunu unutmayın. Daha fazla bilgi için aşağıdaki makalelere bakın:

- Windows Vista ve Windows Server 2008 IIS 7.0 ASP.NET 1.1 yükseltme-- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- IIS 7.0 ile ASP.NET tümleştirme- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)


Bir ASP.NET uygulaması DefaultAppPool kullanıyorsanız, ASP.NET yönlendirmesi (ve bu nedenle ASP.NET MVC) çalışmaya almak için herhangi bir ilave adımı gerçekleştirmeniz gerekmez. ASP.NET uygulaması okumaya devam edin Classic .NET AppPool kullanmak için yapılandırılmışsa, ancak yapmak için daha fazla iş vardır.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>ASP.NET MVC eski IIS sürümleriyle kullanma

ASP.NET MVC ile IIS IIS 7. 0'dan daha eski bir sürümünü kullanmanız gerekir veya IIS 7.0, Klasik modunda kullanmak gerekirse, iki seçeneğiniz vardır. İlk olarak, dosya uzantıları kullanmak için rota tablosu değiştirebilirsiniz. Örneğin, /Store/Details gibi bir URL isteyen yerine /Store.aspx/Details gibi bir URL'nı isteyin.

İkinci seçenek olarak adlandırılan bir şeyler oluşturun, bir *joker karakter betik eşlemesi*. Bir joker karakter betik eşlemesi ASP.NET framework her istek eşlemenizi sağlar.

Web sunucunuzu (örneğin, bir Internet hizmet sağlayıcısı tarafından barındırılan uygulama, ASP.NET MVC) erişiminiz yoksa ilk seçeneği kullanmanız gerekir. Ardından, URL'leri değiştirme istemediğiniz ve web sunucunuzun erişiminiz varsa, ikinci seçeneği kullanabilirsiniz.

Her seçenek aşağıdaki bölümlerde ayrıntılı olarak inceleyeceğiz.

## <a name="adding-extensions-to-the-route-table"></a>Rota tablosunu uzantılar ekleme

IIS eski sürümleriyle çalışmaya ASP.NET yönlendirmesi almak için en kolay yolu, yol tablosu Global.asax dosyasındaki değiştirmektir. Varsayılan ve listeleme 1 değiştirilmemiş Global.asax dosyasında varsayılan yol adlı bir rota yapılandırır.

**1 - Global.asax (üzerinde değişiklik yapılmadan) listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

Listeleme 1'de yapılandırılmış varsayılan yol şöyle rota URL'lere sağlar:

/ Home/Index

/ Ürün/Ayrıntılar/3

/ Ürün

Ne yazık ki, IIS eski sürümleri bu istekleri geçirin ve bu da ASP.NET framework olmaz. Bu nedenle, bu istekleri bir denetleyiciye yönlendirilir olmaz. URL /Home/dizin için bir tarayıcı isteğini yaparsanız, ardından hata sayfası Şekil 2'de elde edersiniz.


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Şekil 2**: 404 bulunamadı hatası alma ([tam boyutlu görüntüyü görmek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))


IIS eski sürümleri, ASP.NET framework yalnızca belirli isteklerini eşleyin. İstek için bir URL doğru dosya uzantısına sahip olmalıdır. Örneğin, ASP.NET framework /SomePage.aspx isteği eşlenen. Ancak, bir istek için /SomePage.htm desteklemez.

ASP.NET framework eşlenmiş bir dosya uzantısı içerdiği için bu nedenle, çalışması için ASP.NET yönlendirmesi almak için şu varsayılan yol değiştirmeniz gerekir.

Bu yapılır adlı bir komut dosyası kullanarak `registermvc.wsf`. ASP.NET MVC 1 sürümde bulunan `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ancak ASP.NET 2'den itibaren bu betiği, kullanılabilir ASP.NET Futures taşındı [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Bu betiği yürüten yeni .mvc uzantısı ile IIS kaydeder. .Mvc uzantısı kaydettikten sonra yolları .mvc uzantısı kullanmasını sağlayarak Global.asax dosyasındaki yollarınızı değiştirebilirsiniz.

Listeleme 2 değiştirilmiş Global.asax dosyasında önceki IIS sürümleriyle çalışır.

**2 - Global.asax (değiştirilmiş uzantılı) listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


Önemli: Global.asax dosyası değiştirildikten sonra ASP.NET MVC uygulamanızı oluşturmak unutmayın.


2 listeleme Global.asax dosyasında yapılan iki önemli değişiklik daha vardır. Artık Global.asax dosyasında tanımlanmış iki yol vardır. Varsayılan yol, ilk rota için URL deseni şimdi şuna benzer:

{controller}.mvc/{action}/{id}

ASP.NET yönlendirme modülü karşılar dosya türlerini .mvc uzantısı eklenmesini değiştirir. Bu değişiklik, ASP.NET MVC uygulaması artık aşağıdaki gibi istekleri yönlendirir:

/Home.MVC/Index/

/Product.MVC/details/3

/Product.MVC/

İkinci rota, kök yolu, yeni bir özelliktir. Bu URL deseni için kök yolu boş bir dizedir. Bu yol, uygulamanızın kök karşı yapılan istekleri eşleştirmek için gereklidir. Örneğin, kök yol şuna benzer bir istek eşleşir:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Bu, rota tablosu değişiklikleri yaptıktan sonra tüm bağlantılar, uygulamanızda bu yeni URL desenleri ile uyumlu olduğundan emin olmak gerekir. Diğer bir deyişle, tüm bağlantılarınızı .mvc uzantısı eklediğinizden emin olun. Ardından, bağlantı oluşturmak üzere Html.ActionLink() yardımcı yöntemini kullanırsanız, herhangi bir değişiklik gerekmez.


Registermvc.wcf betik kullanmak yerine, yeni bir uzantı için ASP.NET framework el ile eşlenmiş IIS ekleyebilirsiniz. Yeni bir uzantı kendiniz eklerken, onay kutusunu etiketli olduğunu emin **dosyanın varolduğunu doğrulayın** yapılmaz.


## <a name="hosted-server"></a>Barındırılan sunucusu

Her zaman, web sunucusuna erişiminiz yok. Bir Internet barındırma sağlayıcısı kullanarak ASP.NET MVC uygulamanızın barındırıyorsanız, örneğin, ardından mutlaka IIS erişemezsiniz.

Bu durumda, ASP.NET framework eşlenmiş var olan dosya uzantılarını birini kullanmanız gerekir. ASP.NET ile eşlenmiş dosya uzantılarının örnekleri, .aspx, .axd ve .ashx uzantıları içerir.

Örneğin, listeleme 3'te değiştirilmiş Global.asax dosyası yerine .mvc uzantısı .aspx uzantısını kullanır.

**3 - Global.asax (.aspx uzantılarıyla değiştirilmiş) listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

3 listeleme Global.asax dosyasında tam olarak bunu yerine .mvc uzantısı .aspx uzantı kullanır olgu dışında önceki Global.asax dosyası ile aynıdır. .Aspx uzantıyı kullanmak için Uzak web sunucunuz üzerinde herhangi bir ayar gerçekleştirmeniz gerekmez.

## <a name="creating-a-wildcard-script-map"></a>Joker karakter betik eşlemesi oluşturma

ASP.NET MVC uygulamanızın URL'lerini değiştirmek istemediğiniz ve web sunucunuzun erişiminiz varsa, ek bir seçeneğiiz vardır. ASP.NET framework web sunucusuna tüm istekleri eşleyen bir joker karakter betik eşlemesi oluşturabilirsiniz. Bu sayede, varsayılan ASP.NET MVC yönlendirme tablosu (Klasik modda) IIS 7.0 veya IIS 6.0 ile kullanabilirsiniz.

Bu seçenek IIS web sunucusuna karşı yapılan her isteği kesmeye neden olduğunu unutmayın. Bu görüntü, klasik ASP sayfaları ve HTML sayfaları için istekleri içerir. Bu nedenle, etkinleştirme bir joker karakter betik eşlemesi ASP.NET'e performans etkilerinin sahip.

Nasıl bir joker karakter betik eşlemesi için IIS 7.0 etkinleştirmek şu şekildedir:

1. Bağlantıları penceresinde uygulamanızı seçin
2. Emin olun **özellikleri** Görünüm Seçili
3. Çift **işleyici eşlemeleri** düğmesi
4. Tıklayın **joker karakter betik Eşlemesi Ekle** (bkz: Şekil 3) bağlayın
5. ASP.NET için yolu girin\_isapi.dll dosyası (, kopyalayabilir bu yolu PageHandlerFactory betik eşleme)
6. MVC adını girin
7. Tıklayın **Tamam** düğmesi


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Şekil 3**: IIS 7. 0'ile bir joker karakter betik eşlemesi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))


IIS 6.0 ile birlikte bir joker karakter betik eşlemesi oluşturmak için aşağıdaki adımları izleyin:

1. Bir Web sitesini sağ tıklatıp Özellikler'i seçin
2. Seçin **giriş dizini** sekmesi
3. Tıklayın **yapılandırma** düğmesi
4. Seçin **eşlemeleri** sekmesi
5. Tıklayın **Ekle** (bkz: Şekil 4) düğmesi
6. ASP.NET yolu yapıştırın\_isapi.dll yürütülebilir alanına (, kopyalayabilir bu yolu .aspx dosyaları için betik eşlemesinden)
7. Etiketli onay kutusunun işaretini kaldırın **osyanın var olduğunu doğrula**
8. Tıklayın **Tamam** düğmesi


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Şekil 4**: IIS 6. 0'ile bir joker karakter betik eşlemesi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))


Joker karakter betik eşlemeleri etkinleştirdikten sonra bir kök yolu içeren bir yol tablosu Global.asax dosyasındaki değiştirmeniz gerekir. Aksi takdirde, uygulamanızın kök sayfa isteğinde bulunduğunda Şekil 5'te hata sayfası alırsınız. Listeleme 4'te değiştirilmiş Global.asax dosyası kullanabilirsiniz.


[![Yeni Proje iletişim kutusu](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Şekil 5**: eksik kök yol hata ([tam boyutlu görüntüyü görmek için tıklatın](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))


**4 - Global.asax (kök yol ile değiştirilmiş) listeleme**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

IIS 7.0 veya IIS 6.0 için joker karakter betik eşlemesi etkinleştirdikten sonra şuna varsayılan rota tablosu ile çalışan istekleri yapabilir:

/

/ Home/Index

/ Ürün/Ayrıntılar/3

/ Ürün

## <a name="summary"></a>Özet

Bu öğreticinin amacı, IIS (veya Klasik modda IIS 7.0) daha eski bir sürümünü kullanırken, ASP.NET MVC nasıl kullanabileceğiniz açıklayacak şekilde oluştu. Önceki IIS sürümleriyle çalışmaya ASP.NET yönlendirmesi almak için iki yöntem ele almıştık: varsayılan rota tablosu değiştirmek veya bir joker karakter betik eşlemesi oluşturun.

İlk seçenek, ASP.NET MVC uygulamanızda kullanılan URL'leri değiştirmeniz gerekir. Bu ilk seçeneği çok önemli bir avantajı, rota tablosunu değiştirmek için bir web sunucusuna erişimi gerekmez ' dir. Bu ilk seçenek bile, ASP.NET MVC uygulamanızın Internet ile barındırırken kullanabileceğiniz barındırma şirketi anlamına gelir.

İkinci seçenek, bir joker karakter betik eşlemesi oluşturmaktır. Bu ikinci seçeneği avantajlarından URL'nizde değiştirmek gerekmez ' dir. Bu ikinci seçeneği bir dezavantajı, ASP.NET MVC uygulamanızın performansını etkileyebilir olmasıdır.

> [!div class="step-by-step"]
> [Önceki](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
