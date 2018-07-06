---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: '2. Bölüm: Denetleyicileri | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 2 denetleyicileri kapsar.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841101"
---
<a name="part-2-controllers"></a>2. Bölüm: denetleyicileri
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 2 denetleyicileri kapsar.


Geleneksel web çerçeveleri ile gelen URL'ler diskteki dosyaları genellikle eşlenir. Örneğin: bir URL isteği ister "/ Products.aspx" veya "/ Products.php" bir "Products.aspx" veya "Products.php" dosyası tarafından işlenir.

Web tabanlı MVC çerçeveleri URL'leri için sunucu kodu biraz farklı bir şekilde eşleştirin. Gelen URL'ler için dosya eşleme yerine bunların yerine URL'leri sınıfları yöntemlerde eşleyin. Bu sınıfların "Denetleyicileri" olarak adlandırılır ve kullanıcı girişini işleme gelen HTTP isteklerini işlemekten sorumlu oldukları, alma ve verilerini kaydetme ve gönderilecek yanıt belirleyen yedekleme istemciye (HTML görüntülemek, dosya indirme, farklı bir'yeniden yönlendirme URL, vb.).

## <a name="adding-a-homecontroller"></a>Bir HomeController ekleme

Biz, MVC müzik Store uygulamamız URL'leri sitemizi giriş sayfasına işleyecek bir denetleyici sınıfı ekleyerek başlarsınız. Biz, ASP.NET MVC varsayılan adlandırma kurallarına uygun ve HomeController çağırın.

Çözüm Gezgini içinde "Denetleyicileri" klasörü sağ tıklatın ve "Ekle" ve "Denetleyici..." komutunu seçin:

![](mvc-music-store-part-2/_static/image1.jpg)

Bu, "Denetleyici Ekle" iletişim kutusu getirir. Denetleyici "HomeController" olarak adlandırın ve Ekle düğmesine basın.

![](mvc-music-store-part-2/_static/image1.png)

Bu, aşağıdaki kod ile HomeController.cs, yeni bir dosya oluşturur:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Olabildiğince basit bir şekilde başlamak için şimdi dizin yöntemi yalnızca bir dize döndüren basit bir yöntem ile değiştirin. İki değişiklik oluşturacağız:

- ActionResult yerine bir dize döndürecek şekilde yöntemini değiştirme
- "Hello gelen giriş" döndürülecek dönüş deyimi değiştirin

Yöntemi gibi görünmelidir:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Artık site çalıştıralım. Biz de bizim web sunucusunu başlatmak ve aşağıdakilerden birini kullanarak site deneyin:

- Hata ayıklama ⇨ hata ayıklamayı Başlat menü öğesini seçin
- Araç çubuğundaki yeşil ok düğmesine tıklayın ![](mvc-music-store-part-2/_static/image2.jpg)
- F5 klavye kısayolunu kullanın.

Yukarıdaki adımları kullanarak projemizdeki derleyin ve ardından yerleşik-görsel Web başlatmak için geliştiricinin ASP.NET Geliştirme Sunucusu neden. Bir bildirim ASP.NET Geliştirme Sunucusu başlatıldıktan belirtmek için ekranın alt köşesinde görünür ve bağlantı noktası numarasını altında çalışır olduğunu gösterir.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer, bizim web sunucusu URL'si gösteren bir tarayıcı penceresi otomatik olarak açılır. Bu web uygulamamıza'ı hızlıca denemeniz bize izin verir:

![](mvc-music-store-part-2/_static/image3.png)

Yeni bir Web sitesi oluşturduğumuz oldukça hızlı –, Tamam, bir üç satır içi işlev eklendi ve metin bir tarayıcıda yapılandırdığımıza göre. Bilimi Doğum değil, ancak bir başlangıçtır.

*Not: Visual Web Developer sitenizi rastgele ücretsiz "bağlantı" noktalarında çalışır ASP.NET Geliştirme Sunucusu içerir. Yukarıdaki ekran görüntüsünde, sitesinin çalışır durumda `http://localhost:26641/`, bağlantı noktası 26641 kullanıyor. Bağlantı noktası numaranızı farklı olacaktır. Biz bu öğreticide URL'leri gibi /Store/Browse hakkında konuşurken, bağlantı noktası numarasından sonra geçer. Bir bağlantı noktası numarasını 26641 varsayıldığında, tarama/Store/dizinine göz atma göz anlamına gelir `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Bir StoreController ekleme

Sitemizi ana sayfası uygulayan bir basit HomeController ekledik. Artık bizim müzik deposu gözatma işlevselliğini uygulamak için kullanacağız başka bir denetleyici ekleyelim. Deposu denetleyicimizin üç senaryoları destekler:

- Bizim müzik deposu içinde müzik türleri listesi sayfası
- Tüm müzik albümleri, belirli bir türe listeleyen bir göz atma sayfasından
- Belirli bir müzik Albüm hakkında bilgi gösteren Ayrıntılar sayfası

Yeni bir StoreController sınıf ekleyerek başlayacağız.. Henüz yapmadıysanız, uygulama tarayıcının kapanması veya hata ayıklama ⇨ hata ayıklamayı Durdur menü öğesi seçilerek çalışmıyor.

Şimdi yeni StoreController ekleyin. İle HomeController yaptığımız gibi Çözüm Gezgini içinde "Denetleyicileri" klasöre sağ tıklayarak ve Add - seçerek bunu&gt;denetleyicisi menü öğesi

![](mvc-music-store-part-2/_static/image4.png)

Yeni sunduğumuz StoreController, "Index" yöntemi zaten var. Bizim müzik deposu içindeki tüm türleri listeler listeleme sayfamızı uygulamak için bu "Index" yöntem kullanacağız. İşlemek için sunduğumuz StoreController istiyoruz iki diğer senaryolar uygulamak için iki ek yöntem de ekleyeceğiz: göz atma ve ayrıntıları.

Bu yöntemler (dizin, göz atma ve ayrıntıları) Denetleyicimizin içinde "Denetleyici eylemlerini" olarak adlandırılır ve HomeController.Index () eylem yöntemine önceden gördüğünüz gibi kendi URL taleplerine yanıt vereceğini ve içeriği (genel olarak bakıldığında) belirlemek için iş Tarayıcı veya URL çağrılan kullanıcı geri gönderilmesi.

StoreController kararlılığımızın theIndex() yöntemi "Hello gelen Store.Index()" dize döndürecek şekilde değiştirerek başlayacağız ve Browse() ve Details() için benzer yöntemler ekleyeceğiz:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Projeyi yeniden çalıştırın ve aşağıdaki URL'ler göz atın:

- / Store
- / Store/Gözat
- / Store/ayrıntıları

Bu URL'lerine erişme denetleyici eylem yöntemlerinde çağırmak ve dize yanıtlarını döndürmek:

![](mvc-music-store-part-2/_static/image5.png)

Bu harika bir deneyimdir ancak bunlar yalnızca sabit dizelerdir. URL'den bilgi alın ve içinde sayfa çıktısını görüntülemek için bunları dinamik olalım.

URL bir sorgu dizesi değerini almak için göz atma eylem yönteminin ilk değiştireceğiz. "Tarzı" parametresi bizim eylem yöntemine ekleyerek bunu. Bunu, ASP.NET MVC çağrıldığında bizim eylem yöntemine "tarzı" adlı bir sorgu dizesi veya form gönderi parametresi otomatik olarak geçer.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Not: Kullanıcı girişi temizleyin için HttpUtility.HtmlEncode yardımcı yöntem kullanıyoruz. Bizim görünüme Javascript ekleme /Store/Browse gibi bir bağlantı içeren gelen engel olur? Tarz =&lt;betik&gt;window.location='http://hackersite.com'&lt;/SCRIPT&gt;.*

Şimdi Şimdi Gözat/Store/dizinine göz atma? Tarz DISCO =

![](mvc-music-store-part-2/_static/image6.png)

Sonraki okuma ve kimliğine adlı giriş parametresi görüntülemek için Ayrıntılar eylemi değiştirelim Bizim önceki yöntem, biz kimlik değerini bir sorgu dizesi parametresi olarak ekleme gerekmez. Bunun yerine biz bunu doğrudan URL içinde kendisi ekleyeceğiz. Örneğin: /Store/Details/5.

ASP.NET MVC herhangi bir şey yapılandırmak zorunda kalmadan kolayca bunun olanak tanır. ASP.NET MVC'nin varsayılan yönlendirme bir URL kesimi eylem yöntem adından sonra "ID" adlı bir parametre olarak değerlendirilecek kuralıdır. Eylem yönteminizi kimliği adlı bir parametreye sahipse sonra ASP.NET MVC otomatik olarak URL kesimi, parametre olarak geçirir.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Uygulamayı çalıştırmak ve /Store/Details/5 için göz atın:

![](mvc-music-store-part-2/_static/image7.png)

Şimdi şu ana kadar yaptıklarımızı özeti:

- Visual Web Developer ile yeni bir ASP.NET MVC projesi oluşturduk
- Bir ASP.NET MVC uygulaması temel bir klasör yapısını Bahsettiğimiz
- ASP.NET geliştirme sunucusu kullanarak sitemizin çalıştırma öğrendik.
- İki denetleyici sınıflarına oluşturduk: bir HomeController ve bir StoreController
- Eylem yöntemleri için URL taleplerine yanıt vereceğini ve tarayıcıya dönüş metni bizim denetleyicileri ekledik


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-1.md)
> [İleri](mvc-music-store-part-3.md)
