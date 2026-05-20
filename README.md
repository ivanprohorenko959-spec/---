Объяснение некоторых функций сайта

Функции работы с LocalStorage (модуль data.js)
Код javascript:

index.html
// data.js
// Модуль управления данными и LocalStorage
const STORAGE_KEYS = {
RECIPES: 'diet_calculator_recipes',
USER_PROFILE: 'diet_calculator_user_profile',
LAST_MENU: 'diet_calculator_last_menu',
LAST_PERIOD: 'diet_calculator_last_period'
};

function saveRecipes(recipes) {
localStorage.setItem( JSON.stringify(recipes));
}

function loadRecipes() {
const data = localStorage.getItem;
if (data) {
return JSON.parse(data);
}
return getDefaultRecipes();
}

function saveUserProfile(user) {
localStorage.setItem(STORAGE_KEYS.USER_PROFILE, JSON.stringify(user));
}

function loadUserProfile() {
const data = localStorage.getItem(STORAGE_KEYS.USER_PROFILE);
return data ? JSON.parse(data) : null;
}

function saveLastMenu(menu, period) {
localStorage.setItem(STORAGE_KEYS.LAST_M
ENU, JSON.stringify(menu));
localStorage.setItem(STORAGE_KEYS.LAST_PERIOD, period);
}

function loadLastMenu() {
const menu = localStorage.getItem(STORAGE_KEYS.LAST_MENU);
const period = localStorage.getItem(STORAGE_KEYS.LAST_PERIOD);
return {
menu: menu ? JSON.parse(menu) : null,
period: period || null
};
}

function getDefaultRecipes() {
return [
{ id: 'r01', name: 'Овсянка с ягодами', ingredients: 'овсяные хлопья 50г, молоко 150мл, ягоды 50г', portion: 250, kcal_per_100: 90, protein_per_100: 3.5, fat_per_100: 2.0, carbs_per_100: 14.0, mealType: 'breakfast' },
{ id: 'r02', name: 'Гречка с куриной грудкой', ingredients: 'гречка 100г, куриная грудка 150г, масло 5г', portion: 350, kcal_per_100: 130, protein_per_100: 12, fat_per_100: 4, carbs_per_100: 15, mealType: 'lunch' },
{ id: 'r03', name: 'Запечённая рыба с овощами', ingredients: 'рыба 150г, брокколи 100г, морковь 50г', portion: 300, kcal_per_100: 110, protein_per_100: 15, fat_per_100: 5, carbs_per_100: 5, mealType: 'dinner' },
{ id: 'r04', name: 'Творог с мёдом', ingredients: 'творог 5% 150г, мёд 10г', portion: 160, kcal_per_100: 140, protein_per_100: 12, fat_per_100: 5, carbs_per_100: 10, mealType: 'snack' },
// ... ещё 8 рецептов
];
}

 Примеры тестовых данных и сценариев

 Проверка расчёта BMR для разных параметров


№ теста Пол Вес (кг) Рост (см) Возраст Ожидаемый BMR Фактический BMR Статус
1 Жен 70 165 30 1420,25 1420,25 Пройден
2 Жен 55 162 45 1189,00 1189,00 Пройден
3 Муж 90 185 35 1957,50 1957,50 Пройден
4 Муж 75 175 28 1685,75 1685,75 Пройден

 Проверка корректировки по цели


Исходный TDEE Цель Ожидаемые калории Фактические Отклонение
2000 Похудение 1700 1700 0%
2000 Поддержание 2000 2000 0%
2000 Набор массы 2200 2200 0%

 Полный цикл работы пользователя (основной сценарий)


1. Пользователь открывает приложение index.html в браузере Chrome.
2. Вводит в форму: возраст 25 лет, вес 80 кг, выбирает цель «Похудение», активность «Средняя».
3. Нажимает кнопку «Рассчитать КБЖУ».
· Ожидание: появление карточек с калориями ~2100 ккал, белками ~157 г, жирами ~58 г, углеводами ~236 г.
· Результат: 2080 ккал, 156 г белков, 58 г жиров, 234 г углеводов. 
4. Нажимает кнопку «Сгенерировать меню на день».
· Ожидание: 4 блюда, суммарная калорийность в пределах 10% от целевой.
· Результат: сумма 1930 ккал (отклонение 7%). 
5. Закрывает браузер, открывает заново.
· Ожидание: поля формы заполнены сохранёнными данными.
· Результат: возраст 25, вес 80, цель «Похудение», активность «Средняя». 

 Обработка ошибок ввода



1. Пользователь вводит возраст 150 лет, вес 80 кг, нажимает «Рассчитать».
· Ожидание: сообщение «Возраст должен быть от 15 до 120 лет».
· Результат: сообщение появилось, расчёт не выполнен.
 2. Пользователь вводит возраст 25, вес 15 кг (меньше 20).
· Ожидание: сообщение «Вес должен быть от 20 до 300 кг».
· Результат: сообщение появилось. 
3. Пользователь не выбирает цель (оставляет значение по умолчанию).
· Ожидание: сообщение «Выберите цель (похудение/поддержание/набор массы)».
· Результат: сообщение появилось.
