# Отчёт о тестировании Credit Card Number Validator

## Краткое описание

30 марта 2021 было проведено функциональное тестирование приложения Credit Card Number Validator.

На тестирование затрачено: 2 часа

В результате тестирования выявлены следующие дефекты:
* [Приложение не работает для номеров карт не из 16 цифр](https://github.com/eguzhova/javaqa_ccnv/issues/1)

## Описание процесса тестирования
В процессе тестирования использовался тестовый сценарий:

1. Создать проект в IntelliJ IDEA с кодом 
```java
public class Main {
  public static void main(String[] args) {
    // TODO: подставлять номер карты нужно сюда между двойными кавычками, без пробелов
    String number = "5351719427810741";
    System.out.println(String.format("Result is %s", isValidCardNumber(number) ? "OK" : "FAIL"));
  }

  public static boolean isValidCardNumber(String number) {
    if (number == null) {
      return false;
    }

    if (number.length() != 16) {
      return false;
    }

    long result = 0;
    for (int i = 0; i < number.length(); i++) {
      int digit;
      try {
        digit = Integer.parseInt(number.charAt(i) + "");
      } catch (NumberFormatException e) {
        return false;
      }

      if (i % 2 == 0) {
        digit *= 2;
        if (digit > 9) {
          digit -= 9;
        }
      }
      result += digit;
    }

    return (result != 0) && (result % 10 == 0);
  }
}
```
2. В четвертой строке "String number" поочередно присваиваем значения из тестовых данных и запускаем код на исполнение
2. Оценить правильность результата

В качестве валидных тестовых данных использовались номера из генератора номеров карт из 14, 16 и 19 знаков с сайта [freeformatter.com](https://www.freeformatter.com/credit-card-number-generator-validator.html#validate)

Список значений и ожидаемый результат
1. 30124538960347 - Result is OK
1. 3530811439624290 - Result is OK
1. 3530305384180424791 - Result is OK
1. 0 - Result is FAIL
1. 9999999999999999 - Result is FAIL

#### Тестирование производилось в следующем окружении:
* MacOS 11.2.3 (20D91)
* openjdk version "11.0.10" 2021-01-19
* IntelliJ IDEA Community Edition "2020.3.3"
