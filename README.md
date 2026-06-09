# Sudoku — Single-File Web App

A zero-dependency Sudoku game in one `.html` file. No build tools, no frameworks, no server.

## Technical highlights

**Generation — Web Worker + backtracking with bitmasks.**
The puzzle is generated in a separate thread so the UI never freezes. Uniqueness of solution is guaranteed: after each cell removal `countSolutions()` runs with an early exit at 2 — if more than one solution exists, the cell is restored. Candidate checking uses integer bitmasks (`Int16Array`) instead of loops, reducing `isSafe` from O(27) to O(1).

**State — typed arrays in memory.**
The board is stored as `Int8Array[9][9]`, not read from the DOM on every check. DOM is written once per game start and on every player input — no polling, no full redraws.

**Validation — numeric Set.**
Errors are tracked as `r * 9 + c` integers in a `Set<number>`, avoiding string key allocation. All three constraints (rows, columns, 3×3 boxes) are checked in a single pass, then `classList.toggle` is called only once per cell.

**Input — event delegation.**
One `click` listener on the `<table>` handles all 81 cells instead of 81 individual listeners.

**Layout — flex without magic numbers.**
`body` is a `flex-column` at `100dvh`. The grid wrapper uses `flex: 1 1 0` + `min-height: 0` to fill remaining space and shrink freely. Font size scales with the real cell pixel size via `ResizeObserver`.

**Win detection** is a byproduct of validation: if the error set is empty and all 81 cells are filled, `body.won` is toggled and all digits turn orange.


Any browser with Web Worker and `100dvh` support (Chrome 105+, Firefox 110+, Safari 15.4+). No installation.

---

# Судоку — Single-File Web App

Гра Судоку в одному `.html` файлі. Без залежностей, без фреймворків, без сервера.

## Технічні характеристики

**Генерація — Web Worker + бектрекінг з бітовими масками.**
Головоломка генерується в окремому потоці — UI не підвисає. Унікальність розв'язку гарантована: після кожного видалення клітинки запускається `countSolutions()` з раннім виходом при 2 розв'язках. Перевірка кандидатів через цілочисельні бітові маски (`Int16Array`) — O(1) замість O(27) циклів.

**Стан — типізовані масиви в пам'яті.**
Дошка зберігається як `Int8Array[9][9]`, а не читається з DOM при кожній перевірці. DOM оновлюється один раз при старті гри і при кожному введенні гравця.

**Валідація — числовий Set.**
Помилки зберігаються як `r * 9 + c` у `Set<number>` — без конкатенації рядків. Три правила (рядки, стовпці, блоки 3×3) перевіряються за один прохід, після чого `classList.toggle` викликається рівно один раз на клітинку.

**Введення — делегування подій.**
Один `click`-слухач на `<table>` замість 81 окремого.

**Layout — flex без магічних констант.**
`body` — `flex-column` на `100dvh`. Обгортка сітки: `flex: 1 1 0` + `min-height: 0` — займає залишок висоти і вільно стискається. Розмір шрифту цифр синхронізується з реальним px-розміром клітинки через `ResizeObserver`.

**Визначення перемоги** є побічним результатом валідації: якщо множина помилок порожня і всі 81 клітинка заповнена — додається `body.won` і всі цифри стають помаранчевими.


Будь-який браузер з підтримкою Web Worker та `100dvh` (Chrome 105+, Firefox 110+, Safari 15.4+). Встановлення не потрібне.
