---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "OWIN başlangıç sınıfı algılama | Microsoft Docs"
author: Praburaj
description: "Bu öğretici, hangi OWIN başlangıç sınıfı yüklenen yapılandırma gösterilmektedir. OWIN hakkında daha fazla bilgi için bir genel bakış, proje Katana bakın. Bu öğretici oluştu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 618f8fa23630dcf9821a54415766dc015694e535
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="owin-startup-class-detection"></a>OWIN başlangıç sınıfı algılama
====================
tarafından [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici, hangi OWIN başlangıç sınıfı yüklenen yapılandırma gösterilmektedir. OWIN hakkında daha fazla bilgi için bkz: [bir genel bakış, proje Katana](an-overview-of-project-katana.md). Bu öğretici Rick Anderson tarafından yazılan ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan ve Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Önkoşullar
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>OWIN başlangıç sınıfı algılama

 Her OWIN uygulama bileşenleri uygulama ardışık düzeni için belirttiğiniz başlangıç sınıfı vardır. İle çalışma zamanı, başlangıç sınıfı bağlanabilir farklı yolu vardır, barındırma modeline bağlı olarak (OwinHost, IIS ve IIS Express) seçin. Bu öğreticide gösterilen başlangıç sınıfı barındırma her uygulamada kullanılabilir. Başlangıç sınıfı barındırma çalışma zamanı bunlardan birini yaklaşıyor kullanarak bağlan:  

1. **Adlandırma**: Katana arar adlı bir sınıf için `Startup` derleme adı veya genel ad alanı eşleşen ad.
2. **OwinStartup özniteliği**: Bu çoğu geliştirici, başlangıç sınıfı belirtmek için sürer yaklaşımdır. Aşağıdaki öznitelik başlangıç sınıfı ayarlanacağını `TestStartup` sınıfını `StartupDemo` ad alanı. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 `OwinStartup` Özniteliği adlandırma kuralı geçersiz kılar. Bu öznitelik ile kolay bir ad da belirtebilirsiniz, ancak, kolay bir ad kullanarak da kullanmanızı gerektirir `appSetting` yapılandırma dosyası öğesi.
3. **Yapılandırma dosyanızda appSetting öğesi**: `appSetting` öğesi geçersiz kılmaları `OwinStartup` özniteliği ve adlandırma kuralı. Birden çok başlangıç sınıfı olabilir (her kullanan bir `OwinStartup` özniteliği) ve hangi başlangıç sınıfı biçimlendirme aşağıdakine benzer kullanarak bir yapılandırma dosyasına yüklenen yapılandırın:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 Derleme ve başlangıç sınıfı açıkça belirtir aşağıdaki anahtarı da kullanılabilir: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 Aşağıdaki XML yapılandırma dosyasında bir kolay başlangıç sınıfı adını belirtir `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 Yukarıdaki biçimlendirme şununla kullanılmalıdır `OwinStartup` neden olur ve kolay bir ad belirtir özniteliği `ProductionStartup2` çalıştırmak için sınıf.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. OWIN başlangıç bulmayı devre dışı bırakmak için ekleme `appSetting owin:AutomaticAppStartup` değerini `"false"` web.config dosyasında.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>OWIN başlangıç kullanarak bir ASP.NET Web uygulaması oluşturma

1. Boş bir Asp.Net web uygulaması oluşturun ve adlandırın **StartupDemo**. -Yükleyin `Microsoft.Owin.Host.SystemWeb` NuGet Paket Yöneticisi'ni kullanarak. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**. Aşağıdaki komutu girin:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- OWIN başlangıç sınıfı ekleyin. Visual Studio 2013'te projeyi sağ tıklatın ve seçin **sınıfı Ekle**. - **Yeni Öğe Ekle** iletişim kutusunda, girin *OWIN* arama alanı ve haline, adını değiştirin ve ardından **Ekle**.  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 Eklemek istediğiniz bir sonraki seferde bir *Owın başlangıç sınıfı*, içinde olacak kullanılabilir **Ekle** menüsü.  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 Alternatif olarak, projeyi sağ tıklatın ve seçin **Ekle**seçeneğini belirleyip **yeni öğe**ve ardından **Owın başlangıç sınıfı**.  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- Oluşturulan kod Değiştir *haline* aşağıdaki dosyasıyla:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 `app.Use` Lambda ifadesi, belirtilen ara yazılım bileşeni OWIN ardışık düzenine kaydetmek için kullanılır. Bu durumda biz gelen isteğini yanıtlamadan önce gelen isteklerin günlüğe yazılmasını ayarlıyorsanız. `next` Parametredir temsilci ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [görev](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) ardışık düzende sonraki bileşene. `app.Run` Lambda ifadesi ardışık düzene gelen istekleri kancalarını ve yanıt mekanizmasını sağlar.
     > [!NOTE]
     > Yukarıdaki kod biz kılınmıştır `OwinStartup` özniteliği ve biz adlı sınıf çalışan kuralına bağlı `Startup` .-tuşuna ***F5*** uygulamayı çalıştırın. Yenileme birkaç kez ulaştı.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
Not: Bu öğreticide görüntüleri gösterilen sayıyı gördüğünüz sayı eşleşmez. Milisaniye dize sayfayı yenileyin, yeni bir yanıtı göstermek için kullanılır.  
 İzleme bilgilerini görebilirsiniz **çıkış** penceresi.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Daha fazla başlangıç sınıfları ekleme

Bu bölümde başka bir başlangıç sınıfı ekleyeceğiz. Uygulamanız için birden çok OWIN başlangıç sınıfı ekleyebilirsiniz. Örneğin, geliştirme, test ve üretim için başlangıç sınıfları oluşturmak isteyebilirsiniz.

1. Yeni bir OWIN başlangıç sınıfı oluşturun ve adlandırın `ProductionStartup`.
2. Oluşturulan kod aşağıdakiyle değiştirin:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Denetim uygulamayı çalıştırmak için F5 tuşuna basın. `OwinStartup` Özniteliği belirtir üretim başlangıç sınıfı çalıştırılır.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Başka bir OWIN başlangıç sınıfı oluşturun ve adlandırın `TestStartup`.
5. Oluşturulan kod aşağıdakiyle değiştirin:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 `OwinStartup` Özniteliği aşırı yukarıdaki belirtir `TestingConfiguration` olarak *kolay* başlangıç sınıfı adı.
6. Açık *web.config* dosya ve başlangıç sınıfı kolay adını belirten OWIN uygulaması başlangıç anahtarını ekleyin:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Denetim uygulamayı çalıştırmak için F5 tuşuna basın. Uygulama ayarları öğe etkileyen ve test alır yapılandırma çalıştırılır.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Kaldırma *kolay* ad alanından `OwinStartup` özniteliğini `TestStartup` sınıfı.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. OWIN uygulaması başlangıç anahtarı Değiştir *web.config* aşağıdaki dosyasıyla:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Geri `OwinStartup` Visual Studio tarafından oluşturulan varsayılan öznitelik kodu her sınıfına özniteliğinde:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 Aşağıdaki OWIN uygulaması başlangıç anahtarlarının her birini çalıştırmak üretim sınıfı neden olur. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 Son başlangıç anahtarı başlangıç yapılandırması yöntemini belirtir. Aşağıdaki OWIN uygulaması başlangıç anahtarı için yapılandırma sınıf adını değiştirmenize izin verir `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Using Owinhost.exe

1. Web.config dosyasında aşağıdaki biçimlendirme ile değiştirin:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 Son anahtarı WINS, bunu bu durumda `TestStartup` belirtilir.
2. Owinhost PMC yükleyin: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Uygulama klasöre gidin (içeren klasör *Web.config* dosyası) ve bir komut istemi ve yazın: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 Komut penceresinde gösterecektir: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. URL ile bir tarayıcıyı başlatacak `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost yukarıda listelenen başlangıç kuralları dikkate alınır.
5. Komut penceresinde OwinHost çıkmak için Enter tuşuna basın.
6. İçinde `ProductionStartup` sınıfı, kolay adını belirten şu OwinStartup özniteliği eklemek *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Komut istemi ve türü: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 Üretim başlangıç sınıfı yüklenir.  
    ![](owin-startup-class-detection/_static/image9.png)  
 Birden çok başlangıç sınıf uygulamamız içeriyor ve bu örnekte, biz çalışma zamanına kadar yüklemek için hangi başlangıç sınıfı ertelenmiş.
8. Aşağıdaki çalışma zamanı başlangıç seçenekleri test edin:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
