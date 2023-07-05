# MongoDB, schema model, dotenv
Əvvəlki mövzuda frontend tərəfdən body və s göndərməyi öyrəndik. Misal istifadəçi qeydiyyatdan keçmək istəyir və öz məlumatlarını yazır. Frontend tərəf bu məlumatları bizə body daxilində göndərdi. Bəs biz bu məlumatları harda saxlayacıq.

---

Ümümiyyətlə backend iki hissədən ibarətdir. Server və Database.
Biz server tərəfdə request, response və s, data tranzaksiyalarını idarə edirik. Database tərəfdə isə məlumatlar saxlanılır. Misal bayaq dediyimiz istifadəçi məlumatları, elektron mağazada olan product məlumatları və s məlumatları database-də saxlayacıq. 

Database-ə misal olaraq - MySQL, MongoDB, MsSQL və s var.
Biz bunlardan **MongoDB** istifadə edəcik.
- Bunun üçün https://www.mongodb.com/ səhifəsinə daxil olub. Gmail ilə qeydiyyatdan keçin. 
- Yeni proyekt qurmaq üçün New Project -ə click edin.
- Proyekt adını yazın. Sonra next -ə click edin. 
- Sizdən başqa istifadəçi olmayacaqsa yenə next-ə click edin.
- Sonra Build Databe -ə click edib database qurun.
- Server parametrlərindən ödənişsiz olanı seçin
- Aşağıda öz istifadəçi adınızı yazın.
- Və parolu yazın. **!!! Parolu bir yerə mütləq qeyd edin.**
- Add my current IP Adress - click edin.
- Finish and close -a click edərək təsdiqləyin.

Artıq databse hazırdır. VS Code-dakı layihəmizlə birləşdirmək üçün terminalı açıb **npm i mongoose** yazıb yükləyək. Yükləndiyinə package.json faylından baxa bilərsiz.

 - MongoDB saytında Database-ə qoşulmaq üçün Connect-ə cllick edin.
- 1-ci step-də Drivers hissəsinə daxil olun.
- 2-ci step-də mongodb+srv tipində olan uzun linki kopyalayın

Və həmin linki aşağıdakı kodda connect hissəsində yerləşdirin.
```js
// index.js faylı
const  mongoose  =  require('mongoose')

mongoose
.connect("linki bura daxil edin")
.then(()  => console.log("MongoDB connected"))
.catch(()  => console.log("MongoDB not connected!!!"));
```
Kopyalayacağınız link təxminən bu tipdə olacaq. 
```
"mongodb+srv://<username>:<password>@databse.bnvfx58.mongodb.net/?retryWrites=true&w=majority"
```
Və siz linkdə < username > və < pasword > hissəsində öz istifadəçi adı və parolunuzu yazacaqsınız. 
Mən **username-> zahido** və **password->zahid12345** qoydum.
```
"mongodb+srv://zahido:zahid12345@databse.bnvfx58.mongodb.net/?retryWrites=true&w=majority"
```
Linkdə məlumatlarınızı yerləşdirdikdən sonra bu kodu əvvəlki layihədə olan kodumuz ilə birləşdirlək.
```js
// index.js faylı
const  express  =  require('express')
const  app  =  express()
  
const  mongoose  =  require('mongoose')
  
const  usersRoute  =  require('./routes/users')
const  productsRoute  =  require('./routes/products')
  
mongoose
	.connect("mongodb+srv://zahido:zahid12345@databse.bnvfx58.mongodb.net/?retryWrites=true&w=majority")
	.then(()  => console.log("MongoDB connected"))
	.catch(()  => console.log("MongoDB not connected!!!"));

app.use(express.json())
app.use('/users',usersRoute)
app.use('/products',productsRoute)

app.get('/',(req,res)=>{
res.json({ message:  "Z-> GET / metodu isledi"  });
})
  
app.listen(3000,()=>{console.log('Z-> Server is runnning PORT:3000')})
```

terminalda npm start etdikdə, **MongoDB connected ** gördünüzsə artıq qoşulubdur. Artıq rahatlıqla database-də məlumat saxlaya və orda əməliyyatlar apara bilərik.

Lakin qarışıq formada məlumatlar saxlaya bilmərik. Misal istifadəçilərin olduğunu bir cluster qururuqsa. Mütləq User schema( strükturu ) qurmalıyıq.
Bunun üçün models adında folder qurup daxilində User.js faylı quraq.
```js
const  mongoose  =  require("mongoose");
  
const  UserSchema  = mongoose.Schema({
	username: String,
	password: String
});
  
module.exports  = mongoose.model("User", UserSchema);
```
Yuxarıdakl kodda mongoose-u import etdik. Sonra schema( strükturu ) qurduq.
Schema-da User parametrlərini və datatype-larını qeyd etdik. Və User adında export edəcəyimizi yazdıq.
Yəni database-ə ilə işləyərkən User yazıb nöqtə qoyub əməliyyatlar edəcik.

Son olaraq bu hazırladığımız User modelini routes folderində olan users.js faylında yuxarıda import edək.
```js
const  router  =  require('express').Router()
const  User  =  require('../models/User')
 
//users routing
router.get('/',(req,res)=>{
	res.json({ message:  "Z-> GET /users metodu isledi"  });
})

router.get("/:id",  (req,  res)  =>  {
	res.json({ message:  "Z-> GET /users/:id metodu isledi"  });
});
  
router.post("/",  (req,  res)  =>  {
	res.json({ message:  "Z-> POST /users metodu isledi"  });
});
   
router.put("/",  (req,  res)  =>  {
	res.json({ message:  "Z-> PUT /users metodu isledi"  });
});
  
router.delete("/",  (req,  res)  =>  {
	res.json({ message:  "Z-> DELETE /users metodu isledi"  });
})
 
module.exports  = router
```

Artıq modeli users.js import etdiyimizə görə MongoDB saytına qayıdıb **Browse Collections**	-a click edib modelimizə baxaq. 
users modelini gördüksə deməli artıq MongoDB modelə qoşulub. 
Artıq biz yeni user əlavə etdikdə, sildikdə, düzəliş etdikdə həmin yerdən baxa bilərik.

## Add User ( POST )
Gəlin MongoDB -yə user əlavə edək.
User əlavə etmək üçün kodumuzu post metodunda yazacıq.
```js
// Add User
router.post("/",  async  (req,  res)  =>  {
	try{
		const  newProduct  =  await  new  User({
			username: req.body.username,
			password: req.body.password,
		});
		const  saved  =  await newProduct.save()
		res.status(200).json({ message:  "Product added Successfully"  });
	}
	catch(err){
		res.status(400).json({message:  "No product added"})
	}
});
```
Yuxarıdakı kodumuz özünün əvvəlki kodundan asılıdır deyə funksiyamız **async** ( asinxron ) olacaq. Və biz newProduct adında bir obyekti **User** schema( strukturunda) qurub, body-dən gələn məlumatlarla birləşdirdik. Və bu newProduct obyektini  **.save( )** edərək database-ə əlavə etdik.


Gəlin POSTMAN vasitəsi ilə /users linkinə POST metod ilə body göndərək.
Body göndərmək əvvəlki mövzuda izah olunub.

Send etdikdən sonra "Product added Successfully" mesajı gəldisə artıq user əlavə olunub. MongoDB-də Browse Collections click edib users hissəsindən baxa bilərsiz.
Bu formada 2 3 user əlavə edin.

## Get Users ( GET )

İndi isə əlavə etdiyimiz user-ləri çağırmaq üçün GET metodu yazaq.
```js
//Get Users
router.get('/',async(req,res)=>{
try{
	const  users  =  await User.find()
	res.status(200).json(users);
}
catch(err){
	res.status(404).json({message:  "Users not found"})
}
})
```

**User.find( )** metodu ilə Databse-də ola **bütün user-ləri çağırdıq** və users dəyişəninə bərabər etdik. Response olaraq users qaytardıq. Əgər bir xəta baş versə xəbərdarlıq mesajı göndərildi.

## Get User By id (GET /:id)

Artıq bütün istifadəçiləri çağıra bilirik. Bəs istifadəçilər arasından yalnız bir istifadəçini çağırmaq istəsək? 

Database-də müəyyən bir xassəyə görə axtarış etdikdə problemlər yarana bilər. Misal biz adlarına görə axtarış veririk və 2 3 nəfərdə eyni addır. Belə halların olmaması üçün bütün obyektlərə MongoDB avtomatik id xassəsi verir.  Və hər bir istifadəçinin **id-si bir-birindən fərqlidir.**. Ona görə axtarış edərkən biz id-sinə görə axtaracıq.

id-ni front tərəfdən body olaraq göndərib axtarmaq da olar. Lakin daha qısa olaraq id linkdə göndərilir.
Misal
```
Bütün istifadəçilər
http://localhost:3000/users

id-si 123 olan istifadçi
http://localhost:3000/users/123

id-si a2b3 olan istifadçi
http://localhost:3000/users/a2b3
```
Front tərəfdən göndərilən linkdəki id-ni server tərəfimizdə **req.params( )** ilə götürəcik. Daha ətraflı əvvəlki mövzuda izah olunub.

```js
//Get User By id
router.get("/:id",  async(req,  res)  =>  {
	try{
		const  user  =  await User.findById(req.params.id)
		res.status(200).json(user);
	}
	catch(err){
		res.status(404).json({message:  "User not found"})
	}
})
```

Artıq database-də olan istifadəçilərdən birinin id-sini götürüb. Həmin id-yə uyğun yoxlanış edə bilərik.

Misal **id-si 64a2bcebd5b164155fb340f8** olan istifadəçini tapmaq istəyiriksə POSTMAN-da GET metodunu seçib link hissəsində bu formada yazırıq.
 ```
http://localhost:3000/users**/64a2bcebd5b164155fb340f8
 ```
 
 ## Delete User By id ( DELETE /:id )
 
İndi isə müəyyən bir istifadəçini id-sinə görə server-dən silməyə çalışaq.
Bunun üçün **User.findByIdAndRemove( )** metodundan istifadə edəcik
```js
//Delete User By id
router.delete("/:id",  async(req,  res)  =>  {
	try{
		const  user  =  await User.findByIdAndRemove(req.params.id)
		res.status(200).json(users);
	}
	catch(err){
		res.status(404).json({message:  "User not found"})
	}
})
```
Bunu da yoxlamaq üçün database-də olan userlərdən birinin id-sini götürüb POSTMAN-da DELETE metodunu silib linkdə həmin id-yə uyğun request göndərin. GetById-dəki kimi


 


