---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Denetleyici ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 772f3c87d6b73b324164bf9619d332a9c01d7476
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756137"
---
<a name="adding-a-controller"></a>Denetleyici ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.


MVC anlamına gelen *model-view-controller*. MVC, iyi düzenlenmiş, test edilebilir ve sürdürmek daha kolay olan uygulamaları geliştirmek için kullanılan bir desendir. MVC tabanlı uygulamalar içerir:

- **M** odels: uygulama verilerini temsil eden ve bu veriler için iş kuralları zorlamak için doğrulama mantığını kullanan sınıflar.
- **V** iews: dinamik olarak HTML yanıtları oluşturmak için uygulamanızın kullandığı şablon dosyaları.
- **C** ontrollers: gelen tarayıcı istekleri işleyen sınıflar model verileri almak ve tarayıcıya bir yanıt döndüreceğini görünüm şablonları belirtin.

Biz Bu öğretici serisinde, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

Denetleyici sınıfı oluşturarak başlayalım. İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **denetleyici Ekle**.

![](adding-a-controller/_static/image1.png)

Yeni denetleyicinize ad &quot;HelloWorldController&quot;. Varsayılan şablonu olarak bırakın **boş MVC denetleyicisi** tıklatıp **Ekle**.

![Denetleyici ekleme](adding-a-controller/_static/image2.png)

İçinde fark **Çözüm Gezgini** yeni bir dosya adlandırılmış oluşturulduğunu *HelloWorldController.cs*. Dosya, IDE'de açık durumdadır.

![](adding-a-controller/_static/image3.png)

Dosyanın içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Denetleyici yöntemleri HTML dizesi bir örnek döndürür. Denetleyici adındaki `HelloWorldController` ve yukarıdaki ilk yöntem adlı `Index`. Şimdi bir tarayıcıdan çağırın. (F5 ya da Ctrl + F5 tuşlarına basın) uygulamayı çalıştırın. Tarayıcıda ekleme &quot;HelloWorld&quot; adres çubuğundaki yolu. (Örneğin, çizimdeki Aşağıda, onun `http://localhost:1234/HelloWorld.`) sayfasını tarayıcıda aşağıdaki ekran görüntüsüne benzer görünür. Yukarıdaki yönteminde kodu doğrudan bir dize döndürdü. Yalnızca bazı HTML döndürmek için sistem edilmesini ve kişiselleştirmeden!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC, gelen URL bağlı olarak farklı denetleyici sınıflarına (ve içlerindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan URL yönlendirme mantığı çağırmak için hangi kodu belirlemek için bu gibi bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

URL'nin ilk bölümünü yürütmek için denetleyici sınıfını belirler. Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı. URL ikinci bölümü yürütmek için bir sınıf üzerinde eylem yöntemini belirler. Bu nedenle */HelloWorld/dizin* neden `Index` yöntemi `HelloWorldController` yürütmek için sınıf. Yalnızca gözatmak için vardı bildirimi */HelloWorld* ve `Index` yöntemi varsayılan olarak kullanıldı. Adlı bir yöntem `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak için varsayılan yöntemdir.

konumuna gözatın `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve bir dize döndürür &quot;Hoş Geldiniz eylem yöntemi budur... &quot;. Varsayılan MVC eşleme `/[Controller]/[ActionName]/[Parameters]`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemi. Kullanmadığınız `[Parameters]` henüz URL parçası.

![](adding-a-controller/_static/image5.png)

Bazı parametre bilgileri URL'den denetleyiciye geçirebilmeniz şimdi örneği biraz değiştirin (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*). Değişiklik, `Welcome` aşağıda gösterildiği gibi iki parametre eklemek için yöntemi. Kodu göstermek için C# isteğe bağlı parametre özelliği kullandığına dikkat edin `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uygulamanızı çalıştırın ve örnek URL'ye Gözat (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. İçin farklı değerler deneyebilirsiniz `name` ve `numtimes` URL. [ASP.NET MVC model bağlama sistemi](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) yönteminizi parametrelerinde adres çubuğundaki Sorgu dizesinden adlandırılmış parametreleri otomatik olarak eşlenir.

![](adding-a-controller/_static/image6.png)

Hem bu örneklerde denetleyicisi bulunurken &quot;VC&quot; MVC bölümü — diğer bir deyişle, Görünüm ve denetleyici iş. Denetleyici HTML doğrudan döndürüyor. Normalde, kod için çok kullanışsız olur beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine genellikle ayrı görünümü şablon dosyası HTML yanıtını oluşturmak amacıyla kullanacağız. Nasıl biz bunu yapmak için sonraki göz atalım.

> [!div class="step-by-step"]
> [Önceki](intro-to-aspnet-mvc-4.md)
> [İleri](adding-a-view.md)
