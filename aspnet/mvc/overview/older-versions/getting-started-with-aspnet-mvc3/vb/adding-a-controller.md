---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Denetleyici (VB) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: fc44a6825c08f2c04dbc1a1332d46771da70b149
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755538"
---
<a name="adding-a-controller-vb"></a>Denetleyici (VB) ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-controller.md) Bu öğreticinin.


MVC anlamına gelen *model-view-controller*. MVC uygulamaları geliştirmek için her bölümü ayrı bir sorumluluğa sahip olacak şekilde modelidir:

- Modeli: Uygulamanız için veriler.
- Görünümler: Şablon dosyalarını, dinamik olarak HTML yanıtları oluşturmak için uygulamanızı kullanır.
- Denetleyicileri:, uygulamaya gelen URL isteklerini işleyen sınıflar model verileri almak ve sonra bir yanıtı istemciye işleme görünüm şablonları belirtin.

Biz bu öğreticide, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

Sağ tıklayarak yeni bir denetleyici oluşturma *denetleyicileri* klasöründe **Çözüm Gezgini** seçip **denetleyici Ekle**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Yeni denetleyicinize ad &quot;HelloWorldController&quot; tıklatıp **Ekle**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

İçinde fark **Çözüm Gezgini** sağ tarafta yeni bir dosya sizin için oluşturulmuş olduğundan adlı *HelloWorldController.cs* ve dosyanın IDE'de açık olduğunu.

İçinde yeni `public class HelloWorldController` engelleme, şu kod gibi görünen iki yeni yöntem oluşturun. Örneğin bir HTML dizesi doğrudan denetleyicisinden getireceğiz.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Denetleyicinizi adlı `HelloWorldController` ve yeni yönteminizi adlı `Index`. (F5 ya da Ctrl + F5 tuşlarına basın) uygulamayı çalıştırın. Tarayıcınız başlatıldıktan sonra ekleme &quot;HelloWorld&quot; adres çubuğundaki yolu. (Bilgisayarımda, sahip `http://localhost:43246/HelloWorld`) tarayıcınız ekran aşağıdaki gibi görünür. Yukarıdaki yönteminde kodu doğrudan bir dize döndürdü. Biz yalnızca bazı HTML döndürmek için sistem bir uyarıyla ve kişiselleştirmeden!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC, gelen URL bağlı olarak farklı denetleyici sınıflarına (ve içlerindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan eşleme mantığı, hangi kod çağrılan denetlemek için bu gibi bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

URL'nin ilk bölümünü yürütmek için denetleyici sınıfını belirler. Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı. URL ikinci bölümü yürütmek için bir sınıf üzerinde eylem yöntemini belirler. Bu nedenle */HelloWorld/dizin* neden `Index` yöntemi `HelloWorldController` yürütmek için sınıf. Yalnızca ziyaret etmek için vardı bildirimi */HelloWorld* yukarıda ve `Index` yöntemi varsayılan olarak kullanıldı. Adlı bir yöntem `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak için varsayılan yöntemdir.

konumuna gözatın `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve bir dize döndürür &quot;Hoş Geldiniz eylem yöntemi budur... &quot;. Varsayılan MVC eşleme `/[Controller]/[ActionName]/[Parameters]`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` yöntemidir. Biz kullanmadığınız `[Parameters]` henüz URL parçası.

![](adding-a-controller/_static/image6.png)

Biz bazı parametre bilgileri denetleyiciye URL'den geçirebilmeniz şimdi örneği biraz değiştirin (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*). Değişiklik, `Welcome` aşağıda gösterildiği gibi iki parametre eklemek için yöntemi. Biz VB isteğe bağlı parametre özelliği belirtmek için kullandığınız Not `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Uygulamanızı çalıştırın ve göz atın `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** İçin farklı değerler deneyebilirsiniz `name` ve `numtimes`. Sistem, adres çubuğuna, sorgu dizesinde yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşler.

![](adding-a-controller/_static/image7.png)

MVC VC kısmı yapılması edilmiş hem bu örneklerde denetleyici — Görünüm ve denetleyici iş olmasıdır. Denetleyici HTML doğrudan döndürüyor. Normalde, kod için çok kullanışsız olur beri HTML doğrudan döndürerek denetleyicileri istiyoruz yok. Bunun yerine genellikle ayrı görünümü şablon dosyası HTML yanıtını oluşturmak amacıyla kullanacağız. Nasıl bu yapabiliriz bakalım.

> [!div class="step-by-step"]
> [Önceki](intro-to-aspnet-mvc-3.md)
> [İleri](adding-a-view.md)
