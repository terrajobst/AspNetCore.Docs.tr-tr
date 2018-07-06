---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: ASP.NET AJAX yerelleştirmesini anlama | Microsoft Docs
author: scottcate
description: Yerelleştirme tasarlama ve Destek belirli bir dil ve kültür için bir uygulama ya da bir uygulama bileşeni tümleştirme işlemidir. MIC...
ms.author: aspnetcontent
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: ce6404ce4faa1018a4f8118f6167a4f93956abd3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815018"
---
<a name="understanding-aspnet-ajax-localization"></a>ASP.NET AJAX yerelleştirmesini anlama
====================
tarafından [Scott Cate](https://github.com/scottcate)

[PDF'yi indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Yerelleştirme tasarlama ve Destek belirli bir dil ve kültür için bir uygulama ya da bir uygulama bileşeni tümleştirme işlemidir. Microsoft ASP.NET platformunu standart .NET yerelleştirme modeli tümleştirerek standart ASP.NET uygulamaları için yerelleştirme için kapsamlı destek sağlar; Microsoft AJAX Framework yerelleştirme gerçekleştirilebilir çeşitli senaryoları desteklemek için tümleşik bir model kullanır.


## <a name="introduction"></a>Giriş

Microsoft ASP.NET teknoloji, nesne yönelimli ve olay odaklı bir programlama modeli sunar ve derlenmiş kod avantajları sahip. Ancak, bazı kötü yanları çoğu, .NET Framework'teki Microsoft AJAX Hizmetleri Kapsüller System.Web.Extensions ad alanında bulunan yeni özellikler çözülebilir teknolojisindeki devralınan, sunucu tarafı işleme modeli vardır 3.5. Bu uzantılar, birçok zengin istemci özellikleri, ASP.NET 2.0 AJAX uzantıları parçası, ancak artık Framework temel sınıf kitaplığının bir parçası önceden kullanılabilen olanak sağlar. Tam sayfa yenileme, Web Hizmetleri aracılığıyla istemci komut dosyası (profil oluşturma API'si ASP.NET dahil) erişim olanağı gerek kalmadan sayfaların kısmi işleme denetimleri ve bu ad alanındaki özellikleri içerir ve kapsamlı bir istemci tarafı API birçok yansıtmak üzere tasarlanmış ASP.NET sunucu tarafı denetim kümesinde görüldüğü denetim düzenleri.

Bu teknik incelemede Microsoft AJAX Framework ve Microsoft AJAX komut dosyası kitaplığı, mevcut işletme ihtiyaçlarına yerelleştirme desteğini ve zaten tümleşik destek Web yerelleştirme gözden geçirme için bağlamında yerelleştirme özellikleri inceler. .NET Framework tarafından sağlanan uygulamaları. Microsoft AJAX komut dosyası kitaplığı tümleşik IDE desteği ve paylaşılabilir kaynak türü sağlayan .NET uygulamaları tarafından zaten kullanılan .resx dosya biçimi kullanır.

Bu teknik incelemede tabanlı Beta 2 sürümünü Microsoft Visual Studio 2008'in üzerinde. Bu Teknik İnceleme de Visual Studio 2008 ile değil Visual Web Developer Express, çalışma ve izlenecek yollar Visual Studio'nun kullanıcı arabirimi göre sağlayacak varsayar. Kod örnekleri, Visual Web Developer Express kullanılamayabilir proje şablonları yararlanacaktır.

## <a name="the-need-for-localization"></a>*Yerelleştirme gerekli*

Kurumsal uygulama geliştiriciler ve Bileşen geliştiriciler özellikle için kültürler ve diller arasındaki farkların farkında Araçlar oluşturma olanağı giderek çıkmıştır. İstemcinin yerel uyarlama olanağı ile bileşenleri tasarlama Geliştirici verimliliğini artırır ve genel olarak çalışması için bir bileşenin uyarlaması için gerekli çalışma miktarını azaltır.

Yerelleştirme tasarlama ve Destek belirli bir dil ve kültür için bir uygulama ya da bir uygulama bileşeni tümleştirme işlemidir. Microsoft ASP.NET platformunu standart .NET yerelleştirme modeli tümleştirerek standart ASP.NET uygulamaları için yerelleştirme için kapsamlı destek sağlar; Microsoft AJAX Framework yerelleştirme gerçekleştirilebilir çeşitli senaryoları desteklemek için tümleşik bir model kullanır. Microsoft AJAX çerçevesiyle betikleri ya da uygu derlemeleri halinde dağıtılan ya da bir statik dosya sistemi yapısı yararlanarak yerelleştirilebilen.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Uydu derlemeleri Betiklerle ekleme*

Standart .NET Framework yerelleştirme stratejisi ile tutarlı olmasını sağlamak, kaynakları uydu derlemelerinde dahil edilebilir. Uydu derlemeleri çeşitli avantajlar sağlayan ikili - geleneksel kaynak edilme üzerinden verilen tüm yerelleştirme daha büyük resmi güncelleştirmeden güncelleştirilebilir, uydu derlemelerine yükleyerek ek yerelleştirmeler dağıtılabilir Proje klasörü ve uydu derlemeleri ana proje derlemesi yeniden yüklenmesi neden olmadan dağıtılabilir. ASP.NET projeleri, özellikle de bu artımlı güncelleştirmeler tarafından kullanılan sistem kaynakları miktarını önemli ölçüde azaltabilir çünkü yararlıdır ve üretim Web sitenizin kullanımı en düşük düzeyde kesintiye uğratır.

Betikler, derlemeye dahil ederek katıştırıldığı bütünleştirilmiş derleme zamanında dahil dosyalarını .resx yönetilen (veya .resources derlenmiş). Kaynakları daha sonra betik uygulama AJAX çalışma zamanı tarafından oluşturulan kod, derleme düzeyinde öznitelikler aracılığıyla kullanıma sunulur

*Ekli komut dosyaları için adlandırma kuralları*

Microsoft AJAX Framework komut dosyası yönetimi, dağıtım ve test betikleri kullanım için çeşitli seçenekler destekler ve yönergeleri Bu seçenekler kolaylaştırmak için sağlanır.

*Hata ayıklamayı kolaylaştırmak için:*

Yayın (üretim) betikleri değil içermelidir `.debug` dosya adında niteleyicisi. Hata ayıklama içermelidir için tasarlanmış komut `.debug` dosya adında.

*Yerelleştirme kolaylaştırmak için:*

Nötr kültüre betikleri herhangi bir kültür tanımlayıcı dosyasının adını içermemelidir. Yerelleştirilmiş kaynakları içeren komut dosyaları için ISO dil kodu dosya adı belirtilmelidir. Örneğin, `es-CO` İspanyolca, Columbia anlamına gelir.

Aşağıdaki tabloda dosya adlandırma kuralları ile örnekler özetlenmektedir:

| Dosya adı | Açıklama |
| --- | --- |
| Script.js | Bir yayın sürümünü nötr kültüre betiği. |
| Script.debug.js | Hata ayıklama sürümü nötr kültüre betiği. |
| Script.en-US.js | Bir yayın sürümü İngilizce, Amerika Birleşik Devletleri betiğine girildi. |
| Script.debug.es-CO.js | Bir hata ayıklama sürümü İspanyolca, Columbia betiğine girildi. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>İzlenecek yol: bir yerelleştirilmiş, katıştırılmış betik oluşturma

*Lütfen unutmayın: sınıf kitaplığı projeleri için proje şablonu Visual Web Developer Express içermez gibi bu kılavuzda Visual Studio 2008 kullanılmasını gerektirir.*

1. ASP.NET AJAX uzantılarını tümleşik yeni bir Web sitesi projesi oluşturun. Başka bir proje, LocalizingResources adlı bir çözüm içinde bir sınıf kitaplığı projesi oluşturun.
2. .Resx kaynak dosyaları DeletionResources.resx ve DeletionResources.es.resx adlı yanı sıra VerifyDeletion.js LocalizingResources projeye adlı bir Jscript dosyası ekleyin. Eski nötr kültürün kaynakları içerir; İkinci İspanyolca dil kaynakları içerir.
3. VerifyDeletion.js için aşağıdaki kodu ekleyin:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

JavaScript normal ifade söz dizimi, tek eğik metinde alışık olanlar (önceki örnekte /FILENAME/ örneğidir) RegExp nesnesi gösterir. MSDN Kitaplığı kapsamlı bir JavaScript başvurusu içerir ve kaynakları yerel JavaScript nesneleri çevrimiçi olarak bulunabilir.

1. Aşağıdaki kaynak dizeleri için DeletionResources.resx ekleyin: 

    **VerifyDelete**: FILENAME silmek istediğinizden emin misiniz?

    **Silinen**: dosya silindi.

1. Aşağıdaki kaynak dizeleri için DeletionResources.es.resx ekleyin: 

    **VerifyDelete**: Est seguro onuk desee quitar FILENAME?

    **Silinen**: FILENAME se ha quitado.
2. Aşağıdaki kod satırlarını AssemblyInfo dosyaya ekleyin:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. System.Web ve System.Web.Extensions başvurular LocalizingResources projeye ekleyin.
2. Web sitesi projesinden LocalizingResources projeye bir başvuru ekleyin.
3. Web sitesi projesi altındaki default.aspx ScriptManager denetimi aşağıdaki ek biçimlendirme güncelleştirin:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Bu işaretleme default.aspx sayfasında, üzerinde herhangi bir yere ekleyin:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. F5 tuşuna basın. İstenirse, hata ayıklamayı etkinleştir. Sayfa yüklendiğinde Sil düğmesine basın. Unutmayın (Bilgisayarınızı tercih İspanyolca dil kaynakları için varsayılan olarak ayarlanmadığı sürece), İngilizce dilinde onaylamanız istenir.
2. Tarayıcı penceresini kapatın ve default.aspx için döndürür. İçinde @Page üstbilgi yönergesi, es-ES Culture ve UICulture otomatik değiştirin. Tarayıcıda web uygulamasını yeniden yeniden başlatmak için F5 tuşuna basın. Bu kez, İspanyolca dosyasında silmeniz istenir dikkat edin:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Tam boyutlu görüntüyü görmek için tıklatın](understanding-asp-net-ajax-localization/_static/image6.png))


Bu izlenecek yol çeşitli kullanımları tercih edilebilir olduğuna dikkat edin. Örneğin, betikler ScriptManager denetimi ile programlı olarak sayfa yükleme sırasında kaydedilebilir.

## <a name="including-a-static-script-file-structure"></a>*Bir statik betik dosya yapısı dahil olmak üzere*

Dağıtım için statik komut dosyalarını kullanarak, devralınan .NET yerelleştirme düzeni kullanmanın avantajlarından bazıları kaybedersiniz. Öncelikli olarak görünür, komut dosyası kaynak dosyaları dahil olmak üzere oluşturulan otomatik türü kaybetmek olur; Yukarıdaki izlenecek yolda, örneğin, ileti ScriptManager denetimi olarak adlandırılan bir otomatik olarak üretilen türe göre kaynakları ifşa edilmedi.

Ancak, bir statik betik dosya yapısı kullanmanın bazı avantajları vardır. Güncelleştirmeleri yeniden derlemeden ve uydu derlemeleri yeniden dağıtmaya gerek kalmadan gerçekleştirilebilir ve bir statik dosya yapısı kullanımını alınmamış işlevselliği küçük bir parçasını bileşeni ile tümleştirmek için ekli komut dosyası geçersiz kılmak için de yapılabilir.

Microsoft Proje derlenirken betik kaynaklarınızı otomatik olarak oluşturarak bir sürüm denetim sorunu önlemenin önerir. Kapsamlı bir kod temel muhafaza ederken, kod değişikliklerini her yerelleştirilmiş betikte yansıtıldığından emin olmak giderek daha zor hale gelebilir. Alternatif olarak, yalnızca tek bir mantıksal betik ve birden fazla yerelleştirme komut dosyası, proje derlenirken dosyaları birleştirme koruyabilirsiniz.

Bildirimli olarak eklenecek kaynakları olmadığından, statik betik dosyaları ekleyerek ya da başvurulan `<asp:ScriptElement>` öğeler bir alt öğesi olarak `<Scripts>` etiketi ScriptManager denetimi veya programlama yoluyla ekleyerek `ScriptReference` nesneleri için `Scripts` özelliği `ScriptManager` sayfasında çalışma zamanında denetim.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager ve kendi rolünde yerelleştirme*

ScriptManager yerelleştirilmiş uygulamalar için birden fazla otomatik davranışlar sağlar:

- Otomatik olarak, ayarları ve adlandırma kurallarına göre komut dosyaları bulur; Örneğin, hata ayıklama etkin betik hata ayıklama modunda yükler ve tarayıcının kullanıcı arabirimi seçimine göre betikleri yükleri yerelleştirilmiş.
- Bu özel kültürler dahil olmak üzere, kültür tanımını sağlar.
- Ancak, HTTP üzerinden betik dosyaların sıkıştırmasını etkinleştirir.
- Birçok istekleri verimli bir şekilde yönetmek için komut dosyaları önbelleğe alır.
- Betikleri aracılığıyla şifrelenmiş bir URL'ye yönelterek yöneltme katmanı ekler.

Komut dosyası başvuruları, program aracılığıyla veya bildirim temelli biçimlendirme ScriptManager denetimi eklenebilir. Düzeltmeler gönderdi gibi betiğin adı büyük olasılıkla değişmez gibi bildirim temelli biçimlendirme betiklerle katıştırılmış derlemeleri web sitesi projesi kendisi dışında çalışırken yararlıdır.

## <a name="summary"></a>Özet

Web uygulamaları, daha büyük bir kitleye ulaşmaya büyüdükçe, daha geniş kültürler ve topluluklarına ulaşmak için gereken iş modeli çekirdek olur; e-ticaret web uygulamaları ile yabancı para başa çıkabilmesi gerekir, içerik yönetim sistemlerini içeriklerini ancak aynı zamanda kendi gezinti ipuçları ve form alanlarını diğer diller ve şirketler bu ihtiyacı olduğunu bilmeniz gereken mümkün değil yalnızca mevcut olması gerekir erişilebilir.

.NET Framework doğası gereği bir Tekdüzen Kaynak dizeleri ve görüntüleri Ara şekilde sunmak için uydu derlemeleri ve XML kaynak (.resx) dosyaları kullanan bir zengin yerelleştirme framework destekler. Microsoft AJAX Framework ve Microsoft AJAX komut dosyası kitaplığı dahil olmak üzere ASP.NET AJAX uzantılarını bu programlama modeli için istemci tarafı koduna kolay kaynak dize aramaları etkinleştirme desteği. Dosya adları belirli bir adlandırma şeması uyduğu sürece otomatik eklenmesi ScriptResource.axd betik kaynaklarında (gerçek .js dosyaları), uydu derlemelerini destekler. Bu destek sayesinde, ASP.NET AJAX uzantılarını betiklerinin yerelleştirme ve Genelleştirme uygulamaların basitleştirin.

## <a name="bio"></a>*Bio*

Scott Cate 1997'den beri Microsoft Web teknolojileri ile çalışmakta olduğu ve myKB.com Yardımcısı ([www.myKB.com](http://www.myKB.com)) tabanlı Bilgi Bankası yazılım çözümlerinizi odaklı uygulamaları burada kendisinin ASP.NET yazma konusunda uzmanlaşmış. Scott temas kurulabileceğini e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog'da [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [İleri](understanding-asp-net-ajax-web-services.md)
