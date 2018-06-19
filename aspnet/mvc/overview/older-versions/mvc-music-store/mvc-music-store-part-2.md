---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: '2. Kısım: Denetleyicileri | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 2. Bölüm denetleyicileri kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878704"
---
<a name="part-2-controllers"></a>Bölüm 2: denetleyicileri
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 2. Bölüm denetleyicileri kapsar.


Geleneksel web çerçeveleri ile gelen URL'ler genellikle disk üzerindeki dosyalara eşlenir. Örneğin: bir URL için bir istek ister "/ Products.aspx" veya "/ Products.php" "Products.aspx" veya "Products.php" dosyası tarafından işlenen.

Web tabanlı MVC çerçeveleri URL'ler için sunucu kodu biraz farklı bir şekilde eşleyin. Gelen URL'ler için dosya eşleme yerine bunların yerine URL'leri sınıfları yöntemlere eşlenir. Bu sınıfların "Denetleyicileri" adı verilir ve kullanıcı girişini işleme gelen HTTP isteklerini işlemekten sorumlu, alma ve verilerini kaydetme ve gönderilecek yanıt belirleme geri istemciye (HTML görüntülemek, dosya indirme, farklı bir'yeniden yönlendirme URL, vb.).

## <a name="adding-a-homecontroller"></a>Bir HomeController ekleme

Biz MVC müzik deposu uygulamamız URL'leri sitemizi giriş sayfasına işleyecek denetleyici sınıfını ekleyerek başlarsınız. Biz, ASP.NET MVC, varsayılan adlandırma kurallarına uygun ve HomeController çağırın.

Çözüm Gezgini içinde "Denetleyicileri" klasörü sağ tıklatın ve "Ekle" ve "Denetleyici..." seçin komut:

![](mvc-music-store-part-2/_static/image1.jpg)

Bu "Denetleyici Ekle" iletişim kutusunu getirir. "HomeController" Denetleyici adı ve Ekle düğmesine basın.

![](mvc-music-store-part-2/_static/image1.png)

Aşağıdaki kod ile bu HomeController.cs, yeni bir dosya oluşturur:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Olabildiğince basit bir şekilde başlamak için şimdi dizin yöntemi yalnızca bir dize döndürür basit bir yöntem ile değiştirin. İki değişiklik vermiyoruz:

- ActionResult yerine bir dize döndürecek şekilde yöntemini değiştirme
- "Hello gelen giriş" döndürmek için dönüş ifadesini değiştirin

Yöntem gibi görünmelidir:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Şimdi Şimdi siteyi çalıştırın. Biz bizim web sunucusu başlatır ve aşağıdakilerden herhangi birini kullanarak site deneyin::

- Hata ayıklama ⇨ hata ayıklamayı Başlat menü öğesini seçin
- Araç çubuğundaki yeşil ok düğmesine tıklayın ![](mvc-music-store-part-2/_static/image2.jpg)
- F5 klavye kısayolunu kullanın.

Yukarıdaki adımları kullanarak Projemizin derleyin ve ardından yerleşik-Visual Web başlatmak için geliştiricinin ASP.NET Geliştirme Sunucusu neden. Bir bildirim ASP.NET Geliştirme Sunucusu başlatıldığını belirtmek için ekranın alt köşedeki görünür ve bağlantı noktası numarasını altında çalıştığını gösterir.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer, bizim web sunucusu URL'si gösteren bir tarayıcı penceresi otomatik olarak açılır. Bu, bize web uygulamamız hızlı şekilde denemenize yönelik izin verir:

![](mvc-music-store-part-2/_static/image3.png)

Yeni bir Web sitesi oluşturduğumuz oldukça hızlı –, Tamam, üç satır işlev eklenir ve metin tarayıcıda olduğuna. Bilim Doğum değil, ancak bir başlangıç değil.

*Not: Visual Web Developer Web sitenizi bir rastgele ücretsiz "port" numaralı çalışacak ASP.NET Geliştirme Sunucusu içerir. Yukarıdaki ekran görüntüsünde, site konumunda çalışan `http://localhost:26641/`, bağlantı noktası 26641 kullanıyor. Bağlantı noktası numarası farklı olacaktır. Biz bu öğreticide URL'SİNİN LIKE /Store/Browse hakkında konuşurken, sonra bağlantı noktası numarası geçer. Bir bağlantı noktası numarasını 26641 varsayıldığında, / deposu/için Gözat gözatma için gözatma anlamına gelir `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Bir StoreController ekleme

Sitemizi giriş sayfasının uygulayan basit bir HomeController eklediğimiz. Şimdi bizim müzik deposu gözatma işlevlerini uygulamak için kullanacağız başka bir denetleyici ekleyelim. Bizim depolama denetleyicisi üç senaryoları destekler:

- Bizim müzik deposundaki müzik türler listeleme sayfası
- Tüm belirli bir tarzını müzik albümleri listeleyen bir Gözat sayfası
- Belirli Müzik albüm hakkındaki bilgileri gösterir Ayrıntılar sayfası

Yeni bir StoreController sınıf ekleyerek başlayacağız.. Henüz yapmadıysanız, uygulama tarayıcının kapanması veya hata ayıklama ⇨ durdurma hata ayıklama menü öğesi seçilerek çalışmayı durdurur.

Şimdi yeni StoreController ekleyin. İle HomeController yaptığımız gibi Biz bu Çözüm Gezgini içinde "Denetleyicileri" klasöre sağ tıklayarak ve Add - seçme gerçekleştirirsiniz&gt;denetleyicisi menü öğesi

![](mvc-music-store-part-2/_static/image4.png)

"Dizin" yöntemi, yeni StoreController zaten var. Bizim müzik deposu içindeki tüm türler listeler listeleme sayfamızı uygulamak için bu "Dizin" yöntem kullanacağız. İşlenecek bizim StoreController istiyoruz iki diğer senaryolar uygulamak için iki ek yöntemleri de ekleyeceğiz: göz atma ve ayrıntıları.

Bu yöntemler (dizin, Gözat ve ayrıntıları) Denetleyicimizin içinde "Denetleyici eylemleri" olarak adlandırılır ve HomeController.Index () eylem yöntemiyle zaten gördüğünüz gibi kendi URL isteklerine yanıt vermek ve (genel olarak bakıldığında) hangi içerik belirlemek için iş Tarayıcı veya URL çağrılan kullanıcı için gönderilmelidir.

Bizim StoreController uygulama theIndex() yöntemi "Merhaba gelen Store.Index()" dize döndürecek şekilde değiştirerek alacağız ve Browse() ve Details() için benzer yöntemler ekleyeceğiz:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Projeyi tekrar çalıştırın ve aşağıdaki URL'ler göz atın:

- / Deposu
- / Deposu/Gözat
- / Deposu/ayrıntıları

Bu URL'lerine erişme, denetleyici eylem yöntemlerinde çağırma ve dize yanıtlar:

![](mvc-music-store-part-2/_static/image5.png)

Harika olan, ancak bunlar yalnızca sabit dizelerdir. URL'den bilgi alın ve sayfa çıktısında görüntülemek için bunları dinamik olalım.

İlk biz URL'den bir sorgu dizesi değerini almak için Gözat eylem yöntemi değiştireceksiniz. Biz, bizim eylem yöntemi için bir "Tarz" parametresini ekleyerek yapabilirsiniz. Biz bunu yaparken ASP.NET MVC çalıştırıldığında bizim eylem yöntemine "Tarz" adlı bir sorgu dizesi veya form post parametreleri otomatik olarak geçer.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Not: Biz HttpUtility.HtmlEncode yardımcı program yöntemi kullanıcı girişini temizlenmeye için kullanmakta olduğunuz. Bizim görünüme Javascript injecting /Store/Browse gibi bir bağlantıyla birlikte gelen engel olur? Tarz =&lt;betik&gt;window.location='http://hackersite.com'&lt;/script&gt;.*

Şimdi Şimdi Gözat/deposu/için Gözat? Tarz DISCO =

![](mvc-music-store-part-2/_static/image6.png)

İleri okuma ve kimliği adlı bir giriş parametresi görüntülemek için Ayrıntılar eylemi değiştirelim Önceki bizim yöntemini farklı olarak, biz kimliği değeri bir sorgu dizesi parametresi olarak katıştırma olmaz. Bunun yerine biz bunu doğrudan URL içinde kendisi katıştırmak. Örneğin: /Store/Details/5.

ASP.NET MVC bize kolayca herhangi bir şey yapılandırmak zorunda kalmadan bunu sağlar. ASP.NET MVC'ın varsayılan yönlendirme URL kesimi sonra eylem yöntemi adı "ID" adlı bir parametre işlemek için kuraldır. Eylem yöntemi kimliği adlı bir parametre varsa sonra ASP.NET MVC otomatik olarak URL kesimini için parametre olarak geçirir.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Uygulamayı çalıştırın ve /Store/Details/5 için göz atın:

![](mvc-music-store-part-2/_static/image7.png)

Şimdi ne biz kadarki yaptıktan olduðunu:

- Visual Web Developer kullanarak yeni bir ASP.NET MVC proje oluşturduk
- Bir ASP.NET MVC uygulaması temel klasör yapısını ele
- Biz bizim Web ASP.NET geliştirme sunucusu kullanarak çalıştırma öğrendiniz
- İki denetleyici sınıf oluşturduk: bir HomeController ve bir StoreController
- Eylem yöntemleri URL isteklerine yanıt vermek ve metin tarayıcıya dönün bizim denetleyicilerine ekledik


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-1.md)
> [sonraki](mvc-music-store-part-3.md)
