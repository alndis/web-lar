
# Web-ларёк
### Автор: Сидиков Никита Александрович

Интернет-магазин с товарами для веб-разработчиков. В нём можно посмотреть каталог товаров, добавить товары в корзину и делать заказы.

## Описание
Проект построен на архитектурном принципе MVP (Model-View-Presenter), который обеспечивает чёткое разделение обязанностей между компонентами:

Model — отвечает за загрузку данных через API, их хранение и обработку пользовательских данных.

View — обеспечивает отображение интерфейса и взаимодействие с пользователем, а также фиксирует события.

EventEmitter — выполняет роль Presenter, связывая Model и View. Он управляет взаимодействием между ними при возникновении событий.

### Стек технологий:
    HTML, SCSS, TS, Webpack

### Структура проекта
```
src/                   # Исходные файлы проекта
├── components/        # Папка с JS-компонентами
│   └── base/         # Базовые классы
├── pages/
│   └── index.html    # HTML-файл главной страницы
├── styles/
│   └── styles.scss   # Корневой файл стилей
├── types/
│   └── types.ts      # Файл с типами данных
├── utils/
│   ├── constants.ts  # Файл с константами
│   └── utils.ts      # Файл с утилитами
└── index.ts          # Точка входа приложения
```


### Ключевые файлы:
- src/pages/index.html — главная страница
- src/types/index.ts — типы данных
- src/index.ts — точка входа в приложение
- src/styles/styles.scss — основные стили
- src/utils/constants.ts — константы
- src/utils/utils.ts — вспомогательные функции

## Программный интерфейс компонентов

### Model (Модель)
Модель ответственна за взаимодействие с данными. Она получает информацию с сервера и передаёт её View для отображения. Каждая модель представляет собой независимый компонент, который может использовать данные и обновлять их по мере необходимости.

- **ApiModel**: Унаследован от класса `Api` и взаимодействует с сервером, получает список товаров и отправляет заказы.
- **BasketModel**: Управляет состоянием корзины покупок, такими как количество товаров, общая стоимость и добавление/удаление товаров.
- **FormModel**: Работает с пользовательскими данными для обработки заказов (адрес, контактные данные и способ оплаты).

### View (Представление)
Представление отвечает за отображение данных и взаимодействие с пользователем. Компоненты View получают данные от Model и представляют их на интерфейсе.

- **Basket**: Отображает корзину покупок, включая количество товаров и их общую стоимость.
- **Card**: Отображает карточки товаров, включая категорию и цену.
- **Order**: Отображает содержимое модальных окон для оформления заказа и выбора способа оплаты.

### EventEmitter (Посредник событий)
Класс, реализующий паттерн "Observer". Он управляет событиями между Model и View. Это обеспечивает связь между различными компонентами без их жёсткой связи.

Методы:
- `on`: Подписка на событие.
- `off`: Отписка от события.
- `trigger`: Генерация события для передачи информации другим компонентам.

## Типы данных, с которыми работает приложение

# Описание типов данных

## 1. `IProductItem`
Этот интерфейс описывает элемент продукта, который будет отображаться в приложении. Он включает в себя следующую информацию:
- **id** (string): Уникальный идентификатор продукта.
- **description** (string): Описание продукта.
- **image** (string): URL-адрес изображения продукта.
- **title** (string): Название продукта.
- **category** (string): Категория продукта.
- **price** (number | null): Цена продукта. Может быть `null`, если цена отсутствует.

## 2. `IActions`
Этот интерфейс используется для описания обработчика событий, который выполняется при клике:
- **onClick** (function): Функция, которая будет вызвана при событии клика. Принимает объект `MouseEvent`.

## 3. `IOrderForm`
Интерфейс, описывающий форму заказа. Включает данные, которые пользователь должен заполнить при оформлении заказа:
- **payment** (string, optional): Способ оплаты.
- **address** (string, optional): Адрес доставки.
- **phone** (string, optional): Номер телефона.
- **email** (string, optional): Адрес электронной почты.
- **total** (string | number, optional): Общая стоимость заказа, может быть строкой или числом.

## 4. `IOrder`
Этот интерфейс расширяет `IOrderForm` и добавляет информацию о товарах, включённых в заказ:
- **items** (string[]): Массив идентификаторов продуктов, которые были выбраны пользователем.

## 5. `IOrderLot`
Интерфейс для представления деталей заказа для оформления покупки. Включает в себя:
- **payment** (string): Способ оплаты.
- **email** (string): Электронная почта пользователя.
- **phone** (string): Номер телефона пользователя.
- **address** (string): Адрес доставки.
- **total** (number): Общая стоимость заказа.
- **items** (string[]): Массив идентификаторов товаров, которые включены в заказ.

## 6. `IOrderResult`
Интерфейс для представления результата оформления заказа:
- **id** (string): Уникальный идентификатор заказа.
- **total** (number): Общая сумма заказа.

## 7. `FormErrors`
Тип для хранения ошибок формы. Это частичное отображение, где ключами являются поля интерфейса `IOrder`, а значениями — сообщения об ошибках:
- **Partial<Record<keyof IOrder, string>>**: Отображение каждого поля формы (например, `payment`, `address`, `phone`, и т. д.) в строку ошибки.

### Типы функций:

1. **handleResponse(response: Response): Promise<object>** — обрабатывает ответ от сервера.
2. **get(uri: string): Promise<object>** — отправляет GET-запрос и возвращает данные.
3. **post(uri: string, data: object, method: ApiPostMethods): Promise<object>** — отправляет POST, PUT или DELETE запросы.

## Описание классов Model, которые позволяют хранить и обрабатывать данные с сервера и от пользователей.

### Класс `ApiModel` Наследуется от `Api` и отвечает за взаимодействие с сервером.

Методы:
- `getListProductCard` - получаем массив объектов(карточек) с сервера.
- `postOrderLot` - получаем ответ от сервера по сделанному/отправленному заказу.

### Класс `BasketModel` управляет данными корзины.

Методы:
- `getCounter` - возвращает количество товаров в корзине.
- `getSumAllProducts` - рассчитывает общую стоимость товаров.
- `setSelectedСard` - добавляет товар в корзину.
- `deleteCardToBasket` - удаляет товар из корзины.
- `clearBasketProducts` - очищает/удаляет все товары из корзины.

### Класс `DataModel` хранит данные о товарах.

Метод:
- `setPreview` - сохранение данных о выбранном товаре.

### Класс `FormModel` работает с пользовательскими данными.

Методы:
- `setOrderAddress` - сохраняет адрес пользователя.
- `validateOrder` - проверяет адрес пользователя и способ оплаты.
- `setOrderData` - сохраняет контактные данные пользователя.
- `validateContacts` - проверяет контактные данные пользователя.
- `getOrderLot` - возвращает данные заказа пользователя с выбранными товарами.

## Классы View позволяют отображать элементы страницы с полученными данными, позволяют взаимодействовать с пользователем.

### Класс `Basket` управляет отображением корзины.

Методы:
- `renderHeaderBasketCounter` - отображает количество товаров в корзине.
- `renderSumAllProducts` - сохраняет и устанавливает сумму синапсов всех товаров в корзине.

### Класс `BasketItem` отвечает за отображение товаров в корзине.

Метод:
- `setPrice` - форматирует цену товара.

### Класс `Card` управляет отображением карточки товара.

Методы:
- `setText` - принимает два значения, первое HTMLElement, второе значение задаёт текстовое содержимое HTMLElement.
- `cardCategory` - принимает строчное значение и создаёт новый className для HTMLElement.
- `setPrice` - принимает цену продукта в числовом значении и возвращает в строчном.

### Класс `CardPreview` наследуется от класса `Card` и управляет отображением подробного описания карточки товара в превью, позволяет добавить карточку в корзину.

Метод:
- `notSale` - принимает данные о продукте, проверяет наличие цены продукта, при отсутствии цены ограничивает покупку.

### Класс `Order` управляет отображением содержимого модального окна и позволяет принять от пользователя метод оплаты и адрес.

Метод:
- `paymentSelection` - выделяет выбранный способ оплаты.

### Класс `Contacts` управляет отображением содержимого модального окна и отвечает за ввод контактных данных пользователя.

### Класс `Modal` управляет отображением модальных окон.

Методы:
- open - открывает модальное окно.
- close - закрывает модальное окно.

### Класс `Success` управляет отображением успешного заказа в модальном окне.

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```

## Сборка

```
npm run build
```

или

```
yarn build
```
