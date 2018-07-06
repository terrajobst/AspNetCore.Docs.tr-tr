---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programlama ASP.NET Web sayfaları (Razor) kullanarak Visual Studio | Microsoft Docs
author: tfitzmac
description: Bu ekte, Razor sözdizimi olan ASP.NET Web Pages programa Visual Studio 2010 veya Visual Web Developer 2010 Express'i nasıl kullanabileceğini açıklar.
ms.author: aspnetcontent
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 46807b464499b2e60d995d37f161ca129d38f439
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834461"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Visual Studio kullanarak ASP.NET Web sayfaları (Razor) programlama
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, programa ASP.NET Web sayfaları (Razor) Web siteleri nasıl Visual Studio veya Visual Web Developer Express kullanabileceğini açıklar.
> 
> Öğrenecekleriniz
> 
> - Ne ile ASP.NET Web sayfaları, Visual Studio sürümünde çalışmak için (her şey ise) yüklemeniz gerekir.
> - Visual Web Developer 2010 Express için ASP.NET Web sayfaları için destek ekleme konusunda.
> - IntelliSense ve hata ayıklayıcı dahil olmak üzere, ASP.NET Razor sayfaları kullanmaya çalışmak için Visual Studio özellikleri kullanma
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 ve WebMatrix 2 ile de çalışır.


WebMatrix veya diğer birçok kod düzenleyicileri kullanarak Razor sözdizimi olan ASP.NET Web sayfaları programlama yapabilirsiniz. Ayrıca, birçok türdeki uygulamayı (yalnızca Web siteleri) oluşturmak için güçlü bir dizi araç sağlar. bir tam özellikli tümleşik geliştirme ortamıdır (IDE) Microsoft Visual Studio da kullanabilirsiniz. ASP.NET Razor sayfaları ile çalışmak için Visual Studio'nun tüm sürümlerinde birini kullanın veya ücretsiz yapabilecekleriniz [Web için Visual Studio Express](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) sürümü.

ASP.NET Razor web sayfaları ile programlama için Visual Studio sağlayan iki özellikle yararlı özellikleri şunlardır:

- *IntelliSense*. Visual Studio'da yerleşik olarak bulunan IntelliSense WebMatrix Intellisense'te daha kapsamlı bir özelliktir.
- *Hata ayıklayıcı*. Hata ayıklayıcı kodunuz bir program çalıştırma, değişkenleri inceleme ve satır kod içerisinde ilerlemeye durdurarak gidermenize olanak tanır.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Visual Studio kullanarak ASP.NET Web sayfaları farklı sürümleri

Visual Studio 2012 ve Visual Studio 2013 için ASP.NET Web Pages destek içerir. (Visual Studio yüklediğinizde ASP.NET Web Pages desteklemek için gerekli paketleri yüklenir.)

Visual Studio 2010 desteği ASP.NET Web sayfaları için varsayılan olarak içermez. ASP.NET Web sayfaları Visual Studio 2010 ile kullanmak için ASP.NET MVC paketini yüklemeniz gerekir. ASP.NET Web Pages 2 almak için ASP.NET MVC 4 yükleyin.

Aşağıdaki tablo farklı Visual Studio sürümlerinde ASP.NET Web sayfaları için destek özetler.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web sayfaları 2** | ASP.NET MVC 4 yükleyin | (Dahil) | (Dahil) |
| **ASP.NET Web sayfaları 3** |  | ASP.NET Web için güncelleştirme 3 ile NuGet sayfaları | (Dahil) |

Visual Studio 2010 ile çalışmak için bkz [yükleme desteği için ASP.NET Web sayfaları Visual Studio 2010'daki](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>WebMatrix Visual Studio'dan başlatmak

Bir proje Webmatrix'te başlatılmış ve Visual Studio'ya geçmek istiyorsanız, WebMatrix kolayca projeyi Visual Studio'da açmak için bir düğme sağlar. Bu düğme, bilgisayarınızda yüklü etkinleştirilmesi için Visual Studio olması gerekir. Aşağıdaki görüntüde Webmatrix'te düğmesini gösterir.

![Visual Studio'yu başlatın](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Düğmeye tıkladığınızda, projeyi Visual Studio'da açılır. WebMatrix ve Visual Studio arasında ileri ve geri sorunsuz geçebilirsiniz. Herhangi bir dosya diğer ortamda değiştirdiyseniz ve en son değişiklikleri almak için yeniden yüklenmesi gerekli size bildirilir.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Visual Studio'da ASP.NET Razor Site oluşturma

Visual Studio'da bir ASP.NET Razor Web sitesi oluşturmak için:

1. Visual Studio veya Visual Web Developer başlatın.
2. İçinde **dosya** menüsünde tıklatın **yeni Web sitesi**.

    ![Yeni web sitesi oluşturma](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. İçinde **yeni Web sitesi** iletişim kutusunda, kullanılacak dili (Visual C# veya Visual Basic) seçin.
4. Seçin **ASP.NET Web sitesi (Razor)** şablonu.

    ![Razor site](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. **Tamam**'ı tıklatın.

Yeni projeniz var ve bazı varsayılan başlamanıza yardımcı olmak için web sayfaları ile doldurulur.

### <a name="using-intellisense"></a>IntelliSense Kullanma

Bir site oluşturduğunuza göre IntelliSense Visual Studio'da nasıl çalıştığını görebilirsiniz.

1. Yeni oluşturduğunuz Web sitesinde, açık *Default.cshtml* sayfası.
2. Sonra `<h3>` etiketler sayfasında yazın `@ServerInfo.` (nokta dahil olmak üzere). IntelliSense için kullanılabilen yöntemler nasıl görüntülendiğine dikkat edin `ServerInfo` açılır listede yok. 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Seçin `GetHtml` yöntemi listesi ve Enter tuşuna basın. IntelliSense otomatik yöntemini doldurur. (C# herhangi bir yöntemle eklemeniz gerekir gibi `()` yöntemi sonra karakter.)  
   Tamamlanan kodu `GetHtml` yöntemi aşağıdaki örnekteki gibi görünür:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Sayfayı çalıştırmak için CTRL + F5 tuşlarına basın. Bu sayfanın bir tarayıcıda görüntülenen nasıl göründüğünü oluşur: 

    ![varsayılan sayfasını tarayıcıda](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Tarayıcıyı kapatın.

### <a name="using-the-debugger"></a>Hata ayıklayıcıyı kullanma

1. Üst kısmındaki *Default.cshtml* ile başlayan satırı sonra bir sayfa `Page.Title`, aşağıdaki kod satırını ekleyin: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Eklemek için bu yeni satırın yanındaki sol tarafındaki Kod Düzenleyicisi'ni gri kenar boşluğunda tıklayın bir *kesme noktası*. Bir kesme noktası programın bu noktada neler olduğunu gördüğünüz şekilde çalışmayı durdurmasına hata ayıklayıcı söyleyen bir işaretçidir.

    ![kesme noktası Ayarla](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Çağrısını kaldırın `ServerInfo.GetHtml` yöntemi ve bir çağrı ekleyin `@myTime` bunun yerine değişken. Bu çağrı yeni kod satırı tarafından döndürülen geçerli saat değerini görüntüler.
4. Hata Ayıklayıcısı'nda sayfayı çalıştırmak için F5 tuşuna basın. Sayfa ayarladığınız kesme noktasına durdurur. Aşağıdaki görüntüde, Sayfa Düzenleyicisi'nde kesme (sarı) ile nasıl göründüğünü gösterir. 

    ![hata ayıklama kesme noktası](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Hata ayıklama araç çubuğunda tıklatın **içine adımla** sonraki kod satırına çalıştırmak için düğme (veya F11 tuşuna basın). Bu düğmeye tıkladığınızda her zaman yürütme kodun sonraki satırına geçin.

    ![Düğme adım](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Değerini incelemek `myTime` üzerine fare işaretçisini tutarak veya görüntülenen değerleri inceleyerek değişken **Yereller** ve **çağrı yığını** windows. Visual Studio, değişkenin değerini görüntüler.

    ![zamanın gösterilme biçimi değeri](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Değişken inceleme ve kodu Adımlayarak işiniz bittiğinde, sayfa her satırında durdurmadan çalıştırmaya devam etmek için F5 tuşuna basın. Tüm kod içerisinde ilerlemeye bitirdiğinizde, tarayıcı sayfasını görüntüler.

Hata ayıklayıcı ve Visual Studio kodu hatalarını ayıklama hakkında daha fazla bilgi için bkz. [izlenecek yol: Web sayfalarında hata ayıklamayı Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Visual Studio ile ASP.NET MVC projeleri Razor kullanma

Razor sözdizimi ASP.NET MVC projeleri oluşturulurken sıkça kullanılır. MVC, dinamik Web siteleri oluşturmak için güçlü, desen tabanlı bir yoludur. ASP.NET Web sayfaları sitesinde bakımını yapmak zor hale gelirse, bir ASP.NET MVC uygulamasını dönüştürme düşünmek isteyebilirsiniz. Bir MVC uygulaması oluşturma örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Visual Studio 2010'da ASP.NET Web Pages desteğini yükleme

Bu bölümde, Visual Web Developer Express 2010 ve ASP.NET Web sayfaları araçları, Visual Studio için yükleme işlemi gösterilmektedir.

1. Web Platformu yükleyicisi zaten sahip değilseniz, şu URL'den indirin:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Web Platformu Yükleyicisi'ni çalıştırın.
3. Tıklayın **ürünleri** sekmesi.

    ![Webpı ürünleri sekmesi](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Arama **ASP.NET MVC 4** (için ASP.NET Web Pages 2) ve ardından **Ekle**. Bu ürünler, ASP.NET Razor Web siteleri oluşturmak için Visual Studio araçları içerir.

    ![Webpı yükleme seçenekleri](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Tıklayın **yükleme** yüklemeyi tamamlamak için.
