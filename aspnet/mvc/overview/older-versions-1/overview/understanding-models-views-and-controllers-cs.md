---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Anlama modeller, görünümler ve denetleyiciler (C#) | Microsoft Docs
author: StephenWalther
description: Modeller, görünümler ve denetleyiciler hakkında kafası? Bu öğreticide, Stephen Walther bir ASP.NET MVC uygulaması farklı bölümlerine tanıtır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c9a0cbbf6f786944d7892fbb14859939f21bdd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="understanding-models-views-and-controllers-c"></a>Anlama modeller, görünümler ve denetleyiciler (C#)
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Modeller, görünümler ve denetleyiciler hakkında kafası? Bu öğreticide, Stephen Walther bir ASP.NET MVC uygulaması farklı bölümlerine tanıtır.


Bu öğretici, bir üst düzey genel bakış ASP.NET MVC ile modeller, görünümler ve denetleyiciler sağlar. Diğer bir deyişle, M açıklar ', V' ve C' ASP.NET mvc'de.

Bu öğretici okuduktan sonra bir ASP.NET MVC uygulaması farklı bölümlerini birlikte nasıl çalıştığını anlamanız gerekir. Ayrıca, bir ASP.NET Web Forms uygulaması ya da Active Server Pages uygulaması bir ASP.NET MVC uygulaması mimarisini nasıl farklı anlamanız gerekir.

## <a name="the-sample-aspnet-mvc-application"></a>Örnek ASP.NET MVC uygulaması

ASP.NET MVC Web uygulamaları oluşturmak için varsayılan Visual Studio şablon bir ASP.NET MVC uygulaması farklı bölümlerini anlamak için kullanılan bir çok basit örnek uygulama içerir. Biz bu Öğreticide bu basit uygulama yararlanın.

Visual Studio 2008 başlatarak MVC şablonu kullanarak yeni bir ASP.NET MVC uygulaması oluşturma ve Dosya menüsü seçeneğini belirleyerek, yeni proje (bkz: Şekil 1). Yeni Proje iletişim kutusunda, sık kullanılan programlama diliniz proje türleri (Visual Basic veya C#) altında seçip **ASP.NET MVC Web uygulaması** şablonlar altında. Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Şekil 01**: Yeni Proje iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-models-views-and-controllers-cs/_static/image2.png))


Yeni bir ASP.NET MVC uygulaması oluşturduğunuzda **birim testi projesi oluşturma** iletişim kutusu görüntülenir (bkz: Şekil 2). Bu iletişim kutusunu, ASP.NET MVC uygulamanızı test etmek için çözümünüzdeki ayrı bir proje oluşturmanızı sağlar. Seçeneğini **Hayır, birim testi projesi oluşturma** tıklatıp **Tamam** düğmesi.


[![Birim testi oluştur iletişim kutusu](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Şekil 02**: birim testi iletişim kutusu oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-models-views-and-controllers-cs/_static/image4.png))


Yeni ASP.NET MVC sonra uygulama oluşturulur. Birkaç klasörleri ve dosyaları Çözüm Gezgini penceresinde görürsünüz. Özellikle, modeller, görünümler ve denetleyiciler üç klasörleri göreceksiniz. Klasör adları tahmin olarak bu klasörlerin modeller, görünümler ve denetleyiciler uygulamak için dosyaları içerir.

Denetleyicileri klasörünü genişletin, AccountController.cs adlı bir dosya ve HomeController.cs adlı bir dosya görmeniz gerekir. Görünümleri klasörünü genişletin, hesap, giriş ve paylaşılan adlı üç alt klasörler görmeniz gerekir. Giriş klasörünü genişletin About.aspx ve Index.aspx (bkz: Şekil 3) adlı iki ek dosyaları görürsünüz. Bu dosyalar varsayılan ASP.NET MVC şablonu dahil örnek uygulaması oluşturur.


[![Çözüm Gezgini penceresi](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Şekil 03**: Çözüm Gezgini penceresi ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-models-views-and-controllers-cs/_static/image6.png))


Menü seçeneğini seçerek örnek uygulamayı çalıştırabilirsiniz **hata ayıklama, hata ayıklamayı Başlat**. Alternatif olarak, F5 tuşuna basabilirsiniz.

Bir ASP.NET uygulamasını ilk kez çalıştırdığınızda, hata ayıklama modunu etkinleştirmek önerir Şekil 4'te iletişim kutusu görüntülenir. Tamam düğmesine tıklayın ve uygulamayı çalıştırın.


[![Hata ayıklama etkin iletişim kutusu](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Şekil 04**: hata ayıklama etkin iletişim ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-models-views-and-controllers-cs/_static/image8.png))


Bir ASP.NET MVC uygulamasını çalıştırdığınızda, Visual Studio web tarayıcınızda uygulamayı başlatır. Örnek uygulama yalnızca iki sayfa içerir: dizin sayfası ve hakkında sayfası. Uygulamayı ilk kez başlatıldığında, (bkz. Şekil 5) dizin sayfası görüntülenir. Üstündeki menü bağlantıyı tıklatarak hakkında sayfasına gidebilirsiniz sağ uygulamasının.


[![Dizin Sayfası](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Şekil 05**: dizin sayfası ([tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-models-views-and-controllers-cs/_static/image11.png))


Tarayıcınızın adres çubuğundaki URL dikkat edin. Hakkında menü bağlantıya tıkladığında, örneğin, tarayıcınızın adres çubuğuna URL'yi değişikliklerini **/ev/hakkında**.

Tarayıcı penceresini kapatın ve Visual Studio'ya geri dönün, yol giriş/yaklaşık bir dosyayı bulmak mümkün olmayacaktır. Dosyaları mevcut değil. Nasıl bu mümkün mü?

## <a name="a-url-does-not-equal-a-page"></a>Bir URL bir sayfa eşit değil

Geleneksel bir ASP.NET Web Forms uygulaması veya bir Active Server Pages uygulaması oluşturduğunuzda, bir URL ve sayfa arasında bire bir ilişkisi yoktur. Sunucudan SomePage.aspx adlı bir sayfaya isterse, ardından daha iyi olması bir sayfa SomePage.aspx adlı disk üzerinde. SomePage.aspx dosya mevcut değilse, bir çirkin elde **404 - sayfa bulunamadı** hata.

Bir ASP.NET MVC uygulaması oluştururken, buna karşılık, yoktur tarayıcınızın adres çubuğuna yazın URL ve uygulamanızda Bul dosyalar arasında hiçbir yazışmaları. Bir ASP.NET MVC uygulamasındaki bir URL bir denetleyici eylemi disk sayfasında yerine karşılık gelir.

Geleneksel bir ASP.NET veya ASP uygulamasında, tarayıcı isteklerini sayfalara eşlenir. Bir ASP.NET MVC uygulamasındaki denetleyici eylemleri için buna karşılık, tarayıcı isteklerini eşlenir. İçerik merkezli bir ASP.NET Web Forms uygulamasıdır. Bir ASP.NET MVC, buna karşılık, uygulama mantığını merkezli uygulamasıdır.

## <a name="understanding-aspnet-routing"></a>ASP.NET yönlendirmeyi anlama

Bir tarayıcı isteğini bir denetleyici eylemi adlı ASP.NET framework'ün için bir özellik aracılığıyla eşlenen *ASP.NET yönlendirme*. ASP.NET yönlendirme için ASP.NET MVC çerçevesi tarafından kullanılan *rota* denetleyici eylemleri için gelen istekleri.

ASP.NET yönlendirme bir yol tablosu gelen istekleri işlemek için kullanır. Web uygulamanızı ilk kez başlatıldığında, bu yol tablosu oluşturulur. Yol tablosu Global.asax dosyasında kurulur. Varsayılan MVC Global.asax dosyası listeleme 1'de yer alır.

**1 - Global.asax listeleme**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Bir ASP.NET uygulamasını ilk kez başlatıldığında, uygulama\_Start() yöntemi çağrılır. 1. listeleme, bu yöntem RegisterRoutes() yöntemini çağırır ve varsayılan rota tablosu RegisterRoutes() yöntemi oluşturur.

Varsayılan rota tablosu bir rota oluşur. Bu varsayılan rotada gelen tüm istekleri (URL kesimi eğik arasında herhangi bir şey olabilir) üç parçalara ayırır. İlk kesimini bir denetleyici adı ile eşlenmiş, ikinci kesim bir eylem adı ile eşlenmiş ve son kesim kimliği adlı eylem geçirilen parametre eşlendi

Örneğin, aşağıdaki URL'yi göz önünde bulundurun:

/ Ürün/Ayrıntılar/3

Bu URL, bu gibi üç parametrelerine ayrıştırılır:

Denetleyici ürün =

Eylem Ayrıntılar =

ID = 3

Varsayılan rota Global.asax dosyasında tanımlanmış tüm üç parametre için varsayılan değerleri içerir. Varsayılan giriş denetleyicisidir, varsayılan eylem dizindir ve varsayılan kimlik boş bir dizedir. Aklınızda bu varsayılan ile aşağıdaki URL'yi nasıl ayrıştırılır göz önünde bulundurun:

/ Çalışan

Bu URL, bu gibi üç parametrelerine ayrıştırılır:

Denetleyici çalışan =

Eylem dizini =

Kimlik =

Son olarak, bir ASP.NET MVC uygulaması herhangi bir URL girmeden açarsanız (örneğin, `http://localhost`) URL şöyle ayrıştırılır sonra:

Denetleyici giriş =

Eylem dizini =

Kimlik =

İstek HomeController sınıfı İNDİS() eylemini yönlendirilir.

## <a name="understanding-controllers"></a>Denetleyicileri anlama

Bir kullanıcı bir MVC uygulaması ile etkileşim şeklini denetlemek için bir denetleyici sorumludur. Bir denetleyici bir ASP.NET MVC uygulaması için akış denetimi mantığını içerir. Bir denetleyici bir tarayıcı isteğini bir kullanıcı yaptığında, bir kullanıcıya geri göndermek için hangi yanıtını belirler.

Yalnızca bir sınıfı (örneğin, bir Visual Basic veya C# sınıfı) denetleyicisidir. ASP.NET MVC uygulaması örnek denetleyicileri klasöründe HomeController.cs adlı bir denetleyicisi içerir. HomeController.cs dosyasının içeriğini listeleme 2'de yeniden oluşturulur.

**2 - HomeController.cs listeleme**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

HomeController İNDİS() ve About() adlı iki yöntem olduğuna dikkat edin. Bu iki yöntem denetleyici tarafından kullanıma sunulan iki eylem karşılık gelir. URL /Home/dizin HomeController.Index() yöntemini çağırır ve URL /Home/hakkında HomeController.About() yöntemini çağırır.

Herhangi bir genel yöntemini bir denetleyicide, denetleyici eylem olarak sunulur. Bu konuda dikkatli olmanız gerekir. Bu, bir denetleyici bulunan tüm genel yöntem Internet erişimi olan herkes tarafından bir tarayıcıya doğru URL'yi girerek çağrılabilir olduğunu anlamına gelir.

## <a name="understanding-views"></a>Anlama görünümleri

İNDİS() ve About(), HomeController sınıfı tarafından kullanıma sunulan iki denetleyici eylemleri hem de bir görünümü döndürün. Bir görünümü HTML biçimlendirmesi ve tarayıcıya gönderilen içerik içerir. Bir görünüm bir ASP.NET MVC uygulaması ile çalışırken, bir sayfa eşdeğerdir.

Kendi görünümlerinizi doğru konumda oluşturmanız gerekir. HomeController.Index() eylemi aşağıdaki yolda bulunan bir görünümü döndürür:

\Views\Home\Index.aspx

HomeController.About() eylemi aşağıdaki yolda bulunan bir görünümü döndürür:

\Views\Home\About.aspx

Bir denetleyici eylemi için bir görünüme dönmek istiyorsanız, genel olarak, daha sonra bir alt denetleyicinizi aynı ada sahip görünümler klasöründe oluşturmanız gerekir. İçinde alt, denetleyici eylem olarak aynı ada sahip bir .aspx dosyası oluşturmanız gerekir.

Listeleme 3 dosyasında About.aspx görünümü içerir.

**3 - About.aspx listeleme**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

İlk satırı listeleme 3'te yoksayarsanız, görünüme geri kalanı çoğunu standart HTML oluşur. Burada istediğiniz herhangi bir HTML girerek görünümü içeriğini değiştirebilirsiniz.

Bir görünüm, Active Server Pages veya ASP.NET Web Forms sayfası çok benzer. Bir görünümü HTML içeriğini ve komut dosyaları içerebilir. Programlama dili (örneğin, C# veya Visual Basic .NET) komut dosyaları, sık kullanılan .NET yazabilirsiniz. Veritabanı verileri gibi dinamik içerik görüntülemek için komut dosyalarını kullanın.

## <a name="understanding-models"></a>Anlama modelleri

Biz denetleyicileri ele ve biz görünümleri ele. Biz tartışmak için gereken son modelleri konudur. Bir MVC modeli nedir?

Bir MVC model tüm bir görünüm veya denetleyicisi bulunmayan, uygulama mantığını içerir. Model tüm uygulama iş mantığı, doğrulama mantığını ve veritabanı erişim mantığının içermelidir. Veritabanınıza erişmek için Microsoft Entity Framework kullanıyorsanız, örneğin, daha sonra Entity Framework sınıflarınızı (.edmx dosyasının) modelleri klasöründe oluşturursunuz.

Bir görünüm yalnızca kullanıcı arabirimi oluşturma ile ilgili mantığının içermelidir. Bir denetleyici yalnızca tam en doğru görünümüne dönün veya kullanıcı başka bir eylem (akış denetimi) yeniden yönlendirmek için gerekli mantığı içermelidir. Şey modelde yer almalıdır.

Genel olarak, fat modelleri ve zayıf denetleyicileri için çalışmalarımızı. Denetleyici yöntemlerinizi yalnızca birkaç satır kod içerir. Bir denetleyici eylemi çok fat alırsa, modelleri klasöründe yeni bir sınıf için mantığı taşımayı düşünmelisiniz.

## <a name="summary"></a>Özet

Bu öğreticide web uygulaması ile yüksek düzey bir genel bakış bir ASP.NET MVC farklı bölümlerini sağlanan. Nasıl ASP.NET yönlendirme gelen tarayıcı isteklerini belirli denetleyici eylemleri için eşler öğrendiniz. Görünümleri tarayıcıya nasıl döndürülür denetleyicileri nasıl düzenlendiğini öğrendiniz. Son olarak, uygulama iş, doğrulama ve veritabanı erişim mantığı modelleri nasıl içeren öğrendiniz.
