Ось пояснювальна стаття українською мовою, яка допоможе зрозуміти код та навчитися його аналізувати.  

---

# Як працює Tic Tac Toe на Java: розбір коду

Ця стаття допоможе вам зрозуміти, як створити просту гру "Хрестики-нулики" з графічним інтерфейсом за допомогою Java. Ми детально розглянемо кожну частину коду, щоб пояснити, як працює програма.  

---

## 1. Імпорт бібліотек  
На початку коду ми імпортуємо необхідні бібліотеки:  
- **`javax.swing.*`** — для створення вікон, кнопок та іншого графічного інтерфейсу.  
- **`java.awt.*`** — для налаштування розмітки (розташування кнопок) і стилів шрифтів.  
- **`java.awt.event.*`** — для обробки подій, таких як натискання кнопок.  
- **`java.util.Random`** — для створення випадкових ходів комп’ютера.  

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;
```

---

## 2. Головний клас гри  
Ми створюємо клас **`TicTacToeGUI`**, який успадковує **`JFrame`** (вікно програми) і реалізує **`ActionListener`** (обробник подій кнопок).  

```java
public class TicTacToeGUI extends JFrame implements ActionListener {
    private final JButton[] buttons = new JButton[9]; // Масив із 9 кнопок для ігрового поля
    private boolean playerTurn = true; // Чий хід: true - гравця, false - комп'ютера
    private final char playerSymbol = 'X'; // Символ гравця
    private final char computerSymbol = 'O'; // Символ комп'ютера
```

---

## 3. Конструктор класу  

**Конструктор** — це спеціальний метод, який виконується при створенні об’єкта.  

### Що ми робимо в конструкторі:  
1. Налаштовуємо вікно програми (назва, розмір, що робити при закритті).  
2. Встановлюємо **розмітку**: у нашому випадку це **`GridLayout`** — сітка 3x3 для кнопок.  
3. Створюємо 9 кнопок і додаємо їх до вікна.  

```java
    public TicTacToeGUI() {
        setTitle("Tic Tac Toe"); // Встановлюємо назву вікна
        setSize(400, 400); // Розмір вікна
        setDefaultCloseOperation(EXIT_ON_CLOSE); // Закриваємо програму при виході
        setLayout(new GridLayout(3, 3)); // Сітка 3x3 для ігрового поля

        for (int i = 0; i < 9; i++) {
            buttons[i] = new JButton(""); // Створюємо кнопку
            buttons[i].setFont(new Font("Arial", Font.BOLD, 40)); // Великий шрифт
            buttons[i].setFocusPainted(false); // Прибираємо рамку
            buttons[i].addActionListener(this); // Додаємо слухача подій
            add(buttons[i]); // Додаємо кнопку до вікна
        }

        setVisible(true); // Робимо вікно видимим
    }
```

---

## 4. Обробка кліків кнопок  

Коли користувач натискає кнопку, метод **`actionPerformed`** визначає, що сталося, і відповідно обробляє подію.  

### Алгоритм:  
1. **Перевірка ходу**: чи зараз хід гравця.  
2. Якщо кнопка порожня, додаємо на неї символ гравця (`X`).  
3. Перевіряємо, чи виграв гравець або чи нічия.  
4. Якщо гра не завершена, передаємо хід комп’ютеру.  

```java
    @Override
    public void actionPerformed(ActionEvent e) {
        JButton clickedButton = (JButton) e.getSource(); // Отримуємо натиснуту кнопку

        if (playerTurn && clickedButton.getText().equals("")) {
            clickedButton.setText(String.valueOf(playerSymbol)); // Встановлюємо "X"
            playerTurn = false; // Передаємо хід комп'ютеру

            if (checkWinner(playerSymbol)) {
                JOptionPane.showMessageDialog(this, "You win!"); // Повідомляємо про перемогу
                resetGame(); // Починаємо нову гру
                return;
            } else if (isDraw()) {
                JOptionPane.showMessageDialog(this, "It's a draw!"); // Нічия
                resetGame();
                return;
            }

            computerMove(); // Викликаємо хід комп’ютера
        }
    }
```

---

## 5. Хід комп’ютера  

**Метод `computerMove`** відповідає за хід комп'ютера:  
1. **Генеруємо випадковий номер кнопки** (0–8).  
2. Перевіряємо, чи кнопка порожня. Якщо ні, пробуємо іншу.  
3. Встановлюємо символ комп'ютера (`O`) на порожню кнопку.  
4. Перевіряємо, чи виграв комп’ютер, або чи нічия.  

```java
    private void computerMove() {
        Random random = new Random();
        int move;

        do {
            move = random.nextInt(9); // Випадкове число від 0 до 8
        } while (!buttons[move].getText().equals("")); // Повторюємо, поки не знайдемо порожню кнопку

        buttons[move].setText(String.valueOf(computerSymbol)); // Встановлюємо "O"

        if (checkWinner(computerSymbol)) {
            JOptionPane.showMessageDialog(this, "Computer wins!"); // Повідомлення про перемогу
            resetGame(); // Перезапуск гри
        } else if (isDraw()) {
            JOptionPane.showMessageDialog(this, "It's a draw!"); // Повідомлення про нічию
            resetGame();
        }

        playerTurn = true; // Повертаємо хід гравцеві
    }
```

---

## 6. Перевірка перемоги  

Метод **`checkWinner`** перевіряє всі можливі комбінації перемоги:  
- Три кнопки в одному рядку.  
- Три кнопки в одному стовпчику.  
- Діагоналі.  

Якщо знайдено комбінацію з однаковими символами, повертається `true`.  

```java
    private boolean checkWinner(char symbol) {
        return (buttons[0].getText().equals(String.valueOf(symbol)) &&
                buttons[1].getText().equals(String.valueOf(symbol)) &&
                buttons[2].getText().equals(String.valueOf(symbol))) || // Верхній рядок
                (buttons[3].getText().equals(String.valueOf(symbol)) &&
                        buttons[4].getText().equals(String.valueOf(symbol)) &&
                        buttons[5].getText().equals(String.valueOf(symbol))) || // Середній рядок
                (buttons[0].getText().equals(String.valueOf(symbol)) &&
                        buttons[4].getText().equals(String.valueOf(symbol)) &&
                        buttons[8].getText().equals(String.valueOf(symbol))); // Діагональ
    }
```

---

## 7. Перевірка нічиєї  

Метод **`isDraw`** перевіряє, чи всі кнопки заповнені. Якщо хоча б одна кнопка порожня, гра триває.  

```java
    private boolean isDraw() {
        for (JButton button : buttons) {
            if (button.getText().equals("")) {
                return false; // Гра триває
            }
        }
        return true; // Усі кнопки заповнені, нічия
    }
```

---

## 8. Перезапуск гри  

Метод **`resetGame`** очищує текст усіх кнопок і повертає хід гравцю.  

```java
    private void resetGame() {
        for (JButton button : buttons) {
            button.setText(""); // Очищуємо кнопку
        }
        playerTurn = true; // Хід гравця
    }
```

---

## 9. Головний метод  

Метод **`main`** запускає гру, створюючи нове вікно Tic Tac Toe.  

```java
    public static void main(String[] args) {
        new TicTacToeGUI(); // Створюємо і запускаємо гру
    }
```

---

## Підсумок  

Цей код створює просту і цікаву гру, яка дозволяє практикувати Java. Він навчає:  
