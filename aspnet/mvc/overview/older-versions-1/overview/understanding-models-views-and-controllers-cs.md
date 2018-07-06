---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Anlama modelleri, görünümleri ve denetleyicileri (C#) | Microsoft Docs
author: StephenWalther
description: Modelleri, görünümleri ve denetleyicileri hakkında yanıltıcı? Bu öğreticide, Stephen Walther bir ASP.NET MVC uygulaması için farklı parçalarını tanıtır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: f5cf39e4652a3e487dcd79b2cf887efb992c2107
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376915"
---
<a name="understanding-models-views-and-controllers-c"></a>Anlama modelleri, görünümleri ve denetleyicileri (C#)
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Modelleri, görünümleri ve denetleyicileri hakkında yanıltıcı? Bu öğreticide, Stephen Walther bir ASP.NET MVC uygulaması için farklı parçalarını tanıtır.


Bu öğreticide modelleri, görünümleri ve denetleyicileri ile ASP.NET MVC üst düzey bir bakış sağlar. Diğer bir deyişle, M açıklar ', V' ve C' ASP.NET mvc'de.

Bu öğreticide okuduktan sonra bir ASP.NET MVC uygulaması farklı bölümlerini birlikte nasıl çalıştığını anlamanız gerekir. Ayrıca, bir ASP.NET MVC uygulaması mimarisi bir ASP.NET Web Forms uygulaması ya da Active Server Pages uygulaması farkı anlamanız gerekir.

## <a name="the-sample-aspnet-mvc-application"></a>Örnek ASP.NET MVC uygulaması

ASP.NET MVC Web uygulamaları oluşturmaya yönelik varsayılan Visual Studio şablon bir ASP.NET MVC uygulaması farklı bölümlerini anlamak için kullanılabilir son derece basit bir örnek uygulamanın içerir. Biz bu Öğreticide bu basit uygulama yararlanın.

Visual Studio 2008 başlatarak MVC şablonu ile yeni bir ASP.NET MVC uygulaması oluşturma ve dosya menü seçeneğini seçerek, yeni proje (bkz. Şekil 1). Yeni Proje iletişim kutusunda proje türleri (Visual Basic veya C#) altında en sevdiğiniz programlama dili seçip **ASP.NET MVC Web uygulaması** şablonlar altında. Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Şekil 01**: Yeni Proje iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](understanding-models-views-and-controllers-cs/_static/image2.png))


Yeni bir ASP.NET MVC uygulaması oluşturduğunuzda **birim testi projesi oluşturma** iletişim kutusu görünür (bkz: Şekil 2). Bu iletişim kutusu çözümünüzü ASP.NET MVC uygulamanızı test etme için ayrı bir proje oluşturmanıza olanak sağlar. Seçeneğini **Hayır, birim testi projesi oluşturma** tıklatıp **Tamam** düğmesi.


[![Birim testi oluştur iletişim kutusu](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Şekil 02**: birim testi oluşturma iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](understanding-models-views-and-controllers-cs/_static/image4.png))


Sonra yeni ASP.NET MVC uygulaması oluşturulur. Birkaç klasörleri ve dosyaları Çözüm Gezgini penceresinde görürsünüz. Özellikle, modelleri, görünümleri ve denetleyicileri adlı üç klasör görürsünüz. Klasör adları tahmin gibi bu klasörleri modelleri, görünümleri ve denetleyicileri uygulamak için dosyaları içerir.

Denetleyicileri klasörünü genişletin, AccountController.cs adlı bir dosya ve HomeController.cs adlı bir dosya görmeniz gerekir. Görünüm klasörü genişletin, hesap, giriş ve paylaşılan adlı üç alt görmeniz gerekir. Giriş klasörünü genişletin, About.aspx ve Index.aspx (bkz: Şekil 3) adlı iki ek dosya görürsünüz. Bu dosyalar varsayılan ASP.NET MVC şablonu dahil örnek uygulaması oluşturur.


[![Çözüm Gezgini penceresi](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Şekil 03**: Çözüm Gezgini penceresi ([tam boyutlu görüntüyü görmek için tıklatın](understanding-models-views-and-controllers-cs/_static/image6.png))


Örnek uygulama menüsü seçeneği belirleyerek çalıştırabileceğiniz **hata ayıklama, hata ayıklamayı Başlat**. Alternatif olarak, F5 tuşuna basabilirsiniz.

Bir ASP.NET uygulamasını ilk kez çalıştırdığınızda, hata ayıklama modunu etkinleştirmenizi önerir Şekil 4'te iletişim kutusu görüntülenir. Tamam düğmesine tıklayın ve uygulama çalışır.


[![Hata ayıklama etkin değil iletişim kutusu](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Şekil 04**: hata ayıklama etkin iletişim ([tam boyutlu görüntüyü görmek için tıklatın](understanding-models-views-and-controllers-cs/_static/image8.png))


Bir ASP.NET MVC uygulamasını çalıştırdığınızda, Visual Studio uygulamayı web tarayıcınızda başlatır. Örnek uygulama yalnızca iki sayfa içerir: dizin sayfası ve hakkında sayfası. Uygulamayı ilk kez başlatıldığında, (bkz: Şekil 5) dizin sayfası görüntülenir. Üstteki menü bağlantıya tıklayarak hakkında sayfasına gidebilirsiniz uygulama sağında.


[![Dizin Sayfası](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Şekil 05**: dizin sayfası ([tam boyutlu görüntüyü görmek için tıklatın](understanding-models-views-and-controllers-cs/_static/image11.png))


Tarayıcınızın adres çubuğundaki URL dikkat edin. Hakkında menü bağlantıya tıkladığında, örneğin, tarayıcınızın adres çubuğuna URL'yi değişikliklerini **/Home/About**.

Tarayıcı penceresini kapatın ve Visual Studio'ya geri dönün, yolu giriş/hakkında bir dosyayı bulmak mümkün olmayacaktır. Dosya yok. Nasıl bu mümkün mü?

## <a name="a-url-does-not-equal-a-page"></a>Bir URL bir sayfa eşit değildir

Geleneksel bir ASP.NET Web Forms uygulaması veya bir Active Server Pages uygulama oluşturduğunuzda, bir URL ve sayfa arasında bire bir ilişkisi yoktur. Sunucudan SomePage.aspx adlı bir sayfa istemek, ardından daha iyi olması bir sayfa SomePage.aspx adlı disk üzerinde. SomePage.aspx dosya mevcut değilse bir çirkin alma **404 - sayfa bulunamadı** hata.

Bir ASP.NET MVC uygulaması oluştururken, buna karşılık, yoktur tarayıcınızın adres çubuğuna yazın URL'yi ve uygulamanızda bulduğunuz dosyaları arasında hiçbir Yazışma. Bir ASP.NET MVC uygulamasındaki bir URL bir denetleyici eylemi bir sayfada disk yerine karşılık gelir.

Geleneksel bir ASP.NET veya ASP uygulamasında, tarayıcı istekler sayfalara eşlenir. Bir ASP.NET MVC uygulamasındaki, buna karşılık, denetleyici eylemleri için tarayıcı istekler eşlenir. İçerik odaklı bir ASP.NET Web Forms uygulamasıdır. Buna karşılık, bir ASP.NET MVC uygulamasını uygulama mantığı merkezli ' dir.

## <a name="understanding-aspnet-routing"></a>ASP.NET yönlendirme anlama

Bir tarayıcı isteğini bir denetleyici eylemi adlı ASP.NET framework'ün için bir özellik üzerinden eşlenen *ASP.NET yönlendirmesi*. ASP.NET yönlendirme için ASP.NET MVC çerçevesi tarafından kullanılan *rota* denetleyici eylemleri için gelen istekler.

ASP.NET yönlendirme bir yol tablosu gelen istekleri işlemek için kullanır. Web uygulamanızı ilk kez başlatıldığında, bu rota tablosu oluşturulur. Rota Global.asax dosyasındaki kurulum tablodur. Varsayılan MVC Global.asax dosyası listeleme 1'de yer alır.

**1 - Global.asax listeleme**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Bir ASP.NET uygulamasını ilk kez başlatıldığında, uygulama\_Start() yöntemi çağrılır. 1, listeleme, bu yöntem RegisterRoutes() yöntemini çağırır ve varsayılan rota tablosu RegisterRoutes() yöntemi oluşturur.

Bir yolu varsayılan rota tablosu oluşur. Bu varsayılan rotada gelen tüm istekleri (URL kesimi eğik arasında herhangi bir şey olduğunu) üç parçalara ayırır. İlk parça için bir denetleyici adı eşlenir, ikinci kesim bir eylem adı için eşlenen ve son segmentinde kimliği adlı eylem için geçirilen parametre eşlendi

Örneğin, aşağıdaki URL'ye göz önünde bulundurun:

/ Ürün/Ayrıntılar/3

Bu URL, bu gibi üç parametrelerine ayrıştırılır:

Denetleyici ürün =

Eylem ayrıntıları =

Kimlik = 3

Varsayılan rota Global.asax dosyasında tanımlanmış tüm üç parametrelerinin varsayılan değerleri içerir. Varsayılan giriş denetleyicisidir, varsayılan eylem dizinidir ve varsayılan kimlik boş bir dizedir. Bu varsayılan aklınızda nasıl aşağıdaki URL ayrıştırılır göz önünde bulundurun:

/ Çalışan

Bu URL, bu gibi üç parametrelerine ayrıştırılır:

Denetleyici çalışan =

Eylem dizini =

Kimlik =

Son olarak, bir ASP.NET MVC uygulaması herhangi bir URL sağlamadan açarsanız (örneğin, `http://localhost`) sonra URL şu şekilde ayrıştırılır:

Denetleyici giriş =

Eylem dizini =

Kimlik =

İstek HomeController sınıfı İNDİS() eylemini yönlendirilir.

## <a name="understanding-controllers"></a>Denetleyicileri anlama

Bir kullanıcı bir MVC uygulamasıyla etkileşim şeklini denetlemek için bir denetleyici sorumludur. Bir denetleyici bir ASP.NET MVC uygulaması için akış denetimi mantığı içerir. Bir denetleyici bir tarayıcı isteğini bir kullanıcı bulunduğunda, bir kullanıcıya geri göndermek için nasıl bir yanıt belirler.

Yalnızca bir sınıf (örneğin, Visual Basic veya C# sınıfı) bir denetleyicisidir. Örnek ASP.NET MVC uygulaması denetleyicileri klasöründe bulunan HomeController.cs adlı bir denetleyicisi içerir. HomeController.cs dosyanın içeriğini listeleme 2'de yeniden oluşturulur.

**2 - HomeController.cs listeleme**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

HomeController İNDİS() ve About() adlı iki yöntem olduğuna dikkat edin. Bu iki yöntem denetleyici tarafından kullanıma sunulan iki eylem karşılık gelir. URL /Home/dizin HomeController.Index() yöntemi çağırır ve URL /Home/hakkında HomeController.About() yöntemi çağırır.

Herhangi bir genel yöntemini bir denetleyicide, denetleyici eylem olarak kullanıma sunulur. Bu konuda dikkatli olmanız gerekir. Bu, bir denetleyici içinde yer alan herhangi bir genel yöntem Internet erişimi olan herkes tarafından bir tarayıcıya doğru URL'yi girerek çağrılabileceğini anlamına gelir.

## <a name="understanding-views"></a>Anlama görünümleri

İNDİS() ve About(), HomeController sınıfı tarafından kullanıma sunulan iki denetleyici eylemleri hem de bir görünüm döndürür. Bir görünümü HTML İşaretleme ve tarayıcıya gönderilen içerik içerir. Görünüm sayfa bir ASP.NET MVC uygulamasını çalışırken eşdeğerdir.

Kendi görünümlerinizi doğru konumda oluşturmanız gerekir. HomeController.Index() eylemi, şu yolda bulunan bir görünümü döndürür:

\Views\Home\Index.aspx

HomeController.About() eylemi, şu yolda bulunan bir görünümü döndürür:

\Views\Home\About.aspx

Bir denetleyici eylemi için bir görünümüne geri dönmek istiyorsanız, genel olarak, ardından görünümleri klasöründe denetleyiciniz aynı ada sahip bir alt klasör oluşturmak ihtiyacınız. Alt klasörde, denetleyici eylem olarak aynı ada sahip .aspx dosyası oluşturmanız gerekir.

3 listeleme dosyasında About.aspx görünümü içerir.

**3 - About.aspx listeleme**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

İlk satırı 3 listeleme yoksayarsanız, geri kalanını görünüm çoğu standart HTML oluşur. Burada istediğiniz herhangi bir HTML girerek görünümün içeriğini değiştirebilirsiniz.

Bir görünümü, bir Active Server Pages ya da ASP.NET Web formları sayfasında çok benzer. Bir görünümü HTML içerik ve betikler içerebilir. Programlama dili (örneğin, C# veya Visual Basic .NET), en sevdiğiniz .NET komut dosyaları yazabilirsiniz. Veritabanı verileri gibi dinamik içerikleri görüntülemek için komut dosyalarını kullanın.

## <a name="understanding-models"></a>Anlama modelleri

Denetleyicileri Bahsettiğimiz ve görünümleri değinmiştik. Tartışmak için gereken son modelleri konudur. Bir MVC modeli nedir?

Bir MVC model uygulama mantığınızın bir görünüm ya da denetleyiciye yer almayan tüm içerir. Modelin tüm uygulama iş mantığı, doğrulama mantığı ve veritabanına erişim mantığı içermelidir. Microsoft Entity Framework, veritabanına erişmek için kullanıyorsanız, örneğin, daha sonra Entity Framework sınıflarınızı (.edmx dosyanızın) modelleri klasöründe oluşturursunuz.

Yalnızca kullanıcı arabirimi oluşturma ile ilgili mantıksal bir görünüm içermelidir. Bir denetleyici yalnızca tam en doğru görünümüne geri dönmek veya kullanıcı başka bir eylem (akış denetimi) yeniden yönlendirmek için gereken mantığı içermelidir. Diğer her şey modelde yer almalıdır.

Genel olarak, fat modelleri ve sıska denetleyicileri için çaba göstermelisiniz. Denetleyici yöntemlerinizi yalnızca birkaç satır kod içermelidir. Bir denetleyici eylemi çok fat alırsa, mantıksal modellerini klasöründe yeni bir sınıf taşıma düşünmelisiniz.

## <a name="summary"></a>Özet

Bu öğreticide bir ASP.NET MVC farklı bölümlerini üst düzey bir bakış ile web uygulaması sağlanan. ASP.NET yönlendirme gelen tarayıcı istekleri belirli bir denetleyici eylemlerine eşlemelerini nasıl öğrendiniz. Görünümler tarayıcıya döndürülen nasıl denetleyicileri nasıl düzenlemek öğrendiniz. Son olarak, uygulama iş, doğrulama ve veritabanına erişim mantığı modelleri nasıl içeren öğrendiniz.
