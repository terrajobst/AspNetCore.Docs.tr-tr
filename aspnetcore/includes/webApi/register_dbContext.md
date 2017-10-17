## <a name="register-the-database-context"></a>Veritabanı bağlamı kaydetme

Veritabanı bağlamı denetleyicisi olarak eklenmeye için ile kaydetmek ihtiyacımız [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. Veritabanı bağlamı için yerleşik destek kullanarak hizmet kapsayıcısı kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection). Değiştir *haline* aşağıdaki dosyasıyla:

[!code-csharp[Ana](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Önceki kod:

* Biz değil kullanmakta olduğunuz kodunu kaldırır.
* Hizmet kapsayıcıya bir bellek içi veritabanına eklenen belirtir.
