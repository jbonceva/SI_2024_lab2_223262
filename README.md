# Втора лабораториска вежба по Софтверско инженерство
## Јордана Бончева 223262
### Control Flow Graph
![ControlFlowGraph drawio](https://github.com/jbonceva/SI_2024_lab2_223262/assets/166951079/f9895014-8d3d-4009-811f-27f00739610e)


### Цикломатска комплетност

Цикломатската комплетност на овој код е 10.

Е(бројот на ребра) -N (бројот на јазлите) +2=36-28+2=10

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
### Тест случаи според Multiple Condition критериум
Се проверуваат комбинациите FXX,TFX,TTF и TTT.

Се креира артикл со соодветните карактеристики и се проверува дали функцијата го враќа соодветниот резултат во зависност од состојбата на артиклот.

```java
 @Test
    void MultipleConditionTest() {
        // Комбинација FXX: price <= 300
        Item FXX = SILab2.createItem("Product1", "8929", 250, 0.1f);
        assertFalse(FXX.getPrice() > 300);

        // Комбинација TFX: price > 300, discount <= 0
        Item TFX = SILab2.createItem("Product2", "8929", 350, 0);
        assertTrue(TFX.getPrice() > 300);
        assertFalse(TFX.getDiscount() > 0);

        // Комбинација TTF: price > 300, discount > 0, barcode не започнува со '0'
        Item TTF = SILab2.createItem("Product3", "6070", 400, 0.1f);
        assertTrue(TTF.getPrice() > 300);
        assertTrue(TTF.getDiscount() > 0);
        assertNotEquals('0', TTF.getBarcode().charAt(0));

        // Комбинација TTT: price > 300, discount > 0, barcode започнува со '0'
        Item TTT = SILab2.createItem("Product4", "0876", 530, 0.1f);
        assertTrue(TTT.getPrice() > 300);
        assertTrue(TTT.getDiscount() > 0);
        assertEquals('0', TTT.getBarcode().charAt(0));
    }
 ```
