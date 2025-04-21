# مدیریت حساب بانکی

<div dir="rtl">
مورد اول:
کلاس AccountBalanceCalculator یک state یا حالت داخلی داره (transactionHistory) ولی متد calculateBalance() اصلاً از اون استفاده نمی‌کنه. یعنی شما می‌تونی هر تعداد transaction اضافه کنی به history، اما بعد که calculateBalance() رو صدا بزنی، هیچ توجهی به اون‌ها نمی‌کنه!

چون که متد addTransaction تست نشده اصلا

</div>
