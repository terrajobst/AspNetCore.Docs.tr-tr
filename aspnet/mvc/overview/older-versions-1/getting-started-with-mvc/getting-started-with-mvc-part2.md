---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Denetleyici ekleme | Microsoft Docs
author: shanselman
description: Bu öğreticide Visual Studio 2013 burada kullanarak kullanılabiliyorsa, güncelleştirilmiş bir sürüm. Yeni t birçok iyileştirme sağlayan ASP.NET MVC 5 öğreticide...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 856b7d98ac8bd30982d81b0609bb9c1288e07e49
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755356"
---
<a name="adding-a-controller"></a>Denetleyici ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Bu öğreticide kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 yeni öğretici kullanır.
> 
> 
> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


MVC, Model, görünüm, denetleyici gösterir. MVC uygulamaları geliştirmek için her bölümü başka bir uygulamadan farklı bir sorumluluğa sahip olacak şekilde bir desendir.

- Modeli: Uygulamanızın veri
- Görünümler: Şablon dosyalarını, dinamik olarak HTML yanıtları oluşturmak için uygulamanızı kullanır.
- Denetleyicileri: uygulama için gelen URL isteklerini işleyen sınıflar model verileri almak ve ardından istemcisine bir yanıt oluşturan şablonları göster belirtin

Biz bu öğreticide, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

Yeni bir denetleyici Çözüm Gezgini içindeki denetleyicileri klasörüne sağ tıklayarak ve denetleyici Ekle seçerek oluşturalım.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Yeni denetleyicinize "HelloWorldController" olarak adlandırın ve Ekle'ye tıklayın.

[![Denetleyici Ekle iletişim kutusu](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Çözüm Gezgini'nde HelloWorldController.cs adlı sizin için yeni bir dosya oluşturulduğunda ve bu dosya artık açıldığında, sağ taraftaki bildirimi **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Yeni ortak sınıfının HelloWorldController içinde şuna iki yeni yöntem oluşturun. Örneğin bir HTML dizesi doğrudan bizim denetleyicisinden getireceğiz.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Denetleyicinizi HelloWorldController olarak adlandırılır ve yeni yönteminizi dizin adı verilir. Uygulamanızı yeniden çalıştırmak kadar önceki (Yürüt düğmesini tıklatın veya bunu yapmak için F5 tuşuna basın). Tarayıcınız başlatıldıktan sonra Adres çubuğuna yolu değiştirmek `http://localhost:xx/HelloWorld` xx ne olursa olsun, bilgisayarınızın numarası olduğu seçildi. Tarayıcınız ekran aşağıdaki gibi görünmelidir. Bizim yöntemi yukarıdaki biz "İçerik" adlı bir yönteme geçirilen bir dizesi döndürdü. Biz, sistem yalnızca bazı HTML döndürür ve kişiselleştirmeden söyledi!

ASP.NET MVC, gelen URL bağlı olarak farklı denetleyici sınıflarına (ve içlerindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan eşleme mantığı, hangi kodun çalıştığını denetlemek için bu gibi bir biçim kullanır:

/ [Controller] / [ActionName] / [parametreler]

URL'nin ilk bölümünü yürütmek için denetleyici sınıfını belirler. Bu nedenle /HelloWorld HelloWorldController sınıfa eşler. URL ikinci bölümü yürütmek için bir sınıf üzerinde eylem yöntemini belirler. Bu nedenle /HelloWorld/Index yürütülecek HelloWorldcontroller sınıfının İNDİS() yöntemi neden olur. Yalnızca yukarıdaki /HelloWorld ve dizin örtük yöntemi ziyaret etmek vardı, dikkat edin. Bu durum, "Index" adlı bir yöntem bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak için varsayılan yöntemdir olmasıdır.

[![Bu benim varsayılan değerdir](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Şimdi, şimdi ziyaret `http://localhost:xx/HelloWorld/Welcome.` artık bizim Hoş Geldiniz yöntemi yürütüldüğünde ve kendi HTML dizesi döndürdü.

Yeniden / [Controller] / [ActionName] / [parametreler] HelloWorld denetleyicisidir ve yöntemi bu durumda olan Hoş Geldiniz. Şu parametre henüz yapmadınız.

[![Hoş Geldiniz eylem yöntemi budur.](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Biz bazı bilgileri URL'den örneğin gibi bizim denetleyicisine geçirebilirsiniz şimdi örneğimizi biraz değiştirin: / HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4. Hoş Geldiniz yönteminizi iki parametreleri ve güncelleştirme gibi aşağıda içerecek şekilde değiştirin. C# isteğe bağlı parametre özelliği geçirilen değil, parametre numTimes 1 varsayılan belirtmek için kullandığımız olduğunu unutmayın.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Uygulamanızı çalıştırın ve ziyaret `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` istediğiniz adı ve numtimes değerinin değiştirilmesi. Sistem sorgu dizenizi adres çubuğundaki yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşlenir.

Bu örneklerin her ikisi de denetleyici tüm iş yapan olmuştur ve HTML doğrudan döndürüyor. Bizim denetleyicileri normalde istiyoruz olmayan HTML kodu için çok kullanışsız olan biten olduğundan doğrudan - döndürüyor. Bunun yerine genellikle ayrı bir görünüm şablon dosyası HTML yanıtını oluşturmak amacıyla kullanacağız. Nasıl bu yapabiliriz bakalım. Tarayıcınızı kapatın ve IDE'ye dönün.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part1.md)
> [İleri](getting-started-with-mvc-part3.md)
