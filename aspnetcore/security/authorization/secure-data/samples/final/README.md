# <a name="how-to-buildrun-secure-user-data-sample"></a>Nasıl yapı/güvenli kullanıcı veri örneği çalıştırma

* Parola Yöneticisi Aracı ile parola ayarlayın:

  `dotnet user-secrets set SeedUserPW <pw>`

* Veritabanını güncelleştirme:

    `dotnet ef database update`
