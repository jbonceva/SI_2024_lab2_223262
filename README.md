# Втора лабораториска вежба по Софтверско инженерство
## Јордана Бончева 223262
### Control Flow Graph
![ControlFlowGraph drawio](https://github.com/jbonceva/SI_2024_lab2_223262/assets/166951079/f0ce401d-c0d9-4f57-9cf0-95bc3f3a929b)



### Цикломатска комплетност

Цикломатската комплетност на овој код е 10.

Е(бројот на ребра) -N (бројот на јазлите) +2=>36-28+2=10

### Тест случаи според Every Branch критериум

Правиме проверка дали функцијата при различен input од корисникот се однесува правилно.
```java
@Test
    void EveryBranchTest() {
        // Креирање на тест објекти
        Item item_invalid_barcode = SILab2.createItem("Banana", "5", 170, 0);
        Item item_no_barcode = SILab2.createItem("Milk", null, 170, 0);
        Item item_valid = SILab2.createItem("Kiwi", "7", 170, 0);
        Item item_high_price_discount_starting_with_zero = SILab2.createItem("Sugar", "900g", 550, 0.1f);

        // Тест случај: allItems е null
        RuntimeException ex;
        ex = assertThrows(RuntimeException.class, () -> SILab2.checkCart(null, 0));
        assertTrue(ex.getMessage().contains("allItems list can't be null!"));

        // Тест случај: item има невалиден баркод
        ex = assertThrows(RuntimeException.class, () -> SILab2.checkCart(items(item_invalid_barcode), 0));
        assertTrue(ex.getMessage().contains("Invalid character in item barcode!"));

        // Тест случај: item нема баркод
        ex = assertThrows(RuntimeException.class, () -> SILab2.checkCart(items(item_no_barcode), 0));
        assertTrue(ex.getMessage().contains("No barcode!"));

        // Тест случај: валиден item и доволен буџет
        assertEquals(true, SILab2.checkCart(items(item_valid), 300));

        // Тест случај: валиден item и недоволен буџет
        assertEquals(false, SILab2.checkCart(items(item_valid), 50));

        // Тест случај: валиден item со висок попуст и почнува со нула баркод
        assertTrue(SILab2.checkCart(items(item_high_price_discount_starting_with_zero), 1000));
    }
    ```

