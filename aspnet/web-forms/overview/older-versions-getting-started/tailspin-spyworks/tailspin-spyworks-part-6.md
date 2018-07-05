---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: '6. Bölüm: ASP.NET üyelik | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 6. bölüm ASP.NET üyelik ekler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6d01a745ca1428f065f564f676d483ee807eb52e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368479"
---
<a name="part-6-aspnet-membership"></a>6. Bölüm: ASP.NET üyeliği
====================
tarafından [ALi Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir. Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 6. bölüm ASP.NET üyelik ekler.


## <a id="_Toc260221672"></a>  ASP.NET üyeliği ile çalışma

![](tailspin-spyworks-part-6/_static/image1.png)

Güvenlik'e tıklayın

![](tailspin-spyworks-part-6/_static/image1.jpg)

Form kimlik doğrulaması kullandığınızdan emin olun.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Birkaç kullanıcı oluşturmak için "Create User" bağlantısını kullanın.

![](tailspin-spyworks-part-6/_static/image3.jpg)

İşiniz bittiğinde, Çözüm Gezgini penceresinde başvurun ve görünümü yenileyin.

![](tailspin-spyworks-part-6/_static/image2.png)

Unutmayın ASPNETDB. MDF ince oluşturuldu. Bu dosya, üyelik gibi temel ASP.NET hizmetlerini desteklemek için tabloları içerir.

Artık kullanıma alma işlemini uygulayan başlayabilirsiniz.

CheckOut.aspx sayfası oluşturarak başlayın.

CheckOut.aspx sayfası, yalnızca kullanıcılar ve oturum açma sayfasına açmadınız yeniden yönlendirme kullanıcılar kaydedilen biz erişimi kısıtlar için oturum açmış olan kullanıcılar tarafından kullanılabilir olmalıdır.

Bunu yapmak için aşağıdaki yapılandırma bölümüne bizim web.config dosyasının ekleyeceğiz.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

ASP.NET Web formları uygulamalarını şablon, otomatik olarak bir kimlik doğrulaması bölümü bizim web.config dosyasına eklenir ve varsayılan oturum açma sayfası oluşturulur.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Biz, kullanıcı oturum açtığında, anonim bir alışveriş sepetini geçirme için dosyanın arkasındaki Login.aspx kodunu değiştirmeniz gerekir. Sayfayı Değiştir\_olay şu şekilde yükleyin.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Ardından bu yeni oturum açan kullanıcının oturum adı ayarlayın ve alışveriş sepetine geçici oturum kimliği, kullanıcının bizim MyShoppingCart sınıfında MigrateCart yöntemi çağırarak değiştirmek için gibi bir "LoggedIn" olayı işleyicisi ekleyin. (.cs dosya içinde uygulanmış)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Uygulama MigrateCart() yöntemi bu ister.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Alışveriş sepeti sayfamızı yaptığımız kadar checkout.aspx içinde bir EntityDataSource ve GridView bizim kullanıma sayfa içinde kullanacağız.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

GridView denetimimiz MyList adlı bir "ondatabound" olay işleyicisi belirtir Not\_RowDataBound Haydi bu olay işleyicisi böyle uygulayın.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Her satır bağlıdır ve en alttaki GridView güncelleştirmeleri alışveriş toplamı sepete yöntemi kalmasını sağlar.

Bu aşamada sizi yerleştirilecek sırası "İnceleme" sunumu uyguladınız.

Şimdi bir boş Sepeti senaryosu sayfamıza birkaç satır kod ekleyerek işlemek\_yükleme olayı:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Kullanıcı "Gönder" düğmesine tıkladıktan sonra aşağıdaki kodu Gönder düğmesini tıklatın olay işleyicisi yürütülür.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Sipariş gönderme işlemi "et" bizim MyShoppingCart sınıfının SubmitOrder() yönteminde uygulanması sağlamaktır.

SubmitOrder olur:

- Tüm satır öğeleri alışveriş sepetini alın ve bunları yeni bir sipariş kaydı ve ilişkili OrderDetails kayıtları oluşturmak için kullanın.
- Sevkiyat Tarihi hesaplayın.
- Alışveriş sepeti temizleyin.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Bu örnek uygulama amaçları için yalnızca iki gün geçerli tarihe ekleyerek biz bir kullanıma sunulduğundan hesaplayın.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Şimdi uygulamayı çalıştıran bize alışveriş işlemi baştan sona test etmek için izin verir.

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-5.md)
> [İleri](tailspin-spyworks-part-7.md)
