---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Bir denetleyici ekleme | Microsoft Docs
author: shanselman
description: Bu öğretici, burada Visual Studio 2013 kullanarak kullanılabiliyorsa, güncelleştirilmiş bir sürüm. Yeni öğretici t birçok iyileştirme sağlayan ASP.NET MVC 5 kullanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869029"
---
<a name="adding-a-controller"></a>Bir denetleyici ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Bu öğretici kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Yeni öğretici Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 kullanır.
> 
> 
> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


MVC Model, görünüm, denetleyici anlamına gelir. MVC birbirinden farklı bir sorumluluk her bölümü sahip olacağı şekilde uygulamaları geliştirmek için bir desen ' dir.

- Modeli: Uygulamanızın veri
- Görünümleri: Dinamik olarak HTML yanıtları oluşturmak için uygulamanızın şablon dosyalarını kullanır.
- Denetleyicileri: uygulama gelen URL isteklerini işleyen sınıflar model verileri almak ve istemcisine geri yanıt işleme görünüm şablonları belirtin

Biz, bu öğreticide, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

Yeni bir denetleyicisi solution Explorer denetleyicileri klasörüne sağ tıklayıp denetleyici Ekle seçerek oluşturalım.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Yeni denetleyicinize "HelloWorldController" olarak adlandırın ve Ekle'yi tıklatın.

[![Denetleyici Ekle iletişim kutusu](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Bildirim HelloWorldController.cs adlı sizin için yeni bir dosya oluşturuldu ve bu dosyayı şimdi açılır sağdaki Çözüm Gezgini'nde **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Yeni ortak sınıfının HelloWorldController içinde şuna iki yeni yöntemi oluşturun. Örnek olarak HTML dizesi doğrudan sunduğumuz denetleyicisinden getireceğiz.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Denetleyicinizi HelloWorldController adlandırılır ve yeni yönteminizi dizin adı verilir. Uygulamanızı yeniden çalıştırma yalnızca eskisi (Oynat düğmesini tıklatın veya bunu yapmak için F5 tuşuna basın). Tarayıcınız başladıktan sonra Adres çubuğuna yolu değiştirmek `http://localhost:xx/HelloWorld` burada xx ne olursa olsun, bilgisayarınızın sayı seçilidir. Tarayıcınız aşağıdaki ekran görüntüsüne görünmelidir. Bizim yukarıdaki yönteminde size bir dize "İçerik" adlı bir yönteme geçirilen döndürdü. Biz, sistem yalnızca bazı HTML döndürür ve olduğu söylediyse!

ASP.NET MVC, gelen URL bağlı olarak farklı denetleyicisi sınıfları (ve bunların içindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan eşleme mantığı hangi kod çalıştığını denetlemek için bu gibi bir biçim kullanır:

/ [Denetleyicisi] / [EylemAdı] / [parametreler]

URL ilk bölümü yürütmek için denetleyici sınıfını belirler. Bu nedenle /HelloWorld HelloWorldController sınıfına eşler. URL ikinci bölümü eylem yöntemine yürütmek için sınıf belirler. Bu nedenle /HelloWorld/Index yürütmek için HelloWorldcontroller sınıfının İNDİS() yöntemi neden olur. Biz yalnızca yukarıdaki /HelloWorld ve dizin kapsanan yöntemi ziyaret etmek olduğunu unutmayın. "Dizin" adlı bir yöntem bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak varsayılan yöntemdir olmasıdır.

[![Bu my varsayılan değerdir](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Şimdi, şimdi ziyaret `http://localhost:xx/HelloWorld/Welcome.` artık bizim Hoş Geldiniz yöntemi yürütülen ve HTML dizesi döndürdü.

Yeniden / [denetleyicisi] / [EylemAdı] / [denetleyicisidir HelloWorld Hoş Geldiniz yöntemi bu durumda böylelikle parametreler]. Biz parametreleri henüz yapmadınız.

[![Hoş Geldiniz eylem yöntem budur](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Böylece biz bazı bilgileri URL'den örneğin gibi bizim denetleyicisine geçirebilirsiniz bizim örnek biraz değiştirelim: / HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4. Hoş Geldiniz yönteminizi iki parametre ve güncelleştirme gibi aşağıda içerecek şekilde değiştirin. C# isteğe bağlı parametre özelliği geçirilen değil, parametre numTimes 1 varsayılan belirtmek için kullandığımız olduğunu unutmayın.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Uygulamanızı çalıştırın ve ziyaret `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` dilediğiniz şekilde adı ve numtimes değerini değiştirme. Sistem, sorgu dizesi adres çubuğundaki yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşlenir.

Bu örneklerin her ikisi de denetleyicisi tüm çalışarak olmuştur ve HTML doğrudan döndürüyor. Normalde biz bizim denetleyicileri istemediğiniz kodu oldukça zahmetli olan bitip beri HTML doğrudan - döndürüyor. Bunun yerine genellikle ayrı bir görünüm şablon dosyası HTML yanıtı oluşturmak yardımcı olmak için kullanacağız. Nasıl bunu yapabiliriz konumundaki bakalım. Tarayıcınızı kapatın ve IDE döndürür.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part1.md)
> [sonraki](getting-started-with-mvc-part3.md)
