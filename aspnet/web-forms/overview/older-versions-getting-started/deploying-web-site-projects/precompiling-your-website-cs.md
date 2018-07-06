---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Web sitenizi (C#) önceden derleme | Microsoft Docs
author: rick-anderson
description: 'Visual Studio, ASP.NET geliştiricilerine iki proje türü sunar: Web Uygulama projeleri (WAPs) ve Web sitesi projeleri (WSPs). Temel farklılıklar betwe birini...'
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: 1227c28600444ed1f2710ba1b5e021aae59fe098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840306"
---
<a name="precompiling-your-website-c"></a>Web sitenizi (C#) önceden derleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio, ASP.NET geliştiricilerine iki proje türü sunar: Web Uygulama projeleri (WAPs) ve Web sitesi projeleri (WSPs). İki proje türü arasındaki önemli farklılıkları birini WAPs eklendiğinde bir WSP kodda web sunucusunda otomatik olarak derlenebilir ise dağıtımdan önce açıkça derlenmiş kod sahip olmasıdır. Ancak, dağıtım öncesinde bir WSP derleneceği mümkündür. Bu öğretici ön derleme avantajlarını anlatıyor ve Visual Studio içinden ve komut satırından bir Web sitesinden derleneceği gösterilmiştir.


## <a name="introduction"></a>Giriş

Visual Studio, ASP.NET geliştiricilerine iki farklı proje türleri sunar: Web uygulama Projeleri'nde (WAP) ve Web sitesi projeleri (WSP). Bu proje türü arasındaki temel farklardan biri olan WAPs gerektiğini *açık derlemesini* WSPs kullanmasa *otomatik derleme*, varsayılan olarak. WAPs ile Web sitesinin içinde oluşturulan tek bir derleme içine web uygulamasının kodunun derleme `Bin` klasör. Dağıtım kapsar biçimlendirme içeriği kopyalama ( `.aspx.ascx`, ve `.master` dosyaları) projesinde, derleme içinde birlikte `Bin` klasörü; arka plan kod sınıfı dosyaları kendilerinin dağıtılması gerekmez. Öte yandan, WSPs biçimlendirme sayfaları hem kendi karşılık gelen arka plan kod sınıfları, üretim ortamına kopyalayarak dağıtın. Arka plan kod sınıfları, isteğe bağlı olarak web sunucusu üzerinde derlenir.

> [!NOTE]
> "Açık derleme Versus otomatik derleme" bölümünde kiracıurl [ *belirleme dosyaları gerekenler dağıtılabilir için* öğretici](determining-what-files-need-to-be-deployed-cs.md) proje arasındaki farklar hakkında daha fazla arka plan için modelleri, açık ve otomatik derleme ve derleme model dağıtım nasıl etkiler.


Otomatik derleme seçeneğini kullanmak basit bir işlemdir. Bir açık derleme adımı vardır ve ihtiyacı olan dosyaları değiştirdi dağıtılması, değiştirilen biçimlendirme sayfaları ve tam olarak derlenmiş derlemenin dağıtımı açık derlemesini BIOS'ta ise. Ancak, otomatik dağıtım, iki olası engelleri vardır:

- Sayfa ilk ziyaret edildiğinde otomatik olarak derlenmelidir olduğundan, ASP.NET sayfası ilk kez dağıtıldıktan sonra istendiğinde belirgin, ancak kısa bir gecikme olabilir.
- Otomatik derleme, iki tanımlayıcı biçimlendirme ve kaynak kodu web sunucusunda mevcut olmasını gerektirir. Web uygulaması, web sunucularında yükleyeceği müşterilere satış üzerinde planlıyorsanız bu çekici olmayan bir seçenek olabilir.

Varsa ya da eksiklikleri Yukarıdaki iki anlaşma Kesiciler, WAP modeline her iki anahtarı olabilir veya *ön derleme* WSP dağıtımından önce. Bu öğretici, barındırılan bir Web sitesi için en uygun ön derleme seçenekleri inceler ve önceden derlenmiş bir Web sitesinin dağıtımı ve ön derleme işlemi size yol gösterir.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>ASP.NET kodu oluşturma ve derleme genel bakış

Biz sırasında ön derleme seçeneklerini göz atmadan önce ilk kod oluşturma ve oluşturduğunuz veya son güncelleme tarihi: ASP.NET sayfası ilk kez istendiğinde oluşan bir derleme hakkında konuşalım. Bildiğiniz gibi ASP.NET sayfaları iki bölümünü oluşur: bildirim temelli işaretlemede `.aspx` dosya; ve bir ayrı arka plan kod sınıfı dosyasında genellikle bir kaynak kod bölümü (`.aspx.cs`). Bir ASP.NET sayfasına istendiğinde çalışma zamanı tarafından gerçekleştirilen adımlar uygulamanın derleme modeline bağlıdır.

WAPs ile sayfalarının kaynak kodu açıkça dağıtılan önce tek bir bütünleştirilmiş kod içine derlenmiş olmalıdır. Dağıtım sırasında bu derleme ve çeşitli biçimlendirme sayfaları üretim ortamına kopyalanır. ASP.NET sayfası için web sunucusuna bir istek ulaştığında çalışma zamanı sayfa arka plan kod sınıfı örneği oluşturur ve başlatır, `ProcessRequest` sayfa yaşam döngüsü başlatır ve sonuç olarak, sayfanın içeriği olarak da döndürülür oluşturur, yöntemi İstek sahibi. Arka plan kod sınıfı dağıtımından önce bir derleme içinde derlenen olduğundan çalışma zamanı ASP.NET sayfa arka plan kod sınıfı ile çalışabilirsiniz.

WSPs ve otomatik derleme, dağıtım öncesinde hiçbir açık derleme adımı yoktur. Bunun yerine, dağıtım, bildirim temelli ve kaynak kod içeriği üretim ortamına kopyalanmasını içerir. İlk kez sayfası oluşturan veya son güncelleme tarihi: ASP.NET sayfası için web sunucusuna bir istek geldiğinde, çalışma zamanı arka plan kod sınıfı bir derlemeye ilk derlemelisiniz. Bu derlemede klasörüne kaydedilir `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, ancak bu klasörün konumu aracılığıyla özelleştirilebilir `<compilation tempDirectory="" />` öğesinin `<system.web>`, genellikle `Web.config`. Bir derleme olduğundan diske kaydedilirse, bu istekler için aynı sayfa üzerinde yeniden derlenmesi gerek yoktur.

> [!NOTE]
> Beklediğiniz gibi yapıldığında kısa bir gecikme bir sayfa sayfa Kodu derlemek ve sonuçta elde edilen bir derleme olarak kaydedebilirsiniz sunucuya onaylanırken yararlanırken, otomatik derleme kullanan bir sitede ilk kez (veya değiştirilmiş bu yana ilk kez) isteme disk.


Kısacası, açık derleme ile Web sitesinin dağıtımdan önce kaynak kodu derlemek için bu adımı gerçekleştirmek zorunda kalmaktan çalışma zamanını kaydetmek gerekir. Oluşturulmuş veya son güncelleme olduğundan otomatik derleme ile çalışma zamanı derleme sayfalarının kaynak kodu, ancak bir hafif başlatma maliyetiyle sayfa ilk ziyaret edildiğinde için işler.

Ancak ASP.NET sayfaları, bildirim temelli parçası hakkında ( `.aspx` dosya)? Arasında bir ilişki olduğunu açıktır `.aspx` dosyaları ve bildirim temelli biçimlendirme içinde tanımlanan Web denetimleri olarak kendi arka plan kod sınıfları kodda kodda erişilebilir. Ayrıca açıktır, içeriği `.aspx` dosyalar sayfası tarafından oluşturulan biçimlendirmenin önemli ölçüde etkiler. Çalışma zamanı metin, HTML, çalışır ve Web denetimi sözdizimi tanımlanan nasıl `.aspx` istenen sayfa oluşturmak için dosya içeriği işlenen?

Çok WSPs WAPs arasında farklılık, alt düzey uygulama ayrıntılarını sidetracked istemiyorsanız ancak buna koysalar çalışma zamanı çeşitli Web denetimleri korumalı üyeleri ve yöntemler içeren bir sınıf dosyası otomatik olarak oluşturur. Üretilen bu dosya olarak uygulanan bir *kısmi sınıf* karşılık gelen arka plan kod sınıfı için. ([Kısmi sınıflar](http://www.dotnet-guide.com/partialclasses.html) içeriğini tek bir sınıf için birden çok dosyaya yayılan izin verir.) Bu nedenle, arka plan kod sınıfı iki yerde tanımlanır: içinde `.aspx.cs` oluşturmak ve bu otomatik olarak oluşturulan sınıfta, çalışma zamanı tarafından oluşturulan dosya. Bu otomatik olarak oluşturulan sınıf içinde depolanan `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` klasör.

Önemli olan çalışma zamanı tarafından işlenecek bir ASP.NET sayfasının İşte çıkardığınız hem, bildirim temelli ve kaynak kod bölümlerini bütünleştirilmiş kod içine derlenmiş olmalıdır. WAPs, kaynak kodu açıkça dağıtımından önce bir bütünleştirilmiş kod derlenir, ancak bildirim temelli biçimlendirme gerekir hala koda dönüştürülür ve web sunucusu üzerinde çalışma zamanı tarafından derlenmiş. Otomatik derleme işlemi kullanılırken WSPs ile web sunucusu tarafından derlenecek hem kaynak kodu hem de bildirim temelli biçimlendirme gerekir.

Açık derlemesini WSP modeliyle kullanmak mümkündür. Açık kaynak kod bölümü gibi WAP modeliyle derleyebilirsiniz. Bunun da ötesinde, bildirim temelli biçimlendirme derleyebilirsiniz.

## <a name="precompilation-options"></a>Ön derleme seçenekleri

.NET Framework ile birlikte gelen bir [ASP.NET derleme aracını (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) WSP modeli kullanılarak oluşturulmuş bir ASP.NET uygulama kaynak kodu (ve hatta içeriği) derlemek etkinleştirir. Bu araç, .NET Framework sürüm 2.0 ile yayımlanan ve bulunan `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` klasörü; kullanılan komut satırından veya Visual Studio içinden gelen derleme menünün Web sitesi yayımlama seçeneği başlatılan.

Derleme aracı derlemesinin iki genel form sağlar: yerinde ön derleme ve dağıtım için ön derleme. Yerinde ön derleme ile çalıştırdığınız `aspnet_compiler.exe` aracını komut satırından ve bilgisayarınızda bulunan bir Web sitesinin fiziksel yolunu ve sanal dizin yolunu belirtin. Derleme araç ardından derlenmiş bir sürümünde depolama projedeki her ASP.NET sayfası derler `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` sayfaların her ilk kez bir tarayıcıdan ziyaret, olduğu gibi klasör. Bu adımı gerçekleştirmeden gelen çalışma zamanı azaltır çünkü yeni dağıtılan ASP.NET sayfaları, sitenizde yapılan ilk istek yerinde ön derleme hızlandırabilirsiniz. Ancak, web sunucusunun programlarından komut satırı olanağına gerektirdiğinden yerinde ön derleme barındırılan Web sitelerinin çoğu için kullanışlı değildir. Paylaşılan barındırma ortamlarında bu erişim düzeyi izin verilmez.

> [!NOTE]
> Yerinde ön derleme hakkında daha fazla bilgi için kullanıma [nasıl yapılır: ASP.NET Web sitesini önceden derleme](https://msdn.microsoft.com/library/ms227972.aspx) ve [ön derleme ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Web sayfaları derleme yerine `Temporary ASP.NET Files` klasör ön derleme dağıtımı için seçtiğiniz ve üretim ortamına dağıtılabilir bir biçimde bir dizine sayfaları derler.

Bu öğreticide inceleyeceğiz dağıtımı için ön derleme iki türü vardır: bir güncelleştirilebilir kullanıcı arabirimi ile ön derleme ve güncelleştirilebilir olmayan kullanıcı arabirimi ile ön derleme. Güncelleştirilebilir kullanıcı arabirimi ile ön derleme bırakır bildirim temelli biçimlendirmede `.aspx`, `.ascx`, ve `.master` dosyaları, böylece görüntülemek ve üretim sunucusu üzerinde bildirim temelli biçimlendirme isterseniz değiştirme bir geliştirici izin verme. Güncelleştirilebilir olmayan kullanıcı arabirimi ile ön derleme oluşturur `.aspx` kaldırır ve herhangi bir içerik ve void sayfaları `.ascx` ve `.master` böylece bildirim temelli biçimlendirme gizleme ve ondan değiştirmesini bir geliştirici yasaklanması dosyaları üretim ortamı.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Güncelleştirilebilir kullanıcı arabirimi ile dağıtmak için önceden derleme

Ön derleme dağıtımı için anlamak için en iyi örneği iş başında görmek için yoludur. Şimdi ön derleme Kitap incelemeleri WSP güncelleştirilebilir kullanıcı arabirimini kullanarak dağıtım için. ASP.NET derleme aracı, Visual Studio'nun derleme menüsünden veya komut satırından çağrılabilir. Bu bölümde, Visual Studio içinden aracından kullanarak inceler; "önceden derleme komut satırdan" bölümünde derleyicisi aracının komut satırından çalıştırma sırasında arar.

Kitap gözden geçirme WSP Visual Studio'da açın, derleme menüsüne gidin ve Web sitesi yayımlama menü seçeneğini belirleyin. Bu Web sitesi yayımlama iletişim kutusu başlatır (bkz **Şekil 1**), burada hedef konum, önceden derlenmiş sitenin kullanıcı arabirimi güncelleştirilebilir olup olmadığını ve diğer derleyici araç seçenekleri belirtebilirsiniz. Hedef konumu bir uzak web sunucusuna veya FTP sunucusu ancak şimdilik bilgisayarınızın sabit sürücüsündeki bir klasör seçin. Güncelleştirilebilir kullanıcı arabirimi ile siteyi önceden derlemeniz istediğimizden, "Bu önceden derlenmiş sitenin güncelleştirilebilir izin ver" onay kutusunu işaretli bırakın ve Tamam'ı tıklatın.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Şekil 1**: ASP.NET derleme aracı belirtilen hedef konuma sitenizi önceden derleme  
 ([Tam boyutlu görüntüyü görmek için tıklatın](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> Web sitesi yayımlama seçeneği da yapı menüsündeki Visual Web Developer'da kullanılabilir değil. Visual Web Developer kullanıyorsanız, komut satırı "ön derleme komut satırdan" bölümünde ele alınmıştır ASP.NET derleme aracını kullanmanız gerekir.


Web sitesini önceden derleme sonra Web sitesi yayımlama iletişim kutusuna girilen hedef konuma gidin. Sitenizin içeriğini bu klasörün içeriğini karşılaştırma için bir dakikanızı ayırın. **Şekil 2** Kitap incelemeleri Web sitesi klasörü gösterir. Her ikisini de içeren Not `.aspx` ve `.aspx.cs` dosyaları. Ayrıca, `Bin` dizin, yalnızca bir dosya içerir `Elmah.dll`, biz de eklenen [önceki öğretici](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Şekil 2**: Proje dizinini içeren `.aspx` ve `.aspx.cs` dosyaları; `Bin` klasörü yalnızca içerir `Elmah.dll`  
 ([Tam boyutlu görüntüyü görmek için tıklatın](precompiling-your-website-cs/_static/image6.png))

**Şekil 3** içerikleri, ASP.NET derleme aracı tarafından oluşturulmuş hedef konumu klasörünü gösterir. Bu klasör, herhangi bir arka plan kod dosyalarında içermiyor. Ayrıca, bu klasörün `Bin` dizin içeren çeşitli derlemeler ve iki `.compiled` yanı sıra dosyaları `Elmah.dll` derleme.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Şekil 3**: hedef konumu klasörünü dosyaları için dağıtım içerir.  
 ([Tam boyutlu görüntüyü görmek için tıklatın](precompiling-your-website-cs/_static/image9.png))

WAPs açık derlemede, bir derleme tüm site dağıtım işlemi için ön derleme oluşturmaz. Bunun yerine, bu birlikte birkaç sayfadaki her derlemeye toplu olarak işler. Ayrıca derler `Global.asax` (varsa) hiçbir sınıflarda yanı sıra, kendi derleme dosyasını `App_Code` klasör. Bildirim temelli biçimlendirme tutmak için ASP.NET dosyaları web sayfaları, kullanıcı denetimleri ve ana sayfalar (`.aspx`, `.ascx`, ve `.master` dosyaları, sırasıyla) olarak kopyalanır-için hedef konum dizindir. Benzer şekilde, `Web.config` dosya kopyalanır düz üzerinden birlikte görüntüler, CSS sınıfları ve PDF dosyaları gibi statik dosyalardır. Derleme aracı çeşitli nasıl işlediğini daha resmi bir açıklaması için dosya türleri, başvurmak [dosya işleme sırasında ASP.NET ön derleme](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Web sitesi yayımlama iletişim kutusunda "Adlandırma sabit kullanılır ve tek sayfa bütünleştirilmiş kodlarını" onay kutusunu işaretleyerek ASP.NET sayfası, kullanıcı denetimi veya ana sayfa başına bir derleme oluşturmak için derleme aracı bildirebilirsiniz. Kendi bütünleştirilmiş kod içine derlenmiş bir ASP.NET sayfasının her sahip dağıtım üzerinde daha ayrıntılı denetim sağlar. Tek bir ASP.NET web sayfası güncelleştirildi ve bu değişiklik dağıtmak için gerekli, örneğin, yalnızca o sayfanın dağıtmanız `.aspx` dosya ve üretim ortamına ilişkili derleme. Başvurun [nasıl yapılır: ASP.NET derleme aracı sabit adlarla oluşturmak](https://msdn.microsoft.com/library/ms228040.aspx) daha fazla bilgi için.


Hedef konum dizin de önceden derlenmiş web projesinin parçası yani değil bir dosyayı içeren `PrecompiledApp.config`. Bu dosya, uygulama derlendiğini ASP.NET çalışma zamanı ve güncelleştirilebilir veya öğleden itibaren güncelleştirilebilir bir kullanıcı Arabirimi ile mi derlendiğini bildirir.

Son olarak, birini açmak için bir dakikanızı ayırarak `.aspx` Visual Studio veya seçtiğiniz metin düzenleyiciyi kullanarak hedef konumunda dosyaları. Güncelleştirilebilir kullanıcı arabirimi ile dağıtmak için önceden derleme, ASP.NET sayfaları hedef konumu dizinde tam aynı biçimlendirme olarak Web sitesi buna karşılık gelen dosyaları içerir.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Güncelleştirilebilir olmayan kullanıcı arabirimi ile dağıtımı için önceden derleme

ASP.NET derleyici aracı, bir site için dağıtım güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile önceden derlemek için de kullanılabilir. Site güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile ön derleme ASP.NET sayfaları, kullanıcı denetimleri ve hedef dizinde ana sayfalar, biçimlendirme kaldırılır, olmasına en önemli fark bir güncelleştirilebilir UI önceden derleme gibi çalışır. Güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile dağıtım için bir Web sitesi önceden derlemek için Web sitesi yayımlama seçeneğini derle menüsünde, ancak "güncelleştirilebilir bu önceden derlenmiş sitenin izin ver" seçeneğinin işaretini kaldırın (bkz **Şekil 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Şekil 4**: "Bu önceden derlenmiş sitenin güncelleştirilebilir izin ver" seçeneği için ön derleme ile bir olmayan güncelleştirilebilir UI'seçeneğinin işaretini kaldırın  
 ([Tam boyutlu görüntüyü görmek için tıklatın](precompiling-your-website-cs/_static/image12.png))

**Şekil 5** güncelleştirilebilir olmayan kullanıcı arabirimi ile ön derleme sonra hedef konumu klasörünü gösterir.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Şekil 5**: güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile dağıtımı için hedef konum klasörü  
 ([Tam boyutlu görüntüyü görmek için tıklatın](precompiling-your-website-cs/_static/image15.png))

Karşılaştırma **Şekil 3** için **Şekil 5**. İki klasör aynı görünür, ancak güncelleştirilebilir olmayan UI klasör ana sayfa eksiktir `Site.master`. Ve while **Şekil 5** bunlar çıkartılır bildirim temelli, biçimlendirme ve yer tutucu metnini yerine olduğunu göreceksiniz bu dosyaların içeriğini görüntülerseniz, çeşitli ASP.NET sayfaları içerir: "Bu bir tarafından oluşturulan bir işaretçi dosyası Ön derleme aracı ve silinmemelidir! "

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Şekil 5**: bildirim temelli biçimlendirme ASP.NET sayfaları kaldırıldı

`Bin` Klasörlerinde **Şekil 3** ve **5** daha önemli ölçüde farklılık gösterir. Ek derlemeler olarak `Bin` klasöründe **Şekil 5** içeren bir `.compiled` her ASP.NET sayfası, kullanıcı denetimi ve ana sayfa dosyası.

Bir site güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile ön derleme burada kişinin veya şirketin yükler veya üretim ortamında bir Web sitesi yöneten değiştirilmesi için ASP.NET sayfaları içeriği istemediğiniz durumlarda yararlı olur. Kendi web sunucularına yükleyin müşterilere satış yapın bir ASP.NET web uygulaması derleme yaparsanız, bunlar görünümünü sitenizin doğrudan düzenleyerek değiştirmemeniz emin olmak isteyebilirsiniz `.aspx` sayfaları bunları gönderin. Yer tutucu sevk güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile bir Web sitenizi önceden derleme tarafından `.aspx` sayfaları böylece müşterileriniz inceleme veya değiştirme içeriklerini önleme yüklemesinin bir parçası olarak.

### <a name="precompiling-from-the-command-line"></a>Komut satırından önceden derleme

Arka planda, ASP.NET derleme aracı Visual Studio Web sitesi yayımlama iletişim kutusunu çağırır (`aspnet_compiler.exe`) Web sitesini önceden derleme için. Alternatif olarak, komut satırından bu aracı çağırabilir. Visual Web Developer kullanırsanız Visual Web Developer'ın yapı menüsünde Web Site yayımlama seçeneğini içermez gibi aslında, ardından derleyici aracı komut satırından çalıştırmak ihtiyacınız olacak.

Komut satırından derleme aracını kullanmak için komut satırına bırakarak ve framework dizinine gezinme Başlat `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Ardından, komut satırına aşağıdaki ifadeyi girin:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Yukarıdaki komutu ASP.NET derleyici aracı başlatır (`aspnet_compiler.exe`) ve aracılığıyla `-p` değiştirmek, köklü Web sitesi yeniden derlemesini bildirir *fiziksel\_yolu\_için\_uygulama*; Bu değer, aşağıdakine benzer olacaktır `C:\MySites\BookReviews`ve tırnak işareti ayrılmış olması.

`-v` Anahtar sitenin sanal dizini belirtir. Sitenizi IIS'teki varsayılan Web sitesi olarak kayıtlı sonra atlayabilirsiniz `-p` geçin ve yalnızca uygulamanın sanal dizini belirtin. Kullanırsanız `-p` geçiş, değer devam etmeden `-v` anahtar Web sitesinin kök gösterir ve uygulama kökü başvurularını çözümlemek için kullanılır. Örneğin, değerini belirtirseniz `-v /MySite` uygulamasındaki başvuran `~/path/file` olarak çözümlenir `~/MySite/path/file`. Kitap incelemeleri site barındırma şirketi web kök dizininde yer olduğu için anahtar kullandığımı `-v /`.

`-f` Anahtarı varsa, üzerine yazmak için derleme aracı bildirir *hedef\_konumu\_klasör* dizin zaten varsa. Atlarsanız `-f` anahtar ve hedef konum klasörü zaten var, derleme aracı hatasıyla çıkılacak: "ASPRUNTIME hata: hedef dizin boş değil. "Lütfen el ile silin veya farklı bir hedef seçin."

`-u` Anahtarı varsa, bir güncelleştirilebilir kullanıcı arabirimi oluşturmak için aracı bildirir. Bu anahtar güncelleştirilebilir olmayan kullanıcı arabirimi ile siteyi önceden derlemeniz yok sayın.

Son olarak, *hedef\_konumu\_klasör* hedef konum dizin; fiziksel yolu bu değer, aşağıdakine benzer olacaktır `C:\MySites\Output\BookReviews`ve tırnak işareti ayrılmış olması.

## <a name="deploying-the-precompiled-website"></a>Önceden derlenmiş Web sitesi dağıtma

Bu noktada her iki güncelleştirilebilir ve güncelleştirilebilir olmayan kullanıcı arabirimi seçenekleri kullanarak bir Web sitesini önceden derleme için ASP.NET derleme aracını kullanmayı gördük. Ancak, örneklerimizde şimdiye kadarki yerel bir klasöre ve üretim ortamı için Web sitesi önceden derlenmiş. Güzel bir haberimiz var önceden derlenmiş Web sitesi dağıtımı, bir kolaylaştırır ve bazı diğer dosya kopyalama mekanizmalarının, veya Visual Studio aracılığıyla gibi tek başına bir FTP istemcisinden yapılabilir olmasıdır.

Web sitesi yayımlama iletişim kutusu (ilk gösterilen **Şekil 1**) önceden derlenmiş Web sitesi dosyalarını nerede kopyalanır belirtir bir hedef konum seçeneği vardır. Bu konum, bir uzak web sunucusuna veya FTP sunucusu olabilir. Uzak bir sunucuya bu metin kutusuna bir metin girme işlemini gerçekleştirir ve Web sitesini tek bir adımda belirtilen sunucuya dağıtır. Alternatif olarak, yerel bir klasöre Web sitesini önceden derleme ve daha sonra el ile bu klasörün içeriğini FTP veya diğer bir yaklaşım aracılığıyla üretim ortamına kopyalayın.

Önceden derlenmiş Web sitesi otomatik olarak Visual Studio'nun Web sitesi yayımlama iletişim kutusu dağıtılan sahip basit siteleri için yararlıdır geliştirme ve üretim ortamları arasında hiçbir yapılandırma farklılıkları olduğu. Bununla birlikte, içinde belirtilenlerle [ *yaygın yapılandırma farklılıkları arasında geliştirme ve üretim* öğretici](common-configuration-differences-between-development-and-production-cs.md) gibi farklılıklar bulunmasını durumdur. Örneğin, Kitap incelemeleri web uygulaması geliştirme ortamında üretim ortamında farklı bir veritabanı kullanır. Visual Studio uzak bir sunucuya Web sitesi yayımlarken körüne yapılandırma dosyası bilgileri geliştirme ortamında kopyalar.

Yapılandırma farkları geliştirme ve üretim ortamları arasında siteler için en iyi bir yerel dizine siteyi önceden derlemeniz, üretim özgü yapılandırma dosyalarını kopyalayın ve ardından önceden derlenmiş çıkışı içeriğini kopyalayın olabilir Üretim.

Dosyalarını geliştirme ortamından üretim ortamına kopyalama Yenileyici başvurmak [ *sitenizi FTP istemcisi kullanarak dağıtma* ](deploying-your-site-using-an-ftp-client-cs.md) ve [  *Uygulamanızın Web sitesi Visual Studio kullanarak dağıtma* ](determining-what-files-need-to-be-deployed-cs.md) öğreticiler.

## <a name="summary"></a>Özet

ASP.NET derlemesinin iki modu destekler: otomatik ve açık. Web sitesi projeleri (WSPs) varsayılan olarak otomatik derleme kullanmasa önceki öğreticilerdeki açıklandığı gibi Web Uygulama projeleri (WAPs) açık derlemesini kullanın. Ancak, açıkça bir WSP dağıtımdan ASP.NET derleme aracını kullanarak derlemek mümkündür.

Bu öğreticide, dağıtım desteği için derleme Aracı'nın ön derleme odaklanan. Dağıtım için önceden derleme, derleme aracı hedef konumu klasörünü oluşturur, belirtilen web uygulamasının kaynak kodu derler ve bu kopyalar. hedef konum klasöre derlemeleri ve içerik dosyaları derlenmiş. Derleme aracı, güncelleştirilebilir veya güncelleştirilebilir olmayan kullanıcı arabirimi oluşturmak için yapılandırılabilir. Güncelleştirilebilir olmayan kullanıcı arabirimi seçeneği ile önceden derleme, içerik dosyalarını bildirim temelli biçimlendirmede kaldırılır. Buna koysalar ön derleme isterseniz Web sitesi projesi tabanlı uygulamanızda hiçbir kaynak kodu dosyaları dahil olmak üzere olmadan ve kaldırılmış, bildirim temelli biçimlendirme dağıtmanıza olanak verir.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET Web sitesi ön derleme](https://msdn.microsoft.com/library/ms228015.aspx)
- [Codebehind ve ASP.NET 2.0 derleme](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [ASP.NET'te ön derleme](http://www.odetocode.com/Articles/417.aspx)
- [ASP.NET'te önceden derlenmiş sitenin seçenekleri](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Önceki](logging-error-details-with-elmah-cs.md)
> [İleri](users-and-roles-on-the-production-website-cs.md)
