---
uid: whitepapers/aspnet-and-iis6
title: IIS 6.0 ile ASP.NET 1.1 çalıştıran | Microsoft Docs
author: rick-anderson
description: Windows Server 2003, IIS 6.0 ve ASP.NET 1.1 içerir, ancak bu bileşenler varsayılan olarak devre dışıdır. Bu teknik, IIS 6.0 etkinleştirmeyi açıklar bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573339"
---
<a name="running-aspnet-11-with-iis-60"></a>IIS 6.0 ile çalışan ASP.NET 1.1
====================
> Windows Server 2003, IIS 6.0 ve ASP.NET 1.1 içerir, ancak bu bileşenler varsayılan olarak devre dışıdır. Bu teknik inceleme, IIS 6.0 ve ASP.NET 1.1 etkinleştirmeyi açıklar ve IIS ve ASP.NET en iyi performans almak için birkaç yapılandırma ayarlarını önerir.
> 
> ASP.NET 1.1 ve IIS 6.0 için geçerlidir.


ASP.NET 1.1, Windows Server Internet Information Server (IIS) sürüm 6.0, en son sürümünü de içeren 2003 ile birlikte gelir. IIS 6.0 ve ASP.NET 1.1 sorunsuz bir şekilde tümleştirmek için tasarlanmıştır ve ASP.NET Şimdi yeni IIS 6.0 çalışan işlem modeli varsayılan olarak ayarlar.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1.1 varsayılan olarak yüklü değil

Microsoft'un sunucu işletim sistemlerinin önceki sürümlerden farklı olarak, Internet Information Server (IIS) varsayılan olarak etkin değildir; veya ASP.NET 1.1 değil. IIS etkinleştirmek için iki seçenek vardır:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>IIS, seçeneği #1 - Sunucu Yapılandırma Sihirbazı etkinleştirme

Windows Server 2003 bir yeni ', sunucu yapılandırma sunucunuz istenen modunda düzgün şekilde yapılandırmanıza yardımcı olması için Sihirbazı' gelir.

-Sihirbazını başlatmak için oturum açmış olmanız gerekir bir yönetici olarak - Sihirbazı'nı çalıştırmak için Not gitmek için: Başlat | Programlar | Yönetim Araçları ve Seç 'sunucunuzu yapılandırın'.

Bir kez seçili ', Sunucu Yapılandırma Sihirbazı' Açılış ekranında görmeniz gerekir:

![](aspnet-and-iis6/_static/image1.jpg)

Tıklatın ' sonraki &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Tıklatın ' sonraki &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

Bu ekranda seçmeniz gerekir ' uygulama sunucusu (IIS, ASP.NET) olarak yapılandırmak için Seçenekler.

Tıklatın ' sonraki &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Sunucuyu uygulama sunucusu olarak yapılandırmak için seçtikten sonra bu ekranı ek yetenekler yüklenmelidir isteyen görüntülenir. Her iki seçenek varsayılan olarak seçilidir. ASP.NET otomatik olarak etkinleştirmek için seçmeniz gerekir ' ASP etkinleştirin. NET'.

Tıklatın ' sonraki &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Bu ekran, yüklenecek seçenekleri nelerdir görüntüler.

Tıklatın ' sonraki &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Seçtiğiniz seçeneklerini yüklenirken bu ekran görürsünüz. Diğer iletişim kutuları Hizmetleri yüklü olarak görünmesini görmek için normal bir durumdur. Ayrıca Windows Server 2003 CD'sini konumunu belirtmeniz istenebilir.

Tıklatın ' sonraki &gt;' tamamlandığında.

![](aspnet-and-iis6/_static/image7.jpg)

'Son' düğmesini tıklatın - Windows Server 2003 IIS 6.0 ve ASP.NET 1.1 desteklemek üzere yapılandırılmıştır.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>IIS, #2 - el ile IIS ve ASP.NET yapılandırma seçeneğini etkinleştirme

', Sunucu Yapılandırma Sihirbazı' kullanmak istemiyorsanız, isteğe bağlı olarak IIS 6.0 ve ASP.NET 'Ekle veya Kaldır' kullanarak 1.1 Denetim Masası'ndan yükleyebilirsiniz.

İlk Denetim Masası'nı açın:

![](aspnet-and-iis6/_static/image8.jpg)

Ardından, '/ Windows Bileşenlerini Ekle Kaldır 'Windows Bileşenleri Sihirbazı' açın üzerinde' tıklatın:

![](aspnet-and-iis6/_static/image9.jpg)

Vurgulayın ve 'Uygulama sunucusu' denetleyin ve sonra '?' Detaylar düğmesi:

![](aspnet-and-iis6/_static/image10.jpg)

ASP.NET yüklemek için kontrol ' ASP. NET'.

Windows Bileşen Sihirbazı dönmek için ' Tamam' ı tıklatın. Tıklatın ' sonraki &gt;' yüklemeye başlamak için Windows Bileşen Sihirbazı:

![](aspnet-and-iis6/_static/image11.jpg)

Diğer iletişim kutuları Hizmetleri yüklü olarak görünmesini görmek için normal bir durumdur. Ayrıca Windows Server 2003 CD'sini konumunu belirtmeniz istenebilir.

Yükleme tamamlandığında Windows Bileşen Sihirbazı'nın son ekranı görürsünüz:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 ve ASP.NET 1.1 şimdi yapılandırılır ve kullanılabilir.

## <a name="recommended-settings"></a>Önerilen ayarları

ASP.NET 1.1 IIS 6.0 ile çalışırken ASP.NET tarafından en iyi performansı elde etmek için önerilen çeşitli yapılandırma ayarları vardır:

- Yapılandırma çalışan işlem bellek sınırları
- Çalışan işlem geri dönüştürme yapılandırma

### <a name="configuring-worker-process-memory-limits"></a>Yapılandırma çalışan işlem bellek sınırları

Varsayılan olarak IIS 6.0 IIS kullanmasına izin verilen bellek miktarı sınırı ayarlamaz. ASP. Önbelleği önceden bellekten kullanılmayan öğeleri kaldırabilmeniz için NET'in önbellek özelliğinin bellek sınırlaması kullanır.

IIS 6.0 özelliğini geri dönüştürme belleği yapılandırma önerilir. Bu açık Internet Information Services Yöneticisi yapılandırmak için (Başlat | Programlar | Yönetimsel Araçlar | Internet Information Services). Açık bir kez 'Uygulama havuzları' klasörünü genişletin:

Her bir uygulama havuzu için:

![](aspnet-and-iis6/_static/image13.jpg)

1. Uygulama havuzunda, örneğin sağ tıklayın 'DefaultAppPool' ve 'Özellikler' seçin:

![](aspnet-and-iis6/_static/image14.jpg)

2. Ardından, bellek üzerinde tıklayarak geri dönüşümü Etkinleştir ' en çok kullanılan bellek (megabayt cinsinden):'. Değeri sunucuda fiziksel (sanal) bellek miktarından daha fazla olmamalıdır, yaklaşık yani 512 MB fiziksel bellek seçimi 310 sahip bir sunucu için fiziksel belleğin % 60. Ayrıca, en fazla 800 MB 2 GB adres alanı kullanırken aşmaması, önerilir. Sunucunun bellek adres alanı 3 GB ise, çalışan işlem maksimum bellek sınırını 1, 800 MB olarak yüksek olabilir:

![](aspnet-and-iis6/_static/image15.jpg)

'Apply' ve 'Tamam' özellikleri iletişim kutusundan çıkmak için tıklatın. Bu, tüm kullanılabilir uygulama havuzları için işlemi yineleyin.

### <a name="configuring-worker-recycling"></a>Geri dönüştürme çalışan yapılandırma

Varsayılan olarak, IIS 6.0, 29 saatte bir çalışan işlemi geri dönüştürmek için yapılandırılır. Bu ASP.NET çalışan bir uygulama için bir bit agresif ve otomatik çalışan işlem geri dönüşümü devre dışı bırakılır önerilir.

Otomatik çalışan işlem geri dönüşümü devre dışı bırakmak için önce Internet Information Services Yöneticisi açın (Başlat | Programlar | Yönetimsel Araçlar | Internet Information Services). Açık bir kez 'Uygulama havuzları' klasörünü genişletin:

![](aspnet-and-iis6/_static/image16.jpg)

Her bir uygulama havuzu için:

1. Uygulama havuzunda, örneğin sağ tıklayın 'DefaultAppPool' ve 'Özellikler' seçin:

![](aspnet-and-iis6/_static/image17.jpg)

2. İşaretini ' Geri Dönüşüm çalışan işlemi (dakika cinsinden):':

![](aspnet-and-iis6/_static/image18.jpg)

'Apply' ve 'Tamam' özellikleri iletişim kutusundan çıkmak için tıklatın. Bu, tüm kullanılabilir uygulama havuzları için işlemi yineleyin.

## <a name="granting-write-access-to-the-file-system"></a>Dosya sistemine yazma erişim izni verme

Uygulamanızın dosya sistemine yazma erişimi gerektirir ve NTFS kullanıyorsanız bir erişim denetimi listesi (ASP.NET erişim izni vermek için ACL) klasör veya dosya değiştirmeniz gerekir.

Örneğin, ASP.NET vermek için c:\inetpub\wwwroot yazma erişimi ilk Gezgini'ni açın ve dizine gidin:

![](aspnet-and-iis6/_static/image19.jpg)

Ardından, sağ tıklayın dizinde, örneğin 'wwwroot' ve Özellikler'i seçin. Özellikler iletişim kutusu açıldıktan sonra 'Security' sekmesini seçin:

![](aspnet-and-iis6/_static/image20.jpg)

Özel bir dizinde c:\inetpub\wwwroot\ dizindir özel IIS 6.0 grup ' IIS\_WPG' zaten okuma izni &amp; yürütme, klasör içeriğini listele ve Okuma izinleri. Ancak, yazma izni vermek için yazma işlemi için izin ver onay kutusuna tıklayın gerekir:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0, artık bu klasörde yazma izni vardır. Diğer klasörlerde, şu adımları - izleyin yazma izinleri vermek için Not, IIS eklemeniz gerekebilir\_zaten yoksa, WPG grubu.

> [!CAUTION]
> IIS için yazma izni verme\_WPG izin vermek bu dizine yazmak herhangi bir ASP.NET uygulama.

## <a name="supporting-integrated-authentication-with-sql-server"></a>SQL Server ile tümleşik kimlik doğrulaması destekleme

Tümleşik kimlik doğrulaması SQL Server oturum açma hesabı doğrulamak için SQL Server'ın Windows NT kimlik doğrulaması kullanmasına izin verir. Bu kullanıcının standart SQL Server oturum açma işlemini atlamanızı sağlar. Bu yaklaşımda, SQL Server Windows NT ağ güvenlik işleminden elde ettiği, parola bilgilerini ve kullanıcı için ayrı oturum açma kimliği veya parola girmeden bir SQL Server veritabanını bir ağ kullanıcısı erişebilir.

Kimlik bilgileri, uygulamanız için bağlantı dizenizi içinde hiç depolandığından ASP.NET uygulamaları için tümleşik kimlik doğrulaması seçme iyi bir seçimdir. Bunun yerine SQL ile bağlantı için kullanılan bağlantı dizesi şu şekilde görünür:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Bu bağlantı dizesi SQL Server SQL sunucusuna erişmeye çalışan uygulama Windows kimlik bilgilerini kullanacak şekilde söyler. ASP.NET/IIS 6 söz konusu olduğunda bu bir hesap IIS'de olacaktır\_WPG grubu.

SQL Server ve ASP.NET arasında tümleşik kimlik doğrulamasını etkinleştirmek için önce SQL Server tümleşik kimlik doğrulaması veya karma mod kimlik doğrulaması - için yapılandırıldığından emin olun gerekecektir bu belirlemek için DBA ile denetleyin. SQL Server bu iki moddan birini ise, tümleşik kimlik doğrulaması kullanabilirsiniz.

SQL Server Enterprise Manager'ı açın (Başlat | Programlar | Microsoft SQL Server | Kuruluş Yöneticisi), uygun sunucuyu seçin ve güvenlik klasörünü genişletin:

![](aspnet-and-iis6/_static/image22.jpg)

Varsa ' BUILTINT\IIS\_WPG' grubu listelenmiyorsa, üzerinde oturum açmalar ve 'Yeni oturum açma' seçin:

![](aspnet-and-iis6/_static/image23.jpg)

İçinde ' adı:' ya da girin textbox ' [sunucu/etki alanı adı] \IIS\_WPG' ya da Windows NT kullanıcı/Grup Seçici'yi açmak için üç nokta düğmesini tıklatın:

![](aspnet-and-iis6/_static/image24.jpg)

Geçerli makinenin IIS seçin\_WPG grubu tıklatın 'Add' ve seçiciyi kapatmak için Tamam.

Sonra da varsayılan veritabanı ve veritabanına erişmek için izinleri ayarlamanız gerekir. Ayarlamak için varsayılan veritabanı seçin açılır listeden, örneğin aşağıdaki Northwind seçilir:

![](aspnet-and-iis6/_static/image25.jpg)

Ardından, veritabanı erişimi sekmesinde tıklatın:

![](aspnet-and-iis6/_static/image26.jpg)

Erişmesine izin vermek istediğiniz her veritabanı için izin ver onay kutusuna tıklayın. De veritabanı rolleri seçmek veritabanı denetleniyor gerekir\_sahibi bulunduğundan emin olun, oturum açma yönetmek ve seçili veritabanını kullanmak için tüm gerekli izinler.

Özellik iletişim kutusundan çıkmak için Tamam'ı tıklatın. ASP.NET uygulamanızı tümleşik SQL Server kimlik doğrulamayı destekleyecek şekilde yapılandırılmıştır.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>ASP.NET 1.0 IIS 6.0 yerel modda çalıştırma

ASP.NET 1.0 IIS 6.0, yalnızca IIS 5 uyumluluk modunda desteklenir.

ASP.NET 1. 0'ı IIS 5.0 uyumluluk modunda çalışacak şekilde yapılandırmak için Internet Hizmetleri Yöneticisi'ni açın ve Web siteleri sağ tıklayın ve Özellikler'i seçin:

![](aspnet-and-iis6/_static/image27.jpg)

Hizmet sekmesine geçin ve denetleme? WWW hizmetini IIS 5.0 yalıtım modunda çalıştır?:

![](aspnet-and-iis6/_static/image28.jpg)
