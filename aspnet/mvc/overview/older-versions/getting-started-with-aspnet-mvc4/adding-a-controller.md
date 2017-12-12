---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Bir denetleyici ekleme | Microsoft Docs
author: Rick-Anderson
description: "Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>Bir denetleyici ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.


MVC anlamına gelir *model-view-controller*. MVC, iyi tasarlanmış, test edilebilir ve bakımını kolay uygulamaları geliştirmek için bir desen olur. MVC tabanlı uygulamalar içerir:

- **M** odels: uygulama verilerini temsil eden ve bu verilere iş kurallarını zorunlu tutmak için doğrulama mantığını kullanan sınıfları.
- **V** iews: dinamik olarak HTML yanıtları oluşturmak için uygulamanızın kullandığı şablon dosyalarını.
- **C** ontrollers: gelen tarayıcı istekleri işleyen sınıflar model verileri almak ve tarayıcıya bir yanıt döndürür görünüm şablonları belirtin.

Biz, Bu öğretici serisinde, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

Denetleyici sınıfı oluşturarak başlayalım. İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörünü ve ardından **denetleyici Ekle**.

![](adding-a-controller/_static/image1.png)

Yeni denetleyicinize ad &quot;HelloWorldController&quot;. Varsayılan şablon olarak bırakın **boş MVC denetleyicisi** tıklatıp **Ekle**.

![Denetleyici ekleme](adding-a-controller/_static/image2.png)

Fark **Çözüm Gezgini** yeni bir dosya adlandırılmış oluşturulduğunu *HelloWorldController.cs*. Dosya IDE içinde açılmış durumda.

![](adding-a-controller/_static/image3.png)

Dosya içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Denetleyici yöntemleri HTML dizesi örnek olarak döndürür. Denetleyici adlı `HelloWorldController` ve yukarıdaki ilk yöntem adlı `Index`. Şimdi bir tarayıcıdan çağırır. Uygulama (F5 veya Ctrl + F5'e basın) çalıştırın. Tarayıcıda append &quot;HelloWorld&quot; adres çubuğundaki yolu. (Örneğin, aşağıdaki, çizimdeki `http://localhost:1234/HelloWorld.`) sayfasını tarayıcıda aşağıdaki ekran görüntüsüne benzeyen arar. Yukarıdaki yöntemi kodu doğrudan bir dize döndürdü. Yalnızca bazı HTML döndürmek için sistem söylediyse ve olduğu!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC, gelen URL bağlı olarak farklı bir denetleyici sınıfları (ve bunların içindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan URL yönlendirme mantığı çağırmak için kodu nedir belirlemek için bu gibi bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

URL ilk bölümü yürütmek için denetleyici sınıfını belirler. Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı. URL ikinci bölümü eylem yöntemine yürütmek için sınıf belirler. Bu nedenle */HelloWorld/dizin* neden olacağından `Index` yöntemi `HelloWorldController` yürütmek için sınıf. Biz yalnızca Gözat gerekiyordu bildirimi */HelloWorld* ve `Index` yöntemi, varsayılan olarak kullanıldı. Bir yöntem adlı olmasıdır `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak varsayılan yöntemdir.

Gözat `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve bir dize döndürür &quot;Hoş Geldiniz eylem yöntem budur... &quot;. Varsayılan MVC eşlemesi `/[Controller]/[ActionName]/[Parameters]`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` eylem yöntemidir. Kullandığınız parolalardan `[Parameters]` URL henüz parçası.

![](adding-a-controller/_static/image5.png)

Böylece denetleyiciye URL'den bazı parametre bilgilerini geçirebilirsiniz örnek biraz değiştirelim (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*). Değişiklik, `Welcome` yöntemi iki parametre aşağıda gösterildiği gibi ekleyin. Kod belirtmek için C# isteğe bağlı parametre özelliği kullandığına dikkat edin `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uygulamanızı çalıştırın ve örnek URL'sine gidin (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. İçin farklı değerler deneyin `name` ve `numtimes` URL. [ASP.NET MVC model bağlama sistem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) adres çubuğundaki sorgu dizesi yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşlenir.

![](adding-a-controller/_static/image6.png)

Her iki Bu örneklerde denetleyicisi bulunurken &quot;VC&quot; MVC kısmı — diğer bir deyişle, Görünüm ve denetleyici çalışma. Denetleyici HTML doğrudan döndürüyor. Normalde kodu çok kullanışsız hale beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine genellikle ayrı görünüm şablon dosyası HTML yanıtı oluşturmak yardımcı olmak için kullanacağız. Nasıl biz bunu yapmak için sonraki olarak bakalım.

>[!div class="step-by-step"]
[Önceki](intro-to-aspnet-mvc-4.md)
[sonraki](adding-a-view.md)
