# # request and response properties

Əvvəlki mövzularda metodlar parametr olaraq ( req, res ) alırdılar. Bu mövzuda bunlar haqqında daha ətraflı öyrənəcik. Request-lərin xüsusiyyətləri ilə başlayaq.

## req.body( )
Ümümiyyətlə frontend-dən backend-ə məlumat iki cür göndərilir. 
JSON və urlencoded formasında. 
Frontend tərəf 
- **JSON** formasında olan məlumatı **body** daxilində,
- **urlencoded** formasında olan məlumatı link daxilində göndərir.

Əvvəlki mövzudakı kodlardan davam edək. Sonuncu dəfə users və products modellərinin hərəsinə 4 metod( get, post, put, delete) yazmışdıq.
Lakin realda post metodu məlumat əlavə etmək üçündür. frontend tərəd bizə request göndərəndə bayaq dedik ki, JSON formatında olan datanı body daxilində göndərir.
Bunun  üçün birinci index.js faylında JSON datanı parse etmək üçün  app.use kodlarının yuxarısında bu kodu yazaq.
```js
//index.js faylı
app.use(express.json())
```
Sonra isə users.js faylında POST metodumuzda request-dən gələn body-ni götürüb console-a çıxardaq.
```js
//users.js faylı
router.post("/",  (req,  res)  =>  {
	console.log(req.body)
	res.json({ message:  "Z-> POST /users metodu isledi"  });
});
```
Backend kodumuz hazırdır. Gəlin POSTMAN proqramı ilə /users linkinə POST request edək. Amma bu dəfə həm də body göndərək. Metodu POST, linki /users etdikdən sonra Body göndərmək üçün linkin aşağısında olan seçimlərdən body-ə click edin. Sonra onun da aşağısında gələn seçimlərdən raw-a click edin. Ona da click etdikdən sonra sağda Text adında drobdown seçim qutusundan JSON seçin.
Artıq body göndərməyə hazırıq. Aşağıda body göndərmək üçün olan hissədə data göndərək.
```json
{
	"name": "Zahid",
	"surname": "Vahabzade"
}
```
POSTMAN-da request göndərməzdən əvvəl serverin terminal-da aktiv olub-olmadığını yoxlayın. Aktiv deyilsə **npm start** yazın.
Send-ə click etdikdə VS Code-da olan terminalda console-a göndərdiyimiz body-ni görəcəksiniz
\
## req.headers( )
Headers request zamanı önəmli məlumatlar göndərmək üçündür. Misal- göndərilən body-nin hansı tipdə olacağına baxaq. Hələki ikisi bilməyiniz kifayətdir. 
- **Content-Type: application/json:** Bu content-type, göndərilən məlumatın JSON formatında olduğunu bildirir.
- **Content-Type: multipart/form-data** Bu content-type, form məlumatlarının göndərilməsi zamanı istifadə edilir. Bu format file upload funksiyalarında işlənir.

Əlavə olaraq request zamanı headers göndərək. Həmin headers-i backend-də görmək üçün req.headers[ ' key ' ] açar sözünü yazırıq. Misal olaraq biz headers hissədən gələn content-type götürmək istəyirik. O zaman POSTMAN proqramında linkin altında olan seçimlərdən headers click edib. key - value göndərək.  
- key hissəsində content-type yazaq
- value hissəsində application/json yazaq.

Users.js faylımızda request-dən gələn headers-i key açar sözü ilə console-a çıxaraq.
```js
//users.js faylı
router.post("/",  (req,  res)  =>  {
	console.log(req.body)
	console.log(req.headers['content-type'])
	res.json({ message:  "Z-> POST /users metodu isledi"  });
});
```
POSTMAN-da request göndərdikdə VS Code-da terminalda key( açar sözə ) uyğun value( dəyəri ) görə bilərik. 


## req.params(  )

req.params( ) adətən backend-ə özəl bir məlumatı linkdə göndərəndə olur. Adətən məlumatların linkdə olan id-sinə görə axtarışında vəya urlencoded formasında olan məlumatların linkdən key ( açar sözünə görə götürülməsində istifadə olunur) 

Misal bizim backend serverimiz-də 3 dənə istifadəçinin olduğu bir array var. 
```js
const  users  = [
	{
		id:  1,
		username:  "Ehmed",
		age:  13,
	},
	{
		id:  2,
		username:  "Leyla",
		age:  21,
	},
	{
		id:  3,
		username:  "Nermin",
		age:  15,
	}
];
```

Və frontend tərəf bizdən bütün user-ləri istəyir. O zaman frontend tərəfdən  bizə ' /users'  linkinə GET metodu göndərəcək. Biz də response olaraq bütün user-ləri qaytaracıq.
```js
// users.js faylı
const  router  =  require('express').Router()

const  users  = [
	{
		id:  1,
		username:  "Ehmed",
		age:  13,
	},
	{
		id:  2,
		username:  "Leyla",
		age:  21,
	},
	{
		id:  3,
		username:  "Nermin",
		age:  15,
	}
];

// users routes
router.get("/", (req,  res)  =>  {
	// response olaraq users array-ini qaytardıq
	res.json(users);
});

router.post("/", (req,  res)  =>  {
	res.json({ message: " users added( POST )" });
});

router.put("/", (req,  res)  =>  {
	res.json({ message: " users changed( PUT )" });
});

router.delete("/", (req,  res)  =>  {
	res.json({ message: " users deleted( DELETE )" });
});
  
module.exports  = router
```

POSTMAN ilə ' /users ' linkin' POST metodu ilə request göndərsəniz.  Response olaraq bütün user-ləri qaytaracaq.

**Bəs biz user-lərin arasından id-si 1 olan isitfadəçini çağırmaq istəsək?**
Burada req.params( ) bizim köməyimizə çatır.
Frontend tərəfdən **id-si 1 olan** user-i çağırmaq üçün request **' /users/1'** formasında olur.
 - **id: 1** olan user-i çağırmaq üçün **'/users/1'**
 - **id: 6** olan user-i çağırmaq üçün **'/users/6'**
 - **id: 23** olan user-i çağırmaq üçün **'/users/23'**

Biz isə backend tərəfdə linki **' /users/1'** bu formada olan GET metodundan **/1** -i götürmək üçün ona uyğun GET metodu yazmalıyıq. 
```js
router.get("/:id", (req,  res)  =>  {
	console.log(req.params.id) // 1
	res.json(req.params.id);
});
```
Linkdən gələn id-ni console-a çıxartdıq. Gəlin həmin id-yə uyğun user-i tapıb göndərək. Tapmasa error qaytarsın.
```js
router.get("/:id", (req,  res)  =>  {
	const  findUser  = users.find((item)  => item.id == req.params.id);
	findUser 
		? res.json(findUser) 
		: res.json({message:"User not found"})
});
```
Artıq /users/:id linkində olan id-ni götürüb, həmin id-ə uyğun user-i tapın response olaraq göndərdik. Tapmasa da mesaj olaraq Error göndərdik.
Lakin bizim göndərdiyimiz response-ları frontend tərəf user kimi başa düşür. Və biz response göndərərkən mütləq status qeyd etməliyik. 200 seriyalı statusCode-lar istəyin qəbul olunub, və istəyə uyğun məlumatın göndərilməsi ilə bağlıdır. 400 seriyalı isə əksinədir. 

```js
router.get("/:id", (req,  res)  =>  {
	const  findUser  = users.find((item)  => item.id == req.params.id);
	findUser 
	? res.status(200).json(findUser)
	: res.status(404).json({message:"User not found"})
});
```
Artıq frontend tərəf gələn response-un statusuna görə istədiyi datanın gəlib gəlmədiyini bilir.
Və aşağıda kodun ümumi halı

POSTMAN-da 

```js
// users.js faylı
const  router  =  require('express').Router()

const  users  = [
	{
		id:  1,
		username:  "Ehmed",
		age:  13,
	},
	{
		id:  2,
		username:  "Leyla",
		age:  21,
	},
	{
		id:  3,
		username:  "Nermin",
		age:  15,
	}
];

// users routes
router.get("/", (req,  res)  =>  {
	res.json(users);
});

router.get("/:id", (req,  res)  =>  {
	const  findUser  = users.find((item)  => item.id == req.params.id);
	findUser 
	? res.status(200).json(findUser)
	: res.status(404).json({message:"User not found"})
});

router.post("/", (req,  res)  =>  {
	res.json({ message: " users added( POST )" });
});

router.put("/", (req,  res)  =>  {
	res.json({ message: " users changed( PUT )" });
});

router.delete("/", (req,  res)  =>  {
	res.json({ message: " users deleted( DELETE )" });
});
  
module.exports  = router
```
