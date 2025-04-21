# مدیریت حساب بانکی

<div dir="rtl">
سوال اول:
کلاس AccountBalanceCalculator یک state یا حالت داخلی داره (transactionHistory) ولی متد calculateBalance() اصلاً از اون استفاده نمی‌کنه. یعنی شما می‌تونی هر تعداد transaction اضافه کنی به history، اما بعد که calculateBalance() رو صدا بزنی، هیچ توجهی به اون‌ها نمی‌کنه!

چون که متد addTransaction تست نشده اصلا


آزمون مردود:
</div>


```java
@Test
    void testBalanceUsingHistory_shouldDetectBug() {
        int balance = AccountBalanceCalculator.calculateBalance(Arrays.asList(
                new Transaction(TransactionType.DEPOSIT, 100),
                new Transaction(TransactionType.WITHDRAWAL, 0)
        ));
        AccountBalanceCalculator.addTransaction(new Transaction(TransactionType.DEPOSIT, 100));
        AccountBalanceCalculator.addTransaction(new Transaction(TransactionType.WITHDRAWAL, 50));

        int balance2 = AccountBalanceCalculator.calculateBalance(AccountBalanceCalculator.getTransactionHistory());

        assertEquals(150, balance);
    }
```


<div dir="rtl">

---------------------

سوال دوم

با اضافه کردن این خط این مشکل حل می شود

transactionHistory.addAll(transactions);
</div>

```java
public static int calculateBalance(List<Transaction> transactions) {
        transactionHistory.addAll(transactions);
        int balance = 0;
        for (Transaction t : transactions) {
            if (t.getType() == TransactionType.DEPOSIT) {
                balance += t.getAmount();
            } else if (t.getType() == TransactionType.WITHDRAWAL) {
                balance -= t.getAmount();
            }

        }
        return balance;
    }
```

----------------------

<div dir="rtl">
سوال سوم:

نوشتن آزمون پس از کدنویسی می‌تواند باعث شود که تست‌ها سوگیری نسبت به پیاده‌سازی موجود داشته باشند و باگ‌های طراحی پنهان بمانند. همچنین ممکن است بخش‌هایی از کد تست‌ناپذیر نوشته شده باشند که نیاز به بازنویسی داشته باشند. در نهایت، نبود تست‌های کافی در طول توسعه می‌تواند اشتباهات منطق پنهانی را دیرتر آشکار کند و هزینه‌ی رفع آن‌ها را افزایش دهد.



</div>


