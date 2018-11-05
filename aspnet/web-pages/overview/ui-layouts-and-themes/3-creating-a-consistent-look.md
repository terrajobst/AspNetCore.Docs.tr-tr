---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Tutarlı bir düzen oluşturma ASP.NET Web sayfaları (Razor) siteler | Microsoft Docs
author: Rick-Anderson
description: Siteniz için web sayfaları oluşturmak için daha verimli hale getirmek için yeniden kullanılabilir içerik (örneğin, üstbilgiler ve altbilgiler) Web sitesi ve, c için bloklarını oluşturabilirsiniz...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 83aef41c0baaeca6a25e09b4ea797ce9ee963a85
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021553"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitelerinde tutarlı bir düzen oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede nasıl Düzen sayfaları bir ASP.NET Web sayfaları (Razor) Web sitesinde içerik (örneğin, üstbilgiler ve altbilgiler) yeniden kullanılabilir bloklar oluşturur ve sitedeki tüm sayfalar için tutarlı bir görünüm oluşturmak için kullanabileceğiniz açıklanmaktadır.
> 
> **Öğrenecekleriniz:** 
> 
> - Yeniden kullanılabilir içerik üstbilgiler ve altbilgiler gibi blokları oluşturma
> - Sitenizdeki bir düzen kullanarak tüm sayfalar için tutarlı bir görünüm oluşturma
> - Nasıl verileri çalışma zamanında bir düzen sayfasına geçirir.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - İçerik blokları, birden çok sayfada eklenecek içeriği HTML biçimli dosyalardır.
> - Web sitesi sayfalarına tarafından paylaşılabilen HTML biçimli içeriği sayfalar düzen sayfaları.
> - `RenderPage`, `RenderBody`, Ve `RenderSection` öğelerin ekleneceği konum ASP yöntemleri.
> - `PageData` İçerik bloğu ile Düzen sayfaları arasında veri paylaşmasını sağlayan sözlük.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="about-layout-pages"></a>Yerleşim sayfaları hakkında

Birçok Web sitesi, bir üstbilgi ve altbilgi veya bunlar oturum açmadıysanız, kullanıcıların söyleyen bir kutu gibi her bir sayfasında görüntülenen içeriğe sahip. ASP.NET ile metin ve biçimlendirme normal web sayfası gibi bir kod içeren bir içerik bloğu ayrı bir dosya oluşturmanıza olanak sağlar. Ardından, diğer bilgilerin görünmesini istediğiniz sitenin sayfalarında içerik bloğu de ekleyebilirsiniz. Bu şekilde aynı içeriğin her sayfasına kopyalayıp gerekmez. Bu gibi ortak içerik oluşturma da sitenizi güncelleştirmek kolaylaştırır. İçeriği, içerik değiştirmeniz gerekir, yalnızca tek bir dosyayı güncelleştirebilirsiniz ve değişiklikleri daha sonra her yerde yansıtılır eklenmiş.

Aşağıdaki diyagramda, nasıl iş içeriği engeller gösterilmektedir. Bir tarayıcı web sunucusundan bir sayfa istediğinde, ASP.NET içeriği blokları noktada ekler. burada `RenderPage` ana sayfada yöntemi çağrılır. Tamamlandı (birleştirilmiş) sayfası, daha sonra tarayıcıya gönderilir.

![RenderPage yöntemi başvurulan bir sayfa geçerli sayfaya nasıl eklediğini gösteren kavramsal diyagram.](3-creating-a-consistent-look/_static/image1.jpg)

Bu yordamda ayrı dosyalarında bulunan iki içeriği blokları (bir üstbilgi ve altbilgi) başvuran bir sayfa oluşturacaksınız. Aynı içerik bloklar sitenizde herhangi bir sayfa kullanabilirsiniz. İşiniz bittiğinde bunun gibi bir sayfa elde edersiniz:

![RenderPage yöntemine yönelik çağrılar içeren bir sayfa çalıştırılmasının sonuçlarını tarayıcıda bir sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image2.jpg)

1. Web sitenizin kök klasöründe adlı bir dosya oluşturun *Index.cshtml*.
2. Mevcut biçimlendirme aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Kök klasöründe adlı bir klasör oluşturun *paylaşılan*.

    > [!NOTE]
    > Adlı bir klasörde web sayfaları arasında paylaşılan dosyaları depolamak için yaygın bir uygulamadır *paylaşılan*.
4. İçinde *paylaşılan* klasöründe adlı bir dosya oluşturun  *\_Header.cshtml*.
5. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Dosya adı olduğuna dikkat edin  *\_Header.cshtml*, alt çizgi ile (\_) öneki olarak. Adının bir alt çizgiyle başlıyorsa, ASP.NET bir sayfa tarayıcıya göndermez. Bu kişiler (yanlışlıkla veya başka türlü) bu sayfaları doğrudan istemelerini engeller. Kullanıcıların bu sayfa istemek gerçekten istemediğinden, içerik bloklarının, içeren ad sayfaların alt çizgi kullanmak için iyi bir fikirdir &#8212; kesinlikle diğer sayfalarına eklenecek kalırlar.
6. İçinde *paylaşılan* klasöründe adlı bir dosya oluşturun  *\_Footer.cshtml* ve içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. İçinde *Index.cshtml* sayfasında, eklemek için iki çağrıları `RenderPage` burada gösterildiği gibi yöntemi:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Bu, bir içerik bloğunu bir web sayfasına nasıl ekleneceğini gösterir. Çağırmanızı `RenderPage` yöntemi ve içeriğini bu noktada eklemek istediğiniz dosyanın adını geçirin. Burada, içeriğini eklediğinizi  *\_Header.cshtml* ve  *\_Footer.cshtml* dosyalarınızı *Index.cshtml* dosya.
8. Çalıştırma *Index.cshtml* sayfasını bir tarayıcıda. (Webmatrix'te, içinde **dosyaları** çalışma alanında, dosyaya sağ tıklayın ve ardından **tarayıcıda Başlat**.)
9. Tarayıcıda, sayfa kaynağı görüntüleyin. (Örneğin, Internet Explorer'da, sayfanın sağ tıklayın ve ardından **kaynağı görüntüle**.)

    Bu içerik bloklarla dizin sayfası biçimlendirme birleştiren tarayıcıya gönderilen web sayfası biçimlendirme görmenize olanak sağlar. Aşağıdaki örnek için işlenen sayfa kaynağı gösterir *Index.cshtml*. Çağrıları `RenderPage` içine yerleştirdiğiniz *Index.cshtml* üstbilgi ve altbilgi dosyaların asıl içeriğini ile değiştirilmiştir.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Yerleşim sayfaları kullanarak tutarlı bir görünüm oluşturma

Şu ana kadar birden çok sayfalarında aynı içerik dahil etmek kolay olduğunu gördünüz. Bir site için tutarlı bir görünüm oluşturmak için daha fazla yapılandırılmış bir yaklaşım, Düzen sayfaları kullanmaktır. Bir düzen sayfası, bir web sayfası yapısını tanımlar, ancak herhangi bir gerçek içeriği içermiyor. Bir düzen sayfası oluşturduktan sonra içeriği içeren ve ardından bunları Düzen sayfasına bağlantı web sayfaları oluşturabilirsiniz. Bu sayfalar görüntülendiğinde, bunlar göre düzen sayfası biçimlendirilmesi. (Bu anlamda bir düzen sayfası bir tür diğer sayfalarında tanımlı içerik için şablon görevi görür.)

Bir çağrı içeren düzen sayfası yalnızca bir HTML sayfası gibi olmasıdır `RenderBody` yöntemi. Konumu `RenderBody` düzen sayfası yönteminde belirler burada içerik sayfası bilgileri dahil edilir.

Aşağıdaki diyagramda nasıl içerik sayfalarını gösterir ve yerleşim sayfaları tamamlandı web sayfası oluşturmak için çalışma zamanında birleştirilir. Bir içerik sayfasının tarayıcı ister. İçerik sayfası kod için sayfa yapısı kullanmak için Düzen sayfası belirten da vardır. Düzen sayfası, içeriğin noktada eklenir burada `RenderBody` yöntemi çağrılır. İçerik bloklar da eklenebilir Düzen sayfasına çağırarak `RenderPage` yöntemi, önceki bölümde yaptığınız şekilde. Web sayfası tamamlandıktan sonra tarayıcıya gönderilir.

![RenderBody yöntemine yönelik çağrılar içeren bir sayfa çalıştırılmasının sonuçlarını tarayıcıda bir sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image3.jpg)

Aşağıdaki yordam bir düzen sayfası ve bağlantı içerik sayfalarının ona nasıl oluşturulacağını gösterir.

1. İçinde *paylaşılan* Web sitenizin, klasör adında bir dosya oluşturun  *\_Layout1.cshtml*.
2. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Kullandığınız `RenderPage` içerik bloklarının eklemek için bir düzen sayfası yöntemi. Bir düzen sayfası, yalnızca bir çağrı içerebilir `RenderBody` yöntemi.
3. İçinde *paylaşılan* klasöründe adlı bir dosya oluşturun  *\_Header2.cshtml* ve varolan içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Kök klasöründe yeni bir klasör oluşturun ve adlandırın *stilleri*.
5. İçinde *stilleri* klasöründe adlı bir dosya oluşturun *Site.css* ve aşağıdaki stil tanımları ekleyin:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Bu stil tanımları yalnızca stil sayfaları, Düzen sayfaları ile nasıl kullanılabileceğini göstermek için aşağıda verilmiştir. İsterseniz, bu öğeler için kendi stilleri tanımlayabilirsiniz.
6. Kök klasöründe adlı bir dosya oluşturun *Content1.cshtml* ve varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Bu düzen sayfası kullanacağı sayfasıdır. Sayfanın üstündeki kod bloğu, bu içeriği biçimlendirmek için kullanılacak Düzen sayfasını gösterir.
7. Çalıştırma *Content1.cshtml* bir tarayıcıda. İşlenen sayfanın biçimini kullanır ve stil sayfası tanımlanmış  *\_Layout1.cshtml* ve içinde tanımlanan metin (içerik) *Content1.cshtml*.

    ![[image]](3-creating-a-consistent-look/_static/image4.jpg)

    Ardından aynı düzen sayfası paylaşabilirsiniz ek içerik sayfaları oluşturmak için 6. adım yineleyebilirsiniz.

    > [!NOTE]
    > Sitenizi ayarlayabilirsiniz, böylece bir klasördeki tüm içerik sayfalarının aynı düzen sayfası otomatik olarak kullanabilirsiniz. Ayrıntılar için bkz [Site genelinde davranışı ASP.NET Web sayfaları için özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Birden çok içerik bölümleri olan sayfa düzeni tasarlama

İçerik sayfası değiştirilebilir içeriğe sahip birden fazla alana sahip düzenleri kullanmak istiyorsanız yararlı olan birden fazla bölüm olabilir. İçerik sayfası her bölüm benzersiz bir ad verin. (Varsayılan bölümü sola adlandırılmamış.) Düzen sayfası eklediğiniz bir `RenderBody` adlandırılmamış (varsayılan) bölümü göründüğü belirtmek için yöntemi. Ardından ayrı eklediğiniz `RenderSection` adlandırılmış bölümler ayrı ayrı işlemek için yöntemler.

Aşağıdaki diyagramda, ASP.NET birden çok bölümlere ayrılmıştır içeriği nasıl işlediği gösterilmektedir. Her adlandırılmış bir bölümün içerik sayfası bölümü bloğunda yer alır. (Bunlar adlı `Header` ve `List` örnekte.) Framework noktada düzen sayfası içeriği bölümüne ekler burada `RenderSection` yöntemi çağrılır. Adsız (varsayılan) bölümü noktada eklenir burada `RenderBody` yöntemi çağrıldığında, daha önce bahsettiğim gibi.

![RenderSection yöntemi başvuruları bölümleri geçerli sayfaya nasıl eklediğini gösteren kavramsal diyagram.](3-creating-a-consistent-look/_static/image5.jpg)

Bu yordam, birden çok içerik bölümleri olan bir içerik sayfası oluşturma ve birden fazla içerik bölümünü destekleyen bir düzen sayfası kullanılarak nasıl oluşturulacağını gösterir.

1. İçinde *paylaşılan* klasöründe adlı bir dosya oluşturun  *\_Layout2.cshtml*.
2. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Kullandığınız `RenderSection` başlığı ve listesine bölümleri işlemek için yöntemi.
3. Kök klasöründe adlı bir dosya oluşturun *Content2.cshtml* ve varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Bu içerik sayfası, sayfanın üst kısmındaki bir kod bloğu içerir. Her adlandırılmış bir bölümün bir bölüm bloğu içinde yer alır. Sayfanın geri kalanını (adlandırılmamış) varsayılan içerik bölümü içerir.
4. Çalıştırma *Content2.cshtml* bir tarayıcıda.

    ![RenderSection yöntemine yönelik çağrılar içeren bir sayfa çalıştırılmasının sonuçlarını tarayıcıda bir sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>İçerik bölümleri isteğe bağlı hale getirme

Normalde, içerik sayfasında oluşturduğunuz bölüm düzeni sayfada tanımlı bölüm eşleşmesi gerekir. Aşağıdakilerden herhangi biri meydana gelirse, hataları alabilirsiniz:

- İçerik sayfası karşılık gelen hiçbir düzen sayfası bölümünde bir bölüm içerir.
- Düzen sayfası içerik bir bölümü içerir.
- Düzen sayfası aynı bölüme birden çok kez oluşturulacak deneyin yöntem çağrılarını içerir.

Ancak, Düzen sayfasında isteğe bağlı olarak bölüm bildirerek adlandırılmış bir bölümün için bu davranışı geçersiz kılabilirsiniz. Bu, bir yerleşim sayfası paylaşabilirsiniz ancak olabilir veya belirli bir bölümünde içeriği olmayabilir birden çok içerik sayfalarını tanımlamanıza olanak sağlar.

1. Açık *Content2.cshtml* ve aşağıdaki bölümde kaldırın:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Sayfayı kaydedin ve ardından bir tarayıcıda çalıştırın. İçerik sayfası içeriği düzen sayfası, yani üst bilgisi bölümü tanımlı bir bölüm için sağlamadığından, bir hata iletisi görüntülenir.

    ![Bir sayfa çalıştırırsanız oluşan bir hata gösteren ekran görüntüsü RenderSection yöntemini çağırır. ancak karşılık gelen bölüm sağlanmadı.](3-creating-a-consistent-look/_static/image7.jpg)
3. İçinde *paylaşılan* açık klasör  *\_Layout2.cshtml* sayfasında ve bu satırı değiştirin:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    Aşağıdaki kod ile:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Alternatif olarak, aynı sonuçları oluşturan aşağıdaki kod bloğu ile önceki kod satırının yerini alabilir:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Çalıştırma *Content2.cshtml* sayfasını bir tarayıcıda yeniden. (Bu sayfanın tarayıcıda açın hala varsa, yalnızca bu yenileyebilirsiniz.) Üst bilgi sahip olsa da bu süre ile herhangi bir hata sayfası görüntülenir.

## <a name="passing-data-to-layout-pages"></a>Düzen sayfalarına veri geçirme

Bir düzen sayfasına başvurmak için gereken içerik sayfasında tanımlanan bir veri olabilir. Bu durumda, içerik sayfasından veri düzeni sayfaya geçirilecek gerekir. Örneğin, bir kullanıcı oturum açma durumunu görüntülemek isteyebilirsiniz veya kullanıcı girişini temel alarak içerik alanları gizlemek veya göstermek isteyebilirsiniz.

Bir düzen sayfası için bir içerik sayfasından veri geçirmek için değerleri içine koyabilirsiniz `PageData` içerik sayfası özelliği. `PageData` Özelliğidir, sayfalar arasında geçirmek istediğiniz verileri içeren ad/değer çiftleri koleksiyonu. Düzen sayfası ardından tanesi değerlerini okuyabilirsiniz `PageData` özelliği.

Başka bir diyagrama aşağıda verilmiştir. Bu bir ASP.NET nasıl kullanabileceğinizi gösterir `PageData` özellik değerleri, içerik sayfasından Düzen sayfaya geçirilecek. ASP.NET web sayfası oluşturmak başladığında oluşturduğu `PageData` koleksiyonu. İçerik sayfasındaki verileri yerleştirmek için kod yazma `PageData` koleksiyonu. Değerler `PageData` koleksiyonu, ayrıca diğer bölümlerinde içerik sayfası veya ek içeriği blokları tarafından erişilebilir.

![Nasıl bir içerik sayfasının PageData sözlük doldurmak ve bu bilgileri geçirmek için Düzen sayfası gösteren kavramsal diyagram.](3-creating-a-consistent-look/_static/image8.jpg)

Aşağıdaki yordamda, bir yerleşim sayfası için bir içerik sayfasından veri iletmek gösterilmiştir. Sayfa çalıştığında, kullanıcının Düzen sayfasında tanımlanan bir listesini göstermek veya gizlemek sağlayan bir düğme görüntüler. Kullanıcı düğmeyi tıklattığınızda, bir true/false (Boole) değerini ayarlar `PageData` özelliği. Düzen sayfası, bu değeri okur ve yanlış ise, liste gizler. Değer da görüntülenip görüntülenmeyeceğini belirlemek için içerik sayfasındaki kullanılır **Gizle listesi** düğmesini veya **listesini göster** düğmesi.

![[image]](3-creating-a-consistent-look/_static/image9.jpg)

1. Kök klasöründe adlı bir dosya oluşturun *Content3.cshtml* ve varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Kod iki veri parçalarını depolar `PageData` özelliği &#8212; web sayfası ve true veya false listesini görüntülenip görüntülenmeyeceğini belirtmek için başlığı.

    ASP.NET sayfasına koşullu olarak bir kod bloğu kullanarak HTML biçimlendirmesini koymadan sağlar dikkat edin. Örneğin, `if/else` sayfasının gövdesi bloğunda belirler bağlı olarak görüntülemek için form `PageData["ShowList"]` ayarlanır true.
2. İçinde *paylaşılan* klasöründe adlı bir dosya oluşturun  *\_Layout3.cshtml* ve varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Düzen sayfası içeren bir ifadede `<title>` başlık değerini alır öğesi `PageData` özelliği. Ayrıca kullanan `ShowList` değerini `PageData` özelliği liste içerik bloğu görüntülenip görüntülenmeyeceğini belirlemek için.
3. İçinde *paylaşılan* klasöründe adlı bir dosya oluşturun  *\_List.cshtml* ve varolan içeriği aşağıdakiyle değiştirin:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Çalıştırma *Content3.cshtml* sayfasını bir tarayıcıda. Sayfanın sol tarafında görünür listesiyle sayfası görüntülenir ve **Gizle listesi** altındaki düğmesini.

    ![Liste ve Gizle'List ' ifadesini içeren bir düğme içeren sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image10.jpg)
5. Tıklayın **Gizle listesi**. Listenin kaybolur ve düğmeyi değişikliklerini **listesini göster**.

    ![Liste ve Göster'List ' ifadesini içeren bir düğme içermez sayfasını gösteren ekran görüntüsü.](3-creating-a-consistent-look/_static/image11.jpg)
6. Tıklayın **listesini göster** düğmesini ve listeden görüntülenen yeniden.

## <a name="additional-resources"></a>Ek Kaynaklar


[ASP.NET Web sayfaları için site geneline yönelik davranışını özelleştirme](https://go.microsoft.com/fwlink/?LinkId=202906)
