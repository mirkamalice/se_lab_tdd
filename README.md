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

----------------------
<div dir="rtl">
روش بر طرف کردن مشکل جدید در تست های جدید:

در تست testTransactionHistoryShouldContainOnlyLastCalculationTransactions انتظار خروجی ۲ داشتیم ولی ۴ میداد چون تاریخچه پاک نمی شد پس کد زیر اضافه کردیم:

</div>

```java 
    public static int calculateBalance(List<Transaction> transactions) {
        transactionHistory.clear(); // new code
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
سوال چهارم:

۱ـ   نیازمندی‌ها روشن‌تر شوند. چون باید دقیقاً مشخص می‌کردم که خروجی مورد انتظار هر بخش چی هست.

۲ـ طراحی بهتر انجام بشه. چون تابع باید فقط به ورودی وابسته باشه تا تست‌پذیر بمونه (یعنی بدون side effect مثل تغییر تاریخچه).

۳ـ اشکال‌ها زودتر دیده بشن. مثلاً همین که تاریخچه‌ی تراکنش‌ها به‌درستی مدیریت نمی‌شد، با تست لو رفت.

۴ـ اعتماد به تغییرات بالا رفت. هر تغییری در کد، فوراً با تست‌ها بررسی می‌شه.


</div>

----------------------
<div dir="rtl">
سوال پنجم:

🔷 مزایا:

♦️ کاهش باگ‌ها: چون همیشه از اول به درستی خروجی فکر می‌کنی.

♦️ کد قابل اطمینان‌تر: تست‌ها مثل سندی برای عملکرد درست کد هستند.

♦️ Refactor کردن راحت‌تره: چون تست‌ها بلافاصله نشون می‌دن چیزی خراب شده یا نه.

♦️ مستندسازی غیررسمی: تست‌ها نشون می‌دن برنامه باید چه رفتاری داشته باشه.

🔶 معایب:

♦️ زمان‌بر در ابتدا: برای پروژه‌های ساده یا فوری ممکنه بنظر اضافه‌کار بیاد.

♦️ نیاز به تجربه: تست خوب نوشتن هم هنر می‌خواد، هم تجربه.

♦️ بعضی چیزها سخت تست‌پذیرند: مخصوصاً کدهایی که به I/O یا زمان وابسته‌اند.

</div>