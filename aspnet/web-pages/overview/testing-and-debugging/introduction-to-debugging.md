---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Giriş hata ayıklama ASP.NET Web sayfaları (Razor) siteleri | Microsoft Docs
author: tfitzmac
description: Hata ayıklama hataları bulma ve kod sayfalarınızda düzeltme işlemidir. Bu bölümde bazı araçları ve hata ayıklama için kullanabileceğiniz teknikleri gösterir ve analyz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: c28d63acda6e585f4aa64f294049c1790faac850
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>(Razor) giriş hata ayıklama ASP.NET Web sayfaları
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sayfalarında hata ayıklamak için çeşitli yollar açıklanmaktadır. Hata ayıklama hataları bulma ve kod sayfalarınızda düzeltme işlemidir.
> 
> **Öğrenecekleriniz:** 
> 
> - Yardımcı olacak bilgileri görüntülemek nasıl çözümlemek ve sayfaları hata ayıklama.
> - Hata ayıklama kullanmayı Visual Studio Araçları.
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
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır. WebMatrix 3 kullanabilirsiniz ancak tümleşik hata ayıklayıcı desteklenmiyor.


İlk başta önlemek için sorun giderme hataları ve sorunları kodunuzdaki önemli bir durum değil. Bunu hatalarla karşılaşırsanız neden olabilecek kodunuzu bölümlerini koyarak yapabilirsiniz `try/catch` engeller. Daha fazla bilgi için hataları işleme bölümüne bakın [ASP.NET Web programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890).

`ServerInfo` Yardımcı olan bir bakış sayfanıza barındıran web sunucusu ortamı hakkındaki bilgileri içeren bir tanı aracı. Ayrıca, bir tarayıcı sayfa istediğinde, gönderilen HTTP isteği bilgilerini gösterir. `ServerInfo` Yardımcı görüntüler geçerli bir kullanıcı kimliği, istekte tarayıcı türü ve benzeri. Bu tür bilgileri sık karşılaşılan sorunları gidermenize yardımcı olabilir.

1. Adlı yeni bir web sayfası oluşturun *ServerInfo.cshtml*.
2. Sayfa sonunda, yalnızca kapatmadan önce `</body>` etiketi, add `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Ekleyebileceğiniz `ServerInfo` sayfa başka bir yerindeki kod. Ancak sonunda ekleme çıktısını okuması daha kolay hale getiren, diğer sayfa içeriği, ayrı tutar.

    > [!NOTE] 
    > 
    > **Önemli** web sayfaları için bir üretim sunucusu taşımadan önce herhangi bir tanılama kod, web sayfalarından kaldırmanız gerekir. Bu uygulandığı `ServerInfo` bir sayfaya kod ekleme ile ilgili diğer tanılama bu makaledeki teknikleri yanı sıra Yardımcısı. Bu tür bilgiler kötü amaçlı kişilerin yararlı olabileceği için sunucu adı, kullanıcı adları, yollar hakkında bilgi, sunucu ve benzer ayrıntıları görmek için Web sitenizin ziyaretçileri istemezsiniz.
3. Sayfayı kaydedin ve tarayıcıda çalıştırın.

    ![Hata ayıklama 1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo` Yardımcısı sayfasında dört tablonun bilgileri görüntüler:

   - Sunucu yapılandırması. Bu bölüm, bilgisayar adı, çalıştırmakta olduğunuz ASP.NET, etki alanı adı ve sunucu zaman sürümü de dahil olmak üzere barındıran web sunucusu hakkında bilgi sağlar.
   - ASP.NET sunucu değişkenleri. Bu bölüm, birçok HTTP protokol ayrıntılarını (çağrılan HTTP değişkenler) hakkında ayrıntılı bilgi sağlar ve bu değerleri her web sayfası isteği bir parçasıdır.
   - HTTP çalışma zamanı bilgileri. Bu bölümde, web sayfanızın altında çalışan Microsoft .NET Framework, yol, önbellek vb. ayrıntıları sürümü ayrıntıları. (İçinde öğrenilen [ASP.NET Web programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890), Razor sözdizimini kendisini kapsamlı bir yazılımı yerleşik olan Microsoft ASP.NET web sunucu teknolojisi üzerinde oluşturulan kullanarak ASP.NET Web sayfaları Geliştirme kitaplığı .NET Framework olarak adlandırılır.)
   - Ortam değişkenleri. Bu bölümde, web sunucusundaki tüm yerel ortam değişkenlerini ve değerleri listesi sağlar.

     Tüm sunucu ve istek bilgilerinin tam bir açıklaması bu makalenin kapsamı dışındadır olmakla birlikte, gördüğünüz `ServerInfo` Yardımcısı çok miktarda bir tanı bilgilerini döndürür. Değerler hakkında daha fazla bilgi için `ServerInfo` döndürür, bkz: [tanınan ortam değişkenleri](https://technet.microsoft.com/library/dd560744(WS.10).aspx) Microsoft TechNet Web sitesinde ve [IIS Sunucu değişkenleri](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) MSDN Web sitesinde.

## <a name="embedding-output-expressions-to-display-page-values"></a>Sayfa değerlerini görüntülemek için katıştırma çıktı ifadeleri

Kodunuzda neler olduğunu görmek için başka bir çıktı ifadeleri sayfasına katıştırmak için yoludur. Bildiğiniz gibi doğrudan bir değişkenin değerini şöyle ekleyerek çıkarabilirsiniz `@myVariable` veya `@(subTotal * 12)` sayfası. Hata ayıklama için bu çıkış ifadeleri stratejik noktalarda kodunuzda yerleştirebilirsiniz. Bu, sayfa çalıştığında anahtar değişkenleri veya hesaplama sonucu değerini görmenize olanak sağlar. Bitirdiğinizde hata ayıklama, ifadeler kaldırma veya bunları açıklama. Bu yordam bir sayfaya hata ayıklama yardımcı olmak için katıştırılmış ifadeler kullanmak için tipik bir yol gösterir.

1. Adlı yeni bir WebMatrix sayfa oluşturma *OutputExpression.cshtml*.
2. Sayfa içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    Örnek kullanan bir `switch` değerini denetlemek için deyimi `weekday` değişkeni ve haftanın hangi gününe bağlı olarak farklı çıktı iletisi olmasından sonra görüntüleme. Örnekte, `if` ilk kod bloğu blokta rasgele bir gün geçerli hafta içi günü değerine ekleyerek haftanın günü değiştirir. Bu gösterim amaçları için sunulan bir hatadır.
3. Sayfayı kaydedin ve tarayıcıda çalıştırın.

    Sayfa için yanlış günün haftanın iletisi görüntülenir. Haftanın hangi günü gerçekte olduğundan, daha sonra bir günlük iletisi görürsünüz. Bu durumda, (kod kasıtlı olarak hatalı günü değeri ayarlar) neden ileti devre dışı olduğundan bildiğiniz olsa da, gerçekte, genellikle burada şeyler kodu yanlış kalacaklarını bilmek zordur. Hata ayıklama için anahtar nesneleri ve değişkenlerin değerine gibi olanlar çıkışı bulmanız gereken `weekday`.
4. Çıktı ifadeleri ekleyerek ekleyin `@weekday` kod açıklamaları tarafından gösterilen iki yerde de gösterildiği gibi. Bu çıktı ifadeleri değişkeni değerleri bu noktada kod yürütülmesine görüntüler.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Kaydedip sayfasını bir tarayıcıda çalıştırın.

    Gerçek haftanın ilk sayfasını görüntüler ve ardından güncelleştirilmiş haftanın gününü sonuçlarının bir gün ve sonuçta elde edilen iletiden ekleme `switch` deyimi. İki değişken ifadeleri çıktısını (`@weekday`) herhangi bir HTML ekleyemiyor çünkü gün arasında boşluk içermeyen `<p>` çıkış; etiketleri yalnızca sınama ifadelerini.

    ![Hata ayıklama 2](introduction-to-debugging/_static/image2.jpg)

    Şimdi hata olduğu görebilirsiniz. İlk görüntülediğinizde `weekday` değişken kodda, bunu doğru günü gösterir. Görüntülediğinizde, ikincisinde ise, sonra `if` kodda engellemek, kapalı bir günüdür. Bir hafta içi günü değişkeni birinci ve ikinci görünüm arasında gecikmesi olduğunu bilmesi. Bu gerçek bir hata varsa, bu tür bir yaklaşım soruna neden olan kod konum daraltmaya yardımcı olacaktır.
6. Kod sayfasında eklediğiniz iki çıktı ifadeleri kaldırarak ve haftanın günü değişiklikleri kod kaldırma düzeltin. Kod kalan, tam bloğunu aşağıdaki gibi görünür:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Bir tarayıcıda. Sayfayı çalıştırın. Bu saati gerçek haftanın günü için görüntülenen doğru iletisini görürsünüz.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Nesne değerlerini görüntülemek için ObjectInfo Yardımcısını kullanma

`ObjectInfo` Yardımcısı, türü ve kendisine geçirdiğiniz her nesnenin değerini görüntüler. Değişkenleri ve nesneleri değerini kodunuzu (çıktı ifadelerle önceki örnekte olduğu gibi) görüntülemek için kullanın ve ayrıca veri nesnesi hakkında bilgi türü görebilirsiniz.

1. Adlı dosyayı açın *OutputExpression.cshtml* daha önce oluşturduğunuz.
2. Sayfadaki tüm kod aşağıdaki kod bloğunu ile değiştirin:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Kaydedip sayfasını bir tarayıcıda çalıştırın.

    ![Hata ayıklama 4](introduction-to-debugging/_static/image3.jpg)

    Bu örnekte, `ObjectInfo` Yardımcısı iki öğe görüntüler:

   - Tür. İlk değişken için türüdür `DayOfWeek`. İkinci değişken için türüdür `String`.
   - Değer. Sayfasında zaten Tebrik değişkenin değerini görüntülemek için değişkeni geçirdiğinizde bu durumda, değer yeniden görüntülenir `ObjectInfo`.

     Daha karmaşık nesneler için `ObjectInfo` Yardımcısı, daha fazla bilgi görüntüleyebilir &#8212; temelde, onu türlerini ve değerlerini tüm nesnenin özelliklerini görüntüleyebilirsiniz.

## <a name="using-debugging-tools-in-visual-studio"></a>Visual Studio'da hata ayıklama araçlarını kullanarak

Daha kapsamlı bir hata ayıklama deneyimini için Visual Studio 2013 veya ücretsiz kullanmak [için Visual Studio Express 2013 Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Visual Studio ile kodunuzda incelemek istediğiniz satırında bir kesme noktası ayarlayabilirsiniz.

![Kesme noktası ayarlama](introduction-to-debugging/_static/image1.png)

Web sitesi test ettiğinizde, yürütme kodu kesme noktasında durur.

![kesme noktası ulaşmak](introduction-to-debugging/_static/image2.png)

Değişkenleri ve kod satırının satır ilerleyebilirsiniz geçerli değerlerini inceleyebilirsiniz.

![değerler bakın](introduction-to-debugging/_static/image3.png)

ASP.NET Razor sayfalarının hata ayıklamak için Visual Studio tümleşik hata ayıklayıcıyı kullanma hakkında daha fazla bilgi için bkz: [programlama ASP.NET Web sayfaları (Razor) kullanarak Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Ek Kaynaklar

- [Visual Studio kullanarak ASP.NET Web sayfaları (Razor) programlama](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS Sunucu değişkenleri](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Ortam değişkenleri kabul](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
