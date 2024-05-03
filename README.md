# comtrade-parsing
Как работать с Comtrade API (актуально на момент весны 2024 года)
## Начало работы
Для начала в нужно зарегистрироваться на [сайте](https://comtradedeveloper.un.org/) (можно по гугл почте)
Мы будем получать данные с этого [URI](https://comtradedeveloper.un.org/api-details#api=comtrade-v1&operation=get-get), на сайте можно изучить его структуру. 

В личном кабинете можно получить ключи доступа.
*Они имеют ограничение в 500 запросов в день и один запрос раз в 10 секунд. Поэтому асинхронно или параллельно парсить данные не получится*
## Построение запроса
*Удонбный конструктор запроса можно удобно посмотреть [тут](https://comtradeplus.un.org/TradeFlow?Frequency=A&Flows=X&CommodityCodes=TOTAL&Partners=0&Reporters=all&period=2023&AggregateBy=none&BreakdownMode=plus)*. Там же можно посмотреть какие данные можно передавать
### Для того чтобы отправить запрос по API нужны следующие параметры:

**Базовый URL**: https://comtradeapi.un.org/data/v1/get/
**Обязательные аргументы**:
- **typeCode** - Type of trade: C for commodities and S for service
- **freqCode** - Trade frequency: A for annual and M for monthly. 
- **clCode** - Trade (**IMTS**) classifications: **HS, SITC, BEC or EBOPS**. Классификация товаров, в лабе используют 'HS'

**Необязательные аргументы:**
- **reporterCode** - код ООН М49. Может быть списком, перечисленным через запятую 
	❗**Важно**: код 0 для всего мира, также встречаются коды несуществующих государств (СССР, Югославия и пр.), встречаются отдельные коды для ЕС и других обьединений, их нужно предварительно чистить
	
- **period** - год или месяц. 
	❗Оба варианта подразумевают период в виде целого числа. *Annual имеет формат YYYY , M формат YYYYMM  (Example: 201001 - 01.2010)*
	
- **partnerCode** - M49 код партнера.
- **partner2Code** - M49 код второго партнера. 
	Например, при параллельном импорте в Россию через Турцию - она будет вторым партнером 
	
- **cmdCode** - коды ТНВЭД. 
	Например: 
	- TOTAL - суммарный (например, если хотим посмотреть топ стран по экспорту)
	- 2709 - по нефти сырой и газовому конденсату
	- 2710 - нефтепродукты
	- 271012 - бензин 
- **flowCode** -  "X" - Export, "M" - Import, есть еще множество, можно найти [тут](https://comtradeplus.un.org/TradeFlow?Frequency=A&Flows=X&CommodityCodes=TOTAL&Partners=0&Reporters=all&period=2023&AggregateBy=none&BreakdownMode=plus)
