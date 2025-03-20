# Валидация ИИН
## Исходный код
Примеры реализации алгоритма на различных языках программирования:
<details>
  <summary>Python</summary>

```python
import re
from functools import reduce
from operator import add, mul
from typing import List
 
 
def multiply(iin: str, weights: List[int]) -> int:
    result = reduce(
        add,
        map(lambda i: mul(*i), zip(map(int, iin), weights))
    )
    return result
 
 
def validate_iin(iin: str) -> bool:
    if not re.match(r'[0-9]{12}', iin):
        return False
    w1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
    w2 = [3, 4, 5, 6, 7, 8, 9, 10, 11, 1, 2]
    check_sum = multiply(iin, w1) % 11
    if check_sum == 10:
        check_sum = multiply(iin, w2) % 11
    if check_sum != int(iin[-1]):
        return False
    return True
```

</details>

<details>
  <summary>JavaScript</summary>

```javascript
function multiply(iin, weights) {
    return weights.reduce((sum, weight, index) => {
        return sum + parseInt(iin[index]) * weight;
    }, 0);
}

function validateIin(iin) {
    const iinPattern = /^[0-9]{12}$/;
    if (!iinPattern.test(iin)) {
        return false;
    }

    const w1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];
    const w2 = [3, 4, 5, 6, 7, 8, 9, 10, 11, 1, 2];

    let checkSum = multiply(iin, w1) % 11;
    if (checkSum === 10) {
        checkSum = multiply(iin, w2) % 11;
    }

    return checkSum === parseInt(iin[iin.length - 1]);
}
```

</details>

<details>
  <summary>Swift</summary>

```swift
import Foundation
 
func multiply(iin: String, weights: [Int]) -> Int {
    var sum = 0
    for (index, char) in iin.enumerated() {
        if let digit = Int(String(char)) {
            sum += digit * weights[index]
        }
    }
    return sum
}
 
func validateIIN(iin: String) -> Bool {
    let iinPattern = "^[0-9]{12}$"
    let regex = try! NSRegularExpression(pattern: iinPattern)
    let range = NSRange(location: 0, length: iin.utf16.count)
 
    if regex.firstMatch(in: iin, options: [], range: range) == nil {
        return false
    }
 
    let w1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
    let w2 = [3, 4, 5, 6, 7, 8, 9, 10, 11, 1, 2]
 
    var checkSum = multiply(iin: iin, weights: w1) % 11
    if checkSum == 10 {
        checkSum = multiply(iin: iin, weights: w2) % 11
    }
 
    if let lastDigit = Int(String(iin.last!)) {
        return checkSum == lastDigit
    } else {
        return false
    }
}
```

</details>

<details>
  <summary>Kotlin</summary>

```kotlin
import java.util.regex.Pattern
 
fun multiply(iin: String, weights: List<Int>): Int {
    return weights.mapIndexed { index, weight ->
        iin[index].toString().toInt() * weight
    }.sum()
}
 
fun validateIIN(iin: String): Boolean {
    val iinPattern = Pattern.compile("^[0-9]{12}\$")
    if (!iinPattern.matcher(iin).matches()) {
        return false
    }
 
    val w1 = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11)
    val w2 = listOf(3, 4, 5, 6, 7, 8, 9, 10, 11, 1, 2)
 
    var checkSum = multiply(iin, w1) % 11
    if (checkSum == 10) {
        checkSum = multiply(iin, w2) % 11
    }
 
    return checkSum == iin.last().toString().toInt()
}
```

</details>

<details>
  <summary>Java</summary>

```java
import java.util.regex.Pattern;

public class IINValidator {

    public static int multiply(String iin, int[] weights) {
        int sum = 0;
        for (int i = 0; i < weights.length; i++) {
            sum += Character.getNumericValue(iin.charAt(i)) * weights[i];
        }
        return sum;
    }

    public static boolean validateIIN(String iin) {
        Pattern iinPattern = Pattern.compile("^[0-9]{12}$");
        if (!iinPattern.matcher(iin).matches()) {
            return false;
        }

        int[] w1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};
        int[] w2 = {3, 4, 5, 6, 7, 8, 9, 10, 11, 1, 2};

        int checkSum = multiply(iin, w1) % 11;
        if (checkSum == 10) {
            checkSum = multiply(iin, w2) % 11;
        }

        return checkSum == Character.getNumericValue(iin.charAt(iin.length() - 1));
    }

    public static void main(String[] args) {
        String iin = "123456789012";
        System.out.println(validateIIN(iin)); // Выводит true или false в зависимости от правильности IIN
    }
}
```

</details>

<details>
  <summary>Go</summary>

```golang
package main

import (
    "fmt"
    "regexp"
    "strconv"
)

func multiply(iin string, weights []int) int {
    sum := 0
    for i, weight := range weights {
        digit, _ := strconv.Atoi(string(iin[i]))
        sum += digit * weight
    }
    return sum
}

func validateIIN(iin string) bool {
    iinPattern := regexp.MustCompile(`^[0-9]{12}$`)
    if !iinPattern.MatchString(iin) {
        return false
    }

    w1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11}
    w2 := []int{3, 4, 5, 6, 7, 8, 9, 10, 11, 1, 2}

    checkSum := multiply(iin, w1) % 11
    if checkSum == 10 {
        checkSum = multiply(iin, w2) % 11
    }

    lastDigit, _ := strconv.Atoi(string(iin[len(iin)-1]))
    return checkSum == lastDigit
}
```

</details>

## Описание
### Цель
Алгоритм предназначен для проверки правильности ИИН. ИИН состоит из 12 цифр, и последняя цифра является контрольной суммой, которая рассчитывается на основании первых 11 цифр. Алгоритм проверяет, соответствует ли последняя цифра расчетам, выполняемым по определенным весам.

### Шаги алгоритма
1. Проверка формата ИИН:

&emsp;&emsp;&emsp;- Все 12 символов строки должны быть цифрами.

&emsp;&emsp;&emsp;- Используем регулярное выражение `^[0-9]{12}$` для этой проверки.

2. Вычисление контрольной суммы с использованием двух наборов весов:

&emsp;&emsp;&emsp;- Первый набор весов: `w1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`

&emsp;&emsp;&emsp;- Второй набор весов: `w2 = [3, 4, 5, 6, 7, 8, 9, 10, 11, 1, 2]`

3. Проверка контрольной суммы:

&emsp;&emsp;&emsp;- Шаг 1: Умножаем каждую цифру ИИН на соответствующий вес из набора `w1` и суммируем результаты.

&emsp;&emsp;&emsp;- Шаг 2: Остаток от деления суммы на 11 (`checkSum = сумма % 11`).

&emsp;&emsp;&emsp;- Если результат равен 10, то:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;- Используем второй набор весов `w2` для повторного вычисления суммы.

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;- Остаток от деления новой суммы на 11 становится новой контрольной суммой.

&emsp;&emsp;&emsp;- Шаг 3: Сравниваем результат с последней цифрой ИИН.

### Пример пошаговой проверки

ИИН: `123456789012`

1. Проверка формата ИИН:

&emsp;&emsp;&emsp;- Регулярное выражение `^[0-9]{12}$` проверяет, состоит ли строка из 12 цифр.

&emsp;&emsp;&emsp;- ИИН удовлетворяет данному условию.

2. Вычисление контрольной суммы (первый набор весов):

```
ИИН:  1  2  3  4  5  6  7  8  9  0  1  2
Вес:  1  2  3  4  5  6  7  8  9  10 11
 
Σ = 1*1 + 2*2 + 3*3 + 4*4 + 5*5 + 6*6 + 7*7 + 8*8 + 9*9 + 0*10 + 1*11 = 1 + 4 + 9 + 16 + 25 + 36 + 49 + 64 + 81 + 0 + 11 = 296
```

&emsp;&emsp;&emsp;- `checkSum = 296 % 11 = 10`

3. Проверка второго набора весов (так как checkSum = 10):

```
ИИН:  1  2  3  4  5  6  7  8  9  0  1  2
Вес:  3  4  5  6  7  8  9  10 11 1  2
 
Σ = 1*3 + 2*4 + 3*5 + 4*6 + 5*7 + 6*8 + 7*9 + 8*10 + 9*11 + 0*1 + 1*2 = 3 + 8 + 15 + 24 + 35 + 48 + 63 + 80 + 99 + 0 + 2 = 377
```

&emsp;&emsp;&emsp;- `checkSum = 377 % 11 = 3`

4. Сравнение с последней цифрой:

&emsp;&emsp;&emsp;- Последняя цифра ИИН: `2`

&emsp;&emsp;&emsp;- Контрольная сумма: `3`

&emsp;&emsp;&emsp;- ИИН считается недействительным, так как `2` не равно `3`.

### Итог
Алгоритм проверяет правильность ИИН путем вычисления контрольной суммы и сравнения её с последней цифрой ИИН. Он использует два набора весов для расчета контрольной суммы и учитывает особый случай, когда первоначальная контрольная сумма равна 10, для чего выполняет повторный расчет с другим набором весов.