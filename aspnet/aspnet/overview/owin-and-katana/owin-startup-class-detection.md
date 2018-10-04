---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN başlangıç sınıfı algılama | Microsoft Docs
author: Praburaj
description: Bu öğreticide, hangi OWIN başlangıç sınıfı yüklenen yapılandırma işlemi gösterilmektedir. Bir genel bakış, Project Katana'ya OWIN hakkında daha fazla bilgi için bkz. Bu öğreticide oluştu...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 49ea01bf2170698de66b633b260664ce3d105ca1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577489"
---
<a name="owin-startup-class-detection"></a>OWIN başlangıç sınıfı algılama
====================
tarafından [Praburaj Yöneticisi](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Bu öğreticide, hangi OWIN başlangıç sınıfı yüklenen yapılandırma işlemi gösterilmektedir. OWIN hakkında daha fazla bilgi için bkz. [bir genel bakış, Project Katana'ya](an-overview-of-project-katana.md). Bu öğreticide, Rick Anderson tarafından yazılmış ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Yöneticisi ve Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Önkoşullar
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>OWIN başlangıç sınıfı algılama

 Her bir OWIN uygulaması bileşenleri uygulama ardışık düzeni için belirttiğiniz başlangıç sınıfı vardır. Çalışma zamanı ile başlangıç sınıfınıza bağlanabilir farklı yolu vardır, barındırma modeline bağlı olarak (OwinHost, IIS ve IIS Express) seçin. Bu öğreticide gösterilen başlangıç sınıfı barındırma her uygulamada kullanılabilir. Başlangıç sınıfı, barındırma çalışma zamanı bunlardan birini yaklaşıyor kullanarak bağlanın:  

1. **Adlandırma kuralı**: Katana görünen adlı bir sınıf için `Startup` derleme adı veya genel ad eşleşen bir ad.
2. **OwinStartup özniteliği**: Çoğu geliştirici, başlangıç sınıfı belirtmek için alacağınız yaklaşım budur. Başlangıç sınıfı aşağıdaki öznitelik ayarlayacak `TestStartup` sınıfını `StartupDemo` ad alanı. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   `OwinStartup` Özniteliği adlandırma kuralını geçersiz kılar. Bu öznitelik ile kolay bir ad da belirtebilirsiniz, ancak bir kolay ad kullanmanızı da kullanmanızı gerekli hale getirmiş `appSetting` yapılandırma dosyasındaki öğesi.
3. **Yapılandırma dosyalarında appSetting öğesi**: `appSetting` öğesini geçersiz kılar `OwinStartup` özniteliği ve adlandırma kuralları. Birden çok başlangıç sınıfı olabilir (kullanarak her bir `OwinStartup` özniteliği) ve hangi başlangıç sınıfı kullanarak biçimlendirme aşağıdakine benzer bir yapılandırma dosyasında yüklenen yapılandırın:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Derleme ve başlangıç sınıfı açıkça belirtir aşağıdaki anahtarı de kullanılabilir: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Yapılandırma dosyasında aşağıdaki XML'i kolay başlangıç sınıf adını belirtir. `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Yukarıdaki biçimlendirme şunlarla birlikte kullanılmalıdır `OwinStartup` kolay bir ad belirtir ve neden özniteliği `ProductionStartup2` çalıştırmak için sınıf.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. OWIN başlangıç bulma devre dışı bırakmak için ekleme `appSetting owin:AutomaticAppStartup` değeriyle `"false"` web.config dosyasında.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>OWIN Başlangıç'ı kullanarak bir ASP.NET Web uygulaması oluşturma

1. Boş bir Asp.Net web uygulaması oluşturun ve adlandırın **StartupDemo**. -Yükleme `Microsoft.Owin.Host.SystemWeb` NuGet Paket Yöneticisi'ni kullanarak. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Aşağıdaki komutu girin:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. OWIN başlangıç sınıfı ekleyin. Visual Studio 2013'te projeyi sağ tıklatın ve seçin **sınıfı Ekle**. - **Yeni Öğe Ekle** iletişim kutusuna *OWIN* arama alanını ve için Startup.cs adını değiştirin ve ardından **Ekle**.  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   Eklemek istediğiniz sonraki açışınızda bir *Owın başlangıç sınıfı*, bunu olacak kullanılabilir **Ekle** menüsü.  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   Alternatif olarak, projeyi sağ tıklatın ve seçin **Ekle**, ardından **yeni öğe**ve ardından **Owın başlangıç sınıfı**.  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- Oluşturulan kod değiştirin *Startup.cs* aşağıdaki dosya:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  `app.Use` Lambda ifadesi belirtilen bir ara yazılım bileşeni OWIN ardışık düzenine kaydetmek için kullanılır. Bu durumda biz gelen isteği yanıtlamayı önce gelen isteklerin günlük ayarlıyorsunuz. `next` Parametresi, temsilci ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [görev](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) ardışık düzende sonraki bileşene. `app.Run` Lambda ifadesi, gelen istekler için bir işlem hattı kancaları ve yanıt mekanizmasını sağlar.
     > [!NOTE]
     > Yukarıdaki kodda biz yorum `OwinStartup` özniteliği ve adlı sınıfını çalışan kuralına bağlı `Startup` .-basın ***F5*** uygulamayı çalıştırın. Yenileme birkaç kez basın.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  Not: Bu öğreticide görüntüde gösterilen sayıya gördüğünüz sayıyı eşleşmez. Milisaniyeden kısa dize, sayfayı yenileyin, yeni bir yanıt göstermek için kullanılır.  
  İzleme bilgileri gördüğünüz **çıkış** penceresi.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Daha fazla başlangıç sınıfları ekleme

Bu bölümde başka bir başlangıç sınıfı ekleyeceğiz. Uygulamanızı birden çok OWIN başlangıç sınıfı ekleyebilirsiniz. Örneğin, geliştirme, test ve üretim için başlangıç sınıfları oluşturmak isteyebilirsiniz.

1. Yeni bir OWIN başlangıç sınıfı oluşturun ve adlandırın `ProductionStartup`.
2. Oluşturulan kodu aşağıdakiyle değiştirin:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Denetim uygulamayı çalıştırmak için F5 tuşuna basın. `OwinStartup` Özniteliği belirtir üretim başlangıç sınıfı çalıştırılır.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Başka bir OWIN başlangıç sınıfı oluşturun ve adlandırın `TestStartup`.
5. Oluşturulan kodu aşağıdakiyle değiştirin:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   `OwinStartup` Özniteliği aşırı yukarıdaki belirtir `TestingConfiguration` olarak *kolay* başlangıç sınıfı adı.
6. Açık *web.config* dosya ve başlangıç sınıfı kolay adını belirten OWIN uygulaması başlangıç anahtarı ekleyin:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Denetim uygulamayı çalıştırmak için F5 tuşuna basın. Uygulama ayarları öğesi etkileyen ve test alır yapılandırma çalıştırılır.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Kaldırma *kolay* ad alanından `OwinStartup` özniteliğini `TestStartup` sınıfı.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. OWIN uygulaması başlangıç anahtarına değiştirin *web.config* aşağıdaki dosya:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Geri döndürme `OwinStartup` her sınıf için Visual Studio tarafından oluşturulan varsayılan öznitelik kodu özniteliği:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Her bir OWIN uygulaması başlangıç anahtarı aşağıdaki üretim sınıfı çalışmasına neden olur. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    Son başlangıç anahtarı başlangıç yapılandırmasını yöntemi belirtir. Aşağıdaki OWIN uygulaması başlangıç anahtarı için yapılandırma sınıfı adını değiştirmenizi sağlar `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Owinhost.exe kullanma

1. Web.config dosyasına aşağıdaki biçimlendirme ile değiştirin:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Son anahtar WINS, bu durumda bunu `TestStartup` belirtilir.
2. Owinhost PMC'yi yükleyin: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Uygulama klasöre gidin (içeren klasörü *Web.config* dosyası) bir komut istemi ve türü: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Komut penceresinde gösterir: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. URL ile bir tarayıcıyı başlatacak `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost başlangıç kurallarına yukarıda listelenen dikkate alınır.
5. Komut penceresinde OwinHost çıkmak için Enter tuşuna basın.
6. İçinde `ProductionStartup` sınıfı, kolay adını belirten şu OwinStartup özniteliği ekleyin *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Komut istemi ve türü: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Üretim başlangıç sınıfı yüklenir.  
    ![](owin-startup-class-detection/_static/image9.png)  
   Uygulamamızı birden çok başlangıç sınıfı vardır ve bu örnekte biz kadar çalışma zamanının hangi başlangıç sınıfı ertelenmiş yürütmeleri vardır.
8. Aşağıdaki çalışma zamanı başlatma seçenekleri test edin:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
