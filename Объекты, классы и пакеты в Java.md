# Practice
3. 1 Основы ООП

В чем разница между классом и объектом?

 - Класс — это конкретный экземпляр объекта.
 - В Java есть только классы, объектов не бывает.
 - Разницы нет, это одно и то же.
 - Объект — это конкретный экземпляр класса. (+)
 
Сопоставьте следующие понятия ООП с их определениями.
 
Абстракция - Выделение важных сущностей и свойств реального мира для их моделирования в программе.
Инкапсуляция - Объединение внутри класса данных и операций с ними; защита данных и других деталей реализации от прямого доступа.
Наследование - Возможность создания производных классов на основе существующих.
Полиморфизм - Возможность использования производного класса там, где разрешен экземпляр базового класса; при этом вызываются методы, переопределенные в производном классе.

3. 2 Пакеты и модификаторы доступа

Допустим, вы пишете класс Example в пакете org.stepic.java.example. Классы из каких пакетов вы можете использовать в классе Example по их коротким именам без явного import'а? (Можно выбрать несколько ответов.)

 - пакет по умолчанию
 - org.stepic.java.example (+)
 - все подпакеты org.stepic.java.example
 - java.util
 - java.lang (+)
 
Какую директиву import надо поставить в начале исходного файла, чтобы получить возможность вызывать любые статические методы класса java.lang.System и обращаться к любым его статическим полям без указания имени класса?
Напишите единственную директиву import точно так, как вы написали бы ее в программе, включая точку запятой в конце.

 - import static java.lang.System.*;
 
Допустим, вы написали в исходном файле Example.java следующую директиву import:

    import java.math.BigDecimal;
    public class Example {
        // ...
    }

Выберите верные утверждения (можно выбрать несколько).

 - Можно ссылаться на класс java.math.BigDecimal по его короткому имени BigDecimal во всех исходных файлах вашей программы.
 - Байткод класса java.math.BigDecimal включается в байткод вашего класса.
 - Исходный код класса java.math.BigDecimal включается в исходный код вашего класса.
 - Можно ссылаться на класс java.math.BigDecimal по его короткому имени BigDecimal в пределах Example.java. (+)
 
3. 3 Объявление класса
 
Какие модификаторы доступа применимы к классу верхнего уровня (т.е. не вложенному в другой класс)?
 
 - protected
 - отсутствие модификатора (пакетный доступ) (+)
 - public (+)
 - private

Какой модификатор доступа имеет конструктор без параметров, автоматически добавляемый компилятором для public-класса?

 - отсутствие модификатора (пакетный доступ)
 - public (+)
 - protected
 - private
 
Выберите верные утверждения про модификатор static.

 - Класс, объявленный с модификатором static, может иметь только статические методы.
 - Вложенный класс, объявленный с модификатором static, теряет возможность обращаться к нестатическим членам внешнего класса. (+)
 - Поле класса, объявленное с модификатором static, — это константа, т.е. значение не может меняться.
 - К полям и методам, имеющим модификатор static, можно обращаться, не создавая экземпляр содержащего их класса. (+)
 
На игровом поле находится робот. Позиция робота на поле описывается двумя целочисленным координатами: X и Y. Ось X смотрит слева направо, ось Y — снизу вверх. (Помните, как рисовали графики функций в школе?)
В начальный момент робот находится в некоторой позиции на поле. Также известно, куда робот смотрит: вверх, вниз, направо или налево. Ваша задача — привести робота в заданную точку игрового поля.

    public class Robot {
        public Direction getDirection() {
            // текущее направление взгляда
        }
        public int getX() {
            // текущая координата X
        }
        public int getY() {
            // текущая координата Y
        }
        public void turnLeft() {
            // повернуться на 90 градусов против часовой стрелки
        }
        public void turnRight() {
            // повернуться на 90 градусов по часовой стрелке
        }
        public void stepForward() {
            // шаг в направлении взгляда
            // за один шаг робот изменяет одну свою координату на единицу
        }
    }

Direction, направление взгляда робота,  — это перечисление:

    public enum Direction {
        UP,
        DOWN,
        LEFT,
        RIGHT
    }
    
Решение:

    public static void moveRobot (Robot robot, int toX, int toY) {
      if (robot.getX() > toX) {
        while (robot.getDirection() != Direction.LEFT) robot.turnLeft();
        do {
          robot.stepForward();
        } while (robot.getX() > toX);
      }
      else if (robot.getX() < toX) {
        while (robot.getDirection() != Direction.RIGHT) robot.turnLeft();
        do {
          robot.stepForward();
        } while (robot.getX() < toX);
      }
      if (robot.getY() < toY) {
        while (robot.getDirection() != Direction.UP) robot.turnLeft();
        do {
          robot.stepForward();
        } while (robot.getY() < toY);
      }
      else if (robot.getY() > toY) {
        while (robot.getDirection() != Direction.DOWN) robot.turnLeft();
        do {
          robot.stepForward();
        } while (robot.getY() > toY);
      }
    }
    
3. 4 Наследование. Класс Object

Выберите условия, необходимые для того, чтобы метод производного класса переопределил метод базового класса.
Это сложный вопрос. Если правильный ответ никак не находится, попробуйте экспериментально проверить все эти условия на каком-нибудь синтетическом примере. Факт успешного переопределения метода поможет подтвердить аннотация @Override.

 - Метод базового класса должен быть виден в производном классе. (+)
 - Метод производного класса должен иметь в точности тот же тип возвращаемого значения, что и метод базового класса.
 - Метод производного класса должен иметь в точности тот же набор параметров, что и метод базового класса. (+)
 - Тип, возвращаемый методом производного класса, должен совпадать или быть подклассом типа, возвращаемого методом базового класса. (+)
 - Метод производного класса должен иметь в точности тот же модификатор доступа, что и метод базового класса.
 - Метод производного класса должен иметь модификатор доступа, такой же или более открытый, чем метод базового класса. (+)
 - Метод производного класса должен быть помечен аннотацией @Override.
 
Где может использоваться модификатор final?

 - В объявлении статического поля класса. (+)
 - В объявлении класса. (+)
 - В объявлении параметра метода. (+)
 - В объявлении конструктора.
 - В объявлении локальной переменной. (+)
 - В объявлении метода. (+)
 - В объявлении нестатического поля класса. (+)
 
Дан класс ComplexNumber. Переопределите в нем методы equals() и hashCode() так, чтобы equals() сравнивал экземпляры ComplexNumber по содержимому полей re и im, а hashCode() был бы согласованным с реализацией equals().
Реализация hashCode(), возвращающая константу или не учитывающая дробную часть re и im, засчитана не будет.

    public final class ComplexNumber {
        private final double re;
        private final double im;
        public ComplexNumber(double re, double im) {
            this.re = re;
            this.im = im;
        }
        public double getRe() {
            return re;
        }
        public double getIm() {
            return im;
        }
        public boolean equals(Object other){
            if (this == other) {
                return true;
            }
            if (other instanceof ComplexNumber){
                ComplexNumber temp = (ComplexNumber)other;
                return this.re == temp.getRe() && this.im == temp.getIm();
            }
            return false;
        }
        public int hashCode(){
           return (int) ((re - 5) * (im + 7));
        }    
    }
    
3. 5 Абстрактные классы и интерфейсы

Выберите верные утверждения про интерфейсы и абстрактные классы.

 - Ключевое слово "interface" — это сокращенная запись для "abstract class".
 - Интерфейсы отличаются от абстрактных классов тем, что не могут содержать реализаций методов.
 - Класс может наследоваться только от одного класса, но может реализовывать сколько угодно интерфейсов. (+)
 - Интерфейсы отличаются от абстрактных классов тем, что могут содержать только публичные методы и публичные статические финальные поля. (+)
 
Укажите необходимое и достаточное условие для того, чтобы интерфейс можно было инстанцировать при помощи лямбда-выражения.

 - Ровно один метод интерфейса должен быть помечен аннотацией @FunctionalInterface.
 - Интерфейс должен быть помечен аннотацией @FunctionalInterface.
 - Интерфейс должен быть из стандартной библиотеки, и в JavaDoc'е к нему должна быть указана такая возможность.
 - Интерфейс должен содержать ровно один абстрактный метод. (+)
 
Реализуйте метод, выполняющий численное интегрирование заданной функции на заданном интервале по формуле левых прямоугольников.
Функция задана объектом, реализующим интерфейс java.util.function.DoubleUnaryOperator. Его метод applyAsDouble() принимает значение аргумента и возвращает значение функции в заданной точке.
Интервал интегрирования задается его конечными точками a и b, причем a<=b. Для получения достаточно точного результата используйте шаг сетки не больше 1e−6.

    public static double integrate(DoubleUnaryOperator f, double a, double b) {
        int i;
        int n = 1000000;
        double result, h;
        result = 0;
        h = (b - a) / n;
        for(i = 0; i < n; i++) {
            result += f.applyAsDouble(a + h * (i + 0.5));
        }
        result *= h;
        return result;
    }
    
Напишите класс AsciiCharSequence, реализующий компактное хранение последовательности ASCII-символов (их коды влезают в один байт) в массиве байт. По сравнению с классом String, хранящим каждый символ как char, AsciiCharSequence будет занимать в два раза меньше памяти.
Класс AsciiCharSequence должен:
 - реализовывать интерфейс java.lang.CharSequence;
 - иметь конструктор, принимающий массив байт;
 - определять методы length(), charAt(), subSequence() и toString().

     public class AsciiCharSequence implements CharSequence {
         private byte[] content;
         public AsciiCharSequence(byte[] content) {
             this.content = content;
         }
         public int length() {
             return content.length;
         }
         public char charAt(int index) {
             return (char)content[index];
         }
         public CharSequence subSequence(int start, int end) {
             byte[] result = new byte[wnd-start];
             for(int i = start; i < end; i++ ){
                 result[i-start] = content[i];
             }
             return new AsciiCharSequence(result);
         }
         public String toString() {
             StringBuilder result = new StringBuilder(content.length);
             for(int i = 0; i < content.length; i++ ){
                 result.append((char)content[i]);
             }
             return result.toString();
         }
     }
     

Пришло время попробовать реализовать иерархию классов определенного вида и решить конкретную задачу.
Представим, вы делаете систему фильтрации комментариев на каком-то веб-портале, будь то новости, видео-хостинг, а может даже для системы онлайн-обучения :)
Вы хотите фильтровать комментарии по разным критериям, уметь легко добавлять новые фильтры и модифицировать старые.
Допустим, мы будем фильтровать спам, комментарии с негативным содержанием и слишком длинные комментарии.
Спам будем фильтровать по наличию указанных ключевых слов в тексте. 
Негативное содержание будем определять по наличию одного из трех смайликов – :( =( :|
Слишком длинные комментарии будем определять исходя из данного числа – максимальной длины комментария.
Вы решили абстрагировать фильтр в виде следующего интерфейса:

    interface TextAnalyzer {
        Label processText(String text);
    }

Label – тип-перечисление, которые содержит метки, которыми будем помечать текст:

    enum Label {
        SPAM, NEGATIVE_TEXT, TOO_LONG, OK
    }

Дальше, вам необходимо реализовать три класса, которые реализуют данный интерфейс: SpamAnalyzer, NegativeTextAnalyzer и TooLongTextAnalyzer.
SpamAnalyzer должен конструироваться от массива строк с ключевыми словами. Объект этого класса должен сохранять в своем состоянии этот массив строк в приватном поле keywords.
NegativeTextAnalyzer должен конструироваться конструктором по-умолчанию.
TooLongTextAnalyzer должен конструироваться от int'а с максимальной допустимой длиной комментария. Объект этого класса должен сохранять в своем состоянии это число в приватном поле maxLength.
Наверняка вы уже заметили, что SpamAnalyzer и NegativeTextAnalyzer во многом похожи – они оба проверяют текст на наличие каких-либо ключевых слов (в случае спама мы получаем их из конструктора, в случае негативного текста мы заранее знаем набор грустных смайликов) и в случае нахождения одного из ключевых слов возвращают  Label (SPAM и NEGATIVE_TEXT соответственно), а если ничего не нашлось – возвращают OK.
Давайте эту логику абстрагируем в абстрактный класс KeywordAnalyzer следующим образом:
Выделим два абстрактных метода getKeywords и getLabel, один из которых будет возвращать набор ключевых слов, а второй метку, которой необходимо пометить положительные срабатывания. Нам незачем показывать эти методы потребителям классов, поэтому оставим доступ к ним только для наследников.
Реализуем processText таким образом, чтобы он зависел только от getKeywords и getLabel.
Сделаем SpamAnalyzer и NegativeTextAnalyzer наследниками KeywordAnalyzer и реализуем абстрактные методы.
Последний штрих – написать метод checkLabels, который будет возвращать метку для комментария по набору анализаторов текста. checkLabels должен возвращать первую не-OK метку в порядке данного набора анализаторов, и OK, если все анализаторы вернули OK.
Используйте, пожалуйста, модификатор доступа по-умолчанию для всех классов.
В итоге, реализуйте классы KeywordAnalyzer, SpamAnalyzer, NegativeTextAnalyzer и TooLongTextAnalyzer и метод checkLabels. TextAnalyzer и Label уже подключены, лишние импорты вам не потребуются.

Решение:

    abstract class KeywordAnalyzer implements TextAnalyzer {
        protected abstract String[] getKeywords();
        protected abstract Label getLabel();
        public Label processText(String text) {
            String[] keywords = getKeywords();
            for (String keyword : keywords) {
                if (text.contains(keyword)) {
                    return getLabel();
                }
            }
            return Label.OK;
        }
    }
    class SpamAnalyzer extends KeywordAnalyzer implements TextAnalyzer {
        private String[] keywords;
        public SpamAnalyzer(String[] keywords) {
            this.keywords = keywords;
        }
        protected String[] getKeywords() {
            return keywords;
        }
        protected Label getLabel() {
            return Label.SPAM;
        }
    }
    class NegativeTextAnalyzer extends KeywordAnalyzer implements TextAnalyzer {
        private final String[] KEYWORDS = {":(", "=(", ":|"};
        protected String[] getKeywords() {
            return KEYWORDS;
        }
        protected Label getLabel() {
            return Label.NEGATIVE_TEXT;
        }
    }
    class TooLongTextAnalyzer implements TextAnalyzer {
        private int maxLength;
        public TooLongTextAnalyzer(int threshold) {
            this.maxLength = threshold;
        }
        public Label processText(String text) {
            if (text.length() > maxLength)
                return Label.TOO_LONG;
            else
                return Label.OK;
        }
    }
     public Label checkLabels(TextAnalyzer[] analyzers, String text) {
            for (int i = 0; i < analyzers.length; i++)
                if (analyzers[i].processText(text) != Label.OK)
                    return analyzers[i].processText(text);
            return Label.OK;
    }
