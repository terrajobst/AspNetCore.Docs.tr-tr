---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Tutarlı bir düzen oluşturma ASP.NET Web sayfaları (Razor) siteleri | Microsoft Docs
author: tfitzmac
description: Siteniz için web sayfaları oluşturmak için daha verimli hale getirmek için Web sitesi ve, c (örneğin, üstbilgiler ve altbilgiler) içeriği yeniden kullanılabilir bloklarını oluşturabilirsiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573378"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitelerinde tutarlı bir düzen oluşturma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede nasıl Düzen sayfaları bir ASP.NET Web sayfaları (Razor) Web sitesi içeriği (örneğin, üstbilgiler ve altbilgiler) yeniden kullanılabilir bloklarını oluşturmak için ve sitedeki tüm sayfalar için tutarlı bir görünüm oluşturmak için kullanabileceğiniz açıklanmaktadır.
> 
> **Öğrenecekleriniz:** 
> 
> - Üstbilgiler ve altbilgiler gibi içeriğinin yeniden kullanılabilir blokları oluşturma
> - Bir düzen kullanarak sitenizdeki tüm sayfalar için tutarlı bir görünüm oluşturma
> - Çalışma zamanında bir düzen sayfası veri iletmek nasıl.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - İçerik blokları, birden çok sayfada eklenecek HTML biçimli içerik içeren dosyalardır.
> - Web sayfalarında paylaştığı HTML biçimli içerik sayfalarıdır Düzen sayfaları.
> - `RenderPage`, `RenderBody`, Ve `RenderSection` sayfa öğelerini eklemek istediğiniz yeri ASP yöntemleri.
> - `PageData` İçerik bloklarının ve Düzen sayfaları arasında veri paylaşmanıza olanak sağlayan sözlük.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="about-layout-pages"></a>Düzen sayfaları hakkında

Birçok Web sitesi üstbilgi ve altbilgi veya kullanıcılar, oturum açtınız olduğunu bildiren bir kutu gibi her sayfada görüntülenen içeriğe sahip. ASP.NET metin, biçimlendirme ve normal web sayfası gibi kodu içeren bir içerik bloğu ile ayrı bir dosya oluşturmanıza olanak sağlar. Diğer sayfalarda bilgilerin görünmesini istediğiniz yere sitesinde, içerik bloğuna sonra ekleyebilirsiniz. Bu şekilde aynı içerik her sayfasına kopyalayıp gerekmez. Bu gibi ortak içerik oluşturma da sitenizi güncelleştirmek kolaylaştırır. Yalnızca tek bir dosya güncelleştirebilir ve değişiklikleri daha sonra her yerde yansıtılır içeriği değiştirmek gerektiğinde içeriği eklenmiş.

Aşağıdaki diyagramda, iş içeriğini nasıl engellediği gösterir. Bir tarayıcı web sunucusundan bir sayfayı istediğinde, ASP.NET içeriği blokları noktada ekler. burada `RenderPage` yöntemi ana sayfasında çağrılır. Tamamlandı (birleştirilmiş) sayfası tarayıcıya gönderilir.

![Nasıl RenderPage yöntemi başvurulan bir sayfa geçerli sayfasına ekler gösteren kavramsal diyagram.](3-creating-a-consistent-look/_static/image1.jpg)

Bu yordamda, ayrı dosyalarda bulunan iki içerik bloklarının (bir üstbilgi ve altbilgi) başvuruda bulunan bir sayfa oluşturacaksınız. Bu aynı içerik blokları, sitenizde herhangi bir sayfayı kullanabilirsiniz. İşiniz bittiğinde, sayfa şöyle elde edersiniz:

![RenderPage yöntemine yönelik çağrılar içeren bir sayfa çalıştırılmasını sonucu tarayıcıda bir sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image2.jpg)

1. Web sitenizin kök klasöründe adlı bir dosya oluşturun *Index.cshtml*.
2. Varolan biçimlendirme aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Kök klasöründe adlı bir klasör oluşturun *paylaşılan*.

    > [!NOTE]
    > Adlı bir klasörde web sayfaları arasında paylaşılan dosyaları depolamak için yaygın bir uygulamadır *paylaşılan*.
4. İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_Header.cshtml*.
5. Varolan içeriğin aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Dosya adı olduğuna dikkat edin  *\_Header.cshtml*, alt çizgi ile (\_) öneki olarak. Adı bir alt çizgiyle başlıyorsa ASP.NET tarayıcıya bir sayfa göndermez. Bu kişiler (yanlışlıkla veya aksi halde) bu sayfaları doğrudan istemelerini engeller. Kullanıcıların bu sayfaları &#8212;isteği gerçekten istemediğiniz çünkü bunların içinde içerik bloklara sahip adı sayfalara alt çizgi kullanmak iyi bir fikirdir; kesinlikle diğer sayfalarına eklenecek kalırlar.
6. İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_Footer.cshtml* ve içeriğini aşağıdakilerle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. İçinde *Index.cshtml* sayfasında, iki çağrıları eklemek `RenderPage` aşağıda gösterildiği gibi yöntemi:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Bu, içerik bloğuna bir web sayfasına eklenecek nasıl gösterir. Çağırmanız `RenderPage` yöntemi ve içeriği o noktada eklemek istediğiniz dosyanın adını geçirin. Burada, içeriğini eklediğinizi  *\_Header.cshtml* ve  *\_Footer.cshtml* içine dosyaları *Index.cshtml* dosya.
8. Çalıştırma *Index.cshtml* sayfasını bir tarayıcıda. (Webmatrix'te, içinde **dosyaları** çalışma alanında, dosyaya sağ tıklayın ve ardından **başlatma tarayıcıda**.)
9. Tarayıcıda, sayfa kaynağı görüntüleyin. (Örneğin, Internet Explorer'da, sayfanın sağ tıklayın ve ardından **kaynağı görüntüle**.)

    Bu içerik bloklarla dizin sayfası biçimlendirme birleştirir tarayıcıya gönderilen web sayfası biçimlendirme görmenize olanak tanır. Aşağıdaki örnek için işlenen sayfa kaynak gösterir *Index.cshtml*. Çağrıları `RenderPage` içine eklenen *Index.cshtml* gerçek üstbilgi ve altbilgi dosyaların içeriğini ile değiştirilmiştir.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Düzen sayfalarını kullanarak tutarlı bir görünüm oluşturma

Şu ana kadar birden çok sayfaya aynı içerik dahil etmek kolaydır gördünüz. Bir site için tutarlı bir görünüm oluşturmak için daha fazla yapılandırılmış bir yaklaşım, Düzen sayfaları kullanmaktır. Düzen sayfası bir web sayfası yapısını tanımlar, ancak herhangi bir gerçek içerik içermiyor. Düzen sayfası oluşturduktan sonra içerik bulunur ve bunları düzeni sayfaya bağlantı web sayfaları oluşturabilirsiniz. Bu sayfaları görüntülendiğinde, bunlar düzen sayfası göre biçimlendirilmiş olması. (Bu anlamda şablon diğer sayfalar tanımlı içerik için bir tür olarak düzen sayfası çalışır.)

Çağrı içeren düzen sayfası yalnızca tüm HTML sayfasını gibi olmasıdır `RenderBody` yöntemi. Konumunu `RenderBody` düzen sayfası yönteminde belirler burada içerik sayfasındaki bilgileri dahil edilir.

Aşağıdaki diyagramda nasıl içerik sayfaları gösterir ve Düzen sayfaları tamamlanmış web sayfası oluşturmak için çalışma zamanında birleştirilir. Tarayıcı bir içerik sayfasını ister. İçerik sayfasını kod için sayfa yapısı kullanmak için Düzen sayfası belirten da vardır. Düzen sayfasındaki içeriği noktada eklenir nerede `RenderBody` yöntemi çağrılır. İçerik blokları da eklenebilir düzeni sayfasına çağırarak `RenderPage` yöntemi, önceki bölümde yaptığınız şekilde. Web sayfası tamamlandıktan sonra tarayıcıya gönderilir.

![RenderBody yöntemine yönelik çağrılar içeren bir sayfa çalıştırılmasını sonucu tarayıcıda bir sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image3.jpg)

Aşağıdaki yordam bir düzen sayfasını ve bağlantı içerik sayfalarının ona nasıl oluşturulacağını gösterir.

1. İçinde *paylaşılan* Web sitenizin klasör adında bir dosya oluşturun  *\_Layout1.cshtml*.
2. Varolan içeriğin aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Kullandığınız `RenderPage` içerik bloklarının eklemek için bir düzen sayfası yöntemi. Düzen sayfası yalnızca bir çağrı içerebilir `RenderBody` yöntemi.
3. İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_Header2.cshtml* ve varolan içeriğin şununla değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Kök klasöründe yeni bir klasör oluşturun ve adlandırın *stilleri*.
5. İçinde *stilleri* klasör adında bir dosya oluşturun *Site.css* ve aşağıdaki stil tanımları ekleyin:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Bu stil tanımları yalnızca stil sayfaları, Düzen sayfaları ile nasıl kullanılabileceğini göstermek için aşağıda verilmiştir. İsterseniz, bu öğeler için kendi stil tanımlayabilirsiniz.
6. Kök klasöründe adlı bir dosya oluşturun *Content1.cshtml* ve varolan içeriğin şununla değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Bu düzen sayfası kullanacağı bir sayfadır. Sayfanın üstündeki kod bloğu bu içeriği biçimlendirmek için kullanılacak Düzen sayfasını gösterir.
7. Çalıştırma *Content1.cshtml* bir tarayıcıda. İşlenen sayfanın biçimini kullanır ve stil sayfası tanımlanan  *\_Layout1.cshtml* ve içinde tanımlanan metin (içerik) *Content1.cshtml*.

    ![[Görüntü]](3-creating-a-consistent-look/_static/image4.jpg)

    Ardından aynı düzen sayfası paylaşabilirsiniz ek içerik sayfaları oluşturmak için 6 yineleyebilirsiniz.

    > [!NOTE]
    > Sitenizi otomatik olarak aynı düzen sayfası bir klasördeki tüm içerik sayfalar için kullanabileceğiniz şekilde ayarlayabilirsiniz. Ayrıntılar için bkz [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Birden çok içerik bölümlerini sahip Düzen sayfaları tasarlama

Bir içerik sayfasını değiştirebilen içeriğe sahip birden fazla alana sahip düzenleri kullanmak istiyorsanız faydalı olduğu birden çok bölüm olabilir. İçerik sayfasında her bölüm bir benzersiz ad verin. (Varsayılan bölümü sol adlandırılmamış.) Düzen sayfasında eklediğiniz bir `RenderBody` yöntemi adlandırılmamış (varsayılan) bölümü nerede görüneceğini belirtirsiniz. Ardından ayrı ekleyin `RenderSection` adlandırılmış bölümler ayrı ayrı işlemek için yöntemleri.

Aşağıdaki diyagramda, ASP.NET birden çok bölümlere ayrılmıştır içeriği nasıl işlediği gösterilmektedir. Her adlandırılmış bölüm içerik sayfasını bölüm bloğunda yer alır. (Adlı `Header` ve `List` örnekte.) Framework noktasında düzen sayfası içerik bölümü ekler nerede `RenderSection` yöntemi çağrılır. Adlandırılmamış (varsayılan) bölümü noktada eklenmiş olduğu `RenderBody` yöntemi çağrıldığında, daha önce gördüğünüz gibi.

![Nasıl RenderSection yöntemi başvuruları bölümleri geçerli sayfasına ekler gösteren kavramsal diyagram.](3-creating-a-consistent-look/_static/image5.jpg)

Bu yordam, birden çok içerik bölümleri olan bir içerik sayfasını oluşturma ve birden çok içerik bölümlerini destekleyen bir düzen sayfasını kullanarak işleme gösterir.

1. İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_Layout2.cshtml*.
2. Varolan içeriğin aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Kullandığınız `RenderSection` üstbilgi ve liste bölümleri işlemek için yöntem.
3. Kök klasöründe adlı bir dosya oluşturun *Content2.cshtml* ve varolan içeriğin şununla değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Bu içerik sayfasını sayfanın üst kısmındaki kod bloğu içerir. Adlandırılmış her bölüm bir bölüm bloğu içinde yer alır. Sayfaya geri kalanı (adlandırılmamış) varsayılan içerik bölümü içerir.
4. Çalıştırma *Content2.cshtml* bir tarayıcıda.

    ![RenderSection yöntemine yönelik çağrılar içeren bir sayfa çalıştırılmasını sonucu tarayıcıda bir sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>İsteğe bağlı içerik bölümlerini yapma

Normalde, içerik sayfasında oluşturduğunuz bölüm düzeni sayfada tanımlı bölümleri eşleşiyor gerekir. Aşağıdakilerden birini oluşursa hataları alabilirsiniz:

- İçerik sayfasını düzen sayfası karşılık gelen hiçbir bölümünde bir bölüm içerir.
- Düzen sayfası olduğu için içerik yok bir bölüm içerir.
- Düzen sayfası aynı bölüm birden çok kez işlenecek deneyin yöntem çağrılarını içerir.

Ancak, Düzen sayfasında isteğe bağlı olarak bölüm bildirerek adlandırılmış bir bölümün bu davranışı geçersiz kılabilirsiniz. Bu düzen sayfası paylaşabilirsiniz ancak olabilir veya belirli bir bölümü için içerik olmayabilir birden çok içerik sayfaları tanımlamanıza olanak sağlar.

1. Açık *Content2.cshtml* ve aşağıdaki bölümde kaldırın:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Sayfayı kaydedin ve ardından bir tarayıcıda çalıştırın. İçerik sayfasını düzen sayfası, yani üstbilgi bölümünde tanımlanmış bir bölümün içeriğini sağlamadığından, bir hata iletisi görüntülenir.

    ![Bir sayfa çalıştırırsanız oluşan hatasından gösteren ekran görüntüsü RenderSection yöntemini çağırır ancak karşılık gelen sağlanmadı.](3-creating-a-consistent-look/_static/image7.jpg)
3. İçinde *paylaşılan* klasörü, açık  *\_Layout2.cshtml* sayfasında ve bu satırı değiştirin:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    Aşağıdaki kod ile:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Alternatif olarak, aynı sonuçları oluşturan aşağıdaki kod bloğu ile önceki kod satırı ile değiştirebilirsiniz:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Çalıştırma *Content2.cshtml* sayfasını bir tarayıcıda yeniden. (Tarayıcıda açın bu sayfayı hala varsa, yalnızca bu yenileyebilirsiniz.) Üst bilgi sahip olsa da bu kez herhangi bir hata ile sayfası görüntülenir.

## <a name="passing-data-to-layout-pages"></a>Düzen sayfaları için veri geçirme

Bir düzen sayfası başvurmak için gereken içerik sayfasındaki tanımlanan verileri olabilir. Bu durumda, içerik sayfasından veri düzeni sayfaya geçirilecek gerekir. Örneğin, bir kullanıcı oturum açma durumunu görüntülemek istediğiniz veya göster veya gizle kullanıcı girişini temel alarak içerik alanları isteyebilirsiniz.

Düzen sayfası için bir içerik sayfasından veri geçirmek için değerleri içine koyabilirsiniz `PageData` içerik sayfasının özelliği. `PageData` Sayfaları arasında geçirmek istediğiniz verileri tutmak ad/değer çiftleri koleksiyonu bir özelliktir. Düzen sayfasında dışı değerlerini sonra okuyabilir `PageData` özelliği.

Başka bir diyagrama aşağıdadır. Bu bir ASP.NET nasıl kullanabileceğinizi gösterir `PageData` değerleri içerik sayfasından düzeni sayfaya geçirilecek özelliği. ASP.NET web sayfası oluşturma başladığında oluşturur `PageData` koleksiyonu. İçerik sayfasındaki verileri yerleştirmek için kod yazma `PageData` koleksiyonu. Değerler `PageData` koleksiyonu, aynı zamanda içerik sayfanın diğer bölümlerinde veya ek içerik bloklarının tarafından erişilebilir.

![Nasıl bir içerik sayfasını PageData sözlük doldurmak ve bu bilgileri düzen sayfası geçirmek gösteren kavramsal diyagramı.](3-creating-a-consistent-look/_static/image8.jpg)

Aşağıdaki yordamda, bir düzen sayfasının içeriği sayfasından veri iletmek gösterilmiştir. Sayfa çalıştığında, düzen sayfası içinde tanımlanmış bir liste göstermek veya gizlemek kullanıcı olanak sağlayan bir düğme görüntüler. Kullanıcıların Düğmeye tıkladığınızda, onu bir true/false (Boole) değerine ayarlar `PageData` özelliği. Düzen sayfası, bu değeri okuyan ve false ise, liste gizler. Değeri ayrıca içerik sayfasındaki görüntülenip görüntülenmeyeceğini belirlemek için kullanılan **Gizle listesi** düğmesini veya **listesini göster** düğmesi.

![[Görüntü]](3-creating-a-consistent-look/_static/image9.jpg)

1. Kök klasöründe adlı bir dosya oluşturun *Content3.cshtml* ve varolan içeriğin şununla değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Verileri iki parça kodunu depolar `PageData` özelliği &#8212; web sayfası ve true veya false listesini görüntülenip görüntülenmeyeceğini belirtmek için başlığı.

    ASP.NET, HTML biçimlendirmesi koşullu kod bloğu kullanarak sayfasına yerleştirmenizi sağlar dikkat edin. Örneğin, `if/else` sayfasının gövdesindeki blok belirler bağlı olarak görüntülemek için hangi form `PageData["ShowList"]` ayarlanmış true.
2. İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_Layout3.cshtml* ve varolan içeriğin şununla değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Düzen sayfası bir ifadeyi içeren `<title>` başlık değerinden alır öğesi `PageData` özelliği. Ayrıca kullanır `ShowList` değerini `PageData` özellik listesi içerik bloğuna görüntülenip görüntülenmeyeceğini belirler.
3. İçinde *paylaşılan* klasör adında bir dosya oluşturun  *\_List.cshtml* ve varolan içeriğin şununla değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Çalıştırma *Content3.cshtml* sayfasını bir tarayıcıda. Sayfanın sol tarafında görünür listesiyle sayfası görüntülenir ve **Gizle listesi** altındaki düğmesini.

    ![Liste ve Gizle'List ' bildiren bir düğme içerir sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image10.jpg)
5. Tıklatın **Gizle listesi**. Listenin kaybolur ve düğmesi değişikliklerini **listesini göster**.

    !['List Göster' bildiren bir düğmeyi ve liste içermez sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image11.jpg)
6. Tıklatın **listesini göster** düğmesi ve listesi görüntülenir yeniden.

## <a name="additional-resources"></a>Ek Kaynaklar


[ASP.NET Web sayfaları için özelleştirme site genelinde davranışı](https://go.microsoft.com/fwlink/?LinkId=202906)
