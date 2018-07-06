---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Giriş hata ayıklama ASP.NET Web sayfaları (Razor) siteler | Microsoft Docs
author: tfitzmac
description: Hata ayıklama hataları bulma ve kod sayfalarınıza düzeltme işlemidir. Bu bölümde bazı araçlar ve hata ayıklamak için kullanabileceğiniz teknikleri gösterir ve analyz...
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: b8a492b065902fa10d3e4c5cccd50e63ea356709
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823809"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>(Razor) giriş hata ayıklama ASP.NET Web sayfaları
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sayfalarında hata ayıklamak için çeşitli yollar açıklanmaktadır. Hata ayıklama hataları bulma ve kod sayfalarınıza düzeltme işlemidir.
> 
> **Öğrenecekleriniz:** 
> 
> - Yardımcı olacak bilgileri görüntülemek nasıl analiz edin ve hata ayıklama sayfaları.
> - Visual Studio'da hata ayıklama kullanmayı araçları.
>   
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - `ServerInfo` Yardımcısı.
> - `ObjectInfo` Yardımcısı.
>   
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
> - Visual Studio 2013
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır. WebMatrix 3'ü kullanabilirsiniz ancak tümleşik hata ayıklayıcı desteklenmiyor.


Hataları ve sorunları kodunuzda sorun giderme işlemlerinin önemli bir yönüdür bunları ilk başta önlemek içindir. Hatalarla karşılaşırsanız neden olabilecek kod bölümlerini koyarak bunu yapabilirsiniz `try/catch` engeller. Hataları işleme hakkında daha fazla bilgi için bölüm bakın [giriş kullanımına ASP.NET Web programlama Razor söz dizimi](https://go.microsoft.com/fwlink/?LinkId=202890).

`ServerInfo` Yardımcı olan bir genel bakış sayfanız barındıran web sunucusu ortamı hakkındaki bilgileri sunan bir tanı aracıdır. Ayrıca, bir tarayıcı sayfa istediğinde gönderilen HTTP isteği bilgilerini gösterir. `ServerInfo` Yardımcısı görüntüler geçerli bir kullanıcı kimliği, istekte tarayıcı türü ve benzeri. Bu tür bilgiler genel sorunları gidermenize yardımcı olabilir.

1. Adlı yeni bir web sayfası oluşturma *ServerInfo.cshtml*.
2. Sayfasının sonunda, yeni kapatmadan önce `</body>` etiketinde, ekleyin `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Ekleyebileceğiniz `ServerInfo` kod sayfası herhangi bir yerindeki. Ancak, sonunda ekleme çıktısını kolay okunur hale getiren, diğer sayfa içeriği, ayrı tutar.

    > [!NOTE] 
    > 
    > **Önemli** bir üretim sunucusu için web sayfaları geçmeden önce web sayfalarınızdan herhangi bir tanılama kodu kaldırmanız gerekir. Bu durum geçerlidir `ServerInfo` bir sayfaya kod ekleme, hatalı durumdaki diğer tanılama teknikler bu makaledeki yanı sıra Yardımcısı. Bu tür bilgiler kötü amaçlı kişilerin yararlı olabileceği için sunucu ve benzer ayrıntıları, sunucu adı, kullanıcı adları, yollar hakkındaki bilgileri görmek için Web sitenizin ziyaretçileri istemezsiniz.
3. Sayfayı kaydedin ve bir tarayıcıda çalıştırın.

    ![Hata ayıklama-1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo` Yardımcısı sayfasında dört tablo bilgileri görüntüler:

   - Sunucu yapılandırması. Bu bölümde, bilgisayar adı, çalıştırmakta olduğunuz ASP.NET, etki alanı adı ve sunucu sürümü dahil olmak üzere barındırma web sunucusu hakkında bilgi sağlar.
   - ASP.NET sunucu değişkenleri. Bu bölümde, birçok HTTP protokolü ayrıntıları (çağrılan HTTP değişkenler) hakkında ayrıntılı bilgi sağlar ve bu değerleri her web sayfa isteğinde bir parçasıdır.
   - HTTP çalışma zamanı bilgileri. Bu bölüm, ayrıntıları, sürümü, web sayfanızın altında çalışan Microsoft .NET Framework, yol, önbellek vb. hakkında ayrıntılar sağlar. (İçinde öğrenilen [kullanımına ASP.NET Web programlama Razor söz dizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890), Razor sözdizimi kendisini kapsamlı bir yazılım üzerinde oluşturulmuş olan Microsoft ASP.NET web sunucusu teknolojileri olanakları kullanan ASP.NET Web sayfaları .NET Framework adlı geliştirme kitaplığını.)
   - Ortam değişkenleri. Bu bölümde web sunucusundaki tüm yerel ortam değişkenlerini ve değerleri listesi sağlar.

     Tüm sunucu ve istek bilgileri tam açıklamasını bu makalenin kapsamı dışındadır, ancak gördüğünüz gibi `ServerInfo` Yardımcısı, çok miktarda tanılama bilgileri döndürür. Değerler hakkında daha fazla bilgi için `ServerInfo` döndürür, bkz: [tanınan ortam değişkenlerini](https://technet.microsoft.com/library/dd560744(WS.10).aspx) Microsoft TechNet Web sitesinde ve [IIS Sunucu değişkenleri](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) MSDN Web sitesinde.

## <a name="embedding-output-expressions-to-display-page-values"></a>Sayfa değerleri görüntülemek için ekleme çıkış ifadeleri

Kodunuzda neler olduğunu görmek için başka bir sayfaya çıkış ifade katıştırma yöntemdir. Bildiğiniz gibi doğrudan bir değişkenin değerini aşağıdakine benzer ekleyerek çıkarabilirsiniz `@myVariable` veya `@(subTotal * 12)` sayfası. Hata ayıklama için kodunuzda bu çıkış ifadeler stratejik noktalarda yerleştirebilirsiniz. Bu, sayfa çalıştığında anahtar değişkenleri veya hesaplama sonucu değerini görmenize olanak sağlar. Bitirdiğinizde, hata ayıklama ifadeleri kaldırabilir veya yorum gönderin. Bu yordam, bir sayfa hataları ayıklamanıza yardımcı için katıştırılmış ifadeleri kullanma için tipik bir yol gösterir.

1. Adlı yeni bir WebMatrix sayfası oluşturma *OutputExpression.cshtml*.
2. Sayfa içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    Örnekte bir `switch` değerini denetlemek için bildirimi `weekday` değişkeni ve sonra görünen bir haftanın gününü bağlı olarak farklı çıkış iletisinin. Örnekte, `if` blok ilk kod bloğu içinde rastgele bir gün için geçerli haftanın gününü değeri ekleyerek haftanın günü değiştirir. Bu gösterim amacıyla sunulan bir hatadır.
3. Sayfayı kaydedin ve bir tarayıcıda çalıştırın.

    Sayfa yanlış haftanın günü için iletisi görüntülenir. Haftanın her günü gerçekten olduğundan, daha sonra bir günlük iletisi görürsünüz. Bu durumda, (kod bilerek yanlış day değeri ayarlar nedeniyle) neden ileti kapalı olduğunu ancak, gerçekte, genellikle şeyleri burada kodda yanlış kalacaklarını bilmek zordur. Hata ayıklamak için değeri olarak anahtar nesneler ve değişkenler gibi neler olup bittiğine bulmanız gereken `weekday`.
4. Çıkış ifadeleri ekleyerek ekleyin `@weekday` kod açıklamaları tarafından gösterilen iki yerde gösterildiği gibi. Bu çıkış ifadeler değişkeni değerlerini o noktada kod yürütme görüntüler.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Kaydedin ve sayfanın tarayıcıda çalıştırın.

    Gerçek haftanın gününü, ilk sayfasını görüntüler ve ardından güncelleştirilmiş haftanın sonuçlarının bir gün ve sonuçta elde edilen iletiden eklemesini `switch` deyimi. İki değişken ifadeleri çıktısı (`@weekday`) gün arasında bir boşluk olmayan herhangi bir HTML eklemediğim sahip `<p>` ; çıkış etiketleri yalnızca test için ifadelerdir.

    ![Hata ayıklama-2](introduction-to-debugging/_static/image2.jpg)

    Şimdi hatanın nerede görebilirsiniz. İlk kez görüntülediğinizde `weekday` değişken kodda, doğru günü gösterir. Görüntülediğinizde, ikincisinde ise, sonra `if` kodda engelleme, kapalı bir gündür. Bu nedenle bir şey haftanın günü değişkeni birinci ve ikinci görünümünü arasında oluştuğunu bildirin. Bu tür bir yaklaşım, bu gerçek bir hata varsa, soruna neden olan kod konumunu sınırlandırmanıza yardımcı olacaktır.
6. Kodu, eklediğiniz iki çıkış ifadeleri kaldırma ve haftanın günü değişiklikleri kod kaldırma sayfasında düzeltin. Kalan, tam kod bloğunu aşağıdaki örnekteki gibi görünür:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Sayfanın tarayıcıda çalıştırın. Bu süre, gerçek haftanın günü için görüntülenen doğru iletisini görürsünüz.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Nesne değerleri görüntülemek için ObjectInfo Yardımcısını kullanma

`ObjectInfo` Yardımcısı, türü ve kendisine geçirdiğiniz her bir nesnenin değerini görüntüler. (Önceki örnekte çıkış ifadelerle yaptığınız gibi), kodunuzda değişkenler ve nesneler değerini görüntülemek için kullanın yanı sıra veri nesnesi hakkında bilgi türü görebilirsiniz.

1. Adlı dosyayı açın *OutputExpression.cshtml* daha önce oluşturduğunuz.
2. Tüm kod sayfasında, aşağıdaki kod bloğunu ile değiştirin:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Kaydedin ve sayfanın tarayıcıda çalıştırın.

    ![Hata ayıklama-4](introduction-to-debugging/_static/image3.jpg)

    Bu örnekte, `ObjectInfo` Yardımcısı iki öğeleri görüntüler:

   - Tür. Birinci değişken için olan türdür `DayOfWeek`. İkinci değişken için olan türdür `String`.
   - Değer. Sayfada zaten Karşılama değişkenin değerini görüntülemek için değişkenine geçirdiğinizde bu durumda, yeniden görüntülenmesi `ObjectInfo`.

     Daha karmaşık nesneler için `ObjectInfo` Yardımcısı, daha fazla bilgi görüntüleyebilir &#8212; temel olarak, bu türleri ve değerleri tüm bir nesnenin özelliklerini görüntüleyebilirsiniz.

## <a name="using-debugging-tools-in-visual-studio"></a>Visual Studio'da hata ayıklama araçlarını kullanarak

Daha kapsamlı bir hata ayıklama deneyimi için Visual Studio 2013 veya ücretsiz kullanmak [Visual Studio Express 2013 Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Visual Studio ile incelemek istediğiniz satırı kodunuzda bir kesme noktası ayarlayabilirsiniz.

![kesme noktası Ayarla](introduction-to-debugging/_static/image1.png)

Web sitesi test ettiğinizde, kodu yürütürken kesme noktasında durur.

![kesme noktası ulaşın](introduction-to-debugging/_static/image2.png)

Değişkenler ve kod satırının satır adımlayın geçerli değerlerini inceleyebilirsiniz.

![değerler bakın](introduction-to-debugging/_static/image3.png)

ASP.NET Razor sayfaları hata ayıklamak için Visual Studio'da tümleşik hata ayıklayıcısı hakkında daha fazla bilgi için bkz. [programlama ASP.NET Web sayfaları (Razor) kullanarak Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Ek Kaynaklar

- [Visual Studio kullanarak ASP.NET Web sayfaları (Razor) programlama](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS Sunucu değişkenleri](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Ortam değişkenlerini tanınan](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
