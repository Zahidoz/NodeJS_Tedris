# Express, Nodemon, send Response

npm paketini yukleyek . . .
```
//terminal
npm init -y
npm i express 
npm i --save-dev nodemon
```


npm paketlərini yüklədikdən sonra **index.js** faylı quraq. Və **package.json** faylında scripts property-sində server start olduqda nodemon ilə faylımızı birləşdirək. **Nodemon** frontend-dəki live server kimidi, yəni server kodunu dəyişdikdə avtomatik dəyişdiklərimizə uyğun server yenilənir.

```json
"scripts":  {
	"start":  "nodemon index.js"
},
```


**index.js** faylımızda express serveri import edib, **3000**-ci PORT-da aktivləşdirək.
```js
const  express  =  require('express')
const  app  =  express()

app.listen(3000,  ()  => console.log("server is running PORT:3000"));
```

Terminalı açıb npm start yazsaq, və sonra Google-da http://localhost:3000/ linkinə daxil olsanız. **Cannot GET /** mesajı görəcəksiniz. Bunun səbəbi odur ki, ' / ' linkinə çata bilmir.
React dərslərindən xatırlayırsınızsa Veb səhifə ilk dəfə açılanda default olaraq ' / ' linki açılırdı. Və biz  path-i ' / ' olan linkdə adətən HomePage, MainPage komponentləri işlədərdik.

Və Express server də aktiv olan kimi ' / ' linkinə default olaraq GET metoduna çatmaq istəyir. 
Xatırlayırsınızsa Javascript-də fetch mövzusunda method yazmasaq orda da default olaraq GET metodu olurdu.

Gəlin bütün metodları qısa nəzərdən keçirək.
- **GET** -  Məlumatı **götürmək** üçün,
- **POST** - Məlumatı **əlavə etmək** üçün.
- **PUT** - Məlumatı düzəltmək, **dəyişmək** üçün
- **DELETE** - Məlumatı **silmək** üçün istifadə olunur.

indi isə linki ' / ' olan, parametr kimi **req**-request, **res**-response  alan GET metodunu yazaq. 
**! ! ! Method mütləq response qaytarmalıdır.** 
- **req** - vasitəsi ilə frontend-dən göndərilən məlumatlara baxmaq olur.
- **res** - vasitəsi ilə frontend-ə cavab göndərmək olur.
\
**! ! ! Method mütləq response qaytarmalıdır.** 
Başlanğıc üçün response göndərməyin 2 növünü bilməyiniz kifayətdir.
- **res.send(" ")** - məlumatı **sadə** formada ekrana göndərmək üçün,  
- **res.json({ })** - məlumatı **json** formatında  göndərmək üçün

## res.send( )
Gəlin birinci res.send metodu ilə baxaq.

```js
const  express  =  require('express')
const  app  =  express()
  
app.get('/',(req,res)=>{
	res.send("/ GET method activated :)")
})

app.listen(3000,  ()  => console.log("z_message-> server is running PORT:3000"));
```

Indi təkrar http://localhost:3000/ linkinə daxil olsanız sadə mətn formasında olan response-u görəcəksiniz.

Əlavə olaraq başqa linklərin GET metodunu yazıb mətn formatında fərqli response göndərək.
```js
const  express  =  require('express')
const  app  =  express()
  
app.get('/',(req,res)=>{
	res.send(' response of GET / ')
})

app.get("/users",  (req,  res)  =>  {
	res.send(' response of GET /users ')
});
  
app.get("/products",  (req,  res)  =>  {
	res.send(' response of GET /products ');
});

app.listen(3000,  ()  => console.log("z_message-> server is running PORT:3000"));
```
Artıq fərqli linklərə göndərilən request-lərə fərqli response-lar göndərə bilirik.
\
## res.json( )
Lakin realda daha çox response JSON formatında göndərilir. O zaman yenidən başlayıb JSON formatında olan response göndərək.

```js
const  express  =  require('express')
const  app  =  express()
  
app.get('/',(req,res)=>{
	res.json({message:"/ GET method activated :)"})
})

app.listen(3000,  ()  => console.log("z_message-> server is running PORT:3000"));
```
Indi təkrar http://localhost:3000/ linkinə daxil olsanız JSON formasında olan response-u görəcəksiniz. 

Əlavə olaraq başqa linklərin GET metodunu yazıb JSON formatında fərqli response göndərək.

```js
const  express  =  require('express')
const  app  =  express()
  
app.get('/',(req,res)=>{
	res.json({message:"/ GET method activated :)"})
})
app.get("/users", (req,  res) => {
	res.json([
		{
			name:  "Leyla",
			age:  26
		},
		{
			name:  "Ehmed",
			age:  34
		},
	]);
});
app.get("/products", (req,  res) => {
	res.json([
		{
			vender:  "Apple",
			model:  "Macbook Pro",
			price:  4900,
		},
		{
			vender:  "Asus",
			model:  "Rog Strix 17",
			price:  3600,
		},
	]);
});

app.listen(3000,  ()  => console.log("z_message-> server is running PORT:3000"));
```


