Что вы делаете если вам необходимо добавить новые таблицы базу данных или столбцы в таблицу(Хотя по поводу новых столбцов нужно больше информации, чем я напишу в этой статье)? В прочем наверное все знают о Code First Migrations.

Во первых View->Package Manager Console Далее в консоли нужно выполнить 3 команды.

> 1. Активация миграции
> 
> ### Enable-Migrations
> 
> 2. Создание миграции
> 
> ### Add-Migration MigrationsName
> 
> 3. Обновление базы данных
> 
> ### Update-Database