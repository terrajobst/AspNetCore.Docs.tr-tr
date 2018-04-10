---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: ASP.NET AJAX yerelleştirme anlama | Microsoft Docs
author: scottcate
description: Yerelleştirme tasarlama ve uygulama ya da bir uygulama bileşeni belirli bir dil ve kültür için destek tümleştirme işlemidir. MIC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="understanding-aspnet-ajax-localization"></a>ASP.NET AJAX yerelleştirme anlama
====================
tarafından [Scott göstermek](https://github.com/scottcate)

[PDF indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Yerelleştirme tasarlama ve uygulama ya da bir uygulama bileşeni belirli bir dil ve kültür için destek tümleştirme işlemidir. Microsoft ASP.NET platformunu standart .NET yerelleştirme modeli tümleştirerek standart ASP.NET uygulamaları için yerelleştirme için kapsamlı destek sağlar; Microsoft AJAX Framework yerelleştirme gerçekleştirilebilir çeşitli senaryoları desteklemek için tümleşik modelini kullanır.


## <a name="introduction"></a>Giriş

Microsoft ASP.NET teknolojisi bir nesne yönelimli ve olay kaynaklı programlama modeli getirir ve derlenmiş kod yararları birleştirmiştir. Bununla birlikte, kendi sunucu tarafı işleme modelini birkaç dezavantajları birçok Microsoft AJAX Hizmetleri .NET Framework yalıtır System.Web.Extensions ad alanı içinde sunulan yeni özelliklerle çözülebilir teknolojisinde devralınmış vardır 3.5. Bu uzantılar birçok zengin istemci özellikleri, ASP.NET 2.0 AJAX uzantıları parçası, ancak Framework temel sınıf kitaplığı şimdi bir parçası olarak önceden kullanılabilir etkinleştirin. Web Hizmetleri aracılığıyla istemci komut dosyası (profil oluşturma API'si ASP.NET dahil) erişim olanağı bir tam sayfa yenileme gerek kalmadan sayfaların kısmi işleme denetimleri ve bu ad alanındaki özellikleri ekleyin ve kapsamlı bir istemci-tarafı API birçok yansıtmak üzere tasarlanmış ASP.NET sunucu tarafı denetimi kümesinde görüldüğü denetim düzenleri.

Bu teknik Microsoft AJAX Framework ve Microsoft AJAX komut dosyası kitaplığı, mevcut iş gereksinimini yerelleştirme desteği ve Web yerelleştirme için zaten tümleşik destek gözden geçirme bağlamında yerelleştirme özelliklerini inceler .NET Framework tarafından sağlanan uygulamaları. Microsoft AJAX komut dosyası kitaplığı tümleşik IDE desteği ve paylaşılabilir kaynak türü sağlayan önceden .NET uygulamaları tarafından kullanılan .resx dosya biçimini kullanır.

Bu teknik temel Microsoft Visual Studio 2008 Beta 2 sürümü üzerinde. Bu Teknik İnceleme de Visual Studio 2008 ile değil Visual Web Developer Express, çalışma ve Visual Studio kullanıcı arabirimi göre izlenecek yollar sağlar varsayar. Bazı kod örnekleri Visual Web Developer Express kullanılamayabilir proje şablonları yararlanacak olan.

## <a name="the-need-for-localization"></a>*Yerelleştirme gereksinimini*

Özellikle kuruluş uygulama geliştiricileri ve Bileşen geliştiriciler için kültürlere ve dilleri arasındaki farkların farkında Araçlar oluşturma yeteneği giderek çıkmıştır. İstemcinin yerel uyarlama olanağı ile bileşenlerini tasarlamayı Geliştirici verimliliğini artırır ve genel olarak çalışması için bir bileşen uyarlama için gereken iş miktarını azaltır.

Yerelleştirme tasarlama ve uygulama ya da bir uygulama bileşeni belirli bir dil ve kültür için destek tümleştirme işlemidir. Microsoft ASP.NET platformunu standart .NET yerelleştirme modeli tümleştirerek standart ASP.NET uygulamaları için yerelleştirme için kapsamlı destek sağlar; Microsoft AJAX Framework yerelleştirme gerçekleştirilebilir çeşitli senaryoları desteklemek için tümleşik modelini kullanır. Microsoft AJAX çerçevesiyle komut dosyaları ya da uydu derlemelerini dağıtılan ya da bir statik dosya sistemi yapısı kullanılarak yerelleştirilebilir.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Uydu derlemeleri kodlarla katıştırma*

Standart .NET Framework yerelleştirme stratejisi ile tutarlı, kaynakları uydu derlemeleri dahil edilebilir. Uydu derlemeleri sağlayan çeşitli avantajları ikili - geleneksel kaynak eklenmesi üzerinden verilen tüm yerelleştirme büyük görüntü güncelleştirmeden güncelleştirilebilir, uydu derlemelerini içine yükleyerek ek yerelleştirmeler dağıtılabilir Proje klasörünü ve uydu derlemelerini yeniden ana proje derlemesinin neden olmadan dağıtılabilir. ASP.NET projeleri, özellikle de bu artımlı güncelleştirmeler tarafından kullanılan sistem kaynaklarının miktarını önemli ölçüde azaltabilir çünkü yararlıdır ve en düşük düzeyde üretim Web sitesi kullanım kesintiye uğratır.

Komut dosyaları katıştırılmış derlemelerine dahil ederek derlemesini derleme zamanında dahil dosyaları .resx yönetilen (veya .resources derlenmiş). Kaynaklarını ardından AJAX çalışma oluşturulan kod, derleme düzeyi öznitelikleri aracılığıyla üzerinden betik uygulama için kullanılabilir hale getirilir

*Katıştırılmış komut dosyaları için adlandırma kuralları*

Microsoft AJAX Framework komut dosyası Yönetimi dağıtım ve komut dosyaları sınama kullanılmak üzere çeşitli seçenekleri destekler ve bu seçenekler kolaylaştırmak için yönergeler sağlanır.

*Hata ayıklama kolaylaştırmak için:*

Yayın (üretim) komut dosyaları dahil `.debug` filename niteleyicisinde. Hata ayıklama içermesi için tasarlanmış betikleri `.debug` dosya.

*Yerelleştirme kolaylaştırmak için:*

Nötr kültür komut dosyaları, dosya adına herhangi bir kültür tanımlayıcısı içermemelidir. Yerelleştirilmiş kaynakları içeren komut dosyaları için ISO dil kodu dosya adı belirtilmelidir. Örneğin, `es-CO` İspanyolca, Columbia anlamına gelir.

Aşağıdaki tabloda adlandırma kuralları örnekleri içeren dosya özetlenmektedir:

| Dosya adı | Açıklama |
| --- | --- |
| Script.js | Bir yayın sürümü kültür Tarafsız komut dosyası. |
| Script.debug.js | Hata ayıklama sürümü kültür Tarafsız komut dosyası. |
| Script.en-US.js | Bir yayın sürümü İngilizce, Amerika Birleşik Devletleri komut dosyası. |
| Script.debug.es-CO.js | Bir hata ayıklama sürümü İspanyolca, Columbia komut dosyası. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>İzlenecek yol: bir yerelleştirilmiş, katıştırılmış komut dosyası oluşturma

*Lütfen unutmayın: Visual Web Developer Express Sınıf Kitaplığı projelerinde için bir proje şablonu içermez gibi bu kılavuzda Visual Studio 2008 kullanılmasını gerektirir.*

1. Yeni bir Web sitesi projesi, ASP.NET AJAX tümleşik uzantılarıyla oluşturun. Başka bir projeye, LocalizingResources adlı çözüm içinde bir sınıf kitaplığı proje oluşturun.
2. .Resx kaynakları dosyaları DeletionResources.resx ve DeletionResources.es.resx adlı yanı sıra VerifyDeletion.js LocalizingResources projeye adlı Jscript dosyası ekleyin. Eski kültür Tarafsız kaynakları içerecek; İkinci İspanyolca dil kaynakları içerir.
3. VerifyDeletion.js için aşağıdaki kodu ekleyin:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

JavaScript Regex sözdizimi, tek eğik metinde alışık olanlar için (önceki örnekte /FILENAME/ örneğidir) RegExp nesnesi gösterir. MSDN Kitaplığı kapsamlı bir JavaScript başvurusu içeriyor ve JavaScript yerel nesneleri kaynakları çevrimiçi olarak bulunabilir.

1. Aşağıdaki kaynak dizeleri için DeletionResources.resx ekleyin: 

    **VerifyDelete**: FILENAME silmek istediğinizden emin misiniz?

    **Silinen**: FILENAME silinmiş.

1. Aşağıdaki kaynak dizeleri için DeletionResources.es.resx ekleyin: 

    **VerifyDelete**: Est seguro que desee quitar FILENAME?

    **Silinen**: FILENAME se ha quitado.
2. Aşağıdaki kod satırlarını AssemblyInfo dosyasına ekleyin:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. System.Web ve System.Web.Extensions başvuruları LocalizingResources projeye ekleyin.
2. Web sitesi projeden LocalizingResources projesine bir başvuru ekleyin.
3. Web sitesi projesini altında default.aspx ScriptManager denetimi ile aşağıdaki ek biçimlendirme güncelleştirmek:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Herhangi bir yere sayfasında default.aspx bu biçimlendirme ekleyin:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. F5 tuşuna basın. İstenirse, hata ayıklamayı etkinleştir. Sayfa yüklendiğinde Sil düğmesine basın. (Bilgisayarınızı İspanyolca dil kaynakları tercih için varsayılan olarak ayarlanmadığı sürece), İngilizce onaylamanız istenir unutmayın.
2. Tarayıcı penceresini kapatın ve için default.aspx dönün. İçinde @Page üstbilgi yönergesi, es-ES kültür ve UICulture Değiştir otomatik. Tarayıcıda web uygulamasını yeniden yeniden başlatmak için F5 tuşuna basın. Bu süre, İspanyolca dosyasında silmeniz istenecektir dikkat edin:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](understanding-asp-net-ajax-localization/_static/image6.png))


Bu kılavuz çeşitli Çeşitlemeler olduğuna dikkat edin. Örneğin, komut dosyaları ScriptManager denetimi ile programlı olarak sayfa yükleme sırasında kaydı yapılamadı.

## <a name="including-a-static-script-file-structure"></a>*Statik betik dosya yapısı dahil*

Statik komut dosyaları için dağıtım kullanırken, devralınan .NET yerelleştirme şeması kullanarak avantajlarından bazıları kaybedersiniz. Öncelikle, komut dosyası kaynak dosyaları dahil olmak üzere oluşturulan otomatik türü kaybetmeniz görülebilir; Yukarıdaki kılavuzda, örneğin, kaynakları ileti ScriptManager denetiminden adlı bir otomatik olarak oluşturulan türü tarafından ortaya.

Ancak, bir statik betik dosya yapısı kullanmanın bazı avantajları vardır. Güncelleştirmeleri yeniden derlenmesi ve uydu derlemelerini gerek olmadan gerçekleştirilebilir ve bir statik dosya yapısı kullanımını da alınmamış işlevselliği küçük bir parçasını bileşeni ile tümleştirmek için katıştırılmış betik geçersiz kılmak için yapılabilir.

Microsoft betik kaynaklarınızı otomatik olarak proje derleme sırasında oluşturarak sürüm denetimi sorunu önlemenin önerir. Kapsamlı bir kod temel korurken kod değişiklikleri yerelleştirilmiş her komut dosyasında yansıtılır sağlamak giderek daha çok zor olabilir. Alternatif olarak, yalnızca tek bir mantık betik ve birden fazla yerelleştirme komut dosyası, proje oluşturulurken dosyaları birleştirme bulundurabilirsiniz.

Bildirimli olarak eklenecek kaynakları olmadığından statik komut dosyaları olmalıdır ekleyerek ya da başvurulan `<asp:ScriptElement>` öğeler bir alt öğesi olarak `<Scripts>` etiketi ScriptManager denetimi veya program aracılığıyla ekleyerek `ScriptReference` nesneleri için `Scripts` özelliği `ScriptManager` çalışma zamanında sayfasında denetimi.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager ve onun rolünde yerelleştirme*

ScriptManager yerelleştirilmiş uygulamalar için birkaç otomatik davranışları sağlar:

- Otomatik olarak ayarlar ve adlandırma kurallarına göre komut dosyaları da bulur; örneği için hata ayıklama etkin betikleri hata ayıklama modunda yükler ve komut dosyaları tarayıcının kullanıcı arabirimi seçimine göre yükleri yerelleştirilmiş.
- Özel kültürler dahil olmak üzere kültürler tanımını sağlar.
- Bu komut dosyaları sıkıştırma HTTP üzerinden sağlar.
- Birçok istekleri verimli bir şekilde yönetmek için komut dosyaları önbelleğe alır.
- Komut dosyaları şifrelenmiş bir URL cmdlet'ine yönelterek yöneltme bir katmanı ekler.

Komut dosyası başvuruları ScriptManager denetimi program aracılığıyla veya bildirim temelli biçimlendirme tarafından eklenebilir. Düzeltmeleri gönderilen gibi komut dosyasının adını olasılıkla değiştirmez gibi bildirim temelli biçimlendirme web sitesi projesini kendisi dışında katıştırılmış derlemeleri komut dosyaları ile çalışırken, özellikle yararlıdır.

## <a name="summary"></a>Özet

Web uygulamaları, daha büyük bir izleyici ulaşması arttıkça, daha geniş kültürler ve toplulukları ulaşabilmesi için gereken iş modeli çekirdek olur; yabancı para ile mücadele etmek e-ticaret web uygulamaları gerekir, içerik yönetim sistemleri içeriklerini ancak aynı zamanda bunların Gezinti ipuçları ve form alanlarını diğer dillere ve şirketler bu ihtiyacı olduğunu bilmeniz yapabileceksiniz değil yalnızca mevcut olması gerekir erişilebilir.

.NET Framework uydu derlemelerini ve XML kaynak (.resx) dosyaları bir Tekdüzen Kaynak dizeleri ve görüntüleri Ara şekilde sunmak için kullanan bir zengin yerelleştirme framework doğası gereği destekler. Microsoft AJAX Framework ve Microsoft AJAX komut dosyası kitaplığı gibi ASP.NET AJAX uzantıları bu programlama modeli için istemci tarafı koda kolay kaynak dize aramaları etkinleştirme desteği. Dosya adları belirli bir adlandırma şeması izlediğiniz sürece otomatik eklenmesi ScriptResource.axd aracılığıyla komut dosyası kaynakları (gerçek .js dosyaları), uydu derlemelerini destekler. Bu destek sayesinde, ASP.NET AJAX uzantıları betikleri yerelleştirme ve Genelleştirme uygulamaların basitleştirin.

## <a name="bio"></a>*Bio*

Tan göstermek Microsoft Web teknolojileri ile bu yana 1997 çalışma ve myKB.com Başkanı ise ([www.myKB.com](http://www.myKB.com)) kendisine ASP.NET yazılırken burada uzmanlaşmış tabanlı Bilgi Bankası yazılım çözümlerini odaklanmış uygulamaları. Tan temas kurulabileceğini doğrula e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog adresindeki [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [sonraki](understanding-asp-net-ajax-web-services.md)
