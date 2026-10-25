# V0: Точка отсчёта и первые инженерные шаги

Итак, самая первая версия. Я ввёл её исключительно для того, чтобы показать свой инженерный путь.

Тут ещё нет какой-либо архитектуры — всё намешано, но что-то я всё же отделял (например, работу с JSON).

Но главное здесь — отсутствие какой-либо базы данных, ведь все данные писались в JSON-файл.

JSON

```
{
    "flag": true,
    "token":"",
    "initDatabase": false,
    "apps": {
        "app1": {
            "name_app": "chrome ",
            "path_app": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe",
            "hotkey": "alt+2"
        }
    },
    "folders": {
        "folder1": {
            "name_folder": "",
            "path_folder": "D:\\web dev curse",
            "hotkey": "alt+3"
        }
    },
    "scripts": {}
}
```

## Проблемы реализации

### 1. Отсутствие разделения ответственности и зон

Нет понятия разделения потоков (поскольку на тот момент с данной проблемой я не сталкивался вовсе).

На данном этапе понятия backend у меня не существовало, и я с фронта сразу писал в бек.

Тут всё намешано:

JavaScript

```
function setToken(){
    const data = read_json_data(data_file_path);
    let new_token = token_input.value;
    data.token = new_token;
    const jsonString = JSON.stringify(data, null, 4);
    fs.writeFile(data_file_path, jsonString, (err) => {
        if (err) {
            console.error('Ошибка записи в файл:', err);
        } else {
            console.log('Данные успешно добавлены/обновлены в файл', data_file_path);
        }
    });
}

function addNewFolder(){
    const data = read_json_data(data_file_path);
    let foldersData = data.folders;
    let last_folder_index = 0;
    if (foldersData) {
        last_folder_index = Object.keys(foldersData).length;
    } else {
        console.warn("data.apps is undefined or null. Starting with index 0.");
    }

    let newfolderKey = `folder${last_folder_index + 1}`;
    const newfolderObject = create_new_folder_object();
    if(newfolderObject == undefined){

    } else {
        data.folders[newfolderKey] = newfolderObject;
        const jsonString = JSON.stringify(data, null, 4);
        fs.writeFile(data_file_path, jsonString, (err) => {
            if (err) {
                console.error('Ошибка записи в файл:', err);
            } else {
                console.log('Данные успешно добавлены/обновлены в файл', data_file_path);
            }
        });
    }
}

function addNewApp() {
    const data = read_json_data(data_file_path);
    if (!data.apps) {
        data.apps = {};
    }
   
    let appsData = data.apps;
    let last_app_index = 0;
    if (appsData) {
        last_app_index = Object.keys(appsData).length;
    } else {
        console.warn("data.apps is undefined or null. Starting with index 0.");
    }
    let newAppKey = `app${last_app_index + 1}`;
    const newAppObject = create_new_app_object();
    if(newAppObject == undefined){
    
    } else {
        data.apps[newAppKey] = newAppObject;
        const jsonString = JSON.stringify(data, null, 4);
        fs.writeFile(data_file_path, jsonString, (err) => {
            if (err) {
                console.error('Ошибка записи в файл:', err);
            } else {
                console.log('Данные успешно добавлены/обновлены в файл', data_file_path);
            }
        });
    }
}
```

Это файл `add_new_app.js`. Казалось бы, он должен отвечать за страницу добавления приложений, но здесь `addNewApp`, `addNewFolder`, `setToken` — три разные зоны ответственности.

### 2. Сравнение работы с потоками

Касательно потоков:

- **Это v0 без потоков:** ![2026-10-16 21-40-24.gif]( assets/2026-10-16%2021-40-24.gif)
    
- **Вот с потоками:** ![2026-10-16 21-40-38.gif](assets/2026-10-16%2021-40-38.gif)
    

Да, возможно разница и не заметна, но в JSON всего два элемента, а если их будет больше, то и виснуть версия без потоков будет больше.

## Интерфейс v0

Вот пример UI:

![Screenshot_14 1.png](assets/Screenshot_14%201.png)

## Итог этапа

На этом у меня в целом всё. Больше рассказать нечего — это был этап 0, мой первый этап на пути становления инженера.


* 🔗 **[Посмотреть архивный код V0](https://github.com/bobr2026/productivity-desktop-app-v0)**
