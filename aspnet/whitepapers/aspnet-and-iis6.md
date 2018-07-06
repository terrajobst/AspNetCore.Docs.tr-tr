---
uid: whitepapers/aspnet-and-iis6
title: ASP.NET 1.1 IIS 6.0 ile çalışan | Microsoft Docs
author: rick-anderson
description: Bu bileşenler, Windows Server 2003 hem IIS 6.0 hem de ASP.NET 1.1 içerse de, varsayılan olarak devre dışıdır. Bu teknik incelemede IIS 6.0 etkinleştirmeyi açıklar bir...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 338059104f46a6c5517212db6e1a54c12677e133
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839300"
---
<a name="running-aspnet-11-with-iis-60"></a>IIS 6.0 ile çalışan ASP.NET 1.1
====================
> Bu bileşenler, Windows Server 2003 hem IIS 6.0 hem de ASP.NET 1.1 içerse de, varsayılan olarak devre dışıdır. Bu teknik incelemede IIS 6.0 ve ASP.NET 1.1 etkinleştirmeyi açıklar ve IIS ve ASP.NET en iyi performans almak için çeşitli yapılandırma ayarlarını önerir.
> 
> 1.1 ASP.NET ve IIS 6.0 için geçerlidir.


ASP.NET 1.1, Windows Server Internet Information Server (IIS) sürüm 6.0, en son sürümünü içeren 2003 ile birlikte gelir. IIS 6.0 ve ASP.NET 1.1 sorunsuz bir şekilde tümleştirmek için tasarlanmıştır ve artık varsayılan olarak yeni IIS 6.0 çalışan işlem modeli ASP.NET.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1.1 varsayılan olarak yüklü değil

Microsoft'un sunucu işletim sistemlerinin önceki sürümlerden farklı olarak, Internet Information Server (IIS) varsayılan olarak etkin değildir; ya da ASP.NET 1.1 silinmez. IIS etkinleştirmek için iki seçenek vardır:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Seçenek #1 - IIS Sunucu Yapılandırma Sihirbazı etkinleştirme

Windows Server 2003, bir yeni 'sunucu yapılandırma sunucunuzu istenen modunda düzgün şekilde yapılandırmanıza yardımcı olması için Sihirbazı' birlikte gelir.

Unutmayın, oturum açmış olmanız gerekir yönetici olarak - sihirbazını çalıştırma - sihirbazını başlatmak için Git: Başlangıç | Programlar | Yönetim Araçları ve Seç 'sunucunuzu'.

Bir kez seçili 'Sunucu Yapılandırma Sihirbazı' açılış ekranı görürsünüz:

![](aspnet-and-iis6/_static/image1.jpg)

Tıklatın ' sonraki &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Tıklatın ' sonraki &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

Bu ekranda seçmeniz gerekecek ' yapılandırma seçenekleri olarak uygulama sunucusu (IIS, ASP.NET).

Tıklatın ' sonraki &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Sunucuyu, uygulama sunucusu olarak yapılandırma seçtikten sonra bu ekran ek yetenekler yüklenmelidir isteyen görüntülenir. Her iki seçeneği varsayılan olarak seçilidir. ASP.NET otomatik olarak etkinleştirmek için seçmeniz gerekir ' ASP etkinleştirin. NET'.

Tıklatın ' sonraki &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Bu ekran, yüklenecek seçenekleri nelerdir görüntüler.

Tıklatın ' sonraki &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Seçtiğiniz seçenekler yüklenirken şu ekranı görürsünüz. Diğer iletişim kutuları Hizmetleri yüklü olarak görünür görmek için normal bir durumdur. Ayrıca Windows 2003 Server yükleme CD'sinde konumunu istenebilir.

Tıklatın ' sonraki &gt;' tamamlandığında.

![](aspnet-and-iis6/_static/image7.jpg)

'Son' düğmesini tıklatın,-Windows Server 2003 IIS 6.0 ve ASP.NET 1.1 desteklemek için artık yapılandırılmıştır.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>IIS, #2 - el ile IIS ve ASP.NET yapılandırma seçeneğini etkinleştirme

'Sunucu Yapılandırma Sihirbazı' kullanmak istemiyorsanız, isteğe bağlı olarak IIS 6.0 ve ASP.NET 1.1 'Program Ekle veya Kaldır' kullanarak Denetim Masası'ndan yükleyebilirsiniz.

İlk kullanarak denetim masasını açın:

![](aspnet-and-iis6/_static/image8.jpg)

Ardından, ' Ekle/Kaldır Windows 'Windows Bileşenleri Sihirbazı' açılır bileşenleri üzerinde' tıklayın:

![](aspnet-and-iis6/_static/image9.jpg)

Vurgulayın ve 'Uygulama sunucusu' denetleyin ve 'Ayrıntılar?' ı Düğme:

![](aspnet-and-iis6/_static/image10.jpg)

ASP.NET yüklemek için kontrol ' ASP. NET'.

Windows Bileşen Sihirbazı için ' Tamam' ı tıklatın. Tıklatın ' sonraki &gt;' yüklemeye başlamak için Windows Bileşen Sihirbazı:

![](aspnet-and-iis6/_static/image11.jpg)

Diğer iletişim kutuları Hizmetleri yüklü olarak görünür görmek için normal bir durumdur. Ayrıca Windows 2003 Server yükleme CD'sinde konumunu istenebilir.

Yükleme tamamlandığında Windows Bileşen Sihirbazı'nın son ekranı görürsünüz:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 ve ASP.NET 1.1 artık yapılandırılmış ve kullanılabilir.

## <a name="recommended-settings"></a>Önerilen ayarlar

ASP.NET 1.1 IIS 6.0 ile çalışan ASP.NET tarafından en iyi performans almak için önerilen bazı yapılandırma ayarları vardır:

- Yapılandırma çalışan işlem bellek sınırları
- Çalışan işlemi geri dönüşümü yapılandırma

### <a name="configuring-worker-process-memory-limits"></a>Yapılandırma çalışan işlem bellek sınırları

Varsayılan olarak IIS 6.0 IIS kullanmasına izin verilen bellek miktarı sınırı ayarlamaz. ASP. Önbellek, proaktif olarak kullanılmayan öğeleri bellekten kaldırabilmeniz için NET önbellek özelliği sınırlaması bellek kullanır.

Bu, IIS 6.0 özelliğini geri dönüştürme bellek yapılandırmanız önerilir. Bu açık Internet Information Services Manager'ı yapılandırmak için (Başlangıç | Programlar | Yönetimsel Araçlar | Internet Information Services). Açık bir kez 'Uygulama havuzları' klasörünü genişletin:

Her bir uygulama havuzu için:

![](aspnet-and-iis6/_static/image13.jpg)

1. Örneğin uygulama havuzunda sağ tıklayın 'DefaultAppPool' ve 'Özellikler' i seçin:

![](aspnet-and-iis6/_static/image14.jpg)

2. Ardından, üzerinde tıklayarak bellek geri dönüşümü Etkinleştir ' en çok kullanılan bellek (megabayt cinsinden):'. Birden çok sunucu (sanal olmayan) fiziksel bellek miktarı değeri olmamalıdır, iyi bir yaklaşık % 60 ' 512 MB fiziksel bellek seçin 310 ile bir sunucu için başka bir deyişle, fiziksel bellek. Ayrıca, en fazla 800 MB 2 GB adres alanı kullanırken aşmaması, önerilir. Sunucunun bellek adresi alanının 3 GB ise, çalışan işlem için maksimum bellek sınırını 1 800 MB olarak yüksek olabilir:

![](aspnet-and-iis6/_static/image15.jpg)

Özellikler iletişim kutusundan çıkmak için 'Uygula' ve 'Tamam' seçeneğine tıklayın. Bu, tüm kullanılabilir uygulama havuzları için yineleyin.

### <a name="configuring-worker-recycling"></a>Geri Dönüşüm çalışan yapılandırma

Varsayılan olarak, IIS 6.0, 29 saatte bir çalışan işlemi geri dönüştürmek için yapılandırılır. ASP.NET çalışan bir uygulama için biraz agresif budur ve otomatik çalışan işlem geri dönüşümü devre dışı bırakıldı önerilir.

Otomatik çalışan işlem geri dönüşümü devre dışı bırakmak için önce Internet Information Services Manager açın (Başlat | Programlar | Yönetimsel Araçlar | Internet Information Services). Açık bir kez 'Uygulama havuzları' klasörünü genişletin:

![](aspnet-and-iis6/_static/image16.jpg)

Her bir uygulama havuzu için:

1. Örneğin uygulama havuzunda sağ tıklayın 'DefaultAppPool' ve 'Özellikler' i seçin:

![](aspnet-and-iis6/_static/image17.jpg)

2. Onay kutusunu temizleyin ' Geri Dönüşüm çalışan işlemi (dakika):':

![](aspnet-and-iis6/_static/image18.jpg)

Özellikler iletişim kutusundan çıkmak için 'Uygula' ve 'Tamam' seçeneğine tıklayın. Bu, tüm kullanılabilir uygulama havuzları için yineleyin.

## <a name="granting-write-access-to-the-file-system"></a>Dosya sistemine yazma erişim izni verme

Uygulamanızın gerektirdiği dosya sistemine yazma erişimi ve NTFS kullanıyorsanız, bir erişim denetimi listesi (ASP.NET erişim izni vermek için ACL) klasör veya dosya çubuğunda değiştirmeniz gerekir.

Örneğin, ASP.NET vermek c:\inetpub\wwwroot yazma erişimi ilk Gezgini'ni açın ve dizine gidin:

![](aspnet-and-iis6/_static/image19.jpg)

Ardından, sağ tıklayarak dizine, örneğin 'wwwroot' ve Özellikler'i seçin. Özellikler iletişim kutusu açıldıktan sonra 'Güvenlik' sekmesinde'ı seçin:

![](aspnet-and-iis6/_static/image20.jpg)

C:\inetpub\wwwroot\ dizindir, özel bir dizinde özel IIS 6.0 grup ' IIS\_WPG' okuma zaten verilmiş &amp; yürütme, klasör içeriklerini listeleme ve Okuma izinleri. Ancak, yazma izni vermek için yazmaya izin ver onay kutusuna tıklayın gerekir:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0, artık bu klasörde yazma izni vardır. Şu adımları - izleyin diğer klasörler üzerinde yazma izinleri vermek için Not: IIS eklemeniz gerekebilir\_zaten yoksa, WPG grubu.

> [!CAUTION]
> IIS için yazma izni verme\_WPG bu dizine yazabilmek herhangi bir ASP.NET uygulama sağlayacaktır.

## <a name="supporting-integrated-authentication-with-sql-server"></a>SQL sunucusu ile tümleşik kimlik doğrulamasını destekleme

Tümleşik kimlik doğrulaması, SQL Server oturum açma hesapları doğrulamak için SQL Server'ın Windows NT kimlik doğrulaması yararlanmasını sağlar. Bu kullanıcının standart SQL Server oturum açma işlemi atlamasını sağlar. Bu yaklaşımda, bir ağ kullanıcı bir SQL Server veritabanını SQL Server Windows NT ağ güvenlik işleminden elde ettiği, parola bilgisi ve kullanıcı için ayrı bir oturum açma kimliği veya parola sağlamadan erişebilir.

Kimlik bilgisi bağlantı dizenizi uygulamanızın içinde hiç olmadığı kadar depolandığından ASP.NET uygulamaları için tümleşik kimlik doğrulaması'nı seçerek iyi bir seçimdir. Bunun yerine SQL'e bağlanmak için kullanılan bağlantı dizesi şu şekilde görünür:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Bu bağlantı dizesi, SQL Server'ın SQL Server erişmeye çalışan uygulamanın Windows kimlik bilgilerini söyler. ASP.NET/IIS 6 söz konusu olduğunda bu bir hesap IIS'de olacaktır\_WPG grubu.

SQL Server ve ASP.NET arasında tümleşik kimlik doğrulamasını etkinleştirmek için önce SQL Server tümleşik kimlik doğrulaması veya karma mod kimlik doğrulaması - için yapılandırılmış olduğundan emin olun gerekecektir bunu belirlemek için DBA ile denetleyin. SQL Server, bu iki moddan birini ise, tümleşik kimlik doğrulaması kullanabilirsiniz.

SQL Server Enterprise Manager'ı açın (Başlat | Programlar | Microsoft SQL Server | Kurumsal Yöneticisi), uygun sunucuyu seçin ve güvenlik klasörü genişletin:

![](aspnet-and-iis6/_static/image22.jpg)

Varsa ' BUILTINT\IIS\_WPG' grup listelenmiyorsa, oturum açma bilgilerini sağ tıklayın ve 'Yeni oturum açma' seçin:

![](aspnet-and-iis6/_static/image23.jpg)

İçinde ' adı:' metin girin ya da ' [sunucu/etki alanı adı] \IIS\_WPG' ya da Windows NT kullanıcı/Grup Seçici'yi açmak için üç nokta düğmesine tıklayın:

![](aspnet-and-iis6/_static/image24.jpg)

Geçerli makinenin IIS seçin\_WPG grubu tıklatın 'Add' ve seçiciyi kapatmak için Tamam.

Ardından varsayılan veritabanı ve veritabanına erişmek için izinleri ayrıca ayarlamanız gerekir. Ayarlamak için varsayılan veritabanını seçin ve açılan listesinde, örneğin aşağıdaki Northwind seçilir:

![](aspnet-and-iis6/_static/image25.jpg)

Ardından, veritabanı erişimi sekmesine tıklayın:

![](aspnet-and-iis6/_static/image26.jpg)

Erişime izin vermek istediğiniz her veritabanı için izin ver onay kutusuna tıklayın. Ayrıca veritabanı rolleri seçmek db denetimi ihtiyacınız olacak\_sahibi, oturum açma bilgilerinizi yönetmek ve seçili veritabanını kullanmak için tüm gerekli izinlere sahip olmadığını olun.

Özellik iletişim kutusundan çıkmak için Tamam'a tıklayın. ASP.NET uygulamanızı, tümleşik SQL Server kimlik doğrulamasını desteklemek için artık yapılandırılmıştır.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>ASP.NET 1.0 IIS 6.0 yerel modda çalıştırma

ASP.NET 1.0 IIS 6.0, yalnızca IIS 5 uyumluluk modunda desteklenir.

ASP.NET IIS 5.0 uyumluluk modunda çalışacak şekilde 1.0 yapılandırmak için Internet Hizmetleri Yöneticisi'ni açın ve Web siteleri sağ tıklayın ve Özellikler'i seçin:

![](aspnet-and-iis6/_static/image27.jpg)

Hizmet sekmesine gidin ve kontrol? WWW hizmetini IIS 5.0 yalıtım modunda çalıştır?:

![](aspnet-and-iis6/_static/image28.jpg)
