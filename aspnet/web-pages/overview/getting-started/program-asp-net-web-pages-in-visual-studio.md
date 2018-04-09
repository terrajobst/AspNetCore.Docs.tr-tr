---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programlama ASP.NET Web sayfaları (Razor) kullanarak Visual Studio | Microsoft Docs
author: tfitzmac
description: Bu ekte, Razor sözdizimi ile ASP.NET Web sayfaları programa Visual Studio 2010 veya Visual Web Developer 2010 Express nasıl kullanabileceğinizi açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Visual Studio kullanarak ASP.NET Web sayfaları (Razor) programlama
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, ASP.NET Web sayfaları (Razor) Web sitelerine programına nasıl Visual Studio veya Visual Web Developer Express kullanabilirsiniz açıklanmaktadır.
> 
> Öğrenecekleriniz
> 
> - Ne ile ASP.NET Web sayfaları, Visual Studio sürümünde çalışması için (her şey varsa) yüklemeniz gerekir.
> - Visual Web Developer 2010 Express için ASP.NET Web sayfaları için destek eklemek nasıl.
> - Özelliklerin Visual Studio'da IntelliSense ve hata ayıklayıcısı olsa da dahil olmak üzere, ASP.NET Razor sayfaları ile çalışmak için nasıl kullanılacağını.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 ve WebMatrix 2 ile de çalışır.


WebMatrix veya diğer birçok kod düzenleyiciler kullanarak Razor sözdizimi ile ASP.NET Web sayfaları programlama yapabilirsiniz. Microsoft Visual birçok türdeki uygulamayı (yalnızca Web siteleri) oluşturmak için güçlü birtakım araçlar sağlayan bir tam özellikli tümleşik geliştirme ortamı (IDE) olan Studio de kullanabilirsiniz. ASP.NET Razor sayfaları ile çalışmak için Visual Studio tam sürümleri birini kullanın ya da ücretsiz yapabilecekleriniz [Web için Visual Studio Express](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) sürümü.

ASP.NET Razor web sayfaları ile programlama için Visual Studio sağlayan iki özellikle yararlı özellikleri şunlardır:

- *IntelliSense*. Visual Studio'ya yerleşik IntelliSense IntelliSense WebMatrix içinde daha kapsamlı özelliğidir.
- *Hata ayıklayıcı*. Hata ayıklayıcı çalıştıran, değişkenleri incelenerek ve satır kod atlama sırada bir program durdurarak kodunuzu gidermenize olanak sağlar.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>ASP.NET Web sayfaları farklı sürümleri ile Visual Studio kullanarak

Visual Studio 2012 ve Visual Studio 2013 için ASP.NET Web sayfaları desteğini içerir. (Visual Studio yüklediğinizde ASP.NET Web sayfalarını desteklemek için gerekli paketleri yüklenir.)

Visual Studio 2010 destek ASP.NET Web sayfaları için varsayılan olarak içermez. Visual Studio 2010 ile ASP.NET Web sayfaları kullanmak için ASP.NET MVC paketini yüklemeniz gerekir. ASP.NET Web Pages 2 almak için ASP.NET MVC 4 yükleyin.

Aşağıdaki tabloda farklı sürümlerinde Visual Studio, ASP.NET Web sayfaları için destek özetler.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | ASP.NET MVC 4 yükleyin | (Dahil) | (Dahil) |
| **ASP.NET Web Pages 3** |  | 3 ile NuGet güncelleştirme ASP.NET Web sayfaları | (Dahil) |

Visual Studio 2010 ile çalışmak için bkz: [yükleme desteği için ASP.NET Web sayfaları Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>WebMatrix Visual Studio'dan başlatma

Bir proje Webmatrix'te başlamış olması ve Visual Studio geçmek istiyorsanız, WebMatrix kolayca projeyi Visual Studio'da açmak için bir düğmeye sağlar. Bu düğme, bilgisayarınızda yüklü etkinleştirilmesi için Visual Studio'yu olması gerekir. Aşağıdaki resimde düğmesi Webmatrix'te gösterir.

![Visual Studio'yu başlatın](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Düğmeye tıkladığınızda, projeyi Visual Studio açılır. WebMatrix ve Visual Studio arasında sorunsuz ve geriye geçebilirsiniz. Tüm dosyalar diğer ortamında değiştirdiyseniz ve en son değişiklikleri almak için yeniden yüklenmesi gereken size bildirilecek.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Visual Studio'da ASP.NET Razor Site oluşturma

Visual Studio'da ASP.NET Razor Web sitesi oluşturmak için:

1. Visual Studio veya Visual Web Developer başlatın.
2. İçinde **dosya** menüsünde tıklatın **yeni Web sitesi**.

    ![Yeni web sitesi oluştur](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. İçinde **yeni Web sitesi** iletişim kutusunda, (Visual C# veya Visual Basic) kullanılacak dili seçin.
4. Seçin **ASP.NET Web sitesi (Razor)** şablonu.

    ![Razor site](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. **Tamam**'ı tıklatın.

Yeni projeniz varsa ve bazı varsayılan başlamanıza yardımcı olmak için web sayfaları ile doldurulur.

### <a name="using-intellisense"></a>IntelliSense Kullanma

Bir site oluşturduğunuza göre IntelliSense Visual Studio'da nasıl işlediğini görebilirsiniz.

1. Yeni oluşturduğunuz Web sitesi açın *Default.cshtml* sayfası.
2. Sonra `<h3>` sayfa etiketleri yazın `@ServerInfo.` (nokta dahil). Kullanılabilir yöntemleri için IntelliSense nasıl görüntülendiğine dikkat edin `ServerInfo` aşağı açılan listesinde Yardımcısı. 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Seçin `GetHtml` yönteminden listesi ve ENTER tuşuna basın. IntelliSense yönteminde otomatik olarak doldurur. (C# herhangi bir yöntem ile eklemelisiniz gibi `()` yöntemi sonra karakterleri.)  
   Tamamlanan kodu `GetHtml` yöntemi aşağıdaki gibi görünür:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Sayfayı çalıştırmak için CTRL + F5 tuşuna basın. Bu sayfa bir tarayıcıda görüntülenen zaman nasıl göründüğünü oluşur: 

    ![varsayılan sayfasını tarayıcıda](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Tarayıcıyı kapatın.

### <a name="using-the-debugger"></a>Hata ayıklayıcıyı kullanma

1. Üstündeki *Default.cshtml* ile başlayan satırı sonra sayfa `Page.Title`, aşağıdaki kod satırını ekleyin: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Kodun soluna Düzenleyicisi gri kenar boşluğunda bu yeni yanındaki eklemek için çizgi bir *kesme noktası*. Bir kesme noktası olanlar görebilmek için bu noktada programı durdurmak için hata ayıklayıcı söyleyen bir işaretçi var.

    ![Kesme noktası ayarlama](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Çağrı kaldırmak `ServerInfo.GetHtml` yöntemini ve bir çağrı ekleyin `@myTime` onun yerine değişken. Bu çağrı yeni kod satırı tarafından döndürülen geçerli zaman değerini görüntüler.
4. Hata ayıklayıcıda sayfayı çalıştırmak için F5 tuşuna basın. Sayfa ayarladığınız kesme noktası durdurur. Aşağıdaki resimde, sayfa nasıl Düzenleyicisi'nde kesme (sarı) ile göründüğünü gösterir. 

    ![hata ayıklama kesme](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Hata ayıklama araç çubuğunda tıklatın **Step Into** kodun sonraki satırında, çalıştırmak için düğmesi (veya F11 tuşuna basın). Bu düğmeye tıkladığınızda her zaman yürütme kodunun bir sonraki satıra ilerleyin.

    ![Düğmesinde adım](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Değerini inceleyin `myTime` üzerine fare işaretçisini tutarak ya da görüntülenen değerler inceleniyor değişken **Yereller** ve **çağrı yığını** windows. Visual Studio değişkenin değerini görüntüler.

    ![Göster zaman değeri](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Değişkeni incelenerek ve kod atlama bittiğinde, her satırın durdurmadan sayfa çalıştırmaya devam etmek için F5 tuşuna basın. Tüm kod atlama bitirdiğinizde, tarayıcı sayfasını görüntüler.

Hata ayıklayıcı ve Visual Studio kodda hata ayıklama hakkında daha fazla bilgi için bkz: [izlenecek yol: Web sayfalarında hata ayıklama Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Visual Studio ile ASP.NET MVC projelerinde Razor kullanma

Razor sözdizimi ASP.NET MVC projelerinde yaygın olarak kullanılır. MVC, dinamik Web siteleri oluşturmak için güçlü, desenleri dayalı bir yoludur. ASP.NET Web sayfaları sitenizi korumak zor olursa, bir ASP.NET MVC uygulamasına dönüştürmeyi göz önüne isteyebilirsiniz. Bir MVC uygulaması oluşturma örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Visual Studio 2010'da ASP.NET Web Pages desteğini yükleme

Bu bölümde Visual Web Developer Express 2010 ve ASP.NET Web sayfaları araçları Visual Studio için nasıl yükleneceğini gösterir.

1. Web Platformu yükleyicisi zaten yoksa, aşağıdaki URL'yi yükleyin:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Web Platformu Yükleyicisi'ni çalıştırın.
3. Tıklatın **ürünleri** sekmesi.

    ![Webpı ürünleri sekmesi](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Arama **ASP.NET MVC 4** (için ASP.NET Web Pages 2) ve ardından **Ekle**. Bu ürünler, ASP.NET Razor Web siteleri oluşturmak için Visual Studio araçları içerir.

    ![Webpı yükleme seçenekleri](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Tıklatın **yükleme** yüklemeyi tamamlayın.
