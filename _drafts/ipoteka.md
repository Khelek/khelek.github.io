https://www.sberbank.ru/ru/person/credits/home/buying_complete_house

Процент ипотеки был 10.5%

years = getint($("#creditTerm").value);
initialFee = getint($("#initialFee").value)
monthlyPayment = getint($("[data-test-id=monthlyPayment]").textContent)
apart_cost = getint($("#estateCost").value)
rent_price = 12 * years * 40000;
debit = (() => {
    sum = initialFee;
    for (var year = 1; year <= years; year++) {
        procents = sum*0.05
        sum = sum + procents + (monthlyPayment - 40000)*12;
        console.log(year, sum, Math.round(procents / 12))
    }
    return sum
})()
full_cost = monthlyPayment * years * 12 + initialFee
credit_cost = full_cost - apart_cost;
console.log(format(full_cost), format(credit_cost), format(rent_price), format(debit), format(debit - apart_cost), format(rent_price - (debit - apart_cost)), format(credit_cost - (rent_price - (debit - apart_cost))))


Получалось, что при 7 лямах на квартиру и 6 годах, ипотека выгоднее аренда на 500к.
Но зато эти 7 лет ты живешь где хочешь, и платишь столько же, сколько за ипотеку.

Интересно, что при более высоких откладываемых суммах становится выгоднее арендовать.
Например при 10л квартире на 9лет, если арендованная квартира также 40к - то это +3.8л.


Но возможно надо считать по другому - если взять месячный платеж от ипотеки, вычесть от него аренду, и отложить - то если ты накопишь ту же сумму за срок ипотеки, то проще арендовать.


То есть, если хочешь квартиру - отладывай столько же как на ипотеку, и арендуй в это время спокойно. Из-за процентов на доход вклада ты не платишь аренду "вникуда".

Но это если плата за ипотеку сильно выше аренды. Иначе накопиться не успеет. - Гениальный вывод! Если платить за ипотеку дороже, чем арендовать, то это невыгодно. логично.

Однако - 54к ипотеки, 40к аренды(+14к откладывается). Через 15лет накапливается вся сумма(6л при 1л первичного). При этом если ты в середине аренды решаешь взять ипотеку(на остаток суммы), это стоит ровно столько же - 54к.
И ты не обязан банку, можешь жить где хочешь.


Только вот стоимость квартиры растет.
