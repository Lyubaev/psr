# PSR-0

Ниже описаны обязательные требования, которые необходимо соблюдать для поддержания функциональной совместимости автозагрузчика.

## Обязательно

- Правильно подготовленные пространства имен и классы должны иметь следующую структуру \&lt;Vendor Name&gt;\ (&lt;Namespace&gt;)\ *&lt;Class Name&gt;
- Каждое пространство имен должно иметь namespace высшего уровня ("Vendor Name").
- Каждый namespace может иметь сколько угодно подпространств имен.
- Каждый разделитель пространств имен преобразуется в DIRECTORY_SEPARATOR при загрузке файла
- Каждый символ _ в имени класса преобразуется в DIRECTORY_SEPARATOR. Символ _ не имеет специального значения в имени пространства имен.
- К соответствующим стандарту пространствам имен и классам добавляется .php при загрузке файла из файловой системы.
- Буквенные символы в имени vendor, namespace или class могут иметь любую комбинацию маленьких и больших букв.

## Примеры
```php
\Doctrine\Common\IsolatedClassLoader =>
/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php

\Symfony\Core\Request =>
/path/to/project/lib/vendor/Symfony/Core/Request.php

\Zend\Acl =>
/path/to/project/lib/vendor/Zend/Acl.php

\Zend\Mail\Message =>
/path/to/project/lib/vendor/Zend/Mail/Message.php
```
### Нижнее подчеркивание в имени Namespace и Класса
```php
\namespace\package\Class_Name =>
/path/to/project/lib/vendor/namespace/package/Class/Name.php

\namespace\package_name\Class_Name =>
/path/to/project/lib/vendor/namespace/package_name/Class/Name.php
```
Стандарт описанный здесь, должен стать наименьшим общим знаменателем для безболезненного создания совместимых загрузчиков. Вы можете проверить это сами использовав примерную реализацию автозагрузчика SplClassLoader.

## Пример реализации

Ниже приведен пример простой функции демонстрирующей как реализуется и работает описанный выше стандарт для автозагрузчиков.
```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

## Реализация SplClassLoader

По ссылке приведен gist с примером реализации SplClassLoader, который вы сможете использовать для загрузки своих классов если они удовлетворяют требованиям описанного стандарта. Такой способ является рекомендуемым способом загрузки классов.

[http://gist.github.com/221634](http://gist.github.com/221634)
