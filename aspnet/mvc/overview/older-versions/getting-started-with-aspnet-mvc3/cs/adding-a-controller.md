---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Denetleyici (C#) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Serivice Paketi 1, hangi i kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: ba6cc715f8c8eaf624ab5314e3afbfd68da11485
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370898"
---
<a name="adding-a-controller-c"></a>Denetleyici (C#) ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir. [C# sürümü indirme](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.


MVC anlamına gelen *model-view-controller*. MVC, iyi düzenlenmiş ve sürdürmek daha kolay olan uygulamaları geliştirmek için kullanılan bir desendir. MVC tabanlı uygulamalar içerir:

- Denetleyicileri: uygulama için gelen istekleri işleyen sınıflar model verileri almak ve sonra istemciye bir yanıt döndüreceğini görünüm şablonları belirtin.
- Modelleri: uygulama verilerini temsil eden ve bu veriler için iş kuralları zorlamak için doğrulama mantığını kullanan sınıflar.
- Görünümler: dinamik olarak HTML yanıtları oluşturmak için uygulamanızın kullandığı şablon dosyaları.

Biz Bu öğretici serisinde, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

Denetleyici sınıfı oluşturarak başlayalım. İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **denetleyici Ekle**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Yeni denetleyicinize "HelloWorldController" olarak adlandırın. Varsayılan şablonu olarak bırakın **boş denetleyici** tıklatıp **Ekle**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

İçinde fark **Çözüm Gezgini** yeni bir dosya adlandırılmış oluşturulduğunu *HelloWorldController.cs*. Dosya, IDE'de açık durumdadır.

![](adding-a-controller/_static/image5.png)

İçinde `public class HelloWorldController` engelleme, şu kod gibi görünen iki yöntem oluşturun. Denetleyici bir HTML dizesi bir örnek döndürür.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Denetleyicinizi adlı `HelloWorldController` ve yukarıdaki ilk yöntem adlı `Index`. Şimdi bir tarayıcıdan çağırın. (F5 ya da Ctrl + F5 tuşlarına basın) uygulamayı çalıştırın. Tarayıcıda, yolun adres çubuğundaki "HelloWorld" ekleyin. (Örneğin, çizimdeki Aşağıda, onun `http://localhost:43246/HelloWorld.`) sayfasını tarayıcıda aşağıdaki ekran görüntüsüne benzer görünür. Yukarıdaki yönteminde kodu doğrudan bir dize döndürdü. Yalnızca bazı HTML döndürmek için sistem edilmesini ve kişiselleştirmeden!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC, gelen URL bağlı olarak farklı denetleyici sınıflarına (ve içlerindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan eşleme mantığı çağırmak için hangi kodu belirlemek için bu gibi bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

URL'nin ilk bölümünü yürütmek için denetleyici sınıfını belirler. Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı. URL ikinci bölümü yürütmek için bir sınıf üzerinde eylem yöntemini belirler. Bu nedenle */HelloWorld/dizin* neden `Index` yöntemi `HelloWorldController` yürütmek için sınıf. Yalnızca gözatmak için vardı bildirimi */HelloWorld* ve `Index` yöntemi varsayılan olarak kullanıldı. Adlı bir yöntem `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak için varsayılan yöntemdir.

Gözat `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve "Bu bir Hoş Geldiniz eylem yönteminin..." dizesini döndürür. Varsayılan MVC eşleme `/[Controller]/[ActionName]/[Parameters]`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemi. Kullanmadığınız `[Parameters]` henüz URL parçası.

![](adding-a-controller/_static/image7.png)

Bazı parametre bilgileri URL'den denetleyiciye geçirebilmeniz şimdi örneği biraz değiştirin (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*). Değişiklik, `Welcome` aşağıda gösterildiği gibi iki parametre eklemek için yöntemi. Kodu göstermek için C# isteğe bağlı parametre özelliği kullandığına dikkat edin `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uygulamanızı çalıştırın ve örnek URL'ye Gözat (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. İçin farklı değerler deneyebilirsiniz `name` ve `numtimes` URL. Sistem otomatik olarak adlandırılmış parametreleri adres çubuğundaki Sorgu dizesinden yönteminizde parametrelerine eşler.

![](adding-a-controller/_static/image8.png)

MVC "VC" bölümünü yapılması edilmiş hem de bu örneklerde denetleyici — diğer bir deyişle, Görünüm ve denetleyici iş. Denetleyici HTML doğrudan döndürüyor. Normalde, kod için çok kullanışsız olur beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine genellikle ayrı görünümü şablon dosyası HTML yanıtını oluşturmak amacıyla kullanacağız. Nasıl biz bunu yapmak için sonraki göz atalım.

> [!div class="step-by-step"]
> [Önceki](intro-to-aspnet-mvc-3.md)
> [İleri](adding-a-view.md)
