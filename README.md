# P2P

<div class="section">

- [<span class="emoji-prefix">🌝</span> Основные понятия](#основные-понятия)
  - [<span class="emoji-prefix">💳</span> Статусы карт](#статусы-карт)
  - [<span class="emoji-prefix">♦️</span> Подтипы карт](#подтипы-карт)
  - [<span class="emoji-prefix">💸</span> Статьи расходов](#статьи-расходов)
- [<span class="emoji-prefix">⚖️</span> Метрики](#метрики)
  - [<span class="emoji-prefix">💰</span> Оборот](#оборот)
- [<span class="emoji-prefix">📖</span> Data Dictionary](#data-dictionary)
  - [<span class="emoji-prefix">💳</span> Cards Transactions](#dd-cards-transactions)
- [<span class="emoji-prefix">📊</span> Отдел аналитики](#отдел-аналитики)
  
</div>





<hr class="custom">





<div class="section">

# 🌝 **Основные понятия** <span id="основные-понятия"></span> 

## 💳 **Статусы карт** <span id="статусы-карт"></span> 
- **119** – Активная  
- **120** – Заблокированная  
- **121** – Купленная / Отключенная  
    - **122** – Карта «в отлёжке» (проходит процедуру ожидания после покупки)  

## ♦️ **Подтипы карт**<span id="подтипы-карт"></span> 
- **122** – В отлёжке (проходит процедуру ожидания после покупки)  
- **123** – Проверка баланса  
- **124** – Заблокированная из-за дропа  
- **125** – Заблокированная из-за банка  
- **126** – Лимиты  
- **127** – Вылет (по карте не было транзакций)  
- **128** – Заблокированная по вине сотрудника  
- **129** – Дефолт  
- **130** – ФОП  
- **131** – Только вывод  
- **132** – Имеет незавершенные заявки
- **133** - Не взяли в работу

## 💸 **Статьи расходов**<span id="статьи-расходов"></span> 
- **34** – Коррекция
- **35** – Неподтвержденный инвойс
- **36** – Коррекция неподтвержденного инвойса 

**Остальные**: 
https://metabase.tradition.com.ua/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjo5LCJpbmZvIjp7ImNhcmQtZW50aXR5LWlkIjoib2pCMTlSTmtwZWlHa25CclNfdERDIn0sInF1ZXJ5Ijp7InNvdXJjZS10YWJsZSI6MjMyN30sInR5cGUiOiJxdWVyeSJ9LCJkaXNwbGF5IjoidGFibGUiLCJ2aXN1YWxpemF0aW9uX3NldHRpbmdzIjp7fX0 

</div>





<hr class="custom">





<div class="section">

# 📊 **Метрики**<span id="метрики"></span> 

## 💰 **Оборот**<span id="оборот"></span>

**Оборот по карте** — это агрегация **сумм транзакций** по карте с условием **успешного статуса транзакции** и **наличием мерчанта**.

#️⃣ **Агрегация выполняется по полю**
```sql
cards_transcations.amount
```

🧩 **Условия**
```sql
cards_transactions.status = 1
exchange_applications.merchant_id IS NOT NULL
```

💵 **Для подсчета сбора / погашения**
```sql
cards_transactions.type = 1
```

🔄 **Для подсчета выдачи / обмена**
```sql
cards_transactions.type = 2
```

💡 **Примеры**

  - 💵 **Погашения сводный:**
    https://metabase.tradition.com.ua/question/3632 

  - 🔄 **Выдача сводный:**
    https://metabase.tradition.com.ua/question/3631

</div>





<hr class="custom">





<div class="section">

# 📖 **Data dictionary**<span id="data-dictionary"></span>

## 💳 **Cards Transactions**<span id="dd-cards-transactions"></span>

<table>
    <tr>
        <th>Название</th>
        <th>Ключ к</th>
        <th>Тип</th>
        <th>Описание</th>
        <th>Дополнение</th>
    </tr>
    <tr>
        <td>ID</td>
        <td></td>
        <td>Int</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>Created At</td>
        <td></td>
        <td class="highlight">Date</td>
        <td>Дата</td>
        <td></td>
    </tr>
    <tr>
        <td>Card ID</td>
        <td>Cards</td>
        <td>Int</td>
        <td>Id карты</td>
        <td></td>
    </tr>
    <tr>
        <td>Type</td>
        <td></td>
        <td>Int</td>
        <td>Тип</td>
        <td>1 – сбор (погашение)<br>2 – выдача (обмен)</td>
    </tr>
    <tr>
        <td>Amount</td>
        <td></td>
        <td class="orange-highlight">Double</td>
        <td>Сумма</td>
        <td></td>
    </tr>
    <tr>
        <td>Balance Before</td>
        <td></td>
        <td class="orange-highlight">Double</td>
        <td>Баланс перед транзой</td>
        <td></td>
    </tr>
    <tr>
        <td>Balance After</td>
        <td></td>
        <td class="orange-highlight">Double</td>
        <td>Баланс после транзы</td>
        <td></td>
    </tr>
    <tr>
        <td>Status</td>
        <td></td>
        <td>Int</td>
        <td>Статус</td>
        <td class="question">?</td>
    </tr>
    <tr>
        <td>Exchange Application Id</td>
        <td></td>
        <td>Int</td>
        <td>Id заявки</td>
        <td></td>
    </tr>
    <tr>
        <td>Comment</td>
        <td></td>
        <td class="light-highlight">Text</td>
        <td>Комментарий</td>
        <td></td>
    </tr>
    <tr>
        <td>Commission</td>
        <td></td>
        <td class="orange-highlight">Double</td>
        <td>Комиссия</td>
        <td class="question">?</td>
    </tr>
    <tr>
        <td>Sub Type</td>
        <td></td>
        <td>Int</td>
        <td>Подтип</td>
        <td class="question">?</td>
    </tr>
    <tr>
        <td>Source Card Transaction Id</td>
        <td></td>
        <td>Int</td>
        <td>?</td>
        <td></td>
    </tr>
    <tr>
        <td>Expense Item Id</td>
        <td></td>
        <td>Int</td>
        <td>Статья расходов</td>
        <td>Поробности</td>
    </tr>
    <tr>
        <td>Bank Transaction UID</td>
        <td></td>
        <td>Int</td>
        <td>?</td>
        <td></td>
    </tr>
    <tr>
        <td>Created By</td>
        <td></td>
        <td>Int</td>
        <td>Создатель транзакции (сотрудник)</td>
        <td></td>
    </tr>
    <tr>
        <td>Account ID</td>
        <td></td>
        <td>Int</td>
        <td>?</td>
        <td>116, null<br>— компания P2P</td>
    </tr>
</table>

</div>





<hr class="custom">





<div class="section">

# 📊 **Отдел аналитики**<span id="отдел-аналитики"></span>

</div>
