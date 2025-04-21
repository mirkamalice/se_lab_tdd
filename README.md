# مدیریت حساب بانکی

<div dir="rtl">
مورد اول:
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












