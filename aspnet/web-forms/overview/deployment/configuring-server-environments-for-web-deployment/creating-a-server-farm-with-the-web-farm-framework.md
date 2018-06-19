---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Web Farm Framework ile bir sunucu grubu oluşturma | Microsoft Docs
author: jrjlee
description: Bu konuda, Web grubu çerçevesi (WFF) 2.0 oluşturmak ve web sunucu grubundan sunucuları koleksiyonu yapılandırmak için nasıl kullanılacağını açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 53a91660953795f2c55edcd795b053641d308dfe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882471"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Web Farm Framework ile bir sunucu grubu oluşturma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, Web grubu çerçevesi (WFF) 2.0 oluşturmak ve web sunucu grubundan sunucuları koleksiyonu yapılandırmak için nasıl kullanılacağını açıklar.


WFF web platformu ürünlerini ve bileşenleri, web uygulamaları, Web siteleri ve yapılandırma ayarlarını birden çok yük dengeli web sunucusu arasında eşitlemenize olanak tanır. Hazırlama ve üretim ortamları gibi birden fazla web sunucusu ihtiyaç duyacağınız senaryolarda bu çok, dağıtım ve yapılandırma işlemini kolaylaştırabilir. Tek bir sunucu için bir web uygulaması dağıtabilirsiniz&#x2014; *birincil sunucu*&#x2014;ve WFF otomatik olarak, web uygulaması sunucu grubundaki tüm diğer web sunucularındaki çoğaltır.

## <a name="understanding-the-web-farm-framework"></a>Web Farm Framework anlama

WFF 2.0 sağlamak, yönetmek ve içerik web sunucuları grubuna dağıtmak için kullanabilirsiniz. Bir WFF dağıtımı üç anahtar sunucu rollerini içerir:

- *Denetleyici sunucusu*. Bu sunucuyu oluşturmak ve WFF sunucu grupları yapılandırmak için kullanın. Denetleyici sunucusunun web platformu bileşenlerini, yapılandırma ayarları ve uygulamaları sunucu grubunda web sunucuları arasında eşitlenmesi yönetir. WFF 2.0 denetleyicisi sunucusuna yükleyin ve denetleyici sunucunun sırayla bir sunucu grubundaki sunucuların her birinde WFF Aracısı'nı yükler. Denetleyici sunucusu kavramsal olarak herhangi bir WFF sunucu grubuna ait değil ve bir tek denetleyici sunucusunun birden çok sunucu grupları yönetebilirsiniz. Bu senaryoda, hazırlama sunucu grubu ve üretim sunucu grubu oluşturmak ve yönetmek için tek bir WFF denetleyicisi sunucu kullanın.
- *Birincil sunucu*. Her WFF sunucu grubu tek bir birincil sunucu içerir. Web platformu bileşenlerini yüklediğinizde veya birincil sunucu uygulamaları Dağıt, WFF değişikliklerinizi diğer sunuculara sunucu grubundaki eşitler.
- *İkincil sunucu*. Her WFF sunucu grubu bir veya daha fazla ikincil sunucuları içerir. Birincil sunucuda yaptığınız tüm değişiklikler, sunucu grubu içindeki her ikincil sunucu için çoğaltılır.

Bu, bu sunucu rolleri Fabrikam, Inc. hazırlama ve üretim ortamlarını nasıl ilişkili olduğunu gösterir:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

Bu senaryoda, hazırlama ortamında ve üretim ortamında her ikisi de WFF sunucu grupları yapılandırılır. Bir tek WFF denetleyici sunucusunun hem grupları yönetir. Her sunucu grubu içinde değişiklikleri birincil sunucuya her ikincil sunucu için çoğaltılır.

Hazırlama ve üretim ortamlarını yapılandırmak başlamadan önce bu makaleler WFF 2.0 temel kavramları öğrenmeniz okumanızı öneririz:

- [IIS 7 için Web Farm Framework 2.0 genel bakış](https://go.microsoft.com/?linkid=9805126)
- [IIS 7 için bir sunucu grubunda Web grubu Framework 2.0 ile ayarlama](https://go.microsoft.com/?linkid=9805127)
- [Sistem ve IIS 7 için Web Farm Framework 2.0 Platform gereksinimleri](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Görev genel bakış

Görevleri ve bu konudaki yönergeler tamamlamak için en az üç sunucuya ihtiyacınız olacak&#x2014;bir WFF denetleyicisi, sunucu grubu için tek bir birincil web sunucusunda ve sunucu grubu için bir veya daha fazla ikincil web sunucusu. Daha fazla ikincil sunucular herhangi bir zamanda WFF sunucu grubuna ekleyebilirsiniz. Oluşturmak ve yapılandırmak için gereken hazırlık veya üretim ortamınız için WFF sunucu grubu için bir yüksek düzeyde:

- Bir denetleyici sunucusunun Internet Information Services (IIS) 7.5 ve WFF 2.0 yükleyerek oluşturun.
- Birincil ve ikincil sunucular, bir ortak yönetici hesabı oluşturmayı ve güvenlik duvarı özel durumlarını yapılandırma hazırlayın.
- Sunucu grubu denetleyicisi sunucusuna IIS Yöneticisi'ni kullanarak yapılandırın.
- IIS uygulama isteği yönlendirme (ARR) veya alternatif bir Yük Dengeleme teknolojisi kullanarak yük dengelemeyi yapılandırın.

Görevleri ve bu konudaki yönergeler ile Windows Server 2008 R2 çalıştıran temiz sunucu yapıları başlatıyorsanız varsayalım. Her sunucu için başlamadan önce emin olun:

- Windows Server 2008 R2 Service Pack 1 ve kullanılabilir tüm güncelleştirmeleri yüklenir.
- Etki alanına katılmış sunucusudur.
- Sunucunun bir statik IP adresi vardır.

> [!NOTE]
> Bilgisayarlar bir etki alanına katılma ile ilgili daha fazla bilgi için bkz: [katılan bilgisayarların etki alanı ve günlüğü üzerinde](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Statik IP adreslerini yapılandırma hakkında daha fazla bilgi için bkz: [bir statik IP adresi yapılandırın](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>WFF denetleyici sunucusu oluşturma

WFF denetleyici sunucusu oluşturmak için IIS 7 veya üstü ve WFF 2.0 veya sonraki sürümünü yüklemeniz gerekir. Perde arkasında WFF IIS Web Dağıtım Aracı (Web dağıtımı) kullanır, grubunuzdaki sunucuların eşitlenecek 2.x. WFF yüklemek için Web Platformu yükleyicisi kullanırsanız, yükleyici otomatik olarak indirin ve yükleyin, Web dağıtımı.

**WFF denetleyici sunucusu oluşturmak için**

1. İndirme ve yükleme [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9739157).
2. Üstündeki **Web Platformu yükleyicisi 3.0** penceresinde tıklatın **ürünleri**.
3. Gezinti bölmesinde, pencerenin sol tarafındaki tıklatın **Server**.
4. İçinde **IIS 7 önerilen Yapılandırması** satır, tıklatın **Ekle**.
5. İçinde <strong>Web Farm Framework 2.</strong> <em>x</em> satır, tıklatın <strong>Ekle</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. **Yükle**'ye tıklatın. Web Platformu yükleyicisi yükleme listesine Web dağıtım aracı, diğer çeşitli bağımlılıkları birlikte ekledi dikkat edin.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Lisans koşullarını gözden geçirin ve koşullarını kabul ederseniz tıklayın **kabul ediyorum**.
8. Yükleme tamamlandığında, tıklatın **son**ve ardından kapatın **Web Platformu yükleyicisi 3.0** penceresi.

## <a name="configure-the-primary-and-secondary-servers"></a>Birincil ve ikincil sunucularını yapılandırma

WFF sunucu grubu oluşturma önce çiftliğinin yapacak web sunucularındaki bazı hazırlık görevleri tamamlamanız gerekir:

- İzin vermek için güvenlik duvarı özel durumları ekleme **Çekirdek Ağ**, **Uzaktan Yönetim**, ve **dosya ve Yazıcı Paylaşımı** WFF denetleyici sunucusuyla iletişim kurmak için özellikleri .
- Bir etki alanı hesabı oluşturun (örneğin, **FABRIKAM\stagingfarm**) Active Directory'de ve her sunucuda yerel Yöneticiler grubuna ekleyin. Sunucu grubu oluşturduğunuzda, bu hesap sunucu grubu yönetici hesabı olarak kullanacaksınız.

Windows Güvenlik Duvarı'nda bu güvenlik duvarı özel durumlarını yapılandırma hakkında daha fazla bilgi için bkz: [sistem ve IIS 7 için Web grubu Framework 2.0 Platform gereksinimlerini](https://go.microsoft.com/?linkid=9805128). Diğer güvenlik duvarı sistemleri için ürün belgelerinize başvurun.

Windows Server 2008 R2'deki yerel administrators grubunun bir etki alanı hesabı eklemek için bir sonraki yordamı kullanabilirsiniz. Sunucu grubuna eklemek istediğiniz her sunucuda bu yordamı gerçekleştirmeniz gerekir&#x2014;diğer bir deyişle, birincil sunucuda ve her ikincil sunucuda yerel Yöneticiler grubuna aynı etki alanı hesabını ekleyin.

**Bir etki alanı hesabı yerel Yöneticiler grubuna eklemek için**

1. Üzerinde **Başlat** menüsündeki **Yönetimsel Araçlar**ve ardından **Sunucu Yöneticisi'ni**.
2. İçinde **Sunucu Yöneticisi'ni** penceresinde ağaç görünümü bölmesinde **yapılandırma**, genişletin **yerel kullanıcılar ve gruplar**ve ardından **grupları**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. İçinde **grupları** bölmesinde çift **Yöneticiler**.
4. İçinde **Yöneticiler özellikleri** iletişim kutusu, tıklatın **Ekle**.
5. İçinde **kullanıcıları, bilgisayarları, hizmet hesaplarını veya grupları** iletişim kutusunda, türü (veya Gözat), etki alanı hesabı için (örneğin, **FABRIKAM\stagingfarm**) ve ardından **Tamam**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. İçinde **Yöneticiler özellikleri** iletişim kutusu, tıklatın **Tamam**.

Sunucularınız için bir sunucu grubuna eklemek artık hazırsınız. Birincil sunucu söz konusu olduğunda, önce veya sunucu grubu oluşturduktan sonra uygulama gereksinimlerinizi karşılamak üzere sunucuyu yapılandırabilirsiniz&#x2014;her iki durumda da WFF sunucuları aynı ürünleri, bileşenleri veya yapılandırma dağıtarak eşitler İkincil sunucularınıza. Basitleştirmek amacıyla, Bu öğretici, sunucu grubu oluşturmayı bitirdiğinizde birincil sunucunun yapılandıracaksınız varsayar.

## <a name="create-the-wff-server-farm"></a>WFF sunucu grubu oluşturma

Bu noktada, tüm sunucularınız WFF sunucu grubuna eklemek hazırsınız:

- Denetleyici sunucusunda WFF yüklediniz.
- Birincil ve ikincil web sunucularınız üzerinde güvenlik duvarı özel durumlarını yapılandırdıktan.
- Bir etki alanı hesabı, birincil ve ikincil web sunucularında yerel administrators grubuna eklediniz.

Sonraki adım, WFF içinde sunucu grubu oluşturmaktır. IIS Yöneticisi'nden WFF denetleyici sunucusunda bunu yapabilirsiniz.

**WFF sunucu grubu oluşturmak için**

1. WFF denetleyici sunucusundaki üzerinde **Başlat** menüsündeki **Yönetimsel Araçlar**ve ardından **Internet Information Services (IIS) Yöneticisi'ni**.
2. İçinde **bağlantıları** bölmesinde yerel sunucu düğümünü genişletin, sağ **sunucu grupları**ve ardından **sunucu grubu oluşturma**.
3. İçinde **sunucu grubu oluşturma** iletişim kutusunda, sunucu grubu için anlamlı bir ad yazın (örneğin, **hazırlama grubu**) ve ardından **sağlama sunucu grubu**.
4. Kullanıcı adı ve her bir sunucuda yerel Yöneticiler grubuna eklenen etki alanı hesabının parolasını yazın.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. **İleri**'ye tıklayın.
6. Üzerinde **sunucuları Ekle** birincil sunucusu, select tam etki alanı adı (FQDN) yazın, sayfa **birincil sunucu**ve ardından **Ekle**.
7. Bu noktada, WFF sağladığınız kimlik bilgilerini kullanarak birincil sunucusu ile iletişim kuracaktır. Bağlantı başarılı olursa, birincil sunucunun tabloya ekleneceğini **sunucuları Ekle** sayfası.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Fark etmiş olabileceğiniz **sunucusu Yük Dengeleme için kullanılabilir** varsayılan olarak seçilidir. WFF IIS ARR modülü Yük Dengeleme uygulamak ve böylece sunucu grubunuzdaki web sunucuları arasında istekleri dağıtmak için kullanır. Çoğu senaryoda, yalnızca temizler **sunucusu Yük Dengeleme için kullanılabilir** bir üçüncü taraf yük dengeleme çözümü yerine kullanmak isterse seçeneği.
8. Üzerinde **sunucuları Ekle** sayfa, ilk ikincil sunucunuzun FQDN'sini yazın ve ardından **Ekle**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Grubunuzdaki tüm ek ikincil sunucular için 7. adımı yineleyin ve ardından **son**.

WFF sunucu grubunuzu artık çalışır durumda. Herhangi bir web platformu ürünlerini veya birincil sunucu ve web uygulamaları veya birincil sunucuya dağıttığınız içeriği Yükleme bileşenleri otomatik olarak tüm ikincil sunucularınız üzerinde sağlanacak.

WFF bir konudur geniş ve karmaşık ve üzerinde daha fazla bilgi edinebilirsiniz [IIS 7 için Microsoft Web grubu Framework 2.0](https://go.microsoft.com/?linkid=9805129) Web sitesi. Anda için ancak, farkında olmanız gereken iki özellikleri konu vardır:

- *Uygulama sağlama* sunucu grubundaki tüm ikincil sunucular üzerinden web uygulamaları ve yapılandırma ayarları gibi birincil sunucudan içerik çoğaltır işlemidir. Örneğin, birincil hazırlama sunucunuza Contact Manager örnek çözümü dağıtırsanız, WFF uygulama sağlama işlemini tüm ikincil hazırlama sunucularınız için bu çözümü dağıtır. Varsayılan olarak, her 30 saniyede uygulama sağlama işlemini çalıştırır.
- *Platform sağlama* web platformu ürünlerini ve sunucu grubundaki tüm ikincil sunucuları birincil sunucudan bileşenlerin eşitler işlemidir. ASP.NET MVC 3 birincil hazırlama sunucusunda yüklerseniz, örneğin, platform sağlama işlemini Web Platformu yükleyicisi ASP.NET MVC 3 tüm ikincil hazırlama sunucularınıza yüklemek için kullanır. Varsayılan olarak, platform sağlama işlemi beş dakikada bir çalışır.

Temel uygulama ve platform sağlama ayarları WFF denetleyicisi sunucunuzda IIS Yöneticisi'nden yönetebilirsiniz.

**Uygulama ve platform ayarları sağlama keşfedin**

1. IIS Yöneticisi ' nde, **bağlantıları** bölmesinde, sunucu grubunuzu seçin.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. İçinde **sunucu grubu** bölmesinde çift **uygulama sağlama**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Gördüğünüz gibi sunucu grubu şu anda web içerik ve yapılandırma ayarlarını birincil sunucunun ve ikincil sunucular arasında her 30 saniyede eşitlemek için yapılandırılır.
4. Tıklatın **geri**, çift tıklayın ve ardından **Platform sağlama**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Gördüğünüz gibi sunucu grubu şu anda web platformu ürünlerini ve bileşenleri birincil sunucunun ve ikincil sunucular arasında beş dakikada bir eşitlemek için yapılandırılır.
6. Tıklatın **geri**.
7. Web Platformu ürünlerini hemen eşitlemek için sunucu grubu zorlamak için **Eylemler** bölmesinde tıklatın **sağlama Platform**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Platform sağlama biraz zaman alabilir. Yükleyici işlemi arka planda server grubunuzdaki ikincil sunucularda çalışır.
8. Sağlama işlemini tamamlamak yeterli zaman izin verdiğiniz sonra ürünleri ve birincil sunucuya eklenen bileşenleri artık ikincil sunucularda çoğaltılmış olduğunu doğrulayabilirsiniz. Örneğin, bir ikincil sunucuya oturum açmak ve kullanmak **Sunucu Yöneticisi'ni** penceresinde web sunucusu rolü yüklü olduğunu doğrulayın.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Ayrıca, çeşitli web platformu bileşenlerini eklendiğini doğrulamak için yüklü programlar listesinde kontrol edebilirsiniz.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Yük Dengeleme yapılandırma

Bir web grubu oluşturduğunuzda, Yük Dengeleme, web sunucuları arasında HTTP isteklerini dağıtmak için çeşit ayarlamanız gerekir. Bu, Windows Server 2008 Ağ Yükü Dengeleme, IIS ARR ya da çözümü bir üçüncü taraf yazılım veya donanım tabanlı Yük Dengeleme olabilir.

WFF yakından IIS giderir ile tümleştirmek için tasarlanmıştır Bu tümleştirme avantajlarından yararlanmak için ARR modülü WFF denetleyicisi sunucusuna yüklemeniz gerekir. Ardından tüm web trafiği denetleyicisi sunucusuna genellikle etki alanı adı sistemi (DNS) kayıtları yapılandırarak doğrudan. Denetleyici sunucusu ardından gelen istekleri grubunuzdaki, sunucu kullanılabilirliği ve çeşitli diğer ölçütlere göre sunucular arasında dağıtır.

> [!NOTE]
> ARR ile WFF kullanmanız gerekmez; üçüncü taraf Yük Dengeleme çözümlerinde çalışmaya WFF yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Web grubu Framework 2.0 IIS 7 için genel bakış](https://go.microsoft.com/?linkid=9805126).


ARR kullanarak Yük Dengeleme en Bu öğretici kapsamında olduğu karmaşık bir konu ile ilgilidir. Ancak, ARR modülünü yüklemek ve yük dengelemesi ile başlamak için bir sonraki yordamı kullanabilirsiniz.

**Yük Dengeleme WFF denetleyici sunucusunda ayarlamak için**

1. WFF denetleyici sunucusunda Web Platformu Yükleyicisi'ni başlatın.
2. Üstündeki **Web Platformu yükleyicisi 3.0** penceresinde tıklatın **ürünleri**.
3. Gezinti bölmesinde, pencerenin sol tarafındaki tıklatın **Server**.
4. İçinde **uygulama isteği yönlendirme 2.5** satır, tıklatın **Ekle**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Tıklatın **yükleme**ve ardından yönergeleri izleyin **Web Platformu yükleme** penceresi.
6. Yükleme tamamlandığında, IIS Yöneticisi'ni başlatın ve **bağlantıları** bölmesinde, sunucu grubu düğümünü tıklatın. Birkaç yeni simgeleri için eklenene bildirimi **sunucu grubu** bölmesi.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. İçinde **sunucu grubu** bölmesinde çift **Yük Dengeleme**.
8. İçinde **Yük Dengeleme** bölmesinde, bir yük seçin Bakiye algoritması (örneğin, **az geçerli istek**).

    > [!NOTE]
    > Yük Dengeleme algoritmaları ve diğer yapılandırma ayarları hakkında daha fazla bilgi için bkz: [uygulama isteği yönlendirme Modülü](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. İçinde **Eylemler** bölmesinde tıklatın **Uygula**.

Temel Yük Dengeleme server grubunuzdaki sunucuları için artık yapılandırdınız. Tüm web grubu trafiğinizi denetleyicisi sunucusuna doğrudan, istekleri seçtiğiniz Yük Dengeleme algoritmasını ve kullanılabilirlik göre gruptaki sunucular arasında dağıtılır.

ARR ile yük dengelemeyi yapılandırma hakkında daha fazla bilgi için bkz: [uygulama isteği yönlendirme Modülü](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Sunucu grubu izleme

Herhangi bir zamanda IIS Yöneticisi'ni denetleyici sunucusundaki sunucusu grubunuzun durumunu izleyebilirsiniz. İçinde **bağlantıları** bölmesinde sunucu grubunuzu genişletin ve ardından **sunucuları**. Bölmeyi son etkinliğin izleme günlüğü ile birlikte gruptaki her sunucunun bir özetini gösterir.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Sonuç

WFF sunucu grubunuzu artık açık ve çalışıyor olması. Tercih ettiğiniz dağıtım yaklaşımı desteklemek için birincil sunucunun yapılandırabilirsiniz&#x2014;Ayrıntılar için daha fazla bilgi bölümüne bakın&#x2014;ve sunucu grubundaki her ikincil sunucudaki yapılandırmanızı çoğaltılır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tüm yönlerini yapılandırma ve WFF kullanma hakkında daha fazla yönergeler için bkz [IIS 7 için Microsoft Web grubu Framework 2.0](https://go.microsoft.com/?linkid=9805129) Web sitesi.

> [!div class="step-by-step"]
> [Önceki](configuring-a-database-server-for-web-deploy-publishing.md)
> [sonraki](configuring-deployment-properties-for-a-target-environment.md)
