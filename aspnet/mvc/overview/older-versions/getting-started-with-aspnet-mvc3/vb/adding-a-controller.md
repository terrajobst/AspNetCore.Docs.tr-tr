---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Bir denetleyici (VB) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9a433083c31c7929f7599e52800c887f301d7727
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30870615"
---
<a name="adding-a-controller-vb"></a>Bir denetleyici (VB) ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-controller.md) Bu öğreticinin.


MVC anlamına gelir *model-view-controller*. MVC her bölümü ayrı bir sorumluluk sahip olacağı şekilde uygulamaları geliştirmek için bir modeli aşağıdaki gibidir:

- Model: Uygulamanız için verileri.
- Görünümleri: Dinamik olarak HTML yanıtları oluşturmak için uygulamanızın şablon dosyalarını kullanır.
- Denetleyicileri: uygulama gelen URL isteklerini işleyen sınıflar model verileri alabilir ve istemciye yanıt işleme görünüm şablonları belirtin.

Biz, bu öğreticide, tüm bu kavramları kapsayan olması ve bunları bir uygulama oluşturmak için nasıl kullanılacağını gösterir.

Yeni bir denetleyicisi oluşturmak için *denetleyicileri* klasöründe **Çözüm Gezgini** seçilerek **denetleyici Ekle**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Yeni denetleyicinize ad &quot;HelloWorldController&quot; tıklatıp **Ekle**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Fark **Çözüm Gezgini** sağ tarafta adlı yeni bir dosya sizin için oluşturulmuş olduğundan *HelloWorldController.cs* ve dosya IDE içinde açılmış durumda.

Yeni içinde `public class HelloWorldController` engellemek, aşağıdaki kod gibi görünen iki yeni yöntemi oluşturun. Örnek olarak HTML dizesi doğrudan denetleyicisinden getireceğiz.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Denetleyicinizi adlı `HelloWorldController` ve yeni yönteminizi adlı `Index`. Uygulama (F5 veya Ctrl + F5'e basın) çalıştırın. Tarayıcınız başladıktan sonra sona &quot;HelloWorld&quot; adres çubuğundaki yolu. (Bilgisayarımda, bunun `http://localhost:43246/HelloWorld`) tarayıcınız aşağıdaki ekran görüntüsüne görünür. Yukarıdaki yöntemi kodu doğrudan bir dize döndürdü. Biz yalnızca bazı HTML döndürmek için sistem söylediyse ve olduğu!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC, gelen URL bağlı olarak farklı bir denetleyici sınıfları (ve bunların içindeki farklı eylem yöntemleri) çağırır. ASP.NET MVC tarafından kullanılan varsayılan eşleme mantığı hangi kod çağrılır denetlemek için bu gibi bir biçim kullanır:

`/[Controller]/[ActionName]/[Parameters]`

URL ilk bölümü yürütmek için denetleyici sınıfını belirler. Bu nedenle */HelloWorld* eşlendiği `HelloWorldController` sınıfı. URL ikinci bölümü eylem yöntemine yürütmek için sınıf belirler. Bu nedenle */HelloWorld/dizin* neden olacağından `Index` yöntemi `HelloWorldController` yürütmek için sınıf. Biz yalnızca ziyaret gerekiyordu bildirimi */HelloWorld* yukarıda ve `Index` yöntemi, varsayılan olarak kullanıldı. Bir yöntem adlı olmasıdır `Index` bir açıkça belirtilmezse, bir denetleyicisinde çağrılacak varsayılan yöntemdir.

Gözat `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Yöntemi çalışır ve bir dize döndürür &quot;Hoş Geldiniz eylem yöntem budur... &quot;. Varsayılan MVC eşlemesi `/[Controller]/[ActionName]/[Parameters]`. Bu URL için denetleyicisidir `HelloWorld` ve `Welcome` yöntemidir. Biz kullanmadıysanız `[Parameters]` URL henüz parçası.

![](adding-a-controller/_static/image6.png)

Böylece biz bazı parametre bilgilerini denetleyiciye URL'den geçirebilirsiniz örnek biraz değiştirelim (örneğin, */HelloWorld/Hoş Geldiniz? adı Scott =&amp;numtimes = 4*). Değişiklik, `Welcome` yöntemi iki parametre aşağıda gösterildiği gibi ekleyin. Biz VB isteğe bağlı parametre özelliği belirtmek için kullandığınız Not `numTimes` Bu parametre için değer iletilmezse, parametresi 1 olarak varsayılan.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Uygulamanızı çalıştırın ve Gözat `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** İçin farklı değerler deneyin `name` ve `numtimes`. Sistem sorgu dizenizi adres çubuğundaki yönteminizi parametrelerinde adlandırılmış parametreleri otomatik olarak eşlenir.

![](adding-a-controller/_static/image7.png)

MVC VC kısmı yapılması edilmiş hem bu örneklerde denetleyicisi — Görünüm ve denetleyici iş olmasıdır. Denetleyici HTML doğrudan döndürüyor. Normalde biz kodu çok kullanışsız hale beri HTML doğrudan döndürerek denetleyicileri istemezsiniz. Bunun yerine genellikle ayrı görünüm şablon dosyası HTML yanıtı oluşturmak yardımcı olmak için kullanacağız. Nasıl bunu yapabiliriz konumundaki bakalım.

> [!div class="step-by-step"]
> [Önceki](intro-to-aspnet-mvc-3.md)
> [sonraki](adding-a-view.md)
