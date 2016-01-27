VkQuery
=============

Класс для работы с API ВКонткте Standalone-приложениям
Для работы приложения нужно сгенерировать [токен](http://vk.com/dev/auth_mobile)

## Установка

```php
$token = '';//Полученый токен, если нужно

include('./VkQuery.php');
$vk = new VkQuery($token);
```

## Примеры использования

## Простое использование для получения данных $vk->getData()

Для начала формируем массив передаваемых значений, в качестве ключа используем название передаваемого параметра согласно документации

```php
$params = array(
'user_ids' => '205387401,16512807,15006777,67838708,208760504',
'fields' => 'sex,bdate,city,country,photo_50',
'name_case' => 'abl'
);
```

Передаем значение через метод getData, сначала указываем используемый метод, далее массив данных сформированный на предыдущем шаге

```php
$user = $vk->getData('users.get', $params);

//После обрабатываем полученные данные $user->response
```

Пример использования метода execute

```php
$vk->getData('execute', $code);
```

## Пример добавления поста на стену $vk->getData()

Формируем необходимие данные, если загружаем в группу указываем ид группы со знаком минус

```php
$param = array(
'owner_id' => -12345,
'from_group' => 1,
'message' => 'Kakoyto text!',
'attachments' => 'photo123_123','http://site.ru'
);
```

Добавляем пост

```php
$vk->getData('wall.post',$param);
```

## Пример загрузки изображений $vk->uploadsImages()

Формируем необходимие данные

```php
$img = array('file1.jpg','file2.jpg'); //Массив изображений, если нужно загрузить белее одной картинки
$group_id = 12345; //id группы, если загружаем в группу
$param['album_id'] = 12345; //id альбома, если загружем в альбом
```

Если передаём больше одного изображения, то посчитаем количество изображений и будем загружать через цикл
Изображения загружаем методом uploadsImage(), в качестве передаваемых значений передаем наши изображения, далее указываем куда будем загружать, если на стену, указываем 'wall', если в альбом, указываем 'album', следующим параметром указываем группу, если нужно загружать картинкку в группу, либо ставим null, далее последний не обязательный параметр $param - доп. параметры, если нужно загружать в альбом, то нужно передать $param['album_id'] - ID альбома, и другие параметры напр. текст к фото и т.п.

Пример загрузки нескольких изображений в определенный альбом группы пользователя (для загрузки в группу нужно иметь права как минимум редактора):

```php
$countImg = count($img); //считаем кол-во изображений
for($i=1;$i<=$countImg;$i++){
 $photoSave = $vk->uploadsImage($img[$i-1], 'album', $group_id, $param);
}
```

Пример загрузки картинки на стену группы

```php
$vk->uploadsImage($img, 'wall', $group_id);
```

Пример загрузки картинки на стену пользователя (на собственную стену)

```php
$vk->uploadsImage($img);
```

В альбом пользователя

```php
$vk->uploadsImage($img, 'album', null, $param['album_id']);
```

## Пример загрузки видео $vk->uploadsVideo()

Формируем необходимие данные

```php
$video = 'file.avi'; //Файл содержащий видео
$param = array(...); //Если необходимо, дополнительные параметри напр. заголовок, описание и т.п., 
//если передаем файл с видео хостинга, то обязательно указываем в ключе 'link'=>'ссылка_на_ролик'
```

При добавлении своего видео

```php
$vk->uploadsVideo($video, $param);
```

При добавлении с видео-хостинга

```php
$vk->uploadsVideo(null, $param);
```

## Пример загрузки аудио $vk->uploadsAudio()

Формируем необходимие данные

```php
$audio = 'file.mp3'; //Загружаемый файл
$param = array(...); //Дополнительные параметры (название и т.п.)
```

Загружаем в вк

```php
$vk->uploadsAudio($audio, $param);
```

## Пример загрузки документов $vk->uploadsDocuments()

Формируем необходимие данные

```php
$file = 'file.doc'; //Загружаемый файл
$param = array(...); //Массив данных для docs.save, по умолчанию null
$group_id = 12345; //id группы, если нужно, либо null
```

Загружаем документ

```php
$vk->uploadsDocuments($file,$param,$group_id);
```
