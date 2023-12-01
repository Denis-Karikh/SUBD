# 1.Установка MonogoDB
```cmd
docker pull mongo
```
### Создание volume
```cmd
cd /Users/denis/Documents
mkdir mongodata
```
### Запуск контейнера
```cmd
docker run -it -v /Users/denis/Documents/mongodata:/data/db -p 27017:27017 --name mongodb -d mongo
```

# 2. Заполнение данными.

### Создаем базу данных CustomersDB
```cmd
use customersDB
```
### Добавляем данные 
```cmd
db.customers.insertMany( <данные_из_файла_generated.json> )
```

# 3. Запросы данных на выборку и обновление

### Выведем пользователей мужского пола:
``cmd
db.customers.find({gender: "male"})
```
Результат 3 записи.

### Найдём пользователей, у которых в массиве друзей имя первого друга Diaz Hodges
```cmd
db.customers.find({"friends.0.name": "Diaz Hodges"})
```
### Результат
``sql
{
    _id: '65699e20e2333f953b85d3da',
    ...
    name: 'Donovan Pollard',
    gender: 'male',
    company: 'BLEEKO',
    ...
  }
```

### Запросы на обновление.

Обновим возраст пользователя с id 65699e20e2333f953b85d3da с 22 до 23:
```sql
db.customers.updateOne({_id : "65699e20e2333f953b85d3da"}, {$set: {age : 23}})
```
Добавим пользователю с id 65699e20e2333f953b85d3da ещё одного друга Brock Buck
```sql
db.customers.updateOne({_id: "65699e20e2333f953b85d3da"}, {$addToSet: {friends: { id: "65699e209efc374cf54f7706", name: "Shirley Buckner" }}})
```

