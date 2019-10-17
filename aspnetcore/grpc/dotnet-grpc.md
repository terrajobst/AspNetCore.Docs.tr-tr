---
title: dotnet-grpc ile Protobuf başvurularını yönetme
author: juntaoluo
description: DotNet-GRPC küresel aracıyla prototip başvuruları ekleme, güncelleştirme, kaldırma ve listeleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: 994597c854a95bb33de1686ab025cb3744cf6845
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519041"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a>dotnet-grpc ile Protobuf başvurularını yönetme

[John Luo](https://github.com/juntaoluo) tarafından

`dotnet-grpc`, bir .NET gRPC projesindeki [protodeğer ( *. proto*)](xref:grpc/basics#proto-file) başvurularını yönetmek için .NET Core küresel bir araçtır. Araç, prototipleme başvurularını eklemek, yenilemek, kaldırmak ve listelemek için kullanılabilir.

## <a name="installation"></a>Yükleme

@No__t-0 [.NET Core küresel aracı](/dotnet/core/tools/global-tools)'nı yüklemek için şu komutu çalıştırın:

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a>Başvuru Ekle

`dotnet-grpc`, *. csproj* dosyasına `<Protobuf />` öğe olarak prototip başvuruları eklemek için kullanılabilir:

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

Prototip başvuruları, C# istemci ve/veya sunucu varlıklarını oluşturmak için kullanılır. @No__t-0 aracı şunları yapabilir:

* Diskteki yerel dosyalardan Prototipsiz başvuru oluştur.
* Bir URL ile belirtilen uzak dosyadan Protobir başvuru oluştur.
* Projeye doğru gRPC paket bağımlılıklarının eklendiğinden emin olun.

Örneğin, `Grpc.AspNetCore` paketi bir Web uygulamasına eklenir. `Grpc.AspNetCore` gRPC sunucusu ve istemci kitaplıklarını ve araç desteğini içerir. Alternatif olarak, yalnızca gRPC istemci kitaplıklarını ve araç desteğini içeren `Grpc.Net.Client` `Grpc.Tools` ve `Google.Protobuf` paketleri konsol uygulamasına eklenir.

### <a name="add-file"></a>Dosya Ekle

@No__t-0 komutu, disk üzerindeki yerel dosyaları prototip başvuruları olarak eklemek için kullanılır. Belirtilen dosya yolları:

* Geçerli dizin veya mutlak yollarla göreli olabilir.
* , [Glob](https://wikipedia.org/wiki/Glob_(programming))model tabanlı dosya için joker karakterler içerebilir.

Herhangi bir dosya proje dizininin dışındaysa, dosyayı Visual Studio 'da `Protos` klasörü altında göstermek için `Link` öğesi eklenir.

### <a name="usage"></a>Kullanım

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a>Arguments

| Bağımsız Değişken | Açıklama |
|-|-|
| dosyaları | Prototip dosyası başvuruları. Bunlar yerel prototipli dosyalar için glob 'nin bir yolu olabilir. |

#### <a name="options"></a>Seçenekler

| Short seçeneği | Long seçeneği | Açıklama |
|-|-|-|
| -p | --Proje | Üzerinde çalışılacak proje dosyasının yolu. Bir dosya belirtilmemişse, komut geçerli dizinde bir arama yapar.
| -s | --Hizmetler | Oluşturulması gereken gRPC Hizmetleri türü. @No__t-0 belirtilirse, Web projeleri için `Both` kullanılır ve Web dışı projeler için `Client` kullanılır. Kabul edilen değerler `Both`, `Client`, `Default`, `None`, `Server` ' tir.
| -i | --ek-içeri aktarma-Dizin | Prototip dosyaları için içeri aktarmalar çözümlenirken kullanılacak ek dizinler. Bu, yolların noktalı virgülle ayrılmış listesidir.
| | --erişim | Oluşturulan C# sınıflar için kullanılacak erişim değiştiricisi. Varsayılan değer `Public` şeklindedir. Kabul edilen değerler `Internal` ve `Public` ' dir.

### <a name="add-url"></a>URL Ekle

@No__t-0 komutu, kaynak URL tarafından belirtilen bir uzak dosyayı prototipde başvuru olarak eklemek için kullanılır. Uzak dosyanın nereye indirileceği belirtmek için bir dosya yolu belirtilmelidir. Dosya yolu, geçerli dizin veya mutlak bir yol ile ilişkili olabilir. Dosya yolu proje dizininin dışındaysa, Visual Studio 'da `Protos` sanal klasörü altında dosyayı göstermek için bir `Link` öğesi eklenir.

### <a name="usage"></a>Kullanım

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a>Arguments

| Bağımsız Değişken | Açıklama |
|-|-|
| 'deki | Uzak protoarabellek dosyasının URL 'SI. |

#### <a name="options"></a>Seçenekler

| Short seçeneği | Long seçeneği | Açıklama |
|-|-|-|
| -o | --çıkış | Uzak protoarabellek dosyası için indirme yolunu belirtir. Bu gerekli bir seçenektir.
| -p | --Proje | Üzerinde çalışılacak proje dosyasının yolu. Bir dosya belirtilmemişse, komut geçerli dizinde bir arama yapar.
| -s | --Hizmetler | Oluşturulması gereken gRPC Hizmetleri türü. @No__t-0 belirtilirse, Web projeleri için `Both` kullanılır ve Web dışı projeler için `Client` kullanılır. Kabul edilen değerler `Both`, `Client`, `Default`, `None`, `Server` ' tir.
| -i | --ek-içeri aktarma-Dizin | Prototip dosyaları için içeri aktarmalar çözümlenirken kullanılacak ek dizinler. Bu, yolların noktalı virgülle ayrılmış listesidir.
| | --erişim | Oluşturulan C# sınıflar için kullanılacak erişim değiştiricisi. Varsayılan değer `Public` ' dır. Kabul edilen değerler `Internal` ve `Public` ' dir.

## <a name="remove"></a>Kaldır

@No__t-0 komutu, *. csproj* dosyasından prototipsiz başvuruları kaldırmak için kullanılır. Komut bağımsız değişken olarak yol bağımsız değişkenlerini ve kaynak URL 'Lerini kabul eder. Araç:

* Yalnızca Prototipsiz başvuruyu kaldırır.
* , İlk olarak uzak bir URL 'den indirilse bile, *. proto* dosyasını silmez.

### <a name="usage"></a>Kullanım

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a>Arguments

| Bağımsız Değişken | Açıklama |
|-|-|
| başvurular | Kaldırılacak prototip başvurularının URL 'Leri veya dosya yolları. |

### <a name="options"></a>Seçenekler

| Short seçeneği | Long seçeneği | Açıklama |
|-|-|-|
| -p | --Proje | Üzerinde çalışılacak proje dosyasının yolu. Bir dosya belirtilmemişse, komut geçerli dizinde bir arama yapar.

## <a name="refresh"></a>Yenile

@No__t-0 komutu, kaynak URL 'den en son içerikle uzak bir başvuruyu güncelleştirmek için kullanılır. Yalnızca indirme dosyası yolu ve kaynak URL 'SI, güncellenmek üzere olan başvuruyu belirtmek için kullanılabilir. Not:

* Dosya içeriğinin karmaları yerel dosyanın güncelleştirilip güncelleştirilmediğini belirleme ile karşılaştırılır.
* Zaman damgası bilgisi karşılaştırılmaz.

Bir güncelleştirme gerekiyorsa araç her zaman yerel dosyayı uzak dosya ile değiştirir.

### <a name="usage"></a>Kullanım

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a>Arguments

| Bağımsız Değişken | Açıklama |
|-|-|
| başvurular | Güncellenmesi gereken uzak prototip başvurularına yönelik URL veya dosya yolları. Tüm Uzak başvuruları yenilemek için bu bağımsız değişkeni boş bırakın. |

### <a name="options"></a>Seçenekler

| Short seçeneği | Long seçeneği | Açıklama |
|-|-|-|
| -p | --Proje | Üzerinde çalışılacak proje dosyasının yolu. Bir dosya belirtilmemişse, komut geçerli dizinde bir arama yapar.
| | --Kuru-Çalıştır | Herhangi bir yeni içerik indirilmeden güncelleştirilenecek dosyaların listesini verir.

## <a name="list"></a>List

@No__t-0 komutu, proje dosyasındaki tüm Prototipsiz başvuruları göstermek için kullanılır. Bir sütunun tüm değerleri varsayılan değerler ise, sütun atlanabilir.

### <a name="usage"></a>Kullanım

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a>Seçenekler

| Short seçeneği | Long seçeneği | Açıklama |
|-|-|-|
| -p | --Proje | Üzerinde çalışılacak proje dosyasının yolu. Bir dosya belirtilmemişse, komut geçerli dizinde bir arama yapar.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
